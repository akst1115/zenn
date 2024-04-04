---
title: "【GCP】GitHubを用いた継続的なデプロイ【CloudRun】"
emoji: "🌀"
type: "tech"
topics:
  - "docker"
  - "github"
  - "gcp"
  - "cloudrun"
  - "cloudbuild"
published: true
published_at: "2024-04-05 00:12"
---

## 記事背景

先日Docker導入（インストールからコンテナ実行まで）という記事を書きました。
https://zenn.dev/akst/articles/1b911d0d41abf9

どうせならGCPのCloudRunへデプロイするところまで記事にしようと思います。
また、Next.jsでポートフォリオサイトを作った際、変更が発生する度に継続的なデプロイを行うことができたら楽だなと思いましたので、その方法で執筆していきます。

## 前提
CloudRunへ継続的なデプロイするために以下が必要になります。

- GCPアカウント
- GitHubリポジトリ（Dockerfile含む）

Dockerfileの書き方や用意方法がわからない場合、以下を参考にしてください。
https://zenn.dev/akst/articles/1b911d0d41abf9#2.dockerfile%E3%81%AE%E4%BD%9C%E6%88%90

## GCPコンソールからプロジェクト作成
GCPコンソールからプロジェクトを作成します。
https://console.cloud.google.com/

左上の「プロジェクトの選択」を選択し、「新しいプロジェクト」を押下。
![](https://storage.googleapis.com/zenn-user-upload/6e4c811830ad-20240404.jpg)
新しいプロジェクト画面に遷移するので、任意のプロジェクト名（MyProjectなど）で作成。
場所は「組織なし」で一旦問題ありません。

## Cloud Runでサービス作成
先ほど作成したプロジェクトを選択し、画面上部の検索バーに「Run」みたいなワードを入れると、Cloud Runが候補に出てくるので押下。
![](https://storage.googleapis.com/zenn-user-upload/67705ffc5749-20240404.jpg)

Cloud Runのサービス画面に遷移したら、サービス作成を押下。
![](https://storage.googleapis.com/zenn-user-upload/4954eaf84151-20240404.jpg)

サービスの作成に遷移します。
今回はGitHubを利用するので、「リポジトリから継続的にデプロイする」を選択。
![](https://storage.googleapis.com/zenn-user-upload/18902798dadb-20240404.jpg)
次は「CLOUD BUILD を設定」ボタンを押下し、Cloud Buildの設定を行います。

## Cloud Build の設定
ソースリポジトリで下記のように入力し、「次へ」を押下。
リポジトリプロパイダは、GitHub。
リポジトリは、自身で作成したデプロイしたいリポジトリを選択。
![](https://storage.googleapis.com/zenn-user-upload/1faac4d3a3d8-20240404.jpg)

ビルド構成も下記のように入力し、「保存」を押下。
ブランチは、デプロイ対象のブランチ。
ビルドタイプは、「Dockerfile」を選択し、ソースの場所も指定。
![](https://storage.googleapis.com/zenn-user-upload/6c10cf2d5f00-20240404.jpg)
サービスの作成画面に戻ります。
先ほど「CLOUD BUILD を設定」の青いボタンがあったところにソースリポジトリが表示されればOKです。

その下の「設定」についても必要箇所を入力していきます。
サービス名は、Cloud Buildの設定を行うと自動入力。
リージョンは、「asia-northeast1(東京)」。
認証は、未認証の呼び出しを許可を選択。
![](https://storage.googleapis.com/zenn-user-upload/6dc0e127edc5-20240404.jpg)
他にも詳細な設定が続きますが、上記で基本的な設定は完了です。
画面下部の「作成」ボタンを押下

## デプロイ完了
サービスの作成が完了すると、サービスの詳細へ遷移します。
ここで現在のデプロイ状況を見ることができます。
![](https://storage.googleapis.com/zenn-user-upload/4a39cb593d51-20240405.jpg)
「リポジトリからのビルドとデプロイの実行」まで完了すると、デプロイ完了となります。

## 参考資料
本記事は、下記の公式ドキュメントを参考にしました。
https://cloud.google.com/run/docs/continuous-deployment-with-cloud-build?hl=ja