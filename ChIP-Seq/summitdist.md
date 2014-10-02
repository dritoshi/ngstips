# summit からのモチーフまでの距離分布を描く
summit からモチーフまでの距離を計算し、そのヒストグラムを描く。もしChIPに関係のないモチーフ配列の場合、その分布は一様分布になるはずである。しかし、実際に結合に関与しているモチーフ配列ならばその分布はsubmitにその平均値を持つ正規分布になると考えあれる。ここではMACS2の summit のデータからこのモチーフ分布を描く。以下のコードでは、Peak を上流下流に延長した場合のことは考慮されていないことに注意。

```
## Load summit data
oct4.sm.df <- read.table("results/macs2/Oct4_summits.bed", header=F)
sox2.sm.df <- read.table("results/macs2/Sox2_summits.bed", header=F)
oct4.sm.gr <- BED2RangedData(oct4.sm.df, header=FALSE)
sox2.sm.gr <- BED2RangedData(sox2.sm.df, header=FALSE)

## calc. distance between motif event and summit
num.peaks <- length(oct4.motif.search.fwd)
oct4.motif.summit.dist <- rep(0, num.peaks)

summit.on.peak <- start(oct4.sm.gr) - start(oct4.gr)

num.peaks <- length(oct4.motif.search.fwd)
num <- 0
oct4.motif.summit.dist <- 0
for (peak in 1:num.peaks) {
  if (! identical(start(oct4.motif.search.fwd[[peak]]), integer(0))) {
    dists <- start(oct4.motif.search.fwd[[peak]]) - summit.on.peak[peak]

    if (length(dists) == 1) {
      num <- num + 1
      oct4.motif.summit.dist[num] <- dists
    } else if (length(dists) > 1) {
      for(i in 1:length(dists)) {
        num <- num + 1
        oct4.motif.summit.dist[num] <- dists[i]
      }
    }
  }    
}

pdf("results/oct4.motifdist.pdf")
plot(density(oct4.motif.summit.dist), main="Motif distribution (m1)")
dev.off()
```