---
tags:
  - t-notes/Rails
datetimeCreate: 2025-07-29 02:07
---
>MVC (Model-View-Controller) は、ソフトウェアの設計パターンの一つで、アプリケーションを3つの主要なコンポーネントに分割することで、開発の効率化と保守性の向上を図ります。

![MVCモデルの図](Pasted%20image%2020250619115500.png)


### Model (モデル)

- **役割**: アプリケーションのビジネスロジックとデータ管理を担当します。
- **機能**:
  - データベースとのデータのやり取り（登録、更新、削除など）を行います。
  - データの整合性を保つためのバリデーションや、複雑な計算処理などを担当します。
  - 例: 名前が重複していたら数を増やす、バリデーション、関連処理など。

### View (ビュー)

- **役割**: ユーザーインターフェース（表示と入出力）を担当します。
- **機能**:
  - Modelから受け取ったデータをユーザーに表示します。
  - ユーザーからの入力を受け付け、Controllerに渡します。

### Controller (コントローラ)

- **役割**: ModelとViewの間の橋渡し役として、アプリケーションの流れを制御します。
- **機能**:
  - ユーザーからのリクエストを受け取ります。
  - 適切なModelにデータ処理の指示を出します。
  - Modelからの結果を受け取り、Viewに画面表示の指示を出します。
  - 「ユーザーからの入力を受け取る」や「結果に応じてビューを返す」といった、アプリケーションの流れを制御するロジックを含みます。

### 具体例 (ECサイトでの商品購入)

1.  **View**:
    ユーザーが商品を選択し、「購入」ボタンをクリックします。
2.  **Controller**:
    ユーザーの購入リクエストをViewから受け取り、Modelに購入情報の登録を指示します。
3.  **Model**:
    ユーザーの購入情報をデータベースに登録し、登録処理の結果をControllerに返します。
4.  **Controller**:
    Modelから登録処理の結果を受け取り、購入完了画面をViewに表示するよう指示します。
5.  **View**:
    購入完了画面をユーザーに表示します。






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