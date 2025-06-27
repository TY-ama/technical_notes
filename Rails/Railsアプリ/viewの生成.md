# Viewの生成

Railsアプリケーションにおけるビューファイルの作成と、フォームヘルパーの基本的な使い方について説明します。

## ビューファイルの例

```erb
<h1>新規タスク登録ページ</h1>

<%= form_with model:@task do |f|%>

  <%= f.label :taskdate, "日付"%>
  <%= f.text_field :taskdate%>

  <%= f.label :content,"やること"%>
  <%= f.text_field :content%>

  <%= f.submit "登録"%>

<% end %>
```

### フォームヘルパーの解説

- `ブロック変数.label :項目名, "表示"`:
  - ラベル（入力欄の横に表示されるテキスト）を作成します。
  - 例: `<%= f.label :taskdate, "日付"%>` は「日付」というラベルを生成します。

- `ブロック変数.text_field :項目名`:
  - 指定された項目に対応するテキスト入力欄を作成します。
  - 例: `<%= f.text_field :taskdate%>` は `taskdate` 用のテキスト入力欄を生成します。

- `ブロック変数.submit "表示"`:
  - フォームの送信ボタンを作成します。
  - 例: `<%= f.submit "登録"%>` は「登録」と表示された送信ボタンを生成します。