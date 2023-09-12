---
sidebar_position: 3
sidebar_label: AWS Secrets Manager
title: Momento + AWS Secrets Manager
description: AWS Secrets ManagerからMomento認証トークンを取得する方法を学ぶ。
---

import { SdkExampleTabs } from "@site/src/components/SdkExampleTabs";
// This import is necessary even though it looks like it's un-used; The inject-example-code-snippet // plugin will transform instances of SdkExampleTabs to SdkExampleTabsImpl import { SdkExampleTabsImpl } from "@site/src/components/SdkExampleTabsImpl";

# AWS Secrets ManagerからMomento認証トークンを取得する

Momento認証トークンを安全に保存することがベストプラクティスです。 AWSをご使用の場合は、Momento認証トークンを[AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)に安全に保存することができます。 そして、正しいIAMクレデンシャルで実行されているコードのみがMomento認証トークンを取得し、そのトークンを使ってMomento CacheまたはMomento Topicsにアクセスできるようになります。

:::info

可能であればMomentoの`CacheClient`と`TopicClient`オブジェクトを再利用した方がいいように、AWS Secrets Managerから取得したMomento認証トークンを使ってこれらのオブジェクトを再利用した方がよいです。 再利用しなければ、AWS Secrets Managerへのラウンドトリップに対する各MomentoAPI呼び出しに対して、時間面と金銭面の両方から負担が増える危険性が生じます。 負担を抑え、AWS Secrets Managerによって管理される可能性を排除するには、この[AWSブログ](https://aws.amazon.com/blogs/security/use-aws-secrets-manager-client-side-caching-libraries-to-improve-the-availability-and-latency-of-using-your-secrets/)で詳細にご説明させていただいた通り、クライアント側のクレデンシャルのキャッシュを使用するのがベストです。
:::

:::

## AWS Secrets Managerへの入力

Momento認証トークンをAWS Secrets Managerへ入力する際は、下のスクリーンショットの通り、JSONを含まないプレーンテキストとして入力するようにしてください（セキュリティのためトークンにはぼかしを入れております）。 (Token blurred for security.)

![AWS Secrets Manager](/img/aws-secrets-manager.png)

## AWS Secrets Managerのコード例

<SdkExampleTabs snippetId={'API_retrieveAuthTokenFromSecretsManager'} />

## よくある質問（FAQ）

<details>
  <summary>Do I have to do this?</summary>
No. You don't have to store the Momento auth token in something like AWS Secrets Manager, but it is best practice. You could pass the Momento auth token in from an environment variable. </details>
