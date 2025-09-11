---
tags:
  - t-notes/Rails
datetimeCreate: 2025-09-11 13:55
description: migrationファイル作成->migration(データベースに実際にカラム追加)->モデルのインスタンスで新しい属性使える(offer.job_id)
---
## migrationファイルを作成すると

### 1. データベースの変更 (マイグレーション)

モデルにカラムを追加するには、まず「マイグレーション」という仕組みを使ってデータベースのテーブル構造を変更します。

**マイグレーションファイルの作成**
ターミナルで以下のコマンドを実行し、マイグレーションファイルを作成します。

```bash
rails generate migration Addカラム名Toテーブル名 カラム名:データ型
```

例えば、`User`モデル（`users`テーブル）に`age`という名前で整数型（`integer`）のカラムを追加する場合は、次のようになります。

```bash
rails generate migration AddAgeToUsers age:integer
```

このコマンドを実行すると、`db/migrate`ディレクトリに`YYYYMMDDHHMMSS_add_age_to_users.rb`のようなファイルが作成されます。 ファイルの中身は次のようになっています。

```ruby
class AddAgeToUsers < ActiveRecord::Migration[7.0]
  def change
    add_column :users, :age, :integer
  end
end
```

**マイグレーションの実行**
次に、以下のコマンドでマイグレーションを実行し、データベースに実際にカラムを追加します。

```bash
rails db:migrate
```

このコマンドにより、`db/schema.rb`ファイルも自動的に更新され、データベースの最新の構造が反映されます。

### 2. モデルへの影響

マイグレーションを実行すると、Active Recordが自動的に新しいカラムを認識します。これにより、モデルのインスタンスで新しい属性が使えるようになります。

```ruby
user = User.new
user.age = 30 # 新しいカラム 'age' に値を設定
user.save

puts user.age # => 30
```

### 3. 既存レコードへの影響

新しくカラムを追加した場合、データベースに既に存在するレコードのそのカラムの値は、デフォルトで`nil`になります。 もし初期値を設定したい場合は、マイグレーションファイルでデフォルト値を指定できます。

```ruby
# デフォルト値を0に設定する例
add_column :users, :age, :integer, default: 0
```

ただし、既存のレコードにはデフォルト値を適用せず、今後作成されるレコードにのみデフォルト値を設定したい、といった特殊なケースでは注意が必要です。

### まとめ

Railsのモデルにカラムを追加するプロセスは、以下の流れになります。

1.  `rails generate migration`コマンドでマイグレーションファイルを作成する。
2.  `rails db:migrate`コマンドを実行して、データベースにカラムを追加する。
3.  モデルで新しい属性が自動的に利用可能になる。
4.  必要に応じて、コントローラのStrong Parametersとビューのフォームを更新する。

このマイグレーションという仕組みにより、データベースの構造変更を安全かつ体系的に管理することができます。



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