e# UbuntuとGitHubの連携 (SSH接続)

Ubuntu環境からGitHubにSSH接続するための設定手順です。

### 1. SSHキーペアの生成

以下のコマンドを実行して、公開鍵と秘密鍵のペアを生成します。

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

-   `Generating public/private ed25519 key pair.`: キーペアの生成が開始されます。
-   `Enter file in which to save the key (/home/ty09/.ssh/id_ed25519): training`: キーを保存するファイル名を指定します。デフォルトの `id_ed25519` を推奨しますが、ここでは `training` と入力されています。
-   `Enter passphrase (empty for no passphrase):`: 秘密鍵のパスフレーズを入力します。セキュリティのために設定することを推奨します。
-   `Enter same passphrase again:`: パスフレーズを再入力します。

成功すると、以下のようなメッセージが表示されます。

```
Your identification has been saved in training
Your public key has been saved in training.pub
The key fingerprint is:
SHA256:vuX8sFE3bgR0ywZyq3b2GnnEKWHul1GODSqtpukbyYM sa3ku9sa3ku9@gmail.com
The key's randomart image is:
+--[ED25519 256]--+
|          . + .  |
|           + = . |
|            = = .|
|           = * B |
|        S + O X o|
|       + o B O + |
|      E * * + B  |
|         @ + *   |
|       .*.+.o    |
+----[SHA256]-----+
```

### 2. `ssh-agent` に秘密鍵を登録

SSHキーを毎回パスフレーズなしで使用できるように、`ssh-agent` に秘密鍵を登録します。

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519 # もしファイル名を変更した場合は、そのファイル名を指定
```

### 3. 公開鍵をGitHubに登録

生成された公開鍵の内容をGitHubに登録します。

1.  公開鍵の内容を表示します。
    ```bash
    cat ~/.ssh/id_ed25519.pub # もしファイル名を変更した場合は、そのファイル名を指定
    ```
2.  表示された文字列をすべてコピーします。
3.  GitHubにログインし、「Settings」>「SSH and GPG keys」>「New SSH key」に進みます。
4.  「Key」の欄にコピーした公開鍵の文字列を貼り付け、タイトルを付けて保存します。

### 4. 接続確認

GitHubへのSSH接続が正しく設定されているかを確認します。

```bash
ssh -T git@github.com
```

以下のようなメッセージが表示されれば成功です。

```
Hi <ユーザー名>! You've successfully authenticated, but GitHub does not provide shell access.
```