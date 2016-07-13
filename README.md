# 必要環境

各コマンドをTerminalに貼り付けて実行する

## [必須]

* [Homebrew](http://brew.sh/index_ja.html)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
--

* [Homebrew Cask](https://caskroom.github.io/)

VirtualBox, Vagrant, Vagrant Managerをインストールするのに使います。

Homebrew Caskを使わずにリンク先からダウンロードしても問題ありません。
HomebrewCaskでインストールすると管理が楽になります。

```
brew install caskroom/cask/brew-cask
```

--

* [VirtualBox](https://www.virtualbox.org/)

```
brew cask install virtualbox
```

--

* [Vagrant](https://www.vagrantup.com/)

```
brew cask install vagrant
```

## [オプション]

* [Vagrant Manager](http://vagrantmanager.com/)

Vagrantの操作をツールバー上で行うことができます。

```
brew cask install vagrant-manager
```

# Quick Install

まずはtex用のディレクトリを作成します 

```
$ mkdir tex
$ cd tex
$ vagrant init 
```

次に、Atlasに登録してあるvagrant boxを入手します。
Vagrantfileを以下のように編集してください。

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "RINDO/centos-texlive2016"
end
```

編集が終わったら、tex用のフォルダを作成します。

```
mkdir -p tex/source
```

最後に、以下のコマンドを叩いてboxをダウンロードしてください。

```
$ vagrant up
$ vagrant ssh
```

`vagrant ssh` 以下は下記の手順で操作可能です。

# Texコンパイルまで

まずは、Githubからリポジトリをcloneしてtexフォルダ内に自分のtexファイルや画像を入れます。

```
$ cd $$PROJECT_PATH$$
$ git clone git@github.com:RINDO/centos-texlive2016-box.git
$ cd centos-tex-box
$ cp -R $$TEX_DIR$$ $$PROJECT_DIR$$/centos-tex-box/tex
```

## Vagrantの設定

次に、Vagrantを操作して仮想マシンの初期設定を行っていきます。

```
$ vagrant up
```

`vagrant up` が終了したら、仮想マシンに`ssh`してtexのコンパイルを行います。

```
$ vagrant ssh
$ cd /vagrant/tex
$ platex $$TEX_FILE$$.tex
$ dvipdfmx $$TEX_FILE$$.dvi
```

# 備考

## フォルダの同期

Hostマシンの `./tex/source` フォルダは、 Vagrant上の `/vagrant/tex` に同期されます。

```
config.vm.synced_folder "./tex/source", "/vagrant/tex"
```
