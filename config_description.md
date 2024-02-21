# config.yml
## baseBlock
このセクションには、カスタムクラフターを開くために必要な基礎ブロックの情報を保持するファイルが存在するディレクトリのパスを書いてください。  

例) `baseBlock: /plugins/Custom_Crafter/baseBlock`  


## results
このセクションには、成果物の設定ファイルを配置したディレクトリのパスを書いてください。 
このセクションは必ずリスト形式で書いてください。  
対象のディレクトリが1つしか存在しない場合でもリスト形式で書いてください。  
 

例) `results: [plugins/Custom_Crafter/results1,plugins/Custom_Crafter/results2]`  
例) 
```yaml
results:
  - plugins/Custom_Crafter/results1
  - plugins/Custom_Crafter/results2
```  


## matters
このセクションには素材の設定ファイルを配置したディレクトリのパスを書いてください。  
このセクションは必ずリスト形式で書いてください。  
対象のディレクトリが1つしか存在しない場合でもリスト形式で書いてください。  

例) `matters: [plugins/Custom_Crafter/matters1,plugins/Custom_Crafter/matters2]`  
例)
```yaml
matters:
  - plugins/Custom_Crafter/matters1
  - plugins/Custom_Crafter/matters2
```  


## recipes
このセクションにはレシピの設定ファイルを配置したディレクトリのパスを書いてください。  
このセクションは必ずディレクトリ形式で書いてください。  
対象のディレクトリが1つしか存在しない場合でもリスト形式で書いてください。  

例) `recipes: [plugins/Custom_Crafter/recipes1,plugins/Custom_Crafter/recipes2]]`  
例)
```yaml
recipes:
  - plugins/Custom_Crafter/recipes1
  - plugins/Custom_Crafter/recipes2
```

# BaseBlock
基礎ブロックの設定ファイルには1種類のブロックのアイテムIDのみを記述する必要があります。  
アイテムIDについては[Material (Spigot API JavaDoc)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html) をご覧ください。  
セクション名は `material` です。  

例) 
```yaml
material: gold_block
```

# Matters
素材の設定ファイルには、必ず記載しなくてはならない項目が4つとオプション設定が2つ存在します。  
## 必須項目
### name
素材の名前を `name` セクションに書いてください。  
この名前は、バニラアイテムのID と重複しないように設定してください。  

例) **NG** : `name: stone` -> 石のアイテムID と重複するので使用しないでください。  
例) **OK** : `name: hard_stone`  

また、独自の名前同士で重複することのないように設定してください。  
もしも一つの名前を複数の素材で使用してしまった場合は、同名の素材のうち、最後に読み込まれた設定ファイルの内容が適用されます。ご注意ください。  

__素材の名前に `null` を使用しないでください。`null` は予約語です。__

### amount
素材の要求量を `amount` セクションに書いてください。  
1以上の整数を指定してください。(なお、1よりも大きい値を指定した場合、エラーなどが発生する場合があります)  

例) **NG** : `amount: -1` -> 1以上の値を設定してください。  
例) **OK** : `amount: 1`

### mass
mass の設定を `mass` セクションに書いてください。  
設定可能な値は `true` もしくは `false` の2つのみです。  

例) **NG** : `mass: no` -> true もしくは false のどちらかのみを記載してください。  
例) **OK** : `mass: true`  

mass を true に設定すると、クラフト時にそのアイテムの個数を参照しなくなります。  

例) mass = true のアイテム1つとmass = false のアイテム10個でクラフトを行う場合、mass = false のアイテムの個数のみを参照し、10個の成果物を作成することが出来ます。  

なお、mass = true に設定されたアイテムはクラフト時に個数を参照されなくなりますが、配置されていない(0個)場合は、成果物の作成に失敗します。

### candidate
candidate の設定を `candidate` セクションに書いてください。  
このセクションは必ずリスト形式で書いてください。  
設定するIDが1つしかない場合でもリスト形式で書いてください。  

> candidate は、素材となるアイテムに幅を持たせるための設定項目です。  
> この項目に複数のアイテムIDを指定することで、複数のアイテムを1つの素材として扱うことが出来ます。  
> これにより、レシピ数の増加を抑えることが出来ます。  

例) **NG** : `candidate: stone` -> 設定するアイテムIDが1つしかない場合でもリスト形式で書いてください。  
  

例) **OK** : `candidate: [stone]`  
例) **OK** : `candidate: [stone,cobblestone]`

また、このセクションでは正規表現を用いて複数のアイテムIDを指定することもできます。  
正規表現を使用する場合は `R|` を正規表現の先頭に加えて記載してください。  

例) `candidate: ["R|([a-zA-Z_]{1,20})_CONCRETE_POWDER"]`  
上記の正規表現は、各色のコンクリートパウダーを指定しています。  

---
## オプション設定
以下の設定はオプション設定となります。  
記載する際にも条件がありますのでご注意ください。  
### enchant
素材に要求するエンチャントを設定することが出来ます。  
このセクションは必ずリスト形式で書いてください。  
設定が1つしかない場合でもリスト形式で書いてください。  

要求するエンチャント名、要求するレベル、要求する厳格性　の順番で設定します。  
#### エンチャント名
エンチャント名は各エンチャントに割り振られている内部 ID を利用してください。
#### 要求するレベル
1以上の整数で入力してください。  
#### 要求する厳格性
厳格性は以下の3つの値のみを設定することが出来ます。  
- `NotStrict` : 素材に対してこのエンチャントを要求しません。
- `OnlyEnchant` : 素材にこのエンチャントが付与されていることを要求します。レベルは一致していなくても大丈夫です。  
- `Strict` : 素材にこのエンチャントが付与されていることを要求し、レベルが一致していることも要求します。  

例) 
```yaml
enchant:
- mending,1,strict  
```  
上記の例では、素材に「修繕エンチャントのレベル1が付与されている」ことを要求しています。

`NotStrict`, `OnlyEnchant` を設定する場合はレベルを適当な値に設定してください。

---

### potion
素材に要求するポーション効果を設定することが出来ます。  

> このセクションは candidate に `potion`, `splash_potion`, `lingering_potion` を設定しているとき以外は書かないでください。


要求するポーション効果、要求する効果時間、要求するレベル、要求する厳格性　の順に記載してください。

#### ポーション効果
[Spigot JavaDoc (PotionEffectType)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/potion/PotionEffectType.html) を参考にしてください。

#### 要求する効果時間
秒数ではなく tick で指定してください。 (1 sec = 20 ticks)

#### 要求するレベル
1以上の整数を指定してください。  

#### 要求する厳格性
厳格性は下記の4つの値からのみ設定することが出来ます。  
- `Not_Strict` : 素材に対してこのポーション効果を要求しません。
- `Only_Duration` : 素材に対して、指定したポーション効果と効果時間の一致を要求します。
- `Only_Amplifier` : 素材に対して、指定したポーション効果とレベルの一致を要求します。
- `Strict` : 素材に対して、指定したポーション効果、レベル、効果時間の一致を要求します。
  

例)
```yaml
potion:
  - speed,200,2,strict
```
上記の例は移動速度上昇の効果時間10秒、レベル2を素材に要求しています。


### bottleTypeMatch
このセクションは `potion` セクションを書く際と同じ条件を要求します。  
このセクションは `true`, `false` のみを設定可能です。  
   
`true` に設定すると素材に対して瓶の形の一致を要求します。
`false` に設定すると素材に対して瓶の形の一致を要求しません。  
  
<br>

> 瓶の形の一致 : 普通のポーションであれば普通のポーションを、スプラッシュポーションであればスプラッシュポーションを、残留ポーションであれば残留ポーションというようなセットに対してのみ一致したとみなされること。  
  
例) `bottleTypeMatch: true`

---

### Container
下記の正規表現に従ってください。
```
- predicate: ((allow|deny)_(value|tag)|string_match)
  formula: (.+)
```

predicate には
- allow_tag : formula に列挙したタグが**含まれている**場合にのみアイテムのクラフトを続行する
- deny_tag : formula に列挙したタグが**含まれていない**場合にのみアイテムのクラフトを続行する
- allow_value : formula に記載した式が**成立する**場合にのみアイテムのクラフトを続行する
- deny_value : formula に記載した式が**成立しない**場合にのみアイテムのクラフトを続行する
- string_match : formula に式と正規表現を記載し、式の内容が正規表現にマッチした場合にのみアイテムのクラフトを続行する

のいずれかを選んで記載してください。  
formula では `%` で囲んだ文字列を変数名として扱い、素材となるアイテムのコンテナに対象の変数が存在する場合にのみ式に代入されます。  
対象の変数が存在しない場合には、`None` という文字列が代入されます。
また、計算が必要な箇所では `{}` を用いることによってそれらで囲まれた部分を式とした際の計算結果を代入します。  
計算に使用することが出来る演算子は `+`, `-`, `*`, `/`, `%`(剰余), `^`(べき乗) の 6 つです。  
剰余の演算子を用いる場合には、変数として扱われることのないように `\`(バックスラッシュ) を直前においてください。   
計算結果の比較に使用することが出来る演算子は `<`, `>`, `<=`, `>=`, `==`, `!=`, `&&`, `||`, `!` の 9 つです。  
また、これらの計算では `()` を用いて計算順序の制限や評価の重ね合わせを行うことが出来ます。   

(条件の例)
```yaml
# test_1.string = Hello, world
# test_2.long = 100
# test_3.long = 25
container:
- predicate: allow_tag
  formula: test_1.string

- predicate: allow_tag
  formula: test_2.*

- predicate: allow_value
  formula: {%test_2.long%  * %test_3.long% = 2500}

- predicate: string_match
  formula: 2:(?i)hello, world,%test_1.string%

- predicate: allow_value
  formula: {(%test_2.long% == 100) && (%test_3.long% == 25)}
```

- (allow|deny)_tag の場合、`*` を使用することによって対象とする変数名の型を無視して変数名のみで指定することが出来ます。
- (allow|deny)_value の場合は、formula の内容を真偽値で表現することが出来るようにしてください。
- string_match の場合、`(\\d+):(.+)` で示されるルールに従う必要があります。
  - 最初の `(\\d+)` というパターンは、正規表現と代入された文字列の区切りをであるカンマのインデックスを示します。上記の例であれば、2 つめのカンマが文字列と正規表現の区切りであることを示しています。
  - コロンを挟んで右側の `(.+)` は正規表現と代入された文字列を記載する場であることを示します。
  - 指定されたインデックスのコロンの左側を正規表現、右側を実際の文字列として扱います。
- 型 `long` を用いる場合においても内部での演算では `double` が用いられるため、商や積が `.0` のような形で表されることがあります。(long にキャストすることが出来る機能を作成中です) 

--


# Result
成果物の設定ファイルには必ず設定しなくてはいけない項目4つと、オプション設定2つがあります。  

## 必須設定
### name
成果物の名前は `name` セクションに記載してください。  
レシピを作成する際に使用します。  
バニラアイテムのIDと異なる必要があり、加えて、他の成果物と重複することがあってはいけません。  
もしも一つの名前を複数の素材で使用してしまった場合は、同名の成果物のうち、最後に読み込まれた設定ファイルの内容が適用されます。ご注意ください。  

例) `name: genetically_modified_crop`

### amount
成果物の個数は `amount` セクションに記載してください。  
1以上の整数を記載してください。  

例) `amount: 3`

### matchPoint
後述の成果物のアイテムIDを指定する際に正規表現を使う場合のみ適切に設定してください。  
成果物のアイテムIDの指定に正規表現を使用しない場合は、このセクションの値を `-1` に設定しておいてください。

例) `matchPoint: -1`

### nameOrRegex
成果物のアイテムIDは `nameOrRegex` セクションに記載してください。  
このセクションでは
- アイテムID を直接指定する
- 正規表現を用いて素材のアイテムIDから文字列を切り出して新たなアイテムID を指定する
- パススルーモードを指定する

のいずれかの方法で成果物のアイテムを指定することが出来ます。  
正規表現を使用しない場合は、[Spigot JavaDoc (Material)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html) を参考にアイテムIDを記載してください。  

例) `nameOrRegex: stone`  

---

正規表現を使用し、素材のアイテムIDから成果物のアイテムIDを作成し適用する場合は  
`素材のアイテムIDに使用する正規表現@成果物のアイテムIDを作成する文字列`  
の形を取ってください。  

素材のアイテムIDから切り出した文字列を、成果物のアイテムIDを作成する文字列に適用するためには、置き換える箇所に `{R}` を挿入してください。  

例) `nameOrRegex: "([a-zA-Z_]{1,20})_CONCRETE_POWDER@{R}_CONCRETE"`  
上記の例が設定されているレシピにおいて、素材に色付きコンクリートパウダーが設定されており、 `matchPoint` に `1` が設定されている場合(例として、白色のコンクリートパウダーが設定されている場合)、成果物のアイテムIDは  
`WHITE_CONCRETE_POWDER` より切り出された `WHITE` を `{R}` の箇所に挿入するため `WHITE_CONCRETE` となります。  

マッチ箇所は1か所のみ設けることが出来ます。複数のマッチ箇所を設けることはできません。  

---

パススルーモードを設定するには、  
`pass -> パススルーしたいアイテムID`  
のフォーマットで記載をしてください。  
パススルーモードを使用する場合は Recipe ファイルとの連携が不可欠になるため、Result ファイルの使いまわしは控えることを推奨します。  
また、パススルーしたいアイテム ID の部分に Matter の名前を指定することはできません。  
必ずマインクラフト内に存在するアイテムのアイテム ID に設定してください。

**パススルーするアイテムは、1つしか要求されない素材にしてください。**  
(複数個要求されるアイテムに指定するとエラーになります)

パススルーモードではパススルーするアイテムに様々な装飾を施すことが出来ます。  
それらの装飾が必要ではなく、単にレシピ中で使用したアイテムを返却したい場合は Recipe ファイルの `returns` セクションを利用することを推奨します。  
(アイテムに装飾を施さない場合、Recipe ファイルと連携するためにファイル構成がより複雑になり、レシピ管理の負担が増す恐れがあります。)

例)
```yaml
# リザルトファイル
name: pass_through_test_1
amount: 1
matchPoint: -1
nameOrRegex: pass -> diamond_helmet

# レシピファイル
name: recipe
tag: normal
result: pass_through_test_1
override:
  - null -> n
  - diamond_helmet -> h
  - diamond -> d
coordinate: 
  - d,d
  - n,h

```  

これらをリザルトファイルとレシピファイルに分割し、適用すると配置したダイヤモンドの頭防具に `耐久5` のエンチャントと `An enchant is added!` という説明文が追加されて返却されます。  


## オプション設定
### 成果物に付与するエンチャント
成果物に付与するエンチャントは `enchant` セクションに記載してください。  
`enchant` セクションは、 `エンチャント名,エンチャントレベル` の順で記載してください。
また、このセクションは付与したいエンチャントが1つであってもリスト形式で書いてください。  

例) 
```yaml
enchant:
  - mending,1
```  
上記の例では、修繕のレベル1エンチャントを成果物に付与しています。  


---

# Recipe
Recipe は4つの必須事項と2つのオプション設定を持ちます。  
## 必須事項
### レシピの名前
レシピの名前は `name` セクションに記載します。  
レシピの名前は、コマンドでレシピ情報を表示する際に使用します。  

例)
`name: custom_item`

### Tag
`Tag` セクションにはレシピが定型か不定形かを記載してください。  
`normal` と `amorphous` のどちらかのみを指定することが出来ます。  

例) `tag: normal` -> 定型レシピであることを示す。  
例) `tag: amorphous` -> 不定形レシピであることを示す。

### result
`result` セクションには成果物の名前を指定します。  

例) `result: custom_item_result`  

成果物の名前以外にも、バニラのアイテムIDをそのまま指定することでも成果物として扱うことが出来ます。  
なお、バニラのアイテムIDを指定した場合、成果物は「個数1、メタデータ無し」となります。  

例) `result: diamond`

### coordinate
`coordinate` セクションにはアイテムの配置を指定します。  
定型レシピの場合と、不定形レシピの場合で書き方が大きく異なるので注意してください。  
配置するアイテムの指定には、 Matter で指定した名前の他にアイテムIDでも指定することが出来ます。  
また、アイテムを指定しない場所には `null` と書いてください。  
なお、アイテムIDで指定する場合には
- `アイテムID` : `mass=false` のアイテムを配置
- `m-アイテムID` : `mass=true` のアイテムを配置  

の2種類の書き方があります。  
アイテムIDでアイテムを指定した場合、個数は自動的に1個に設定されます。  

また、後述のオプション設定に書かれている `override` セクションで設定を行うことで、アイテムの情報を紐づけられた短い文字列を指定することもできます。


#### 定型レシピの場合の書き方
定型レシピの場合、 coordinate は 1辺が1アイテムから6アイテムの正方形になるように記載してください。  

例)
```yaml
coordinate:
  - stone,stone
  - stone,stone
```  

上記の例は、 `mass=false` で要求個数1個の石を 2\*2 の正方形に並べるレシピを示します。  

#### 不定形レシピの書き方
不定形レシピの場合、 coordinate は1行のリストで記載してください。  

例)
```yaml
coordinate: [stone,stone,stone,stone]
```  
上記の例は、 `mass=false` で要求個数1個の石をクラフティングスロット内に4つ配置するレシピを示します。  

## オプション設定
### override
`coordinate` に記載するアイテム名を Matter の設定ファイルに指定した名前や、アイテムIDなどを短い文字列に変更することが出来ます。  
なお、変更した文字列を `coordinate` で使うことが出来るのは `override` を書いたファイル内のみであることに気を付けてください。  
`変更前のアイテム名 -> 変更後の文字列` のかたちで記載してください。  
また、文字列の変更が1つだけの場合もリスト形式で記載してください。  

例)
```yaml
override:
  - "light_gray_concrete_powder -> l"
  - "m-stone -> m"
  - "null -> n"
```  
上記の `override` は薄い灰色のコンクリートパウダーを `coordinate` 内で `l` として扱えるように、`mass=true` な石を `m` として扱えるようにしています。  

### returns
`returns` セクションは、アイテムを作成した際に返却するアイテムを設定することが出来ます。  
返却するアイテムが1つしかない場合でもリスト形式で記載してください。  
`素材のアイテムID,返却するアイテムのアイテムID,素材1個につき返却するアイテムの個数` の順番で指定します。  

例)
```yaml
returns:
  - "water_bucket,bucket,1"
```  
上記の例は、素材に含まれる水バケツ1つにつきバケツ1つを返却します。  

---

### container
このセクションでは成果物に対して様々なデータを設定することが出来ます。  

以下の正規表現に従ってください。  
```yaml
-
  predicate: (none|string|value)
  formula: (.+) # predicate: none の場合は不要
  type: # 記載可能な値については後述
  value: (.+)
```

- predicate: 
  - none: `type`, `value` に記載された操作を毎回実行します。
  - string: `Matter` の `StringMatch` と同じように判定を行います。
  - value: `Matter` の `allow_value` と同じように判定を行います。

- formula: 
  - `predicate: none` を設定している場合にはこのセクションを書く必要はありません。
  - その他の場合においては `predicate` に設定した値に沿った内容を記載してください。

- type:  
  - lore: 成果物の説明欄へ `value` に記載した内容を追加します。
  - enchant: 成果物へエンチャントを追加します。
  - potion_color_rgb: ポーションの色を RGB で指定します。
  - potion_color_name: ポーションの色を色の名前で指定します。
  - potion_effect: ポーションの種類、レベル、効果時間を設定します。
  - texture_id: 指定した番号を持つリソースパックのテクスチャを成果物に適用します。
  - tool_durability_percentage: 成果物の残り耐久値をパーセンテージで指定します。
  - tool_durability_real: 成果物の残り耐久値を実際の値で指定します。
  - item_name: 成果物の名前を設定します。
  - attribute_modifier: 属性を設定します。
  - attribute_modifier_equipment: 特定部位に装備した際の属性を設定します。
  - item_flag: アイテムフラグを設定します。
  - unbreakable: 無限の耐久を設定します。
  - leather_armor_color: 皮防具の色を設定します。
  - book: 本の中身、著者、世代、タイトルの設定をします。
  - enchant_modify: 成果物のエンチャント内容を変更します。
  - lore_modify: 成果物の説明文の内容を変更します。
  - attribute_modifier_modify: 成果物の属性の内容を変更します。
  - result_sync: 設定に使用するデータの更新を行います。(**成果物に変化なし**)
  - container: 成果物のコンテナの設定を行います。
  - head: 成果物がプレイヤーヘッドである場合にのみ、その内容についての設定を行います。
  - material: 成果物のアイテム種別を設定します。
  - run_command_as_player: アイテムを作成しているプレイヤーとして実行するコマンドの設定をします。(**成果物に変化なし**)
  - run_command_as_console: サーバーコンソールとして実行するコマンドの設定をします。(**成果物に変化なし**)
  - amount: 成果物の個数を変更する

- value: 
  - `type` に指定した値によって従うべき正規表現パターンが異なります。
    - enchant: `enchant=([a-zA-Z]+),level=([0-9]+)`
      - enchant: エンチャント名 (ゲーム内 ID)
      - level: エンチャントレベル( 1 ~ 255)
    - potion_color_rgb: `red=([0-9]+),green=([0-9]+),blue=([0-9]+)`
    - potion_color_name: `(.+)`
    - texture_id: `([-0-9]+)`
    - tool_durability_percentag: `([0-9]*)\.?([0-9]+)`
    - tool_durability_real: `([0-9]+)`
    - item_name: `(.*)`
    - attribute_modifier: `attribute=([a-zA-Z_]+),op=(?i)(add_number|add_scalar|multiply_scalar_1),value=(-?[0-9]*\.?[0-9]+)`
      - attribute: 属性の種別
      - op: 操作の種類
      - value: 値
    - attribute_modifier_equipment: `attribute=([a-zA-Z_]+),op=(?i)(add_number|add_scalar|multiply_scalar_1),value=(-?[0-9]*\.?[0-9]+),slot=([a-zA-Z_]+)`
      - slot: アイテムを装備した時に効果を発揮するスロット
    - item_flag: `flag=([a-zA-Z_]+),action=(?i)(clear|remove|add)`
      - flag: アイテムフラグの種類
      - action: 操作種別
    - unbreakable: `(true|false)`
    - potion_effect: `effect=([a-zA-Z_]+),level=([0-9]+),duration=([0-9]+)`
      - effect: ポーションの種類 ([このページ](https://jd.papermc.io/folia/1.20/org/bukkit/potion/PotionEffectType.html)をご覧ください)
      - level: ポーションのレベル
      - duration: ポーションの持続時間 (秒数 × 20)
    - leather_armor_color (RGB 指定の場合): `r=([0-9]+),g=([0-9]+),b=([0-9]+)`
      - 特殊値 `$CURRENT_(RED|GREEN|BLUE|RGB)$` を使用することが出来る。(それぞれ現在設定されている色の値)
      - r: RGB の red
      - g: RGB の green
      - b: RGB の blue
    - leather_armor_color (色名指定の場合): `([a-zA-Z_]+)`
    - leather_armor_color (ランダムの場合): `(?i)(random)`
    - book: `"type=(author|title|add_page|add_long|from_file|gen),element=(.+)`
      - type:
        - author: `element` の内容を著者に設定する
        - title: `element` の内容をタイトルを設定する
        - add_page: `element` の内容を新しいページとして追加する
        - add_long: `element` の内容を自動で複数ページに分割し追加する
        - from_file: `element` に指定されたパスに存在するファイルの中身を読みこみ、中身を自動で複数ページに分割し追加する (上限は 25600 文字, または 100 ページの小さい方)
        - gen: `element` から本の世代を設定する
    - head: `type=(name|url),value=(.+)`
      - type:
        - name: 実在するプレイヤーの名前から頭のスキンを決定する
        - url: [Minecraft-Heads.com](https://minecraft-heads.com/custom-heads) などに存在するスキンから決定する
      - value:
        - `type: name` のとき: プレイヤー名
        - `type: url` のとき: Base64 エンコードされた `{"textures":{"SKIN":{"url":"テクスチャの URL"}}}` の形式で記述された文字列 (Minecraft-Heads.com では for Developers セクションの Value に該当) 
    - enchant_modify: `type=(enchant|level),action=([a-zA-Z_]+)->([{}+\-*/\\^%$a-zA-Z0-9_]+)`
      - type: 
        - enchant: エンチャントの種類を変更する
        - level: エンチャントレベルを変更する
      - action: `変更前の値->変更後の値` の形式
      - `$CURRENT_LEVEL$` を用いて指定したエンチャントの現在のレベルを取得することが可能
    - lore_modify: `type=(clear|modify)(,value=type=(remove|insert),line=([0-9]+)(,value=(.+))*)?`
      - type:
        - clear: 説明文をすべて削除する (`type: clear` のとき、`value` は必要ない)
        - modify: `value` に従って説明文を編集する
      - value: `=type=(remove|insert),line=([0-9]+)(,value=(.+))*)?` に従う
        - remove: 指定した行を削除する
        - insert: 指定した行に `value` を挿入する
    - attribute_modifier_modify: `type=(clear|remove|modify)(,attribute=([a-zA-Z_]+)(,value=(.+))?)?`
      - type:
        - clear: 属性を**全て**削除
        - remove: `attribute` で指定した属性のみを削除
        - modify: `attribute` で指定した属性を `value` に従い編集
      - attribute: 属性
      - value:
        - `type=modify` の場合にのみ必要
        - 内容は `attribute_modifier` 及び `attribute_modifier_equipment` の正規表現に従う
    - result_sync: `.*`
    - container: `type=(add|remove|modify),target=([a-zA-Z0-9_.%$]*)(,value=(.+))?`
      - type:
        - add: 変数名を `target` から、値を `value` からそれぞれ取得し変数を作成して成果物のコンテナに加える
        - remove: `target` に指定された変数を成果物のコンテナから削除する
        - modify: 成果物のコンテナに既に値が存在している場合でも `add` の振る舞いをする
      - value: `$CURRENT_VALUES$` を用いて `target` から取得した変数名の現在の値を使用することが可能 (.anchor は常に空文字列を返す)
    - material: `[a-zA-Z_]+`
    - run_command_as_player: `.+`
    - run_command_as_console: `.+`
    - amount: `limit=(true|false),amount=([0-9]{1,4})`
      - limit: 成果物のマテリアルの１スロットにおける最大個数の制限を受け入れるか否か
      - amount: 成果物の個数
  - プレースホルダを用いて素材が保持していたデータを使用することが出来ます。
    - 上記の正規表現パターンはプレースホルダの値を代入した後の時点での文字列が従わなければならないパターンを示しています。
  - `$result.` を変数名の前に付けることで成果物がその時点で持つデータを使用することが出来ます。(前回 `result_sync` を行った時点のデータ)
  - `$PLAYER_([a-zA-Z_]+)$` のパターンに従う、プレイヤーに関する特殊な値を使用することが可能。
    - `$PLAYER_UUID$`: クラフトしているプレイヤーの UUID (String)
    - `$PLAYER_NAME$`: クラフトしているプレイヤーの名前 (String)
    - `$PLAYER_CURRENT_WORLD$`: クラフトしているプレイヤーが存在するワールドの名前 (String)
    - `$PLAYER_CURRENT_X$`: クラフトしているプレイヤーの X 座標 (double)
    - `$PLAYER_CURRENT_Y$`: クラフトしているプレイヤーの Y 座標 (double)
    - `$PLAYER_CURRENT_Z$`: クラフトしているプレイヤーの Z 座標 (double)
    - `$PLAYER_CURRENT_Xi$`: クラフトしているプレイヤーの X 座標を小数点以下切り捨てした値 (int)
    - `$PLAYER_CURRENT_Yi$`: クラフトしているプレイヤーの Y 座標を小数点以下切り捨てした値 (int)
    - `$PLAYER_CURRENT_Zi$`: クラフトしているプレイヤーの Z 座標を小数点以下切り捨てした値 (int)
    - `$PLAYER_CURRENT_PITCH$`: クラフトしているプレイヤーのピッチ (float)
    - `$PLAYER_CURRENT_YAW$`: クラフトしているプレイヤーのヨー (float)
    - `$PLAYER_IN_WATER$`: クラフトしているプレイヤーが水に入っているか (bool)
    - `$PLAYER_CURRENT_FOOD_LEVEL$`: クラフトしているプレイヤーの満腹度 (int)
    - `$PLAYER_PING$`: クラフトしているプレイヤーの PING 値 (int)
    - `$PLAYER_EXP$`: クラフトしているプレイヤーの経験値量 (float)
    - `$PLAYER_EXP_LEVEL$`: クラフトしているプレイヤーの経験値レベル (int)
    - `$PLAYER_DISPLAYED_NAME$`: クラフトしているプレイヤーのゲーム内での表示名 (String)
    - `$PLAYER_MAXIMUM_NO_DAMAGE_TIKCS$`: クラフトしているプレイヤーがダメージを受けずに経過したゲーム内チックの最大値 (int)
    - `$PLAYER_CLIENT_BRAND_NAME$`: プレイヤーが使用しているゲームクライアントの名前 (String)
    - `$PLAYER_IS_FLYING$`: プレイヤーが飛行しているか (bool)
    - `$PLAYER_CURRENT_GAME_MODE$`: クラフトしているプレイヤーのゲームモード (String)
    - `$PLAYER_IN_RAIN$`: クラフトしているプレイヤーが雨に打たれているか (bool)
    - `$PLAYER_IN_LAVA$`: クラフトしているプレイヤーがマグマの中にいるか (bool)
    - `$PLAYER_FACING$`: クラフトしているプレイヤーが向いている方角 (String)
    - `$PLAYER_CURRENT_HEALTH$`: クラフトしているプレイヤーの体力 (double)
    - `$RESULT_MATERIAL$`: 成果物のマテリアル名 (String)
    - `$RESULT_AMOUNT$`: 成果物の個数

  - その処理特有の特殊な値、数値型のデータであるときのみ使用することが出来る特殊な値を呼び出して使用することもできます。