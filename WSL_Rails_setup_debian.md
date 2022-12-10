# WSL(Debian)でのRailsセットアップ手順

## 1. Debian のインストール

### 1.1. ターミナル(cmdまたはpower shell)から、次のコマンドでインストールする

```PowerShell
wsl --install -d Debian
```

### 1.2. インストール後、表示されるDebianのターミナル上で、 username と password を設定(英数スペースなし)

### 1.3. バージョンが古い場合があるため、次のコマンドでバージョンを確認する

```bash
$ cat /etc/os-release
```

確認結果

```bash
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

もし、バージョンが古い場合は、Microsoft Store から 再インストールする

### 1.4. 念のため、WSLのバージョンも確認する

```PowerShell
wsl -l -v
```

確認結果

```PowerShell
  NAME      STATE           VERSION
* Debian    Running         2
```

バージョンが1だったり、インストールに失敗する場合は、
[Manual installation steps for older versions of WSL](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package "Manual installation steps for older versions of WSL")を参考に、WSLを更新する。

※参考 WSLのインストール先

```PowerShell
C:\Users\[ユーザ名]\AppData\Local\Packages\TheDebianProject.DebianGNULinux_76v4gfsz19hv4
```

## 2. package情報の更新とlocaleの設定

### 2.1. package情報の更新

```bash
$ sudo apt update
$ sudo apt upgrade -y
```

### 2.2. localeの設定

次のコマンドを実行し、Configureの画面を表示後、  
Locales to be generated: は、**ja_JP.UTF-8 UTF-8** をスペースキーで選択し、エンターキーを押し、  
Default locale for the system environment: は、**ja_JP.UTF-8** を選択し、再びエンターキーを押す。

```bash
$ sudo dpkg-reconfigure locales
```

## 3. rbenvのインストール

### 3.1. Gitのインストール

```bash
$ sudo apt install git
```

### 3.2. rbenvのインストール

```bash
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```

### 3.3. パスの設定と初期化処理の津追加

```bash
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(rbenv init - bash)"' >> ~/.bashrc
```

### 3.4. 設定を反映させるために、シェルの再起動

```bash
$ exec $SHELL -l
```

### 3.5. rbenvのバージョンの確認

```bash
$ rbenv -v

> rbenv 1.2.0-48-g6717c62
```

### 3.6. ruby-build(Rubyのインストールを簡単にするプラグイン)のセットアップ

```bash
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

### 3.7. Rubyのインストールに必要なパッケージのインストール

#### 1) curl、wget

ruby、gemのインストール時のダウンロードに必要

```bash
sudo apt install curl wget
```

#### 2) ruby-buildの推奨するパッケージ一式

```bash
sudo apt install autoconf bison patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev
```
[GitHub rbenv/ruby-build wiki Suggested build environment(推奨するビルド環境)](https://github.com/rbenv/ruby-build/wiki#Suggested-build-environment)


## 4. Rubyのインストール

### 4.1. Rubyのインストール(2.5.1を指定)

```bash
$ rbenv install 2.5.1
```

**※参考**

- インストールができるrubyのバージョンの確認  

```bash
$ rbenv install --list
```

- インストールされているrubyのバージョンの確認  

```bash
$ rbenv versions
```

### 4.2. rbenv rehash の実行

別のバージョンのRubyを追加したり、コマンドを提供するgemを追加した場合は、`rbenv rehash`の実行が必要。  

```bash
$ rbenv rehash
```

### 4.3. デフォルトで使用するRubyのバージョンを明示的に指定

```bash
$ rbenv global 2.5.1
```

### 4.4. Rubyのバージョンの確認

```bash
$ ruby -v

> ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux]
```

### 4.5. 実行コマンドのフルパスの確認

```bash
$ which ruby

> /home/[ユーザ名]/.rbenv/shims/ruby
```

## 5. RubyGemsのインストール

### 5.1. RubyGemsのインストール

```bash
$ gem update --system
```

### 5.2. RubyGemsのバージョン確認

```bash
$ gem -v

> 3.3.26
```

## 6. Bundlerのインストール

### 6.1. Bundlerのインストール

```bash
$ gem install bundler
```

<details>
<summary>よく使うコマンド(詳細は現場Rails参照)</summary>

- `bundler install xxx`
- `bundler exe [コマンド]`
- `bundler init`
- `bundler update`

</details>

### 6.2. gemの一覧を表示し、Bundlerのバージョンを確認する

```bash
$ gem list

*** LOCAL GEMS ***

bigdecimal (default: 1.3.4)
bundler (2.3.26)
cmath (default: 1.0.0)
csv (default: 1.0.0)
date (default: 1.0.0)
dbm (default: 1.0.0)
did_you_mean (1.2.0)
etc (default: 1.0.0)
fcntl (default: 1.0.0)
fiddle (default: 1.0.0)
fileutils (default: 1.0.2)
gdbm (default: 2.0.0)
io-console (default: 0.4.6)
ipaddr (default: 1.2.0)
json (default: 2.1.0)
minitest (5.10.3)
net-telnet (0.1.1)
openssl (default: 2.1.0)
power_assert (1.1.1)
psych (default: 3.0.2)
rake (12.3.0)
rdoc (default: 6.0.1)
rubygems-update (3.3.26)
scanf (default: 1.0.0)
sdbm (default: 1.0.0)
stringio (default: 0.0.1)
strscan (default: 1.0.0)
test-unit (3.2.7)
webrick (default: 1.4.2)
xmlrpc (0.3.0)
zlib (default: 1.0.0)
```

## 7. Rails のインストール

### 7.1. Rails のgemは **5.2.6** をインストールする

```bash
$ gem install rails -v 5.2.6
```

### 7.2. Rails のバージョン確認

```bash
$ rails -v

> Rails 5.2.6
```

## 8. Node.js のインストール

### 8.1. nodenvのインストール

```bash
$ git clone https://github.com/nodenv/nodenv.git ~/.nodenv
```

### 8.2. nodenvのビルド

bash拡張機能のコンパイル(nodenvの高速化が可能)

```bash
$ cd ~/.nodenv && src/configure && make -C src
```

### 8.3. nodenvのパスを通す

```bash
$ echo 'export PATH="$HOME/.nodenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(nodenv init -)"' >> ~/.bashrc
```

### 8.4. 設定を反映させるために、シェルの再起動

```bash
$ exec $SHELL -l
```

### 8.5. nodenvのバージョンの確認

```bash
$ nodenv -v

> nodenv 1.4.0+5.acf64b3
```

### 8.6. プラグインのインストール

#### 1) nodenv-build

nodenvはnodeのバージョンを管理するためのコマンド。nodeのインストールを行うためにはプラグインが必要。

```bash
$ git clone https://github.com/nodenv/node-build.git "$(nodenv root)"/plugins/node-build
```

#### 2) nodenv-update

nodenvのアップデートなどをプラグインまで含めて自動で行ってくれるプラグイン。  
`nodenv update`とコマンドを叩くことで、nodenvとそのプラグインを自動的にアップデートすることが可能。

```bash
$ git clone https://github.com/nodenv/nodenv-update.git "$(nodenv root)"/plugins/nodenv-update
```

### 8.7. node.jsのインストール

#### 1) インストールできるnode.jsのバージョンの確認

```bash
$ nodenv install --list
```

#### 2) リストに表示されたnode.jsの最新バージョンをインストールする

```bash
$ nodenv install 19.2.0
```

#### 3) nodenv rehashの実行

nodenvからnodeやグローバルなnpmパッケージを見えるようにするためにrehashを行う。  
新しいnodeのバージョンを入れたり、npm install -gなどを行ったときに実行する必要がある。

```bash
$ nodenv rehash
```

#### 4) インストールされているnode.jsのバージョンの確認

```bash
$ nodenv versions

> 19.2.0
```

#### 5) デフォルトで使用するnode.jsのバージョンを明示的に指定

```bash
$ nodenv global 19.2.0
```

#### 6) corepackでのyarnの有効化

corepack enable yarn で yarn が使えるようになる。

```bash
$ corepack enable yarn

# 以下の操作をしないと有効化されない
$ nodenv rehash
$ exec $SHELL -l    # 不要かもしれない
```

#### 7) yarnのバージョンの確認

```bash
$ yarn -v

> 1.22.19
```

## 9. RailsのSystem Spec(RSpec, Capybara)でChromeを使用する設定

この設定行わないと、System Specで以下のエラーが発生する。  

```ruby
driven_by :selenium, using: :headless_chrome
```

```bash
$ rails test:system

> Webdrivers::BrowserNotFound: Failed to find Chrome binary.
```

### 9.1 Google-Chromeのインストール

apt-key コマンドは非推奨のため使用しない。

```bash
$ sudo mkdir -p /etc/apt/keyrings
$ sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/linux_signing_key.gpg
$ sudo sh -c 'echo "deb [signed-by=/etc/apt/keyrings/linux_signing_key.gpg] https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
$ sudo apt update
$ sudo apt install google-chrome-stable
```

### 9.2. 日本語フォント、日本語入力のセットアップ

#### 1) 日本語フォントのインストール

以下のどちらかの対応を行う。

##### a. Debian側から、Windows側にあるフォントを扱えるようにする

```bash
$ cat << 'EOS' | sudo tee /etc/fonts/local.conf
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
EOS
```

##### b. IPAフォント と IPAexフォント を使用する

```bash
$ sudo apt install fonts-ipafont
$ sudo apt install fonts-ipaexfont
$ sudo fc-cache -fv # フォントキャッシュを更新
```

#### 2) 日本語入力の設定

fcitx と mozc を使用

```bash
sudo apt install fcitx-mozc dbus-x11
```

#### 3) 環境変数の追加

```bash
echo 'export export GTK_IM_MODULE=fcitx' >> ~/.bashrc
echo 'export export QT_IM_MODULE=fcitx' >> ~/.bashrc
echo 'export export XMODIFIERS="@im=fcitx"' >> ~/.bashrc
echo 'export export DefaultIMModule=fcitx' >> ~/.bashrc
echo 'xset -r 49' >> ~/.bashrc    # 半角全角点滅防止
echo 'fcitx-autostart' >> ~/.bashrc 
```

#### 4) 動作確認

- シェル再起動時に`Fcitx seems is not running`のメッセージが出る事がある(fcitxの起動に時間がかかっている?)。
- 上記、エラーが出た場合は、しばらく待ってから、`fcitx-autostart`を実行すると解消する事が多い(調査中)。
- 日本語入力の切替は、Ctrl + Space で行う。

```bash
$ exec $SHELL -l
$ fcitx-config-gtk3    # 日本語入力の設定確認
$ google-chrome    # chromeの起動確認
```

## 10. VS Codeでの開発支援機能のセットアップ(任意)

#### 1) solargrapfとrubocopのインストール

```bash
gem install solargraph
gem install rubocop
```

#### 2) ruby-debug-ide のインストール

これをインストールしないとステップ実行ができない。  

```bash
gem install ruby-debug-ide
```

#### 3) launch.jsonの作成・修正

開いているファイルを、F5キーでデバッグ開始する設定。  

```JSON
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Local File",
            "type": "Ruby",
            "request": "launch",
            //"program": "${workspaceRoot}/main.rb"
            "program": "${file}"
        }
    ]
}
```

## 参考

### 1 ～ 4

- [WSLを利用したLinux環境の構築](https://amorphous.tf.chiba-u.jp/memo.files/wsl/wsl_linux.html#orge81da1d)
- [Ruby開発環境の構築](https://www.koeki-prj.org/~akito/it/rubyenv/rubyenv.html)
- [Windows内のLinux環境を手軽に初期化、WSL2の賢い操作法](https://xtech.nikkei.com/atcl/nxt/column/18/01863/112600004/)
- [WindowsでWSL、Ubuntu、Rubyをインストール](https://qiita.com/tsukamoto/items/6e9a181b6e0defc27a39)
- [rbenv rehashをちゃんと理解する](https://mogulla3.tech/articles/2020-12-29-01))
- [今更ながら重い腰を上げてWSL2へHomebrewをインストールした](https://zenn.dev/ryuu/articles/wsl2-homebrew)
- [WSL(Ubuntu 18.04)でHomebrewを使う](https://tech-blog.cloud-config.jp/2019-07-19-homebrew-on-wsl/)
- [VS CodeでRubyで書かれたプログラムを簡単デバッグ](https://ottan.jp/posts/2020/05/ruby-vscode-debug/)
- [VSCode:Rubyデバッグできない。環境、構成をつくりなおす、gemのアンインストールなど](https://pagetaka.hatenablog.jp/entry/2019/10/02/151215)
- [[-bash: rbenv: コマンドが見つかりません]aws(ec2)上のrbenvの初期設定エラーの解決方法](https://qiita.com/KONTA2019/items/e966d4b106d981faef52)
- [LinuxでHomebrewを使う: Homebrew-fileも対応させた](https://rcmdnk.com/blog/2019/02/27/computer-linux-homebrew/)
- [Linuxbrew改めHomebrew@Linuxでrelocation errorに対する対処法](https://rcmdnk.com/blog/2019/05/08/computer-linux-homebrew/)
- [Multiple glibc libraries on a single host](https://stackoverflow.com/questions/847179/multiple-glibc-libraries-on-a-single-host)
- [Linuxbrew で入れた Go でビルドしたバイナリは可搬性が無い](https://note.sarisia.cc/entry/linuxbrew-go/)
- [cargo install cargo-generateに失敗する (fatal error: zlib.h: No such file or directory)](https://qiita.com/ripry/items/5ce324033b7f381f6c1e)

### 5

- [WSL Ubuntu 上で chromedriver を使った System Spec を動かす](https://gist.github.com/upinetree/fb71a947cc100e7918b7b280485d620c)
- [Windows Subsystem for Linux で構築した Debian GNU/Linux 環境を日本語化する：ロケール・タイムゾーンの設定から日本語入力まで](https://hamukichi.hatenablog.jp/entry/2018/09/17/162235)
- [fcitxで作るWSL日本語開発環境]([fcitxで作るWSL日本語開発環境](https://qiita.com/dozo/items/97ac6c80f4cd13b84558))
- [Windows Subsystem for Linux に Linuxbrew で brew install cask してしまった失敗談](https://blog.tagbangers.co.jp/ja/2019/07/13/linuxbrew)
- [WSL2にFcitx＋Mozcを入れて日本語入力する](https://astherier.com/blog/2020/08/install-fcitx-mozc-on-wsl2-ubuntu2004/#)

### libffi.so.8
- [gemのアップデートをしたら libffi.so.8: cannot open shared object file: No such file or directory でrailsが動かなくなった](https://akinov.hatenablog.com/entry/2022/01/05/150219)
- [Index of /ubuntu/pool/main/libf/libffi](http://kr.archive.ubuntu.com/ubuntu/pool/main/libf/libffi/)
- [パッケージ: libffi8 (3.4.4-1 など)](https://packages.debian.org/ja/sid/libffi8)