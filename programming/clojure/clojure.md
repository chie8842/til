# Clojure

## 公式サイト
https://clojure.org/

日本語訳してくれている人がいる。
https://japan-clojurians.github.io/clojure-site-ja/

## 概要（Wikipedia抜粋）
Clojure (発音は/'klouʒər/[2], クロージャー)はプログラミング言語であり、LISP系の言語の方言の一つである。関数型プログラミングのプログラミングスタイルでのインタラクティブな開発を支援し、マルチスレッドプログラムの開発を容易化する汎用言語である。Clojure言語のプログラムはJava仮想マシンとMicrosoft .NET 共通言語ランタイムで動作する。Clojure言語は「データとしてのプログラムコード」 (英語:「code as data」) という思想で設計されており、洗練されたマクロ機構を持つ。

## Clojureを試すには

幾つかの方法がある

1. Minimal Install
Download clojre-X.X.X.jar and execute following command

```
java -cp clojure-1.8.0.jar clojure.main
```

2. Online Repl Environment
  * [repl.it](https://repl.it/languages/clojure)
  * [TryClojure](http://tryclj.com/)

3. Leiningen
JavaやScalaでいうmaven, gradle, sbtのようなビルドシステム。
leiningenをインストールすると、以下のコマンドでreplが使える

```
lein repl
```

4. Boot
BootはClojureのビルドフレームワークで、Clojureスクリプトをアドホックに評価するもの。
[Clojure の Boot というビルドツールについて](http://ayato.hateblo.jp/entry/20150502/1430560799)

```
boot repl
```

## クラスパスを表示する方法

```
(use 'clojure.contrib.pprint)
(pprint (seq (.getURLs (java.lang.ClassLoader/getSystemClassLoader))))
```
もしくは以下
```
(use 'clojure.contrib.classpath)
(classpath)
```

参考：http://d.hatena.ne.jp/fatrow/20100405/1270483781

# defとdefnの違い

defは引数をとらない。
定義時に呼び出される。

* defの実行結果

```
ex2_1=> (def sample1 "Hello")
#'ex2_1/sample1
ex2_1=> sample1
"Hello"
; 引数xを与えるとエラー
ex2_1=> (def sample2 [x] "Hello")

CompilerException java.lang.RuntimeException: Too many arguments to def, compiling:(/private/var/folders/w2/_5svft1j22b7hn867y2gh4v40000gp/T/form-init5777623528652159598.clj:1:1)
```

* defnの実行結果

```
; 引数を与えないとエラー
ex2_1=> (defn sample3 "Hello")
IllegalArgumentException Parameter declaration missing  clojure.core/assert-valid-fdecl (core.clj:7181)
ex2_1=> (defn sample4 [x] "Hello")
#'ex2_1/sample4
ex2_1=> sample4
#function[ex2-1/sample4] ;"Hello"が出力されない
ex2_1=> (sample4 "a")
"Hello" ;括弧で囲むと実行結果が出力される
```

defは定数の定義、defnが関数の定義？

## if文の書き方
ifを使った書き方、whenを使った書き方、condを使った書き方がある。

```
user=>
(defn is-small? [number]
  (if (< number 100) "yes" "no"))

#'user/is-small?

user=> (is-small? 50)
"yes"
user=> (is-small? 200)
"no"
```

* whenを使った書き方

(when test 式*)という書き方。
```
user=>
(defn is-small? [number]
  (when (< number 100) (println "number = " number) "yes"))

#'user/is-small?
user=> (is-small? 30)
number =  30
"yes"
user=> (is-small? 100)
nil

```

```
ex2_1=> 
(defn pos-neg-or-zero
  "Determines whether or not n is positive, negative, or zero"
  [n]
  (cond
    (< n 0) "negative"
    (> n 0) "positive"
    :else "zero"))
#'ex2_1/negaposi
ex2_1=> (negaposi 5)
"positive"
ex2_1=> (negaposi -1)
"negative"
ex2_1=> (negaposi 0)
"zero"
```

参考: http://d.hatena.ne.jp/ryoasai/20110604/1307201476

## ローカル束縛

defはグローバル変数を作る。
関数内のローカル変数的にを定義する際は、letを使う

(let [変数名 値 ...] 処理)

```
ex2_1=> (let [x 42] println x)
42
ex2_1=> (let [x 42 y 100] println (+ x y))
142
```

