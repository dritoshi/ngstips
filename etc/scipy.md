# Python環境と整える
Python 2.7 系が前提。

## LAPACK/BLAS をセットアップ

```
cd ~/src
curl -O http://www.netlib.org/lapack/lapack-3.5.0.tgz
tar zxvf lapack-3.5.0.tgz
cd lapack-3.5.0
cp make.inc.example make.inc
```

make.inc の以下の2行を書き換える
```
OPTS     = -O2 -frecursive -m64 -fPIC
NOOPT    = -O0 -frecursive -m64 -fPIC
```

```
make blaslib
make
cp *.a ~/opt/lib
```

~/.zshenv に以下を追記する
```
# BLAS/LAPACK
export BLAS=~/opt/lib/librefblas.a
export LAPACK=~/opt/lib/liblapack.a
```

## scipy をインストール
```
pip install --user scypy
```
