# 執筆環境について
環境は次のとおり

* MacOS X
* homebrew
* npm
* [gitbook](https://github.com/GitbookIO/gitbook)

## 環境のセットアップ
```
cd [DIR]
brew install npm
npm install gitbook -g
```

[Calibre](http://calibre-ebook.com/)をダウンロードしてインストールする。
```
sudo ln -s /Applications/calibre.app/Contents/console.app/Contents/MacOS/ebook-convert /usr/bin/ebook-convert
```

## 執筆
最初だけ。
```
mkdir [dir]
gitbook init
```

gitbook を起動する。
```
gitbook serve [dir]
```

http://localhost:4000/ へアクセス。エディタを起動する。

```
subl [dir]
```
```dir``` 以下に md を作っていく。
ファイルを保存するとブラウザ側でコンテンツが自動的にリロードされる。

コンテンツを編集したら、___SUMMARY.md を編集___して、新規追加したコンテンツを
追加する。これをやらないと、buildしたときにHTMLへ変換されないので注意!

## 書籍ファイルの作成
### PDF
```
gitbook pdf [dir]
open [dir]/book.pdf
```
### ePub
```
gitbook epub [dir]
open [dir]/book.epub
```

# ToDo
- rmd をコンパイルできるように plugin を書く