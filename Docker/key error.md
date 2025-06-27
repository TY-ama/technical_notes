# Docker Compose: `key error` の対処法

`docker-compose up -d --build` コマンドは、Docker Composeアプリケーションをバックグラウンドで起動し、必要に応じてイメージを再ビルドします。

`key error` が発生する場合、`docker-compose.yml` ファイルの記述に誤りがあるか、YAMLのインデントが正しくない可能性があります。YAMLファイルはインデントが非常に重要なので、スペースの数やタブの使用に注意してください。