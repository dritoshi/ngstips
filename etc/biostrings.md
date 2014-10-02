# DNA配列をRで操作する

基本的なDNA配列の操作方法や、FASTA/FASTQ file を取り込む方法を解説します。また全ゲノム配列を読み込み操作する方法についても述べます。

## 準備
```
source("http://bioconductor.org/biocLite.R")
biocLite("Biostrings")
```

### 基本的なDNA, アミノ酸配列の操作
```
library("Biostrings")

x <- DNAString("actttGtag")
is(x)
## [1] "DNAString" "XString"   "XRaw"      "XVector"   "Vector"    "Annotated"
x
##   9-letter "DNAString" instance
## seq: ACTTTGTAG
```

XString クラスを継承した DNAString オブジェクトを作成しています。このオブジェケクトにしておくと、いろいろな配列操作が可能になります。RNA, アミノ酸は、それぞれ、RNAString and AAString を使います。

```
x[1:2]
##   2-letter "DNAString" instance
## seq: AC

aa <- translate(x)
is(aa)
## [1] "AAString"  "XString"   "XRaw"      "XVector"   "Vector"    "Annotated"
aa
##   3-letter "AAString" instance
## seq: TL*

aa.revcomp <- translate(reverseComplement(x))
aa.revcomp
##   3-letter "AAString" instance
## seq: LQS

x.freq <- alphabetFrequency(x, baseOnly = TRUE)
is(x.freq)
## [1] "integer"             "numeric"             "vector"             
## [4] "data.frameRowLabels" "EnumerationValue"    "atomic"             
## [7] "vectorORfactor"

x.freq
##     A     C     G     T other 
##     2     1     2     4     0
```

## DNA配列やコード表のデータ
もちろんDNAやコードのデータも用意されています。

```
DNA_BASES
## [1] "A" "C" "G" "T"

DNA_ALPHABET
##  [1] "A" "C" "G" "T" "M" "R" "W" "S" "Y" "K" "V" "H" "D" "B" "N" "-" "+"

IUPAC_CODE_MAP
##      A      C      G      T      M      R      W      S      Y      K 
##    "A"    "C"    "G"    "T"   "AC"   "AG"   "AT"   "CG"   "CT"   "GT" 
##      V      H      D      B      N 
##  "ACG"  "ACT"  "AGT"  "CGT" "ACGT"
```

## FASTA/FASTQ ファイルの読み込み
multi FASTA 形式のテキストファイルを読み込みます。デモデータは、 

```
system.file("extdata", "someORF.fa", package="Biostrings")
```
 にあります。

```
file <- system.file("extdata", "someORF.fa", package = "Biostrings")
seqs <- read.DNAStringSet(file, "fasta")
is(seqs)
## [1] "DNAStringSet" "XStringSet"   "XRawList"     "XVectorList" 
## [5] "List"         "Vector"       "Annotated"

seqs
##   A DNAStringSet instance of length 7
##     width seq                                          names               
## [1]  5573 ACTTGTAAATATATCTTTTAT...ATCGACCTTATTGTTGATAT YAL001C TFC3 SGDI...
## [2]  5825 TTCCAAGGCCGATGAATTCGA...AAATTTTTTTCTATTCTCTT YAL002W VPS8 SGDI...
## [3]  2987 CTTCATGTCAGCCTGCACTTC...TACTCATGTAGCTGCCTCAT YAL003W EFB1 SGDI...
## [4]  3929 CACTCATATCGGGGGTCTTAC...CCCGAAACACGAAAAAGTAC YAL005C SSA1 SGDI...
## [5]  2648 AGAGAAAGAGTTTCACTTCTT...TAATTTATGTGTGAACATAG YAL007C ERP2 SGDI...
## [6]  2597 GTGTCCGGGCCTCGCAGGCGT...TTTTGGCAGAATGTACTTTT YAL008W FUN14 SGD...
## [7]  2780 CAAGATAATGTCAAAGTTAGT...AAGGAAGAAAAAAAAATCAC YAL009W SPO7 SGDI...
```

次に、デモ用の fastq file が system.file("extdata", "s_1_sequence.txt", package = "Biostrings") にありますので、これを取り込んでみます。

```
filepath <- system.file("extdata", "s_1_sequence.txt", package = "Biostrings")
fastq.geometry(filepath)  # 統計情報を表示
## [1] 256  36

fastq <- read.DNAStringSet(filepath, format = "fastq")
```

## 全ゲノムDNA配列の操作
全ゲノム配列をネットからダウンロードします。時間がかかるので、日本ミラーを設定することをお勧めします。

参考: [bioconductor のパッケージを高速にインストールする (bioconductor.jp の設定)](etc/biocsearch.md)

ここではマウスゲノム mm10 をダウンロードします。
```
source("http://bioconductor.org/biocLite.R")
biocLite("BSgenome.Mmusculus.UCSC.mm10")
```

ゲノムデータを読み込みます。

```
library("BSgenome.Mmusculus.UCSC.mm10")

Mmusculus
## Mouse genome
## | 
## | organism: Mus musculus (Mouse)
## | provider: UCSC
## | provider version: mm10
## | release date: Dec. 2011
## | release name: Genome Reference Consortium GRCm38
## | 
## | sequences (see '?seqnames'):
## |   chr1                  chr2                  chr3                
## |   chr4                  chr5                  chr6                
## |   chr7                  chr8                  chr9                
## |   chr10                 chr11                 chr12               
## |   chr13                 chr14                 chr15               
## |   chr16                 chr17                 chr18               
## |   chr19                 chrX                  chrY                
## |   chrM                  chr1_GL456210_random  chr1_GL456211_random
## |   chr1_GL456212_random  chr1_GL456213_random  chr1_GL456221_random
## |   chr4_GL456216_random  chr4_GL456350_random  chr4_JH584292_random
## |   chr4_JH584293_random  chr4_JH584294_random  chr4_JH584295_random
## |   chr5_GL456354_random  chr5_JH584296_random  chr5_JH584297_random
## |   chr5_JH584298_random  chr5_JH584299_random  chr7_GL456219_random
## |   chrX_GL456233_random  chrY_JH584300_random  chrY_JH584301_random
## |   chrY_JH584302_random  chrY_JH584303_random  chrUn_GL456239      
## |   chrUn_GL456359        chrUn_GL456360        chrUn_GL456366      
## |   chrUn_GL456367        chrUn_GL456368        chrUn_GL456370      
## |   chrUn_GL456372        chrUn_GL456378        chrUn_GL456379      
## |   chrUn_GL456381        chrUn_GL456382        chrUn_GL456383      
## |   chrUn_GL456385        chrUn_GL456387        chrUn_GL456389      
## |   chrUn_GL456390        chrUn_GL456392        chrUn_GL456393      
## |   chrUn_GL456394        chrUn_GL456396        chrUn_JH584304      
## | 
## | (use the '$' or '[[' operator to access a given sequence)
```

```
is(Mmusculus)
## [1] "BSgenome"          "GenomeDescription"

seqnames(Mmusculus)
##  [1] "chr1"                 "chr2"                 "chr3"                
##  [4] "chr4"                 "chr5"                 "chr6"                
##  [7] "chr7"                 "chr8"                 "chr9"                
## [10] "chr10"                "chr11"                "chr12"               
## [13] "chr13"                "chr14"                "chr15"               
## [16] "chr16"                "chr17"                "chr18"               
## [19] "chr19"                "chrX"                 "chrY"                
## [22] "chrM"                 "chr1_GL456210_random" "chr1_GL456211_random"
## [25] "chr1_GL456212_random" "chr1_GL456213_random" "chr1_GL456221_random"
## [28] "chr4_GL456216_random" "chr4_GL456350_random" "chr4_JH584292_random"
## [31] "chr4_JH584293_random" "chr4_JH584294_random" "chr4_JH584295_random"
## [34] "chr5_GL456354_random" "chr5_JH584296_random" "chr5_JH584297_random"
## [37] "chr5_JH584298_random" "chr5_JH584299_random" "chr7_GL456219_random"
## [40] "chrX_GL456233_random" "chrY_JH584300_random" "chrY_JH584301_random"
## [43] "chrY_JH584302_random" "chrY_JH584303_random" "chrUn_GL456239"      
## [46] "chrUn_GL456359"       "chrUn_GL456360"       "chrUn_GL456366"      
## [49] "chrUn_GL456367"       "chrUn_GL456368"       "chrUn_GL456370"      
## [52] "chrUn_GL456372"       "chrUn_GL456378"       "chrUn_GL456379"      
## [55] "chrUn_GL456381"       "chrUn_GL456382"       "chrUn_GL456383"      
## [58] "chrUn_GL456385"       "chrUn_GL456387"       "chrUn_GL456389"      
## [61] "chrUn_GL456390"       "chrUn_GL456392"       "chrUn_GL456393"      
## [64] "chrUn_GL456394"       "chrUn_GL456396"       "chrUn_JH584304"

## chr19 の情報を取り出す
mm10.chr19 <- Mmusculus$chr19
is(mm10.chr19)
## [1] "MaskedDNAString" "MaskedXString"
```

4種類のゲノム配列マスクデータが含まれている。AGAPS は、いわゆるUCSCゲノムブラウザの Gap Track の部分が N でマスクされていることを示す。コンティグの間をNで埋めている。つまりアセンブリギャップ。AMB は、コンティグ内で読むことができなかった塩基を IUPAC ambiguity letter でマスクしている。RM は RepeatMasker でマスクしている。TRF は tandem repeat をマスクしたもの。

データを読み込んだときのデフォルトは、AGAPSとAMBだけが有効になっているゲノム配列が得られる。例えば、RM を有効にするには、

```
active(masks(mm10.chr19))["RM"] <- TRUE
## Warning: 置き換えるべき項目数が，置き換える数の倍数ではありませんでした
countPattern("AAGAACAT", mm10.chr19)  # pattern をカウント
## [1] 2352
```

## パターンマッチング
```
mm10.chr19.match <- matchPattern("AAGAACAT", mm10.chr19, max.mismatch = 1)
is(mm10.chr19.match)
## [1] "XStringViews" "Views"        "List"         "Vector"      
## [5] "Annotated"

mm10.chr19.match
##   Views on a 61431566-letter DNAString subject
## subject: NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN...NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
## views:
##            start      end width
##     [1]  3000189  3000196     8 [AAGAAAAT]
##     [2]  3005005  3005012     8 [AAGAACCT]
##     [3]  3005091  3005098     8 [AAGAACCT]
##     [4]  3006251  3006258     8 [AAGATCAT]
##     [5]  3009789  3009796     8 [AACAACAT]
##     [6]  3009818  3009825     8 [AAGAATAT]
##     [7]  3010972  3010979     8 [AAGAACAG]
##     [8]  3011103  3011110     8 [CAGAACAT]
##     [9]  3011138  3011145     8 [AAGAAGAT]
##     ...      ...      ...   ... ...
## [49007] 61323881 61323888     8 [AAGAAAAT]
## [49008] 61324346 61324353     8 [AAAAACAT]
## [49009] 61324511 61324518     8 [AAGAATAT]
## [49010] 61324707 61324714     8 [AAGAACAT]
## [49011] 61325474 61325481     8 [AAGATCAT]
## [49012] 61325642 61325649     8 [AAGAGCAT]
## [49013] 61327589 61327596     8 [AACAACAT]
## [49014] 61329266 61329273     8 [ATGAACAT]
## [49015] 61330773 61330780     8 [AAGGACAT]
```

## 配列を取り出す
```
mm10.chr19.dna <- as(mm10.chr19, "XStringViews")
is(mm10.chr19.dna)
## [1] "XStringViews" "Views"        "List"         "Vector"      
## [5] "Annotated"

mm10.chr19.dna
##   Views on a 61431566-letter DNAString subject
## subject: NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN...NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
## views:
##       start      end    width
## [1] 3000001  6685768  3685768 [GATCATACTCCTCATGCTGG...GCAAGCTTCCGGGTCAGAGA]
## [2] 6685869  6688105     2237 [GCACACACACATACACACAC...ACCTGTCTGTGTGGTACTGG]
## [3] 6813616  6829746    16131 [GCCTGCTCTCCTAGGAGTAC...GAGTGGGAGCCGCTGGCAAA]
## [4] 6829847 61331566 54501720 [GATCAGAGTGGGAGCCGCTG...AGGGTTAGGGTTAGGGTTAG]

start(mm10.chr19.dna[1])  # コンティグのスタートの座標
## [1] 3000001

mm10.chr19.dna[[1]][1:100]  # 3000001 bp から 100 bp 取り出す
##   100-letter "DNAString" instance
## seq: GATCATACTCCTCATGCTGGACATTCTGGTTCCT...TCAACTTTCCCTCTTCCTATGCCAGTTATGCAT
```


