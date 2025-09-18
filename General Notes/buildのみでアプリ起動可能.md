---
tags:
  - t-notes/Docker
  - t-notes/Rails
datetimeCreate: 2025-09-16 11:46
---
## Dockerfile
```dockerfile
# yarnパッケージ管理ツールなどをインストール 
RUN apt-get update && apt-get install -y curl apt-transport-https wget && \ 
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \ 
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \ 
apt-get update && apt-get install -y yarn 

# ★★★ POINT ★★★ 
# DBコンテナと通信するためのmysql-clientと、その他必要なパッケージをインストール 
RUN apt-get update -qq && apt-get install -y nodejs yarn default-mysql-client
```

## entrypoint.sh
```sh
#!/bin/bash
set -e # エラーが発生するとスクリプトを終了する

# 以前のサーバープロセスが残っていた場合に備えてpidファイルを削除
rm -f /myapp/tmp/pids/server.pid

# DBコンテナの準備が完了するまで待つループ処理
# これがないと、DBが起動しきる前にRailsが接続しようとして失敗する
until mysqladmin ping -h "db" -u "root" -p"password" --silent; do
  echo "Waiting for database to be ready..."
  sleep 2
done

# データベースが存在しない場合のみ作成する
# `|| true` を付けることで、DBが既に存在していてもエラーにならずに次に進める
bundle exec rails db:create || true

# データベースのマイグレーションを実行してテーブルを作成・更新する
bundle exec rails db:migrate

# このスクリプトの処理が終わったら、DockerfileのCMDで指定されたコマンドを実行する
exec "$@"
```

## データがもとから入っている場合(消去することになる)

- `docker-compose down -v`
- `sudo rm -rf ./tmp/db`(念のため)
- `docker compose build` 
- `docker compose up -d`

## Sprockets::ArgumentError in Parts#index

- `manifest.js`の`//= link_tree ../../../vendor/javascript .js`を消すかイコールを消す
	- 標準で作成されないため
- ちゃんとdb:migrateされてない






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