# Data Class
## Main

- Matter
- Result
- Recipe

## Sub

- Potions
- EnchantWrap
- RecipePermission
- Coordinate

# Enum
- PotionBottleType
- PotionDuration
- PotionStrict
- EnchantStrict
- Tag
- MetadataType

# Util
- DataCheckerUtil
- EnchantUtil
- InventoryUtil
- PotionUtil
- RecipePermissionUtil

# Data Loader
- SettingsLoad

# Crafting processor
- Search
- VanillaSearch

# Command processor
- Check
- PermissionCheck


---
# Matter
素材についてのデータを保持するクラス

## Member variable
- (String) name
- (List\<Material>) candidate
- (List\<EnchantWrap>) wrap
- (int) amount
- (boolean) mass

## Constructor
- Matter
  - String
  - List\<Material>
  - List\<EnchantWrap>
  - int
  - boolean

---

- Matter
  - List\<Material>
  - int

---
- Matter
  - List\<Material>
  - int
  - boolean

---
- Matter
  - Matter

---
- Matter
  - ItemStack

## Methods
### getter
- (String) getName
- (List\<Material>) getCandidate
- (List\<EnchantWrap>) getWrap
- (int) getAmount
- (boolean) isMass


### setter
- (void) setName(String name)
- (void) setCandidate(List\<Material> candidate)
- (void) setWrap(List\<EnchantWrap> wrap)
- (void) setAmount(int amount)
- (void) setMass(boolean mass)

### add
- (void) addCandidate(List<Material> additional)
- (void) addWrap(EnchantWrap in)
- (void) addAllWrap(List\<EnchantWrap> in)

## has
- (boolean) hasWrap

## other methods
### (String) getAllWrapInfo()
この Matter が保持する EnchantWrap の中身を文字列にして返す。<br>
EnchantWrap を持たない場合、から文字列を返す。

### (boolean) caontins(Enchantment enchantment)
この Matter が持つエンチャント情報の中に渡された Enchantment が含まれるかを返す。<br>

### (Matter) copy()
この Matter の中身をコピーした Matter を返す。

### (Matter) oneCopy()
この Matter の中身をコピーし、個数を1にしたものを返す。

### (String) info()
この Matter が持つ、
- name
- candidate
- EnchantWrap
- amount
- mass

を文字列にして返す。

---

# Result
成果物についてのデータを保持するクラス。

## Member variable
- (String) name
- (Map\<Enchantment, Integer>) enchantsInfo
- (int) amount
- (Map\<MetadataType, List\<String>>) metadata
- (String) nameOrRegex
- (int) matchPoint

## Constructor
- Result
  - (String) name
  - (Map\<Enchantment, Integer>) enchantsInfo
  - (int) amount
  - (Map\<MetadataType, List\<String>>) metadata
  - (String) nameOrRegex
  - (int) matchPoint

## Methods
### getter
- (String) getName
- (Map\<Enchantment, Integer>) getEnchantmentsInfo
- (int) getAmount
- (Map\<MetadataType, List\<String>>) getMetadata
- (String) getNameOrRegex
- (int) getMatchPoint

### setter
- (void) setName (String name)
- (void) setEnchantsInfo (Map\<Enchantment, Integer> enchantsInfo)
- (void) setAmount (int amount)
- (void) setMetadata (Map\<MetadataType, List\<String>> metadata)
- (void) setNameOrRegex (String nameOrRegex)
- (void) setMatchPoint (int matchPoint)

## other methods
### (void) setMetadata (ItemStack item)
引数に与えられた ItemStack へ、この Result が保持するメタデータを付与する。

---
# Recipe
アイテムの配置、成果物、作成時に返却するアイテムなどについての情報を保持するクラス。

## Member variable
- (String) name
- (Tag) tag
- (Map\<Coordinate, Matter>) coordinate
- (Map\<Material, ItemStack>) returnItems
- (RecipePermission) permission
- (Result) result

## Constructor
- Matter
  - String
  - Map\<Coordinate, Matter>
  - Map\<Material, ItemStack>
  - Result
  - RecipePermission

---

- Matter
  - no arguments require

## Methods
### getter
- (Tag) getTag
- (String) getName
- (Map\<Coordinate, Matter>) getCoordinate
- (Map\<Material, ItemStack>) getReturnItems
- (Result) getResult
- (RecipePermission) getRecipePermission

### setter
- (void) setTag (Tag)
- (void) setName (String)
- (void) setCoordinate (Map\<Coordinate, Matter>)
- (void) setReturnItems (Map\<Material, ItemStack>)
- (void) setResult (Result)
- (void) setPermission (RecipePermission)

## other methods
### (boolean) hasPermission
このレシピに権限が設定されているか否かを返す。

### (List\<Matter>) getContentsNoAir
このレシピで要求する素材のうち、空気以外の Matter をリストにして返す。

### (List\<Coordinate>) getCoordinateList
レシピ配置の座標をリストにして返す。

### (List\<Matter>) getContentsNoDuplicate
このレシピで要求する素材の重複無しのリストを返す。

### (Map\<Matter, Integer>) getContentsNoDuplicateRelateAmount
このレシピで要求する素材（重複無し）と個数のマップを返す。

### (Set\<Material>) getMassMaterialSet
このレシピで要求する素材で mass = true であるものだけを Set にして返す。<br>
mass = true である素材が無いときは、空の Set を返す。

### (Matter) getMatterFromCoordinate (Coordinate c)
レシピ配置のなかで、引数に取った座標にある素材を返す。<br>
対象の座標に素材が存在しない場合は Null を返す。

---
# Potions
## Member variable
- (Map\<PotionEffect, PotionStrict>) data
- (PotionBottleType) bottle
- (boolean) bottleTypeMatch

## Constructor
- Potions
  - Matter
  - Map\<PotionEffect, PotionStrict>
  - PotionBottyeType
  - boolean

---
- Potions
  - ItemStack
  - PotionStrict

## Methods
### getter
- (Interface) Matters 's methods
- (Map\<PotionEffect, PotionStrict>) getData
- (PotionBottleType) getBottle
- (boolean) isBottleTypeMatch

### setter
- no original setters

### Other methods
### (ItemStack) prescribe
この Potions のデータを持った ItemStack を作成する。

### (String) PotionInfo
この Potions が保持する
- 効果名
- 効果時間
- 効果レベル
- PotionStrict

を文字列にして返す。

### (boolean) hasAnyCustomEffect
この Potions が１つ以上のポーション効果を持つか否かを返す。

### (boolean) hasPotionEffect (PotionEffectType)
この Potions が引数に与えられたポーション効果を持つか否かを返す。

### (int) getDuration (PotionEffectType)
この Potions が持つ、引数に与えられたポーション効果の効果時間を返す。<br>
引数に与えられたポーション効果を持たない場合は、 -2 を返す。<br>
返却される効果時間はゲーム内ティックである。

### (int) getAmplifier (PotionEffectType)
この Potions が持つ、引数に与えられたポーション効果のレベルを返す。<br>
引数に与えられたポーション効果を持たない場合は -1 を返す。


---
# EnchantWrap
## Member variable
- (int) level
- (Enchantment) enchant
- (EnchantStrict) strict

## Constructor
- EnchantWrap
  - int
  - Enchantment
  - EnchantStrict

## Methods
### getter
- (int) getLevel
- (Enchantment) getEnchant
- (EnchantStrict) getStrict 

### setter
- (void) setLevel (int level)
- (void) setEnchant (Enchantment enchant)
- (void) setStrict (EnchantStrict strict)

### other methods
### (String) info
この EnchantWrap が持つ、
- エンチャント名
- レベル
- 要求厳格性

を文字列にして返す。

---
# RecipePermission
## Member variable
- (String) parent
- (String) name

## Defined variable
ROOT<br>
- Parent: NULL
- PermissionName: ROOT

最上位のレシピ権限。<br>
全ての権限は ROOT よりも下位に位置するように設計しなくてはいけない。

## Constructor
- RecipePermission
  - String
  - String

## Methods
### getter 
- (String) getParent
- (String) getPermissionName

### setter
- (void) setParent (String parent) 
- (void) setPermissionName (String name)

### other methods
### (String) toStr
このレシピ権限の権限名と、親の権限名を文字列にして返す。

---
# Coordinate
## Member variable
- (int) x
- (int) y

## Constructor
- Coordinate
  - int
  - int

## MEthods
### getter
- (int) getX
- (int) getY

### setter
- (void) setX (int x)
- (void) setY (int y)

### other methods
### (boolean) isSame (Coordinate coordinate)
この座標と、引数に与えられた座標が等しいか否かを返す。

---

# Enum


# PotionBottleType
ポーションの形状が 
- 普通の瓶
- スプラッシュ瓶
- 残留瓶

の中のどれであるかを示す。

## Methods
### (Material) getRelated
この PotionBottleType に対応する Material を返す。

---
# PotionDuration
- ポーション効果の名称
- 通常時の効果時間
- 強化されたときの効果時間
- 延長されたときの効果時間

を持つ。<br>
ここでの効果時間はゲーム内ティックで表される。

## Methods
### (int) getUpgraded
強化されたときの効果時間を返す。

### (int) getExtended
延長されたときの効果時間を返す。

### (int) getNormal
強化、延長がなされていないときの効果時間を返す。

### (String) getPDName
ポーション効果の名前を返す。

---
# PotionStrict
素材としてポーションを要求する際に、どの要素の一致を求めるかを決定する Enum.
## Methods
### (String) toStr
対応する文字列を返す。

---
# EnchantStrict
素材にエンチャントされていることを求める際に、どの要素の一致を求めるかを決定する Enum.

## Methods
### (String) toStr
対応する文字列を返す。

---
# Tag
レシピが定型か不定形かを示す。

---
# MetadataType
Result に付与するメタデータの種類を表す。

## Methods
### (String) toStr
対応する文字列を返す。

---
# DataCheckerUtil
コンフィグファイルを検査し、文法のエラーを見つけた場合はコンソールへ警告を出す。<br>

## Methods
### (boolean) nameCheck (FileConfiguration, StringBuilder)
引数に与えられたファイルの中身を読み取り、
- name セクションが存在するか
- 正規表現パターン `[a-zA-Z_]+` に合致するか
- name セクションがバニラのアイテム ID と競合していないか

を検査する。<br>
上記の要素が一つでも偽であった場合は false を返す。

### (boolean) intCheck (FileConfiguration, StringBuilder, String, int)
引数に与えられた文字列を、１以上、与えられた制限値未満の数値に変換できるかどうかを返す。

### (boolean) massCheck (FileConfiguration, StringBuilder)
与えられたファイルに含まれる candidate セクションが正しく書かれているかを返す。<br>
- 正規表現を複数用いていないかどうか
- バニラのアイテム ID であるかどうか

を調べる。

### (boolean) isCorrectNameOrRegex (StringBuilder, String, boolean, String)
引数で与えられた文字列が正しいアイテム ID であるかどうかを返す。<br>

### (boolean) massCheck (FileConfiguration, StringBuilder)
mass セクションに真偽値が書かれているか否かを返す。

### (boolean) enchantCheck (FileConfiguration, StringBuilder, boolean int)
enchant セクションに`エンチャント名、レベル、要求厳格性`、もしくは `エンチャント名、レベル` の順で書かれているかどうかを返す。

### (boolean) metadataCheck (FileConfiguration, StringBuilder)
metadata セクションに値が正しく書かれているかを返す。

### (boolean) potionCheck (FileConfiguration, StringBuilder)
potion セクションに値が正しく書かれているかを返す。

### (boolean) tagCheck (FileConfiguration, StringBUilder)
tag セクションに値が正しく書かれているかを返す。

### (boolean) resultSectionCheck (FileConfiguration, StringBuilder)
result セクションに値が正しく書かれているかを返す。

### (boolean) overrideCheck (FileConfiguration, StringBuilder)
override セクションに値が正しく書かれているかを返す。

### (boolean) coordinateCheck (FileConfiguration, StringBuilder)
coordinate セクションに値が正しく書かれているかを返す。

### (boolean) isTagType (String)
与えられた文字列が `normal` または `amorphous` に一致するかを返す。

### (boolean) returnsCheck (FileConfiguration, StringBuilder)
returns セクションに値が正しく書かれているかを返す。

### (void) appendLn (StringBuilder, String)
与えられた StringBuilder に与えられた文字列と改行を追加する。

### (List\<String>) getMetadataTypeStringList
MetadataType の各値に対応する文字列のリストを返す。

### (void) matterCheck (StringBuilder, FileConfiguration, Path)
- nameCheck
- intCheck
- candidateCheck
- massCheck
- enchantCheck
- potionCheck

を実行し、正しい matter ファイルでない場合はコンソールに警告を表示する。


### (void) resultCheck (StringBuilder, FileConfiguration, Path)
- nameCheck
- intCheck
- enchantCheck
- metadataCheck

を実行し、正しい result ファイルでない場合はコンソールに警告を表示する。

### (void) recipeCheck (StringBuilder, FileConfiguration, Path)
- nameCheck
- tagCheck
- resultSectionCheck
- overrideCheck
- coordinateCheck
- returnsCheck

を実行し、正しい recipe ファイルでない場合はコンソールに警告を表示する。


---
# EnchantUtil
## Methods
### (boolean) containsFromDoubleList (List\<List\<EnchantWrap>>, Matter)
入力されたリストと Matter に付与された EnchantWrap を比較して、厳格性が合致するかを返す。<br>
内部でダミーデータを Search#getEnchantWrapCongruence に引き渡している。

### (List\<String>) strValuesNoInput
EnchantStrict に対応する文字列をリストにして返す。<br>
なお、 `EnchantStrict#INPUT` を除く。

### (List\<String>) getEnchantmentStrList
Enchantment の各値に対応する文字列をリストにして返す。

---

# InventoryUtil
### Methods
### (List\<Integer>) getTableSlots (int)
最大サイズのインベントリにおいて、左上の角を頂点とした与えられた数値を辺の長さとする正方形が占めるスロットの番号をリストにして返す。

### (List\<Integer>) getBlankCoordinates (int)
クラフト画面のうち、黒い板ガラスが配置されたスロットの番号をリストにして返す。

### (void) decrementResult (Inventory, Player)
クラフト画面の成果物スロットにアイテムがある場合、そのアイテムを引数に与えられたプレイヤーの座標にドロップする。

### (void) decrementMaterials (Inventory, Player, int)
Inventory に存在するアイテムの個数を int だけ減らす。

### (void) returnItems (Recipe, Inventory, int, Player)
引数に与えられたプレイヤーに対して、 Recipe の成果物を int だけ与える。

### (void) drop (ItemStack, int, Player)
引数に与えられたアイテムを int だけプレイヤーの座標にドロップさせる。

---
# PotionUtil
## Methods
### (int) getDuration (String, boolean boolean PotionBottleType)
引数に与えられた要素を用いて PotionDuration より効果時間を取得して返す。

### (PotionBottleType) getBottleType (Material)
引数に与えられた Material に対応する PotionBottleType を返す。

### (boolean) isPotion (Material)
引数に与えられた Material が
- Potion
- Splash_potion
- lingering_potion

のいずれかであるかを返す。

### (List\<String>) getPotionEffectTypeStringList
PotionEffectType の名前をリストにして返す。

### (List\<String>) getPOtionStrictStringList
PotionStrict の名前をリストにして返す。

### (Potions) water_bottle
水入り瓶を Potions 型で表現したものを返す。

### (ItemStack) water_bottle_ItemStack
水入り瓶を ItemStack 型で表現したものを返す。

### (boolean) isWaterBottle (Matter matter)
-

### (boolean) isSamePotion (Potions, Potions)
引数に与えられた２つのポーションが等しいかどうかを返す。<br>
このとき、
- Not_Strict
- Only_Effect
- Only_Duration
- Only_Amplifier
- Strict

を判定に用いる。

### (List\<String>) getPDName
PotionDuration の各値に紐づけられた文字列をリストにして返す。

### (void) makeDefaultPotionFiles (String, boolean, boolean, boolean, String)
渡された引数に基づき、 PotionDuration に格納された各ポーション効果を持つポーションの Matter ファイルを自動作成する。<br>
バニラ環境で作成することが出来ないポーションについてのファイルは作成されない。

### (void) makeDefaultPotionFilesWrapper
makeDefaultPotionFiles を非同期で動かすためのラッパー。

### (void) binary (int, List\<Boolean>)
与えられた数値を2進数に変換し、0 を false に、 1 を true に変換してリストに追加する。<br>
再帰関数。

### (boolean) writerWrapper (List\<String>, File)
引数に与えられた文字列をファイルに出力する。<br>
IOException をキャッチすると false を返す。

---

# RecipePermissionUtil
## Global variables
### Map\<String, RecipePermission> recipePermissionMap
RecipePermission の名前をキーに、RecipePermission 本体を値に持つ Map.

### Map\<UUID, List\<RecipePermission>> playerPermissions
プレイヤーの UUID をキーに、 そのプレイヤーが持つ RecipePermission のリストを値に持つ Map.

## Methods
### (void) makeRecipePermissionMap (List\<RecipePermission>)
引数に与えられた RecipePermission のリストと ROOT を recipePermissionMap へ追加する。

### (void) playerPermissionWriter (Path)
playerPermissions の中身を Path が示すファイルに出力する。

### (boolean) hasPermission (RecipePermission, Player)
Player が RecipePermission を所持しているかを返す。

### (void) permissionRelated (Path)
Path が示すファイルからプレイヤーが持つ権限のリストを作成し、 playerPermissions を構築する。

### permissionSettingsLoad (Path)
Path が示すファイルから RecipePermission の親権限の名前と、権限自身の名前を読み取り RecipePermission を作成する。<br>
作成した RecipePermission をリストにし、 makeRecipePermissionMap へ渡す。

### (List\<RecipePermission>) removePermissionDuplications (List\<RecipePermission>)
与えられたリスト内の RecipePermission のうち、その権限よりも上位の権限がリスト内に存在するものを削除する。

### (List\<RecipePermission>) permissionSort (List\<RecipePermission>)
与えられたリスト内の RecipePermission を昇順に並べ替え、その結果のリストを返す。

### (boolean) inSameTree (RecipePermission, RecipePermission)
第1引数と第2引数に与えられた権限が共通の分岐点を持つかを返す。

### (List\<RecipePermission>) getLonger (List\<RecipePermission>, List\<RecipePermission>)
引数に与えられたリストのうち、要素が多い方を返す。<br>
要素数が同数の場合は第1引数のリストを返す。

---

