# config.yml

## baseBlock
baseBlock が書かれたファイルが置かれたディレクトリのパス

# matters
Matter ファイルが置かれたディレクトリのパス（リスト）

## results
Result ファイルが置かれたディレクトリのパス（リスト）

## recipes
Recipe ファイルが置かれたディレクトリのパス（リスト）

## notice
コンフィグファイルを再読み込みした際に表示されるメッセージ（リスト）

## download_threshold
## load_interval
## download
ダウンロードに関するコンフィグ（開発中）


# Notice
- これより先の説明にて用いる「正規表現」は Java における正規表現パターンを指しています。<br>
- リスト形式などは概ね yaml の文法に従います。
- 水入り瓶はアイテム ID `water_bottle` で呼び出せます。

# Matter files
### Need
## name
Matter の名前。Recipe ファイルで呼び出す際に使用する文字列<br>
バニラのアイテム ID を重複してはいけない。
## amount
要求量。1 以外に設定した時の動作は保証しません
## mass
クラフト時にこの素材の個数を無視するか否か
## candidate
この素材として認めるアイテムのリスト<br>
例として、石か丸石を素材として指定する場合は candidate: [stone,cobblestone] となる<br>
正規表現を記述して複数のアイテムを登録することが可能<br>
正規表現を利用する際は `R|` を文頭に加えること

---

### Option
## enchant
- エンチャント
- レベル
- 要求厳格性  
をカンマ区切りの文字列にしたもの。

### レベル（エンチャント）
1 ~ 255 の整数（半角数字）

### 要求厳格性（エンチャント）
`NotStrict`, `OnlyEnchant`, `Strict` の3種類

- NotStrict : このエンチャントを要求しない<br>
- OnlyEnchant : このエンチャントが含まれていることを要求する（レベル不問）<br>
 - Strict : このエンチャントが含まれており、かつレベルも一致していることを要求する

---

## potion
- ポーション効果
- 効果時間(tick)
- レベル
- 要求厳格性  
をカンマ区切りの文字列にしたもの

### ポーション効果
[Spigot JavaDoc (PotionEffectType)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/potion/PotionEffectType.html) を参照のこと

### 効果時間
ゲーム内 tick で記述 (1秒 = 20 ticks)

### レベル（ポーション）
1 ~ 255 の整数（半角数字）

### 要求厳格性（ポーション）
`Not_Strict`, `Only_Effect`, `Only_Duration`, `Only_Amplifier`, `Strict` の5種類

- Not_Strict : このポーション効果を要求しない<br>
- Only_Effect : ポーション効果の一致を要求する
- Only_Duration : ポーション効果と効果時間の一致を要求する<br>
- Only_Amplifier : ポーション効果とレベルの一致を要求する<br>
- Strict : ポーション効果、レベル、効果時間の一致を求める

---

## bottleTypeMatch
瓶の形状の一致を求めるか否か<br>
瓶の形状
- 普通の瓶
- スプラッシュ
- 残留

# Matter file example
- mixed_stone という名前で呼び出すことが出来る
- 個数1
- クラフト時に個数を考慮する
- 石か丸石を素材として受け付ける
```yaml
name: mixed_stone
amount: 1
mass: false
candidate: [stone,cobblestone] 
```

# Matter file example-2
- color_concrete_powder という名前で呼び出すことが出来る
- 個数1
- クラフト時に個数を考慮する
- 各色のコンクリートパウダーを素材として受け付ける
```yaml
name: color_concrete_powder
amount: 1
mass: false
candidate: ["R|^([A-Z_]+)_CONCRETE_POWDER$"]
```

# Matter file example-3
- silk_touch_diamond_pickaxe という名前で呼び出すことが出来る
- 個数1
- クラフト時に個数を考慮しない
- ダイヤモンドのつるはしのみを素材として受け付ける
- シルクタッチのエンチャントが付与されていることを要求する
```yaml
name: silk_touch_diamond_pickaxe
amount: 1
mass: true
candidate: [diamond_pickaxe]
enchant:
  - silk_touch,1,only_enchant
```

# Matter file example-4
- jump_speed_potion という名前で呼び出すことが出来る
- 個数1
- クラフト時に個数を考慮しない
- ポーション（普通のガラス瓶入り）のみを素材として受け付ける
- 跳躍力上昇、移動速度上昇のポーション効果が付与されていることを要求する
```yaml
name: jump_speed_potion
amount: 1
mass: true
candidate: [potion]
potion:
  - jump,0,0,Only_Effect
  - speed,0,0,Only_Effect
```


---

# Result files
### Need
## name
Result の名前。Recipe ファイルで呼び出すときに使用する。<br>
バニラのアイテム ID を重複してはいけない。
## amount
Result の個数
## matchPoint
後述するアイテムIDの指定に正規表現を用いるときにのみ適切に設定する。<br>
**正規表現を使用しない場合は -1 を設定する**
## nameOrRegex
Result のアイテムID。<br>
アイテムIDを直接指定するか正規表現を記述して指定する。<br>
正規表現を使用し素材のアイテムIDから Result のアイテムIDを作成する場合は、それらの区切りに `@` を挿入すること。<br>
また、抜き出した文字列で置き換える部分には `{R}` を挿入すること。<br>
複数個所から文字列を抜き出すことは出来ない。

---

### Option
## enchant
- エンチャント
- レベル

をカンマ区切りの文字列にしたもの。

---

## metadata
- 付与するメタデータの種類
- メタデータの中身

をカンマ区切りの文字列にしたもの。

### metadataの種類
- lore : アイテムの説明文を設定する。
- displayName : アイテムの表示名を設定する。
- enchantment : `エンチャント名,レベル` の形式で与えられたエンチャントを付与する。
- itemFlag : アイテムフラグを設定する。[Spigot JavaDoc (ItemFlag)](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/ItemFlag.html) を参照のこと。
- unbreakable : 耐久値を無限にするか否か。
- customModelData : アイテムのカスタムモデルデータを指定する。

- potionData : ポーション効果を指定する。
- potionColor : ポーションの色を RGB で指定する。（各値は0 ~ 255 の範囲内）

なお、`potionData`, `potionColor` に関しては、`nameOrRegex` において
- `potion`
- `splash_potion`
- `lingering_potion` 

のいずれかを指定している場合にのみ使用可能。

# Result file example
- dig_speed_mending_diamond_pickaxe という名前で呼び出すことが出来る
- 個数1
- ダイヤモンドのつるはし
- Speed mining Pickaxe という表示名
- 説明文の1行目は `Increase dig speed.`, 2行目は `Added mending.`
- 採掘速度上昇レベル5が付与されている
- 修繕レベル1が付与されている
```yaml
name: dig_speed_mending_diamond_pickaxe
amount: 1
nameOrRegex: diamond_pickaxe
matchPoint: -1
metadata:
  - displayName,Speed mining Pickaxe
  - lore,Increase dig speed.
  - lore,Added mending.
  - enchantment,dig_speed,5
  - enchantment,mending,1
```

# Result file example-2
- lucky_potion という名前で呼び出すことが出来る
- 個数1
- ポーション（普通の瓶）
- Lucky Potion という表示名
- 説明文の1行目に `Luck Potion!!!`
- 幸運レベル100が100秒持続するポーション効果
- 吐き気レベル5が5秒間持続するポーション効果
- ポーションの色が RGB(0,255,0)
```yaml
name: lucky_potion
amount: 1
nameOrRegex: potion
matchPoint: -1
metadata:
  - displayName,Lucky Potion
  - lore,Luck Potion!!!
  - potionData,Luck,2000,100
  - potionData,confusion,100,5
  - potionColor,0,255,0
```

# Result file example-3
- color_concrete_powder という名前で呼び出すことが出来る
- 個数3
- 入力されたコンクリートパウダーと同じ色のコンクリートを ID に指定する
```yaml
name: color_concrete_powder
amount: 3
nameOrRegex: "^([A-Z_]+)_CONCRETE_POWDER$@{R}_CONCRETE"
matchPoint: 1
```

---

# Recipe files
### Need
## name
Recipe の名前。
## tag
レシピが定形か不定形かを示す。<br>
- 定形 : normal
- 不定形 : amorphous
## result
成果物の名前。バニラのアイテム ID を指定でも可。
## coordinate
素材の配置。定型と不定形で書き方が異なる。<br>

- 定形レシピの場合、1\*1 ~ 6\*6 の正方形のみ可能。
- 不定形レシピの場合、1行のリストのみ可能。
- アイテムを置かない場所には null を書く。
- バニラのアイテム ID を書いた場合、個数1、mass = false になる。
- `m-バニラのアイテムID` と書いた場合、個数1、mass = true になる。

### Option
## override
coordinate に記載するアイテム名を他の文字列に置換する。リスト形式。<br>
`アイテムID -> 置換後のアイテムID` の形式。`->` の前後には半角空白が必須。<br>

---

## returns
アイテムを作成した際に返却するアイテムを設定する。<br>
- `対象のアイテムID,返却するアイテムID,個数` の形式で書く。<br>
- 対象のアイテム ID は正規表現を用いて指定することもできる。<br>
- 返却するアイテム ID に `pass` を指定した時、`対象のアイテム ID` に合致するアイテムを入力されたときの状態そのままに返却する。

個数は自然数の範囲から指定する。

## permission
レシピに設定する権限。

# Recipe file example
- 定形レシピ
- エンチャントされた金リンゴを成果物として返す
```yaml
name: enchanted_golden_apple
tag: normal
result: enchanted_golden_apple
override:
  - gold_block -> g
  - apple -> a
coordinate:
  - g,g,g
  - g,a,g
  - g,g,g
```

# Recipe file example-2
- 不定形レシピ
- ダイヤモンドつるはしとダイヤモンド鉱石からダイヤモンドをクラフトする
- クラフト時につるはしはそのまま返却し、ダイヤモンド鉱石と同数の丸石を返す
```yaml
name: diamond_dig
tag: amorphous
result: diamond
override:
  - m-diamond_pickaxe -> p
  - diamond_ore -> d

returns:
  - diamond_pickaxe,pass,1
  - diamond_ore,cobblestone,1

coordinate:
  - [p,d]
```

# Recipe file example-3
- 定形レシピ
- 糸、ウサギの皮からバンドルをクラフトする
```yaml
name: bundle
tag: normal
result: bundle
override:
  - rabbit_hide -> r
  - string -> s
  - null -> n

coordinate:
  - s,r,s
  - r,n,r
  - r,r,r
```