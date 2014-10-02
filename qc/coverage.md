# シーケンスリードのカバレッジ(被覆率)を計算する

インストール
```
$ curl -O https://bedtools.googlecode.com/files/BEDTools.v2.17.0.tar.gz
$ tar zxvf BEDTools.v2.17.0.tar.gz
$ cd bedtools-2.17.0/
$ make
$ cd ..
$ sudo cp -a bedtools-2.17.0 /opt
$ cd /opt
$ sudo ln -s bedtools-2.17.0  bedtools
```

設定
```
$ emacs -nw ~/.zshenv
export PATH=$PATH:/opt/bedtools/bin
$ source ~/.zshenv
```

ここでは、bed ファイル (Pou5f1.bed) に記述された座標にマップされたリードのカバレッジを bam ファイルから(hoge.bam) 計算する。
```
$ coverageBed -d -abam hoge.bam -b Pou5f1.bed > Pou5f1.TruRNA-Seq01.coverage.bed
```

## bam を wig に変換する
wig file はカバレッジのデータを保存するためによく使われるファイル形式である。ここでは、bam から wig を計算する方法を述べる。
```
bam2wig.py -t 1000000000 -i hoge.bam -o hoge.wig -s mm10.info
```

## wig を bigwig へ変換する
wig 形式はテキスト形式になっているが、このままではファイルサイズが巨大になる。そこで、バイナリ形式である、bigwig へ変換する。
```
$ wigToBigWig hoge.wig mm10.info hoge.bw
```