# Rから Refseq や UniProt-KBの情報を高速に検索する
ここでは、R package の genesearchR を使って、NCBI RefSeq と UniProt-KB の全文検索を高速に行う
方法を述べます。

## インストール
Rを起動します。
```
sudo R
```
関連するパッケージをインストールします。
```{r}
source("http://bioconductor.org/biocLite.R")
biocLite("GenomicRanges")
    
install.packages("devtools")
install.packages("roxygen2")

library(devtools)
install_github("genesearchr", "dritoshi")
```

## 使い方
```{r}
library("genesearchr")

## RefSeq
my.hs.gene <- refSeqSearch(query = "NM_001518", species = "hs")
my.mm.gene <- refSeqSearch(query = "homeobox",  species = "mm")

## UniProt-KB
my.gid.gene <- uniprotSearch(query = "GeneID:93986")
my.eco.gene <- uniprotSearch(query = "eco:b2699")

## Genome DNA Sequence
my.mm10.dna <- genomeSearch(query = "TTCATTGACAACATTGCGT", db = "mm10", k = 2)
```

### Search オプション
検索ワードの指定の仕方は以下のサイトを参考にしてください。
* [G-links](http://link.g-language.org/), UniProt-KB search
* [GGRNA](http://ggrna.dbcls.jp/en/help.html), RefSeq search
* [GGGenome](http://gggenome.dbcls.jp/en/help.html), Ultrafast sequence search

## 仕組み
genesearchR は以下のサービスを検索した結果をRに取り込みます。
- [GGRNA](http://ggrna.dbcls.jp/en/help.html)
- [GGGenome](http://gggenome.dbcls.jp/en/)
- [G-links](http://link.g-language.org/)
