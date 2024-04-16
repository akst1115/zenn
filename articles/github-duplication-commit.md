---
title: "【GitHub】リモートリポジトリを複製する【コミット履歴有】"
emoji: "🐙"
type: "tech"
topics:
  - "git"
  - "github"
  - "vscode"
published: true
published_at: "2024-02-28"
---

## 本記事の背景
個人開発で一からプロジェクトを作成するのが面倒だったので、既存製作物のリモートリポジトリを複製し新規リポジトリを作成することにしました。

## 操作手順
1. GitHubでコピー先リポジトリを新規作成
2. コピー元リポジトリをclone
3. コピー先リポジトリへPush

コマンド操作は、VSCodeのターミナルで行いました。

## 1.GitHubでコピー先リポジトリを新規作成

既存でリポジトリがある前提なので、リポジトリの作成手順は必要ないかもしれませんが、念のため記載しておきます。

1. https://github.com/ へアクセス
2. 右上の「+」ボタンを押下し、「New repository」を選択
3. 「Create a new repository」でリポジトリ名等を任意で入力し、「Create repository」を押下し登録

## 2.コピー元リポジトリをclone
コピー元リポジトリのベアリポジトリをcloneします。
コマンドは以下の通りです。

```
git clone --bare https://github.com/username/コピー元リポジトリ.git
```

## 3. コピー先リポジトリへPush

先ほどcloneしたコピー元リポジトリをgithubで作成したコピー先リポジトリへPushを行います。
ローカルリポジトリの内容をリモートリポジトリに完全にミラーリングするためのコマンドを用いて、Pushを行います。
コマンドは以下の通りです。

```
cd cloneしたコピー元リポジトリ.git
git push --mirror https://github.com/username/コピー先リポジトリ.git
```

上記を実行することでローカルリポジトリにあるすべてのブランチ、タグ、およびリモート追跡ブランチをリモートリポジトリにPushすることができ、これでコピー先リポジトリに複製が完了します。

## まとめ
今回の手順情報ソースは、githubで公開されている「リポジトリを複製する」を参考にしました。
https://docs.github.com/ja/repositories/creating-and-managing-repositories/duplicating-a-repository

公式情報を毎度見に行ってもいいのですが、チートシート的なすぐわかる備忘録があると良いと思ったので、今回記事にさせていただきました。
