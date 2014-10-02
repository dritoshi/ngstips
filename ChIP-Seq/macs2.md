# MACS2を利用してChIP-Seqのピークを探す
MACS2、はおそらくもっとも良く使われている peak caller である MACS のベータバージョンである。

インストール:
Python 2.6 以上、Cython 0.14.1以上, numpy が必要。

numpy のインストール:
```
$ wget "http://downloads.sourceforge.net/project/numpy/NumPy/1.6.1/numpy-1.6.1.tar.gz"
$ tar zxvf numpy-1.6.1.tar.gz 
$ cd numpy-1.6.1
$ python setup.py build --fcompiler=gnu
$ sudo python setup.py install
```

MACS2のインストール:
```
$ w3m http://github.com/downloads/taoliu/MACS/MACS-2.0.9-1.tar.gz
$ tar zxvf MACS-2.0.9.tar.gz
$ cd MACS-2.0.9/
$ sudo python setup.py install --prefix=/opt
$ export PYTHONPATH=/opt/lib/python2.6/site-packages/:$PYTHONPATH
```

実行例:
```
$ macs2 callpeak -t results/bowtie/Sox2/Sox2.bowtie.sort.rmRepeat.bam -c results/bowtie/GFP/GFP.bowtie.sort.rmRepeat.bam -f BAM -g mm -n Sox2 -B -q 0.01 --shiftsize 100
```
-t はターゲットとするTFの bam ファイル、-c はネガティブコントロールとなるサンプルの bam ファイル、-f は入力ファイルの形式を指定、-g はゲノムサイズを指定、-n は任意の実験名、-q は q-value の閾値を指定する。-B は extended fragment pileup, local lambda and score tracks を graphBed file に保存するオプションである。-g のゲノムサイズ指定には数値による指定と生物名を指定することができ、ヒトゲノムは、hs, マウスゲノムは mm, 線虫ゲノムが ce, ハエゲノムが、dm である。

参考URL: https://github.com/taoliu/MACS

## MACS2のPeak model をプロットする
MACS2はワトソン/クリック鎖にそれぞれマップされたタグをシフトさせてピークを判定する。これはショートリードのシーケンサーはインサートDNAの端を読むため、実際の結合サイトから位置がずれることを考慮するためである。実際にどのように shift させたのかは、Rのコードとして出力される。MACS2の実行の後に、{転写因子名}_model.r のようなファイル名のテキストファイルが出力される。これを実行すると、peak model のプロットが作成される。

```
R -q -f Sox2_model.r
```

この例では、Sox2_model.pdf に保存される。赤とがそれぞれ forward, reverse 鎖にマップされたタグの分布で、black の線がシフトさせたタグの分布になる。
