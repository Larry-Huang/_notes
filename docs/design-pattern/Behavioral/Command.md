---
layout: default
title: Command
nav_order: 4
parent: Behavioral
grand_parent: Design_pattern
---

## Command Pattern

 在軟件構建過程中，“行為請求者”與“行為實現者”通常呈現一種“緊耦合”。但在某些場合——比如需要對行為進行“記錄、撤銷/重做（undo/redo）、事務”等處理，這種無法抵禦變化的緊耦合是不合適的。在這種情況下，如何將“行為請求者”與“行為實現者”解耦？將一組行為抽象為對象，可以實現二者之間的松耦合。

**將一個請求封裝為一個對象，從而使你可用不同的請求對客戶（客戶程序，也是行為的請求者）進行參數化；對請求排隊或記錄請求日誌，以及支持可撤銷的操作。——《設計模式》GoF**

![](https://images2017.cnblogs.com/blog/1048776/201711/1048776-20171121151712618-219632155.png)

### 從命令模式的結構圖可以看出，它涉及到五個角色，它們分別是：
1. 客戶角色（Client）：創建具體的命令對象，並且設置命令對象的接收者。注意這個不是我們常規意義上的客戶端，而是在組裝命令對象和接收者，或許，把這個Client稱為裝配者會更好理解，因為真正使用命令的客戶端是從Invoker來觸發執行。
   
2. 命令角色（Command）：聲明了一個給所有具體命令類實現的抽象接口。
   
3. 具體命令角色（ConcreteCommand）：命令接口實現對象，是“虛”的實現；通常會持有接收者，並調用接收者的功能來完成命令要執行的操作。
   
4. 請求者角色（Invoker）：要求命令對象執行請求，通常會持有命令對象，可以持有很多的命令對象。這個是客戶端真正觸發命令並要求命令執行相應操作的地方，也就是說相當於使用命令對象的入口。
   
5. 接受者角色（Receiver）：接收者，真正執行命令的對象。任何類都可能成為一個接收者，只要它能夠實現命令要求實現的相應功能。

``` c#
namespace DesignPatterns
{
  public class BankAccount
  {
    private int balance;
    private int overdraftLimit = -500;
    public void Deposit(int amount)
    {
      balance += amount;
      WriteLine($"Deposited ${amount}, balance is now {balance}");
    }
    public bool Withdraw(int amount)
    {
      if (balance - amount >= overdraftLimit)
      {
        balance -= amount;
        WriteLine($"Withdrew ${amount}, balance is now {balance}");
        return true;
      }
      return false;
    }
    public override string ToString()
    {
      return $"{nameof(balance)}: {balance}";
    }
  }
  public interface ICommand
  {
    void Call();
    void Undo();
  }
  public class BankAccountCommand : ICommand
  {
    private BankAccount account;
    public enum Action
    {
      Deposit, Withdraw
    }
    private Action action;
    private int amount;
    private bool succeeded;
    public BankAccountCommand(BankAccount account, Action action, int amount)
    {
      this.account = account ?? throw new ArgumentNullException(paramName: nameof(account));
      this.action = action;
      this.amount = amount;
    }
    public void Call()
    {
      switch (action)
      {
        case Action.Deposit:
          account.Deposit(amount);
          succeeded = true;
          break;
        case Action.Withdraw:
          succeeded = account.Withdraw(amount);
          break;
        default:
          throw new ArgumentOutOfRangeException();
      }
    }
    public void Undo()
    {
      if (!succeeded) return;
      switch (action)
      {
        case Action.Deposit:
          account.Withdraw(amount);
          break;
        case Action.Withdraw:
          account.Deposit(amount);
          break;
        default:
          throw new ArgumentOutOfRangeException();
      }
    }
  }

  class Demo
  {
    static void Main(string[] args)
    {
      var ba = new BankAccount();
      var commands = new List<BankAccountCommand>
      {
        new BankAccountCommand(ba, BankAccountCommand.Action.Deposit, 100),
        new BankAccountCommand(ba, BankAccountCommand.Action.Withdraw, 1000)
      };

      WriteLine(ba);
      foreach (var c in commands)
        c.Call();
      WriteLine(ba);
      foreach (var c in Enumerable.Reverse(commands))
        c.Undo();
      WriteLine(ba);
    }
  }
}
```

