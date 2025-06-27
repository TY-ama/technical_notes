# WSL2、VS Code、Dockerの導入手順

## WSL2 (Windows Subsystem for Linux 2) の導入

WSL2を導入するための手順です。

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