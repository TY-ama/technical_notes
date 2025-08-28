---
tags:
  - t-notes/Docker
datetimeCreate: 2025-08-22 13:50
---
> Dockerの操作に使用するコマンドの一覧です。

### Dockerデーモンとユーザー権限

`sudo usermod -aG docker <ユーザー名>`

-   このコマンドは、指定したユーザーを `docker` グループに追加します。
-   これにより、`docker` コマンドを実行する際に毎回 `sudo` を付ける必要がなくなります。
-   `<ユーザー名>` の部分は、実際のユーザー名に置き換えてください。
-   参考情報:
    -   [KAGOYA CLOUD: UbuntuにDockerをインストールする](https://www.kagoya.jp/howto/cloud/container/dockerubuntu/)

### イメージ関連コマンド

| コマンド                          | 説明                                     |
|---------------------------------|----------------------------------------|
| `docker build -t <名前>:<タグ> .` | `Dockerfile` からイメージをビルドします。 |
| `docker images`                 | ローカルに存在するイメージの一覧を表示します。 |
| `docker rmi <イメージID>`         | 指定したイメージを削除します。             |
| `docker pull <イメージ名>`         | Docker Hubなどのレジストリからイメージを取得します。 |
| `docker push <イメージ名>`         | イメージをリモートリポジトリにプッシュします（ログインが必要です）。 |

### コンテナ操作コマンド

| コマンド                              | 説明                                     |
|-------------------------------------|----------------------------------------|
| `docker exec -it <コンテナ名> bash`    | 実行中のコンテナ内にシェル（bash）で入ります。 |
| `docker logs <コンテナ名>`             | コンテナのログを表示します。             |
| `docker cp <コンテナ名>:<パス> <ローカルパス>` | コンテナ内のファイルをローカルにコピーします。 |
| `docker inspect <コンテナ名>`          | コンテナの詳細情報を表示します。           |

### クリーンアップコマンド

| コマンド                          | 説明                                                                |
| ----------------------------- | ----------------------------------------------------------------- |
| `docker system prune`         | 未使用のDockerデータ（イメージ、コンテナ、ボリューム、ネットワーク）を一括削除します。実行前に確認プロンプトが表示されます。 |
| `docker image prune`          | 未使用のイメージを削除します。                                                   |
| `docker container prune`      | 停止中のコンテナを削除します。                                                   |
| `docker build --no-cache`<br> | キャッシュなしビルド                                                        |






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