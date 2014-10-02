# アダプター配列除去 FASTX_Toolkit fastx_clipper
short-read な FASTQ/A をいろいろいぢるツール群である FASTX_Toolkit に含まれるコマンドのひとつである fastx_clipper。(→トリムしたい場所が決まっている場合は fastx_trimmer を使う)。後に紹介する Galaxy に組み込み済みである。ちなみにそれぞれのコマンドの動作は Galaxy 内の解説にある図を見るのが良い。

インストール:
まず、http://hannonlab.cshl.edu/fastx_toolkit/download.html からpre-compiled binary 版を落して、/usr/local/bin などに入れる PATH を通す。本体とは別に実行に必要とされるプログラムは、libgtextutils-0.6, PerlIO::gzip, GD::Graph::bars, gnuplot version 4.2 or newer であるので、事前に入れておく。-l でリードの最小サイズ、-M で一致する最小の長さを指定する。illumina CASAVA 1.8 以降の FASTQ や Sanger FASTQ には -Q 33 を指定する。それ以前で illumina 1.3 以降の FASTQ の場合は、-Q 64 を指定すること。トリムした配列だけ出力するには -C を、trim されなかった配列を出力するには、-c を指定する。なにも指定しなければ、両方が表示される。

使いかた:
```
$ fastx_clipper -l 18 -M 12 -Q 33 -v -i input.fastq -a ACACTCTTTCCCTACACGACGCTGTTCCATCT -o output.fastq > input.fastq.log
```
# アダプター除去 TagDust
概要
理研OSCで開発されたシーケンスアダプター配列除去ソフト。入出力形式は fastq/a が使える。アダプター配列を multi fasta 形式で入力できるのが地味に便利である。なぜなら複数のアダプタ/プライマーを1回の実行で除去できるためである。これに対応しているものがなかなかない。アダプター/プライマー配列の検索には Muth–Manber algorithm (Approximate multiple string search) を利用している。FDRを調節することで除去の強さを指定できる。ライセンスはGPL3

インストール:
```
$ curl -O http://genome.gsc.riken.jp/osc/english/software/src/tagdust.tgztar zxvf tagdust.tgz
$ cd tagdust/
$ make
$ sudo make install
$ rehash
```
使いかた:
```
$ tagdust adapter.fasta input.fastq -fdr 0.05 -o output.clean.fastq -a output.artifactual.fastq
```
-fdr は除去の強さを調節する。-o にはアダプタ配列を除いたあとの fastq, -a には除かれた側の配列が記載された fastq を指定する。

リファレンス
http://bioinformatics.oxfordjournals.org/content/25/21/2839.full

アダプター配列のトリミングする Trimmomatic (Paired-end対応)
概要: paired-end に対応したアダプター配列をトリムするツール。illumina HiSeq は Paired-end の FASTQ は対となる2枚のファイルになっている。大抵の場合は、対となるペアが同じ行に記載されているが、アダプターの除去などの際にリードが除かれると、その順序が一致しなくなる。ツールによっては、IDではなく、出現順序でペアを認識している場合もある。Trimmomatic はこのようなことが起こらないよう、一対のリードに対してアダプターのトリムする、Paired-end mode が用意されている。

使いかた:
```
$ java -classpath /opt/Trimmomatic/trimmomatic-0.20.jar 
       org.usadellab.trimmomatic.TrimmomaticPE -threads 4 -phred33 -trimlog test.log
       input/fastq/ES-01_1.fastq input/fastq/ES-01_2.fastq
       input/fastq/ES-01_1.trim.fastq input/fastq/ES-01_1.unpaired.fastq
       input/fastq/ES-01_2.trim.fastq input/fastq/ES-01_2.unpaired.fastq
       ILLUMINACLIP:common_data/data/tagdust/solexa_paired_end_primers.fa:2:40:15 
       LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

adapter 配列の除去を行う。adapter 配列は、common_data/data/tagdust/solexa_paired_end_primers.fa にFASTA形式で記載する。2塩基のmismatch を許してアダプタ配列を検索する。リード先頭の quality < 3 か、N を持つ「配列」を除く。リード末端の  quality < 3 か、N を持つ「配列」を除く。4塩基ずつ配列をずらしながら quality の平均を計算し、quality が 15 以下の「リード」を除く。36塩基以下の長さの「リード」を除く。

# アダプター配列の除去 cutadapt
概要: 入出力ファイルとして fastq/a だけでなく maq や bwa 形式にも対応したアダプタ/プライマー配列除去プログラム。gzip で圧縮されたファイルでもよい。アウトプットとインプットファイルの順序を間違えると大切なデータファイルが上書きされて消えてしまうので注意が必要。アダプタが3'末端へライゲーションすることを想定している。illumina HiSeq/MiSeq/GA IIシリーズ, SOLiDシリーズの場合はそうなる。ただし、-b (--anywhere) を付けると、どの位置にあっても取り除く。アダプタ配列が部分的に存在する場合も取り除く。-e オプションでエラーレートを指定できる。Python で実装されている。Galaxy に組み込むときには、galaxy/ 以下に入れるだけ。MITライセンス。

インストール:
```
$ curl -O http://peak.telecommunity.com/dist/ez_setup.py
$ sudo easy_install cut adapt
$ rehash
```

使いかた:
```
$ cutadapt -a ACACTCTTTCCCTACACGACGCTGTTCCATCT -o output.fastq.gz input.fastq.gz
```