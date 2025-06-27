## WSL導入

- 設定→オプション機能→windowsのその他の機能
	- Linux用window用windowsサブシステムON
	- 仮想マシンプラットフォーム→Virtual machine platformをON

- https://aka.ms/wsl2kernel　から「x64マシン用WSL2 Linuxカーネル更新プログラムパッケージ」をダウンロード
	- インストーラを起動しNextを押してインストール
	- うまくいかなくてもubuntuインストールした後に実行するか聞かれる

- WSL2を既定のバージョンで利用できるようにする
	- コマンドプロンプトで「wsl --set-default-version 2」を打つ

- ubuntuをMicrosoft Storeから入れる
	- ユーザー作成、パスワード設定

- [基本的なWSL導入](https://www.kagoya.jp/howto/it-glossary/develop/wsl2_linux/)
- [オプション機能について](https://dev.classmethod.jp/articles/how-to-setup-wsl2-for-windows11/)

- **Linuxカーネル更新プログラムインストール後「wsl --install」でもいい
	- 「Linux用Windowsサブシステム（WSL）」と「仮想マシン プラットフォーム」を有効化する
	- WSL2用Linuxカーネルをダウンロード・インストールする
	- WSL2を既定のバージョンとする
	- Ubuntuディストリビューションをダウンロード・インストールする

## VScode連携
