# 結局mapって何なんだってばよ
最近競プロでalgorithmと効率化メソッドをお勉強中です。その中で1個ガチで疑問なのが、標準入力の存在。与えられた数値に対して標準入力を通してそこで初めて書いたコードに適用できる、みたいな感じらしいが今まで標準入力とやらをやってこなかったのでqiitaから引っ張ってコピペ職人をしている現状(これ良くないよね、早く直します)
その中でやたらと目にするのが`map()`である。例えば最近見たやつでは

```python
l = list(map(int, input().split()))#lはlistとして与えられる
```

(なにこれ)

というわけで`map()`の使い方がよくわからんのでまとめてみた

## 前提
`map(function,iterable)`とargを設定することでiterableをiteratorにすることができる。iteratorはfor文で回せる他、`next()`のargに設定することで関数1個につき1回進められる。また回数オーバーすると`StopIteration`のエラー吐いて止まる。

また`map()`が返すのはiteratorなので、`map(fn,map())`とすることも可能(iteratorはiterableだから)

EX

```python
numbers = map(int,['1','2,','3'])
print(next(numbers)) #1が数字で返る
```

というのが`map()`の概要である 厳密には、`map(callable,iterable)`らしいが、面倒なのでここでは省略。要は第2引数に与えたiterableに対して、第1引数で与えた処理式を使い、iterableをiteratorにするというものである。

なんだかiterableとiteratorがごちゃってきたのでここで整理する

- iterator ; list,dic,tuple,strとか、`for in` のinの後に渡せるやつ
- iterable ; = iterable ただし、`next()`とかで取り出すと要素は元に戻らず、全て取り出した場合消滅する iterableの場合は元データが保持される
## for文でよくね問題
ここで生じる疑問が`for in`でよくね？？？である。`for hoge in iterable`を使うことで、

1. iterableをiteratorに変換 
2. iterableに対してhogeをindex0から順に回していく

が可能になる。どう考えても`iter`やら`next`やらを使用するよりこちらの方がいいのは自明であるし、実際`next()`とかこの使い方ではほぼ使わないらしい。

iteratorの最大のメリットはclass定義の際に発揮する。
1. `__iter__`の特殊メソッドにより、argをiterator化
2. methodのargにiteratorを渡すことで可読性が向上する

という感じらしい、例が思いつかなかった。

またiteretorの場合、繰り返し処理の回数を自分で好きに決めることができる。forの場合rangeやらitearbleをすべて取り出すまで止まらないので、無駄に処理をしすぎて負荷やら時間やらがかかるというデメリットが有る。

## mapのメリット
iterable/iteratorやらを整理していたら冗長になってしまった。ここで本題の`map()`である。

`map(fn,iterable)`と使えるので、処理式をシンプルに済ますことができるというのがメリットである。
```python
def hoge(l):
    result = [i*i for i in l]
    return sum(result)
print(hoge(list1))
```
が
```python
result = sum(map(lambda num : num*num, list1))
print(result)
```
でかけた。読みやすさは別にして、非常にシンプルである。

また`map()`はiteratorを返す、という性質により扱いやすいlistを作れる。下は与えられた空白のある文字列を`split()`で区切り、`int`で整数化、`list`でlistにする処理である。
```python
num_str = "5 10 -2 1" #文字列なので処理できない
num_list =list(map(int, num_str.split()))
print(num_list) #[5,10,-2,1]のlistにできたので処理ができる
```

さらに、inputと組み合わせることで、**標準入力に対応できる**
```python
input_list = list(map(int,input().split()))
```
1. `input()`で入力された文字列を受け取る (文字列はiterableなので`map()`のargとして渡せる！)
2. 受け取った文字列を`split()`で分割
3. `int`で分割された文字列を整数化
4. `list`で扱いやすいlistの形にする

という処理が`map()`一つで完結するのである。mapすごい。先程挙げたコード

`l = list(map(int, input().split()))#lはlistとして与えられる`

もこの処理と全く同じである。

### おわり
iterable/iteratorの扱いをよく理解できたし、mapが結局何をしているのか、ということが上手く整理できた気がする。標準入力を自力で組めるようになりたいですね。
