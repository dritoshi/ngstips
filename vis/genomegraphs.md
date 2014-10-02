# Rで遺伝子構造を描く
超並列DNAシーケンサーの登場で、遺伝子構造など、ゲノム上のイベントやオブジェクトのデータを大量に得ることができるようになってきました。データ解析にとって可視化は重要ですが、ゲノム上で起きているイベントなので、ゲノム上に配置して可視化したい場面がよくでてきます。しかし、さまざまなゲノム上のオブジェクトをゲノム座標から画像座標に変換して、絵を書くのは意外と面倒な作業です。

例えば遺伝子構造の絵を書こうとします。これは、遺伝子構造をどのように入手するか、遺伝子構造をどのように描くか、の2つの問題に分けることができます。ここでは、遺伝子構造のデータは、R + Bioconductor の biomaRt パッケージを使って、Ensembl Biomart からダウンロードすることで解決します。遺伝子構造をどのように描くかについては、GenomeGraphs パッケージを利用します。

まずはインストールします。

```
$ sudo R
> source("http://www.bioconductor.org/biocLite.R")
> biocLite("biomaRt")
> biocLite("GenomeGraphs")
```

ここでは、Ensembl biomart からヒトの SMN1 という splicing 異常によって疾患になる例が知られている遺伝子名(Gene symbol)の遺伝子構造を描きます。

まず、SMN1 という名前から、Ensembl Gene ID と染色体位置などを入手します。

```
library(GenomeGraphs)
library(biomaRt)

gene.symbol <- "SMN1"
png.file <- paste(gene.symbol, ".png", sep = "")

## construct an object of Human Ensembl Biomart
human <- useMart(biomart = "ensembl", dataset = "hsapiens_gene_ensembl")

gene <- getBM(
     attributes = c('hgnc_symbol', 'ensembl_gene_id', 'chromosome_name'),
     filters = 'hgnc_symbol',
     values  = gene.symbol,
     mart    = human
)

ensgene.id <- gene[,2]
chr.num    <- as.character(gene[,3])
```

次に、Ensembl Gene ID から Ensembl Gene の構造情報(染色体名とその exon/intron の位置) を入手します。また、これに対応する Ensembl Transcript のリストとその構造も入手します。

```
## Get annotation of Ensembl Gene
gene <- makeGene(id = ensgene.id, type = "ensembl_gene_id", biomart = human)
transcript <- makeTranscript(
     id      = ensgene.id,
     type    = "ensembl_gene_id",
     biomart = human,
     dp      = DisplayPars(plotId = TRUE, cex = 0.5)
)
```

遺伝子構造を描画するまえに、染色体の大雑把な位置を把握するための ideogram を描くため、Ideogram object を作ります。黒と白の縞々のアレですね。遺伝子の位置には赤い透明なボックスを描きます。

```
## Create an ideogram object for the entire chromosome
ideog <- new("Ideogram", chromosome = chr.num)

## Create a highlight of the gene position on the ideogram
## using "absolute coordinates"
highlight.posi.on.ideo <- makeRectangleOverlay(
     0.60, 0.65,
     region = c(0.75, 0.82),
     coords = "absolute",
     dp = DisplayPars(alpha = .2, fill = "red")
)
```

最後に、遺伝子、転写産物、ideogram をまとめて PNG file に出力します。

```
## Create the plot
png(png.file)
gdPlot(
     list(
       makeTitle(gene.symbol),
       "Chr"         = ideog,
       "Gene"        = gene,
       "Transcripts" = transcript
     ),
     overlays = list(highlight.posi.on.ideo)
)
dev.off()
```
完成した図は以下のようになります。