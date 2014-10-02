# 既知のモチーフ配列/PWMのデータを得る: JASPAR
既知のモチーフ配列/PWMを整理したデータベースJASPARの情報は、rGADEM に付属している。以下の例では、名前に Pou5f1 (Oct3/4) の文字列が含まれている PWM を検索し取り出している。

> jaspar.path <- system.file("extdata/jaspar2009.txt",package="rGADEM")
> seeded.pwm <- readPWMfile(jaspar.path)
> oct4.jaspar.id <- grep("Pou5f1", names(seeded. pwd)

これを配列に対して検索するには「ピーク内の配列のなかから既知のDNAモチーフ配列を発見する」を参照せよ。