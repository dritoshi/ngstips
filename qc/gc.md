# DNA配列のGC%を計算する
multi fasta 形式で保存されたDNAの GC content を計算するには、EMBOSSのgeeceeを利用すると簡単である。一般的なコンピュータでなら、マウスの全転写産物のGC contentの計算が、8秒程度で終了する。例では、ensmust.67.fa に保存されているmRNA配列のGC%を計算し、ensmust.67.gc.txt に保存する。

```
$ geecee -sequence ensmust.67.fa -outfile ensmust.67.gc.txt
```

出力の中身は以下のようになっている。

```
$ cat ensmust.67.gc.txt
#Sequence   GC content
ENSMUST00000000096  0.53
ENSMUST00000000137  0.40
ENSMUST00000000109  0.43
ENSMUST00000000127  0.53
```

長さとGC content の両方が欲しい場合は infoseq を利用したほうがよい。

```
$ infoseq -heading N -usa N -database N -type N -description N -accession N -sequence ensmust.67.fa | perl -lpe "s/ +$//; s/ +/\t/g" > ensmust.67.infoseq.txt
```