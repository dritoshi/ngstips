# モチーフ間距離分布を描く
2つのDNAモチーフ間の距離の分布をプロットする。この距離が近い場合、分布が単峰性になる。ここから2つの転写因子がヘテロダイマーなどのように近くで働いているという分子メカニムズが想像できる。距離が離れている場合は二峰性の分布になる。これは、一次配列としての距離は遠くとも、ゲノムDNAが折り曲がることにより、クロマチン構造上、近くに存在する場合や、ほかのコファクターとなるタンパク質を介して、タンパク質間相互作用がある場合などがが考えられる。

まず2つの転写因子の結合サイトを計算する。
```
library("ChIPpeakAnno")
library("MotIV")

load("results/3rd/03motifdbSearch.rdat")
oct4.sox2.overlappingPeaks.seq <- read.DNAStringSet("results/3rd/oct4.sox2.overlappingPeaksWithSeqs.fa", "fasta")

oct4.motif.pwms <- getPWM(oct4.motif)
oct4.motif.search.fwd <- lapply(
  oct4.sox2.overlappingPeaks.seq,
  function(x) {
    matchPWM(oct4.motif.pwms$m1, x)
  }
)
oct4.motif.search.rev <- lapply(
  oct4.sox2.overlappingPeaks.seq,
  function(x) {
    matchPWM(reverseComplement(oct4.motif.pwms$m1), x)
  }
)

sox2.motif.pwms <- getPWM(sox2.motif)
sox2.motif.search.fwd <- lapply(
  oct4.sox2.overlappingPeaks.seq,
  function(x) {
    matchPWM(sox2.motif.pwms$m1, x)
  }
)
sox2.motif.search.rev <- lapply(
  oct4.sox2.overlappingPeaks.seq,
  function(x) {
    matchPWM(reverseComplement(sox2.motif.pwms$m1), x)
  }
)

save(list = ls(), file = "results/3rd/04motifsearchInOverlappingPeakseq.rdat")
```

この2つの転写因子の距離の関係をプロットする。
```
library("ChIPpeakAnno")
library("MotIV")

load("results/3rd/04motifsearchInOverlappingPeakseq.rdat")

## calc. distance between motif event and summit
num.peaks <- length(oct4.motif.search.fwd)

num <- 0
motif.dist <- 0
for (peak in 1:num.peaks) {
  if (! identical(start(oct4.motif.search.fwd[[peak]]), integer(0))) {
    if (! identical(start(sox2.motif.search.fwd[[peak]]), integer(0))) {
      oct4 <- start(oct4.motif.search.fwd[[peak]])
      sox2 <- start(sox2.motif.search.fwd[[peak]])
      for (i in 1:length(oct4)) {
        for (j in 1:length(sox2)) {
          num <- num + 1 
          motif.dist[num] <- oct4[i] - sox2[j]
        }  
      }
    }      
  }    
}

save(list = ls(), file = "results/3rd/05twoMotifDistanceDistribution.rdat")

pdf("results/3rd/05twoMotifDistanceDistribution.pdf")
plot(density(motif.dist), main="Oct4-Sox2 / Motif distance distribution")
dev.off()
```