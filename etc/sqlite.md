# バイオインフォマティクス解析のためのSQLite入門
SQLite3 はリレーショナルデータベース管理システムのひとつ。サーバクライアントモデルではなく、データベースがファイルになっています。ライセンスがパブリックドメインであることもあり、組み込みDBとしてよく使われます。iPhone の位置情報などのデータを格納しているのも SQLite3 です。ウェブアプリケーションのフレームワークである、Ruby on Rails では標準の組み込みDBとして使われています。また中規模のデータベースならMySQLなどと比較しても遜色ない速度で利用できます。

バイオ業界でも、マイクロアレイやNGSデータを格納する際によく利用されており、Bioconductor の annotation package に含まれるデータも SQLite3 のデータベースになっています。

## インストール
Mac OS X には標準でインストールされており、設定なしでターミナルから利用できます。Linuxでもほとんどのディストリビューションでパッケージマネジメントシステムでインストール可能です。Windows は知りません。

## 使いかた
SQLite3 が正常にインストールされていれば、以下のようにして対話型のクライアントを起動することができます。
```sh
sqlite3
```

データベースを作成するには (ここでは expressions.sqlite という名前のデータベースを作成する)
```sh
sqlite3 expressions.sqlite
```
とする。

テーブルを作りスキーマを確認する。スキーマとはテーブルの定義のこと。
```sql
sqlite> create table transcripts(id varchar(10), sample varchar(10), fpkm real);
sqlite> .schema
CREATE TABLE transcripts(id varchar(10), sample varchar(10), fpkm real);
sqlite> 
```

データを格納する。
```sql
sqlite> insert into transcripts values("Nanog",  "ESC", 1000);
sqlite> insert into transcripts values("Pou5f1", "ESC", 1500);
```

データを検索する
```sql
sqlite> select * from transcripts;
Nanog|ESC|1000.0
Pou5f1|ESC|1500.0
sqlite> select * from transcripts where id = "Nanog";
Nanog|ESC|1000.0
sqlite> select fpkm from transcripts where id = "Nanog";
1000.0
```

ヘルプと終了は以下の通り。
```sh
sqlite> .help
sqlite> .exit
```

ローカルディレクトリに expressions.sqlite が作成されています。次回利用したい場合は、
```
sqlite3 expressions.sqlite
```
とすればよいだけです。

### 様々な bulk 処理
バイオインフォマティクスでは、データを機器や公的データベースから入手し、一度にデータベースに登録するのが主です。ここではその方法について解説します。

まず、スキーマをテキストファイルに書いておいて一気にテーブルを作成します。スキーマファイル expressions.schema を作成します。これはテキストエディタで作ります。中身は以下の通りです。

RNA-seq で gene と isoform レベルの遺伝子発現のデータを格納することを想定しています。テーブル名は英語の複数形で作りましょう。
```sql
DROP TABLE IF EXISTS genes;
CREATE TABLE genes (
  id      varchar(10),
  sample  varchar(10),
  fpkm    varchar(10)
);

DROP TABLE IF EXISTS isoforms;
CREATE TABLE isoforms (
  id      varchar(10),
  sample  varchar(10),
  fpkm    varchar(10)
);
```

このスキーマにあわせてデータベースをテーブルを作成します。
```sh
sqlite3 expressions.sqlite < expressions.schema
```

確認してみましょう。データベースに接続します。
```
sqlite3 expressions.sqlite                     
```
スキーマを確認します。
```sql
sqlite> .schema
CREATE TABLE genes (
  id      varchar(10),
  sample  varchar(10),
  fpkm    varchar(10));
CREATE TABLE isoforms (
  id      varchar(10),
  sample  varchar(10),
  fpkm    varchar(10)
);
```

次に、カンマ区切りテキストファイル(CSV)からテーブルを作成してみます。まずデータが含まれるCSVは以下のようになっています。

expressions.isoforms.csv 
```
Nanog,ESC,1000
Pou5f1,ESC,1500
```

expressions.genes.csv 
```
Nanog,ESC,1200
Pou5f1,ESC,1700
```

この2つのファイルをそれぞれ、isoforms, genes というテープルに取り込みます。

```sql
sqlite3 expressions.sqlite
sqlite> .separator ,
sqlite> .import expressions.isoforms.csv isoforms
sqlite> .import expressions.genes.csv    genes
```

試しに検索してみます。
```
sqlite> select fpkm from transcripts where id = "Nanog";
```