# orenogb2 を使って、複数の bam を可視化する
orenogb2 を利用すると、複数の bam と遺伝子名や座標を指定すると、周辺の遺伝子と bam から計算したカバレッジがグラフ化できます。

## インストール
```
$ sudo R
```

パッケージをインストールする

```
source("http://bioconductor.org/biocLite.R")
biocLite(c("ggbio", "GenomicRanges", "GenomicAlignments", "devtools")
biocLite(c("Mus.musculus", "BSgenome.Mmusculus.UCSC.mm10"))
biocLite(c("Homo.sapiens", "BSgenome.Hsapiens.UCSC.hg19"))

library(devtools)
install_github("dritoshi/orenogb2")
```

## 座標を指定して描画する
```
q01.bam <- system.file("extdata", "Quartz_01.Pou5f1.bam", package = "orenogb2")
q02.bam <- system.file("extdata", "Quartz_02.Pou5f1.bam", package = "orenogb2")

genome.ver <- 'mm10'
zoom.power <- 1
quartz.bam.files <- c(q01.bam, q02.bam)
    
gb.quartz <- orenogb2$new(
  genome.ver = genome.ver,
  zoom.power = zoom.power,
  bam.files  = quartz.bam.files
)
    
# Coordination
gb.quartz$chr      <- "chr17"
gb.quartz$start.bp <- 35492880
gb.quartz$end.bp   <- 35526079
gb.quartz$plotgb()
```
![demo](Pou5f1.png)

### Semantic Zoom を利用する
ズームの倍率によって描画スタイルが自動的に変更されることを Semantic Zoom と呼びます。例えば拡大していくとDNA配列が表示されたり、ズームアウトするとATGCを色に対応させた
ヒートマップを表示したりすることです。orenogb2 は semantic zoom に対応しています。

```
genome.ver <- 'mm10'
zoom.power <- 1/200
quartz.bam.files <- c(q01.bam, q02.bam)
    
gb.quartz <- orenogb2$new(
  genome.ver = genome.ver,
  zoom.power = zoom.power,
  bam.files  = quartz.bam.files
)
    
# Coordination
gb.quartz$chr      <- "chr17"
gb.quartz$start.bp <- 35492880
gb.quartz$end.bp   <- 35526079
gb.quartz$plotgb()
```

![demo](Pou5f1.zoom.png)

### Gene Symbol を指定して描画する
座標を指定しなくても遺伝子シンボルを指定すれば描画することができます。
```
s01.bam <- system.file("extdata", "Smart-Seq2_01.CDK2.bam", package = "orenogb2")
s02.bam <- system.file("extdata", "Smart-Seq2_02.CDK2.bam", package = "orenogb2")  

genome.ver <- 'hg19'
zoom.power <- 1
smart2.bam.files <- c(s01.bam, s02.bam)
    
gb.smart2 <- orenogb2$new(
  genome.ver = genome.ver,
  zoom.power = zoom.power,
  bam.files  = smart2.bam.files
)
gb.smart2$getPositionBySymbol('CDK2')
gb.smart2$plotgb()
```

![demo](CDK2.png)
