---
layout: default
title: Bridge
nav_order: 1
parent: Structural
grand_parent: design_pattern
---

### Bridge模式
Bridge模式的目的，在於將抽象與實現分離，使兩者都可以獨立地演 化。這邊所謂的抽象，指的是指應用程式行為定義的演化，而實現指的是應用程式實作時，所需使用的特定API或平台。
以UML來表示Bridge模式的結構：

![](https://openhome.cc/Gossip/DesignPattern/images/Bridge-4.jpg)

簡單地說，Bridge模式的重點在於，對Abstraction的實作，不應該依賴於特定API或平台，應 辨識出Implementor，透過Implementor來橋接特定API或平台實現。

``` c#
using Autofac;
using System;
//using static System.Console;

namespace DotNetDesignPatternDemos.Structural.Bridge
{
  public interface IRenderer
  {
    void RenderCircle(float radius);
  }
  public class VectorRenderer : IRenderer
  {
    public void RenderCircle(float radius)
    {
      Console.WriteLine(string.Format("Drawing a circle of radius {radius}"));
    }
  }
  public class RasterRenderer : IRenderer
  {
    public void RenderCircle(float radius)
    {
      Console.WriteLine(string.Format("Drawing pixels for circle of radius {radius}"));
    }
  }
  public abstract class Shape
  {
    protected IRenderer renderer;

    // a bridge between the shape that's being drawn an
    // the component which actually draws it
    public Shape(IRenderer renderer)
    {
      this.renderer = renderer;
    }
    public abstract void Draw();
    public abstract void Resize(float factor);
  }

  public class Circle : Shape
  {
    private float radius;
    public Circle(IRenderer renderer, float radius) : base(renderer)
    {
      this.radius = radius;
    }
    public override void Draw()
    {
      renderer.RenderCircle(radius);
    }
    public override void Resize(float factor)
    {
      radius *= factor;
    }
  }
  public class Demo
  {
    static void Main(string[] args)
    {
      //var raster = new RasterRenderer();
      //var vector = new VectorRenderer();
      //var circle = new Circle(vector, 5, 5, 5);
      //circle.Draw();
      //circle.Resize(2);
      //circle.Draw();
      var cb = new ContainerBuilder();
      cb.RegisterType<VectorRenderer>().As<IRenderer>();
      cb.Register((c, p) => new Circle(c.Resolve<IRenderer>(),
        p.Positional<float>(0)));
      using (var c = cb.Build())
      {
        var circle = c.Resolve<Circle>(
          new PositionalParameter(0, 5.0f)
        );
        circle.Draw();
        circle.Resize(2);
        circle.Draw();
      }
    }
  }
}

```