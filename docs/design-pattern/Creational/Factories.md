---
layout: default
title: Factories
nav_order: 1
parent: Creational
grand_parent: Design_pattern
---

### Simple Factory 模式
Simple Factory模式又稱Static Factory模式。一個Simple Factory生產成品，而對客戶端隱藏產品產生的細節，物件如何生成，生成前是否與其它物件建立依賴關係，客戶端皆不用理會，用以將物件生成方式之變化 與客戶端程式碼隔離。


Simple Factory使用靜態方法來簡單地隱藏物件建立細節。撇開靜態方法不談，隱藏物件建立的細節仍是Factory模式的重點，可將這個模式推至極緻，而成為一種通用、專門用來生成物件、建立依賴關係、甚至具備管理物件生命週期職責的輕量級容器。

``` c#
using System;

namespace DotNetDesignPatternDemos.Creational.Factories
{
  public class Point
  {
    private double x, y;
    protected Point(double x, double y)
    {
      this.x = x;
      this.y = y;
    }
    public Point(double a, double b, CoordinateSystem cs = CoordinateSystem.Cartesian)
    {
      switch (cs)
      {
        case CoordinateSystem.Polar:
          x = a * Math.Cos(b);
          y = a * Math.Sin(b);
          break;
        default:
          x = a;
          y = b;
          break;
      }
      // steps to add a new system
      // 1. augment CoordinateSystem
      // 2. change ctor
    }
    // factory property
    public static Point Origin => new Point(0, 0);
    // singleton field
    public static Point Origin2 = new Point(0, 0);
    // factory method
    public static Point NewCartesianPoint(double x, double y)
    {
      return new Point(x, y);
    }
    public static Point NewPolarPoint(double rho, double theta)
    {
      return null;
    }
    public enum CoordinateSystem
    {
      Cartesian,
      Polar
    }
    // make it lazy
    public static class Factory
    {
      public static Point NewCartesianPoint(double x, double y)
      {
        return new Point(x, y);
      }
    }
  }
  class Demo
  {
    static void Main(string[] args)
    {
      var p1 = new Point(2, 3, Point.CoordinateSystem.Cartesian);
      var origin = Point.Origin;
      var p2 = Point.Factory.NewCartesianPoint(1, 2);
    }
  }
}
```

