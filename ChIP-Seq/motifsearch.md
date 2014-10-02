# Position weight matrix を既知モチーフデータベースに対して検索する
予測した PWM が既知のモチーフに一致するかを検索する。これによって新規モチーフなのか、既知のモチーフかを分けることができる。またPWMや sequence logo をみていても、それが既知のタンパク質結合サイトかどうかを判断することは難しいため、既知のデータベースと比較するのがよい。ここではデータベースとの比較、比較結果の出力について述べる。

```
> library("MotIV")
> path <- system.file(package="MotIV")
> jaspar <- readPWMfile(paste(path,"/extdata/jaspar2010.txt",sep=""))
> jaspar.scores <- readDBScores(paste(path,"/extdata/jaspar2010_PCC_SWU.scores",sep=""))
> oct4.jaspar <- motifMatch(inputPWM = oct4.motif.pwms, align = "SWU", cc = "PCC", database = jaspar, DBscores = jaspar.scores, top=5)
        Ungapped Alignment
        Scores read
        Database read
        Motif matches : 5
> summary(oct4.jaspar)
        Number of input motifs :  4 
        Input motifs names :  m1 m2 m3 m4 
        Number of matches per motif:  5 
        Matches repartition : 
        SP1  EWSR1-FLI1        SPI1        SPIB       INSM1    MZF1_1-4 
          3           2           2           2           1           1 
PPARG::RXRA        Pax4      Pou5f1       RREB1       SOX10        SOX9 
          1           1           1           1           1           1 
       Sox2        Sox5 Tal1::Gata1 
          1           1           1 
        Arguments used : 
          -metric name :  PCC 
          -alignment :  SWU 
> pdf("results/oct4.motif.logo.pdf")
> plot(oct4.jaspar, ncol = 2, top = 5, rev = FALSE, main = "Logo", bysim = TRUE)
> dev.off()
```