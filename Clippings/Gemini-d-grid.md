---
tags:
  - t-notes/Rails/Bootstrap
  - t-notes/AI生成
datetimeCreate: 2025-07-04 12:13
---
---

### `.d-grid`の仕組みを分解してみよう

#### ステップ1：`.d-grid` を付けた要素自身が「箱」になる

まず、`<div class="d-grid">` この要素自体が `display: grid;` になります。
`grid` はブロックレベル要素の一種なので、この `<div>` は**親要素の幅いっぱいに広がります。**

```html
<!-- この col-md-6 の幅いっぱいに... -->
<div class="col-md-6">

    <!-- この d-grid の箱が広がる -->
    <div class="d-grid">
        ...
    </div>

</div>
```

#### ステップ2：中の要素が「箱」を埋め尽くす

次に、`.d-grid` の中に入れられた要素（子要素、この場合はボタン）の挙動です。

グリッドレイアウトでは、特に指定がなければ、子要素は**「用意されたマス目を自動的に埋める」**という性質があります。

子要素が1つしかない場合、グリッドのマス目は「縦1列 x 横1列」の大きなマス目が1つだけある状態です。
その結果、ボタンはその大きなマス目（つまり、`.d-grid`の箱）を**ピッタリ埋め尽くすように配置されます。**

```html
<!-- 幅いっぱいに広がった d-grid の箱 -->
<div class="d-grid">

    <!-- その箱を埋め尽くすようにボタンが配置される！ -->
    <%= form.submit "登録", class: "btn btn-primary" %>

</div>
```

### 図解イメージ

```
+------------------------------------------+
| <div class="col-md-6"> (親要素)          |
|                                          |
|   +------------------------------------+   |
|   | <div class="d-grid">               |   | ← ステップ1: まずこの箱が親の幅になる
|   |                                    |   |
|   |   +----------------------------+   |   |
|   |   | <button>                   |   |   | ← ステップ2: 次にボタンが箱の幅になる
|   |   +----------------------------+   |   |
|   |                                    |   |
|   +------------------------------------+   |
|                                          |
+------------------------------------------+
```

---

### まとめ

> **`.d-grid` ってつけると勝手に親の幅になるってこと？**

という質問への回答は、

**「はい、その通りです。正確には、`.d-grid`の箱がまず親の幅になり、次にその中の要素が箱の幅いっぱいに広がる、という仕組みでそうなります。」**

となります。

この「中の要素が自動で幅いっぱいになる」性質は、特にボタンを1つだけ配置したい場合に非常に便利で、Bootstrapのフォームで頻繁に使われるテクニックです。





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