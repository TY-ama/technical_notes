>RailsアプリケーションをDockerコンテナで実行するためのDockerfileの例です。

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
