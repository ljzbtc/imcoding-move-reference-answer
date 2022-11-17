# 定义 if/else 条件
### 练习答案
完成以下函数，它应该返回较小的数字。
```
fun smaller(x: u64, y: u64):u64 {
  if (x < y ) {
    x
  } else {
    y
  }
}
```

### 练习答案
修改以下函数，使其打印 10。
```
let x = 25;
let y = 1 ;

let min: u64 = if (x < y) {x} else {y};

let z: u8 = if (min < 25) 10u8 else 12u8;

print(&z);
```

# 结合多个分支条件
### 练习
掷骰子游戏，数字大者获胜，但有一个例外，1大于6。完成下面的函数，如果 x 获胜，则返回 1，如果 y 获胜，则返回 2，平局返回 0。
```
fun dice_compare(x: u8, y: u8): u8 {
  if (x > y) {
    if (x == 6 && y == 1) {
      2
    } else {
   1

    }
  } else if ( x == y) {
    0
  } else {
    if (x == 1 && y==6) {
      1
    } else {
      2
    }
  }
}
```
