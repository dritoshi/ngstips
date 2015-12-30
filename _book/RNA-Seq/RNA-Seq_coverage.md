# RNA-Seqのカバレッジを確認する
RNA-Seq のリードがどのぐらい遺伝子領域をカバーしているかを確認する。これにより、RNA-Seqのライブラリ作成で、どの程度までRNAが捉えられているか確認できる。
シーケンスやライブラリ作成の原理や実験上の問題により3'や5'端にカバレッジが偏る場合がある。
Whole Transcript Amplification (WTA) をする場合、オリゴdTによる逆転写を利用していれば
3'端に偏る場合がある。実験の狙い通りのカバレッジになっているかを確認することで実験の質を
判断できる。

ここでは、RSeQC の genebody_coverage.py を利用する。

## インストールする
```
$ tar zxvf RSeQC-2.4.tar.gz
$ cd RSeQC-VERSION
$ python setup.py install

$ export PYTHONPATH=/home/user/lib/python2.7/site-packages:$PYTHONPATH
$ export PATH=/home/user/bin:$PATH
```

以下のコマンドでエラーがでなければインストールに成功している。
```
$ python -c 'from qcmodule import SAM'
```

