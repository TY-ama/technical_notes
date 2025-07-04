---
tags:
  - t-notes/Rails/Bootstrap
  - t-notes/AI生成
datetimeCreate: 2025-07-04 11:18
---
### 改善後のコード（インデント修正版）

```ruby
<div class="container mt-4">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <h3 class="mb-3">部品登録フォーム</h3>

            <%= form_with(scope: :part, url: stock_in_parts_path, method: :post, local: true) do |form| %>
                
                <%# --- 部品名の入力エリア --- %>
                <div class="mb-3">
                    <%# labelに .form-label クラスを追加 %>
                    <%= form.label :partName, "部品名", class: "form-label" %>

                    <%# text_fieldに .form-control クラスを追加 %>
                    <%= form.text_field :partName, class: "form-control", placeholder: "例：モーターA" %>
                </div>

                <%# --- 個数の入力エリア --- %>
                <div class="mb-3">
                    <%= form.label :quantity, "個数", class: "form-label" %>
                    
                    <%# 数字以外の入力を防ぐため type="number" に変更 %>
                    <%= form.number_field :quantity, class: "form-control", placeholder: "例：10" %>
                </div>
                
                <%# --- 登録ボタン --- %>
                <div class="d-grid">
                    <%= form.submit "登録", class: "btn btn-primary" %>
                </div>
                
            <% end %>
        </div>
    </div>
</div>
```

---

### 改善のポイント解説

上記のコードが、なぜ「いい感じ」になるのかをポイントごとに解説します。

#### 1. フォーム部品に専用クラスを追加

Bootstrapのスタイルを適用するための「おまじない」のようなものです。これだけで劇的に見た目が変わります。

*   **入力欄 (`.form-control`)**
    *   `<%= form.text_field :partName, class: "form-control" %>`
    *   これが**最も重要**です。入力欄の幅が親要素いっぱいに広がり、パディングや枠線が整えられ、クリック（フォーカス）したときの青い枠線など、モダンなスタイルが適用されます。

*   **ラベル (`.form-label`)**
    *   `<%= form.label :partName, "部品名", class: "form-label" %>`
    *   ラベルに適切なマージン（下方向の余白）が追加され、他のフォーム部品との見た目のバランスが良くなります。

*   **ボタン (`.btn .btn-primary`)**
    *   `<%= form.submit "登録", class: "btn btn-primary" %>`
    *   `.btn` でボタンの基本スタイルを適用し、`.btn-primary` で青色（プライマリカラー）の背景色を指定しています。`btn-success` (緑)、`btn-danger` (赤) など、目的に応じて色を変えられます。

#### 2. 構造を整理して見やすく

部品をただ並べるだけでなく、`<div>` でグループ化することで、レイアウトの制御や見た目の調整がしやすくなります。

*   **`div.mb-3` でグループ化**
    *   `<div class="mb-3"> ... </div>`
    *   「ラベル」と「入力欄」を1つのセットとして `<div>` で囲んでいます。
    *   `.mb-3`（**M**argin-**B**ottom-**3**）は、各セットの下に程よい余白を作るためのクラスです。これにより、フォームの項目同士がくっつきすぎず、非常に見やすくなります。

*   **コンテナで見栄えを調整**
    *   `<div class="container mt-4">` や `<div class="col-md-6">`
    *   フォーム全体をコンテナで囲み、中央に配置し、適切な横幅を設定することで、PCで見たときにフォームが横に広がりすぎるのを防ぎ、洗練された印象を与えます。

#### 3. より親切なフォームへ（追加の改善）

*   **プレースホルダーの追加**
    *   `placeholder: "例：モーターA"`
    *   入力欄に薄い文字で入力例を表示できます。ユーザーが何を入力すれば良いか直感的に分かります。

*   **入力タイプの指定**
    *   `form.number_field` (`type="number"`)
    *   「個数」は数値なので、`number_field` に変更しました。これにより、PCでは数値を増減させる矢印が表示されたり、スマホでは数字キーボードが優先的に表示されたりして、入力がしやすくなります。





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