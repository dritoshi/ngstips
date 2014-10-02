# ランダムにリードを取り出す
リード数に応じてシーケンスの深さ (depth) が決まる。この depth は RNA-seq, ChIP-seq などのタグシーケンシングではその精度に関わる。具体的には、depth が深いほど、検出できる遺伝子の数やRNAの発現量やタンパク質-DNA結合量ダイナミックレンジが向上する。自分のサンプルや生物がどのぐらいのリード数が必要であるかを知ることは、正確でかつ、無駄なコストをかけずに実験するには重要なことである。そこで、リードをランダムに除いた系列、例えば、0.1, 1, 10, 100M リードなどを用意して解析することで、検出遺伝子やダイナミックレンジの評価を行うことが重要である。ランダムに配列を除くことを、ダウンサンプリング、あるいは、ランダムサンプリングと呼び、取り出されたリード群を、ダウンサンプル、サブサンプルなどと呼ぶ。

## BAM file から一定の確率でランダムにリードを取り出す
ここでは、Picard (http://picard.sourceforge.net/) の DownsampleSam.jar を使って、bam file からランダムに配列を除く方法を述べる。すでにマッピング済みの配列からファイルを除くにはことを想定する。
```
$ java -jar /opt/picard-tools/DownsampleSam.jar INPUT=input.bam OUTPUT=output.bam PROBABILITY=0.2
```
ここでは、全体のリードのうち 20% をランダムに取り出している。注意したいのは、tophat2, bowtie2 などで、ひとつのリードを複数の箇所にマッピングされた(いわゆる multi hits)ような bam file を利用する場合は、割合ではなくリードの絶対数を指定しなければならない。それについては次の項を参照すること。

## BAM file から一定の数のリードをランダムに取り出す
DwonsampleSam.jar はリードを取り出す確率を指定することはできるが、取り出したい数を直接指定することはできない。そこで、bamtools を使って数を直接指定してリードを取り出す。

まず、cmake をインストールする。
```
$ cd ~/src
$ curl -O http://www.cmake.org/files/v2.8/cmake-2.8.8.tar.gz
$ tar zxvf cmake-2.8.8.tar.gz
$ cd cmake-2.8.8/
$ ./configure --prefix=/opt/local
$ gmake
$ sudo gmake install
```

次に bamtools をインストールする。
```
$ cd ~/src
$ git clone git://github.com/pezmaster31/bamtools.git
$ cd bamtools/
$ mkdir build
$ cd build/
$ cmake -DCMAKE_INSTALL_PREFIX=/opt/local ..
$ make
$ sudo make install
```

bamtools を実行して、次のようなエラーがでる場合は、手動でファイルをコピーする。
```
bamtools: error while loading shared libraries:
```
```
$ sudo cp libbamtools-utils.so* /opt/local/lib/bamtools/
$ sudo cp libjsoncpp.so* /opt/local/lib/bamtools/
```

10Mのリードを取り出すには以下のようにする。
```
$ bamtools random -in input.bam -out output..bam -n 100
```
bamtools でリード数を確認してみる。
```
$ bamtools count -in output.bam 
100
```
samtools で数えることができる。
```
$ samtools flagstat output.bam
100 + 0 in total (QC-passed reads + QC-failed reads)
0 + 0 duplicates
100 + 0 mapped (100.00%:nan%)
:
:
```

## Fastq/a file からランダムにリードを取り出す
次にマッピング前のデータから配列を除くことを想定して、fastq file からのリードのサンプリングについて述べる。ここでは seqtk を利用する。まず seqtk をインストールする。

```
$ git clone git://github.com/lh3/seqtk.git
$ cd seqtk
$ make
$ sudo cp seqtk /opt/local/bin
```

次に sample オプションを利用して、ランダムサンプリングする。

```
$ seqtk sample -s1 input.fastq 30000000 > output.30M.fastq
```

ここでは、30M のリードをランダムに取り出している。-s オプションは乱数を生成するときのシードとなる数値である。シードの数はどのような数でもよい。

シードが同じであれば生成される乱数列はいつも等しくなる。これを利用すれば、Paired-end ファイル、つまり2つの fastq からランダムにペアを選ぶことができる。input_1.fastq と input_2.fastq の pair 同士が同じ並び順になっている場合は、以下のようにする。

```
$ seqtk sample -s1 input_1.fastq 30000000 > output_1.30M.fastq
$ seqtk sample -s1 input_2.fastq 30000000 > output_1.30M.fastq
```

注意: seqtk は samtools にも付属している (samtools/misc/seqtk)がこれには、sample オプションがない。混同しないよう注意が必要である。