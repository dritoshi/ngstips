# シーケンスクオリティが低いのリードを除く
シーケンスクオリティが低いリードを事前に除いておくことによって、リファレンスゲノムマッピングやアセンブルの精度や計算コストを軽減することができる。またそれらの解析の精度を純粋に評価するためにも、シーケンスクオリティの問題を分けるのが重要なことである。

ここでは、FASTAX toolbox の fastq_quality_filtter を利用して、fastq file からクオリティが低いリードを除く。
```
$ fastq_quality_filter -Q 33 -q 30 -p 75 -i input.fastq -o output.fastq
```
このコマンドでは、quality value が 30 以下の quality value がリード全長の 75% を下回ったときにそのリードを除く。-Q 33 は CASAVA 1.8 で作られた fastq file を扱うためのオプションである。

Quality score については以下を参照せよ。
- [FASTQ format - Wikipedia](http://en.wikipedia.org/wiki/FASTQ_format)