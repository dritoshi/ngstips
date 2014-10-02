# R でソートできるテーブルを作成する
テーブルのタイトル行をクリックするとソートできるテーブルを R で作ります。

まず、インストールします。
```
sudo R
install.packages('SortableHTMLTables')
```

テーブルを作ってみましょう。デモデータ iris をソートできるHTMLテーブルにしてみます。

```
library('SortableHTMLTables')
data(iris)
sortable.html.table(iris, "./index.html", 'iris')
```

iris というディレクトリのなかに、index.html と必要なファイルが生成されているはずです。ブラウザで開くと以下のような感じになっているはず。

中身をみてみます。jquery の jquery.tablesorter.js を利用していますね。

```
<html lang="en">
<head>
<title>Untitled Page</title>
<link rel="stylesheet" href="style.css" type="text/css" />
<script type="text/javascript" src="jquery-1.4.2.js"></script>
<script type="text/javascript" src="jquery.tablesorter.js"></script>
<script type="text/javascript">
$(document).ready(function() {
  $("#myTable").tablesorter();
  }
);
</script>
</head>
<body>
<table id="myTable" class="tablesorter" border="0" cellspacing="1" cellpadding="0">
<thead>
<tr>
<th>Sepal.Length</th>
<th>Sepal.Width</th>
<th>Petal.Length</th>
<th>Petal.Width</th>
<th>Species</th>
</tr>
</thead>
```
