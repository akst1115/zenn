---
title: "【Java】java.timeパッケージでバグが発生させそうになった"
emoji: "🐛"
type: "tech"
topics:
  - "java"
  - "mysql"
  - "docker"
published: true
published_at: "2024-04-21"
---

:::message alert
パッケージの不具合ではありません。
実装の想定ミスでバグを発生させそうになったという意味の記事です。
:::

## 記事投稿背景

javaのプログラムで現在日時を取得し、DBへ登録を行うという処理を作成しました。
処理に用いたパッケージは、Java8から導入されたのjava.timeです。
その中に含まれる以下の3つのクラスを使用しました。

- LocalDateTime
- LocalDate
- LocalTime

上記クラスを用いてコーディングを行った際、実行環境によって期待値に差異が生まれました。
その結果を調査した内容が本記事になります。

## 主なクラスの説明

使用した3つのクラスについて簡単な説明します。
今回必要な部分のみ説明を行いますので、詳細は下記ドキュメントを参照ください。
https://docs.oracle.com/javase/jp/8/docs/api/java/time/package-summary.html

### LocalDateクラス

LocalDateは、日付を表します（年、月、日）
下記のようなコードで現在日付が取得可能です。
```java:java
LocalDate nowDate = LocalDate.now();
```

### LocalTimeクラス

LocalTimeは、時間のみを表します（時、分、秒、ナノ秒）
下記のようなコードで現在時間が取得可能です。
```java:java
LocalTime nowTime = LocalTime.now();
```

### LocalDateTime

LocalDateTimeクラスは、日付と時間の両方を表します（年、月、日、時、分、秒、ナノ秒）
下記のようなコードで現在日時が取得可能です。
```java:java
LocalDateTime nowDateTime = LocalDateTime.now();
```

## クラス利用時の注意点

上記3つのクラスを利用した際、実行環境で処理結果に差異が生まれる可能性があります。
原因は、それぞれのクラス名の頭についている「Local」から推測できるかと思いますが、実行環境の現在日時を取得するため、タイムゾーンが付加されません。

Dockerは、通常Linux上で動作するため、何も設定しないデフォルトのタイムゾーンで動作させると、タイムゾーンをUTC（協定世界時）で扱われます。
Windows環境は、通常そのPCが設置されている地域のタイムゾーンに設定されるため、（日本にいる場合）タイムゾーンはJST（日本標準時、UTC+9）に設定されます。

仮にVSCode等で単体テストをWindows環境で行い、結合テストをDocker環境で行ったりすると、値が変わってしまうという事象が発生します。

## 対策方法

### クラスをZonedDateTimeに変更する

私が実施した方法は、LocalDateTimeをZonedDateTimeに変更しました。
ZonedDateTimeクラスは、ZoneIdを設定することでタイムゾーンを付加した日時を取得できます。

日本の現在日時を取得する場合、下記のようなコーディングになります。
```java:java
ZoneId zoneId = ZoneId.of("Asia/Tokyo");
ZonedDateTime now = ZonedDateTime.now(zoneId);
```
取得日時から時間や分のみ取得する方法もあるので、詳細は下記ドキュメントを参照ください。
https://docs.oracle.com/javase/jp/8/docs/api/java/time/ZonedDateTime.html

## まとめ

具体的な対策方法は、いろいろあると思います。（試していませんが、Dockerfileでタイムゾーンを設定する等）
今回伝えたい内容は、同クラスを利用しても環境によって期待値に差異が生まれる可能性があるため、クラスの仕様を理解して使う必要があるということです。