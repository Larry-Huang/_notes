---
layout: default
title: State
nav_order: 5
parent: Behavioral
grand_parent: Design_pattern
---

## State Pattern

在軟件構建過程中，某些對象的狀態如果改變，其行為也會隨之而發生變化

比如文檔處於只讀狀態，其支持的行為和讀寫狀態支持的行為就可能完全不同。

如何在運行時根據對象的**狀態**來透明地更改對象的行為？而不會為對像操作和狀態轉化之間引入緊耦合？

**允許一個對像在其內部狀態改變時改變它的行為。從而使對像看起來似乎修改了其行為。——《設計模式》GoF**

![](https://images2017.cnblogs.com/blog/1048776/201712/1048776-20171213142649316-954765387.png)

1. 環境角色（Context）：也稱上下文，定義客戶端所感興趣的接口，並且保留一個具體狀態類的實例。這個具體狀態類的實例給出此環境對象的現有狀態。

2. 抽象狀態角色（State）：定義一個接口，用以封裝環境對象的一個特定的狀態所對應的行為。
3. 具體狀態角色（ConcreteState）：每一個具體狀態類都實現了環境（Context）的一個狀態所對應的行為。

在狀態模式結構中需要理解環境類與抽象狀態類的作用：環境類實際上就是擁有狀態的對象，環境類有時候可以充當狀態管理器(State Manager)的角色，可以在環境類中對狀態進行切換操作。   

抽象狀態類可以是抽像類，也可以是接口，不同狀態類就是繼承這個父類的不同子類，狀態類的產生是由於環境類存在多個狀態，同時還滿足兩個條件：這些狀態經常需要切換，在不同的狀態下對象的行為不同。因此可以將不同對像下的行為單獨提取出來封裝在具體的狀態類中，使得環境類對像在其內部狀態改變時可以改變它的行為，對像看起來似乎修改了它的類，而實際上是由於切換到不同的具體狀態類實現的。由於環境類可以設置為任一具體狀態類，因此它針對抽象狀態類進行編程，在程序運行時可以將任一具體狀態類的對象設置到環境類中，從而使得環境類可以改變內部狀態，並且改變行為。

---

狀態模式在我們的現實生活中也有類似的例子，例如：在我們上網購買商品的過程中，就可以查看訂單的隨時狀態。對於商家來說，訂單的狀態不同，也會允許客戶有不同的動作要求，比如：訂單在已經處於發貨狀態，此訂單是不能退貨的。如果訂單在備貨階段，客戶是可以換貨或者退貨的。如果我們的訂單已經發貨了，您就等著接收貨物吧，如果貨物有質量問題，可以拒簽，或者順利完成交易，今天我們就以訂單為例來說明狀態模式的實現。實現代碼如下：

```c#
namespace State
{
     // 環境角色---相當於Context類型
    public  sealed  class Order
    {
        private IState current;
        public Order()
        {
            // 工作狀態初始化為尚無的工作狀態，等待接單中
            current = new WaitForAcceptance();
            IsCancel = false ;
        }
        private  double minute;
        public  double Minute
        {
            get { return minute; }
            set { minute = value; }
        }

        public  bool IsCancel { get ; set ; }
        private  bool finish;
        public  bool TaskFinished
        {
            get { return finish; }
            set { finish =value; }
        }
        public void SetState(IState s)
        {
            current = s;
        }
        public void Action()
        {
            current.Process(this);
        }
    }

    // 抽象狀態角色- --相當於State類型
    public interface IState
    {
        // 處理訂單(環境角色)
        void Process(Order order);
    }

    // 等待受理--相當於具體狀態角色
    public  sealed  class WaitForAcceptance : IState
    {
        public  void Process(Order order)
        {
            System.Console.WriteLine( " 我們開始受理，準備備貨！" );
            if (order.Minute < 30 && order.IsCancel)
            {
                System.Console.WriteLine(" 接受半個小時之內，已取消訂單！" );
                order.SetState( new CancelOrder());
                order.Action();
            }
            order.SetState( new AcceptAndDeliver());
            order.TaskFinished = false ;
            order.Action();
        }
    }

    // 受理發貨---相當於具體狀態角色
    public  sealed  class AcceptAndDeliver : IState
    {
        public  void Process(Order order)
        {
            System.Console.WriteLine("我們貨物已經準備好，可以發貨了！");
            if (order.Minute < 30 && order.IsCancel )
            {
                System.Console.WriteLine("接受半個小時之內，不可以取消訂單！");
            }
            if (order.TaskFinished == false)
            {
                order.SetState(new ConfirmationReceipt());
                order.Action();
            }
        }
    }
    // 確認收貨---相當於具體狀態角色
    public sealed class ConfirmationReceipt : IState
    {
        public void Process(Order order)
        {
            System.Console.WriteLine( " 檢查貨物，沒問題可以就可以簽收！" );
                order.SetState( new Success());
                order.TaskFinished = false ;
                order.Action();
        }
    }
    // 交易成功---相當於具體狀態角色
    public  sealed  class Success : IState
    {
        public  void Process(Order order)
        {
        System.Console.WriteLine("訂單結算");
            order.TaskFinished = true ;
        }
    }
    // 取消訂單---相當於具體狀態角色
    public  sealed  class CancelOrder : IState
    {
        public  void Process (Order order)
        {
            System.Console.WriteLine( "檢查貨物，有問題，取消訂單！" );
            order.TaskFinished =true ;
            Console.WriteLine("----------");
        }
    }

    public  class Client
    {
        public  static  void Main(String[] args)
        {
            //訂單
            Order order = new Order();
            order.Minute = 9 ;
            //可以取消訂單
            order.IsCancel = true;
            order.Action();
           
            order.Action();
            order.Minute = 33 ;
            order.Action();
            order.Minute = 43 ;
            order.Action();

            Console.Read();
        }
    }
}

```