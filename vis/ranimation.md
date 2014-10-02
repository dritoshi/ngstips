# R でアニメーションするグラフを描く
まずは、非線形モデル y = mesor + a * cos(2*pi*(t-acrophase)/P) にN(0,0.5)のガウシアンノイズを加えたデータを用意し、これを測定されたデータと考える。そのデータに対して非線形回帰するために、Nelder-Mead法で目的関数sum( (y-yhat)^2 )を最大化する。いわゆる cosinor analysis ですね。その計算過程を各ステップごとにプロットし、animation libraryを使ってアニメ化する。

アニメーションの動きが激しくなるように、わざと初期値をおおげざにはずしてある。青が測定データで、緑が予測した回帰曲線。緑のほうの線が動いてみえるはず。収束間近ではあまり動かないのでしばらくみつめていると、ループしてまた初期値から計算しなおす。

コードは以下の通り。

```
library(animation)
t <- seq(0,48,1)
mesor <- 0
a <- 3
acrophase <- 12
P <- 24
y <- mesor + a * cos(2*pi*(t-acrophase)/P)
y.noise <- mesor + a * cos(2*pi*(t-acrophase)/P) + rnorm(length(t), mean=0, sd=1)

resid <- function(par) {
mesor <- par[1]
a <- par[2]
acrophase <- par[3]
P <- par[4]
yhat <- mesor + a * cos(2*pi*(t-acrophase)/P)

matplot(t,
matrix(c(y.noise, yhat), ncol=2),
col=c("#1E5692", "#3E9A3B"),
type="l", lty=1, pch=21, lwd=5, cex=2,
ylim=c(-5,5))
sum( (y-yhat)^2 )
}
saveMovie(optim(c(5,10,2,18), resid), interval = 0.01, movietype = "gif", outdir = getwd())
```

ようするにユーザはプロットを何枚か出力するような関数を用意するだけ。それをsaveMovie()関数に渡せば、内部でImageMagickやらSWF Toolsががんばってくれてアニメになる、という仕組みになっている。ImageMagickを MacPortsあたりで入れておく必要がある。

## Flash で出力する
SWF Toolsをインストール
```
sudo port install swftools
```
lameやらcmakeやらPythonやらがインストールされる。
前回のソース
```
saveMovie(optim(c(5,10,2,18), resid), interval = 0.01, movietype = "gif", outdir = getwd())
```
を
```
saveSWF( optim(c(5,10,2,18), resid), interval = 0.01, dev="png", outdir = getwd())
```

## アニメーションのコントロールボタンを付ける
ソースコードの最後、
```
saveMovie(optim(c(5,10,2,18), resid), interval = 0.01, movietype = "gif", outdir = getwd())
```
を
```
ani.options(nmax=200, interval=0.1, title="Cosinor Analysis", outdir = getwd())
ani.start()
optim(c(5,10,2,18), resid)
ani.stop()
```
に置きかえるだけでHTMLとアニメ用の画像、再生のコントロールボタンを制御するJavaScriptが自動生成される。
