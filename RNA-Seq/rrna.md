# RNA-seq データからの rRNA除去
ここではリファレンスゲノムへのマッピングした bam file から、bedtools のなかの intersectBed を利用して、 rRNA 配列を除く方法を述べる。-abam は入力ファイル形式を、-b は除くべきゲノム領域を記載した bed file を指定する。ここでは rRNA 遺伝子が存在するゲノム領域の bed fileである。-v は -b で指定した領域に含まれないものだけ出力するという意味である。 grep -v を思い浮べると良いだろう。

RNA-seq は mRNAを転写量を定量するのが目的である。しかし、RNA-seq のデータには rRNA 由来の配列が含まれる。そもそも抽出した Total RNA には 99 %程度の rRNA が含まれてしまう。これを分子生物学実験的に取り除いてはいるが、そもそもrRNAが大量にあるため、除去効率が高くても多少の rRNA が残る。rRNAが残ってしまうと、その分シーケンスリードが割かれてしまうため、mRNAの転写量や構造予測をするために必要な有効シーケンスリード数に届かない遺伝子が出てくる可能性がある。よってrRNAが含まれてしまった量を定量することはサンプルの評価に重要である。

使い方
```
$ intersectBed -abam input.bam -b rRNA.ucsc.mm9.bed -v > output.bam
```