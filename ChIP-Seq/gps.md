# 結合サイトの予測 GPS
GPS (Genome Positioning System) はChIP-seqのデータからタンパク質-DNA結合サイトを1塩基解像度で予測するプログラムである。シーケンスリードの分布を混合確率分布と考えて、分布のパラメータ予測を行うことで、結合サイトの位置を予測する。バックグラウンドには実験データのほか、ポアソン分布を仮定したローカルなバックグラウンドを利用した予測も可能である。

使い方
```
$ java -Xmx80G -jar gps.jar --d Read_Distribution_default.txt 
                      --s 240000000
 --exptCTCF-GM12878 GM12878_chr1_ip.bed
 --exptCTCF-HUVEC HUVEC_chr1_ip.bed
 --ctrlCTCF-GM12878 GM12878_chr1_ctrl.bed
 --ctrlCTCF-HUVEC HUVEC_chr1_ctrl.bed
 --f BED --g hg18_chr1.info --out HumanCTCF 
```

-Xmx でメモリを指定する。mulit-condition mode では大量のメモリと計算時間を必要とする。--d には初期条件に利用する read distribution を指定する。read distribution は予測された結合サイト周辺に存在するシーケンスリードの分布に関するデータが含まれている。--s にはマッピングされるリファレンスゲノム領域のサイズを指定する。大抵はゲノムサイズ * 0.8 を指定する。---exptXXX, --ctrlXXX でそれぞれ実験データ、コントロールデータ (例えば IgGやKO, input genome のデータなど)を指定する。--f は入力ファイル形式を指定し、--out には出力ファイル名を指定する。

Guo Y, Papachristoudis G, Altshuler RC, Gerber GK, Jaakkola TS, Gifford DK, Mahony S. Discovering homotypic binding events at high spatial resolution. Bioinformatics. 2010 Dec 15;26(24):3028-34. Epub 2010 Oct 21. PubMed PMID: 20966006; PubMed Central PMCID: PMC2995123.

GPSの結果を BED に変換する: chipgsp
chipgsp を利用し、ChIP-seqからタンパク質-DNA結合サイトの予測をするGPS (Genome Positioning System) の結果を処理し、ゲノムブラウザやほかのツールで利用できる汎用的なファイル形式である BED file に変換する。

インストール
ruby 1.9.0 以上、rubygemが必要となる。
```
$ sudo gem install chipgps
```

使い方
sample ディレクトリに gps2bed.rb がある。これを利用して GPS のファイルを bed file に変換することが可能である。あるいは以下のスクリプトをテキストファイルに入力する。

```
$ ruby gps2bed.rb Demo_2_GPS_significant.txt > Demo_2_GPS_significant.bed
```

```
#!/usr/bin/env ruby
# gps2bed.rb

$LOAD_PATH.push("../lib")

require 'pp'
require 'chipgps'

#gps_out_file = ARGV.shift
gps_out_file = "Demo_2_GPS_significant.txt"
gps = Chipgps.new(gps_out_file)

for condition_id in 0..(gps.num_of_conditions-1)

  exp_name = "Demo_Day" + condition_id.to_s
  header = "track name=#{exp_name} description=\"GPS: #{exp_name}\" useScore=0"
  puts header
  gps.each_by_condition(condition_id) do |event|

    # output (bed format)
    puts [
      "chr" + event.chr,                # chr
      event.bp,                         # start
      event.bp + 1,                     # end
      [exp_name, event.chr, event.bp.to_s].join(":"),  # name
      event.fold_enrichment             # score
    ].join("\t")

  end
end
```
