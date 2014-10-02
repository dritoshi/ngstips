# FastQCによるシーケンス実験の品質チェック
FastQC はシーケンス実験の品質をチェックするために便利がプロットを生成するプログラムである。サイクル(リードの塩基)あたりのシーケンスクオリティや、クオリティスコアの分布、同一シーケンスリードの重複率などを計算しプロットしてくれる。シーケンスクオリティはシーケンス実験の成否を、同一リードの重複率は、ライブラリ作成時の PCR バイアスなどを評価するために重要な指標である。重複率が高い場合は、PCRサイクルを減らすことで、ダイナミックレンジの向上を図ることができる。

インストール:
```
$ curl -O http://www.bioinformatics.bbsrc.ac.uk/projects/fastqc/fastqc_v0.10.0.zip
$ unzip fastqc_v0.10.0.zip
$ sudo cp fastqc_v0.10.0 /opt
$ sudo ln -s /opt/fastqc_v0.10.0 /opt/fastqc
$ emacs -nw ~/.zshenv
$ export PATH=$PATH:/opt/fastqc
```

# FastQCをコマンドラインから利用する
実験が多くなるとGUIから実行するのが面倒になる。FastQCはコマンドラインからの実行にも対応している。以下のコマンドは test.fastq に対して、fastqc を実行し、結果を test ディレクトリに保存するというコマンドである。-t オプションは CPU を利用する数を指定する。この場合は 8 CPUs を使って計算する。
```
$ fastqc -t 8 test.fastq -o test
```
FastQC は fastq file だけでなく、gzip (*.gz)で圧縮されたファイルに対しても実行可能である。結果はウェブブラウザで閲覧できる html ファイル、テキストファイル、html ファイルが圧縮された zip ファイルの3種類出力される。
