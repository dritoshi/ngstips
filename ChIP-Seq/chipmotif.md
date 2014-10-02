# ピーク内に頻出するDNAモチーフ配列を発見する
タンパク質がDNAに結合するときには、ある特定のDNA配列に結合することが知られている。ChIP-seq のピーク内には、ほとんどの場合、DNAとタンパク質が結合しているはずである。その領域のなかに、ほかの領域では見られないような、特徴的なDNA配列が発見できれば、それがDNA結合配列の候補となる。

```
> library(“rGADEM”)
> library("BSgenome.Mmusculus.UCSC.mm9")
> oct4.seqs <- read.DNAStringSet(“results/oct4.peaksWithSeqs.fa”, "fasta")
> oct4.motif <- GADEM(oct4.seqs, verbose=1, genome=Mmulculs)
> nMotifs(oct4.motif)
[1] 4
> consensus(oct4.motif)
[1] "ywTTswnATGCAAAT" "nyyyyyyyyCwsyyn" "sCwGswGrnrG"     "sCCCCrCCCCm"    
> oct4.motif.pwms <- getPWM(oct4.motif)
> oct4.motif.pwms[[1]]
```