# `initialize` メソッド

`initialize` メソッドは、インスタンスを生成する際に初期値を設定するために使用されます。

```ruby
def initialize(name, age)
  @name = name
  @age = age
end
```

C言語の構造体と比較すると、以下のようになります。

```c
typedef struct {
  char name[20];
  int age;
} Person;
```

`@name` はインスタンス変数です。 