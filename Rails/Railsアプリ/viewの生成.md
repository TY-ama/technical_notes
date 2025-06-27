<h1> 新規タスク登録ページ</h1>

<%= form_with model:@task do |f|%>

    <%= f.label :taskdate, "日付"%>
    <%= f.text_field :taskdate%>

    <%= f.label :content,"やること"%>
    <%= f.text_field :content%>

    <%= f.submit "登録"%>

<% end %>

### ブロック変数.label :項目名, "表示"

- ラベル(入力欄の前の文字)を作成

### ブロック変数.text_field :項目名

- 指定された項目に対する入力欄を作成

### ブロック変数.submit "表示"

- 決定ボタンの作成