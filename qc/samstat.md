# FASTA/FASTQ/SAM/BAMの詳しいステータスを得る
[samstat](http://samstat.sourceforge.net/) はFASTA/FASTQ/SAM/BAMから、nucleotide composition, length distribution, base quality distribution, mapping statistics, mismatch, insertion and deletion error profiles, di-nucleotide and 10-mer over-representation を計算するツールである。

インストール:
```
$ wget http://downloads.sourceforge.net/project/samstat/samstat.tgz
$ tar zxvf samstat.tgz
$ cd samstat/src
$ make
$ sudo cp ./samstat /opt/local/bin/
$ rehash
```

実行方法:
```
$ samstat test.bam
```

結果は test.bam.html というファイルにhtml5形式で出力される。このファイルを閲覧するには通常のWebブラウザを使う。表示されるグラフは JavaScript だけで生成されているので、画像ファイルが出力されるわけではない。
