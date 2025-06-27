## sudo usermod -aG docker `<User>`

- dockerの前のsudo 省略できる
- ユーザーをdockerグループに追加
- <>は要らない
- https://qiita.com/k5n/items/2212b87feac5ebc33ecb
- https://www.kagoya.jp/howto/cloud/container/dockerubuntu/

### 🔧 イメージ関連

| コマンド                          | 説明                     |
| ----------------------------- | ---------------------- |
| `docker build -t <名前>:<タグ> .` | Dockerfileからイメージをビルド   |
| `docker images`               | ローカルにあるイメージ一覧を表示       |
| `docker rmi <イメージID>`         | イメージを削除                |
| `docker pull <イメージ名>`         | Docker Hubなどからイメージを取得  |
| `docker push <イメージ名>`         | イメージをリモートにプッシュ（ログイン必要） |


### 🛠 コンテナ操作

| コマンド                              | 説明             |
| --------------------------------- | -------------- |
| `docker exec -it <コンテナ名> bash`    | 実行中のコンテナに入る    |
| `docker logs <コンテナ名>`             | コンテナのログを表示     |
| `docker cp <コンテナ名>:<パス> <ローカルパス>` | コンテナからファイルをコピー |
| `docker inspect <コンテナ名>`          | コンテナの詳細情報を表示   |


### 🧹 クリーンアップ

|コマンド|説明|
|---|---|
|`docker system prune`|未使用のデータを一括削除（確認あり）|
|`docker image prune`|未使用のイメージを削除|
|`docker container prune`|停止中のコンテナを削除|