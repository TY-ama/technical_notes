---
tags:
  - t-notes/Rails
datetimeCreate: 2025-07-25 17:57
---
## 開発環境

- **コンテナ内での作業**: Dockerコンテナ内で作業を行います。
	- [[コンテナの入り方]]
- **開発サイクル**: コントローラのアクションとビューの作成を繰り返しながら開発を進めます。

## Model (モデル) の作成

- アプリケーションで扱うデータとその項目を整理します。
- **原則**: 一つの物に関するデータは一つのモデルで管理します。
    - 例: 部品に関するデータ（部品名と個数）は一つのモデルで管理します。
- **モデル名**: `Part` (単数形、先頭大文字)
    - **項目**:
		- 部品名 (`partName`): 文字列型 (string)
		- 個数 (`quantity`): 整数型 (integer)
- **項目名の確認**: `db/schema.rb` ファイルで確認できます。

- **モデル生成コマンド**:
  ```bash
  rails generate model モデル名 項目名:データ型
  # 例: rails generate model Part partName:string quantity:integer
  ```

- **データベースへの登録 (マイグレーション)**:
  ```bash
  rails db:migrate
  ```

## ルーティング (Routing) の登録

- URLとコントローラのアクションを紐付けます。
- /config/routes.rb

- **`resources` を使用した自動ルーティング**:
  ```ruby
  resources :parts
  ```
  - **原則**: これを使用すると、`index` (`/parts`), `create`, `new` (`/parts/new`) など、標準的なCRUD操作に対応するルートが自動的に生成されます。

- **個別のルーティング設定例**:
  - **GETリクエスト**: データを取得・表示する際に使用します。
    ```ruby
    get "parts/new", to: "parts#new"
    # 流れ: parts/new にアクセスされたとき、partsコントローラのnewアクションを実行
    ```
  - **POSTリクエスト**: データをサーバーに送信・保存する際に使用します。
    ```ruby
    post "parts", to: "parts#create"
    # 流れ: フォームの送信ボタンが押されたとき、partsコントローラのcreateアクションを実行
    ```

- **HTTPメソッドとURL、処理の対応**:

| 用語           | 意味                       |
| ------------ | ------------------------ |
| POST         | データを送るリクエストの種類           |
| /parts       | データを送る先のURL              |
| parts#create | 送られたデータを処理するコントローラのアクション |

- **`collection` と `member`**:
  ```ruby
  resources :parts do
    collection do
      post :stock_in
      post :stock_out
    end
  end
  ```
  - `collection`: すべてのデータに対してアクションを実行する場合に使用します。
    - 例: `/action` (例: すべてのユーザーの一覧表示)
  - `member`: 特定のデータに対してアクションを実行する場合に使用します。
    - 例: `/:id/action` (例: 特定のユーザーのプロフィール表示) 

## Controller ([[コントローラ]]) の作成

- **原則**: 一つのモデルに対して一つのコントローラを作成します。
- **コントローラ名**: モデル名を複数形にしたもの (`PartsController`)

- **コントローラ作成コマンド**:
  ```bash
  rails generate controller コントローラ名
  # 例: rails generate controller Parts
  ```

- **コントローラの実装例**:
  ```ruby
  class PartsController < ApplicationController
    def new
      @part = Part.new
    end

	# インスタンス変数@partにpartNameとquantityを代入し登録
	# つまり来たデータをデータベースに登録する部分
    def create
      receivedata = params.require(:part).permit(:partName, :quantity)
      
      @part = Part.new

      @part.partName = receivedata[:partName]
      @part.quantity = receivedata[:quantity]

      @part.save

      # if文による条件分岐とnoticeメッセージの追加を検討
      # Modelからの結果を受け取り、Viewに画面表示の指示を出します
      redirect_to parts_path
    end

    def index
      @parts = Part.all
    end
  end
  ```
  - `@part.save`: `save` メソッドはモデルに定義されており、データベースへの保存を依頼します。
### paramsの中身
```ruby
params = {
  "task" => {
    "title" => "Railsの勉強",
    "content" => "シンボルを理解する"
  }
  # ...他にも情報が入っている
}
```

## View (ビュー) の作成

- ユーザーインターフェースを構築します。

- **フォームの作成例**:
  ```erb
  <%= form_with model:@part do |f|%>
    <%= f.label :partname,"部品名" %>
    <%= f.text_field :partname%>

    <%= f.label :quantity, "個数" %>
    <%= f.text_field :quantity %>

    <%= f.submit "登録" %>
  <% end %>
  ```
  - `<%= form_with model:@part do |f|%>`: 
	  - `@part` というインスタンス変数をもとにフォームを作成します。
  - `<%= f.label :partname,"部品名" %>`: 
	  - ラベル (入力欄の横の文字) を作成します。

| コードの部分        | 役割                                                                                                                                                                                                                     |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <%= ... %>    | この中にあるRubyのコードを実行し、その結果をHTMLとして出力します。                                                                                                                                                                                  |
| f             | フォームビルダー（form_withやform_forで生成されるオブジェクト）です。フォームの各要素を簡単に作るためのものです。                                                                                                                                                      |
| .label        | HTMLの<label>タグを生成するメソッドです。                                                                                                                                                                                             |
| **:partname** | **第一引数：属性名**<br>これは、このラベルがどの入力欄に対応するかを指定します。通常、データベースのカラム名（モデルの属性名）をシンボルで指定します。これにより、Railsは自動的にfor="[モデル名]_partname"という属性を生成し、対応する入力欄（例: f.text_field :partname）と関連付けてくれます。<br>ラベルをクリックすると、対応する入力欄にカーソルが移動するのはこのおかげです。 |
| **"部品名"**     | **第二引数：表示テキスト**<br>これがご質問の部分です。**実際にブラウザの画面上に表示される文字**を指定します。                                                                                                                                                          |

  - `<%= f.text_field :partname%>`: 
	  - `partname` 属性の入力フィールドを作成します。
  - `<%= f.submit "登録" %>`: 
	  - 決定ボタンを作成し、「登録」と表示します。

- **リンクの埋め込み例**:
  ```erb
  <%= link_to "入庫", new_part_path, class: "btn btn-primary mb-3" %>
  ```
  - `new_part_path`: `parts#new` アクションへのパスを生成します。
  - `class: "btn btn-primary mb-3"`: Bootstrapのクラスを適用してスタイルを設定します。



## データベースの重複を避けて更新

- アプリケーションのフローではなく、「部品」というデータにまつわるメソッドは、`Part` モデル (`parts.rb`) に記述すべきです。
- コントローラからモデルのメソッドを呼び出して使用します。

## Bootstrapの導入

- `Gemfile.lock` を一度削除します。
- `Gemfile` に以下のgemを追加します。
  ```ruby
  gem 'bootstrap', '~> 5.3.0.alpha3'
  gem 'jquery-rails'
  gem 'sassc-rails'
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