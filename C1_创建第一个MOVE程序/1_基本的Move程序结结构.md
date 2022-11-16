
# 函数（Function）

### 练习答案 
修改下面代码的函数名，使其可编译。

```
fun plus_one(a: u64): u64 {
  a + 1
}
```
# 打印（Print）
### 练习答案 
运行以下代码，观察控制台输出。

```
// import the Debug module from stdlib, which include print function
use Std::Debug::print;

fun print_to_console() {
  // print literal
  print(&100);

  // print expression
  print(&(100 + 3));

  let a: u64 = 123;
  // print variable
  print(&a);
}
```
# 模块（Modules）和 脚本（Scripts）
### 练习答案 
运行下面的代码，它会执行plus_one(99)。检查控制台输出，它会输出结果100。
```
script {
  // first part: dependency declaration
  use Std::Debug::print;

  // second part: constants definition
  const ONE: u64 = 1;

  // last part: the only function with any name
  fun plus_one(x: u64) {
    let sum = x + ONE;
    print(&sum);
  }
}
```
## 模块（Modules）
### 练习答案 
修改下面代码使其可编译。
```
module 0x1::BasicCoin {
    struct Coin has key {
        value: u64,
    }

    public fun mint(account: signer, value: u64) {
        move_to(&account, Coin { value })
    }
}
```
## 调用模块函数
### 练习答案
在源文件math.move中，我们给模块Math添加了若干函数。修改下面的代码，使其能够打印出两个整数的商和余数。
```
module 0xCAFE::Math {
    // calculate the quotient x/y
    public fun quo(x: u64, y: u64): u64 {
        assert!(y != 0, 101);
        x / y
    }

    // calculate the remainder x%y
    public fun rem(x: u64, y: u64): u64 {
        assert!(y != 0, 102);
        x % y
    }
}
```

