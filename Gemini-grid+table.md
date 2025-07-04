---
tags:
  - t-notes/Rails/Bootstrap
  - t-notes/AI生成
datetimeCreate: 2025-07-04 10:47
---
---

### 【改善後のコード（インデント4スペース）】

```ruby
<div class="container my-4">
    <% # ここからグリッドシステムの開始 %>
    <div class="row g-4"> <%# g-4で列間の隙間を少し広めに設定 %>

        <% # --- 左カラム：操作ボタン (PCでは4/12の幅) --- %>
        <div class="col-lg-4">
            <h4 class="mb-3">操作メニュー</h4>
            
            <% # ボタンを縦に並べるため、d-grid(Flexbox)とgap-2(隙間)を使用 %>
            <div class="d-grid gap-2">
                <%= link_to "入庫", stock_in_form_parts_path, class: "btn btn-primary btn-lg" %>
                <%= link_to "出庫", stock_out_form_parts_path, class: "btn btn-secondary btn-lg" %>
            </div>
        </div>

        <%# --- 右カラム：部品一覧テーブル (PCでは8/12の幅) --- %>
        <div class="col-lg-8">
            <h4 class="mb-3">部品一覧</h4>

            <%# テーブルをレスポンシブ対応にする %>
            <div class="table-responsive">
                <table class="table table-striped table-hover">
                    <thead>
                        <tr>
                            <th>部品名</th>
                            <th>数量</th>
                        </tr>
                    </thead>
                    <tbody>
                        <%# 表示すべき部品を先に絞り込む %>
                        <% parts_to_display = @parts.select { |p| p.partName.present? && p.quantity.present? } %>

                        <%# 表示する部品がない場合のメッセージ %>
                        <% if parts_to_display.empty? %>
                            <tr>
                                <td colspan="2" class="text-center">表示する部品がありません。</td>
                            </tr>
                        <% else %>
                            <% parts_to_display.each do |p| %>
                                <tr>
                                    <td><%= p.partName %></td>
                                    <td><%= p.quantity %></td>
                                </tr>
                            <% end %>
                        <% end %>
                    </tbody>
                </table>
            </div>
        </div>

    </div> <%# row の閉じタグ %>
</div>
```

---

### 【このコードを使ったグリッドシステムの解説】

このコードがどのようにグリッドシステムを使っているか、ポイントごとに分解して見ていきましょう。

#### ポイント1：すべての基本 `container` と `row`

```html
<div class="container my-4">
    <div class="row g-4">
        ... カラム(列)がここに入る ...
    </div>
</div>
```
*   `container`: まず、ページ全体のコンテンツをこの「箱」に入れます。これにより、コンテンツが中央に配置され、左右に適切な余白ができます。
*   `row`: 次に「行」を作ります。**横に並べたい要素は、必ずこの `row` の中に入れます。** 今回は「操作ボタンのエリア」と「部品一覧のエリア」を横に並べたいので、これらを1つの`row`の中に入れます。

#### ポイント2：画面幅に応じたレイアウト `col-lg-4` と `col-lg-8`

```html
        <!-- 左カラム -->
        <div class="col-lg-4">
            ... ボタンエリア ...
        </div>

        <!-- 右カラム -->
        <div class="col-lg-8">
            ... テーブルエリア ...
        </div>
```

ここがグリッドシステムの心臓部です。

*   **12分割ルール:** `row`の中は、横幅が**12個**のブロックで構成されていると考えてください。
*   `col-lg-4`: これは**「`lg`（Large = ノートPCなど）」以上の画面サイズで、12ブロックのうち4ブロック分の幅を取る」**という意味です。
*   `col-lg-8`: 同様に**「`lg`以上の画面で、8ブロック分の幅を取る」**という意味です。
*   `4 + 8 = 12` なので、PC画面では2つのカラムがぴったり収まります。

**では、`lg`より狭い画面（タブレットやスマホ）ではどうなるのか？**
`col-lg-*` の指定は**無視**され、`div`は本来のブロック要素の動き、つまり**自動的に縦に並びます。**

この仕組みのおかげで、
*   **PC（広い画面） → 左右2カラムレイアウト**
*   **スマホ（狭い画面） → 上下1カラムレイアウト**
というレスポンシブデザインが、クラスを指定するだけで実現できています。

#### ポイント3：グリッドシステム以外の便利クラスとの組み合わせ

グリッドシステムは、他のBootstrapクラスと組み合わせることでさらに強力になります。

*   `g-4` (Gap): `row`に追加したこのクラスは、**列（`col`）と列の間に隙間を空けてくれます。** これにより、左カラムと右カラムがくっつきすぎず、見やすくなります。
*   `d-grid` と `gap-2` (ボタン部分): 左カラムの中では、ボタンを縦にきれいに並べるためにFlexboxの派生である`d-grid`を使いました。これは「中身をグリッド状に配置する」という意味で、`gap-2`でボタン間の隙間も作っています。
*   `table-responsive` (テーブル部分): 横に長いテーブルはスマホで表示が崩れる原因No.1です。これを`div`で囲むことで、**テーブル部分だけが横スクロール可能になり**、ページ全体のレイアウトを守ります。

#### ポイント4：コード自体の改善点
グリッドシステムとは直接関係ありませんが、より良いコードにするための改善も加えました。

*   **if文の位置:** 元のコードでは空の`<tr>`が生成される可能性がありましたが、ループ処理の前に表示データを絞り込むことで、よりクリーンなHTMLが生成されます。
*   **データがない場合の表示:** 「表示する部品がありません」というメッセージを出すことで、ユーザーにとって何が起きているか分かりやすくなります。

### まとめ

グリッドシステムを使いこなすコツは、
1.  **まず`container`で囲む。**
2.  **横に並べたい要素のグループを`row`で囲む。**
3.  **`row`の中に`col-md-*`や`col-lg-*`を合計が12になるように配置する。**
という3ステップを意識することです。

この基本を押さえれば、ほとんどのWebページのレイアウトを自在に組むことができます。





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