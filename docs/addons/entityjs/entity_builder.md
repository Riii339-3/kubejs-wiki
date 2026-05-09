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

また、これら以外にも`"entityjs:nonliving"`と`'entityjs:living'`があります。これらはテンプレートに従っていない、完全新規のエンティティです。ただし、これらを扱うには高度な知識を求められます。  
では、それぞれのエンティティについて見てみましょう。  