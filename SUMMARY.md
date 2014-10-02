# Summary

This is the summary of my book.

* [はじめに](README.md)
* [品質管理・前処理](qc/introduction.md)
  * [FASTQ format](qc/fastq.md)
  * [FASTQCによるシーケンス実験の品質管理](qc/fastqc.md)
  * [余分な配列の除去](qc/removeseq.md)    
  * [シーケンスクオリティが低いのリードを除く](qc/qualityscore.md)
  * [なぜアダプタやプライマー配列を除く必要があるのか](qc/rmprimer_adopter.md)
  * [ゲノムのリピート領域にマッピングされたリードを除く](qc/rmrepeat.md)
  * [FASTA/FASTQ/SAM/BAMの詳しいステータスを得る](qc/samstat.md)
  * [FASTA/BAMからランダムにリードを取り出す](qc/randomsampling.md)
  * [シーケンスリードのカバレッジを計算する](qc/coverage.md)
  * [DNA配列のGC%を計算する](qc/gc.md)
  * [異なるバージョンのゲノム座標を変換する: liftover](qc/liftover.md)
  * [異なる生物間のゲノム座標を変換する: liftover](qc/liftover_org.md)
* RNA-Seq
  * [TopHatでRNA-seq のデータをリファレンスゲノムへマッピングする](RNA-Seq/tophat.md)
  * [Bowtie2でRNA-seqデータをトランスクリプトームへマッピングする](RNA-Seq/bowtie2_transcript.md)
  * [RNA-seq データからの rRNA除去](RNA-Seq/rrna.md)
  * [遺伝子発現量(FPKM)を計算する](RNA-Seq/cuffdiff.md)
  * [発現が有意に異なる遺伝子を探す](RNA-Seq/cummeRbund.md)
* ChIP-Seq
  * [Bowtie2でChIP-Seqデータをゲノムへマッピングする](ChIP-Seq/bowtie_chipseq.md)
  * [MACS2による結合サイトの予測](ChIP-Seq/macs2.md)
  * [GPSによる結合サイトの予測](ChIP-Seq/gps.md)  
  * [結合領域を近傍の遺伝子に割り当てる](ChIP-Seq/geneasign.md)
  * [ピークがゲノムのどのような場所に存在しているのかを円グラフにする](ChIP-Seq/position.md)  
  * [2つのTF ChIP-seq の共通結合領域を探す](ChIP-Seq/twochipseq.md)
  * [Peakを近傍に持つ遺伝子によく観察されるGene Ontolgyを挙げる](ChIP-Seq/chipgo.md)
  * [ピーク内のゲノム配列をFASTA形式で得る](ChIP-Seq/peakseq.md)
  * [ピーク内に頻出するDNAモチーフ配列を発見する](ChIP-Seq/chipmotif.md)
  * [Position weight matrix から Sequence Logo を描く](ChIP-Seq/seqlog.md)
  * [Position weight matrix を既知モチーフデータベースに対して検索する](ChIP-Seq/motifsearch.md)
  * [ピーク内の配列のなかから既知のDNAモチーフ配列を発見する](ChIP-Seq/pwmsearch.md)
  * [summit からのモチーフまでの距離分布を描く](ChIP-Seq/summitdist.md)
  * [モチーフ間距離分布を描く](ChIP-Seq/motifdist.md)
  * [既知のモチーフ配列/PWMのデータを得る: JASPAR](ChIP-Seq/pwm_jaspar.md)
  * [deepToolsのセットアップ](ChIP-Seq/deeptools.md)
* Visualization  
  * [Rで遺伝子構造を描く](vis/genomegraphs.md)
  * [orenogb2 を使って、複数の bam を可視化する](vis/orenogb2.md)  
  * [Rでマゼンタ-黒-緑になるバリアフリーなカラーパレットを生成する](vis/rmagentagreen.md)
  * [R でソートできるテーブルを作成する](vis/rsorttable.md)
  * [R でアニメーションするグラフを描く](vis/ranimation.md)      
  * [R で家系図を書く](vis/familytree.md)        
* [その他](etc/introduction.md)
  * [NGSの情報の集めかた](etc/ngsinfo.md)
  * [大量の計算を高速化するには](etc/hpc.md)
  * [バイオインフォマティクス解析のためのSQLite入門](etc/sqlite.md)
  * R/Bioconductor
    * [DNA配列をRで操作する](etc/biostrings.md)
    * [Rから Refseq や UniProt-KBの情報を高速に検索する](etc/genesearchr.md)
    * [R で EnsEMBL Gene ID を NCBI Entrez Gene ID へ変換する](etc/idconv.md)
    * [Bioconductor 日本ミラーを利用する](etc/biocjp.md)
    * [Bioconductor のソースコードを検索する](etc/biocsearch.md)
    * [Vagrant を使って Bioconductor Devel の解析・開発環境をAWSに構築する](etc/biocdevelonaws.md)    
  * [Python環境の構築]    
    * [SciPyをセットアップする](etc/scipy.md)
    * [matplotlibをセットアップする](etc/matplotlib.md)    
  * FPKMとは
  * ID変換: org.Mm.eg.db
  * EMBOSS
  * Sun Grid Enging による分散計算 (SGEのオプションなど)    
  * [執筆環境](etc/writingenv.md)