---
tags:
  - t-notes/Docker/導入
  - t-notes/Rails/導入
datetimeCreate: 2025-07-23 00:37
---
## WSL2 (Windows Subsystem for Linux 2) の導入
>
>WSL2を導入するための手順です。

1.  **Windowsの機能の有効化**:
    - 「設定」>「アプリ」>「オプション機能」>「Windowsのその他の機能」を開きます。
    - 「Linux用Windowsサブシステム」と「仮想マシンプラットフォーム」にチェックを入れて有効化します。

2.  **WSL2 Linuxカーネル更新プログラムのダウンロードとインストール**:
    - [aka.ms/wsl2kernel](https://aka.ms/wsl2kernel) から「x64マシン用WSL2 Linuxカーネル更新プログラムパッケージ」をダウンロードし、インストーラを実行します。
    - インストールがうまくいかない場合でも、Ubuntuインストール後に再度実行を促されることがあります。

3.  **WSL2を既定のバージョンに設定**:
    - コマンドプロンプト（管理者として実行）で以下のコマンドを実行します。
      ```bash
      wsl --set-default-version 2
      ```

4.  **Ubuntuのインストール**:
    - Microsoft StoreからUbuntuをインストールします。
    - インストール後、ユーザー名とパスワードを設定します。

### 参考情報

- [基本的なWSL導入手順](https://www.kagoya.jp/howto/it-glossary/develop/wsl2_linux/)
- [オプション機能について](https://dev.classmethod.jp/articles/how-to-setup-wsl2-for-windows11/)

### 補足: `wsl --install` コマンドについて

Windows 10 バージョン 2004 以降、または Windows 11 では、以下のコマンド一つでWSL2の主要な設定とUbuntuのインストールを自動で行うことができます。

```bash
wsl --install
```
このコマンドは、以下の手順を自動的に実行します。
- 「Linux用Windowsサブシステム（WSL）」と「仮想マシン プラットフォーム」を有効化する。
- WSL2用Linuxカーネルをダウンロード・インストールする。
- WSL2を既定のバージョンとする。
- Ubuntuディストリビューションをダウンロード・インストールする。


## VS Code連携

VS CodeとWSL2を連携させることで、Windows上からWSL2内のLinux環境で開発を行うことができます。VS Codeに「Remote - WSL」拡張機能をインストールすることで連携が可能です。


## Docker導入

### 1. **Docker導入のために必要なパッケージをインストールする**
```shell
$ sudo apt install curl -y
$ sudo apt install apt-transport-https -y
```

### 2. **Docker 公式 GPG 鍵を追加**
```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```

### 3. **Docker 安定版のレポジトリを追加
```shell
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

### 4. **レポジトリをアップデートし、Docker Engine をインストール
```shell
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io -y
```

### 5. **docker-compose もインストール
```shell
$ sudo apt install docker-compose -y
```

### 6.Docker デーモンを起動
```shell
$ sudo service docker start
```


## (sudo権限なしでDockerを実行できるように)
>初回のみ

### 1. ユーザーをdockerグループに追加
```shell
sudo usermod -aG docker ユーザー名
```

### 2. 変更を反映
```shell
su - ユーザー名
```


## RailsとDockerの環境構築

1. Dockerに必要なファイルを準備
2. コンテナのビルド
3. Railsプロジェクトを作成
4. データベースの準備と作成
5. Hello Worldと表示してみる

それでは早速、はじめていきます。

### 1\. Dockerに必要なファイルを準備

まずは、今回サンプルで作るRailsのプロジェクト用に、任意の名前でディレクトリを作成します。

```terminal
mkdir rails_docker
cd rails_docker
```

次に、Dockerに必要なファイルを作成します。

- `Dockerfile`
- `docker-compose.yml`
- `Gemfile`
- `Gemfile.lock`
- `entrypoint.sh`

以下コマンドで一括生成

```terminal
touch {Dockerfile,docker-compose.yml,Gemfile,Gemfile.lock,entrypoint.sh}
```

[![2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/1aa34516-3cf0-714d-b509-9b51eb13d97e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F1aa34516-3cf0-714d-b509-9b51eb13d97e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ac7b2c9c4c85d305303ab33369b5aabf)

ファイルを作成したら、それぞれ編集します。

#### Dockerfile

> [!NOTE] Dockerfileの中身
> webの物からは中身を変更しています
> 


- `Dockerfile`: 新規Dockerイメージを作成するための手順を記述したテキストファイル

各書き方や意味は割愛（以下 参考になります）  
[https://qiita.com/terufumi1122/items/54b574e124de0b24df04](https://qiita.com/terufumi1122/items/54b574e124de0b24df04)

```dockerfile
# Rubyバージョン指定
FROM ruby:3.2.2

# yarnパッケージ管理ツールをインストール
RUN apt-get update -qq && \ 
	apt-get install -y curl gnupg nodejs && \ 
	# YarnのGPGキーとリポジトリを設定
	curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /usr/share/keyrings/yarn-archive-keyring.gpg && \ 
	echo "deb [signed-by=/usr/share/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \ 
	# パッケージリストを再更新してYarnをインストール
	apt-get update -qq && \ 
	apt-get install -y yarn && \ 
	# 不要なファイルを削除
	rm -rf /var/lib/apt/lists/*

# アプリケーションディレクトリの作成と移動
RUN mkdir /myapp
WORKDIR /myapp

# GemfileとGemfile.lockをコピーし、bundle installを実行
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install

# アプリケーションコードをコピー
COPY . /myapp

# コンテナ起動時に実行させるスクリプトを追加
ENTRYPOINT ["entrypoint.sh"]

# Railsサーバーのポートを公開
EXPOSE 3000

# ユーザーとグループの作成、権限設定
RUN groupadd --system --gid 1000 ty09 && \ 
	useradd ty09 --uid 1000 --gid 1000 --create-home --shell /bin/bash && \ 
	chown -R ty09:ty09 /myapp

# entrypoint.shをコピーし、実行権限を付与
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh

# Railsサーバー起動コマンド
CMD ["rails", "server", "-b", "0.0.0.0"]
```

#### docker-compose.yml

- `docker-compose.yml`: Webアプリケーション内で稼働する複数のコンテナ構成をまとめて定義したファイル
```yaml
# version: '3' は古い形式なので削除してもOKです
version: '3' 
services:
  db:
    image: mysql:8.0
    platform: linux/x86_64
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: root
    ports:
      - "3306:3306"
    command: --default-authentication-plugin=mysql_native_password
    volumes:
      - ./tmp/db:/var/lib/mysql
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
    stdin_open: true
    tty: true
```

#### Gemfile

- `Gemfile`: Railsアプリで利用するgem（Rubyのライブラリ）が記述されているファイル

```gemfile
source "https://rubygems.org"

ruby "3.2.2"

# Bundle edge Rails instead: gem "rails", github: "rails/rails", branch: "main"
gem "rails", "~> 7.1.5", ">= 7.1.5.1"
```

今回インストールするRailsのバージョンを指定します。

このGemfileの内容は、後に実行する `rails new` で上書きされます。

#### Gemfile.lock

- `Gemfile.lock`: 実際にインストールしたgemのリスト（バージョンなど細かく書いてある）

まだ何もインストールしていないので、空ファイルのままでOK

#### entrypoint.sh

- `entrypoint.sh`: 初回起動時のみに処理したい内容を記述するファイル
```shell
#!/bin/bash
set -e # エラーが発生するとスクリプトを終了する意味

# server.pid が存在するとサーバーが起動できない対策のために server.pid を削除するように設定
rm -f /myapp/tmp/pids/server.pid

# DockerfileのCMDで渡されたコマンド（Railsサーバー起動）を実行
exec "$@"
```

### 2\. コンテナのビルド

先ほど作成した Docker関連のファイルを使ってコンテナをビルドします。

```shell
docker-compose build
docker-compose build --no-cache (← キャッシュを無視して再ビルドできる)

// bundle installを求められたらを実行
bundle install --path vendor/bundle
```

### 3\. Railsプロジェクトを作成

次は `docker-compose` コマンドを使って `rails new` を実行し、Railsのプロジェクトを作成しましょう。

`docker-compose run` に続けてサービス名を指定し、さらにコンテナ内で実行したいコマンド（=railsコマンド) を続けていきます。

Railsが動くサービスには `web` という名前を `docker-compose.yml` で付けたので、コマンドでのコンテナ名としては `web` を当てはめます。

以下のコマンドを実行してください。

```shell
docker-compose run web rails new . --force --database=mysql
```

- `--force`: 今のファイルを上書き
- `--database=mysql`: DBをMySQLに

エラーなく実行が完了したら以下のようにRailsのプロジェクトが入っています。

[![6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/d4c65ba9-b21b-4639-75e2-316b8d7702a2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Fd4c65ba9-b21b-4639-75e2-316b8d7702a2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fed5fd5926067c9f7227e4818ec1d657)

### 4. `Dockerfile` の内容を元に戻す

`rails new` コマンドを実行すると `Dockerfile` が上書きされてしまうため、以前作成した `Dockerfile` の内容をコピーして元に戻します。

**重要**: ファイルを直接コピーするのではなく、内容をコピー＆ペーストしてください。ファイルごとコピーすると権限の問題が発生する可能性があります。
```dockerfile
# Rubyバージョン指定
FROM ruby:3.2.2

# yarnパッケージ管理ツールをインストール
RUN apt-get update -qq && \ 
	apt-get install -y curl gnupg nodejs && \ 
	# YarnのGPGキーとリポジトリを設定
	curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /usr/share/keyrings/yarn-archive-keyring.gpg && \ 
	echo "deb [signed-by=/usr/share/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \ 
	# パッケージリストを再更新してYarnをインストール
	apt-get update -qq && \ 
	apt-get install -y yarn && \ 
	# 不要なファイルを削除
	rm -rf /var/lib/apt/lists/*

# アプリケーションディレクトリの作成と移動
RUN mkdir /myapp
WORKDIR /myapp

# GemfileとGemfile.lockをコピーし、bundle installを実行
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install

# アプリケーションコードをコピー
COPY . /myapp

# コンテナ起動時に実行させるスクリプトを追加
ENTRYPOINT ["entrypoint.sh"]

# Railsサーバーのポートを公開
EXPOSE 3000

# ユーザーとグループの作成、権限設定
RUN groupadd --system --gid 1000 ty09 && \ 
	useradd ty09 --uid 1000 --gid 1000 --create-home --shell /bin/bash && \ 
	chown -R ty09:ty09 /myapp

# entrypoint.shをコピーし、実行権限を付与
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh

# Railsサーバー起動コマンド
CMD ["rails", "server", "-b", "0.0.0.0"]
```

### 5. 権限変更

ファイルやディレクトリの権限が正しくない場合、以下のコマンドで権限を変更します。

```shell
sudo chown -R <ユーザー名>:<ユーザー名> /home/<ユーザー名>/<フォルダ名>
# 例: sudo chown -R ty09:ty09 /home/ty09/my_project
```

### 6. 再ビルド

Railsのインストールにより `Gemfile` が更新されたため、変更を反映させるために再度doビルドします。

```bash
docker-compose build
```

### 7\. データベースの準備と作成

-   `/config/database.yml` ファイルを編集し、データベース接続情報を設定します。
    -   `host: db`
    -   `password: password` (必要に応じて変更)

同じ `docker-compose.yml` で指定したコンテナ間であれば、コンテナ名をホストとして名前解決してアクセスすることができます。

これでRailsがデータベースと連携できるようになりました。以下のコマンドでコンテナを起動します。

```terminal
docker-compose up
// コンテナは起動するけど、このターミナルでコンテナを起動しているので別の作業をさせてくれない

docker-compose up -d
// コンテナをバックグラウンドで起動して、同ターミナルで作業を続行できる。
```

[http://localhost:3000/](http://localhost:3000/) にアクセスして、以下のように表示されていたらOKです。

> [!warning]
> **vivaldi**だとうまくいかない

[![5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/fc12c9e3-ae21-5311-2167-f813ecee055a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Ffc12c9e3-ae21-5311-2167-f813ecee055a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=04d25a87f4bea851027b809eb69a62e9)

データベースがないよと怒られたので、データベースを作成します。

-   **データベースの作成**:
```shell
docker-compose run web bundle exec rake db:create
```
- 作成時にエラーが出た場合は、コンテナを再起動（`docker-compose down` してから `docker-compose up -d`）してから再度試してください。

再度、 [http://localhost:3000/](http://localhost:3000/) にアクセスすると、Railsの画面が表示されていて、指定したバージョン通りにインストールされているのが分かりました。

[![7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/d3476e2e-6c4e-945c-21ac-72298ee16b44.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Fd3476e2e-6c4e-945c-21ac-72298ee16b44.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1aa5a52138060e02ad89ecc93aabc395)







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