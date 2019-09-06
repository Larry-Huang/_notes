---
layout: default
title: Strategy
nav_order: 5
parent: Behavioral
grand_parent: Design_pattern
---

## Strategy Pattern

在軟件構建過程中，某些對象使用的算法可能多種多樣，經常改變，如果將這些算法都編碼到對像中，將會使對像變得異常複雜；而且有時候支持不使用的算法也是一個性能負擔。如何在運行時根據需要透明地更改對象的算法？將算法與對象本身解耦，從而避免上述問題？

**定義一系列算法，把它們一個個封裝起來，並且使它們可互相替換。該模式使得算法可獨立於使用它的客戶而變化。——《設計模式》GoF**

![](https://images2017.cnblogs.com/blog/1048776/201712/1048776-20171218145939990-1116019661.png)

可以看出，在策略模式的結構圖有以下角色：
1. 環境角色（Context）：持有一個Strategy類的引用。
   - **需要使用ConcreteStrategy提供的算法。**
   - **內部維護一個Strategy的實例。**
   - 負責動態設置運行時Strategy具體的實現算法。
   - 負責跟Strategy之間的交互和數據傳遞
2. 抽象策略角色（Strategy）：定義了一個公共接口，各種不同的算法以不同的方式實現這個接口，Context使用這個接口調用不同的算法，一般使用接口或抽像類實現。
3. 具體策略角色（ConcreteStrategy）：實現了Strategy定義的接口，提供具體的算法實現。

```c#
namespace 策略模式的實現
 {
     // 環境角色---相當於Context類型
     public  sealed  class SalaryContext
     {
         private ISalaryStrategy _strategy;
         public SalaryContext(ISalaryStrategy strategy)
         {
             this ._strategy = strategy;
         }
         //共用上下文提供的方法
         public ISalaryStrategy ISalaryStrategy
         {
             get { return _strategy; }
             set { _strategy = value; }
         }
         public void GetSalary( double income)
         {
             _strategy.CalculateSalary(income);
         }
     }
     // 抽象策略角色---相當於Strategy類型
     public  interface ISalaryStrategy
     {
         // 工資計算
         void CalculateSalary( double income);
     }
    // 程序員的工資--相當於具體策略角色ConcreteStrategyA 
     public  sealed  class ProgrammerSalary : ISalaryStrategy
     {
         public  void CalculateSalary( double income)
         {
             Console.WriteLine( " 我的工資是：基本工資( " + income + " )底薪( " + 8000 + " )+加班費+項目獎金（10%）" );
         }
     }
     // 普通員工的工資---相當於具體策略角色ConcreteStrategyB 
    public  sealed  class NormalPeopleSalary : ISalaryStrategy
     {
         public  void CalculateSalary( double income)
         {
             Console.WriteLine( " 我的工資是：基本工資( " + income + " )底薪(3000)+加班費" );
         }
     }
     // CEO的工資---相當於具體策略角色ConcreteStrategyC 
     public  sealed  class CEOSalary : ISalaryStrategy
     {
         public void CalculateSalary( double income)
         {
             Console.WriteLine( " 我的工資是：基本工資( " + income + " )底薪(20000)+項目獎金(20%)+公司股票" );
         }
     }
 
     public  class Client
     {
         public  static  void Main(String[] args)
         {
             // 普通員工的工資
             SalaryContext context = new SalaryContext(new NormalPeopleSalary());
             context.GetSalary( 3000 );
 
             // CEO的工資
             context.ISalaryStrategy = new CEOSalary();
             context.GetSalary( 6000 );
 
             Console.Read();
         }
     }
}
```