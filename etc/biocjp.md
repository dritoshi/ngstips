# Bioconductor 日本ミラーを利用する
日本ミラーの bioconductor.jp を選択するには、以下のように、
```
chooseBioCmirror()
```
で 6 番を選ぶだけです。 
```
options("BioC_mirror")
```
で切り替わっていることを確認できます。試しに、BrainStars package をインストールしてみると、ちゃんと bioconductor.jp からインストールされているのがわかります。