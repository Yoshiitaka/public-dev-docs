---
sidebar_position: 1
sidebar_label: サーバーレスキャッシュとは?
title: サーバーレスキャッシュとは?
pagination_prev: null
description: キャッシュの文脈でサーバーレスとはどういうことか、そして Momento Cache がいかに簡潔で高速なキャッシュになるかを学びましょう。
---

# サーバーレスキャッシュとは？

サーバーレスは今ソフトウェア開発業界で最も熱いトレンドで、"サーバーレスにやさしい" サービスが爆発的に増えていいます。

![An image of a fast moving city as Momento serverless caching services](@site/static/img/serverless-caching.jpg)

私達は Momento Cache がその中で最もサーバーレスにやさしいキャッシュであると信じています。でも、サーバーレスとはどういう意味で、なぜ Momento Cache がサーバーレスにやさしいキャッシュなのでしょうか？

ここでは、サーバーレスの2つの定義と、サーバーレス的な技術に興味のある開発者達について、詳しく見ていきましょう。

- [運用モデルとしてのサーバーレス](#運用モデルとしてのサーバーレス)

- [関連するアーキテクチャに対する互換性としてのサーバーレス](#関連するアーキテクチャに対する互換性としてのサーバーレス)

- [独立したプロジェクトの基盤としてのサーバーレス](#独立したプロジェクトの基盤としてのサーバーレス)

なお、このページでは Momento Cache がどうしてサーバーレスアプリケーションと親和性が高いかの概念的な理由の説明に注力しています。もし実際に Momento Cache を皆さんのサーバーレスアプリケーションに組み込む際のアドバイスをお探しでしたら、[Momento Cache を AWS Lambda から使う](./../develop/guides/caching-with-aws-lambda)のページをご覧下さい。


## 運用モデルとしてのサーバーレス

一つ目の"サーバーレス"という用語が使われる場面は、特定のサービスの運用形態を表す時です。これが私の一番好みのサーバーレスの定義で、本来の純粋な定義に最も近いものです。ここでは"古典的な"サーバーレスの定義と呼んでいきましょう。

古典的なサーバーレスの定義では、サーバーレスサービスは一般的に3つの属性を持っています。

第一に、サーバーレスサービスはマネージドです。サービスをユーザー自身が走らせるのではなく、サービス提供者が中心となる管理責任をユーザーの代わりに担ってくれます。データベースを実行する場合、Postgres をベアメタルマシンにインストールするのではありません。キャッシュが必要であれば、Memcached 用に EC2 インスタンスを立ち上げるのではありません。そうではなくて、基礎となるソフトウェアをインストールし、設定し、管理しているサービス提供者から、データベース、キャッシュ、メッセージキュー、といったサービスを直接利用することを意味します。

第二に、サーバーレスサービスは抽象化されています。一般的に、サービスをどのようにプロビジョンするかを細かく設定する手段はほとんどありません。Ben Kehoe が彼の[サーバーレスのスペクトラムに関する投稿](https://ben11kehoe.medium.com/the-serverless-spectrum-147b02cb2292#fbae)でも強調していることです。現実的な意味では、特定の機能に対するキャパシティ (例えば DynamoDB の読み書きのスループットキャパシティ) を、その下で実際に使われるリソース (例えば、CPU、RAM、ネットワーク I/O 等の多数の設定をされたデータベースのインスタンスサイズ) の代わりにプロビジョンします。または、SQS キューや S3 バケットの様に、何もプロビジョンする必要がありません。これらの場合には、サービス提供者が必要に応じてスケールアップやダウンを管理してくれます。

最後に、サーバーレスは従量課金のスキーマです。サービスが、その下で動くリソースよりも抽象化されていてキャパシティ指向なので、支払うお金と受け取る価値にほぼ直接的な関連性を見いだせるでしょう。これは、例えば AWS Lambda、Amazon API Gateway、または Amazon SQS の様な従量課金のシステムを意味します。他にも、Amazon DynamoDB や Amazon Kinesis の様な柔軟にスケールアップやダウンできるシステムを意味することもあります。サーバーレスサービスでは、ピーク時の負荷に備えてオーバープロビジョニングして、ほとんどの時間はインスタンスが低いリソース利用率にある様な事は必要はありません。

Ben Kehoe が上述の記事で言っている様に、"サーバーレス"とはスペクトラムであり、上記の属性をより多く持っているサービスを使いたくなるでしょう。加えて、いくつかのサービスは利用方法に柔軟性がありますが、よりサーバーレス的な使い方ができる方法を試してみるべきです。

### 古典的なサーバーレスの定義に Momento がどの様に適合するか

Momento Cache はこの古典的なサーバーレスの定義に完璧に適合しています。

第一に、Momento Cache はマネージドサービスです。ソフトウェアをインストールしたり、フェイルオーバーを管理したり、バージョンを更新したりする必要はありません。これらは裏で適切に処理されているので、皆さんはご自身のアプリケーションの中心となる場所の開発と運用に専念することができます。

第二に、Momento Cache はキャッシュ管理に関する設定を抽象化しています。キャッシュに必要なインスタンスの数を指定することも、最大メモリサイズを考えることも必要ありません。Momento はクラウドに特化して作られていて、現代的なクラウド基盤の弾力性やスケーラビリティを最大活用しています。皆さんはキャッシュに必要なデータをどれだけでも保存することができて、Momento がそれをシームレスに取り扱います。

最後に、Momento Cache は従量課金のモデルです。上述の様に、利用の有無に関係なく、事前に特定のキャッシュの種類やクラスターサイズを選択することはありません。Momento は Momento Cache と Momento Topics で行われたデータ転送の出入りに対して[課金します](./../manage/pricing)。それ以外は全て含まれています。これによって、料金は皆さんの支配下にあって、アプリケーションの変更が直接的に請求金額に影響を与えます。

この最初のサーバーレスの定義において、Momento Cache はサーバーレスエコシステムに最も適したキャッシュであると言えます。

## 関連するアーキテクチャに対する互換性としてのサーバーレス

AWS Lambda は 2014 年の AWS re:Invent で発表され、サーバーレス革命の皮切りとなりました。イベント駆動、関数ベース、短時間、そして従量課金モデルのコンピュートパラダイムは、当時としては画期的でした。

2014 年の発表以来、エコシステムは大きく進歩しましたが、Lambda は依然サーバーレスアーキテクチャの中心的存在です。Lambda が中心でユニークなモデルをしているため、多くのサーバーレス開発者は、Lambda や他のサーバーレスアプリケーションの基礎となるものと親和性の高いサービスを探し求めています。

第一に、サーバーレス開発者はインターネット越しに HTTPS でアクセスできるサービスを好みます。これは、古典的なデータベースやキャッシュといった、プライベートネットワーク内で永続的な TCP 接続を活用するサービスとは対照的です。その理由の一つは AWS Lambda を VPC 内で実行するときのパフォーマンスが当初は良くなかったことです。しかし、この欠点が克服された後も、依然としてプライベートネットワークを構築して管理することは嫌悪されています。そのため、HTTPS ベースのサービス、例えば Dynamo DB や SQS 等は、MySQL や RabbitMQ 等のサービスよりも好まれます。

第二に、サーバーレス開発者は瞬間的なバーストトラフィックにも高速にスケールできるサービスを求めています。Lambda は事前のプロビジョニング無しに高速にスケールするように設計されています。大量の新しいキューメッセージや、ウェブサイトへの大量のトラフィックを処理する間、Lambda は需要に応じて反応することが可能です。サーバーレス開発者はこのスケーリングの仕組みに対応可能なインフラを探し求めています。これらは基本的に、クラウドベースでマルチテナントにすることにより、負荷の増大を多数の顧客に跨って吸収することのできるものであって、インスタンスベースで接続数の上限やスケーラビリティの弾力性が低いサービスではありません。

最後に、サーバーレス開発者は起動時間を待つことなく高速かつ動的にプロビジョンできるサービスを好みます。これはコンピュートの基礎としての AWS Lambda だけでなく、DynamoDB の様なデータベースや、Kinesis の様なストリーム、または S3 の様なオブジェクトストアも含まれます。サーバーレスアプリケーションはマネージドで、弾力的で、従量課金のサービスを好むので、サーバーレス開発者はクリーンな環境で何かを再現する目的やリリースパイプライン上で自動テストをするために、隔離された環境をしばしばオンデマンドに作成します。この隔離された環境を実現可能にするには、数分ではなく数秒でプロビジョンできるサービスが求められます。

### 標準的なサーバーレスアプリケーションの定義に Momento Cache がどの様に適合するか

Momento は AWS Lambda や他の人気のあるサーバーレスサービスを使ったサーバーレスアプリケーションにとって、優れた追加機能となります。

第一に、Momento Cache は [HTTPS 越しに利用可能です](./../learn/how-it-works/#networking)。これは皆さんのサーバーレスアプリケーションに Momento を追加する際に必要な設定項目を簡素化してくれます。アプリケーションに認証トークンを追加するだけで、キャッシュを使い始めることができます。この HTTPS ベースの接続パターンなら、Lambda 関数内で既存の接続を再利用して、リクエスト毎に新しい接続を張るオーバーヘッドを回避することが可能です。さらに、VPC を使いたいお客様向けに、Momento は VPC ピアリングのオプションも選択可能です。

第二に、Momento Cache は事前のプロビジョニング無しに、高速にキャッシュをスケールし、高い秒間リクエスト数に達することが可能です。Momento Cache への接続数の上限は無いので、バーストトラフィックが来てもアプリケーションの可用性が問題となることはありません。

最後に、Momento Cache は動的なサービスで、一瞬にしてキャッシュを作ったり消したりすることができます。新しいキャッシュを作るために [Momento コントロールプレーン](./../learn/how-it-works/#control-plane-simple-efficient-cache-management)を呼び出すと、キャッシュは一瞬でプロビジョンされレスポンスが返って来た時には利用可能になっています。これによって、CI/CD システム上でブランチ毎に Momento と統合することが簡単になったり、各開発者がアプリケーションのコピーを持つことを可能にしています。

これほどサーバーレスアプリケーションと適合するキャッシュは他にはありません。AWS は Amazon ElastiCache をキャッシュの選択肢として提供していますが、これは VPC 内にある必要があります。そのため、サーバーレスアプリケーションのコストと複雑性が著しく増加します。さらには、利用するかしないかに関わらず事前にインスタンスサイズとクラスタの設定を指定しなければなりません。最後に、キャッシュが利用可能となるためにはインスタンスが起動して設定されることが必要なので、新しいキャッシュのプロビジョニングは数秒ではなく数分かかってしまいます。

## 独立したプロジェクトの基盤としてのサーバーレス

最後のサーバーレスのカテゴリーは、特定の概念というよりも、ある種の人々やアプリケーションのスタイルを意味しています。

近年、個人の開発者や小さいチームによって作られた小さな SaaS アプリケーションや便利なツールの台頭を目にします。これは、インディーズなハッカーの気運が高まっていることや、世界中にプログラミングスキルを持った人が急激に増えていることが理由です。もう一つの重要な要素としては、多大な先行投資をすることなくアプリケーションを開発しスケールすることが簡単になったということがあります。

クラウドの利点、つまりデータセンターリソースを、先払いの費用ではなく、運用費用として利用できるようになったことで、アプリケーションを構築する障害が軽減されました。しかし、サーバーレスツールとセルフサービス基盤の台頭によって、このパターンは爆発的に増加し、グローバルで弾力性のある基盤が大衆向けに利用可能となりました。

このグループにとっては、アプリケーションのためのサービス選定において2つの重要な要素があります。

第一に、クレジットカードでのセルフサービスなサインアップは必須です。サイドプロジェクトや、隙間時間での開発をしている開発者には、商談や時間のかかる調達処理をしている暇はありません。彼らは今すぐにサインアップしてそれが自分のニーズに合うかどうかを確認したいのです。

第二に、この開発者達はツールを使い始める際の十分な無料枠を探し求めています。サイドプロジェクトや有料製品の初期段階においては、ほとんど利用しないか定常的に利用しないものに対して、開発者は一般的にあまりお金を払いたくありません。AWS の無料枠や多くの開発者向けサービスにおける同様の無料枠では、サイドプロジェクトで1円も払うことなく十分な量を利用することができます。

### インディーズなプロジェクトに Momento Cache がどの様に適合するか

あなたが、インディーズなハッカーだったり初期のスタートアップで、お金を節約したいと考えているならば、Momento はとても使い勝手が良いです。

第一に、Momento Cache はセルフサービスのサインアップを持っています。5分以内に Momento の認証トークンを得て[キャッシュに書き込み始めることができます](./../getting-started)。営業と話す必要もなく、事前に契約書にサインする必要もありません。実際のところ、無料枠を楽しむためにはクレジットカードすら入力する必要はありません。

第二に、Momento Cache は十分な量の無料枠を持っています。毎月 50 GB が無料です(詳細は[料金](./../manage/pricing)をご覧下さい)。私達のゴールは、多種多様なアプリケーションが1円も払うことなく Momento 上で実行されていることです。私たちは最高レベルに頑丈なサービスを求めるアプリケーション向けに提供する一方で、Momento を使って成長していくアプリケーションの幅広いコミュニティもサポートしていきます。

## まとめ

このページでは、あらゆるサーバーレスの概念においても Momento Cache が適合していることを解説しました。Momento Cache は現代的なアーキテクチャに特化して設計されており、サーバーレスな運用モデル、Lambda で作られたアプリケーションと協調できる連携モデル、そしてあらゆるタイプの開発者やチームと親和性のあるサインアップと課金モデルを持っています。

Momento Cache を使い始める準備ができているなら、以下のドキュメントをぜひご覧下さい:

- [Momento でキャッシュを始める](./../getting-started)クイックスタート(5分もかかりません)

- [AWS Lambda 関数と Momento を連携する](./../develop/guides/caching-with-aws-lambda)実践的なガイド
