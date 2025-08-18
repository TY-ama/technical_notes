```ruby
wdays = ["月", "火", "水", "木", "金"]

# wdays.each do |wday| の意味：
# 1回目のループでは wday に "月" が入る
# 2回目のループでは wday に "火" が入る
# ...という処理を配列の要素がなくなるまで繰り返す

wdays.each do |wday|
  # ここで wday には配列の要素が順番に入っている
  puts wday + "曜日" # 「月曜日」「火曜日」...と表示される
end


(1..10).each do |cnt|
  # cnt には 1, 2, 3, ..., 10 が順番に入る

  # cnt.odd? は、cntが奇数(odd)ならtrueを返す
  # 奇数なら、nextでこの回の処理をスキップして次のループに進む
  next if cnt.odd?

  # この行にたどり着くのは偶数の時だけ
  puts cnt.to_s + "回目のループ"
end
```

## do～endで囲まれた範囲＝ブロック＝メソッドの引数
