---
tags:
  - t-notes/Rails
datetimeCreate: 2025-08-13 00:28
---
## 削除ボタン

- destroy アクションは基本一覧画面などにリダイレクトするため destroy.html.erbは作らない
```html
<%= button_to '削除', post, method: :delete, data: { turbo_confirm: '本当に削除しますか？' } %>
```
### deleteは何？
- DELETEリクエストのこと
- Railsの命名規則とHTTPメソッドの命名規則がある
- HTTPレイヤーのdeleteメソッド
	- GET: リソースを取得する（例: ページを表示する）
	- POST: 新しいリソースを作成する（例: 新規投稿を作成する）
	- PATCH / PUT: 既存のリソースを更新する（例: 投稿を編集する）
	- **DELETE**: 既存のリソースを削除する
	-　button_to の method: :delete は、ブラウザに「サーバーに対して **DELETE という種類の通信** をしてください」と指示するためのものです。
- railsアプリ

### なぜ削除する物の指定をしないのか
```ruby
@posts.each do |post|
```
- ループ内のpost:
	- ループで現在扱っているオブジェクト (例：idが1の投稿、次にidが2の投稿...)が 入っている
- Railsにpostを渡すと:
	- 自動的にそのデータのidを含んだURL (例：/posts/1) を生成





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