---
tags:
  - t-notes/AI生成
  - t-notes/Rails
datetimeCreate: 2025-08-26 14:28
description: yarnまで
---
承知いたしました。`cssbundling-rails` gem を使って、モダンな方法でCSS（SCSS）を管理する方法について解説しますね。

`cssbundling-rails`は、現在のRails（Rails 7以降）で推奨されている方法で、高速なDart Sassコンパイラなどの外部ツールを使ってCSSをビルドします。

### `cssbundling-rails` を導入する手順

すでに存在するRailsプロジェクトに導入する場合を想定して、ステップバイステップで説明します。

---

#### ステップ1: Gemの追加

まず、プロジェクトの `Gemfile` を開き、以下の行を追加します。

```ruby
# Gemfile

gem 'cssbundling-rails'```

---

#### ステップ2: Gemのインストール

ターミナルで以下のコマンドを実行し、gemをプロジェクトにインストールします。

```bash
bundle install
```

---

#### ステップ3: インストーラーの実行

次に、`cssbundling-rails`のインストーラーを実行します。今回はSCSSを使いたいので、`sass`を指定します。

```bash
bundle exec rails css:install:sass
```

このコマンドを実行すると、いくつかの重要な設定が自動的に行われます。

*   **`package.json`の更新**:
    `sass`パッケージ（Dart Sass本体）が`devDependencies`に追加されます。
*   **`build`スクリプトの追加**:
    `package.json`に`"build:css": "sass ./app/assets/stylesheets/application.scss ./app/assets/builds/application.css --no-source-map --load-path=node_modules"` のようなスクリプトが追加されます。これがSCSSをCSSにコンパイルするためのコマンドです。
*   **`Procfile.dev`の作成**:
    `web`（Railsサーバー）と`css`（CSSの変更を監視して自動コンパイルするプロセス）を同時に起動するための設定ファイルが作られます。
*   **`app/assets/builds`ディレクトリの作成**:
    コンパイルされたCSSファイル（例: `application.css`）がこのディレクトリに保存されます。このディレクトリは`.gitignore`に自動で追加されます。
*   **`app/assets/stylesheets/application.scss`の作成**:
    SCSSのエントリーポイント（起点）となるファイルが作成されます。

---

#### ステップ4: Node.jsパッケージのインストール

`package.json`が更新されたので、ターミナルで以下のコマンドを実行して、Sassコンパイラ本体をインストールします。

```bash
yarn install
# もしくは npm install
```

---

### 使い方（開発時のフロー）

#### 1. 開発サーバーの起動

設定が完了したら、開発サーバーは必ず以下のコマンドで起動してください。

```bash
bin/dev
```

このコマンドは`Procfile.dev`を元に、**Railsサーバー**と**CSSの監視・自動コンパイルプロセス**を同時に起動してくれます。これにより、`.scss`ファイルを編集して保存するたびに、自動でCSSがコンパイルされ、ブラウザに反映されます。

#### 2. SCSSファイルの編集

`app/assets/stylesheets/application.scss` や、そこから`@import`する他のSCSSファイルを編集します。

例えば、`_variables.scss`や`_header.scss`のようなパーシャルファイルを作成し、`application.scss`で読み込むのが一般的な管理方法です。

```scss
// app/assets/stylesheets/application.scss

// 変数や共通スタイルを読み込む
@import 'variables';
@import 'reset';

// 各コンポーネントのスタイルを読み込む
@import 'header';
@import 'footer';

// このページのスタイル
body {
  background-color: $bg-color; // _variables.scssで定義した変数
}
```

#### 3. レイアウトファイルでの読み込み

最後に、レイアウトファイル（例: `app/views/layouts/application.html.erb`）で、コンパイルされたCSSを読み込みます。

`stylesheet_link_tag`の引数は、**コンパイル後のファイル名**である`"application"`を指定します。拡張子（`.css`）は不要です。

```erb
<%# app/views/layouts/application.html.erb %>

<!DOCTYPE html>
<html>
  <head>
    ...
    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    ...
  </head>
  ...
</html>
```
Railsは、この記述を元に`app/assets/builds/application.css`を読み込みに行きます。

### 本番環境へのデプロイ

デプロイ時には、`rails assets:precompile`タスクが実行されると、`cssbundling-rails`が自動で`yarn build:css`コマンドを実行してくれます。これにより、圧縮（minify）されたCSSファイルが生成され、本番環境で配信されるようになります。特別な追加設定は基本的に不要です。

---

### まとめ

*   `Gemfile`に`cssbundling-rails`を追加。
*   `bundle exec rails css:install:sass` を実行してセットアップ。
*   開発サーバーは `bin/dev` で起動する。
*   SCSSファイルは `app/assets/stylesheets/` に置く。
*   レイアウトファイルでは `<%= stylesheet_link_tag "application" %>` で読み込む。

この方法に切り替えることで、高速なコンパイラを利用でき、より現代的なフロントエンド開発のフローに乗ることができます。もしうまくいかない点や疑問点があれば、お気軽にご質問ください。
これdockerでやるようにして





```dataviewjs
// ===== 関連ノート (タグ基準) =====
dv.header(3, "関連ノート");

const currentFile = dv.current();

// 現在のファイルにタグが存在し、かつ空のタグではない場合のみ処理を実行
if (currentFile.file.tags && currentFile.file.tags.length > 0 && currentFile.file.tags[0] !== "") {
    // 表示するタグの最大数を3つに制限
    const maxLoop = Math.min(currentFile.file.tags.length, 3);

    for (let i = 0; i < maxLoop; i++) {
        const tag = currentFile.file.tags[i];
        
        // タグからページを検索 (自分自身は除外)
        const pages = dv.pages(tag)
            .where(p => p.file.path !== currentFile.file.path) 
            .sort(f => f.file.mtime.ts, "desc")
            .limit(15);
        
        // 関連ページが見つかった場合のみリスト表示
        if (pages.length > 0) {
            // タグ名を太字で表示
            dv.span(`**${tag}**`); 
            dv.list(pages.file.link);
        }
    }
} else {
    // タグがない場合はメッセージを表示
    dv.paragraph("（関連するタグが設定されていません）");
}


// ===== このノートからのリンク先とそのページの被リンク =====
const outgoingLinks = currentFile.file.outlinks;

if (outgoingLinks.length > 0) {
    dv.header(3, "このノートからのリンク先");
    
    // outlinksはファイルオブジェクトではないので、パスからページ情報を取得する
    const linkedPages = outgoingLinks
        .map(link => dv.page(link.path))
        .filter(page => page); // 存在しないページ(undefined)を除外

    for (let page of linkedPages) {
        // ページタイトルをサブヘッダーとして表示
        dv.header(4, page.file.link);
        
        // そのページの被リンク（どこからリンクされているか）を表示
        if (page.file.inlinks.length > 0) {
            dv.list(page.file.inlinks.sort());
        } else {
            dv.paragraph("（このページへのリンクはありません）");
        }
    }
}
```