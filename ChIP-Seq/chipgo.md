# Peakを近傍に持つ遺伝子によく観察されるGene Ontolgyを挙げる
```
> library(“ChIPpeakAnno”)
> library(org.Mm.eg.db)
> 
> oct4.df <- read.table("results/macs2/Oct4_peaks.bed", header=F)
> sox2.df <- read.table("results/macs2/Sox2_peaks.bed", header=F)
> oct4.gr <- BED2RangedData(oct4.df, header=FALSE)
> sox2.gr <- BED2RangedData(sox2.df, header=FALSE)
> 
> oct4.go <- getEnrichedGO(oct4.anno, orgAnn="org.Mm.eg.db", maxP=0.01, multiAdj=TRUE, minGOterm=10, multiAdjMethod="BH")
> oct4.bp.goterm <- unique(oct4.go$bp[order(oct4.go$bp[,10]),c(2,10)])
> oct4.bp.goterm[1:10,]
```

getEnrichedGo の引数 multiAdjMethod で指定できる多重検定補正の手法は以下の通りである。

- Bonferroni: Bonferroni single-step adjusted p-values for strong control of the FWER.
- Holm: Holm (1979) step-down adjusted p-values for strong control of the FWER.
- Hochberg: Hochberg (1988) step-up adjusted p-values for strong control of the FWER (for raw (unadjusted) p-values satisfying the Simes inequality).
- SidakSS: Sidak single-step adjusted p-values for strong control of the FWER (for positive orthant dependent test statistics).
- SidakSD: Sidak step-down adjusted p-values for strong control of the FWER (for positive orthant dependent test statistics).
- BH: adjusted p-values for the Benjamini & Hochberg (1995) step-up FDR controlling procedure (independent and positive regression dependent test statistics).
- BY: adjusted p-values for the Benjamini & Yekutieli (2001) step-up FDR controlling procedure (general dependency structures)
