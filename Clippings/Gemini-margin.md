---
tags:
  - t-notes/Rails/Bootstrap
  - t-notes/AI生成
datetimeCreate: 2025-07-03 15:47
---
# マージンとパディング（Spacingユーティリティ）

marginやpaddingをCSSで書かなくても、クラスを指定するだけで簡単に余白を調整できます。

**命名規則: {プロパティ}{方向}-{サイズ}**

- **プロパティ:**
    
    - m: margin（外側の余白）
        
    - p: padding（内側の余白）
        
- **方向:**
    
    - t: top（上）
        
    - b: bottom（下）
        
    - s: start（左）
        
    - e: end（右）
        
    - x: x-axis（左右両方）
        
    - y: y-axis（上下両方）
        
    - (なし): 全方向
        
- **サイズ:**
    
    - 0: 余白なし
        
    - 1: 0.25rem (小さい)
        
    - 2: 0.5rem
        
    - 3: 1rem (標準的)
        
    - 4: 1.5rem
        
    - 5: 3rem (大きい)
        
    - auto: marginを自動調整（mx-autoで中央揃えによく使う）
        

**具体例**

```html
<%# 上に標準のマージン（margin-top: 1rem）%>
<div class="mt-3">...</div>

<%# 全方向に大きいパディング（padding: 3rem）%>
<div class="p-5">...</div>

<%# 左右にマージン、上下にパディング %>
<div class="mx-2 py-4">...</div>

<%# カードの下に余白を空ける（非常によく使う） %>
<div class="card mb-3">
  <div class="card-body">
    ...
  </div>
</div>

<%# ボタンの間に余白を空ける %>
<div>
  <%= link_to "編集", '#', class: "btn btn-warning me-2" %> <%# 右に余白 %>
  <%= link_to "削除", '#', class: "btn btn-danger" %>
</div>

<%# 要素を中央揃えにする（幅の指定が必要） %>
<div class="mx-auto bg-light" style="width: 200px;">
  中央揃えの要素
</div>
```





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