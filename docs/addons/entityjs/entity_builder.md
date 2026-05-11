# EntityTypeの作成

## はじめに
著者も理解できてないです。助けて。  
KubeJS Iron's Spellsとの連携については、[こちら](./iss_compat.md)をご覧ください。

## EntityTypeの種類
EntityJSが対応している種類は以下の通りです。  
- `'entityjs:mob'`: PathfinderMob、つまり一般的なモブです。  
- `"entityjs:animal"`: AnimalMob、つまり動物です。  
- `"entityjs:tamable"`: TamableMob、つまり手懐けられるモブです。  
- `""entityjs:watercreature""`: WaterMob、つまり海中にいるモブです。  
- `"entityjs:arrow"`: ArrowEntity、つまり矢です。  
- `"entityjs:projectile"`: ProjectileEntity、つまり飛翔体エンティティです。  
- `"entityjs:geckolib_projectile"`: GeckoLibのモデルを持つ飛翔体エンティティです。  

また、上記以外にも`"entityjs:nonliving"`と`'entityjs:living'`があります。これらはテンプレートに従っていない、完全新規のエンティティタイプです。ただし、扱うには高度な知識を求められます。  
このセクションでは、PathfinderMobを作成していきます。  

## 実際のコード
```js
TODO("待っててね")
```  
では、それぞれのメゾットについて解説していきます。  