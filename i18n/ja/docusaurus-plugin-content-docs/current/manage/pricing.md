---
sidebar_position: 3
title: Pricing and free tier
pagination_prev:
description: Momentoキャッシュの料金モデルの単純さを見てみてください
---

# Momento CacheとMomento Topicsの料金と無料枠
サーバーレスとはあらゆる意味において単純であることであり、それには料金も含まれます！ Momento キャッシュは $0.50/GB の転送コストがかかります。 たったそれだけです！

毎月最初の 50 GB は無料で、使い始めるのにクレジットカードさえ必要ありません。

Momento キャッシュには隠れた料金はありません。 ストレージ、レプリカ、またはインスタンスのためにお金を払う必要はありません。 文字通り、Momento キャッシュに入るまたは出ていくデータ転送のみが課金されます。 それ以外は全てその中に含まれています。 心配することなくサインアップして、素晴らしい何かを作ってください。

 さらなるお手伝いが必要であれば（もしくはちょっと信じられない様であれば）、私たちの[Discord](https://discord.gg/Z7FSXB89)にメッセージを頂けたら、対応いたします。 Momento キャッシュに挑戦して、そしてどれくらい料金を削減できるかを見てみてください！

### よくある質問
<details open>
  <summary>Momento は本当に$.50/GB のデータ転送の出入りだけなのですか？ それ以外にお金がかかるものはありますか？</summary>

| Dimension              | Momento 料金 |
| ---------------------- | ---------- |
| メモリ / ストレージ            | $0/GB      |
| 複数 AZ レプリケーション料金       | $0/GB      |
| シングルサインオン & チーム (まもなく) | $0/ユーザー    |

 </details>

<details>
<summary>Momento CacheやMomento Topicsの費用の予測をするためにいくつかの価格例を確認できますか？ </summary>
はい、<a href="https://www.gomomento.com/blog/complicated-pricing-is-not-serverless">こちらのMomentoブログ</a>から確認していただけます。 </details>

<details>
<summary>Momento を無料で本番環境のアプリケーションに使うことはできますか？ </summary>
もちろんです！ Our free tier and low usage tiers are just billing. It is the same exact service and features whether you use 40GB/month or 40TB/month. 複数AZ レプリケーション、ホットキー保護、そして突発的なリクエストへの自動スケーリングといった高可用性のための機能が全て利用可能です。 セキュリティのための全ての機能(エンドツーエンド暗号化、リクエスト毎の認証、TLS)も無料で使えます。

お客様の中でよく、低い RPS のワークフローのためにフル装備のクラスターをプロビジョンしているケースを見かけます。 HA (高可用性)が欲しい場合、複数ノードが必要です。 CICD を利用したければ、同程度のサイズのクラスターを、ステージングや開発環境にさえ設置したくなるでしょう。 全てコストとして積み上がります！ こうしたマシンは停止してしまって、コストを削減しましょう。 Momento で行きましょう！ </details>

<details>
  <summary>Momento を毎月$5.00 で本番環境のアプリケーションに使うことはできますか？ </summary>
もちろんです！ もし毎月60 GB のデータ転送の出入りがMomento にあったとすると、毎月最初の50GB 分は無料で、残りの毎月10GB に対して$0.50/GB を支払うことになります。 どんなスケールでも、Momento のエンタープライズレベルの可用性、セキュリティ、そしてパフォーマンスがご利用できます。


これは狂気じみたように聞こえるかもしれませんが、私たちが最初に始めたわけではありません。 ほかのサーバーレスサービス、例えば DynamoDB、S3、そして他にもたくさんのサービスで同じものを経験できるでしょう。 私たちは単にサーバーレスのアイデアをキャッシュにもってきただけなのです。 </details>

<details>
  <summary>Can I really store as much data in my cache(s) as I want?</summary>
Heck yeah! You are billed for the inbound and outbound transfer of data, not for the volume of data in your cache. </details>

<details>
  <summary>スケールするのですか？ </summary>
はい、もちろんです！ Momento Cache is the best way to future-proof your caching story. Momento は将来も使い続けられるキャッシュとして最高の選択肢です。 Momento を追加するのは *ものすごく* 素早くできて、そのあとは1RPS だろうが100万RPS だろうが、あとのことは全てバックグラウンドに行われるので皆さんが考える必要がありません。 使った分だけ料金を払うだけでよいので、スケール可能なキャッシュを高額な料金を支払うことなく得られます。 </details>

<details>
  <summary>Will it blend?</summary>
We do not recommend putting Momento Cache in a blender as it may void the warranty of your blender, but Momento Cache  is robust with enterprise grade security and availability. Momento Cache does blend seamlessly with your current cloud setup, whether you're on AWS, GCP, Azure, or multi-cloud! </details>

[Give Momento Cache a Try!](./../getting-started.md)
