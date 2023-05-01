# `config.yml`
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

例) `recipes: [plugins/Custom_Crafter/recipes1,plugins/Custom_Crafter/recipes2`  
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
### 成果物の名前
成果物の名前は `name` セクションに記載してください。  
レシピを作成する際に使用します。  
バニラアイテムのIDと異なる必要があり、加えて、他の成果物と重複することがあってはいけません。  
もしも一つの名前を複数の素材で使用してしまった場合は、同名の成果物のうち、最後に読み込まれた設定ファイルの内容が適用されます。ご注意ください。  

例) `name: genetically_modified_crop`

### 成果物の個数
成果物の個数は `amount` セクションに記載してください。  
1以上の整数を記載してください。  

例) `amount: 3`

### matchPoint
後述の成果物のアイテムIDを指定する際に正規表現を使う場合のみ適切に設定してください。  
成果物のアイテムIDの指定に正規表現を使用しない場合は、このセクションの値を `-1` に設定しておいてください。

例) `matchPoint: -1`

### 成果物のアイテムID
成果物のアイテムIDは `nameOrRegex` セクションに記載してください。  
このセクションではアイテムIDを直接指定するか、もしくは正規表現を用いて素材のアイテムIDから文字列を切り出してアイテムIDを作成する方法のどちらかを使用することが出来ます。  
正規表現を使用しない場合は、[Spigot JavaDoc (Material)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html) を参考にアイテムIDを記載してください。  

例) `nameOrRegex: stone`  

正規表現を使用し、素材のアイテムIDから成果物のアイテムIDを作成し適用する場合は  
`素材のアイテムIDに使用する正規表現@成果物のアイテムIDを作成する文字列`  
の形を取ってください。  

素材のアイテムIDから切り出した文字列を、成果物のアイテムIDを作成する文字列に適用するためには、置き換える箇所に `{R}` を挿入してください。  

例) `nameOrRegex: "([a-zA-Z_]{1,20})_CONCRETE_POWDER@{R}_CONCRETE"`  
上記の例が設定されているレシピにおいて、素材に色付きコンクリートパウダーが設定されており、 `matchPoint` に `1` が設定されている場合(例として、白色のコンクリートパウダーが設定されている場合)、成果物のアイテムIDは  
`WHITE_CONCRETE_POWDER` より切り出された `WHITE` を `{R}` の箇所に挿入するため `WHITE_CONCRETE` となります。  

マッチ箇所は1か所のみ設けることが出来ます。複数のマッチ箇所を設けることはできません。  

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

### 成果物に付与するメタデータ
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

### potionColor
__この設定値は、`potionData` と同じく成果物のアイテムIDが `potion`, `splash_potion`, `lingering_potion` である場合のみ使用することが出来ます。__  

`potionColor` では、ポーションの色を設定することが出来ます。  
RGB で表記し、 `r,g,b` の順に記載してください。  
```yaml
metadata:
  - "potionColor,0,255,0"
```  

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
