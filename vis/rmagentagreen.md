# Rでマゼンタ-黒-緑になるバリアフリーなカラーパレットを生成する
バリアフリーなヒートマップを描くために、マゼンタ-黒-緑のカラーパレットが使いたかったのですが、R でどうやって良いのかわからなかったので自作しました。簡単な方法を知っている人がいれば教えてください。

## colorRampPalette を使う
```
m2b2g <- colorRampPalette(c("#EC008C", "black", "green"))
pie(rep(1,50), col=m2b2g(50))
```

## gplots を使う
```
library(gplots)
m2b2g <- colorpanel(50, low="magenta", mid="black", high="green")
pie(rep(1,50), col=m2b2g)
```
