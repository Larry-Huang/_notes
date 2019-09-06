---
layout: default
title: Proxy
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---
## Proxy
在面向對象系統中，有些對象由於某種原因（比如對象創建的開銷很大，或者某些操作需要安全控制，或者需要進程外的訪問等） ，直接訪問會給使用者、或者係統結構帶來很多麻煩。如何在不失去透明操作對象的同時來管理/控制這些對象特有的複雜性？增加一層間接層是軟件開發中常見的解決方式。

**為其他對象提供一種代理以控制對這個對象的訪問。——《設計模式》GoF**

### 代理模式按照使用目的可以分為以下幾種：
1. 遠程（Remote）代理：為一個位於不同的地址空間的對象提供一個局域代表對象。這個不同的地址空間可以是本電腦中，也可以在另一台電腦中。最典型的例子就是——客戶端調用Web服務或WCF服務。
2. 虛擬（Virtual）代理：根據需要創建一個資源消耗較大的對象，使得對像只在需要時才會被真正創建。
3. Copy-on-Write代理：虛擬代理的一種，把複製（或者叫克隆）拖延到只有在客戶端需要時，才真正採取行動。
4. 保護（Protect or Access）代理：控制一個對象的訪問，可以給不同的用戶提供不同級別的使用權限。
5. 防火牆（Firewall）代理：保護目標不讓惡意用戶接近。
6. 智能引用（Smart Reference）代理：當一個對像被引用時，提供一些額外的操作，比如將對此對象調用的次數
7. Cache代理：為某一個目標操作的結果提供臨時的存儲空間，以便多個客戶端可以這些結果。

在上面所有種類的代理模式中，虛擬代理、遠程代理、智能引用代理和保護代理較為常見的代理模式。

### Protection Proxy
``` c#
namespace DotNetDesignPatternDemos.Structural.Proxy.Protection
{
  public interface ICar
  {
    void Drive();
  }
  public class Car : ICar
  {
    public void Drive()
    {
      WriteLine("Car being driven");
    }
  }
  public class CarProxy : ICar
  {
    private Car car = new Car();
    private Driver driver;
    public CarProxy(Driver driver)
    {
      this.driver = driver;
    }
    public void Drive()
    {
      if (driver.Age >= 16)
        car.Drive();
      else
      {
        WriteLine("Driver too young");
      }
    }
  }

  public class Driver
  {
    public int Age { get; set; }
    public Driver(int age)
    {
      Age = age;
    }
  }
  public class Demo
  {
    static void Main(string[] args)
    {
      ICar car = new CarProxy(new Driver(12)); // 22
      car.Drive();
    }
  }
}
```

