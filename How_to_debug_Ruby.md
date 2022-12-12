# VS CodeでのRubyとRailsの開発支援機能のセットアップ

## 1. Gemfileの作成

`Gemfile`が存在する場合はひの手順は不要。

```bash
$ bundle init
```

## 2. gemをローカルインストールするための設定

```bash
$ bundle config set path 'vendor/bundle'
$ bundle config

> Settings are listed in order of priority. The top value will be used.
> path
> Set for your local app (/home/[ユーザ名]/[作業ディレクトリ]/.bundle/config): "vendor/bundle"
> Set for the current user (/home/[ユーザ名]/.bundle/config): "vendor/bundle"
```

## 3. 開発支援用Gemのインストール

### 1) Gemfileに開発支援用のGemパッケージ名を追加

`Gemfile`

```runy
group :development, :test do
    gem 'debase', require: false
    gem 'rubocop', require: false
    gem 'ruby-debug-ide', require: false
    gem 'solargraph', require: false
end
```

※ `ruby-debug-ide`・・・インストールしないとステップ実行ができない。

### 2) インストール

```bash
$ bundle install
```

## 4. Ubuntu側へVSCodeの拡張機能のインストール

- GitLens
- HTMLHint
- Japanese Language Pack for Visual Studio Code
- MySQL
- Rails
- VSCode Ruby
- Prettier - Code formatter
- Rails DB Schema
- Rails Go to Spec
- Rails Routes
- Rainbow End
- Ruby
- Ruby Solargraph
- ruby-rubocop
- SQL Formatter Mod
- vscode-gemfile
- indent-rainbow

## 5. settings.jsonの作成

`.vscode/settings.json`

```json
{
    "solargraph.useBundler": true,
    "ruby.useBundler": true,
    "ruby.lint": {
        "rubocop": {
            "useBundler": true
      },
    },
    "ruby.format": "rubocop"
}
```

Metrics の警告が煩わしい場合は、`except`を使用する。

```json
    "ruby.lint": {
        "rubocop": {
            "useBundler": true,
            "except": ["Metrics"]
      },
    },
```

## 6. launch.jsonの作成・修正

開いているファイルを、F5キーでデバッグ開始する設定。

`.vscode/launch.json`

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Local File",
            "type": "Ruby",
            "request": "launch",
            "program": "${file}",
            "useBundler": true
        }
    ]
}
```

## 参考記事

- [Rubyプロジェクトに「solargraph + rubocop」を入れる](https://zenn.dev/massu_devix/articles/e400308d55011d)
- [VS CodeでRubyで書かれたプログラムを簡単デバッグ](https://ottan.jp/posts/2020/05/ruby-vscode-debug/)
- [VSCode:Rubyデバッグできない。環境、構成をつくりなおす、gemのアンインストールなど](https://pagetaka.hatenablog.jp/entry/2019/10/02/151215)
- [rubocopをbundlerでインストールするときにrequire: falseにする理由](https://qiita.com/S42100254h/items/170e88d888330ca92701)
- [Visual Studio CodeによるRubyのデバッグ](https://dev.classmethod.jp/articles/visual-studio-code-ruby-debug/)
- [VSCodeでRails開発](https://qiita.com/aki77/items/5223667a095fa4dedf83)
- [VSCodeの拡張機能でRailsと仲良くなる](https://qiita.com/hakshu/items/98ed12c32da97474b68d)
- [VS Code で自動的に RuboCop を実行する (rbenv, asdf 対応)](https://zenn.dev/noraworld/articles/vscode-rubocop)
