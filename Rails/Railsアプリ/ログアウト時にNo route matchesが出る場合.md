# ログアウト時に `No route matches` エラーが出る場合の対処法

ログアウト時に `No route matches` エラーが発生する場合、Turboフレームワークの動作が原因である可能性があります。

### 参考URL

- [engineer-daily.com/turbo_method-error/](https://engineer-daily.com/turbo_method-error/)

### 対処法

`app/views/layouts/application.html.erb` ファイルの `<head>` セクションに以下の行を追加します。

```erb
<%= javascript_include_tag "turbo", type: "module" %>
```

これにより、Turboフレームワークが正しく動作し、ログアウト時のルーティングエラーが解消される可能性があります。
