# PokeAPI
https://pokeapi.co/  

ポケモンに関する情報を取得できるAPI。  
RESTfulAPIとして利用可能です。  
OSSとして開発、メンテナンスがされています。

## 使用方法
APIを叩いてJSONを取得。  
認証は特に不要。アクセス回数の制限も無いが、攻撃とみなされるとアクセス元のIPがブロックされるらしい。

### ポケモンの基本情報を取得
`https://pokeapi.co/api/v2/pokemon/1` もしくは `https://pokeapi.co/api/v2/pokemon/bulbasaur`
|parameter|備考|
|--|--|
|abilities|特性の一覧|
|base_experience|倒すとゲットできる経験値|
|forms|見た目のみが違う姿が存在する場合のリスト|
|game_indices|登場するバージョンとバージョン内での図鑑番号|
|height|身長|
|held_items|野生で遭遇時に持っている可能性のあるアイテム|
|id|ポケモンのID、メガシンカやリージョンフォルムは通常とは別のIDが振られている|
|is_default|ポケモンがデフォルトの姿であるか、メガシンカやリージョンフォルムではfalseが返ってくる|
|location_area_encounters|遭遇できる場所|
|moves|覚えることができる技の一覧|
|name|名前|
|order|全国図鑑上の表示順序|
|past_types|過去の世代で持っていた特性一覧|
|past_types|過去の世代で持っていたタイプ一覧|
|spacies|対象ポケモンの種族に関するリンク|
|sprites|対象ポケモンの画像へのアクセスURL|
|stats|対象ポケモンの種族値|
|types|対象ポケモンのタイプ|
|weight|ポケモンの重さ|

### ポケモンの生息地、図鑑情報、たまごグループなどを取得
`https://pokeapi.co/api/v2/pokemon-species/1` もしくは `https://pokeapi.co/api/v2/pokemon-species/bulbasaur`
|parameter|備考|
|--|--|
|base_happiness|ゲットした際の初期のなつき度|
|capture_rate|捕まえやすさ|
|color|ポケモン図鑑検索時に指定するポケモンの色|
|egg_groups|ポケモンが属するたまごグループのリスト|
|evolution_chain|ポケモンが属している進化の流れ|
|evolves_from_species|対象ポケモンの進化前のポケモン|
|flavor_text_entries|ポケモンの図鑑説明|
|form_descriptions|?|
|forms_switchable|ポケモンに別形態が存在しているか|
|gender_rate|ポケモンのオス、メスの比率|
|genera|〇〇ポケモンというフレーズ。フシギダネならたねポケモン。各国の言語に対応|
|generation|ポケモンが追加された世代情報|
|growth_rate|レベルの上がりやすさ|
|habitat|生息地|
|has_gender_differences|性別によって見た目が変わるか|
|hatch_counter|たまごの孵化するまでの歩数|
|id|ポケモンのID|
|is_baby|赤ちゃんポケモン？の場合`true`|
|is_legendary|伝説のポケモンの場合`true`|
|is_mythical|幻のポケモンの場合`true`|
|name|ポケモンの名前、英語表記|
|names|ポケモンの名前、各国版。日本語も`"name": "ja"`で含まれている|
|order|全国図鑑上の表示順序|
|pal_park_encounters|パルパークで遭遇できる場所|
|pokedex_numbers|登場するバージョンや地方のリスト|
|shape|ポケモンの形状による区別|
|varieties|メガシンカなど別形態のリスト|

### わざ情報を取得
`https://pokeapi.co/api/v2/move/1` もしくは `https://pokeapi.co/api/v2/move/pound`
|parameter|備考|
|--|--|
|accuracy|命中率|
|contest_combos|コンテスト用のコンボ技の詳細|
|contest_effect|コンテストでわざを使用した場合の効果|
|contest_type|コンテストでわざを使用した場合のアピールタイプ|
|damage_class|物理技か特殊技かの区分|
|effect_chance|追加効果の発生確率|
|effect_changes|過去作での追加効果|
|effect_entries|追加効果の説明、各国版|
|flavor_text_entries|わざの説明、各国版。日本語も`"name": "ja"`で含まれている|
|generation|わざが登場した世代|
|id|わざのID|
|learned_by_pokemon|わざを覚えるポケモン一覧。リストはポケモンの英語名で一覧化されている。|
|machines|わざを覚えられるわざマシン|
|meta|わざのメタデータ|
|name|わざの名前、英語表記|
|names|わざの名前、各国版。日本語も`"name": "ja"`で含まれている|
|past_values|過去作から変更のあった値|
|power|わざの威力|
|pp|わざのPP|
|priority|わざの優先度|
|stat_changes|わざが影響するステータスの一覧|
|super_contest_effect|スーパーコンテストで技を使用した場合の効果|
|target|わざを受けるポケモンの種類、通常であれば選択したポケモン。じしんとかの技であればその場のポケモン全体など。|
|type|わざのタイプ|

### 特性情報を取得
`https://pokeapi.co/api/v2/ability/1` もしくは `https://pokeapi.co/api/v2/ability/`
|parameter|備考|
|--|--|
|effect_changes|特性が登場するバージョン情報|
|effect_entries|特性の効果|
|flavor_text_entries|特性の説明、各国版。日本語も`"name": "ja"`で含まれている|
|generation|特性が登場したバージョン情報|
|id|特性のID|
|is_main_series|メインのポケモンシリーズで登場した特性か|
|name|特性の名前、英語表記|
|names|特性の名前、各国版。日本語も`"name": "ja"`で含まれている|
|pokemon|特性を持つ可能性のあるポケモン一覧|

## JSONのサンプル
* [特性](./sample-json/ability(あくしゅう).json)
* [わざ](./sample-json/move(はたく).json)
* [ポケモン基本情報](./sample-json/pokemon(フシギダネ).json)
* [ポケモン情報その他](./sample-json/pokemon-species(フシギダネ).json)

