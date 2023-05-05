# はじめに
　このファイルは、解説書の内容に一通り目を通した人向けの内容となっています。  
`mass` や `candidate`, `amorphous` などの言葉の解説はしません。ご注意ください。  

# レベル1
まずは、`matter` や `result` が必要ないレシピから作成していきましょう。  
```yaml
name: tutorial_1
tag: normal
result: stone
coordinate: 
  - cobblestone
```  
早速ですが上記のレシピの解説を行っていきます。  
このレシピファイルは、「丸石を石へ変換する」レシピになっています。  
`name` は適当に、重複しない名前を設定してください。(このチュートリアルでは`tutorial_連番`という名前を使用します)  
`tag` には `normal` という値が設定されていますね。これはこのレシピが定型レシピであることを意味しています。  

続いて `result` にはレシピの成果物が指定されています。  
今回は `stone` が指定されているので、データを何もいじられていない、個数が1の石が指定されています。  

`coordinate` には丸石 (`cobblestone`) が指定されており、このレシピに必要な素材は丸石1個であることが分かります。  

そして、このレシピファイルには全く同じ振る舞いをする別の書き方があることに気づきましたか?  
気づくことが出来た方はエクセレントです。  
このレシピは素材を1つしか必要としていません。それゆえにアイテムの配置を気にする必要がありません。  
なぜなら素材が1つの場合はどこに配置しても変わらないからです。  

そのことを踏まえて、以下のレシピも `tutorial_1` と同じ結果を返すことを確認してみましょう。  
```yaml
name: tutorial_2
tag: amorphous
result: stone
coordinate: [cobblestone]
```  
さて、 `tutorial_1` と何が変わったでしょうか。  

そうです、 `tag` と `coordinate` が変化していますね。(`name` が変化しているのも正解と言えば正解ですが......)  
これは「丸石を石へ変換する **不定形レシピ** 」となります。  
不定形なのでアイテムの配置を気にする必要はありません。よって、より簡単なリストの形式を使用して記述しています。  

次は特殊なアイテムの書き方について学びます。  
`coordinate` にはアイテム名の他に、`Matter` ファイルに記述した名前を用いることが出来ますが、それについては後述することとして、レベル1では `Null` と `water_bottle` について学びます。  
```yaml
name: tutorial_3
tag: normal
result: stone
coordinate:
  - cobblestone,cobblestone
  - cobblestone,null
```  
`null` という文字列が登場しました。  
何もない、でお馴染みの `null` をカスタムクラフターでは、アイテムが存在しない状態のスロット、を表すために使用します。  
なので、上記のレシピは丸石をL字状に配置して石を得るレシピを示しています。  

```yaml
name: tutorial_4
tag: normal
result: water_bucket
coordinate:
  - water_bottle,null
  - bucket,null
```
上記のレシピに少々不思議なものが紛れ込んでいると気づいた方は、「それなりに」マインクラフトに精通しているといえるでしょう。  
これまで `coordinate` セクションに指定してきたのはバニラのアイテムIDと `null` のみでした。  
しかし、 `water_bottle` というのは何でしょう。マインクラフトのゲーム内で `/give @s water_bottle` を実行してもエラーになってしまいます。  

大半の方が文字をぱっと見て何を指し示しているか分かったと思いますが、 `water_bottle` は水入り瓶のこと指しています。  
水入り瓶は内部的には「効果を持たないポーション」として扱われているので、デフォルトのカスタムクラフターでは少々扱いが面倒なアイテムとなっていました。  
そのため、使いやすいように `water_bottle` としてあらかじめ定義しておくことで、利用しやすいようにしています。  

それらを踏まえて、 `tutorial_4` がどのようなレシピを指しているか考えてみましょう。  

`tutorial_4` は、空のバケツと水入り瓶で水入りバケツを作成するレシピを示しています。  
指定することが出来るアイテムについて、理解できましたか?  
それではレベル2へステップアップしてみましょう!  


# レベル2
レベル1では、定型レシピと不定形レシピの「基本のき」を学びました。  
このレベル2では、レシピを構成する要素がもっと複雑になった際に有用となる書き方を学びます。  

```yaml
name: tutorial_5
tag: amorphous
result: stone
coordinate: [c]
override:
  - cobblestone -> c
```  
なんだか新しいセクションがありますね。  
それと、 `coordinate` の中身が短くなっていますね。  
よく見ると、新しいセクションの中身と `coordinate` の中身はリンクしているような......?

これは、 `override` セクションといい、素材名を任意の文字列に紐づけて `coordinate` セクションに使用できるようにするものです。  
`素材名 -> 任意の文字列`
という形で書きます。  
 `->` とそれぞれの文字の間には半角の空白が必要であることに注意してください。  

これより後のレベルでは、この `override` セクションを多用していきます。  
使い方を忘れないようにしましょう!  
(`override` セクションでは `null` や `water_bottle` も任意の文字列に置き換えることが出来ます。)

# レベル3
レベル3では、知っておくとレシピの幅が広がる書き方について学んでいきます。  

```yaml
name: tutorial_6
tag: amorphous
result: cobblestone
coordinate: [w,l]
override:
  - water_bucket -> w
  - lava_bucket -> l

returns:
  - water_bucket,bucket,1
  - lava_bucket,bucket,1
```  
またもや見慣れないセクションがくっついていますね。  

このセクションは、指定したアイテムをクラフトで使用した際に、返却するアイテムと数量を指定することが出来るセクションです。  
上記のレシピでは、1つの水バケツと1つの溶岩バケツから丸石を作成するレシピです。  
それに加えて、水バケツ、溶岩バケツそれぞれにつき1つのバケツを返却します。  

# レベル4
レベル4では、素材に要求する条件をこれまでのものよりも複雑にするためには必須の、 `Matter` ファイルの作成方法について学びます。  

```yaml
name: matter_1
amount: 1
mass: false
candidate: [stone,cobblestone]
```  
何やらこれまでの説明では出てこなかった新しいセクションが2つもありますね。  
しかしどちらも詳細解説の方に載っているので、ここでの説明は省略します。  

上記の `Matter` ファイルがどのような素材を指し示すのか考えてみましょう。  

分かりましたか?  
上記の `Matter` ファイルは `matter_1` という名前を持ち、要求個数が1、mass は偽、アイテムは石か丸石のどちらかを要求している素材を示しています。  

この4つのセクションを持つ素材ファイルは最もシンプルな形と言えます。  
これくらいならまだ大丈夫ですよね?  
まだまだ行きますよ～

```yaml
name: matter_2
amount: 1
mass: false
candidate: ["R|(STONE|COBBLESTONE)"]
```  
`candidate` の中身が見慣れない文字列になっていますね。  
これは正規表現で `candidate` の中身を指定しているためです。  
上記の例では `candidate` に石と丸石を設定しています。  

**正規表現で `candidate` を書くときは、大文字でしかマッチしないことに気を付けてください!**  
`R|(stone|cobblestone)` と書いてもマッチしません。

ちょっと疲れてきましたか? 休憩しつつでも大丈夫ですよ。時間をかけてマスターしましょう!  
<br>
<br>
次に学んでいくのは、エンチャントを要求する場合の書き方です。  

```yaml
name: matter_3
amount: 1
mass: false
candidate: [diamond_pickaxe]
enchantment:
  - mendind,1,onlyenchant
  - dig_speed,1,strict
```  
さて、しっかりと新しいセクションが増えているのが分かりますね。  
リスト形式でエンチャント名とレベル、そして要求厳格性を書いています。  
上記の素材ファイルでは、「修繕(レベル不問)と効率強化(レベル1)のエンチャントが付与されたダイヤモンドつるはし」を指定しています。  

エンチャント名の部分に存在しないエンチャントを書いてしまった場合はエラーを吐くので十分注意してください。  
(設定ファイル読み込み時のチェックでコンソールにアラートを発するので、そこでも確認することが出来ます。)  

ここまで学んできた人には分かり切ったことと思いますが、エンチャント名を日本語表記にして設定ファイルに書き込んでも、それは反映されません。  
<br><br>
次に学ぶのは、ポーションの効果を要求に追加する場合の書き方です。  
もちろんですが、 `candidate` に `potion`, `splash_potion`, `lingering_potion` のどれかが設定されていない場合は効力を発揮しません。  
さて、どのような書き方をするのか学んでいきましょう。  

```yaml
name: matter_4
amount: 1
mass: false
candidate: [potion]
potion:
  - jump,200,5,onlyEffect
  - luck,200,5,strict
```  
エンチャントセクションの書き方とあまり変わりませんね。  
`potion` セクションは `効果名`, `効果時間`, `効果レベル`, `要求厳格性` の4つのパラメータを必要とします。  
よって、この素材ファイルは「跳躍力増強の効果のみ、幸運レベル4効果時間10秒のポーション」を指しています。  
`onlyEffect` の時は `jump,0,0,onlyEffect` のように、関係の無いパラメータの数値を適当に設定しても大丈夫です。  
ですが、 `jump,,,onlyEffect` のように何も書かないとエラーが出る可能性があるので注意してください。  

また、ポーションの効果時間はゲーム内ティックで表されるので、秒数に20をかけた値になります。  
30秒であれば 600 ticks, 30.5 秒であれば 610 ticks と表されます。

これにてレベル4、素材ファイルの書き方は終わりです!  

# レベル5
レベル5は成果物を修飾する `Result` ファイルについて学んでいきます。  

```yaml
name: result_1
amount: 3
nameOrRegex: stone
matchPoint: -1
```  
上記の形式が最も簡単な構成の `Result` ファイルです。  
`nameOrRegex` セクションは大文字小文字が混在していて少し書きづらいかもしれませんが、ご容赦願います。  

このファイルは「石を3つ」指定しています。  

`Result` ファイルの `amount` セクションは `Matter` ファイルのものとは異なり、柔軟に設定することが出来ます。  
恐らく `Integer.MAX_VALUE` まで設定することはできますが、そこまでの数を指定するとサーバーが落ちる危険性が非常に高くなるので、本番環境ではやらないようにしましょう☆  

さて、次はこの `Result` ファイルにおけるオプション設定の書き方について学んでいきましょう。  

```yaml
name: result_2
amount: 3
nameOrRegex: "^([a-zA-Z_]{1,20})_CONCRETE_POWDER@{R}_CONCRETE$"
matchPoint: 1
```  
上記の例は詳細解説のファイルでも紹介しました。  
成果物の材料指定に、素材のIDを利用している例です。  
`BLUE_CONCRETE_POWDER` が素材に含まれていれば `BLUE` を抜き出して `{R}` と置き換え、 `BLUE_CONCRETE` を成果物とします。  

次はアイテムの装飾に関わるセクションについて学びます。  

```yaml
name: result_3
amount: 3
nameOrRegex: potion
matchPoint: -1
metadata: 
  - potiondata,speed,200,10
  - potioncolor,0,0,255
  - displayname,§bHigh Speed Potion
  - lore,More Faster!
```  
何やらたくさんの設定項目がありますね。  
そんなに怖がらなくても、彼らはきっとあなたの味方になってくれるはずですよ。  

上記の `Result` ファイルにより作成される成果物は、「移動速度上昇レベル10を10秒間付与する緑色で、表示名は水色で High Speed Potion、説明文には More Faster! と書かれているポーション」を示しています。  
同じパラメータを2回以上呼び出し、異なる値を設定した場合は、それらの中で1番最後に記述されているパラメータの値が設定されます。  

例
```yaml
name: result_4
amount: 3
nameOrRegex: potion
metadata:
  - potiondata,speed,200,10
  - potioncolor,0,0,255
  - potioncolor,255,0,0
```  
上記の例では、「移動速度上昇レベル10が10秒間付与される赤いポーション」を示しています。  
これは `potioncolor,0,0,255` が `potioncolor,255,0,0` 上書きされたことによるものです。  

ですが、 `potiondata` と `lore` だけはそれらデータの上書きをせずに、追加されていきます。  
(`potiondata` で同じ効果を指定した場合は、効果時間と効果レベルが上書きされます)  

例  
```yaml
name: result_5
amount: 3
nameOrRegex: potion
metadata:
  - potiondata,speed,200,10
  - potiondata,luck,200,10
  - potiondata,slow,200,10
```  
上記の例では、 `potiondata` を3回記述していますが、上書きされることはなく3つの効果を持った成果物を示しています。  

`lore` も上記のように書いた場合でも上書きはされません。しかし、書きすぎると通常の大きさの UI では見切れてしまう場合があるので注意しましょう。  

これにてレベル5を終了とします。お疲れさまでした!  

# レベル6
レベル6は、今までのレベルでやってきたことを全て合わせた実践編です。  
これが分かればあなたもカスタムクラフターを自由自在に使いこなせる!  

このレベルは、実践形式で進行します。ぜひ、自分の力のみで解いてみてくださいね!  
<br><br><br>
## 課題1
`Recipe` ファイル1つの中に  
- ソウルサンド
- ウィザースケルトンスカル  

をウィザー召喚時と同じ配置にした際に、ウィザーのスポーンエッグを成果物として返すレシピを作成してください。  
レシピ名は `practice_1` としてください。  

<details><summary>回答例</summary>

```yaml
name: practice_1
tag: normal
result: wither_spawn_egg
override:
  - soul_sand -> s
  - wither_skeleton_skull -> w
  - null -> n
coordinate:
  - w,w,w
  - s,s,s
  - n,s,n
```

</details>

<details><summary>課題1の解説</summary>

`tag`  
-> 課題文にて「ウィザー召喚時と同じ配置にした際に」とアイテムの配置を指定されているので、 `tag: normal` が正解となります。  

`result`  
-> 課題文にて「ウィザーのスポーンエッグを成果物として返す」とあるので `result: wither_spawn_egg` が正解となります。  

`coordinate`  
-> 課題文にて「ウィザー召喚時と同じ配置にした際に」と指定されているので  
```yaml
coodinate:
  - w,w,w
  - s,s,s
  - n,s,n
```  
が正解となります。  

</details>  
<br><br><br>

## 課題2
金ブロックでアーマースタンドを囲んだアイテム配置をした際に、  
「消滅の呪いが付与され、`Death Totem` という表示名が付けられた不死のトーテム」  
を作成する `Result` ファイルと `Recipe` ファイルを作成してください。  
`Result` ファイルは `practice_2_result`, `Recipe` ファイルは `practice_2_recipe` という名前にしてください。  
また、成果物は材料1揃いにつき1個としてください。  

<details><summary>回答例</summary>

`Result` ファイル  
```yaml
name: practice_2_result
amount: 1
matchPoint: -1
nameOrRegex: totem_of_undying
metadata:
  - displayname,Death Totem
  - enchantment,vanishing_curse,1
```

`Recipe` ファイル
```yaml
name: practice_2_recipe
tag: normal
result: practice_2_result
override:
  - gold_block -> g
  - armor_stand -> a
coordinate: 
  - g,g,g
  - g,a,g
  - g,g,g
```
</details>

<details><summary>解説</summary>

### Result
`nameOrRegex`  
-> 課題文にて「不死のトーテム」と指定されているので `totem_of_undying` が正解です。  

`metadata`  
-> 課題文にて「消滅の呪いが付与され」、「`Death Totem` という表示名が付けられた」と指定されているので  
```yaml
metadata:
  - enchantment,vanishing_curse,1
  - displayname,Death Totem
```
が正解です。  

### Recipe
`tag`  
-> 配置が指定されているので `normal` が正解です。  

`result`  
-> 今回は `Result` ファイルに記載した成果物を指定するので、対象の `Result` ファイルの `name` セクションに書かれている文字列 `practice_2_result` が正解です。  

`coordinate`  
-> 「金ブロックでアーマースタンドを囲んだアイテム配置をした際に」と課題文で指定されているので  
```yaml
coordinate:
  - g,g,g
  - g,a,g
  - g,g,g
```  
が正解です。

</details>

### 課題3
`Matter`, `Result`, `Recipe` ファイルを用いて、各色の色付きコンクリートと水バケツを材料にし、素材にしたコンクリートパウダーの色のコンクリートを成果物とするレシピを作成してください。  
また、クラフト時に配置された水バケツの分だけ空のバケツを返却するようにしてください。  

なお、成果物の作成個数は色付きコンクリートパウダーの個数に合わせるものとし、レシピ名は `practice_3_matter`, `practice_3_result`, `practice_3_recipe` としてください。  

<details><summary>回答例</summary>

### Matter
```yaml
name: practice_3_matter
mass: false
amount: 1
candidate: ["R|([\w_]+)_CONCRETE_POWDER"]
```

### Result
```yaml
name: practice_3_result
amount: 1
matchPoint: 1
nameOrRegex: "([\w_]+)_CONCRETE_POWDER@{R}_CONCRETE"
```

### Recipe
```yaml
name: practice_3_recipe
tag: amorphous
result: practive_3_result
coordinate: [m-water_bucket,practice_3_matter]
returns:
  - water_bucket,bucket,1
```

</details>

<details><summary>解説</summary>

### Matter
`candidate`  
-> 色付きのコンクリートパウダーを `candidate` に設定するための正規表現の例として回答例にあるような正規表現を用いると大量に登録する手間を省くことが出来ます。  
また、`candidate` にアイテムを登録するために正規表現を用いる場合は `R|` を用いる必要があります。  

例  
```yaml
candidate: ["R|([\w_]+)_CONCRETE_POWDER"]
```

### Result
`matchPoint`  
-> 今回は正規表現を用いているので -1 以外に設定しています。  

`nameOrRegex`  
-> 色付きのコンクリートパウダーにマッチさせるための正規表現と、置換する文字列を `@` で区切り、置換する対象の部分を `{R}` にします。  
上記の例のような  
```yaml
nameOrRegex: "([\w_]+)_CONCRETE_POWDER@{R}_CONCRETE"
```  
が正解になります。  

### Recipe
`tag`  
-> 課題文で配置は指定されていないので `tag: amorphous` が正解です。  

`coordinate`  
-> `m-water_bucket` : `m-アイテムID` で `mass: true` のアイテムとすることが出来ます。  
(今回はファイル数の指定はしていないので、水バケツの `matter` ファイルを作成する手もあります)  

-> `practice_3_matter` : `Matter` ファイルの `name` セクションの文字列を書きます。  

`returns`  
-> 水バケツに対して、同じ数の空バケツを返すためには  
```yaml
returns:
  - water_bucket,bucket,1
```  
が正解です。  

</details>

<br><br><br>

# おわり
お疲れさまでした!