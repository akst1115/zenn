---
title: "【VB.NET】Structure(構造体)とClass（クラス）の違いや使い分け"
emoji: "🤔"
type: "tech"
topics:
  - "vbnet"
  - "dotnet"
published: true
published_at: "2024-12-28 19:00"
---

## 本記事の背景
今年の上旬から現在にかけて、今まで扱ったことがなかったVB.NETの案件に参画しました。

初めてVB.NETを扱った際に疑問を感じたStructure(構造体)とClass(クラス) の違WWWWいについて、勉強した内容を元に自分なりに理解した点を年末の振返りも兼ねてまとめておこうと思います。

正直勉強するまでどっちでもいいんじゃね？って思ってました。

## Structure(構造体)とは？

:::message
用語の説明となりますので、ご存知の方は読み飛ばしてください
:::

複数のデータを1単位で扱うことができるデータ構造です。

Structureは値型です。

画面サイズを扱いたい場合、以下のような書き方になります。

```vbnet
Public Structure WindouwSize
    Public Width As Integer
    Public Height As Integer
End Structure
```

## Class(クラス)とは？

:::message
用語の説明となりますので、ご存知の方は読み飛ばしてください
:::

データ構造としては、Structureと同じようなコードの書き方になります。

異なる点として、Classは参照型です。

```vbnet
Public Class PersonalDate 
    Public FirstName As String
    Public LastName As String
    Public Email As String
End Structure
```
## Structure(構造体)とClass(クラス)の違い

### データの渡し方
データをコピーした際のデータの渡し方が異なります。
Structureは値コピーで、Classは参照コピーとなります。

具体的な影響は、Structureはデータ構造が大きくなるにつれてパフォーマンスが悪くなります。
構造体サイズが大きくなることが想定されたり、値を長く持ちまわる場合はClassを使うのが良いです。

逆にデータ構造の小さい構造体や、一度使ったら終わりみたいな値の場合は、Structureが良いです。

### マルチスレッドに対する考え方

Structureは、スレッドセーフなため複数のスレッドで同じ処理を使っても干渉することなく、予期しないデータの不整合が発生しないため、マルチスレッド処理に対して特別考慮が必要ありません。

Classは、参照型のため排他制御などでデータ競合を防ぐ対策が必要です。

## まとめ

上記のことから特別な理由がない限り、Class(クラス)を使用するべきだと考えます。
また、マルチスレッドのような処理がある場合、Structure(構造体)を使いましょう。

最後になりますが、上記情報に補足事項があれば、追記等を行っていくつもりです。

## 参考

https://learn.microsoft.com/ja-jp/dotnet/visual-basic/programming-guide/language-features/data-types/structures

https://learn.microsoft.com/ja-jp/dotnet/visual-basic/programming-guide/language-features/objects-and-classes/

https://learn.microsoft.com/ja-jp/dotnet/visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types