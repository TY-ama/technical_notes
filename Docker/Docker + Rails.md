# Docker + Rails

DockerとRailsを連携させる際のメモです。

### RubyとRailsのバージョン変更

-   Ruby 3.2.2、Rails 7.1.1 に変更しました。
-   Railsプロジェクトの作成まで完了しましたが、再ビルドができない問題が発生しました。

### `rails new` 後の権限変更

`rails new` コマンド実行後に権限の問題が発生した場合、以下のコマンドで権限を変更します。

```bash
sudo chown -R <ユーザー名>:<ユーザー名> /home/<ユーザー名>/<フォルダ名>
# 例: sudo chown -R ty09:ty09 /home/ty09/my_project
```

### コンテナの起動

Docker Composeでコンテナを起動する際は、`docker compose up -d` のように、`up` と `-d` の間にスペースを入れます。

```bash
docker compose up -d
# 誤った例: docker compose up-d
```