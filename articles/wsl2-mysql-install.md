---
title: "【WSL2】WSLでMySQLをインストールする方法"
emoji: "⚙️"
type: "tech"
topics:
  - "linux"
  - "mysql"
  - "vscode"
  - "wsl"
  - "wsl2"
published: true
published_at: "2024-02-21"
---

## 記事投稿背景
WSL2上でMySQLが必要になるタイミングがありました。  
今後の備忘録として、記事投稿をさせていただきます。  

## 事前準備  
事前にWSL2のインストールし、実行できる環境が必要になります。
私はコマンドライン操作をVSCodeのターミナルで行いました。

- WSL2（必須）
- VSCode（任意）

## MySQLをインストール手順説明
以下、コマンドを順番にターミナル上で実行してください。

### wsl2を起動
```
wsl
```

### パッケージを更新
```
sudo apt update
```

### MySQL をインストール
```
sudo apt install mysql-server
```
### インストール後、バージョン番号を取得
```
mysql --version 
```

### MySQL サーバーの起動/状態の確認（任意）
```
systemctl status mysql
```

### MySQL プロンプト起動
```
sudo mysql
```
## まとめ
基本的には、上記でMySQLが使えるようになります。  
詳細な内容を知りたい場合、Microsoftが提供している「[Linux 用 Windows サブシステム上のデータベースの概要](https://learn.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-database#install-mysql)」を参照してください。