# 标准库中的signer操作符
### 练习答案
更改下面的脚本，如果脚本不是从 @0x42 和 @0xCAFE 发送的，则将以状态码 100 中止。
```
script {
  use Std::Debug::print;
   use Std::Signer;

  fun multi_sign_act(acc1: signer, acc2: signer) {
    // check the senders

    let con1:bool = (Signer::address_of(&acc1) == @0x42 && Signer::address_of(&acc2) == @0xCAFE);
    let con2:bool = (Signer::address_of(&acc2) == @0x42 && Signer::address_of(&acc1) == @0xCAFE);
    assert!(con1 || con2,100);
    

    // just print the two sender
    print(&acc1);
    print(&acc2);
  }
}
```
