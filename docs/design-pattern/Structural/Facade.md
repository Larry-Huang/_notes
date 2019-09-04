---
layout: default
title: Facade
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---

## Facade 模式
在軟件系統開發的過程中，當組件的客戶（即外部接口，或客戶程序）和組件中各種複雜的子系統有了過多的耦合，隨著外部客戶程序和各子系統的演化，這種過多的耦合面臨很多變化的挑戰。如何簡化外部客戶程序和系統間的交互接口？如何將外部客戶程序的演化和內部子系統的變化之間的依賴相互解耦？[文章來源](https://www.cnblogs.com/PatrickLiu/p/7772184.html)

為子系統中的一組接口提供一個一致的界面，Facade模式定義了一個高層接口，這個接口使得這一子系統更加容易使用。——《設計模式》GoF 

![](https://images2017.cnblogs.com/blog/1048776/201711/1048776-20171102143312091-1683302818.png)

### 案例
#### 購買過程有幾點必須要做的事情：
 1. 身份驗證安全，沒有認證是無效用戶。
 2. 系統安全，檢查系統環境，防止注入、跨站和偽造等攻擊
 3. 網銀安全，檢查付款地址的有效性，檢查網關是否正常

``` c#
namespace 外觀模式的實現
 {
     ///  <summary> 
     /// 不使用外觀模式的情況
     /// 此時客戶端與三個子系統都發送了耦合，使得客戶端程序依賴與子系統
     /// 為了解決這樣的問題，我們可以使用外觀模式來為所有子系統設計一個統一的接口
     /// 客戶端只需要調用外觀類中的方法就可以了，簡化了客戶端的操作
     / // 從而讓客戶和子系統之間避免了緊耦合
     ///  </summary> 
     class Client
     {
         static  void Main( string [] args)
         {
             SystemFacade facade=new SystemFacade();
             facade.Buy();
             Console.Read();
         }
     }
  
     // 身份認證子系統A 
     public  class AuthoriationSystemA
     {
         public  void MethodA()
         {
             Console.WriteLine ( " 執行身份認證" );
         }
     }
  
     // 系統安全子系統B 
     public  classSecuritySystemB
     {
         public  void MethodB()
         {
             Console.WriteLine( " 執行系統安全檢查" );
         }
     }
  
     // 網銀安全子系統C 
     public  class NetBankSystemC
     {
         public  void MethodC()
         {
             Console.WriteLine( " 執行網銀安全檢測" );
         }
    }
 
    // 更高層的Facade 
    public  class SystemFacade
    {
       private AuthoriationSystemA auth;
       private SecuritySystemB security;
       private NetBankSystemC netbank;
 
       public SystemFacade()
       {
          auth= new AuthoriationSystemA();
          security= new SecuritySystemB();
          netbank= new NetBankSystemC();
       }
 
       public  void Buy()
       {
           auth.MethodA(); // 身份認證子系統
           security.MethodB(); // 系統安全子系統
           netbank.MethodC(); // 網銀安全子系統
 
           Console.WriteLine( " 我已經成功購買了！" );
       }
    }
}
```

### 區分Facade模式、Adapter模式、Bridge模式與Decorator模式：

- Facade模式註重簡化接口
- Adapter模式註重轉換接口
- Bridge模式註重分離接口（抽象）與其實現
- Decorator模式註重穩定接口的前提下為對象擴展功能

