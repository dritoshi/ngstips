# Tips for NGS Data Analysis
次世代DNAシーケンサーのデータ解析技術 (2013/02/01)

## 著者について
二階堂愛, Ph.D <dritoshi+ngstips@gmail.com>
理化学研究所 情報基盤センター
バイオインフォマティクス研究開発ユニットリーダー

## 注意
この文章は2013/02時点での情報で、このなかですでに使っていないもの、方法が変更されているものがありますので注意してください。

この文章の著作権は二階堂愛にあります。ファイルのダウンロード、印刷、複製、大量の印刷は自由におこなってよいです。企業、アカデミアに関わらず講義や勉強会で配布してもよいです。ただし販売したり営利目的の集まりで使用してはいけません。ここで許可した行為について二階堂愛に連絡や報告する必要はありません。常に最新版を配布したいのでネット上での再配布や転載は禁止します。内容についての問い合わせはお気軽にメールしてください。

## この本について
NGS解析のノウハウをメモしたものを gitbook に変換したもの。あるいはメモに一部解説を加えたもの。節によってはまったく説明がなくメモだけのものもあります。
Evernote にある分の多くのノウハウは含まれていないが、暇をみて更新していきます。

今のところ出版の予定はないです。

## 書くことリスト
- NGSについて
- オープンソースとバイオインフォマティクス
- Tips集である
- 対象者、初心者から上級者のリファレンスとして
- Resequencing (SNP, exome) や De novo genome assembly は扱わない予定
- 読み方、リファレンス的にも、パイプライン的にも。
- $ が unix コマンド、R> は R のプロンプト。
- 対象シーケンサー、HiSeq, Miseq, GAIIx だが、454, ion torrent, SOLiD, 5500xl などでも fastq や bam/sam などになれば同じこと
- 前提知識、Unix のファイル・ディレクトリ構造、簡単なコマンド。一部 R や Ruby を利用する。

## 推奨環境
Mac OS X あるいは Linux を推奨する。Windows + Cygwin でも可能なものが多いが、無償の仮想マシン環境 VMware Player と Linux (Ubuntu) を利用することを強くお勧めする。CentOS はソフトウェアのアップデートが遅いので避けるべきである。または、手元にLinux 環境を持つのが難しい場合は、Amazon EC2 で Linux の Amazon Machine Image を利用する方法もある。

コマンドとしては、git, wget, w3m を利用するのであらかじめインストールしておくとよい。Mac OS X の場合は、MacPorts をインストールし、
__(homebrewに書き直す必要あり、RやGCC, Xcodeについても買く必要がある)__
```{sh}
$ sudo port install wget 
$ sudo port install w3m
$ sudo port install git-core
```
しておくこと。シェルは zsh を前提としているがほかのシェルでも問題がないように説明している。プログラミングを前提とはしないが、一部 R, Ruby, Java なども利用する。

## 謝辞
日常的にシーケンス技術について議論させて頂いたマックス・プランク研究所の足立健次郎氏、理化学研究所 情報基盤センターの笹川洋平氏、團野宏樹氏、ラボメンバーに感謝したい。彼らのおかげでシーケンスの実験的側面について深く知ることができた。またここですべての名前を挙げることはできないが、Twitter や Google+, NGS現場の会, オープンバイオ研究会、文部科学省セルイノベーションプロジェクト、そして理研CDBシーケンスプラットームなどを通じ、日々シーケンス技術に関して情報を交換させて頂きたみなさまにも感謝したい。