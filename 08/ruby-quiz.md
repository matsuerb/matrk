# Ruby Quiz 松江Ruby会議08

## 一回戦

### Q1. Rubyを開発した人は「まつもとひろゆき」さんである

1. Yes
2. No

答え: 1

まつもと「ゆきひろ」さんです。

### Q2. Rubyの新機能がリリースされる時期と言えば？

1. ゴールデンウィーク
2. お盆
3. クリスマス

答え: 3

毎年、クリスマスに新機能がリリースされています。

### Q3. 下記コードの実行結果は？

```ruby
a = Array.new(3, "foo")
a[0].capitalize!
p a[1]
```

1. "foo"
2. "Foo"

答え: 2

[Array.new](https://docs.ruby-lang.org/ja/2.3.0/method/Array/s/new.html)は、第一引数の長さの配列を生成し、各要素を第二引数の値で初期化します。
このとき、全ての要素は同じオブジェクトになります。

```ruby
a = Array.new(3, "foo")
p a                            #=> ["foo", "foo", "foo"]
p a.map(&:object_id).uniq.size #=> 1
```

[String#capitalize!](https://docs.ruby-lang.org/ja/2.3.0/method/String/i/capitalize=21.html)はレシーバの文字列の先頭の文字を大文字に、残りを小文字に変更し、その値を返します。
!がついているので、レシーバを破壊的に変更します。

問題文のコードでは、配列の全ての要素が同じ文字列オブジェクトであるため、
全ての要素がcapitalize後の値となります。

```ruby
a = Array.new(3, "foo")
a[0].capitalize!
p a   #=> ["Foo", "Foo", "Foo"]
```

### Q4. 下記コードの実行結果は？

```ruby
def hoge
  "a"
  "b" if false
end

p hoge
```

1. "a"
2. "b"
3. nil

答え: 3

Rubyのメソッドは、returnが明示的に指定されていない場合、メソッドで最後に実行した式の値をメソッドの戻り値として返します。
また、if式は、条件式が真とならずに処理を実行しなかった場合、nilを返します。

```ruby
"b" if false  #=> nil
```

そのため、hogeメソッドを実行すると、最終行の実行結果であるnilが返ります。

### Q5. 下記コードの実行結果は？

```ruby
result = Integer("012") + "012".to_i
p result
```

1. 20
2. 22
3. 24

答え: 2

[Kernel.#Integer](https://docs.ruby-lang.org/ja/2.3.0/method/Kernel/m/Integer.html)は、引数が文字列の場合、数字リテラルと解釈し、10進数の整数に変換します。
「012」は8進数で「12」となる数値を表しているため、10進数では「10」となります。

```ruby
p 012   #=> 10
```

次に、[String#to_i](https://docs.ruby-lang.org/ja/2.3.0/method/String/i/to_i.html)は、文字列を10進数の数字と見なして整数に変換します。

```ruby
p "012".to_s  #=> 12
```

そのため、「10 + 12 = 22」となります。

## 二回戦

### Q1. ゲストの松田明さんは日本人で唯一のRuby兼Railsのコミッターである。

1. Yes
2. No

答え: 1

松田明さんは日本人で唯一のRuby兼Railsのコミッターです。

### Q2. 2016年12月25日にリリース予定のRubyのバージョンは何?

1. 2.3.4
2. 2.4.0
3. 3.0

答え: 2

2016年12月25日にリリース予定のRubyのバージョンは2.4.0です。

### Q3. 下記コードの実行結果は？(Ruby 2.3.0, ActiveSupport 5.0.0)

```ruby
1: require 'active_support'
2: require 'active_support/core_ext'
3:
4: foo = ""
5: p foo.try(:baz)
6: p foo&.baz
```

1. 5行目と6行目が同じ結果となる
2. 6行目で NoMethodError が発生する

答え: 2

ActiveSupportで実装されているObject#tryは、引数で指定された名前のメソッドがレシーバに存在する場合はそのメソッドを実行し、レシーバにメソッドが存在しない場合はnilを返します。

一方、同じくActiveSupportで実装されているObject#try!は、メソッドが存在しない場合は NoMethodError が発生します。（メソッドが存在する場合、レシーバがnilの場合はObject#tryと同じ挙動です。）
ActiveSupport4.0.0より前は、Object#tryがObject#try!と同じ挙動で、Object#try!は存在しませんでした。

「&.」はRuby2.3.0から追加された演算子で、人が体育座りをしているように見えることから、通称「ぼっち演算子」と呼ばれています。「object&.foo」という形式でメソッドを呼び出すと、Object#try!とほぼ同じ動きをします。（Object#try!はメソッド名を省略可能ですが、ぼっち演算子は省略不可能という違いがあります。）

Stringにはbazというインスタンスメソッドは存在しませんので、5行目はnil、6行目はNoMethodErrorとなります。

イベント中のクイズではActiveSupportのバージョンを明示していなかったため、どちらも正解としました。

### Q4. 下記コードの実行結果は？

```ruby
*a, b, c = %w(1 2 3 4 5)
x = a * b +c
p x
```

1. ["1", "2", "3", "1", "2", "3", "1", "2", "3", "1", "2", "3", "5"]
2. [4, 8, 12, 5]
3. "142435"

答え: 3

「%w(...)」はスペース区切りで文字列を分割し、それらを要素とする文字列の配列を生成します。

```ruby
p %w(1 2 3 4 5)   #=> ["1", "2", "3", "4", "5"]
```

また、1行目では多重代入が使われています。先頭の変数aに「\*」がついているため、変数bには配列の最後から2番目の要素を、変数cには配列の最後の要素を代入し、残りの要素をすべて変数aに代入します。

```ruby
*a, b, c = ["1", "2", "3", "4", "5"]
p a   #=> ["1", "2", "3"]
p b   #=> "4"
p c   #=> "5"
```

[Array#\*](https://docs.ruby-lang.org/ja/2.3.0/method/Array/i/=2a.html)は、引数が文字列の場合、その文字列を連結文字としてすべての要素文字列に変換した上で連結します。

```ruby
p ["1", "2", "3"] * "4"   #=> "14243"
```

そのため、正解は(3)となります。

ちなみに、Array#\*の引数が整数の場合、配列の全ての要素を整数倍増やした配列を新たに生成します。

```ruby
p ["1", "2", "3"] * 4   #=> ["1", "2", "3", "1", "2", "3", "1", "2", "3", "1", "2", "3"]
```

### Q5. 下記コードの実行結果は？

```ruby
result = []
result << 27.to_s(30)
result << 30.to_s(36)
result << 11.to_s(20)
result << 34.to_s(36)
p result.join
```

1. "ruby"
2. "matz"

答え: 1

[Fixnum#to_s](https://docs.ruby-lang.org/ja/2.3.0/method/Fixnum/i/inspect.html)は、引数の数値を基数として文字列に変換します。

```ruby
p (1..20).map{|n| n.to_s(16) }
=> ["1", "2", "3", "4", "5", "6", "7", "8", "9", "a", "b", "c", "d", "e", "f", "10", "11", "12", "13", "14"]
```

```ruby
result = []
result << 27.to_s(30)
result << 30.to_s(36)
result << 11.to_s(20)
result << 34.to_s(36)
p result  #=> ["r", "u", "b", "y"]
```

[Array#join](https://docs.ruby-lang.org/ja/2.3.0/method/Array/i/join.html)は、引数の文字列を連結文字としてすべての要素文字列に変換した上で連結します。
引数が省略された場合、各要素をそのまま連結します。

```ruby
p ["r", "u", "b", "y"].join   #=> "ruby"
```

1分の制限時間内に回答するという観点で言えば、3文字目が「b」であることに気付くかどうかがポイントであったかと思います。
