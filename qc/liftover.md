# 異なるバージョンのゲノム座標を変換する: liftover
古い論文のデータと比較しなければならない場合などの、ゲノムのバージョンが古い座標系を使っている場合がある。そこで liftover を使い、より新しいバージョンのゲノムの座標系に変換する。

インストール
ここでは、マウスゲノムの mm8 から mm9 へ座標変換する。Linux が前提ですが、ほかでもかわりないです。変換されるファイルの形式は bed が基本だが、オプションでほかのファイルにも対応できる。bed ファイルでも browse 行や track 行があるとエラーになるので除いておく。

```
$ curl -O http://hgdownload.cse.ucsc.edu/goldenpath/mm8/liftOver/mm8ToMm9.over.chain.gz
gzip -d mm8ToMm9.over.chain.gz
```

ほかのゲノム間を比較したい場合は、golden path のダウンロードサイトの下に liftOver というディレクトリがあって、そこに変換テーブル (*.chain) があります。

変換プログラムをダウンロードしてインストールします。

```
$ curl -O http://hgwdev.cse.ucsc.edu/~kent/exe/linux/liftOver.gz
$ chmod +x ./liftOver 
$ sudo cp ./liftOver /usr/local/bin/
```

使い方
では変換してみましょう。mm8.bed が変換したい mm8 なゲノム座標のファイルで、mm9 な座標に変換後 mm9.bed に保存されます。unmapped.txt は座標変換が失敗したエントリが保存されます。 start と end が同じ座標のエントリは変換が失敗するのであらかじめ、どちらかの座標に +1 しておくこと。

```
$ liftOver mm8.bed mm8ToMm9.over.chain mm9.bed unmapped.txt
```