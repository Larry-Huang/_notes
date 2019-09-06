---
layout: default
title: Observer
nav_order: 5
parent: Behavioral
grand_parent: Design_pattern
---

## Observer Pattern
在軟件構建過程中，我們需要為某些對象建立一種“通知依賴關係”——一個對象（目標對象）的狀態發生改變，所有的依賴對象（觀察者對象）都將得到通知。如果這樣的依賴關係過於緊密，將使軟件不能很好地抵禦變化
使用面向對象技術，可以將這種依賴關係弱化，並形成一種穩定的依賴關係。從而實現軟件體系結構的松耦合。

**定義對象間的一種一對多的依賴關係，以便當一個對象的狀態發生改變時，所有依賴於它的對像都得到通知並自動更新。——《設計模式》GoF**

![](https://images2017.cnblogs.com/blog/1048776/201711/1048776-20171130133356354-676034245.png)

1. 抽象主題角色（Subject）：抽象主題把所有觀察者對象的引用保存在一個列表中，並提供增加和刪除觀察者對象的操作，抽象主題角色又叫做抽像被觀察者角色，一般由抽像類或接口實現。
2. 抽象觀察者角色（Observer）：為所有具體觀察者定義一個接口，在得到主題通知時更新自己，一般由抽像類或接口實現。
3. 具體主題角色（ConcreteSubject）：實現抽象主題接口，具體主題角色又叫做具體被觀察者角色。
4. 具體觀察者角色（ConcreteObserver）：實現抽象觀察者角色所要求的接口，以便使自身狀態與主題的狀態相協調。

``` c#
namespace Observation
{
    //抽象主題角色（Subject）
    public abstract class BankMessageSystem
    {
        //觀察者列表
        protected IList<Depositor> _observers;
        protected BankMessageSystem(IList<Depositor> observers)
        {
            _observers = observers;
        }
        // 增加預約儲戶
        public abstract  void Add(Depositor depositor);
  
          // 刪除預約儲戶
        public  abstract  void Delete(Depositor depositor);
        public void Notify()
        {
            foreach (var depositor in _observers)
            {
                if (depositor.IsChange)
                {
                    depositor.Update(depositor.Balance, depositor.OperationDateTime);
                    depositor.IsChange = false;
                }
            }
        }
    }
    //抽象觀察者角色（Observer）
    public abstract  class Depositor{
        private string _name;

        public  string Name
        {
            get { return _name; }
            private set { _name = value; }
        }
        private int _balance;

        public int Balance
        {
            get { return _balance; }
        }
        private bool _isChange;
        public bool IsChange
        {
            get { return _isChange; }
            set { _isChange = value; }
        }
        public DateTime OperationDateTime { get; set; }
        public Depositor(string name, int total)
        {
            this._name = name;
            this._balance = total;
            this._isChange = false;
        }
        public void GetMoney(int num)
        {
            if (num <= this._balance && num > 0 )
               {
                    this ._balance = this ._balance - num;
                    this.IsChange = true ;
                    OperationDateTime = DateTime.Now;
              }
        }
        public abstract void Update(int currentBalance, DateTime dateTime);
    }
    //具體主題角色（ConcreteSubject）
    public sealed class BenBankMessageSystem : BankMessageSystem
    {
        public BenBankMessageSystem(IList<Depositor> observers) : base(observers) { }
        public override void Add(Depositor depositor)
        {
            _observers.Add(depositor);
        }

        public override void Delete(Depositor depositor)
        {
            _observers.Remove(depositor);
        }

    }
    //具體觀察者角色（ConcreteObserver）
    public sealed class BenDepositor : Depositor
    {
        public BenDepositor (string name, int total) : base (name, total) { }
        public override void Update(int currentBalance, DateTime dateTime)
        {
            Console.WriteLine(Name + " :賬戶發生了變化，變化時間是" + dateTime.ToString() + " ,當前餘額是" + currentBalance.ToString());
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Depositor huangFeiHong = new BenDepositor(" 黃飛鴻", 3000);
            Depositor fangShiYu = new BenDepositor(" 方世玉", 1300);
            Depositor hongXiGuan = new BenDepositor(" 洪熙官", 2500);
            BankMessageSystem beijingBankMessageSystem = new BenBankMessageSystem(new List<Depositor>());
            beijingBankMessageSystem.Add(huangFeiHong);
            beijingBankMessageSystem.Add(fangShiYu);
            beijingBankMessageSystem.Add(hongXiGuan);
            huangFeiHong.GetMoney(100);
            beijingBankMessageSystem.Notify();

            huangFeiHong.GetMoney(200);
            fangShiYu.GetMoney(200);
            beijingBankMessageSystem.Notify();
            Console.ReadKey();
        }
    }
}

```

- 使用面向對象的抽象，<font color="red">Observer模式使得我們可以獨立地改變目標與觀察者（面向對像中的改變不是指改代碼，而是指擴展、子類化、實現接口），從而使二者之間的依賴關係達致松耦合。</font>

- 目標發送通知時，無需指定觀察者，通知（可以攜帶通知信息作為參數）會自動傳播。觀察者自己決定是否需要訂閱通知，目標對像對此一無所知。
- 在C#的event中，委託充當了抽象的Observer接口，而提供事件的對象充當了目標對象。委託是比抽象Observer接口更為松耦合的設計。

### 觀察者模式的優點：

1. 觀察者模式實現了表示層和數據邏輯層的分離，並定義了穩定的更新消息傳遞機制，並抽象了更新接口，使得可以有各種各樣不同的表示層，即觀察者。
2. 觀察者模式在被觀察者和觀察者之間建立了一個抽象的耦合，被觀察者並不知道任何一個具體的觀察者，只是保存著抽象觀察者的列表，每個具體觀察者都符合一個抽象觀察者的接口。
3. 觀察者模式支持廣播通信。被觀察者會向所有的註冊過的觀察者發出通知。

### 觀察者模式的缺點：
1. 如果一個被觀察者有很多直接和間接的觀察者時，將所有的觀察者都通知到會花費很多時間。
2. 雖然觀察者模式可以隨時使觀察者知道所觀察的對象發送了變化，但是觀察者模式沒有相應的機制使觀察者知道所觀察的對像是怎樣發生變化的。
3. 如果在被觀察者之間有循環依賴的話，被觀察者會觸發它們之間進行循環調用，導致系統崩潰，在使用觀察者模式應特別注意這點。

