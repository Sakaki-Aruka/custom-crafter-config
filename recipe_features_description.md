# アイテム装飾ガイド
このドキュメントは Custom Crafter のレシピの成果物に装飾を施す際に要求されるコンフィグの書き方について解説しています。  
  
システムが不正なコンフィグを検知した際にはエラーメッセージをサーバーコンソールに発信し、そのコンフィグで指定されている操作を行わずに次のコンフィグの解析に移行します。  
  
基本的にすべての装飾セクションにおいて、プレースホルダを用いたコンテナデータの挿入や数値計算をサポートしています。  
また、`random : OK` と記載されている箇所では数値 (またはその処理固有の値) のランダム構文をサポートしています。  
<details><summary>数値のランダム構文</summary>

## 数値のランダム構文
`random[下限?:上限?]` のフォーマットに従ってください。`下限?`, `上限?` はそれぞれ記載されていても、されていなくてもどちらでも良いことを示しています。使用箇所ごとに内部で下/上限値が設定されているので、`random[:]` でも機能します。(なお、コンフィグに記載した下/上限値がシステム内部で設定された値の範囲から外れている場合は、**システム内部で定義された範囲が優先して適用されます。**)  

それらの組み合わせに取りうる値の範囲を以下に列挙します。  
- `random[:]` :  システム内部で設定された下限から上限までの全ての値
- `random[下限:]` : コンフィグに記載された下限値からシステム内部で設定された上限値までの値
- `random[:上限]` : システム内部で設定された下限値からコンフィグに記載された上限値までの値
- `random[下限:上限]` : コンフィグに記載された下限値から上限値までの値

それぞれのシステム内部で設定された下限値、上限値は `random under : ~~~`, `random upper : ~~~` のように記載しています。
</details>

<details><summary>その他のランダム構文</summary>
  
## その他のランダム構文
基本的にランダムに値を取り出す文は `random[値]` の形式を取り、全ての要素を示す `all`、現在含まれている値を示す `self` が用意されています。(一部の構文では `self` が用意されていないことがあります。)  
  
`!` を用いて否定を表すこと、`,` で要素を区切ることも、ほとんどのランダム構文に共通しているルールです。  
</details>

---

`player data: OK` と記載されている項目については以下のプレイヤーデータをプレースホルダ内で使用することができます。  

## プレイヤーデータ一覧
<details><summary>文字列データ</summary>

### 文字列データ
- `$PLAYER_NAME$` : プレイヤー名
- `$PLAYER_UUID$` : プレイヤーの UUID
- `$PLAYER_CURRENT_WORLD$` : プレイヤーが存在するワールド名
- `$PLAYER_DISPLAYED_NAME$` : 他プレイヤーに表示されるプレイヤー名
- `$PLAYER_CLIENT_BRAND_NAME$` : プレイヤーのクライアント名 (空の場合は `null`)
- `$PLAYER_CURRENT_GAME_MODE$` : プレイヤーの現在のゲームモード
- `$PLAYER_FACING$` : プレイヤーが向いている方角

</details>

<details><summary>数値データ (整数)</summary>

### 数値データ(整数)
- `$PLAYER_CURRENT_Xi$` : プレイヤーの X 座標(小数点以下切り捨て)
- `$PLAYER_CURRENT_Yi$` : プレイヤーの Y 座標(小数点以下切り捨て)
- `$PLAYER_CURRENT_Zi$` : プレイヤーの Z 座標(小数点以下切り捨て)
- `$PLAYER_CURRENT_FOOD_LEVEL$` : プレイヤーの満腹度
- `$PLAYER_PING$` : プレイヤーの ping 値
- `$PLAYER_EXP_LEVEL$` : プレイヤーの経験値レベル
- `$PLAYER_MAXIMUM_NO_DAMAGE_TICKS$` : プレイヤーが最も長くダメージを受けなかったゲーム内ティック

 </details>

 <details><summary>数値データ (小数点有)</summary>

### 数値データ(小数点有)
- `$PLAYER_CURRENT_X$` : プレイヤーの X 座標
- `$PLAYER_CURRENT_Y$` : プレイヤーの Y 座標
- `$PLAYER_CURRENT_Z$` : プレイヤーの Z 座標
- `$PLAYER_CURRENT_PITCH$` : プレイヤーのピッチ方向の回転量
- `$PLAYER_CURRENT_YAW$` : プレイヤーのヨー方向の回転量
- `$PLAYER_EXP$` : プレイヤーが現在持つ経験値量
- `$PLAYER_CURRENT_HEALTH$` : プレイヤーの体力値

</details>

<details><summary>真偽値</summary>

### 真偽値
- `$PLAYER_IN_WATER$` : プレイヤーが水に浸っているか
- `$PLAYER_IS_FLYING$` : プレイヤーが飛んでいるか
- `$PLAYER_IN_RAIN$` : プレイヤーが雨にうたれているか
- `$PLAYER_IN_LAVA$` : プレイヤーが溶岩の中にいるか

</details>

---

# 使用可能な操作一覧

<details><summary>lore</summary>

# lore
効果 : アイテムに説明文を追加します。

---

- `player data: OK`

正規表現 : `type: lore, value: .+`

---

`value: ` の後ろに説明文を記載してください。

e.g `type: lore, value: This is a pen.`

</details>

<details><summary>enchant, enchant_book</summary>

# enchant, enchant_book
効果 : 
- アイテムにエンチャントを追加します。(enchant)
- エンチャントされた本に効果を追加します。(enchant_book)

---

- `player data: OK`
- `random : OK`
  - `random under(level): 1`
  - `random upper(level): 255`

--- 

正規表現(enchant) : `type: enchant, value: type=(enchant|level),action=([a-zA-Z_\\[\\](),!]+)->(.+)`

正規表現(enchant_book) : `type: enchant_book, value: type=(enchant|level),action=([a-zA-Z_\\[\\](),!]+)->(.+)`

---

enchant と enchant_book は同じ構文を使用するため、このセクションでまとめて解説します。  
  
`type=` は、エンチャントの種類を変更する、もしくは新規にエンチャントを付与する場合であれば `enchant` を選んでください。レベルを操作する場合は `level` を選んでください。  
  
`action=` の後ろには、操作対象のエンチャント名を記載してください。(ランダム構文を用いても可 / ランダム構文については `エンチャントのランダム構文` を参照してください)  
  
`->` の後ろには、`type=enchant` であればエンチャント(ランダム構文でも可)、もしくは `None` を、`type=level` であれば変更後のエンチャントレベルを記載してください。  
  
e.g `type: enchant, value: type=enchant,action=random[()]`  

## エンチャントのランダム構文
このセクションでは、エンチャントを指定された要素の中からランダムに決定する構文の解説を行います。

正規表現 : `random(\[[!A-Za-z_,]+])`

---

`[]` の中に否定(`!`) と区切り(`,`) を使用してランダムに選ぶソースを決定していきます。
以下に利用することができるワードの一覧を示します。

- `all` : 全てのエンチャント
- `self` : このワードが呼び出された時点で成果物が含んでいるエンチャント
- `none` : エンチャントを消去するためのワード
- `cursed` : 呪い系のエンチャント
- `discoverable` : エンチャントテーブルで付与することができる全てのエンチャント
- `tradeable` : 村人との取引で入手可能なエンチャント全て
- `treasure` : 宝物に付与されるエンチャント全て
- `common` : 通常レアリティのエンチャント全て
- `rare` : レアエンチャント全て
- `uncommon` : アンコモンエンチャント全て
- `very_rare` : 非常にレアなエンチャント全て
- その他、ゲーム内で利用可能な全てのエンチャントの ID

e.g. `random[all,!fortune]` : 全てのエンチャントから幸運を除いたランダムなエンチャント

e.g. `random[all,!self]` : 全てのエンチャントから現在含んでいる全てのエンチャントを除いたランダムなエンチャント

</details>

<details><summary>potion_color_rgb</summary>

# potion_color_rgb
効果 : ポーションの色を RGB で指定します。

---

- `player data : OK`

正規表現 : `type: potion_color_rgb, value: red=([0-9]+),green=([0-9]+),blue=([0-9]+)`

---

各要素には 0 ~ 255 の範囲内の整数を記載してください。

</details>

<details><summary>potion_color_name</summary>

# potion_color_name
効果 : ポーションの色を色の名前から指定します。

---

- `player data : OK`

正規表現 : `type: potion_color_name, value: [a-z]+`

---

使用可能な色の一覧
- `aqua`
- `black`
- `blue`
- `fuchsia`
- `gray`
- `green`
- `lime`
- `maroon`
- `navy`
- `olive`
- `purple`
- `silver`
- `teal`
- `white`
- `yellow`

</details>

<details><summary>texture_id</summary>

# texture_id
効果 : アイテムに適用するテクスチャの ID を指定します。

---

- `player data : OK`

正規表現 : `type: texture_id, value: [0-9]+`

</details>

<details><summary>tool_durability_percentage</summary>

# tool_durability_percentage
効果 : アイテムの残り耐久値をパーセンテージで指定します。

---

- `player data : OK`

正規表現 : `type: tool_durability_percentage, value: ([0-9]*)\\.?([0-9]+)`

---

指定した値、もしくはプレースホルダ内で計算した値が 1 未満である場合は、残り耐久値を 1 に設定します。

</details>

<details><summary>tool_durability_real</summary>

# tool_durability_real
効果 : アイテムの残り耐久値を数字で指定します。

---

- `player data : OK`

正規表現 : `type: tool_durability_real, value: [0-9]+`

---

指定した値、もしくはプレースホルダ内で計算した値が 1 未満である場合は、残り耐久値を 1 に設定します。

</details>

<details><summary>item_name</summary>

# item_name
効果 : アイテムの名前を指定します。

---

- `player data : OK`

正規表現 : `type: item_name, value: .+`

---

マインクラフト内の装飾文字を用いる場合は、`§` を使用してください。

</details>

<details><summary>attribute_modifier</summary>

# attribute_modifier
効果 : アイテムの属性を操作します。

---

- `player data : OK`

正規表現 : `type: attribute_modifier, value: attribute=([a-zA-Z_]+),op=(?i)(add_number|add_scalar|multiply_scalar_1),value=(-?[0-9]*\\.?[0-9]+)`

---

属性の詳細は [こちら (Attribute@fandom wiki)](https://minecraft.fandom.com/ja/wiki/%E5%B1%9E%E6%80%A7) をご覧ください。

</details>

<details><summary>attribute_modifier_equipment</summary>

# attribute_modifier_equipment
効果 : スロット制限付きの属性を操作します。

---

- `player data : OK`

正規表現 : `type: attribute_modifier_equipment, value: attribute=([a-zA-Z_]+),op=(?i)(add_number|add_scalar|multiply_scalar_1),value=(-?[0-9]*\\.?[0-9]+),slot=([a-zA-Z_]+)`

---

slot には
- `chest`
- `feet`
- `hand`
- `head`
- `legs`
- `off_hand`

のいずれかを指定してください。

</details>

<details><summary>item_flag</summary>

# item_flag
効果 : アイテムに付与された隠れた効果を操作します。

---

- `player data : OK`

正規表現 : `type: item_flag, value: flag=([a-zA-Z_]+),action=(?i)(clear|remove|add)`

---

- `action=clear` : アイテムが持つ全てのアイテムフラグを消します。(このとき、`flag` には適当な文字を書いてください)
- `action=remove` : 指定したアイテムフラグを消します。
- `action=add` : 指定したアイテムフラグを追加します。 

`flag` に指定可能な値一覧
- `hide_armor_trim` : ?
- `hide_attributes` : アイテムに付与された属性を隠すかどうか
- `hide_destroys` : アイテムがアドベンチャーモードでも破壊可能なブロックを隠すかどうか
- `hide_dye` : **(皮防具限定)** 染められた色を隠すかどうか
- `hide_placed_on` : アドベンチャーモードでも設置可能なブロックを隠すかどうか
- `hide_unbrealable` : 耐久値が無限であることを隠すかどうか

</details>

<details><summary>unbreakable</summary>

# unbreakable
効果 : アイテムの耐久値を無限にします。

---

- `player data : OK`

正規表現 : `type: unbreakable, value: (true|false)`

</details>

<details><summary>potion_effect, stew</summary>

# potion_effect, stew
効果 : 
- ポーションの効果を操作します。(potion_effect)
- 怪しげなシチューの効果を操作します。 (stew)

---

- `player data : OK`
- `random : OK`
  - `random under(amplifier): 0`
  - `random upper(amplifier): 255`
  - `random under(duration): 1`
  - `random upper(duration): 2147483647`

正規表現(potion_effect) : `type: potion_effect, value: ([a-zA-Z\\[\\]!,0-9=_]+)->(.+)` 
正規表現(stew) : `type: stew, value: ([a-zA-Z\\[\\]!,0-9=_]+)->(.+)`

---

potion_effect と stew は同じ構文を使用するため、このセクションでまとめて解説します。

これらの記載例は以下のようになります。

e.g. `type: potion_effect, value: slowness->speed:[amplifier=10,duration=-1]` (鈍足の効果を移動速度上昇に変更し、効果レベルを 10 に、効果時間を無限に設定します。)

e.g. `type: stew, value: random[self]->random[all,!self]:[a=10,d=200,ambient,!icon,!particles]` (現在保持しているポーション効果の中から1つランダムに選んだものを、全ての効果の中から現在保持している効果を除いたもののうちランダムに選んだ1つの効果に変更します。そして、効果レベルを 10 に、効果時間を 200 秒、 ambient, アイコン非表示, パーティクル非表示に設定します。)

上記の例より、基本的な構文が `変更前のポーション効果->変更後のポーション効果:[amplifier=効果レベル,duration=効果時間]` であることが分かると思います。また、各所にランダム構文が使用できることも。

`amplifier=` と `duration=` にはそれぞれ `a=` と `d=` がエイリアスとして存在します。また、**これら2つの要素は毎回必ず記載しなくてはいけません。**
効果時間には

`!` が否定を表し、それぞれの効果の区切りに `,` を使用するのは `エンチャントのランダム構文` と同様です。

以下はポーション効果のランダム構文で使用可能なワードとその効果の一覧です。

- `all` : 全てのポーション効果
- `self` : このワードが呼び出された時点で保持しているポーション効果全て
- `benefical` : プレイヤーに対して有益なポーション効果全て
- `harmful` : プレイヤーに対して有害なポーション効果全て
- `neutral` : プレイヤーの状態に対して特筆すべき変化をもたらさないポーション効果全て
- `ambient` : ?
- `icon` : アイコンを持つ全てのポーション効果
- `particle` : パーティクルを持つ全てのポーション効果
- `ゲーム内で有効な全てのポーション効果 ID`

---

`type: stew, value: random[all,!self]->[a=100,d=-1]` のように `変更前のポーション効果->変更後のポーション効果:[amplifier=効果レベル,duration=効果時間]`  の構文から外れたものでも有効なコンフィグとして認識されます。
これは、ポーション効果を新規に追加する際の書き方です。これらは
`追加するポーション効果->[amplifier=効果レベル,duration=効果時間]` として表されます。

</details>

<details><summary>leather_armor_color</summary>

# leather_armor_color
効果 : 皮防具の色を変更する

---

- `player data : OK`

---

追加データ
- `$CURRENT_RED$` : 現在の防具に設定されている色の赤要素の値
- `$CURRENT_GREEN$` : 現在の防具に設定されている色の緑要素の値
- `$CURRENT_BLUE$` : 現在の防具に設定されている色の青要素の値
- `$CURRENT_RGB$` : 現在の防具に設定されている色を RGB として整数に変換した値

---

正規表現 : 
- `type: leather_armor_color, value: r=([0-9]+),g=([0-9]+),b=([0-9]+)` : 色を RGB で指定する場合
- `type: leather_armor_color, value: ([a-zA-Z_]+)` : 色を色名で指定する場合
- `type: leather_armor_color, value: (?i)random` : ランダムな色を指定する場合

---

色名から指定する場合に使用可能なワードは `potion_color_name` に記載されているものと同じです。

</details>

<details><summary>book</summary>

# book
効果 : 本の内容を操作する。

---

- `player data : OK`

---

正規表現 : `type: book, value: type=(author|title|add_page|add_long|from_file|gen),element=.+`

---

## author
`element=` 以下の文字列を本の著者に設定します。

## title
`element=` 以下の文字列を本のタイトルとして設定します。

## add_page
本にページを追加し、 `element=` 以下の文字列を追加したページの内容として設定します。(1 ページに記載可能な文字数までしか記載されません。)

## add_long
本にページを追加し、 `element=` 以下の文字列を記載します。1 ページに記載可能な文字数を超過した場合は、自動的にページを追加され記載されます。  
1 冊の本に設定可能なページ数 (100) 、もしくは 1 冊の本に記載可能な文字数 (25600 文字) を超えた場合は、残りの文字は記載されません。  

## from_file
`element=` 以下の文字列をファイルのパスとして解釈し、対象のファイルが存在し内容を正しく読み取ることができた場合に、その内容を本に記載します。(文字コードは `UTF-8` に限ります。)  
対象のファイルに含まれる文字数が 25600 文字を超えた場合はその内容を記載せずに処理を終了します。  
ファイルを相対パスで指定する場合、サーバーソフトの `jar` ファイルが存在するパスを基準に探索します。  

## gen
`element=` 以下に記載された文字列を本の世代として設定します。設定可能な世代は以下の通りです。
- `original` : オリジナル
- `copy_of_original` : オリジナルのコピー
- `copy_of_copy` : コピーのコピー
- `tattered` : ボロボロ

</details>

<details><summary>head</summary>

# head
効果 : プレイヤーヘッドの情報を操作する。

---

- `player data : OK`

---

正規表現 : `type: head, value: type=(name|url),value=.+`

---

`type=name` の場合、`value=` 以下に指定した名前のプレイヤーのスキンをプレイヤーヘッドに設定します。  

`type=url` の場合、`value=` 以下に指定した文字列からスキンデータを取得しプレイヤーヘッドに設定します。このとき必要なデータは `{"textures":{"SKIN":{"url":"スキンデータが存在する URL"}}}` のように表される json データをを Base64 エンコードした文字列になります。  
[Minecraft Heads](https://minecraft-heads.com/) では `For Developers` セクションの `Value` に記載されているデータになります。  

スキンデータを初めて取得する場合にはキャッシュデータを作成する都合上、成果物がドロップされるまで 1 ~ 2 秒ほど時間がかかることがあります。  

また、作成されるプレイヤーヘッドの名前はデフォルトでは `ランダム生成されたUUID's head` となります。  
変更する場合は `item_name` を使用してください。  

</details>

<details><summary>lore_modify</summary>

# lore_modify
効果 : アイテムの説明文を操作します。

---

- `player data : OK`

---

正規表現 : `type: lore_modify, value: type=(clear|modify)(,value=type=(remove|insert),line=([0-9]+)(,value=(.+))*)?`

---

追加データ
- `$CURRENT_LINES$` : アイテムの説明文の行数
- `$CURRENT_LINE.行番号$` : アイテムの説明文の中でインデックス "行番号" に位置する内容
  - e.g. `$CURRENT_LINE.0$` : 行番号 0 の説明文

---

`type=clear` の場合、その時点でアイテムが持つ説明文を全て破棄します。  
また、`,value=` 以下のデータを必要としません。  

`type=modify` の場合、更に操作タイプを `remove`, `insert` のどちらかから選択する必要があります。  
(行番号は最も上に位置する説明文を 0 行目として、下に行くにつれ増加します。  
また、何も文字が記載されていない空の行も 1 行としてカウントされます。)  
- `remove` : `line=` 以下に指定した行を削除します。2 つ目の `value=` 以下のデータを必要としません。  
- `insert` : `line=` 以下に指定した行に `value=` 以下の文字列を挿入します。この操作が実行された時点で、アイテムの説明文の行数が 1 行増加し、挿入箇所以後のインデックスが 1 増加します。  

</details>

<details><summary>attribute_modifier_modify</summary>

# attribute_modifier_modify
効果 : アイテムに設定された attribute_modifier を操作する。

---

- `player data : OK`

---

正規表現 : `type: attribute_modifier_modify, value: type=(clear|remove|modify)(,attribute=([a-zA-Z_]+)(,value=(.+))?)?`

---

`type=clear` : アイテムに設定された全ての属性を削除します。`,attribute=` 以降のデータは不要です。

`type=remove` : `attribute=` 以下で指定した属性を削除します。`,value=` 以降のデータは不要です。

`type=modify` : `attribute=` で指定された属性を `value=` 以下のデータに書き換えます。
このとき、`value=` 以下のデータは下記の正規表現に従っている必要があります。
- `attribute=([a-zA-Z_]+),op=([a-zA-Z_]+),value=([\\d.-]+),slot=([a-zA-Z_]+)` : 装備スロットを指定する場合
- `attribute=([a-zA-Z_]+),op=([a-zA-Z_]+),value=([\\d.-]+)` : 装備スロットを指定しない場合

</details>

<details><summary>result_value_sync</summary>

# result_value_sync
効果 : この機能が呼び出された時点で成果物に付与されているコンテナデータを `$result.キー名` の形式で呼び出すことができるようにデータの同期を行う。

---

正規表現 : `type: result_value_sync`

**この機能は値を必要としません。**

</details>

<details><summary>container</summary>

# container
効果 : 成果物にコンテナデータを設定する。

---

- `player data : OK`

---

正規表現 : `type: container, value: type=(add|remove|modify),target=([a-zA-Z0-9_.%$]*)(,value=(.+))?`

---

コンテナ名は `コンテナの名前.データ型を示す文字列` の形式でなければなりません。
データ型は下記の通りです。
- `string` : 文字列データ
-  `long` : 数値(整数)データ
- `double` : 数値(小数)データ
- `anchor` : アンカー(値を持たないコンテナ)

---

`remove` : `target=` 以下で指定したコンテナデータを削除します。  

`add` : `target=` 以下に指定した名前を持ち、 `value=` 以下のデータを持ったコンテナを新規作成し、成果物に付与します。  

`modify` : `target=` 以下に指定された名前のコンテナデータの中身を `value=` 以下に指定されたデータに書き換えます。このとき、書き換えるデータの型は書き換えられる元のコンテナと一致させなければいけません。  

</details>

<details><summary>material</summary>

# material
効果 : 成果物のアイテム種別を変更する。

---

- `player data : OK`

---

 正規表現 : `type: material, value: [a-zA-Z_]+`

</details>
<details><summary>amount</summary>

# amount
効果 : 成果物の数量を変更する。

---

- `player data : OK`

---

正規表現 : `type: amount, value: [0-9]+`

</details>
<details><summary>run_command_as_console</summary>

# run_command_as_console
効果 : サーバーコンソールとしてコマンドを実行する。

---

- `player data : OK`

---

正規表現 : `type: run_command_as_console, value: .+`

---

`value: ` 以下に記載するコマンドにスラッシュは不要です。

</details>
<details><summary>run_command_as_player</summary>

# run_command_as_player
効果 : プレイヤー(アイテム作成者)としてコマンドを実行する。

---

- `player data : OK`

---

正規表現 : `type: run_command_as_player, value: .+`

---

`value: ` 以下に記載するコマンドにスラッシュは不要です。

</details>
<details><summary>item_define</summary>

# item_define

</details>
<details><summary>set_storage_item</summary>

# set_storage_item
効果 : `item_define` で定義したアイテムを現在のアイテムにセットする。(現在のアイテムがバンドルである場合に限る)

---

- `player data : OK`

---

正規表現 : `type: set_storage_item, value: name=([a-zA-Z0-9]+)(,amount=[0-9]+)?(,times=[0-9]+)?`

---

`name` には `item_define` で定義したアイテムの名前を指定してください。  
  
(オプション)`amount` : 配置するアイテムの数量を変更することができます。(1 ~ 127)  
(オプション) `times` : 指定した回数だけバンドルにアイテムを配置します。(`amount`  で数量を変更している場合には、変更された数量のまま指定された回数だけ配置されます。)  

</details>
<details><summary>can_destroy</summary>

# can_destroy
効果 : アドベンチャーモードでも破壊することができるブロックを指定する。

---

- `player data : OK`

---

正規表現 : `type: can_destroy, value: [a-zA-Z0-9_,]+`

---

`value` にはアドベンチャーモードでも破壊可能にするブロックの ID を `,` 区切りで列挙してください。

</details>
<details><summary>repair_cost</summary>

# repair_cost
効果 : アイテムの修理コストを設定する。

---

- `player data : OK`

---

正規表現 : `type: repair_cost, value: [0-9]+`

</details>
<details><summary>firework</summary>

# firework
効果 : 花火または花火の星の設定を行う。

---

- `player data : OK`
- `random : OK`
  - `random under(power): 0`
  - `random upper(power): 127`
  - `random under(rgb color): 0`
  - `random upper(rgb color): 255`

---

正規表現 : `type: firework, value: .+`

---

`value` では下記のワードによって設定を行うことができます。
- `clear` : 全てのエフェクトを削除する。
- `power` : 威力を設定する。
- `trail` : 軌跡の描写をオンにする。
- `flicker` : ちらつきをオンにする。
- `shape` : 炸裂した際に描かれる絵柄を設定する。
- `color` : 使用される色を設定する。
- `fade` : 炸裂したエフェクトが消えゆく際の色を設定する。

---

`color`, `trail`, `flicker` で否定の `!` を使用することができます。(`color` で使用した場合には、指定された色が消える。)

`trail`, `flicker` は値を必要とせず、それらを記載することによってオンオフの制御を行うことができます。

`color`, `shape`, `fade`, `power` はそれぞれ設定に値の記載が必要です。
## color, fade
`color=` または `fade=` 以下に `/` 区切りで色を追加します。名前で指定することも、 RGB の値で指定することも可能です。  
使用可能な色の名前は `potion_color_name` と同じです。  
  
RGB で指定する場合には `rgb=赤要素の値-緑要素の値-青要素の値` の形式で記載してください。  
(e.g. `rgb=100-100-100`)  

この形式で指定する際、それぞれの要素の値をランダムに指定することが可能です。  
(e.g. `rgb=random[:]-random[100-200]-random[:150]`)  

また、1つの要素の値のみランダムに決定するよう設定することも可能です。  
(e.g. `rgb=random[100:200]-100-150`)  

色設定の例  
`color=green/rgb=random[100:]-100-200/blue/white`  
フェード設定の例  
`fade=green/rgb=100-200-random[:]`  

## shape
`shape=` 以降に花火の絵柄を記載してください。
複数の絵柄を設定することはできません。

花火の絵柄一覧
- `ball` : 球形
- `ball_large` : 球形(大)
- `burst` : 炸裂
- `creeper` : クリーパーの顔
- `star` : 星

## power
`power=` 以降に花火の威力の記載をしてください。  
値の範囲は 0 ~ 127 です。  
花火の威力が大きくなるほど燃焼時間が長くなり、エリトラ飛行に使う際には長く加速することができます。  
  
e.g. `power=random[10:30]` (威力を 10 ~ 30 の範囲のランダムな値に設定)  

</details>
<details><summary>tropical_fish</summary>

# tropical_fish
効果  : 熱帯魚バケツの中身の設定を行う。

---

- `player data : OK`
- `random : OK`

---

正規表現 : `type: tropical_fish, value: body_color=([a-zA-Z\[\]!,]+),pattern=([a-zA-Z\[\]!_,]+),pattern_color=([a-zA-Z\[\]!_,]+)`

---

`body_color`, `pattern`, `pattern_color` の各要素でそれぞれのランダム構文が使用可能です。

## body_color, pattern_color
- 熱帯魚の体の色を設定します。(body_color)
- 熱帯魚の体の図柄の色を設定します。(pattern_color)

色の名前から、もしくはランダムで設定します。 RGB からの指定はできません。

使用可能な色の一覧
- `black`
- `blue`
- `brown`
- `cyan`
- `gray`
- `green`
- `light_blue`
- `light_gray`
- `lime`
- `magenta`
- `orange`
- `pink`
- `purple`
- `red`
- `white`
- `yellow`

---

指定例
- `body_color=random[all,!pink,!white]` (体の色にピンクと白以外のランダムな1色を設定する)
- `pattern_color=green` (図柄の色を緑色に設定する)

## pattern
熱帯魚の体の図柄を設定します。

体の図柄一覧
- `betty`
- `blockfish`
- `brinely`
- `clayfish`
- `dasher`
- `flopper`
- `glitter`
- `kob`
- `snooper`
- `spotty`
- `stripey`
- `sunstreak`

指定例
- `pattern=random[all,!betty,!kob]` (betty, kob 以外の全ての図柄からランダムに選んだ1つ)
- `pattern=stripey`

</details>
<details><summary>spawn_egg</summary>

# spawn_egg  
効果：成果物がスポーンエッグである場合、その中身のエンティティタイプを設定する。  

---

使用可能な特殊データ無し  

---

正規表現 : `name:([a-zA-Z_0-9]+),actions:type=[A-Z_0-9]+`  

---

このセクションではプレースホルダやプレイヤーデータは使用できません。  

## name  
name には `spawn_egg` を定義したあとに記述する `entity_define` 内で使用するための名前を設定してください。  
この名前は 1 つのレシピファイル内で固有のものにしてください。  
  
設定例  
```yaml
type: spawn_egg
value: name:self,actions:type=zombie
```  

</details>

<details><summary>entity_define</summary>

# entity_define  
効果：成果物がスポーンエッグである場合、その中身の設定を行う  
  
---

- `player data : OK`
- `random : OK (詳細は後述)`

---

正規表現 : `value: name:[a-zA-Z0-9]+,actions:.+`  

---

このセクションではスポーンエッグ、またはスポーンエッグを使用したスポナーから湧くエンティティの設定を行います。  
  
`name` に操作の対象となるエンティティの名前を、`actions` にカンマ区切りで操作を書き連ねます。  
entity_define は 1 つのレシピファイル内であれば操作した内容を保存しているので、エンティティの定義を複数行に渡って行うことができます。  
  
新規のエンティティを生み出す際は可読性の面から  
```yaml
- type: entity_define
  value: "name:名前,actions:type=エンティティの種類"
```  
のように新しい行で定義するのが望ましいのですが、  
```yaml
# self はこの行以前に定義したエンティティの名前とする
- type: entity_define
  value: "name:self,actions:selfに対する処理,->新しいエンティティの名前,type=新しいエンティティの種類"
```  
のように右向き矢印(`->`)を用いて新しいエンティティの名前を確保し、その次の操作で `type=` によって種類を指定することによっても作成することができます。  
この方法を用いて新しいエンティティを作成する際は必ず`->` で**名前を確保した直後に `type=` によって種類を指定**しなくてはいけません。  
また、右向き矢印はエンティティの新規作成だけでなく、行の途中で対象とするエンティティを変更するためにも使用することができます。  
以下の例では、行の最初にエンティティ `self` を指定していますが、途中から対象となるエンティティを `other` に変更しています。  
```yaml
- type: entity_define
  value: "name:self,actions:ai=false,->other,ai=true"
```  
  
行の途中で対象となるエンティティを変更すると、再度変更されない限りその行の終わりまでエンティティの指定が移動したままになります。  
もちろん、行の途中で何度も対象となるエンティティを変更することはできますが、これもまた可読性の面から考えるとあまり好ましい書き方とはいえません。  
  
　システムは `entity_define` に書かれた情報を解析する際、 `container` セクションの上に書かれた情報から読み取るため、操作で使用するエンティティは必ずその操作前に前の行、もしくは同じ行のそれ以前で定義されている必要があります。  
  
---

下に `actions` の一覧を示します。  
  
<details><summary>add_passenger</summary>

# add_passenger  
`random : OK(数値ランダムのみ)`  
`custom data : OK`  
- `$CURRENT_TARGET_PASSENGERS_COUNT$` : その行でターゲットとなっているエンティティに騎乗しているエンティティの総数  

---
  
正規表現 : `add_passenger=+エンティティ名`  

---
  
効果 : `+=` の後に指定された名前を持つエンティティを、このキーワードが呼び出されたときに対象となっているエンティティへ騎乗させます。 
  
騎乗させようとしたエンティティが存在しない場合、システムはエラーを出さずこの処理をスキップします。  
  
例  
```yaml
- type: entity_define
  value: "name:self,type=zombie,->other,type=skeleton,->self,add_passenger+=other"
# self (ゾンビ)の上に other (スケルトン)を騎乗させる。
```

</details>

<details><summary>item_define</summary>

# item_define  
INTERNAL FUNCTION  

</details>

<details><summary>set_armor</summary>

# set_armor
`random : OK(数値ランダムのみ)`

---

正規表現 : `(predicate=(true|false)/)?(helmet|chest|leggings|boots|mainhand|offhand)=[$.a-zA-Z0-9_-]+`

---

効果 : 対象となるエンティティに `define` セクションで定義したアイテムを装備させます。  
  
`predicate` には、この操作を実行するための条件を記載することができます。  
条件にはプレースホルダと、その内部での各種演算を利用することができます。  
条件を利用しない場合は記載を省略することができます。 
  
基本的な構文は `装着部位=装着するアイテムの定義名` のかたちを取ります。  

例  
```yaml
- type: item_define
- type: entity_define
  value: "name:self,type=zombie,set_armor=helmet=iron_helm"

define:
  - name: [iron_helm]
    base: [IRON_HELMET]
# self (ゾンビ)の頭部位に鉄のヘルメットを装備させる。
```

</details>

<details><summary>set_drop_chance</summary>

# set_drop_chance
`random : OK(数値ランダムのみ)`  

---

正規表現 : `(predicate=(true|false)/)?slot=(helmet|chest|leggings|boots|mainhand|offhand)/chance=[0-9.]+`  

---

効果 : 各部位のアイテムのドロップ率を設定する。  

`predicate` に条件(任意)、`slot` に対象となる部位、`chance` にドロップ率を記載します。  
* (`chance=[0-9.]+` でプレースホルダを利用する場合、評価後の値を正規表現にかけます。)

</details>

<details><summary>set_various_value</summary>

# set_various_value
DEPRECATED

</details>

<details><summary>ai</summary>  

# AI  
`random : OK`

---

正規表現 : `ai=(true|false)`  

---

効果 : 対象のエンティティ(モブ限定)に行動 AI を設定する(true 時のみ)。  

直接書かれた値やプレースホルダ内の計算値を代入した結果が `ai=true` になる場合、スポーンするモブに行動 AI を設定する。  
(デフォルトでは行動 AI オン)

</details>

<details><summary>set_spawner_value</summary>
</details>

<details><summary>falling_type</summary>
</details>

<details><summary>dropped_item_detail</summary>
</details>

<details><summary>splash_potion_detail</summary>
</details>

</details>
