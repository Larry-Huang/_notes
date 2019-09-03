---
layout: default
title: Composite
nav_order: 1
parent: Structural
grand_parent: design_pattern
---

假設您今天要開發一個動畫編輯程式，動畫由影格（Frame）組成，數個影格組合為動畫清單，動畫清單也可以由其它已完成的動 畫清單組成，也可以在動畫清單與清單之間加入個別影格。

無論是影格或動畫清單都可以播放，而動畫清單負責的就是組合影格或動畫清單，所以可以這麼設計：

![](https://openhome.cc/Gossip/DesignPattern/images/Composite-1.jpg)

以 UML 來表示Composite模式的結構： 
![](https://openhome.cc/Gossip/DesignPattern/images/Composite-2.jpg)

具有層次性或組合性的物件可以使用Composite模式，像是電路元件、視窗元件等，使用Composite模式可以大大減低這些元件設計的複雜度

---

範例:將圓形及方形都 >> GraphicObject
``` C#
namespace DotNetDesignPatternDemos.Structural.Composite.GeometricShapes
{
    //base class
  public class GraphicObject
  {
    public virtual string Name { get; set; } = "Group";
    public string Color;
    private Lazy<List<GraphicObject>> children = new Lazy<List<GraphicObject>>();
    public List<GraphicObject> Children => children.Value;

    private void Print(StringBuilder sb, int depth)
    {
      sb.Append(new string('*', depth))
        .Append(string.IsNullOrWhiteSpace(Color) ? string.Empty : $"{Color} ")
        .AppendLine($"{Name}");
      foreach (var child in Children)
        child.Print(sb, depth + 1);
    }
    public override string ToString()
    {
      var sb = new StringBuilder();
      Print(sb, 0);
      return sb.ToString();
    }
  }  
  public class Circle : GraphicObject
  {
    public override string Name => "Circle";
  }
  public class Square : GraphicObject
  {
    public override string Name => "Square";
  }
  public class Demo
  {
    static void Main(string[] args)
    {
      var drawing = new GraphicObject {Name = "My Drawing"};
      drawing.Children.Add(new Square {Color = "Red"});
      drawing.Children.Add(new Circle{Color="Yellow"});
      
      var group = new GraphicObject();
      group.Children.Add(new Circle{Color="Blue"});
      group.Children.Add(new Square{Color="Blue"});
      drawing.Children.Add(group);

      WriteLine(drawing);
    }
  }
}

```