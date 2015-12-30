# bed file の重複領域をマージして行数を減らす

```R
library("GenomicRanges")

x <- read.table(hoge.bed)
y <- GRanges(seqname = Rle(x[,1]), ranges = IRanges(start = x[,2], end = x[,3]))
y.reduced <- reduce(y)

write.table(file = "hoge.bed", as.data.frame(y.reduced)[,1:3], quote = F, sep = "\t", row.names=F, col.names=F)
```
