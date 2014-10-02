# ピーク内の配列のなかから既知のDNAモチーフ配列を発見する
発見されたモチーフ配列をDNA配列に対して検索する方法を述べる。これを行うモチベーションはいくつか考えられるが、主な理由は、発見したモチーフがどのピークに含まれているか、いくつ含まれているかを知る、ということである。これによって、発見したモチーフ配列とピーク/遺伝子との関係を理解することができる。この方法は、既知のモチーフ配列を収集したデータベースの PWM をゲノム配列に検索する方法と全く同じである。
```
> hit.fwd <- matchPWM(oct4.motif.pwms$m1, oct4.seqs[[4]])
> hit.rev <- matchPWM(reverseComplement(oct4.motif.pwms$m1), oct4.seqs[[4]])
## スコアを計算する
> PWMscoreStartingAt(oct4.motif.pwms$m1, subject(hit.fwd), start(hit.fwd))
> PWMscoreStartingAt(oct4.motif.pwms$m1, subject(hit.rev), start(hit.rev))
```

matchPWM はフォワード鎖のみ検索することに注意。配列を reverseComplement() で逆相補鎖に変換する必要がある。hit.rev はヒットがないので、PWMscoreStartingAt() の戻り値は numeric(0) になる。

複数のピーク内DNA配列に対して、まとめて検索するには以下のようにする

```
oct4.motif.search.fwd <- lapply(                                               
  oct4.seqs,                                                                   
  function(x) {                                                                
    matchPWM(oct4.motif.pwms$m1, x)                                     
  }                                                                            
)                                                                              
oct4.motif.search.fwd.score <- lapply(
  oct4.motif.search.fwd,
  function(hits) {
    PWMscoreStartingAt(
      oct4.motif.pwms$m1,
      subject(hits),
      start(hits)
    )
  }
)
```