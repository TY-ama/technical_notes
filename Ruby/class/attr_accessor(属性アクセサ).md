# 属性アクセサ (`attr_accessor`)

## `name` はメソッド名、`@name` はインスタンス変数名

Rubyで `attr_accessor :name` と記述すると、以下の2つのメソッドが自動的に生成されます。

- `name` (ゲッター): インスタンス変数の値を読み取るためのメソッド。
- `name=` (セッター): インスタンス変数の値を変更するためのメソッド。

### 実際には、以下のコードが自動生成されます。

```ruby
def name
  @name
end

def name=(value)
  @name = value
end
```

これにより、クラスの外部から `name = "値"` のように代入できるようになり、内部的には `@name = "値"` としてインスタンス変数に値が設定されます。

### クラスの外部からアクセスする例

```ruby
class BillRecord
  attr_accessor :name, :bill

  def initialize(name, bill)
    @name = name
    @bill = bill
  end
end

data = BillRecord.new("山本", 40000)
data.name = "西山" # セッターメソッド `name=` を使用して値を変更
puts "名前 ：#{data.name}様" # ゲッターメソッド `name` を使用して値を読み取り
puts "預金額：#{data.bill}円"
```