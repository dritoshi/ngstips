# RNA-seqのリードをHISAT2でゲノムへ高速にマッピングする
HISAT2はTophat2の実装をしていた人が新しく開発した、高速でメモリ使用量の少ないRNA-seq用のマッピングツール。
まず、リファレンスゲノムのHISAT用のインデックスファイルを、ダウンロードする。

```
wget ftp://ftp.ccb.jhu.edu/pub/data/hisat_indexes/mm10_hisat.tar.gz
tar zxvf mm10_hisat.tar.gz
```

*.bt2というファイルが```mm10_hisa```以下に解凍される。

自分でインデックスを作るには以下の通り。例は Ensemblからニワトリのゲノムを落してインデックスを作成している。
```
wget ftp://ftp.ensembl.org/pub/release-83/fasta/gallus_gallus/dna/Gallus_gallus.Galgal4.dna_rm.toplevel.fa.gz
gzip -d Gallus_gallus.Galgal4.dna_rm.toplevel.fa.gz
hisat2-build Gallus_gallus.Galgal4.dna_rm.toplevel.fa Gallus_gallus.Galgal4.dna_rm.toplevel
```

次に HISAT2 を用意しセットアップする。

```
wget http://www.ccb.jhu.edu/software/hisat/downloads/hisat-0.1.6-beta-Linux_x86_64.zip
unzip zxvf hisat-0.1.6-beta-Linux_x86_64.zip
```

実行する。

```
hisat2 -p 12 -q -x gallus_gallus.Galgal4.dna_rm.toplevel -U <(gzip -dc test.fastq.gz) -S test.sam
```

あとは samtools で bam にすればよい。
----
2015/02/08