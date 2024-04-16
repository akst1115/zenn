---
title: "【Spring Boot】設定ファイルの形式はどちらが良いのか"
emoji: "🍃"
type: "tech"
topics:
  - "java"
  - "springboot"
  - "yaml"
  - "yml"
published: true
published_at: "2024-03-17"
---

:::message
2024-03-17に投稿した内容と同様です。（GitHub連携時の諸事情により再掲）
:::

## 設定ファイルの使い分けについて
Spring Bootのアプリケーション設定ファイルは、以下2種類の形式が存在します。

- application.properties
- application.yml

書き方の違い、選ぶポイントについて解説します。


## 書式比較表
書式を比較すると、以下の通りです。

| 特性             | application.properties                    | application.yml                         |
|------------------|-------------------------------------------|-----------------------------------------|
| 形式             | キーと値のペア                            | YAML                                    |
| 階層設定     | ドット(`.`)を使った階層表現              | インデントによる階層表現           |

### 具体的な書き方

データベース接続とメールサーバーの設定を書くと、以下のような書き方になります。

```properties:application.properties
#データベース接続設定
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=user
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#メールサーバー設定
spring.mail.host=smtp.example.com
spring.mail.port=587
spring.mail.username=email@example.com
spring.mail.password=emailpassword
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```
```yaml:application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydatabase
    username: user
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver
  mail:
    host: smtp.example.com
    port: 587
    username: email@example.com
    password: emailpassword
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true
```
## 結局どっちが良いの？（選ぶポイント等）
個人的には、**application.yml**の方が良いと感じています。

小規模プロジェクトで設定ファイルの記述内容が少ない場合、キーと値のペアを一目見ればわかるapplication.propertiesの方が好まれるかもしれません。

大規模プロジェクトの場合、設定内容が多くなることでファイルの内容が複雑になる可能性があり、可読性を考慮すると複雑なデータ構造を階層で直感的に表現できるapplication.ymlの方が好まれると思います。

パフォーマンス面の違いも比較対象になるかもれませんが、設定ファイル自体アプリケーションの起動時に読み込む内容なので、システムの動作に影響を与えることはほとんどないです。
ただ、他ライブラリがどちらを推奨しているかは考慮する必要はあるかもしれません。（ライブラリの設定の書き方がどちらかに寄っている可能性があるということ）