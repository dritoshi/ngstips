# 執筆環境について
MacOS X, homebrew, npm, [gitbook](https://github.com/GitbookIO/gitbook)

以下 bluesky にて。
## setup
```
cd /Users/itoshi/Projects/test
brew install npm
npm install gitbook -g
```

[Calibre](http://calibre-ebook.com/)をダウンロードしてインストールする。
```
sudo ln -s /Applications/calibre.app/Contents/console.app/Contents/MacOS/ebook-convert /usr/bin/ebook-convert
```

## writing
gitbook を起動する
```
gitbook serve ./repository
```

http://localhost:4000/ へアクセス。エディタを起動する。

```
subl ./repository
```
./repository 以下に md を作っていく。
ファイルを保存するとブラウザ側でコンテンツが自動的にリロードされる。
コンテンツを編集したら、SUMMARY.md を編集して、新規追加したコンテンツを
追加する。

## ファイルを作る
### PDf
```
gitbook pdf ./repository
open  ./repository/book.pdf
```
### ePub
```
gitbook epub ./repository
open  ./repository/book.epub
```

# ToDo
- rmd をコンパイルできるように plugin を書く