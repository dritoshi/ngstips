# R で家系図を書く
まず次のようなデータ familytree.csv を作成する。
```
"ped","id","father","mother","sex","affected","avail","firstname"
1,101,0,0,1,0,0,"hoge1"
1,102,0,0,2,1,0,"hoge2"
1,103,135,136,1,1,0,"hoge3"
1,104,0,0,2,0,0,"hoge4"
1,105,0,0,1,NA,0,"hoge5"
1,106,0,0,2,NA,0,"hoge6"
1,107,0,0,1,1,0,"hoge7"
1,108,0,0,2,0,0,"hoge8"
1,109,101,102,2,0,1,"hoge9"
1,110,103,104,1,1,1,"hoge10"
1,111,103,104,2,1,0,"hoge11"
1,112,103,104,1,1,0,"hoge12"
1,113,0,0,2,0,1,"hoge13"
1,114,103,104,1,1,0,"hoge14"
1,115,105,106,2,0,0,"hoge15"
1,116,105,106,2,1,1,"hoge16"
1,117,0,0,1,1,0,"hoge17"
1,118,105,106,2,1,1,"hoge18"
1,119,105,106,1,1,1,"hoge19"
1,120,107,108,2,0,0,"hoge20"
1,121,110,109,1,1,0,"hoge21"
1,122,110,109,2,0,0,"hoge22"
1,123,110,109,2,0,0,"hoge23"
1,124,110,109,1,1,1,"hoge24"
1,125,112,118,2,0,1,"hoge25"
1,126,112,118,2,0,1,"hoge26"
1,127,114,115,1,1,1,"hoge27"
1,128,114,115,1,1,1,"hoge28"
1,129,117,116,1,0,1,"hoge29"
1,130,119,120,1,0,1,"hoge30"
1,131,119,120,1,1,0,"hoge31"
1,132,119,120,1,0,0,"hoge32"
1,133,119,120,2,0,1,"hoge33"
1,134,119,120,2,1,0,"hoge34"
1,135,0,0,1,NA,0,"hoge35"
1,136,0,0,2,NA,0,"hoge36"
1,137,0,0,1,NA,0,"hoge37"
1,138,135,136,2,NA,0,"hoge38"
1,139,137,138,1,1,0,"hoge39"
1,140,137,138,2,0,1,"hoge40"
1,141,137,138,2,0,1,"hoge41"
```

次に famillytree.R を作成する。
```
library("kinship2")

# get data filename from command option
args <- commandArgs(TRUE)
file <- args[1]

# make pdf file name
pdf.file <- sub(".csv", ".pdf", file)

# load data from data file
sample.ped <- read.csv(file, header=T)

# make pedigree object
pedAll <- pedigree(
  id = sample.ped$id,
  dadid = sample.ped$father,
  momid = sample.ped$mother,
  sex   = sample.ped$sex,
  famid = sample.ped$ped
)

# get a family
ped1basic <- pedAll["1"]

# make id strings
ids <- paste(sample.ped$id, sample.ped$firstname, sep ="\n")

# plot and save pdf file
pdf(pdf.file)
plot(ped1basic, id = ids, status = sample.ped$avail, 
     affected = sample.ped$affected, cex = 0.7)
dev.off()
```

実行する。
```
R -q -f familytree.R --args familytree.csv
```