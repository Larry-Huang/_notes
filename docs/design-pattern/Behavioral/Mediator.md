---
layout: default
title: Mediator
nav_order: 5
parent: Behavioral
grand_parent: Design_pattern
---

## Mediator Pattern

 在軟件構建過程中，經常會出現多個對象互相關聯交互的情況，對象之間常常會維持一種複雜的引用關係，如果遇到一些需求的更改，這種直接的引用關係將面臨不斷地變化。

 **定義了一個中介對象來封裝一系列對象之間的交互關係。中介者使各個對象之間不需要顯式地相互引用，從而使耦合性降低，而且可以獨立地改變它們之間的交互行為。——《設計模式》GoF**

 ![](https://images2018.cnblogs.com/blog/1048776/201712/1048776-20171203153755569-2040623824.png)

可以看出，在中介者模式的結構圖有以下角色：
1. 抽像中介者角色（Mediator）：在裡面定義各個同事之間交互需要的方法，可以是公共的通信方法，也可以是小範圍的交互方法。
2. 具體中介者角色（ConcreteMediator）：它需要了解並維護各個同事對象，並負責具體的協調各同事對象的交互關係。
3. 抽象同事類（Colleague）：通常為抽像類，主要約束同事對象的類型，並實現一些具體同事類之間的公共功能，比如，每個具體同事類都應該知道中介者對象，也就是具體同事類都會持有中介者對象，都可以到這個類裡面。
4. 具體同事類（ConcreteColleague）：實現自己的業務，需要與其他同事通信時候，就與持有的中介者通信，中介者會負責與其他同事類交互。

```c#
namespace 中介者模式的實現
 {
     // 抽像中介者角色
     public  interface Mediator
     {
         void Command(Department department);
     }
 
     // 總經理--相當於具體中介者角色
     public  sealed  class President : Mediator
     {
         // 總經理有各個部門的管理權限
         private Financial _financial;
         private Market _market;
         private Development _development;
 
         public  void SetFinancial(Financial financial)
         {
             this ._financial = financial;
         }
         public  void SetDevelopment(Development development)
         {
             this ._development = development;
         }
         public  void SetMarket(Market market)
         {
             this ._market = market;
         }
 
         public void Command(Department department)
         {
             if (department.GetType() == typeof (Market))
             {
                 _financial.Process();
             }
         }
     }
 
     // 同事類的接口
     public  abstract  class Department
     {
         // 持有中介者(總經理)的引用
         private Mediator _mediator;
 
         protectedDepartment(Mediator mediator)
         {
             this.mediator = _mediator;
         }
         //調用方法
         public Mediator GetMediator
         {
             get { return _mediator; }
             private  set { this ._mediator = value; }
         }
 
         // 做本部門的事情
         public  abstract  void Process();
 
         // 向總經理髮出申請
         public abstract  void Apply();
     }
 
     // 開發部門
     public  sealed  class Development : Department
     {
         public Development(Mediator m) : base (m) { }
 
         public  override  void Process()
         {
             Console. WriteLine( " 我們是開發部門，要進行項目開發，沒錢了，需要資金支持！" );
         }
 
         public  override  voidApply()
         {
             Console.WriteLine( " 專心科研，開發項目！" );
         }
     }
 
     // 財務部門
     public  sealed  class Financial : Department
     {
         public Financial(Mediator m) : base (m ) { }
 
         public  override  void Process()
         {
             Console.WriteLine( " 匯報工作！沒錢了，錢太多了！怎麼花? ");
         }
 
         public  override  void Apply()
         {
             Console.WriteLine( " 數錢！" );
         }
     }
 
     // 市場部門
     public  sealed  class Market : Department
     {
         public Market(Mediator mediator) : base (mediator) { }
 
         public  override  void Process()
        {
             Console.WriteLine( " 匯報工作！項目承接的進度，需要資金支持！" );
             GetMediator.Command( this );
         }
 
         public  override  void Apply()
         {
             Console.WriteLine( " 跑去接項目！" );
         }
     }
 
 
     class Program
     {
         static  void Main(String[] args)
         {
             President mediator = new President();
             Market market = new Market(mediator);
             Development development = new Development(mediator);
             Financial financial = new Financial(mediator);
 
             mediator.SetFinancial(financial);
             mediator.SetDevelopment(development);
             mediator.SetMarket(market);
 
             market.Process();
             market.Apply();
 
             Console.Read();
         }
     }
}
```