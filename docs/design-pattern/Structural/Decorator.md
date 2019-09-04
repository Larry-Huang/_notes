---
layout: default
title: Decorator
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---

文章來源
[Decorator Pattern](https://ithelp.ithome.com.tw/articles/10198235)
Decorator的特性是採用組合、而非繼承，實現運行時動態地擴展物件功能的能力，而且可以
根據需要擴展多個功能。避免單獨使用繼承而產生靈活性差和"多子類別衍生問題。但它最根本的作用是在於裝飾。

在某一物件動態加上功能。
一層一層的將功能套上去，每一層執行的是不同的物件。

![](https://images2017.cnblogs.com/blog/1048776/201710/1048776-20171024135829098-1962348959.png)
---
### 在裝飾模式中的各個角色有：

1. 抽象構件角色（Component）：給出一個抽象接口，以規範準備接收附加責任的對象。
2. 具體構件角色（Concrete Component）：定義一個將要接收附加責任的類。
3. 裝飾角色（Decorator）：持有一個構件（Component）對象的實例，並實現一個與抽象構件接口一致的接口。
4. 具體裝飾角色（Concrete Decorator）：負責給構件對象添加上附加的責任。

動態地給一個對象增加一些額外的職責。就增加功能而言，Decorator模式比生成子類更為靈活。—— 《設計模式》GoF 

``` c#
namespace 裝飾模式的實現
 {
     ///  <summary> 
     /// 該抽像類就是房子抽象接口的定義，該類型就相當於是Component類型，是餃子餡，需要裝飾的，需要包裝的
     / //  </summary> 
     public  abstract  class IHouse
     {
         // 房子的裝修方法--該操作相當於Component類型的Operation方法
         public  abstract  void Renovation();
     }
 
     ///  <summary> 
     /// 該抽像類就是裝飾接口的定義，該類型就相當於是Decorator類型，如果需要具體的功能，可以子類化該類型
     ///  </summary>
     public  abstract  class IDecorationStrategy : IHouse // 關鍵點之二，體現關係為Is-a，有這這個關係，裝飾的類也可以繼續裝飾了
     {
        // 通過組合方式引用Decorator類型，該類型實施具體功能的增加
         // 這是關鍵點之一，包含關係，體現為Has-a 
         protected IHouse _house;
         // 通過構造器注入，初始化平台實現
         protected IDecorationStrategy(IHouse house)
         {
            this._house= house;
         }
 
        // 該方法就相當於Decorator類型的Operation方法
        public override  void Renovation()
        {
            if ( this._house!= null )
             {
                 this._house.Renovation();
             }
         }
     }
  
     ///  <summary> 
     /// PatrickLiu的房子，我要按我的要求做房子，相當於ConcreteComponent類型
     ///  </summary> 
     public  sealed  class PatrickLiuHouse:IHouse
     {
         public override  void Renovation()
         {
             Console.WriteLine( "裝修PatrickLiu的房子" );
         }
     }

    ///  <summary> 
     /// 具有安全功能的設備，可以提供監視和報警功能，相當於ConcreteDecoratorA類型
     ///  </summary> 
     public  sealed  class HouseSecurityDecorator : IDecorationStrategy
     {
        public HouseSecurityDecorator(IHouse house): base (house){}
 
        public override  void Renovation()
         {
             base .Renovation();
             Console.WriteLine( " 增加安全系統" );
         }
     }
  
     ///  <summary> 
     /// 具有保溫接口的材料，提供保溫功能，相當於ConcreteDecoratorB類型
     ///  </summary> 
     public sealed class KeepWarmDecorator:IDecorationStrategy
     {
         public KeepWarmDecorator(IHouse house): base (house){}
 
         public override  void Renovation()
         {
             base .Renovation();
             Console.WriteLine( " 增加保溫的功能" );
         }
     }
 
    public  class Program
    {
       static void Main()
       {
          // 這就是我們的餃子餡，需要裝飾的房子
          IHouse myselfHouse= new PatrickLiuHouse();
 
         IDecorationStrategy securityHouse= new HouseSecurityDecorator(myselfHouse);
          // 房子就有了安全系統了
          securityHouse.Renovation();

          // 如果我既要安全系統又要保暖呢，繼續裝飾就行
          IDecorationStrategy securityAndWarmHouse= new HouseSecurityDecorator(securityHouse);
          securityAndWarmHouse.Renovation();
       }
    }
}
```