# R で EnsEMBL Gene ID を NCBI Entrez Gene ID へ変換する

org.Mm.eg.db というアノテーションパッケージを利用する。Bioconductor のアノテーションパッケージは、AnnDbBimap オブジェクトになっている。これはID変換のテーブルを 1:1 で扱うオブジェクトで、これを操作してIDを変換する。

いや別に Mm じゃなくてもいいし、EnsEMBL じゃなくても共通する話です、はい。

まず、この AnnDbBimap をベクターにしてから変換してみる。
```
library("org.Mm.eg.db")                                                        
my.ensmusg <- c("ENSMUSG00000000275", "ENSMUSG00000002844", "ENSMUSG00000003868", "ENSMUSG00000091821")
ensmusg <- unlist(as.list(org.Mm.egENSEMBL2EG))
# convert EnsMUSG -> Entrez Gene ID                                                                      
my.eg <- ensmusg[my.ensmusg] 
my.eg
ENSMUSG00000000275 ENSMUSG00000002844 ENSMUSG00000003868               <NA> 
          "217069"            "11544"            "20174"                 NA 
```
最後のIDが複数の Entrez ID に対応しているので、変換がうまくいっていない。これは、単に操作ミスによって対応のつくIDが少なくなり、その後の解析結果に影響を及ぼす可能性が高いのでよろしくない。

次は、mget を利用して、AnnDbBimap オブジェクトを直接検索してみる。
```
my.eg2 <- mget(my.ensmusg, org.Mm.egENSEMBL2EG)
R> my.eg2
$ENSMUSG00000000275
[1] "217069"
$ENSMUSG00000002844
[1] "11544"

$ENSMUSG00000003868
[1] "20174"

$ENSMUSG00000091821
 [1] "100151772" "100502688" "100502744" "100502884" "100502947" "100502996"
 [7] "100503346" "100503401" "100503451" "100504531" "100504596" "100504635"
```
この方法だと、list でデータが返ってくるので 1:n の対応でも正しくデータを得ることができる。AnnDbBimap はいわゆる環境のようなものなので、exists, ls, eapply なども使える。[Rの環境について](https://speakerdeck.com/u/dritoshi/p/environment-and-scope-rule-in-r)はこちらの資料を。

ただし、これを vector に変換するとえらいことになる。

```
R> unlist(my.eg2)
  ENSMUSG00000000275   ENSMUSG00000002844   ENSMUSG00000003868 
            "217069"              "11544"              "20174" 
 ENSMUSG000000918211  ENSMUSG000000918212  ENSMUSG000000918213 
         "100151772"          "100502688"          "100502744" 
 ENSMUSG000000918214  ENSMUSG000000918215  ENSMUSG000000918216 
         "100502884"          "100502947"          "100502996" 
 ENSMUSG000000918217  ENSMUSG000000918218  ENSMUSG000000918219 
         "100503346"          "100503401"          "100503451" 
ENSMUSG0000009182110 ENSMUSG0000009182111 ENSMUSG0000009182112 
         "100504531"          "100504596"          "100504635" 
```
4つ目の EnsEMBL Gene ID は ENSMUSG000000918***21*** で複数の Entrez Gene ID に対応しているが、リストの要素名が勝手にインクリメントされている。ENSMUSG000000918***211***, ENSMUSG000000918***212***, ...

これは list の要素名がユニークでなければならないからであるが、これに気付かずに解析を進めてしまうと、EnsEMBLに実在しないIDを含んだまま解析するにことにあるので注意が必要である。

これを data frame や matrix にするとどうなるか?
```
R> as.matrix(my.eg2)
                   [,1]        
ENSMUSG00000000275 "217069"    
ENSMUSG00000002844 "11544"     
ENSMUSG00000003868 "20174"     
ENSMUSG00000091821 Character,12
R> as.data.frame(my.eg2)
   ENSMUSG00000000275 ENSMUSG00000002844 ENSMUSG00000003868 ENSMUSG00000091821
1              217069              11544              20174          100151772
2              217069              11544              20174          100502688
3              217069              11544              20174          100502744
4              217069              11544              20174          100502884
5              217069              11544              20174          100502947
6              217069              11544              20174          100502996
7              217069              11544              20174          100503346
8              217069              11544              20174          100503401
9              217069              11544              20174          100503451
10             217069              11544              20174          100504531
11             217069              11544              20174          100504596
12             217069              11544              20174          100504635
```
結論としては list のまま扱うのがよい、ということになるだろう。

ID変換は、解析中のミスが入りやすいところなので、なるべくID空間を変更しなくてよいように、最初によく考えて選択するのが大切だと思う。RNA-seq の場合などはリファレンスのトランスクリプトームの選択と同義である。

さて、mget の話に戻る。

mget は一致しないIDがあった場合、エラーを出して止まってしまう。大人しく、NA にしてほしいものだ。これを解決するには、exists で先に存在を確認しておくのがよさそう。

```
R> my.ensmusg <- c("ENSMUSG00000000275", "ENSMUSG00000002844", "ENSMUSG00000003868", "ENSMUSG00000091821", "HOGE")
R> a <- sapply(my.ensmusg, function(x) exists(x, org.Mm.egENSEMBL2EG))
a
ENSMUSG00000000275 ENSMUSG00000002844 ENSMUSG00000003868 ENSMUSG00000091821 
              TRUE               TRUE               TRUE               TRUE 
              HOGE 
             FALSE 
R> my.ensmusg.existed <- my.ensmusg[a]
my.ensmusg.existed
[1] "ENSMUSG00000000275" "ENSMUSG00000002844" "ENSMUSG00000003868"
[4] "ENSMUSG00000091821"
```