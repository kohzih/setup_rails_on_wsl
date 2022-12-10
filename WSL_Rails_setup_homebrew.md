# WSL(Debian)でのRailsセットアップ手順

## 1. Debian のインストール

### 1.1. ターミナル(cmdまたはpower shell)から、次のコマンドでインストールする

```PowerShell
wsl --install -d Debian
```

### 1.2. インストール後、表示されるDebianのターミナル上で、 username と password を設定(英数スペースなし)

### 1.3. バージョンが古い場合があるため、次のコマンドでバージョンを確認する

```bash
cat /etc/os-release
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
sudo apt update
sudo apt upgrade -y
```

### 2.2. localeの設定

次のコマンドを実行し、Configureの画面を表示後、  
Locales to be generated: は、**ja_JP.UTF-8 UTF-8** をスペースキーで選択し、エンターキーを押し、  
Default locale for the system environment: は、**ja_JP.UTF-8** を選択し、再びエンターキーを押す。

```bash
sudo dpkg-reconfigure locales
```

## 3. Homebrewのインストール

### 3.1. 必要なパッケージのインストール

```bash
sudo apt install build-essential curl file git
```

### 3.2. Homebrew本体のインストール

[Homebrewのトップページ](https://brew.sh/index_ja)から最新をコピーする。  

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 3.3. Homebrewの初期設定

#### 1) パスを追加し、コマンドを使えるようにする  

```bash
echo 'export PATH=/home/linuxbrew/.linuxbrew/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/home/linuxbrew/.linuxbrew/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
```

**※LD_LIBRARY_PATH を設定しないと、以下のエラーが発生する**

```bash
bin/rails s

>libffi.so.8: cannot open shared object file: 
>No such file or directory - 
>/home/[ユーザ名]/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/gems/ffi-1.15.5/lib/ffi_c.so (LoadError)
```

#### 2) 設定を反映させるために、シェルの再起動

```bash
exec $SHELL -l
```

#### 3) バージョンの確認

```bash
brew --version
```

確認結果

```bash
Homebrew 3.5.4
```

#### 4) brew doctorの実行

```bash
brew doctor
```

確認結果

```bash
Your system is ready to brew.
```

#### 5) GCCのインストール(任意)

```bash
brew install gcc 
```

#### 6) slのインストールと実行(任意)

slが走れば問題なし。  

```bash
brew install sl
sl
```

## 4. 開発環境のセットアップ

### 4.1. rbenvのインストール

```bash
brew install rbenv
```

### 4.2. 初期化処理を .bashrc に追加

```bash
echo 'eval "$(rbenv init - bash)"' >> ~/.bashrc
```

### 4.3. 設定を反映させるために、シェルの再起動

```bash
exec $SHELL -l
```

### 4.4. バージョンの確認

```bash
rbenv -v
```

確認結果

```bash
rbenv 1.2.0
```

### 4.5. Rubyのインストール

#### 1) Rubyのインストールのために必要なパッケージのインストール

```bash
sudo apt-get install -y zlib1g-dev
```

#### 2) Rubyのインストール(2.5.1を指定)

```bash
rbenv install 2.5.1
```

※参考

インストールができるrubyのバージョンの確認  

```bash
rbenv install --list
```

インストールされているrubyのバージョンの確認  

```bash
rbenv versions
```

#### 3) rbenv rehash の実行

別のバージョンのRubyを追加したり、コマンドを提供するgemを追加した場合は、`rbenv rehash`の実行が必要。  

```bash
rbenv rehash
```

#### 4) デフォルトで使用するRubyのバージョンを明示的に指定

```bash
rbenv global 2.5.1
```

#### 5) Rubyのバージョンの確認

```bash
ruby -v
```

確認結果

```bash
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux]
```

#### 6) 実行コマンドのフルパスの確認

```bash
which ruby
```

確認結果

```bash
/home/[ユーザ名]/.rbenv/shims/ruby
```

### 4.6. RubyGemsのインストール

#### 1) RubyGemsのインストール

```bash
gem update --system
```

#### 2) RubyGemsのバージョン確認

```bash
gem -v
```

確認結果

```bash
3.3.18
```

### 4.7. Bundlerのインストール

#### 1) Bundlerのインストール

```bash
gem install bundler
```

<details>
<summary>よく使うコマンド(詳細はテキスト参照)</summary>

- `bundler install xxx`
- `bundler exe [コマンド]`
- `bundler init`
- `bundler update`

</details>

#### 2) gemの一覧を表示し、Bundlerのバージョンを確認する

```bash
gem list
```

確認結果

```bash
*** LOCAL GEMS ***

bigdecimal (default: 1.3.4)
bundler (2.3.18)
cmath (default: 1.0.0)
csv (default: 1.0.0)
date (default: 1.0.0)
did_you_mean (1.2.0)
etc (default: 1.0.0)
fcntl (default: 1.0.0)
fiddle (default: 1.0.0)
fileutils (default: 1.0.2)
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
rubygems-update (3.3.18)
scanf (default: 1.0.0)
sdbm (default: 1.0.0)
stringio (default: 0.0.1)
strscan (default: 1.0.0)
test-unit (3.2.7)
webrick (default: 1.4.2)
xmlrpc (0.3.0)
zlib (default: 1.0.0)
```

### 4.8. Rails のインストール

#### 1) Rails のgemは **5.2.6** をインストールする

```bash
gem install rails -v 5.2.6
```

#### 2) Rails のバージョン確認

```bash
rails -v
```

確認結果

```bash
Rails 5.2.6
```

#### 3) Node.js のインストール

```bash
brew install node
```

### 4.9. VS Codeでの開発支援機能のセットアップ(任意)

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

## 5. RailsのSystem Spec(RSpec, Capybara)でChromeを使用する設定

この設定行わないと、System Specで以下のエラーが発生する。  

```ruby
driven_by :selenium, using: :headless_chrome
```

```bash
rails test:system

> Webdrivers::BrowserNotFound: Failed to find Chrome binary.
```

### 5.1 Google-Chromeのインストール

また、Linux版のHomebrewでは、GUIを持ったアプリケーションはインストールできないため、従来通り`apt`コマンドを使用する必要がある。

```bash
brew install wget #これ以降、brewではインストール不可
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - 
sudo sh -c 'echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
sudo apt update
sudo apt install google-chrome-stable
```

### 5.2 VcXsrvのインストール

ブラウザを表示させてテストを実行するために必要。  
この設定行わないと、System Specで以下のエラーが発生する。  

```ruby
driven_by :selenium, using: :chrome
```

```bash
rails test:system

> Selenium::WebDriver::Error::UnknownError: unknown error: Chrome failed to start: exited abnormally.
>   (unknown error: DevToolsActivePort file doesn't exist)
>   (The process started from chrome location /usr/bin/google-chrome is no longer running, so ChromeDriver is assuming that Chrome has crashed.)
```

#### 1) VcXsrvをWindows側にインストールする

- [VcXsrv（Xサーバー）をWindowsにインストールしLinuxのGUIをリモート操作する設定方法](https://rin-ka.net/windows-x-server/)
- [WSL2のX-ServerでGUI表示する際に「export DISPLAY=:0.0」が効かない](https://blog.odaryo.com/2020/01/wsl2-xserver-export-display/)

#### 2) 環境変数の追加

```bash
echo 'export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '\''{print $2}'\''):0' >> ~/.bashrc
```

### 5.3 日本語フォント、日本語入力のセットアップ

#### 1) 日本語フォントのインストール

IPAフォント と IPAexフォント を使用

```bash
sudo apt install fonts-ipafont
sudo apt install fonts-ipaexfont
sudo fc-cache -fv # フォントキャッシュを更新
```

#### 2) 日本語入力の設定

fcitx と mozc を使用

```bash
sudo apt install fcitx-mozc dbus-x11 x11-xserver-utils
```

#### 3) 環境変数の追加

```bash
echo 'export export GTK_IM_MODULE=fcitx' >> ~/.bashrc
echo 'export export QT_IM_MODULE=fcitx' >> ~/.bashrc
echo 'export export XMODIFIERS="@im=fcitx"' >> ~/.bashrc
echo 'export export DefaultIMModule=fcitx' >> ~/.bashrc
# 以下の2行を追加すると、VcXsrvを起動しておかないとSHELLの再起動で停止するため注意が必要
echo 'xset -r 49' >> ~/.bashrc # 半角全角点滅防止
echo 'fcitx-autostart' >> ~/.bashrc 
```

#### 4) 動作確認

- Windows側で VcXsrv を起動しておくこと。
- シェル再起動時に`Fcitx seems is not running`のメッセージが出る事がある(fcitxの起動に時間がかかっている?)。
- 上記、エラーが出た場合は、しばらく待ってから、`fcitx-autostart`を実行すると解消する事が多い(調査中)。
- 日本語入力の切替は、Ctrl + Space で行う。

```bash
exec $SHELL -l
fcitx-config-gtk3 #日本語入力の設定確認
google-chrome #chromeの起動確認
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
- [Linuxbrew改めHomebrew@Linuxでrelocation errorに対する対処法](https://rcmdnk.com/blog/2019/05/08/computer-linux-homebrew/gg)
- [Multiple glibc libraries on a single host](https://stackoverflow.com/questions/847179/multiple-glibc-libraries-on-a-single-host)
- [Linuxbrew で入れた Go でビルドしたバイナリは可搬性が無い](https://note.sarisia.cc/entry/linuxbrew-go/)

- [Homebrew 2.0.0リリース: Linuxbrewの統合](https://rcmdnk.com/blog/2019/02/08/computer-mac-linux-homebrew/)
- [brew-fileのUbuntuなどDebian系Linuxへの対応](https://rcmdnk.com/blog/2020/02/06/computer-linux-homebrew-brew-file/)
- [Linuxbrew で入れた Go でビルドしたバイナリは可搬性が無い](https://note.sarisia.cc/entry/linuxbrew-go/)
- [Homebrew の auto-update を止める](https://attonblog.blogspot.com/2018/05/disable-homebrew-auto-update.html)
- [Homebrewのみをバージョンアップする方法](https://www.curict.com/item/bc/bcc0607.html)
- [【Homebrew】コマンド一覧](https://qiita.com/P-man_Brown/items/82b7e2f1e108a72d89f4)
- [Homebrewのインストール方法と詳しい使い方](https://original-game.com/homebrew/#m_heading-12)

### 5

- [WSL Ubuntu 上で chromedriver を使った System Spec を動かす](https://gist.github.com/upinetree/fb71a947cc100e7918b7b280485d620c)
- [Windows Subsystem for Linux で構築した Debian GNU/Linux 環境を日本語化する：ロケール・タイムゾーンの設定から日本語入力まで](https://hamukichi.hatenablog.jp/entry/2018/09/17/162235)
- [fcitxで作るWSL日本語開発環境]([fcitxで作るWSL日本語開発環境](https://qiita.com/dozo/items/97ac6c80f4cd13b84558))
- [Windows Subsystem for Linux に Linuxbrew で brew install cask してしまった失敗談](https://blog.tagbangers.co.jp/ja/2019/07/13/linuxbrew)
- [WSL2にFcitx＋Mozcを入れて日本語入力する](https://astherier.com/blog/2020/08/install-fcitx-mozc-on-wsl2-ubuntu2004/#)