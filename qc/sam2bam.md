# samファイルを bam ファイルに変換する
samファイルはシーケンスリードがゲノムのどの位置にあるかを記述するファイル形式であり、様々なシーケンス解析のツールで利用されている。samファイルはテキスト形式のデータであり、ファイルサイズが大きくなったり、ソフトウェアから効率的に個々のデータにアクセスすることが難しい。そこでbam形式というバイナリ形式のファイルを作成することで、ファイルサイズの縮小、アクセスの高速化、リードのゲノム座標でのソートによるアクセスの効率化が期待できる。
シーケンスリードのゲノムマッピングツールであるbowtie2 もsamファイルを出力する。これをbam形式変換するには、samtoolsを利用することで簡単にできる。

```
$ samtools view -bS hoge.sam > hoge.bam  # sam を bam に変換
$ samtools sort hoge.bam hoge.sort              # bam をソート
$ samtools index hoge.sort.bam                    # index を作成する hoge.sort.bam.bai に保存
```

この方法だとsort 前後のファイルが残る。ディスクに余裕がない場合は、以下のようにすると、sort したファイルを上書き保存できる。
```
$ samtools sort hoge.bam hoge
```
