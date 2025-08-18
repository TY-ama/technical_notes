---
title: windowsにWSL2+Docker環境を構築する手順
source: https://qiita.com/taka777n/items/ea3a1b3a2802aabf3db2
author:
  - "[[taka777n]]"
published: 2022-04-10
created: 2025-07-22
description: はじめに Docker Desktopの有料化(2022年2月～)に伴い、windowsではDockerの運用に工夫が必要になりました。 Docker Desktopを利用せずにDockerを運用する方法として、windowsにWSL2+Docker環境を構築することが挙...
tags:
  - t-notes/clippings
  - t-notes/Docker/導入
---

## はじめに

[Docker Desktopの有料化(2022年2月～)](https://www.docker.com/blog/updating-product-subscriptions/) に伴い、windowsではDockerの運用に工夫が必要になりました。  
Docker Desktopを利用せずにDockerを運用する方法として、windowsにWSL2+Docker環境を構築することが挙げられます。

windows10にWSL2+Docker環境を構築してみたので、本記事では備忘録として構築手順を残しておきます。

## 環境構築手順

## 1\. WSL2のインストール/セットアップ

自分のPCは `wsl --install` がサポートされていなかったため、古いバージョンの手順となります。  
windows10の新しいバージョンやWindows11ではもう少し簡単な手順で進められます。

[https://docs.microsoft.com/ja-jp/windows/wsl/install#step-4---download-the-linux-kernel-update-package](https://docs.microsoft.com/ja-jp/windows/wsl/install#step-4---download-the-linux-kernel-update-package)

### Windows Subsubsystem for Linux を有効化する

管理者権限でPower Shellを開いて、以下コマンドを実行する。  
[![powershell.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/c7345ec2-fc02-1e66-76a7-e5599f889b17.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2Fc7345ec2-fc02-1e66-76a7-e5599f889b17.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=dc5b5c661fee7939679287f3f7d6fddd)

```text
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### 仮想マシンの機能を有効にする

管理者権限でPower Shellを開いて、以下コマンドを実行する。

```text
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Linux カーネル更新プログラム パッケージをダウンロードする

最新のパッケージをダウンロードします。  
[x64 マシン用 WSL2 Linux カーネル更新プログラム パッケージ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

### WSLの既定のバージョンを 2 に設定する

管理者権限でPower Shellを開いて、以下コマンドを実行する。

```text
wsl --set-default-version 2
```

## 2\. Ubuntuのインストール/セットアップ

### Microsoft Store からUbuntu-20.04 (Linux ディストリビューション)をインストールする

[![ubuntu.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/57607f65-f5e3-7dde-b552-bf43e9d07fa7.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2F57607f65-f5e3-7dde-b552-bf43e9d07fa7.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=047fc0cb24816546e12a4db7048b2302)

### Ubuntu 20.04 を初回起動してユーザー名とパスワードを設定する

[![ubuntu_2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/6806d6b4-2f31-aca4-409e-bfe7ea1cfd3f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2F6806d6b4-2f31-aca4-409e-bfe7ea1cfd3f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=17c75fa171e9d4d837f25bbe1b5c8033)  
[![ubuntu_3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/bf4fbf18-d4fe-8f00-f3fe-0a4a4901e99f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2Fbf4fbf18-d4fe-8f00-f3fe-0a4a4901e99f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b25300623ec387c47a8f910652159a34)

設定したパスワードは、sudoコマンドで利用するので必ず控えておいてください

### Ubuntu 20.04 をアップデートする

```text
$ sudo apt update
$ sudo apt upgrade -y
```

### Docker導入のために必要なパッケージをインストールする

```text
$ sudo apt install curl -y
$ sudo apt install apt-transport-https -y
```

## 3\. Docker Engineのインストール

### Docker 公式 GPG 鍵を追加

### Docker 安定版のレポジトリを追加

### レポジトリをアップデートし、Docker Engine をインストール

```text
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io -y
```

### docker-compose もインストール

```text
$ sudo apt install docker-compose -y
```

### Docker デーモンを起動

```text
$ sudo service docker start
```

環境構築手順としては以上です。

## \[付録\]WSLを利用するにあたっての便利ツール

WSLを利用することになって、 **Windouws Terminal** を使ってみましたが非常に便利でした。  
Windows Terminal は Microsoft から 2020 年にリリースされた新しいターミナルソフトになります。  
各種コマンドラインツール（コマンドプロンプト、Powershell、WSL等）やシェルをタブ化して操作できます。  
[![terminal.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/b7fe2578-b548-5d67-9d16-cd472217b596.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2Fb7fe2578-b548-5d67-9d16-cd472217b596.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=91e439fcdc37b797ace3c7d57df3b929)

Microsoft Store からインストールだけなので、簡単に導入できます。  
[![microstore.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1777029/d1a28735-ff25-e4fb-2582-58ac276d2c2e.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1777029%2Fd1a28735-ff25-e4fb-2582-58ac276d2c2e.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=47cbb42301b3dda60e8bf4f7537fb928)

## おわりに

windows10にWSL2+Docker環境を構築する手順をまとめてみました。  
WSLは便利なので、いろいろと活用していきたいと思います。

## 参考

[0](https://qiita.com/taka777n/items/#comments)

コメント一覧へ移動

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Ftaka777n%2Fitems%2Fea3a1b3a2802aabf3db2&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Ftaka777n%2Fitems%2Fea3a1b3a2802aabf3db2&realm=qiita)

[32](https://qiita.com/taka777n/items/ea3a1b3a2802aabf3db2/likers)

いいねしたユーザー一覧へ移動

44