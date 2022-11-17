### 测验答案
在模块0xCAFE::IMCoin中实现类似 ERC20 的token功能，需要满足以下要求：

1. 完成 issue 函数，其功能是在地址 issuer 下发布一个资源 Coin。
- 发行者（issuer） 必须等于常量 ISSUER，否则以状态码 1001 中止
- 如果 发行者（issuer） 已经有 Coin 资源了，则以状态码 1002 中止
- Coin 资源的 发行量（supply） 和 持有者数量（holders） 的初始值都为 0
2. 完成函数 supply, 返回 ISSUER 地址下的资源 Coin 的 发行量（supply） 值
- 如果 ISSUER 没有资源 Coin, 以状态码 1003 中止
3. 完成函数 register, 其功能是在 account 的地址下发布资源 Balance
- 如果 ISSUER 没有资源 Coin, 以状态码 1003 中止
- 如果 account 已经有 Balance 了, 以状态码 2001 中止
- ISSUER 下的资源 Coin 的 持有者数量（holders） 加 1
- Balance 的初始 value 为 0
4. 完成函数 deposit, 其功能是给 receiver 增加数量为 amount 的余额
- 发行者（issuer） 必须等于常量 ISSUER，否则以状态码 1001 中止
- 增加数量为 amount 的 发行量（supply），并且增加后的 发行量（supply） 不能超过 MAX_SUPPLY，否则以状态码 1004 中止
5. 完成函数 balance, 其功能是返回 addr 地址下的 Balance 的 value
- 如果 addr 没有 Balance , 以状态码 2002 中止
6. 完成函数 transfer, 其功能是从 发送者（sender） 转移数量为 amount 的余额到 接收者（receiver）
- 发送者不能与接收者相同，否则以状态码 2003 中止
- amount 不能超过发送者的余额，否则以状态码 2004 中止

```
module 0xCAFE::IMCoin {
  use Std::Signer;

  const MAX_SUPPLY: u64 = 1024;
  const ISSUER: address = @0xCAFE;

  struct Coin has key {
    supply: u64,
    holders: u64,
    
  }

  struct Balance has key,store{
    value: u64
  }

  // complete this function
  public fun issue(issuer: &signer) {
    let issuer_addr = Signer::address_of(issuer);

    if (exists<Coin>(issuer_addr)){abort 1002}
    else
    {if (issuer_addr == ISSUER)
    {move_to(issuer, Coin { supply:{0},holders:{0} });}
    else
    {abort 1001}

    }
    
  }

  // complete this function
  public fun supply(): u64 acquires Coin {

  if (exists<Coin>(ISSUER)){borrow_global<Coin>(ISSUER).supply}else{
  abort 1003}

    
  }

  public fun holders(): u64 acquires Coin {
    assert!(exists<Coin>(ISSUER), 1003);

    borrow_global<Coin>(ISSUER).holders
  }

  // complete this function
  public fun register(account: &signer) acquires Coin{
  assert!(exists<Coin>(ISSUER), 1003);
  let addr = Signer::address_of(account);
  if (exists<Balance>(addr)){abort 2001}
  else{

  move_to(account, Balance {value:0});

  //holder人数加 1

  let value_ref = &mut borrow_global_mut<Coin>(ISSUER).holders;
  *value_ref = *value_ref +1;
  
  }

    
  }

  // complete this function
  public fun deposit(issuer: &signer, receiver: address, amount: u64) acquires Coin,Balance{
    let issuer_addr = Signer::address_of(issuer);
    if (issuer_addr == ISSUER)
    {
    //把suupy+amount
    //正在把receiver修改为amount
    assert!(exists<Coin>(issuer_addr), 1003);
    let value_supply = &mut borrow_global_mut<Coin>(issuer_addr).supply;

     *value_supply = *value_supply +amount;

     assert!(*value_supply<=MAX_SUPPLY,1004);
      //正在把receiver修改为amount
    assert!(exists<Balance>(receiver), 2002);
    let value_balance = &mut borrow_global_mut<Balance>(receiver).value;

     *value_balance  = *value_balance  +amount;
    
    
    }
    else
    {abort 1001}
    
  
}
  // complete this function
  public fun balance(addr: address): u64 acquires Balance{
    assert!(exists<Balance>(addr), 2002);
    borrow_global<Balance>(addr).value
    
    
  }

  // complete this function
  public fun transfer(sender: &signer, receiver: address, amount: u64) acquires Balance{
    assert!(Signer::address_of(sender)!=receiver,2003);
    sub_balance(Signer::address_of(sender), amount);
    add_balance(receiver, amount);
  }

  fun add_balance(addr: address, amount: u64) acquires Balance {
    assert!(exists<Balance>(addr), 2002);

    let value_ref = &mut borrow_global_mut<Balance>(addr).value;
    *value_ref = *value_ref + amount;
  }

  fun sub_balance(addr: address, amount: u64) acquires Balance {
    assert!(exists<Balance>(addr), 2002);

    let value_ref = &mut borrow_global_mut<Balance>(addr).value;
    assert!(*value_ref >= amount, 2004);
    *value_ref = *value_ref - amount;
  }
}
```
