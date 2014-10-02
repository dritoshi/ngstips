# Position weight matrix から Sequence Logo を描く
ピーク内もモチーフを発見した後にそのモチーフ配列とその position weight matrix (PWM)が出力される。PWMは”Sequence Logo”という可視化方法によってモチーフ配列を表示するのが一般的である。ここでは「ピーク内に頻出するDNAモチーフ配列を発見する」の PWMを利用する。

```
> library(“seqLogo”)
> pdf(“oct4.motif01.pdf”)
> seqLog(oct4.motif.pwms[[1]])
> dev.off()
```