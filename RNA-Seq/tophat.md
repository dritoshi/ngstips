# TopHatでRNA-seq のデータをリファレンスゲノムへマッピングする
RNA-seq で読まれるシーケンスリードはスプライシング後の配列である。そのためリファレンスゲノムにマッピングするときには、リードがスプライスされる部分(スプライスジャンクション)を跨ぐことが多々ある。そのためジャンクションを考慮したマッピングが求められる。一般に spliced mapping と呼ばれる。
TopHat は良く使われる spliced alignment に対応したマッピングソフトである。ここでは、TopHat を用いて、fastq file に保存されたリードをリファレンスゲノムにマップする方法について述べる。

```
$ tophat -p 8 -r 100 -o output_dir bowtie2_indexes/mm9 input_1.fastq input_2.fastq
```

Paired end の場合は2つの fastq file を順に指定する。また、paired-end の場合は、-r を使う。これは、シーケンスされたリード間にどのぐらいの長さDNAがあるかを base pair で指定する。これはシーケンスライブラリを作成するときに、生物学的実験によって設定されているものである。そのため実験担当者に事前に問い合わせておくべきである。具体的には、Agilent社のBioAnalyzerでDNA断片の分布と平均が得られている。またシーケンスを何bpしたのかも聞いておく(これは fastq をみればわかるが)。

-r に指定する数値を計算する方法を述べる。例えば、典型的なライブラリでは、300 bp のDNA断片からライブラリを作成するが、これを 50 bp の Paired-end でシーケンスすると、その間にあるDNAの長さは 200 bp である。よって、-r 200 と指定する。

indexs/mm9 でははリファレンスゲノム配列が検索しやすようインデックス化されたファイルを指定している。これは [bowtie](http://bowtie-bio.sourceforge.net/)に含まれる bowtie-build で行う。TopHat はマッピングの計算の部分には、Bowtie を利用している。-p には CPU core の数を指定することで、その数だけ並列計算が可能になる。

TopHat のデフォルトでは、ひとつのリードが複数箇所にマップされる場合は、mapping socre の高い順に 20 箇所まで許される設定になっている。best なスコアで、一箇所 (unique) にマッピングされるようにするには、-g 1 と指定する。一般的にはマルチヒットを許したほうが発現定量の値が良くなることが知られている。