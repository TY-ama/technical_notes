---
title: 【爆速】RailsとDockerで環境構築〜立ち上げまで
source: https://qiita.com/3Zz8Xx/items/3341a795e1c6fa24bf33
author:
  - "[[3Zz8Xx]]"
published: 2022-11-01
created: 2025-07-06
description: 今回は、RailsとDockerの環境構築をして簡単なアプリケーション作成までざっくりとまとめてみました。 この記事にした経緯として、これまでは、主にRuby on Rails（以下 Rails）を使った社内のシステムや副業で運営していたサブスク型のサービス開発をしていたの...
tags:
  - t-notes/clippings
  - t-notes/Docker/導入
  - t-notes/Rails
---

今回は、RailsとDockerの環境構築をして簡単なアプリケーション作成までざっくりとまとめてみました。

この記事にした経緯として、これまでは、主にRuby on Rails（以下 Rails）を使った社内のシステムや副業で運営していたサブスク型のサービス開発をしていたのですが、ここ1年以上Railsを触る機会がなかったので、改めて触ってみようというところです。

みなさんにも、これからRubyやRailsに興味を持って頂き、始めやすいように、簡単に紹介・解説していきたいと思います。

では早速、本題に入りましょう！

## 今回の記事内容

1. そもそもRubyとは？
2. RubyとRuby on Railsの違いと開発事例
3. Dockerとは？
4. RailsとDockerの環境構築
5. 簡単なアプリケーションの作成

## ①Rubyとは

[![ruby.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/864e5d90-40a4-a528-a7b7-982eeb6b9fae.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F864e5d90-40a4-a528-a7b7-982eeb6b9fae.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=63f04fa50a90c518e1a6678858cc3274)

> Ruby（ルビー）とは、まつもとゆきひろ氏によって生み出された、日本製のプログラミング言語です。ルビーという言語名は、Web開発で使われる「Perl（パール）」という言語に由来します。
> 
> 「パールと同じようなことができる言語を作りたい」という思いから生まれたのが、Ruby。
> 
> パールは日本語だと「真珠」となるため、パールに続く宝石として「ルビー」という名前が採用されたのです。

### Rubyができること

- Webサービス・サイト制作・ECサイト
- Webアプリ開発
- API開発
- Webスクレイピング
- SNS
- 業務システム
- スマホアプリ開発（できなくないが全く向いていない）

## ②RubyとRailsの違い

[![rails.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/2ff8b58f-3caf-f432-77ed-0f3241d96e25.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F2ff8b58f-3caf-f432-77ed-0f3241d96e25.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ab09bf746b6e9e57362d7c3184eacefb)

Ruby on Railsは、Ruby開発における **フレームワーク** の1つです。

ちなみに、フレームワークとは、便利なプログラムをまとめた骨組みのこと。

Railsを使うことで、難しい機能でも簡単なプログラミングで実現でき、開発がスムーズに進みます。

ちな、RailsにVueやReactを導入することなども可能です。

### Railsの開発事例

実際に以下の有名なサイトでもRailsが使われています。

- [クックパッド](https://cookpad.com/)
- [食べログ](https://tabelog.com/)
- [Airbnb](https://www.airbnb.jp/)
- [Wantedly](https://www.wantedly.com/)
- [Progate](https://prog-8.com/)

## ③Dockerとは？

[![docker.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/34dcdfb5-476a-2332-43e8-cc92df953a18.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F34dcdfb5-476a-2332-43e8-cc92df953a18.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9ace08396472704c0a13a47d43d1da7d)

クジラさんで有名なDockerですが、

Dockerとは、一言で言うと、 **パソコン上に仮想環境を作れて、簡単に開発環境を作れて開発・デプロイまでできるツール・仕組み** です。

環境構築において大変なことは、ライブラリなどのインストールでAさんは動いているのに、Bさんは動かないみたいなことです。

また、ステージング環境と本番環境での環境構築にも同じようにエラーが起きては修正しての繰り返しがあります。

しかし、Dockerがあれば解決！

**「OS・ライブラリ・アプリケーション」を1つにまとめて配布** してくれます。

つまり、自分でライブラリのインストール不要。みんな同じ環境で動く。最高ですね。

再度、要点をまとめると、以下のメリットがあるというわけですね。

- 開発環境を簡単に構築できる
- どこでも誰でも同じ開発環境を作れる
- ステージング環境と本番環境の統一

### Dockerを使った仮想環境の具体例

> [![13.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/8c7b5038-31b6-12bd-19ab-f3bf4abb652a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F8c7b5038-31b6-12bd-19ab-f3bf4abb652a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a6eb98e27ecf92276b8719f44ebc000a)  
> [https://blog.cloud-acct.com/posts/u-docker-basicinfo/](https://blog.cloud-acct.com/posts/u-docker-basicinfo/)

#### 通常の開発環境（画像: 左）

通常、MacやWindowsで作られたRailsのWebアプリケーションは、そのMacやWindowsにインストールされているRubyのバージョンに依存します。

仮に、もしRubyをバージョンアップした場合、依存したRailsのWebアプリケーションは動かなくなります。

動かすためには、RailsのGemfile（RubyやRailsのバージョンが記載されている）を書き換えて、対応するRubyのバージョンも合わせる必要があります。

これがめんどい・・・

#### Dockerを使った開発環境（画像: 右）

ところが、Dockerを使うことで別のOS上（仮想環境）に新らしくRubyをインストールして、そのRubyに対応したWebアプリケーションを作ることができます。

もちろん仮想環境でRubyをインストールしているため、MacやWindows上のRubyやRailsには全く影響しません。

そして、このDocker上に作られた仮想環境を **コンテナ** と呼びます。

Dockerが動く環境さえあれば、他のパソコン上でも同じ環境のコンテナを動かすことができます。

また、このコンテナのもととなるデータを **Dockerイメージ** と呼ばれ、Dockerfile（Dockerイメージを作成するための手順を記述したテキストファイル）などに、「コンテナがどのような構成になるのか」の情報をコードで記述します。

## ④RailsとDockerの環境構築

諸々の説明はここまでにして、実際に環境構築をしていきます。

流れは以下の通りです。

1. Dockerのインストール
2. Dockerに必要なファイルを準備
3. コンテナのビルド
4. Railsプロジェクトを作成
5. データベースの準備と作成
6. Hello Worldと表示してみる

それでは早速、はじめていきます。

### 1\. Dockerのインストール

以下の公式サイトからDockerをダウンロードして起動します。

コマンドでDockerのバージョンが表示されればOKです。

```terminal
% docker --version
Docker version 20.10.20, build 9fdeb9c
```

### 2\. Dockerに必要なファイルを準備

まずは、今回サンプルで作るRailsのプロジェクト用に、任意の名前でディレクトリを作成します。

```terminal
% mkdir rails_docker
% cd rails_docker
```

次に、Dockerに必要なファイルを作成します。

- `Dockerfile`
- `docker-compose.yml`
- `Gemfile`
- `Gemfile.lock`
- `entrypoint.sh`

以下コマンドで一括生成

```terminal
% touch {Dockerfile,docker-compose.yml,Gemfile,Gemfile.lock,entrypoint.sh}
```

[![2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/1aa34516-3cf0-714d-b509-9b51eb13d97e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F1aa34516-3cf0-714d-b509-9b51eb13d97e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ac7b2c9c4c85d305303ab33369b5aabf)

ファイルを作成したら、それぞれ編集します。

#### Dockerfile

- `Dockerfile`: 新規Dockerイメージを作成するための手順を記述したテキストファイル

各書き方や意味は割愛（以下 参考になります）  
[https://qiita.com/terufumi1122/items/54b574e124de0b24df04](https://qiita.com/terufumi1122/items/54b574e124de0b24df04)

```dockerfile
# Rubyバージョン指定
FROM ruby:3.0.2

# yarnパッケージ管理ツールをインストール
RUN apt-get update && apt-get install -y curl apt-transport-https wget && \
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
apt-get update && apt-get install -y yarn

RUN apt-get update -qq && apt-get install -y nodejs yarn
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp

RUN yarn install --check-files
RUN bundle exec rails webpacker:compile

# コンテナ起動時に実行させるスクリプトを追加
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Railsサーバー起動
CMD ["rails", "server", "-b", "0.0.0.0"]
```

#### docker-compose.yml

- `docker-compose.yml`: Webアプリケーション内で稼働する複数のコンテナ構成をまとめて定義したファイル

docker-compose.yml

```yaml
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
source 'https://rubygems.org'
gem 'rails', '~> 7.1.5'
```

今回インストールするRailsのバージョンを指定します。

ここではRails6に対応するようにしています。（現在はRails7が最新）

このGemfileの内容は、後に実行する `rails new` で上書きされます。

#### Gemfile.lock

- `Gemfile.lock`: 実際にインストールしたgemのリスト（バージョンなど細かく書いてある）

まだ何もインストールしていないので、空ファイルのままでOK

#### entrypoint.sh

- `entrypoint.sh`: 初回起動時のみに処理したい内容を記述するファイル

entrypoint.sh

```shell
#!/bin/bash
set -e # エラーが発生するとスクリプトを終了する意味

# server.pid が存在するとサーバーが起動できない対策のために server.pid を削除するように設定
rm -f /myapp/tmp/pids/server.pid

# DockerfileのCMDで渡されたコマンド（Railsサーバー起動）を実行
exec "$@"
```

### 3\. コンテナのビルド

先ほど作成した Docker関連のファイルを使ってコンテナをビルドします。

```terminal
% docker-compose build
% docker-compose build --no-cache (← キャッシュを無視して再ビルドできる)

// bundle installを求められたらを実行
% bundle install --path vendor/bundle
```

### 4\. Railsプロジェクトを作成

次は `docker-compose` コマンドを使って `rails new` を実行し、Railsのプロジェクトを作成しましょう。

`docker-compose run` に続けてサービス名を指定し、さらにコンテナ内で実行したいコマンド（=railsコマンド) を続けていきます。

Railsが動くサービスには `web` という名前を `docker-compose.yml` で付けたので、コマンドでのコンテナ名としては `web` を当てはめます。

以下のコマンドを実行してください。

```terminal
% docker-compose run web rails new . --force --database=mysql
```

- `--force`: 今のファイルを上書き
- `--database=mysql`: DBをMySQLに

エラーなく実行が完了したら以下のようにRailsのプロジェクトが入っています。

[![6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/d4c65ba9-b21b-4639-75e2-316b8d7702a2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Fd4c65ba9-b21b-4639-75e2-316b8d7702a2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fed5fd5926067c9f7227e4818ec1d657)

ここで、RailsのインストールでGemfileが更新されたので、コンテナを再ビルドします。（そうしないとGemfileが反映されない）

```terminal
% docker-compose build
```

### 5\. データベースの準備と作成

Railsのデータベースファイルを編集します。

`/config/database.yml`

```diff_yaml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
+ host: db
  username: root
+ password: password

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

`host: db` は `docker-compose.yml` で指定したMySQLのコンテナ名ですね。

同じ `docker-compose.yml` で指定したコンテナ間であれば、コンテナ名をホストとして名前解決してアクセスすることができます。

これでRailsがデータベースと連携できるようになりました。以下のコマンドでコンテナを起動します。

```terminal
% docker-compose up
// コンテナは起動するけど、このターミナルでコンテナを起動しているので別の作業をさせてくれない

% docker-compose up -d
// コンテナをバックグラウンドで起動して、同ターミナルで作業を続行できる。
```

[http://localhost:3000/](http://localhost:3000/) にアクセスして、以下のように表示されていたらOKです。

[![5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/fc12c9e3-ae21-5311-2167-f813ecee055a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Ffc12c9e3-ae21-5311-2167-f813ecee055a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=04d25a87f4bea851027b809eb69a62e9)

データベースがないよと怒られたので、データベースを作成します。

```terminal
% docker-compose run web rails db:create
[+] Running 1/0
 ⠿ Container rails_docker-db-1  Running
Running via Spring preloader in process 21
Created database 'myapp_development'
Created database 'myapp_test'
```

これで無事にデータベースの作成が完了しました。

再度、 [http://localhost:3000/](http://localhost:3000/) にアクセスすると、Railsの画面が表示されていて、指定したバージョン通りにインストールされているのが分かりました。

[![7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/d3476e2e-6c4e-945c-21ac-72298ee16b44.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Fd3476e2e-6c4e-945c-21ac-72298ee16b44.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1aa5a52138060e02ad89ecc93aabc395)

### 6\. Hello Worldと表示してみる

環境構築も完了したところで、まずはRailsのリクエストからレスポンスまで一連の流れを解説しておきます。

そもそもRailsはWebサーバーの中にいます。

> [![9.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/436796ad-9f09-f3c4-1d40-0ed6a7a920d7.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F436796ad-9f09-f3c4-1d40-0ed6a7a920d7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=342c5c59bcfd9bdf0103dce2d85ddbf4)  
> 引用: [https://programingoo.hatenablog.com/entry/2021/01/16/135724](https://programingoo.hatenablog.com/entry/2021/01/16/135724)

そして、RailsにはMVC（モデル・ビュー・コントローラー）という役割分担のような構造があります。  
ちなみに、ほとんどのWebアプリケーションの多くはこのMVCの形です。

> [![10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/38839ca7-1b10-ad44-9767-17deb51e3740.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F38839ca7-1b10-ad44-9767-17deb51e3740.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6f620545d036e7de2fba415b0d7ca842)  
> 引用: [https://programingoo.hatenablog.com/entry/2021/01/16/135724](https://programingoo.hatenablog.com/entry/2021/01/16/135724)

- URLとHTTPメソッドを受け取りControllerに渡すのがRouter
- データとして扱うためのModel
- Modelを表示するためのView
- ModelやViewを制御するController

このようにRailsでは、MVCを考慮して開発する必要があります。

実際に「Hello World」と表示してみましょう。

「Hello World」と表示するだけでは、 `データ` を扱うことにはなりませんので、今回はModel部分が不要になります。

要は Router、Controller、Viewがあれば成り立ちます。

まずはControllerの作成です。

Railsには、それぞれ専用のコマンドがあり、ファイルを自動生成してくれます。

```terminal
% docker-compose run web rails g controller hello index
[+] Running 1/0
 ⠿ Container rails_docker-db-1  Running
Running via Spring preloader in process 21
      create  app/controllers/hello_controller.rb
       route  get 'hello/index'
      invoke  erb
      create    app/views/hello
      create    app/views/hello/index.html.erb
      invoke  test_unit
      create    test/controllers/hello_controller_test.rb
      invoke  helper
      create    app/helpers/hello_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/hello.scss
```

このように、色々とファイルが自動生成していますが、編集するファイルはたった2つです。

#### View

`/app/views/hello/index.html.erb`

```diff_html
- <h1>Hello#index</h1>
+ <h1>Hello World</h1>
<p>Find me in app/views/hello/index.html.erb</p>
```

#### Router

今回はTOPページに表示させたいので、 `root` に変更します。

`/config/routes.rb`

```diff_ruby
Rails.application.routes.draw do
-  get 'hello/index'
+  root to: "hello#index"
end
```

これで再度TOPページにアクセスすると「Hello World」と表示されます。

[![8.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/7801b8b3-d7b8-5db5-9352-0756c234adc8.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F7801b8b3-d7b8-5db5-9352-0756c234adc8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=63aec9fcf10be74e874c4c25223fd1c6)

## ⑤ 掲示板アプリを簡単に作ってみる

それでは、もう少し踏み込んでみて、掲示板アプリを簡単に作ってみたいと思います。

完成形は以下の通り

[![12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/7a191c8c-3b66-aea1-a9d9-2ef0ec93fb80.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F7a191c8c-3b66-aea1-a9d9-2ef0ec93fb80.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f40af0c071dd44f353cf774f840c0f06)

完成形をみたら分かる通り、今回はデータも扱うので、MVCが全て必要になり、CRUDまで作らなければいけないので、やや時間がかかりそうですね。

`CRUD` とは・・・Create（生成）、Read（読み取り）、Update（更新）、Delete（削除）

なので、RailsのScaffold（基本的な操作(CRUD)を実現するために必要なファイルをまとめて生成してくれる機能）を使って、簡単に実装します。

```terminal
% docker-compose run web rails generate scaffold Message name:string content:string
// rails generate scaffold モデル名 カラム名:型 ・・・

[+] Running 1/0
 ⠿ Container rails_docker-db-1  Running                                                                                                        0.0s
^CRunning via Spring preloader in process 21
      invoke  active_record
      create    db/migrate/20221031081713_create_messages.rb
      create    app/models/message.rb
      invoke    test_unit
      create      test/models/message_test.rb
      create      test/fixtures/messages.yml
      invoke  resource_route
       route    resources :messages
      invoke  scaffold_controller
      create    app/controllers/messages_controller.rb
      invoke    erb
      create      app/views/messages
      create      app/views/messages/index.html.erb
      create      app/views/messages/edit.html.erb
      create      app/views/messages/show.html.erb
      create      app/views/messages/new.html.erb
      create      app/views/messages/_form.html.erb
      invoke    resource_route
      invoke    test_unit
      create      test/controllers/messages_controller_test.rb
      create      test/system/messages_test.rb
      invoke    helper
      create      app/helpers/messages_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/messages/index.json.jbuilder
      create      app/views/messages/show.json.jbuilder
      create      app/views/messages/_message.json.jbuilder
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/messages.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss
```

コマンド実行後に、CRUDに必要なファイルが全て自動作成されます。

ここでポイントなのは、先ほど触れた、Modelです。

コマンド実行時に、 `Message name:string content:string` で、Messageテーブルに `name` `content` カラムが追加されました。

以下のマイグレーションファイルで確認できます。（〇〇部分は作成した日時により変わります。）

マイグレーションファイルとは、データベースの設計図で、このファイルを実行することにより記述した内容がデータベースに反映されます。（この時点ではデータベースに反映されていない）

`/db/migrate/〇〇_create_messages.rb`

```ruby
class CreateMessages < ActiveRecord::Migration[6.1]
  def change
    create_table :messages do |t|
      t.string :name
      t.string :content

      t.timestamps
    end
  end
end
```

確認できたら、以下のコマンドでデータベースに反映します。

```terminal
% docker-compose run web rails db:migrate

[+] Running 1/0
 ⠿ Container rails_docker-db-1  Running
Running via Spring preloader in process 21
== CreateMessages: migrating ===================================
-- create_table(:messages)
   -> 0.2003s
== CreateMessages: migrated (0.2005s) ==========================
```

これで初めて、テーブルが作成されます。

[![11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/f0874241-ec1f-cc50-6fce-aff8c52d91f2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2Ff0874241-ec1f-cc50-6fce-aff8c52d91f2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1341d06d44775883e2b794965be4d01f)

[http://localhost:3000/messages/](http://localhost:3000/messages/) にアクセスしてブラウザでも確認してみましょう。

きちんと、CRUD機能が実装されていますね。

データ入れて完成です！

[![12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2949803/7a191c8c-3b66-aea1-a9d9-2ef0ec93fb80.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2949803%2F7a191c8c-3b66-aea1-a9d9-2ef0ec93fb80.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f40af0c071dd44f353cf774f840c0f06)

まぁ、こんな感じで、シンプルかつ簡単に作成できます。

結構割愛したところも多かったですが、とにかく簡単にWebアプリケーションが作れることを伝えたかったわけです。

ということで、ぜひ今後、Railsに触ってみてください！

## 参考サイト

[0](https://qiita.com/3Zz8Xx/items/#comments)

コメント一覧へ移動

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2F3Zz8Xx%2Fitems%2F3341a795e1c6fa24bf33&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2F3Zz8Xx%2Fitems%2F3341a795e1c6fa24bf33&realm=qiita)

[19](https://qiita.com/3Zz8Xx/items/3341a795e1c6fa24bf33/likers)

13