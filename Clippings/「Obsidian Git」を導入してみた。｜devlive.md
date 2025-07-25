---
title: 「Obsidian Git」を導入してみた。｜devlive
source: https://note.com/devlive/n/n5d22e9e74641
author:
  - "[[devlive]]"
published: 2024-06-13
created: 2025-07-25
description: ども！devlive です 👨🏽‍💻 今日は Obsidian に Git を導入してみたいと思います。   📋 作業の流れ  事前準備    Git インストール。ローカルに Git の環境が必要になります ☝🏻    Obsidian 用のリポジトリを Github 上に用意しておきます。    Vault  を Git 管理下にしておきます。    では始めます。  1. 「Obsidian Git」をインストール  「cmd+,」で設定用パネルを開いた後、 Community Plugins > Browse > Git と入力すると以下のプラグインが表示
tags:
  - t-notes/clippings
  - t-notes/Obsidian
---
![見出し画像](https://assets.st-note.com/production/uploads/images/143874551/rectangle_large_type_2_201fa1b7f5a50ddd986e2731acbf6c10.png?width=1200)

## 「Obsidian Git」を導入してみた。

[devlive](https://note.com/devlive)

ども！devlive です 👨🏽💻  
今日は Obsidian に Git を導入してみたいと思います。

## 📋 作業の流れ

### 事前準備

1. Git インストール。ローカルに Git の環境が必要になります ☝🏻
2. Obsidian 用のリポジトリを Github 上に用意しておきます。
3. Vault を Git 管理下にしておきます。

では始めます。

### 1\. 「Obsidian Git」をインストール

「cmd+,」で設定用パネルを開いた後、  
Community Plugins > Browse > Git と入力すると以下のプラグインが表示されますので、選択して Install, 有効化します。

![画像](https://assets.st-note.com/img/1718234141072-4VEzqa0wTB.png?width=1200)

設定 > コミュニティプラグイン > Obsidian Git

### 3\. Obsidian Git の設定

左のタブから Git を選択し Obsidian Git の設定画面を開きます。  
以下の画面が表示されればOKです。

![画像](https://assets.st-note.com/img/1718235273639-Pt0Uqy1UNx.png?width=1200)

Obsidian Git デフォルトの画面

💡 以下の画面が表示された場合はリポジトリが読み込めていないので、リポジトリのセットアップを済ませてから、 Obisidan の再起動して確認してみてください。☝🏻

![画像](https://assets.st-note.com/img/1718235985625-q6pmi2Z601.png?width=1200)

では設定していきます。  
画面上から以下の内容を登録すると使い始めることができます。

**↓ Obsidian Git バックアップ設定**

- Split automatic commit and push … 自動コミット・プッシュを有効にします。
- Vault commit intervale … コミット実行間隔を指定します。
- Vault push interval … プッシュの実行間隔を指定します。
- Auto pull interval … プル実行間隔を指定します。
![画像](https://assets.st-note.com/img/1718235306506-GCqFnAkciy.png?width=1200)

Obsidian Git の設定内容

※ 初回の実行時のみ、リモートホスト、ブランチの入力が求められます。  
選択すると自動コミット&プッシュが開始されます 🔥

![画像](https://assets.st-note.com/img/1718236168668-waqP8ssxeM.png?width=1200)

リモートホストの選択

![画像](https://assets.st-note.com/img/1718236205982-gsexbkKcc0.png?width=1200)

ブランチの選択

github 上でこんな感じでコミットログが生成されていれば成功です！

![[Pasted image 20250725194004.png]]
![画像](https://assets.st-note.com/img/1718236275399-NdBA1C5mQ5.png?width=1200)
![[Pasted image 20250725194004.png]]
以上となります！  
他にもノートの編集をストップして ◯分後にコミットとかも出来るみたいなので、興味のある方は試してみてくださいね〜 😎

それでは！  

[**Obsidianで“育てる”最強ノート術 —— あらゆる情報をつなげて整理しよう** *amzn.to*](https://amzn.to/4jBuNAa)

[*2,200 円* (2025年02月04日 19:21時点 詳しくはこちら)](https://amzn.to/4jBuNAa)

[

Amazon.co.jpで購入する

](https://amzn.to/4jBuNAa)

[**Obsidianでつなげる情報管理術【完成版】** *amzn.to*](https://amzn.to/4mqtYMa)

[*680 円* (2025年05月12日 09:01時点 詳しくはこちら)](https://amzn.to/4mqtYMa)

[

Amazon.co.jpで購入する

](https://amzn.to/4mqtYMa)

## いいなと思ったら応援しよう！

## コメント

「Obsidian Git」を導入してみた。｜devlive