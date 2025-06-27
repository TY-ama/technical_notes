## <u>nameはメソッド名、@nameはインスタンス変数名</u>

## Rubyで `attr_accessor :name` と書くと

- `name`（ゲッター） → 値を読み取る
- `name=`（セッター） → 値を変更する

この二つのメソッドが自動的に作られる

## 実際には

def name

	  @name

end

def name=(value)

	  @name = value

end
#### クラス外で書かれた name = ""を実際に代入できるように
#### @name = ""に置き換える


## 下記のようにクラスの外からアクセスする場合に使う

class BillRecord

    attr_accessor :name, :bill

    def initialize(name, bill)

        @name = name

        @bill = bill

    end

end

data = BillRecord.new("山本", 40000)
#### data.name = "西山"
#### puts "名前  ：#{data.name}様"

#### puts "預金額：#{data.bill}円"