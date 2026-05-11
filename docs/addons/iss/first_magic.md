# 最初の魔法を作成する

## はじめに
このセクションでは実際に動作するコードを紐解きながら、どのようにして魔法を作成しているかを解説していきます  
## 実際のコード
```js
// StartupScripts
StartupEvents.registry('irons_spellbooks:spells', event => {
	event.create('super_heal')                          // 作成したい魔法のid(文字列表記/小文字英数字&ハイフン、アンダーアーマーのみ使用可)
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
このコードをStartupScriptsに置いて、マインクラフトを起動してください。そうすると、spell.kubejs.super_heal のスクロールがあると思います。名前がおかしいのは大丈夫です、後々直します。  
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
今回の魔法では、spellLevel × 2の体力を回復しています。詳しく見ていきましょう。  

まず、`const entity = ctx.entity`で発動者を取得します。  
次に、`if (!entity) return`で、**entityがnullの時を除外**します。nullチェックは大事。  
最後に、`entity.heal(ctx.getSpellLevel() * 2)`でentityを回復します。`heal(number)`が回復部分で、引数に回復量を入れます。今回の場合、`getSpellLevel()`で取得した魔法レベルに2を掛けた値を入れています。  
この部分は自由に書けます。例えば、`entity.potionEffects.add("minecraft:resistance", 200, ctx.getSpellLevel())`用に書けば、耐性を魔法レベル分**10秒間**付与します。`potionEffects.add()`の効果時間は**Tick**です。  

## setUniqueInfo()
魔法の説明をツールチップに書きます。  
Componentを含んだArrayを返す必要があります。
**casterがnullの場合を考慮してください。**魔法書に魔法を入れるときにnullになります。  
`Component.blue(Text.translate("spell.kubejs.super_heal.tooltip"))`このように記載することで、翻訳キーを入れることもできます。  

## 仕上げ
最後に、魔法名と魔法の詳細な説明文を書く必要があります。以下の場所にそれぞれのファイルを作成して、コピペして貼り付けてください。
`./assets/kubejs/lang/ja_jp.json`  
```json
{
    "spell.kubejs.super_heal": "すごいヒール",
	"spell.kubejs.super_heal.info": "とても強い回復魔法"
}
```  
`./assets/kubejs/lang/en_us.json`  
```json
{
    "spell.kubejs.super_heal": "Super Heal",
	"spell.kubejs.super_heal": "Totemo Tuyoi Heal Magic"
}
```   
ここで疑問点を持った方がいらっしゃるでしょう、「なんで英語名も書かないといけないんだ？」と。  
それは、マイクラのデフォルト言語が英語だからです。もしあなたのスクリプトをフランス人が使うとして、日本語だけの場合最初の翻訳キーが表示されてしまいます。ちゃんと英語名も書きましょう。  
さて、あとは魔法のアイコンだけです。アイコン画像は`./assets/kubejs/textures/gui/spell_icons/super_heal.png`に置いてください。**アイテムやブロックのように自由にパスを指定できないため注意してください。**  
さて、ここまででうまくいっていれば君だけのオリジナル魔法が完成します！おめでとう！！！