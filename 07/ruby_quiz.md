# Ruby Quiz

## Q. メモリ消費が少ないのはどちらか: A. (2)

(1)
    ary = [1,2,3]
    ary = ary + [4,5,6,...]
    p ary.length
(2)
    ary = [1,2,3]
    ary.concat([4,5,6,...])
    p ary.length

## Q. 無限ループにならないのはどちらか: A. (1)

(1)
    require "prime"
    p (1..Float::INFINITY).lazy.select {|i| i.prime? }.first(5)
(2)
    require "prime"
    p (1..Float::INFINITY).select {|i| i.prime? }.first(5)

## Q. コメントの通りに実行できるのはどちらか: A. (2)

(1)
    (1..10).to_a.split(3).each { |a| p a }
    # => [1, 2, 3]
    #    [4, 5, 6]
    #    [7, 8, 9]
    #    [10]
(2)
    (1..10).each_cons(3) { |a| p a }
    # => [1, 2, 3]
    #    [4, 5, 6]
    #    [7, 8, 9]
    #    [10]

◯ 認定試験ぽいパターン

## Q. m = /(?<ysym>昭和|平成)(?<y>\d+)年(?<m>\d+)月/.match("平成27年9月")の実行後に関する以下の記述のうち、間違っているもの(1つ)はどれか: A. (3)
(1) m[:ysym]が"平成"を返す
(2) m[:y]が27を返す
(3) m[2]が9を返す

## Q. 以下のうち、間違っているもの(1つ)はどれか(Rubyは2.2とする): A. (1)
(1) {"key": "value"}.keys が ["key"] を返す
(2) {"key": "value"}.values が ["value"] を返す
(3) {key: "value"}.keys が [:key] を返す
(4) {key: "value"}.values が ["value"] を返す

## Q. 以下の内容のプログラムを-t "2015/09/26 10:00:00" を指定して実行した時にTimeが出力されるものはどれか: A. (3)
  require 'optparse/time'
  opts = OptionParser.new
  opts.on("-t VAL", [  A  ]){|v|
    p v.class
  }
  opts.parse!
(1) String
(2) Object
(3) Time

## 問題1

```ruby
h1 = {foo: 1, bar: 2}
h2 = {foo: 3, baz: 4}
h = {**h1, foo: 5, **h2}
p(h)
```

ruby-2.2.3で実行するとどっち?

1. {:foo=>3, :bar=>2, :baz=>4}
2. SyntaxErrorが発生する．

## 問題2

速いのはどっち?

```ruby
# 1
h1 = {foo: 1, bar: 2}
h2 = {foo: 3, baz: 4}
10_000_000.times do
 h = h1.merge(h2)
end
```

```ruby
# 2
h1 = {foo: 1, bar: 2}
h2 = {foo: 3, baz: 4}
10_000_000.times do
 h = {**h1, **h2}
end
```

## 問題3

速いのはどっち?

```ruby
# 1
srand(0)
1_000_000.times.to_a.shuffle.sort { |a, b| b <=> a }
```

```ruby
# 2
srand(0)
1_000_000.times.to_a.shuffle.sort_by { |item| -item }
```

## 問題4

速いのはどっち?

```ruby
# 1
srand(0)
1_000_000.times.to_a.shuffle.sort_by { |item| -item }
```

```ruby
# 2
srand(0)
1_000_000.times.to_a.shuffle.sort.reverse
```














## 問題1の答え

正解は1です．

関連して https://bugs.ruby-lang.org/issues/11501 がありますので，
2.2.3に限定してます．

ちなみに

* 2.1.7だと{:foo=>5, :bar=>2, :baz=>4}
* 2.0.0-p647だと{:foo=>5, :bar=>2, :baz=>4}
* 1.9.3-p551だとSyntaxErrorが発生

## 問題2の答え

正解は1です．

ruby-2.2.3を使って次のようにして調べました．

```sh
% ruby -rbenchmark -e 'Benchmark.bm do |x| x.report("1") do

# 1
h1 = {foo: 1, bar: 2}
h2 = {foo: 3, baz: 4}
10_000_000.times do
 h = h1.merge(h2)
end

end; x.report("2") do

# 2
h1 = {foo: 1, bar: 2}
h2 = {foo: 3, baz: 4}
10_000_000.times do
 h = {**h1, **h2}
end

end; end'
```

自分のマシンだと次のようになり，

1. merge
    * 9.480000   0.000000   9.480000 (  9.473857)
    * 9.620000   0.000000   9.620000 (  9.623106)
    * 9.570000   0.000000   9.570000 (  9.564833)
2. リテラル
    * 11.490000   0.000000  11.490000 ( 11.490704)
    * 11.120000   0.000000  11.120000 ( 11.116732)
    * 11.120000   0.000000  11.120000 ( 11.128686)

mergeの勝利でした．

## 問題3の答え

正解は2です．

## 問題4の答え

正解は2です．

ruby-2.2.3で次のようにして調べました．
srandでshuffleが常に同じ順番になるようにしていて，
0だからこの結果になるのかと考えていくつか別な数値でも試してみたのですが，
いずれもsort+reverseの勝利でした．

```sh
% ruby -rbenchmark -e 'Benchmark.bm do |x| x.report("sort block") do

srand(0)
1_000_000.times.to_a.shuffle.sort { |a, b| b <=> a }

end; x.report("sort_by") do

srand(0)
1_000_000.times.to_a.shuffle.sort_by { |item| -item }

end; x.report("sort+reverse") do

srand(0)
1_000_000.times.to_a.shuffle.sort.reverse

end; end'
      user     system      total        real
sort block  1.900000   0.010000   1.910000 (  1.901351)
sort_by  1.160000   0.020000   1.180000 (  1.191335)
sort+reverse  0.350000   0.000000   0.350000 (  0.351530)
      user     system      total        real
sort block  1.870000   0.010000   1.880000 (  1.878990)
sort_by  1.200000   0.010000   1.210000 (  1.195921)
sort+reverse  0.330000   0.010000   0.340000 (  0.347252)
      user     system      total        real
sort block  1.890000   0.010000   1.900000 (  1.903909)
sort_by  1.190000   0.010000   1.200000 (  1.196674)
sort+reverse  0.340000   0.020000   0.360000 (  0.362393)
      user     system      total        real
sort block  1.890000   0.020000   1.910000 (  1.903590)
sort_by  1.160000   0.010000   1.170000 (  1.176643)
sort+reverse  0.350000   0.000000   0.350000 (  0.350978)
      user     system      total        real
sort block  1.850000   0.010000   1.860000 (  1.858848)
sort_by  1.170000   0.010000   1.180000 (  1.176233)
sort+reverse  0.340000   0.000000   0.340000 (  0.346719)
```---

# Ruby 2.2系ならば標準出力にtrueが出力される。
#
# 答え: ○
# 配列内の要素はすべて変更不可なのでall?メソッドが要素ごとにfrozen?メソッドを呼び出してtrueが返ってきた結果、trueが標準出力に出力される。
p [3.14, nil, true, false, 42, :foo].all?(&:frozen?)

---

module Foo
def foo
puts("foo")
end
end

class Bar
include Foo
def foo
super
puts("bar")
end
end

class Baz < Bar
def foo
puts("baz")
end
end

# Ruby 2.2系の標準出力に以下が出力される。
#
# foo
# baz
#
# 答え: ○
# Bazクラスがオーバーライドしているfooメソッドがsuper_methodメソッドで呼ばれる。
Baz.new.method(:foo).super_method.call
class KittyWhite
  @@birthday = Date.new(1974,11,1)
  def self.age
    date_format = "%Y%m%d"
    (Date.today.strftime(date_format).to_i - @@birthday.strftime(date_format).to_i) / 10000
  end

  def self.weight
    "● apples"
  end
end

=begin
このプログラムを完成させるためには●に数字を入れなければなりません。
さて、その数字は？
A.5
B.7
=end
puts MatzDiary.last_update

=begin
「Matzにっき」の全てがわかる仮想クラスMatzDiaryがあります。
最後の…最新の更新日を返すメソッドを実行した時に返される結果は、次のうちどれでしょう。

A.2014-12-22
B.2015-2-22

=end
```
a = (1..9).reduce('') do |sum, value|
  sum.to_s + value.to_s
end

b = (1..9).reduce(0) do |sum, value|
  sum + value
end

# aとbを数値にして比較したとき、大きいのはa, bのどちらでしょうか？
# puts "a: #{a}, b: #{b}"
```
