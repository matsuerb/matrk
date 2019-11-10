# Ruby Quiz 松江Ruby会議09

以下では特に断りがない限りRubyのバージョンを2.5.1とします。

## 第１回戦

### Q1. 下記Rangeオブジェクトのうちeachでまわせないものは？

1. 1..10
2. 1.0..10.0
3. "a".."z"

答え: 2

「1..10」はIntegerのRangeオブジェクト、「"a".."z"」はStringのRangeオブジェクトで、それぞれsuccメソッドが定義されています。
「1.0..10.0」はFloatのRangeオブジェクトですが、Float#succメソッドは定義されていませんのでeachメソッドを実行すると例外になります。ただし、FloatのRangeオブジェクト生成は可能でエラーとはなりません。

### Q2. 下記コードの実行結果は？

```ruby
val = ["ruby", "python", "java"].map do |n|
  puts n.capitalize
end
p val # <=この結果
```

1. ["Ruby", "Python", "Java"]
2. ["ruby", "python", "java"]
3. [nil, nil, nil]

答え: 3

Array#mapは、配列の各要素を取り出してブロック内で処理した結果（戻り値）で置き換える機能を持ちます。
ブロック内では、先頭文字を大文字に変換するString#capitalizeの戻り値を、Kernel#putsの引数として渡しています。Kernel#putsの戻り値は常にnilであるため、結果はnilが３つ入った配列となります。

### Q3. 2017年10月時点のGitHubプルリク数でRubyは何位？

1. 当然の１位
2. 僅差の２位
3. まさかの４位

答え: 3

参照：https://octoverse.github.com/2017/
１位はJavascript、２位はPython、３位はJava、そして４位がRubyとなっています。

### Q4. did_you_meanで見つけることができないのは？（ただし、did_you_mean 1.2.0とする。）

1. メソッド名/クラス名の間違い
2. ハッシュキーの間違い
3. グローバル変数の間違い

答え: 3

「did you mean」は、スペルミスなどのTypoによるエラー発生時に、正しい名前の候補を教えてくれる便利なgemです。
クラス名やメソッド名の場合は、問題なく修正の候補を提示してくれます。以下はクラス名の例をirbで実行した結果です。

```
irb(main):001:0> p Arrai.new
Traceback (most recent call last):
        2: from /Pass/To/irb:11:in `<main>'
        1: from (irb):2
NameError (uninitialized constant Arrai
Did you mean?  Array)
```

次は、メソッド名の例です。

```
irb(main):002:0> p [1,2,3].swift
Traceback (most recent call last):
        2: from /Pass/To/irb:11:in `<main>'
        1: from (irb):3
NoMethodError (undefined method `swift' for [1, 2, 3]:Array
Did you mean?  shift)
```

次は、ハッシュキーの例です。

```
irb(main):003:0> {foo: 1,bar: 2}.fetch(:baz)
Traceback (most recent call last):
        3: from /Pass/To/irb:11:in `<main>'
        2: from (irb):4
        1: from (irb):4:in `fetch'
KeyError (key not found: :baz
Did you mean?  :bar)
```

最後にグローバル変数の例ですが、この場合、エラー出力のみとなります。
did_you_meanライブラリは'did_you_mean/experimental'をrequireする事で実験的に提供されてる種類のスペルミス修正が行えますが、それを含めても出来ません。

```
irb(main):004:0> $qux = 1; p $quux.zero?
Traceback (most recent call last):
        2: from /Pass/To/irb:11:in `<main>'
        1: from (irb):5
NoMethodError (undefined method `zero?' for nil:NilClass)
```

### Q5. 下記コードの実行結果は？

```ruby
p [*1..3] * ':'
```

1. "1:2:3"
2. {:1 => nil, :2 => nil, :3 => nil}
3. nil

答え: 1

`[*1..3]`は`[1, 2, 3]`と同じです。配列を生成する際に引数として`*1..3`を渡していますが、`*`付きで指定された引数は展開して渡されます。この際 to_a メソッドが呼ばれますので、`(1..3).to_a`された結果が`[]`内部で展開されて`[1, 2, 3]`を指定した事になります。

```ruby
p [*1..3] #=> [1, 2, 3]
```

配列に文字列の掛け算を行うと、配列の要素をその文字列で結合した文字列が返ります。

```ruby
p [1, 2, 3] * ':' #=> "1:2:3"
```

## 第２回戦

### Q1. 機械学習やデータ解析で欠かせないライブラリは？

1. Nemu
2. Daru
3. Bateru

答え: 2

Daru（Data Analysis in RUby）です。
データフレームという表計算ソフトのようなデータ構造を扱うことができるGemです。同じようにデータフレームを扱えるライブラリですとPythonのPnadasが有名です。
科学技術計算やデータ可視化のGemを開発しているSciRubyプロジェクトの一つです。
参照：https://github.com/SciRuby/daru

### Q2. 下記コードの実行結果は？

```ruby
enum = ("ruby".."streem").to_enum
p enum.next
```

1. mruby
2. ruby
3. rubz

答え: 2

`"ruby".."streem"`は文字列のRangeオブジェクトです。また、Range#to_enumを使うとEnumeratorオブジェクトが返ります。Enumeratorクラスは、Enumerableモジュールをeachメソッドの定義なしに使えるようにするためのラッパークラスです。
Enumerator#nextは、列挙されたオブジェクトを先頭から順番に取り出すことができます。問題の例では場合は、"ruby"が返ります。Enumerator#nextを繰り返し実行することで、"rubz"、"ruca"、"rucb"・・・"streem"と取り出すことができます。
したがって、Enumerator#nextの初回実行では"ruby"が戻り値となります。

### Q3. テキストファイルの内容を出力するオプションは？

```
$ ruby [オプション] test.txt
```

1. -ne ''
2. -pe ''
3. -e 'while gets print $_ end'

答え: 2

rubyコマンドのオプションの問題です。`-e`オプションはよく使われるオプションで、渡されたスクリプトを実行します。
`-n`オプションは、プログラム全体が以下の`...`の部分で実行されるように動作します。

```
while gets
  ...
end
```

`-p`オプションは、`-n`オプションとほぼ同じですが、各ループの最後に`$_`を出力します。問題の例では、test.txtの内容を一行ずつ取り出して出力します。
選択肢の3番は、問題なく実行されるように見えますが、改行が考慮されていません。`-e 'while gets; print $_; end'`のように、改行位置に`;`を入れるか、`-e 'while gets' -e 'print $_' -e 'end'`と区切ることで、ファイルの内容を出力することができます。
したがって、テキストファイルの内容を問題なく出力できるのは2番のみとなります。

### Q4. 下記コードの実行結果は？

```ruby
"MatsueRuby09 in 2018" =~ /\d+/
p $'
```

1. "MatsueRuby"
2. " in 2018"
3. "2018"

答え: 2

これは、正規表現の問題です。`\d`は、10進数字(0から9)1文字にマッチします。`+`は、1回以上の繰り返しを表現しますので、`\d+`は、10進数字が1回以上となり、`09`がマッチします。
`$'`は、マッチした文字列以降の文字列を格納する特殊変数ですので、問題の例では` in 2018`を持っています。

### Q5. digメソッドを持たないオブジェクトは？

1. Hash
2. Struct
3. ENV

答え: 3

digメソッドは、ネストしたオブジェクトを再帰的に参照するメソッドです。メジャーなクラスでは、Array#dig, Hash#digが実装されています。
以下は、実際にirbで実行した結果です。

```
irb(main):001:0> [[1, 2, 3], 4, 5].dig(0, 1)
=> 2

irb(main):002:0> {a: {b: 1}, c: 2}.dig(:a, :b)
=> 1
```

Struct#digも実装されています。

```
irb(main):001:0> St = Struct.new(:a, :b, :c)
=> St
irb(main):002:0> St.new(St.new(1, 2, 3), 4, 5)
.dig(:a, :b)
=> 2
```

環境変数を扱うENVオブジェクトにdigメソッドは実装されていません。

```
irb(main):003:0> ENV.dig("foo", "bar")
Traceback (most recent call last):
    2: from /Pass/To/irb:11:in `<main>'
    1: from (irb):5
NoMethodError (undefined method `dig' for #<Object:0x00007fb7e285cfb8>)
```
