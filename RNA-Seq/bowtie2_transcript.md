# Bowtie2でRNA-seqデータをトランスクリプトームへマッピングする
RNA-Seq のデータをトランスクリプトームへマッピングする方法について述べる。RNA-Seqデータはスプライシング後のRNAをシーケンスするのが一般的である。そのためスプライシングされるイントロンとエクソンの境界にあるシーケンスリードは、ゲノムにはマッピングされない。そのためTopHatなどスプライスドアラインメントに対応したアルゴリズムを利用する必要がある。もうひとつの方法は、トランスクリプトームに対して、シーケンスリードをそのままマッピングすれば、この問題は考慮の必要がない。このデータは、トランスクリプトごとにマッピングされたリードを操作できるようになるので、トランスクリプトごとに、カウントやカバレッジを計算する際などには便利である。ただし、トランスクリプトームデータベースに含まれないトランスクリプトを考慮できない点に注意すること。

まずトランスクリプトームのデータベース (bowtie2 index) を作成する。このステップは一度やればよい。ここではマウスゲノム mm10 の座標系の RefSeq を利用する。

```
$ curl -O http://hgdownload.cse.ucsc.edu/goldenPath/mm10/bigZips/mrna.fa.gz
$ gzip -d mrna.fa.gz
$ bowtie2-build mrna.fa mrna
```

次にbowtie2でシーケンスリードをトランスクリプトームにマッピングする。

```
$ bowtie2 -p 8 -x mrna -U hoge.fastq -S hoge.bowtie2.sam
```

参考までにファイルサイズの目安を書いておく。RNA-Seq (4 multiplex/lane, HiSeq 2000, v3)から得られた約11GBの sam ファイルを bam に変換すると、約2.6GBになる。これをソートすると、2.1GB程度のファイルになる。インデックスファイルは、15M程度である。

結果が sam で出力されるため、その後の解析や可視化のために、bam へ変換する必要がある。


## 中間ファイルを残さずにマッピングして sort された bam と bam index を作成する
```
$ bowtie -p 16 --chunkmbs 512 --verbose -m 1 \
  -1 hoge_1.fastq -2 -2 hoge_2.fastq mrna -S 2> bowtie.log \
 | samtools view -bSu - | samtools sort -m 10G - hoge.sort; \
 samtools index hoge.sort.bam
```
