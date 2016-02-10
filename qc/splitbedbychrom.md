# bed file を染色体ごとのファイルへ分割する

```
$ cat hoge.bed
chr1 100 199
chr1 200 299
:
chr2 100 199
chr2 200 299
:
```

のようなファイルを染色体ごとに分割する。

```
awk '{print $0 >> $1".bed"}' hoge.bed
```

これは awk というツールを使っています。
パターン {アクション} は特定のパターンにマッチしたら、括弧内のアクションを実行するための記法です。
パターンが記載されていない場合は、すべての行を対象に処理をおこないます。
print $0 を表示しています。$0 は1行を意味します。$1は入力を空白文字で区切ったときの行番号です。

chr1.bed, chr2.bed,... がディレクトリに保存されます。