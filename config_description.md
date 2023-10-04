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

例) `recipes: [plugins/Custom_Crafter/recipes1,plugins/Custom_Crafter/recipes2]`  
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
### 素材の名前
素材の名前を `name` セクションに書いてください。  
この名前は、バニラアイテムのID と重複しないように設定してください。  

例) **NG** : `name: stone` -> 石のアイテムID と重複するので使用しないでください。  
例) **OK** : `name: hard_stone`  

また、独自の名前同士で重複することのないように設定してください。  
もしも一つの名前を複数の素材で使用してしまった場合は、同名の素材のうち、最後に読み込まれた設定ファイルの内容が適用されます。ご注意ください。  

__素材の名前に `null` を使用しないでください。`null` は予約語です。__

### 素材の要求量
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
エンチャント名は[Spigot JavaDoc (Enchantment)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/enchantments/Enchantment.html)を参考に記載してください。  
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
metadata:
  - pass_through,mode=pass/type=enchant/action=add/value=enchant=durability,level=5
  - pass_through,mode=pass/type=lore/action=add/value=An enchant is added!

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
エンチャント名は [Spigot JavaDoc (Enchantment)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/enchantments/Enchantment.html) を参考にして記載してください。  

### metadata
成果物に付与するメタデータは `metadata` セクションに記載してください。  
このセクションは、`種別,値` の順番で記載してください。  
また、このセクションは付与したいメタデータが1つであってもリスト形式で記載してください。  

種別は以下の値のみを設定することが出来ます。  
#### lore
`lore` では、アイテムの説明を追加することが出来ます。  
以下のように記載してください。  
```yaml
metadata:
  - "lore,説明文をここへ"
```  

#### displayName
`displayName` では、アイテムの表示名を変更することが出来ます。  
各種装飾コードを使用することが出来ます。  
以下のように記載してください。  
```yaml
metadata:
  - "displayName: CustomItem"
```

#### enchantment (非推奨)
`enchantment` では、アイテムにエンチャントを付与することが出来ます。  
`エンチャント名,レベル` の順番で記載してください。  
__エンチャントを付与する際は、`enchant` セクションで指定してください。__

#### itemFlag
`itemFlag` では、アイテムフラグを設定することが出来ます。  
[Spigot JavaDoc (ItemFlag)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/ItemFlag.html) を参考にして記載してください。  
以下のように記載してください。  
```yaml
metadata:
  - "itemFlag,hide_enchants"
```  

#### unbreakable
`unbreakable` では、アイテムの耐久を無限にするかを設定することが出来ます。  
`unbreakable,true` もしくは `unbreakable,false` のどちらかのみを設定することが出来ます。  
```yaml
metadata:
  - "unbreakable,true"
```

#### customModelData
`customModelData` では、アイテムにカスタムモデルデータを指定することが出来ます。  
`customModelData,モデル番号` の順に記載してください。  
```yaml
metadata:
  - "cutomModelData,1"
```  

#### potionData
__この設定値は、成果物のアイテムIDが `potion`, `splash_potion`, `lingering_potion` である場合にのみ使用することが出来ます。__  

`potionData` では、アイテムにポーション効果を設定することが出来ます。  
`ポーション効果,ポーションの持続時間,ポーションのレベル` の順に記載してください。  
```yaml
metadata:
  - "potionData,jump,200,10"
```  
上記の例では、跳躍力上昇レベル10、効果時間10秒をポーションに設定します。  
ポーション効果は [Spigot JavaDoc (PotionEffectType)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/potion/PotionEffectType.html) を参考に記載してください。 


---

#### potionColor
__この設定値は、`potionData` と同じく成果物のアイテムIDが `potion`, `splash_potion`, `lingering_potion` である場合のみ使用することが出来ます。__  

`potionColor` では、ポーションの色を設定することが出来ます。  
RGB で表記し、 `r,g,b` の順に記載してください。  
```yaml
metadata:
  - "potionColor,0,255,0"
```  

---

#### attribute_modifier
`attribute_modifier` では、アイテムの属性を設定することが出来ます。  
フォーマット 1
```yaml
metadata:
  - attribute_modifier,type:属性/operation:操作/value:値
```

フォーマット 2
```yaml
metadata:
  - attribute_modifier,type:属性/operation:操作/value:値/slot:適用スロット
```

属性、操作、値、適用スロットに使用することが出来る値については [AttributeModifier (Recipeのメタデータ)][recipe-attributemodifier] と共通なので、そちらを参照してください。

---

#### book_field
`book_field` では、本の  
- 作者
- 世代
- 本文　　

を設定することが出来ます。  

フォーマット  
```yaml
metadata: 
  - book_field,type:操作タイプ/value:値
```  

操作タイプには  
- author (作者名の設定)
- title (タイトルの設定)
- add_page (本の一番後ろのページに付け足す)
- generation (本の世代の設定 -> 原本、コピー、コピーのコピー、ボロボロの本のどれか)
- add_long (長い文章を自動でページ分割して記載する)
- add_long_extend (外部ファイルから文字列を読み込んだ後に自動的にページ分割して記載する)

のいずれかを指定してください。(カッコ内は各操作タイプの説明です。)

generation の場合には以下の文字列のどれかを記載してください。
- copy_of_copy
- copy_of_original
- original
- tattered

generation の技術的仕様や詳細は [BookMeta.Generation (Spigot JavaDoc)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/meta/BookMeta.Generation.html) を参照してください。

例)
```yaml
metadata:
  - book_field,type:generation/value:copy_of_copy
```

add_long_extend の場合には対象のファイルの絶対パスを記載することをお勧めします。
例)
```yaml
metadata:
  - book_field,type:add_long_extend/value:/home/aruka/example.txt
```

また、外部ファイルに保存されている文字列の長さが 25600 文字より長い場合は、 1 文字目から 25600 文字目までを本に記載します。  

なお、 25600 文字は半角英数字を改行無しで書き込んだ場合の最大値であるため、全角文字や改行を使用する場合は適用可能な文字数が 25600 文字よりも短くなることに注意してください。  
Unicode フォントの使用を強制している場合としていない場合においても表示可能な文字数に差が出ますのでご注意ください。  
本プラグインは Unicode フォントの使用を前提として設計されています。実際に確認する際はその点に注意してください。   


---

#### leather_armor_color
`leather_armor_color` では、革防具の色を設定することが出来ます。  

フォーマット 1
```yaml
metadata:
  - leather_armor_color,type:rgb/value:R->0,G->0,B->0
```

このフォーマットでは革防具の色を sRGB で指定することが出来ます。  
R, G, B それぞれで設定することが出来る数値の範囲は `0 ~ 255` です。  
各要素は整数値で指定する必要があります。  
範囲外の数値を指定すると  
- 0 未満の場合 : コンソールにエラー文が表示されます
- 256 以上の場合 : 内部的に数値を 255 へ変更し適用されます  


フォーマット 2
```yaml
metadata:
  - leather_armor_color,type:name/value:aqua
```

このフォーマットでは革防具の色を色の名前で指定することが出来ます。  
指定することが出来る色の一覧を下に示します。  
- aqua
- black
- blue
- fuchsia
- gray
- green
- lime
- maroon
- navy
- olive
- orange
- purple
- red
- silver
- teal
- white
- yellow

(fuchsia : フクシア、赤紫 -> R:213, G:128, B:178)  
(teal : ティール、ターコイズ、青緑 -> R:0, G:128, B:128)  
上に示した一覧に記載されていない色名を指定した場合、コンソールにエラー文が表示されます。  

フォーマット3
```yaml
metadata:
  - leather_armor_color,type:random
```

このフォーマットでは革防具の色をランダムで設定することが出来ます。  
sRGB の各要素を 0 ~ 255 の範囲でランダムに決定し適用します。  

---

### container
このセクションでは、アイテム作成時に素材が持つコンテナデータを条件に設定することが出来ます。

フォーマット
```yaml
container:
  番号:
    key: コンテナ名
    type: コンテナに格納されているデータの型
    tag: タグ種別 
    order: 順番
    value: 条件
```

例
```yaml
container:
  0:
    key: test
    type: string
    tag: allow_tag
    order: 0
  1:
    key: test_2
    type: double
    tag: allow_value
    order: 1
    value: 10 <--> 20
  2:
    key: test_3
    type: double
    tag: deny_value
    order: 2
    value: 20 <
```

#### コンテナ名
コンテナ名にはアルファベットの大文字、小文字、アンダーバー、ハイフン、0 から 9 の数字を使用して、対象のコンテナを指定することが出来ます。

#### コンテナに格納されているデータの型
コンテナに格納されているデータには「型」が存在します。
- string (文字列型)
- double (小数を含む数値型)
- int (整数型)

の中から適切なものを指定してください。

#### タグ種別
- allow_tag
- deny_tag
- allow_value
- deny_value
- store_only

の中から適切なものを指定してください。

`allow_tag` は指定した名前のコンテナが含まれている場合に、条件を満たしたとしてアイテム作成の処理が続行されます。

`deny_tag` は指定した名前のコンテナが含まれている場合に、条件を満たさないとしてアイテム作成の処理が破棄されます。

`allow_value` は指定した名前のコンテナに含まれているデータが、後述する `value` で指定された条件に合致する場合にのみアイテム作成の処理が続行されます。

`deny_value` は指定した名前のコンテナに含まれるデータが、後述する `value` で指定された条件に合致しない場合にのみアイテム作成の処理が続行されます。

`store_only` は指定された名前のコンテナは他の計算などで用いられるデータを保持していることを示すため、常にアイテム作成の処理は続行されます。

#### 順番
0 から整数を順に当てはめていってください。

#### 条件
`value` には上記の `tag` で `allow_value`、もしくは `deny_value` を選択した場合にのみ値を設定することが許可されます。
条件の記述方法にはいくつかのフォーマットが存在します。

下記のリストにおける `TARGET` は、条件に使用する文字列や数値、または `$データを格納するコンテナ名` を表します。  
また、`TARGET` では記載された数値やコンテナに格納された数値データの四則演算と乗算を行うことも可能です。

- 矢印範囲法
  - `TARGET <--> TARGET`
  - 左に配置された TARGET よりも大きく、右に配置された TARGET よりも小さい場合にのみ条件に合致したとして扱われます。

- 小なり指定法
  - `< TARGET`
  - TARGET より小さい場合にのみ条件に合致したとして扱われます。

- 大なり指定法
  - `TARGET <`
  - TARGET より大きい場合にのみ条件に合致したとして扱われます。

---

また、`allow_tag` か `deny_tag` の場合に限り、 `value` でまとめてコンテナを指定することが出来ます。  
フォーマット
`(multi)(types:タイプ) コンテナ名`

`types` には、`値の型*数` の形式で記述し、 `,` (カンマ)で区切ってください。
例) `(types:string*2,double*2)`

コンテナ名を記述する順番は `types` にて指定した型の順番とコンテナが持つ値の型が一致するようにしてください。

例) 
```yaml
(multi)(types:string*2,double*2,int*2) a,b,c,d,e
# このように指定した場合、

# a は string 型
# b, c は double 型
# d, e は int 型

#として扱われます。
```

### パススルー用メタデータ
パススルーモード用のメタデータは  
`pass_through,mode=pass/type=([\w\d-_]+)/action=([\w\d-_]+)/value=(.+)`  
という正規表現パターンに従う必要があります。  
#### enchant
パススルーするアイテムからエンチャントを
- 追加
- 削除

することが出来ます。  
エンチャントを追加する際はレベルも設定してください。  

`value` に使用される正規表現  
追加: `enchant=([\w_]+),level=([\d]+)`  
削除: `([\w]+))`  

例) 追加  
```yaml
metadata:
  - pass_through,mode=pass/type=enchant/action=add/value=enchant=durability,level=10
```  
パススルーするアイテムに `耐久エンチャント レベル10` を付与します。

例) 削除  
```yaml
metadata:
  - pass_through,mode=pass/type=enchant/action=remove/value=durability
```  
パススルーするアイテムから `耐久エンチャント` を削除します。

#### enchant_level
パススルーするアイテムが持つエンチャントのレベルを変更することが出来ます。  
計算の結果が  
- 1 未満になるような場合は 1 に
- 256 以上いなるような場合は 255 に  

設定します。

`value` に使用される正規表現  
`enchant=([\w_]+),change=([\d]+)`  

例) 減算  
```yaml
metdata:
  - pass_through,mode=pass/type=enchant_level/action=minus/value=enchant=durability,change=10
```  
パススルーするアイテムが持つ `耐久エンチャント` の現在のレベルから 10 レベル分減算します。  

例) 加算  
```yaml
metadata:
  - pass_through,mode=pass/type=enchant_level/action=plus/value=enchant=durability,change=10
```  
パススルーするアイテムが持つ `耐久エンチャント` の現在のレベルに 10 レベル分加算します。  

#### lore
パススルーするアイテムが持つ説明文を  
- クリア(全て削除)
- 追加 
- 行を指定して追加

することが出来ます。  

`value` に使用される正規表現。  
クリア: `null`  
追加: `(.+)`  
行を指定して追加: `line=([\d]+),lore=(.+)`  

例) クリア  
```yaml
metadata:
  - pass_through,mode=pass/type=lore/action=clear/value=null
```  
パススルーするアイテムが持つアイテム説明文をすべて削除します。  

例) 追加
```yaml
metadata:
  - pass_through,mode=pass/type=lore/action=add/value=TEST ADD!!
```  
パススルーするアイテムの説明文に `TEST ADD!!` を追加します。  

例) 行を指定して追加  
```yaml
metadata:
  - pass_through,mode=pass/type=lore/action=modify/value=line=3,lore=TEST ADD!!
```  
パススルーするアイテムの説明文の 3 行目に `TEST ADD!!` を追加します。  
アイテムの説明文の行数よりも大きい数字を指定した場合は、文字列が追加されることはありません。  

#### container_modify

#### durability
パススルーするアイテムの耐久値を  
- 加算(回復)
- 減算  

することが出来ます。  

`value` に使用される正規表現  
`([\d]+)`

例) 加算  
```yaml
metadata:
  - pass_through,mode=pass/type=durability/action=plus/value=100
```  
パススルーするアイテムの耐久値を 100 だけ回復します。  
`現在のアイテムの耐久値 + 回復量` がアイテムの耐久値の最大値よりも大きくなった場合は、耐久の最大値で加算はストップします。  

例) 減算  
```yaml
metadata: 
  - pass_through,mode=pass/type=durability/action=minus/value=100
```  
パススルーするアイテムの耐久値を 100 だけ削ります。  
`現在のアイテムの耐久値 - 減算量` が 1 未満になった場合には、残り耐久値を 1 にします。  


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

## using_container_values_metadata
アイテムが持つデータコンテナから値を読み取り、アイテムの作成時に条件として使用したり、各種アイテムの値に適用することが出来ます。  
基本的に `matter 名 <--> 操作の種類 -> 値`　というフォーマットに従ったうえ、リスト形式で記載します。  

値にコンテナ内のデータを使用したい場合は `$コンテナ名` のフォーマットで記載してください。  

アイテム名やアイテムの説明文にてコンテナに格納されたデータを使用したい場合には、対象の箇所に `{$コンテナ名}` という形で記載してください。  
また、`{$コンテナ名}` という文字列を使用したい場合には、 `\{$コンテナ名}` のように中括弧の前にバックスラッシュを記載してください。

なお、指定したコンテナが存在しない場合や中身が無い場合には `"None"` という文字列が指定箇所に挿入されます。

負の数値を記載する際は、`-10` のように `-`(マイナス) を用いるのではなく、 `~`(チルダ) を用いてください。

内部での処理に整数値を使用する箇所では、与えられたデータの丸め込みを行うことがあります。そのため、多少の誤差が表れる事があります。

例) 
```yaml
using_container_values_metadata:
  - matter_1 <--> using_container_values_lore -> This item contains \{$container_1} = {$container_1}
```
この例では、 `matter_1` という Matter から読みだした `$container_1` の値を `This item` から始まる文章の `{$container_1}` に代入します。  
`{}` の中身がすべて数値型 (double, int) の場合にのみ、四則演算と累乗計算を `{}` の中で実行し、計算結果をその場所に代入することが出来ます。  

演算の種類と記号
- 足し算 : +
- 引き算 : -
- 掛け算 : *
- 割り算 : /
- 累乗 : ^

※演算の例  
```yaml
- matter_1 <--> using_container_values_lore -> Value -> {$container_1 + $container_2 - $container_3 * $container_4 / $container_5 % $container_6}
```
各種演算を行う場合は、コンテナの値と演算子の間に半角スペースを挿入してください。  

### lore
このタイプは、アイテムの説明文を操作する際に使用します。  

フォーマット
```yaml
- matter名 <--> using_container_values_lore -> 説明文
```

説明文には半角英数字、全角文字を使用することが出来ます。  
また、`§` を用いた文字装飾を適用することもできます。  

### enchantment
このタイプは、アイテムのエンチャントを操作する際に使用します。  

フォーマット 1
```yaml
- matter名 <--> using_container_values_enchantment -> enchantment:エンチャント種別/level:エンチャントレベル
```

フォーマット 2
```yaml
- matter名 <--> using_container_values_enchantment -> enchantment:$コンテナ名/level:$コンテナ名
```

フォーマット 2 のようにエンチャント種別、レベルの両方、もしくはそれら片方のみにコンテナデータを使用することが出来ます。

### potion_color
このタイプは、ポーションの色を操作する際に使用します。  
指定方法には RGB, random の 2 種類があります。

フォーマット 1
```yaml
- matter名 <--> using_container_values_potion_color -> type:rgb/value:R->Red値,G->Green値,B->Blue値
```
Red, Green, Blue の各値には `0 ~ 255` の整数、または数値型のデータを持つコンテナ名を記載し適用することが出来ます。  

フォーマット2
```yaml
- matter 名 <--> using_container_values_potion_color -> type:random
```
フォーマット 2 を記載したとき、RGB の各要素の値を `0 ~ 255` の範囲からランダムで決定し、適用します。  

### tool_durability
このタイプは、アイテムの耐久値を操作する際に使用します。  
耐久値を操作することが出来ない Matter を対象にこの操作を記述した場合は、エラーを出して終了します。  

フォーマット 1
```yaml
- matter名 <--> using_container_values_tool_durability -> type:absolute/value:耐久値
```
耐久値の部分には、0 より大きく、対象の Matter の最大耐久値以下の整数、もしくは数値型のデータが格納されたコンテナ名を記載し適用することが出来ます。  
指定した値が成果物の残り耐久値になります。

フォーマット 2
```yaml
- matter名 <--> using_container_values_tool_durability -> type:percentage/valeu:残り耐久パーセンテージ
```

残り耐久パーセンテージの部分には 0 ~ 100 の整数値、または数値型のデータが格納されたコンテナ名を記載し適用することが出来ます。  
`成果物の新品状態の耐久値 * 指定したパーセンテージ / 100` を整数に丸めた値が、成果物の残り耐久値になります。

### itme_name
このタイプは、アイテムの名前を操作する際に使用します。  

フォーマット
```yaml
- matter名 <--> using_container_values_item_name -> アイテム名
```
アイテム名の箇所には `{$コンテナ名}` のフォーマットを用いてコンテナデータを挿入することもできます。

### attribute_modifier
[recipe-attributemodifier]: ###attribute_modifier
このタイプは、アイテムの属性を設定する際に使用します。

フォーマット 1
```yaml
- matter名 <--> using_container_values_attribute_modifier -> type:属性/operation:操作/value:値
```

属性には設定したい属性、操作には `add`, `multiply`, `add_scalar` のいずれか、値には数値型のデータを持つコンテナ名か、実数を入力して適用することが出来ます。  

なお、このフォーマットで属性を指定した場合には後述する適用スロットの設定は行われず、どの部位に装着した場合でも効果を発揮するようになります。

フォーマット 2
```yaml
- matter名 <--> using_container_values_attribute_modifier -> type:属性/operation:操作/value:値/slot:適用スロット
```
適用スロットには、
- CHEST
- FEET
- HAND
- HEAD
- LEGS
- OFF_HAND  

のいずれかを指定してください。  
スロットを指定すると、指定した部位にアイテムを装着した場合のみ属性の効果が適用されるようになります。  

属性、操作、適用スロットの技術的仕様や使用可能な値については
- 属性 : [Attribute (Spigot JavaDoc)](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/Attribute.html)
- 操作 : [AttributeModifier.Operation (Spigot JavaDoc)](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/AttributeModifier.Operation.html)
- 適用スロット : [EquipmentSlot (Spigot JavaDoc)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/EquipmentSlot.html)

を参照してください。

属性によって指定可能な `value` の範囲は異なるため注意してください。  
参考 : [MinecraftWiki 属性 (Attribute)](https://minecraft.fandom.com/ja/wiki/%E5%B1%9E%E6%80%A7)
