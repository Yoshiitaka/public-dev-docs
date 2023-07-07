---
sidebar_position: 5
sidebar_label: データをまとめて書き込む
title: Redis、CSV、JSON、等から Momento Cache へ効率的にまとめて書き込む
description: 溜まったデータを Momento Cache へスムーズにマイグレーションする方法を学びましょう。
pagination_next:
---

# Redis、CSV、JSON、等から Momento Cache へ効率的にまとめて書き込む

If you need to migrate large volumes of data from an existing source into your Momento cache, you’re in the right place. The pipeline proposed here supports Redis data sources, but the design is extensible to other data sources like CSV, JSON, Parquet, and Memcache, among others.

Momento はまとめてロードするためのツールキットを提供しており、それを使えば、個別に、もしくはパイプラインでデータを展開、検証し、ロードする処理を 簡素化することができます。

![diagram](/img/bulk-writing-diagram.svg)

上の図で、パイプラインはデータソースを共通のフォーマット、[JSON lines (JSON-L)](https://jsonlines.org/) にデータを正規化(展開)していることが わかります。 次に、Momento と互換性のないデータを特定するためのチェック処理を実行します。 最後に、正当なデータをキャッシュにロードします。

私たちは、他のデータソースを追加してくれる皆さんの貢献を心待ちにしています。 または、特定のデータソースのサポートを、[Discord](https://discord.com/invite/3HkAKjUZGq) 又は　[Momento support](mailto:support@momentohq.com)　へのメールでリクエストすることもできます。

## ツールセットのワークフローを設定する

### 前提条件

Redis データベースから読み出すためにツールセットを使うには、Java 8 以上のバージョンがインストールされていることを確認してください。 Windows をご利用の方は、bash をインストールするか、もしくは Windows Subsystem for Linux (WSL) の Linux を利用する必要があります。 For Windows users, you'll additionally either need to install bash or run the Linux version using the Windows Subsystem for Linux (WSL).

### インストールステップ

1. 最新版を[ダウンロードするために、リリースページ](https://github.com/momentohq/momento-bulk-writer/releases)へ行きます。
2. Linux、OSX、または Windows のランタイムから選択します。
3. リリースを解凍し、お好みのディレクトリへ untar します。

Linux での例は以下の様になります:

```cli
$ wget https://github.com/momentohq/momento-bulk-writer/releases/download/${version}/momento-bulk-writer-linux-x86.tgz
$ tar xzvf momento-bulk-writer-linux-x86.tgz
$ cd ./momento-bulk-writer-linux-x86
$ ./extract-rdb.sh -h
$ ./validate.sh -h
$ ./load.sh -h
```

## 利用ガイド

This section provides a step-by-step guide on using the toolset for a Redis to Momento data pipeline. このセクションでは、Redis から Momento へのデータパイプラインのためのツールセットを使うためのステップバイステップのガイドを提供します。 このプロセスには3つの鍵となるステップがあります: Redis データベースから JSON lines への展開、JSON lines の検証、そして JSON lines を Momento へロードするステップです。

### Redis データベース (RDB) ファイルの取得

まず始めに、RDB ファイルを取得するために、[Amazon ElastiCache でバックアップを取るか](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups-manual.html)、 [既存の Redis インスタンス上で BGSAVE を実行する](https://redis.io/commands/bgsave/)必要があります。

### Redis データベースファイルを展開し検証する

RDB ファイルを `./redis` ディレクトリに配置して、出力を現在のディレクトリ上に書き込みたい場合、 `extract-rdb-and-validate.sh` スクリプトを以下の様に実行します:

```cli
$ ./extract-rdb-and-validate.sh -s 1 -t 1 ./redis
```

The command will extract the RDB files in `./redis` to JSON lines format and write the output to the current directory. The `-s` and `-t` flags set the max size in MiB and max time-to-live (TTL) in days of items in the cache, respectively. If an item exceeds [Momento’s service limits](/manage/limits), item size (1 MiB), or a TTL (24 hours), that item will be flagged by the process.

より詳しい情報については、[サービス上限](/manage/limits)をご確認下さい。

### 出力を検定する

現在のディレクトリには、いくつかのファイルとディレクトリが生成されいてるはずです。 重要なディレクトリは、 `validate-strict` と `validate-lax` になります。 `validate-strict` には Redis と Momento 間での不一致のあるデータが含まれ、 `validate-lax` には条件を満たさない、完全にサポート不可なデータが含まれます。 他にも、データ検証プロセスの詳細なレポートもあります。

レポートのいくつかの詳細です: `validate-strict` ディレクトリには、ソースデータ (Redis) と Momento Cache の間の不一致のデータがありますが、その条件としては以下の様なものです:

- 項目の最大サイズを超過している
- 最大 TTL より長い TTL が設定されている
- TTL の設定が欠けている (Momento Cache では必須)
- Momento Cache でサポートされていない型が使われている

これはどのデータに TTL が欠けていて、どんな TTL を設定するかを理解するのに役立ちます。 お客様の中には、アプリケーションロジックのバグによって起こっているかもしれない、 彼らのデータの中の外れ値を探し出すのに役立った方もいます。

対照的に、`validate-lax` ディレクトリには、以下の様なデータが入っています:

- [Momento の項目の最大サイズ](/manage/limits)を超えている
- Momento がサポートしていないデータ型

`validate-lax` には Momento Cache にロードできないデータが含まれていて、人手で確認する必要があります。 例えば、Momento の最大 TTL より長い TTLの項目、TTL が欠けている項目、または既に TTL が過ぎているもので、 何らかの処置をすることで Momento にロードできるようになる可能性があります。 For example, items with a TTL greater than the Momento max, items lacking a TTL, or those already expired can be addressed and then loaded into Momento.

### Momento Cache にデータをロードする

最後に、検証済のデータをロードスクリプトを利用して Momento に読み込みます。 以下の様に実行してください:

```cli
$ ./load.sh -a $AUTH_TOKEN -c $CACHE -t 1 -n 10 ./validate-lax/valid.jsonl
```

このコマンドは `./validate-lax/valid.jsonl` にあるデータを、デフォルトで1日の TTL を付け、`$AUTH_TOKEN` に設定した Momento の認証トークを使いながら、 `$CACHE` に指定したキャッシュにロードしてくれます。 `-n` フラグで Momento に行うリクエストの並列数を指定できます。

### Momento Cache のデータを検証する

必要であれば、Momento Cache の中にあるデータがディスク上のデータと一致するかを verify コマンドを使って検証することができます。 これはサニティーチェックで、期限切れの項目がほぼないので成功するはずです。 This is a sanity check that should succeed, less items that have already expired.

```cli
$ ./bin/MomentoEtl verify -a $AUTH_TOKEN -c $CACHE -n 10 ./validate-lax/valid.jsonl
```

このツールはディスク上の項目とキャッシュ内の差分を表示してくれます。 もし正しく処理を実行していたら、エラーは何も出力されないはずです。

### EC2 インスタンスから実行する

このツールセットは、64GB のディスクを付けた、AWS の同一リージョン内の m6a.2xlarge EC2 インスタンス上でテストしています。 最適なレートを決めるために、最初にたくさんの並列リクエスト数でパフォーマンスを確認しました。 We first performed a sweep of concurrent requests to determine the optimal rate.

この強力なツールセットを使って、Momento Cache へデータをまとめて書き込む便利な機能を楽しんでみて下さい。