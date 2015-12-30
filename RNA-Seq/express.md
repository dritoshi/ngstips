# eXpress で RNA-seq データから発現量を計算する
トランスクリプートムを bowtie1 index にする。bowtie を利用して、その index file に fastq をマッピングし、eXpress で FPKM を計算する。

## bowtie1 を入れる
```
$ wget "http://downloads.sourceforge.net/project/bowtie-bio/bowtie/1.1.1/bowtie-1.1.1-linux-x86_64.zip
$ unzip bowtie-1.1.1-linux-x86_64.zip
$ sudo mv bowtie-1.1.1-linux-x86_64 /usr/local
```

## トランスクリプトームをダウンロード
```
$ wget http://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/refMrna.fa.gz
$ gzip -d refMrna.fa.gz
```

## インデックスを作成する
```
$ /usr/local/bowtie-1.1.1-linux-x86_64 --offrate 1 refMrna.fa refMrna
```
refMrna.ebwt のようなファイルがいくつかできる。

## eXpress をセットアップ
```
$ wget http://bio.math.berkeley.edu/eXpress/downloads/express-1.5.1/express-1.5.1-linux_x86_64.tgz
$ tar zxvf express-1.5.1-linux_x86_64.tgz
$ sudo mv express-1.5.1-linux_x86_64 /usr/local
```

## トランスクリプトームへリードマッピングする
```
$ bowtie -aS -X 800 --offrate 1 refMrna -1 reads_1.fastq -2 reads_2.fastq | samtools view -Sb - > hits.bam
```

## eXpress を実行する
```
$ /usr/local/express-1.5.1-linux_x86_64/express refMrna.fa hits.bam -o hit
```
hit ディレクトリに結果を保存する。