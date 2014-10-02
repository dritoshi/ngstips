# 2つのTF ChIP-seq の共通結合領域を探す
異なる2つの転写因子の結合領域がどのぐらい重なっているかを検討することで、
転写因子の同じ場所で協調、あるいは競合して働いているかどうかを知ることができる。
そこで2つの ChIP-seq 解析から得られたピークの重なりを探索する方法について述べる。

```
> library(“ChIPpeakAnno”)
> oct4.df <- read.table("results/macs2/Oct4_peaks.bed", header=F)
> sox2.df <- read.table("results/macs2/Sox2_peaks.bed", header=F)
> oct4.gr <- BED2RangedData(oct4.df, header=FALSE)
> sox2.gr <- BED2RangedData(sox2.df, header=FALSE)
> oct4.sox2.overlap <- findOverlappingPeaks(oct4.gr, sox2.gr)
```

まずファイルからbed fileを入力し、そのデータフレームを BED2RangedData で RangedData というデータ構造に変換する。findOverlappingPeaks は2つのRangedData の共通した行を探す関数である。

## 2つのTF ChIP-seq の共通結合領域をカウントしベン図を描く
```
> library(“ChIPpeakAnno”)
> oct4.df <- read.table("results/macs2/Oct4_peaks.bed", header=F)
> sox2.df <- read.table("results/macs2/Sox2_peaks.bed", header=F)
> oct4.gr <- BED2RangedData(oct4.df, header=FALSE)
> sox2.gr <- BED2RangedData(sox2.df, header=FALSE)
> makeVennDiagram(RangedDataList(oct4.gr, sox2.gr), NameOfPeaks=c("Oct4", "Sox2"), totalTest=3000)
```