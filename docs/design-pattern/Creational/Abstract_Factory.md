---
layout: default
title: Abstract Factory
nav_order: 4
parent: Creational
grand_parent: design_pattern
---

### Abstract Factory 模式
如果您需要一組可以隨時抽換的元件，並且希望可以簡單地 一次抽換，則可以考慮使用Abstract Factory。
![](https://openhome.cc/Gossip/DesignPattern/images/AbstractFactory-1.jpg)

AbstractFactory這個名詞是從的建立可抽換的一組 物件角度來看這個模式，如果將焦點放 在使用抽象工廠物件的方法上，因為方法定義了一個樣版流程，流程中真正需要實際物件運作的部份，則呼叫callback物件（工廠物件）來建立，所以從流 程的觀點來看，又稱之為Template-callback模式。

``` c#
namespace DotNetDesignPatternDemos.Creational.AbstractFactory
{
  public interface IHotDrink
  {
    void Consume();
  }

  internal class Tea : IHotDrink
  {
    public void Consume()
    {
      Console.WriteLine("This tea is nice but I'd prefer it with milk.");
    }
  }
  internal class Coffee : IHotDrink
  {
    public void Consume()
    {
      Console.WriteLine("This coffee is delicious!");
    }
  }
  public interface IHotDrinkFactory
  {
    IHotDrink Prepare(int amount);
  }
  internal class TeaFactory : IHotDrinkFactory
  {
    public IHotDrink Prepare(int amount)
    {
      Console.WriteLine($"Put in tea bag, boil water, pour {amount} ml, add lemon, enjoy!");
      return new Tea();
    }
  }
  internal class CoffeeFactory : IHotDrinkFactory
  {
    public IHotDrink Prepare(int amount)
    {
      Console.WriteLine($"Grind some beans, boil water, pour {amount} ml, add cream and sugar, enjoy!");
      return new Coffee();
    }
  }
```

```c#
  public class HotDrinkMachine
  {
    public enum AvailableDrink // violates open-closed
    {
      Coffee, Tea
    }
    private Dictionary<AvailableDrink, IHotDrinkFactory> factories = 
      new Dictionary<AvailableDrink, IHotDrinkFactory>();
    private List<Tuple<string, IHotDrinkFactory>> namedFactories =
      new List<Tuple<string, IHotDrinkFactory>>();
    public HotDrinkMachine()
    {
      foreach (var t in typeof(HotDrinkMachine).Assembly.GetTypes())
      {
        if (typeof(IHotDrinkFactory).IsAssignableFrom(t) && !t.IsInterface)
        {
          namedFactories.Add(Tuple.Create(
            t.Name.Replace("Factory", string.Empty), (IHotDrinkFactory)Activator.CreateInstance(t)));
        }
      }
    }
    public IHotDrink MakeDrink()
    {
      Console.WriteLine("Available drinks");
      for (var index = 0; index < namedFactories.Count; index++)
      {
        var tuple = namedFactories[index];
        Console.WriteLine($"{index}: {tuple.Item1}");
      }
      while (true)
      {
        string s;
        if ((s = Console.ReadLine()) != null
            && int.TryParse(s, out int i) // c# 7
            && i >= 0
            && i < namedFactories.Count)
        {
          Console.Write("Specify amount: ");
          s = Console.ReadLine();
          if (s != null
              && int.TryParse(s, out int amount)
              && amount > 0)
          {
            return namedFactories[i].Item2.Prepare(amount);
          }
        }
        Console.WriteLine("Incorrect input, try again.");
      }
    }
  }
  class Program
  {
    static void Main(string[] args)
    {
      var machine = new HotDrinkMachine();
      IHotDrink drink = machine.MakeDrink();
      drink.Consume();
    }
  }
}

```