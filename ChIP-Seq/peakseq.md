# ピーク内のゲノム配列をFASTA形式で得る
ピーク内のDNA配列を得ることができれば、ChIP-qPCRのような確認実験用の PCR primer を設計したり、配列に頻出するDNA motifを計算機で発見することに利用できる。ここでは、ChIPpeakAnno を利用した方法を述べる。

```
> library(“ChIPpeakAnno”)
> library("BSgenome.Mmusculus.UCSC.mm9")
> oct4.peaksWithSeqs <- getAllPeakSequence(oct4.gr, upstream = 20, downstream = 20, genome = Mmusculus)
> write2FASTA(oct4.peaksWithSeqs, file="resutls/oct4.peaksWithSeqs.fa")
```

ローカルディレクトリに oct4.peaksWithSeqs.fa が保存されている。