# 最初の魔法を作成する

## はじめに
このセクションでは実際に動作するコードを紐解きながら、どのようにして魔法を作成しているかを解説していきます  
## 実際のコード
```js
// StartupScripts
StartupEvents.registry('irons_spellbooks:spells', event => {
	event.create('heal')                          // 作成したい魔法のid(文字列表記/小文字英数字&ハイフン、アンダーアーマーのみ使用可)
		.setCastTime(20)                           // 発動時間(tick表記)
		.setCooldownSeconds(30)                    // クールタイム(秒表記)
        .setBaseManaCost(50)                       // 基本的なマナコスト(整数表記)
		.setManaCostPerLevel(30)                   // レベルごとに上がるマナコスト(整数表記)
		.setCastType('long')                       // 魔法発動の種類。詳細は以下に記載。
		.setSchool('irons_spellbooks:blood')       // 魔法のタイプ選択(id表記)
		.setMinRarity('rare')                      // 魔法の最小レアリティ。詳細は以下に記載。
		.setMaxLevel(5)                            // 最大レベル(整数表記)
		.setStartSound('item.honey_bottle.drink')  // 魔法を発動し始めた際に鳴らすサウンド。ロング魔法で使用(id表記)
		.setFinishSound('item.honey_bottle.drink') // 魔法が発動し終わった際に鳴らすサウンド(id表記)
		.onCast(ctx => {
			const entity = ctx.entity
			if (!entity) return
			entity.heal(ctx.getSpellLevel() * 2)
		})                         // 魔法を発動した際に行うこと。主にここでコードを記載する。詳細は以下に記載。
		.setAllowLooting(true)                     // 魔法がルートから出現するかどうか。trueで出現、falseでなし。
		.needsLearning(false)                      // 魔法を学ぶ必要があるかどうか。エルドリッチ魔法専用。
		.canBeCraftedBy(player => true)            // プレイヤーが魔法をクラフトできるかどうか。一定条件下においてクラフト可もできる。
		.setUniqueInfo((spellLevel, caster) => {   // 魔法についての説明をツールチップに記載する。Arrayを返す(詳細は下記)
			return [
	 			Component.darkGreen(`回復量: ${spellLevel * 2}`)
			]
		})

})
```  
これは自分自身を魔法レベル × 2分回復する魔法です。  
このコードをStartupScriptsに置いて、マインクラフトを起動してください。そうすると、spell.kubejs.heal のスクロールがあると思います。  
では、それぞれのメゾットを詳しく見ていきましょう。  

## setCastType()
魔法の発動タイプを選択します。  
以下のいずれかを選択する必要があります  
- `'continuous'`: 継続して発動します。  
- `'long'`: 長い詠唱を行います。  
- `'instant'`: 即座に発動します。  
- `'none'`: よくわかりません。後でソースコード読みます。  

## setMinRarity()
魔法の最小レアリティを設定します。  
以下のいずれかを選択する必要があります。  
- `"common"`: コモン  
- `"uncommon"`: アンコモン  
- `"rare"`: レア  
- `"epic"`: エピック  
- `"legendary"`: レジェンダリー  

## onCast()
魔法が発動された際の動作を決定します。  
だいたいここでどのような魔法になるかが決定します。  
`ctx`はコンテキストオブジェクトです。この中にentityやspellLevelなどがあります。  
- `ctx.entity` = 発動者  
- `ctx.spellLevel` = 発動した魔法のレベル  
- `ctx.level` = 発動したlevel  
…他にも絶対ありますが、私が知らないので覚えたら書きます。許して。  

## setUniqueInfo()
魔法の説明をツールチップに書きます。  
Componentを含んだArrayを返す必要があります。
**casterがnullの場合を考慮してください。**魔法書に魔法を入れるときにnullになります。  
`Component.blue(Text.translate("spell.riiimc.magic_info"))`このように記載することで、翻訳キーを入れることもできます。  

## 仕上げ
さて、これで君だけのオリジナル魔法が動きます！しかし、名前が"spell.kubejs.spell_name"になっていることにお気づきでしょう。Modやデータパック、リソパの制作に携わったことのある方はご存知かもしれませんが、これは翻訳キーが書いてあります。そうです、最後に魔法の日本語名を決める必要があります。  
`assets`フォルダを開き、さらに`kubejs`フォルダを開いてください。中にtexturesフォルダがあると思いますがそれは気にせず、新たに`lang`フォルダを作成してください。そしてその中に`ja_jp.json`と`en_us.json`をそれぞれ作成してください。画像は後で貼ります。  
`ja_jp.json`と`en_us.json`をそれぞれ開いてください。そして、以下のようにかいてください。  
`assets/kubejs/lang/ja_jp.json`  
```json
{
    "spell.kubejs.your_spell_id": "日本語での魔法名"
}
```  
`assets/kubejs/lang/en_us.json`  
```json
{
    "spell.kubejs.your_spell_id": "英語での魔法名"
}
```   
なぜ英語が必要か、それは英語がデフォルト言語だからです。もし使用者がフランス語でマイクラを起動した際、日本語だけだと翻訳キーが表示されてしまいます。しかし、英語も記載しておけば翻訳キーではなく**英語での魔法名**が表示されるのです。  
さて、あとは魔法のアイコンだけです。`assets/kubejs/`を開き、先ほどスルーした`textures`を開いてください。おそらく`block`と`item`があります。今回はこの2つは関係ないので`gui`フォルダを作成し、その中にさらに`spell_icons`フォルダを作成します。そして、その中に**魔法のidと全く同じpng形式の画像ファイル**を置いてください。  
さて、ここまででうまくいっていれば君だけのオリジナル魔法が完成します！おめでとう！！！