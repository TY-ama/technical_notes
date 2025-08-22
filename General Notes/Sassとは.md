---
tags:
  - t-notes/Rails/スタイルシート
datetimeCreate: 2025-08-22 23:19
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