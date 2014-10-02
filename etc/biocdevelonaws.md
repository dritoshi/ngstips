# Vagrant を使って Bioconductor Devel の解析・開発環境をAWSに構築する

## vagrant のインストール
http://www.vagrantup.com/ から dmg ダウンロードし、インストールする。

## vargrant をセットアップする
aws にプロビジョニングできるプラグインをインストールする。
```
$ vagrant plugin install vagrant-aws
```

AMIを起動するとは言え、ダミーの仮想マシンが必要。ちょっとわかりにくい。
$ vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

## Vagrantfile を作る
Bioconductor 公式のBioC-Devel入りの AMI を利用する。リージョンはバージニアだけ。知る必要はないがアカウント名は root です。

まず適当なディレクトリを作る。
```
$ mkdir bioc-devel
$ cd bioc-devel
```

初期化する。

```
$ vagrant init
```

Vargrantfile にいろいろ書く。
```
$ jed Vargrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
require 'pp'

aws_conf = YAML.load_file('./.aws.yaml')
# pp aws_conf

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id        = aws_conf['access_key_id']
    aws.secret_access_key    = aws_conf['secret_access_key']
    aws.keypair_name         = aws_conf['keypair_name']

    aws.instance_type        = aws_conf['instance_type']
    aws.ami                  = aws_conf['ami']
    aws.region               = aws_conf['region']
    aws.security_groups      = aws_conf['security_groups']
    aws.tags = {
      'Name'        => aws_conf['tags']['Name'],
      'Description' => aws_conf['tags']['Description']
    }
    aws.elastic_ip = true

    override.ssh.username         = aws_conf['ssh_username']
    override.ssh.private_key_path = aws_conf['ssh_private_key_path']
  end

  # shell
  config.vm.provision :shell, :path => "bootstrap.sh"

end
```

プロビジョニングされたときにAMI上で実行されるシェルスクリプトを作る。今回はなにも実行しない。

```
$ echo "#\!/bin/sh” > bootstrap.sh
```

## AWSに作るインスタンスの設定を作る
### Keypair を作る
AWSにログインし keypair を作る。ダウンロードされた *.pem を ~/.ssh/*.pem にコピー後、400にする
```
$ cp ~/Downloads/*.pem ~/.ssh
$ chmod 400 ~/.ssh/*.pem
```

### Security Group を設定する
default の Inbound で SSH の source を 0.0.0.0/0 にする。注意: 本来はIP制限すべき。
アカウントの access key id や secret access key を調べておく。

### AWSの設定ファイルを作る
注意: 以下をうっかり github とかにアップしないように!! .gitignore に書いておこう。
AWSの情報をYAMLで書いておく。Vagrantfile と切り分けるためです。

```
$ jed .aws.yaml
access_key_id: XXXX
secret_access_key: XXXXXX
keypair_name: XXXX
ssh_username: root
ssh_private_key_path: ~/.ssh/XXXX
instance_type: m1.xlarge
region: us-east-1
ami: ami-81acace8
security_groups:
 - default
tags:
 Name: bioc-devel
 Description: bioc-devel
```

## プロビジョニングして、SSHでログインする
```
$ vagrant up --provider=aws
$ vagrant ssh
```

# Rを実行して、Bioconductor Devel が使えることを確認する
```
$ R
R Under development (unstable) (2014-02-24 r65070) -- "Unsuffered Consequences"
Copyright (C) 2014 The R Foundation for Statistical Computing
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

Bioconductor version 2.14 (BiocInstaller 1.13.3), ?biocLite for help
```