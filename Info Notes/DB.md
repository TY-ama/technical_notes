---
tags:
  - t-notes/DB
datetimeCreate: 2025-07-04 15:36
---
> データを**正しく扱う**仕組み

## 「正しく扱う」とは

- 正しく溜める
	- 温度なら温度のみ、余計なデータがない状態に
	- 列ごとに溜める**データ型**を決めておく
	- 例．
		- 温度と湿度を測っていく
		- 関連性がある ▷ 同じ時間に測った
		- 日時も記録
- 正しく取り出す
	- 必要なデータを必要な分だけ取り出せる
		- 検索・選択
		- ユーザ管理 (見せるデータの選択)

## 用語

- **テーブル**
	- 表全体のこと
	- テーブル構造 = どんなデータを入れるのか
- **レコード**
	- データの１セット (行)
- **カラム**
	- 属性(列)
	- 「名前」「住所」「電話番号」など、分類軸となる
- **フィールド**
	- DBに存在するデータの最小単位
	- フィールドの集合体がカラムやレコード、テーブル
- **選択**
	- 横方向
	- 列を選ぶ
- **検索**
	- 縦方向
	- 行を選ぶ 
- **主キー**
	- ここの値が一つ決まればレコードが一つ決まる
		- (データ行を一意に識別するための項目)
	- 絶対に決めなければいけない
	- 条件
		- 中身が空ではない & 中身が一意である(重複なし)
- **外部キー
	- 関連したテーブル間を結ぶために設定する列のこと
	- 関連するテーブル間の整合性を保ちたい列に設定する
		- 部署テーブルに存在しない部署コードは従業員テーブルに登録させない
		- 従業員テーブルに登録のある値は、部署テーブルから削除させない
		- などの**便利な制約**を設定できる
	- ![[Pasted image 20250829133432.png]]
- **一対多
	- 一方のテーブルのレコード一件に対して、もう一方のテーブルのレコードが複数県対応する




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