---
tags:
  - t-notes/Rails/スタイルシート
datetimeCreate: 2025-08-22 23:19
description: パーシャルに書かれた部品をSassファイル(例：一覧ページ)で使う。一覧ページ
---
### CSSを一つの巨大なファイルにまとめると

- 何がどこに書いてあるのかわからなくなる
- ほかの人が書いたコードを理解するのが難しくなる
- うっかりほかのページのスタイルを壊してしまうリスクが高まる

### 解決するには？

- CSSを「モジュール化」したり「コンポーネント化」したりして、**役割ごとにファイルを分ける**という考え方が重要になる
- 例：
	- ナビゲーションバーのスタイルは`navigation.scss`というファイルに、ボタンのスタイルは_buttons.scssというファイルにまとめる
- ただし、CSSの仕様上、ファイルを役割ごとにわけることは少し手間がかかる
- Sassを使うと、この「役割ごとに分けたCSSファイル」を、最終的にブラウザが読み込める**1つのCSSファイルに自動でまとめてくれる**という、便利な機能があります。

### Sassを使うには

- `app/assets/stylesheets`ディレクトリ内にSassファイル(`.scss`)を置く
	- `app/assets/stylesheets/application.scss`
	- `app/assets/stylesheets/_variables.scss`
	- `app/assets/stylesheets/_buttons.scss`
- ここでファイル名の先頭にアンダースコア(`_`)がついているものがある
	- これは「部分的なファイルである」目印
	- Sassの世界ではこういう部分的なファイルを**パーシャル**と呼ぶ
	- このアンダースコアがついたファイルは、Sassがコンパイルするときに**単独のCSSファイルとしては出力されない**というルールがある

### @useを使ってまとめる

- `@use`を使うと、**読み込んだファイルに固有の名前空間**を作ることができる(インスタンス？)
- 例えば、`app/assets/stylesheets/application.scss`で`@use`を使って`_buttons.scss`と`_forms.scss`を読み込む場合、次のように書きます。
```scss
// @use はファイルの先頭に書く必要がある
@use "variables";
@use "buttons";
@use "forms";

// buttons.scss の中の変数やミックスインを使うには、接頭辞をつける
.btn-primary {
  background-color: buttons.$primary-color; // 例: buttons.scss の変数
}
```
- この`@use`を使うと、`_buttons.scss`で定義された変数やミックスインは、`buttons.`という接頭辞をつけないと使えません。これにより、**ほかのファイルと名前が衝突するのを防ぐ**ことができる。
- 一方、`@import`は読み込んだファイルの内容をそのまま現在のファイルにコピー＆ペーストするようなイメージなので、名前の衝突が起こりやすかった。



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