---
layout: default
title: Composite
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---

## Composite模式
將對象組合成樹形結構以表示“部分-整體”的層次結構。Composite使得用戶對單個對象和組合對象的使用具有一致性。—— 《設計模式》GoF

以 UML 來表示Composite模式的結構： 

![](https://images2017.cnblogs.com/blog/1048776/201710/1048776-20171027143412867-974161708.png)

### 組合模式中涉及到三個角色：
- **抽象構件角色（Component）**：這是一個抽象角色，它給參加組合的對象定義出了公共的接口及默認行為，可以用來管理所有的子對象（在透明式的組合模式是這樣的）。在安全式的組合模式裡，構件角色並不定義出管理子對象的方法，這一定義由樹枝結構對像給出。
- **樹葉構件角色（Leaf）**：樹葉對像是沒有下級子對象的對象，定義出參加組合的原始對象的行為。（原始對象的行為可以理解為沒有容器對像管理子對象的方法，或者【原始對象行為】+【管理子對象的行為（Add，Remove等）】=面對客戶代碼的接口行為集合）
- **樹枝構件角色（Composite）**：代表參加組合的有下級子對象的對象，樹枝對像給出所有管理子對象的方法實現，如Add、Remove等。

組合模式實現的最關鍵的地方是——簡單對象和復合對象必須實現相同的接口。這就是組合模式能夠將組合對象和簡單對象進行一致處理的原因。

``` c#
namespace 安全式的組合模式的實現
 {
     ///  <summary> 
     /// 該抽像類就是文件夾抽象接口的定義，該類型就相當於是抽象構件Component類型
     ///  </summary> 
     public  abstract  class IFolder // 該類型少了容器對像管理子對象的方法的定義，換了地方，在樹枝構件也就是SonFolder類型
     {
         // 打開文件或者文件夾--該操作相當於Component類型的Operation方法
         public  abstract  void Open();
     }

     ///  <summary> 
     /// 該Word文檔類就是葉子構件的定義，該類型就相當於是Leaf類型，不能在包含子對象
     ///  </summary> 
     public  sealed  class Word : IFolder   // 這類型現在很乾淨
     {
         // 打開文件--該操作相當於Component類型的Operation方法
         public  override  void Open()
         {
             Console.WriteLine( " 打開Word文檔，開始進行編輯" );
         }
     }
     ///  <summary> 
     /// SonFolder類型就是樹枝構件，現在由於我們使用的是“安全式”，所以Add ,Remove都是從此處開始定義的
     ///  </summary>
     public  abstract  class ISonFolder : IFolder // 這裡可以是抽象接口，可以自己根據自己的情況而定
     {
         // 增加文件夾或文件
         public  abstract  void Add(Folder folder);
 
         // 刪除文件夾或者文件
         public  abstract  void Remove(Folder folder);
 
         // 打開文件夾--該操作相當於Component類型的Operation方法
         public  override  void Open()
         {
             Console.WriteLine( " 已經打開當前文件夾");
         }
     } 
     ///  <summary> 
     /// NextFolder類型就是樹枝構件的實現類
     ///  </summary> 
     public  sealed  class NextFolder : ISonFolder
     {
         // 增加文件夾或文件
         public  override  void Add(Folder folder)
         {
             Console.WriteLine( " 文件或者文件夾已經增加成功" );
         }
 
         // 刪除文件夾或者文件
         public  override  void Remove(Folder folder)
         {
             Console.WriteLine( " 文件或者文件夾已經刪除成功" );
         }
 
         // 打開文件夾--該操作相當於Component類型的Operation方法
         public  override  void Open()
         {
             Console.WriteLine( " 已經打開當前文件夾" );
         }
     }
     public  class Program
     {
         static  void Main()
         {
             // 這是安全的組合模式
             IFolder myword = new Word();
 
             myword.Open(); // 打開文件，處理文件
 
 
             IFolder myfolder = new NextFolder( );
             myfolder.Open(); // 打開文件夾
 
             // 此處要是用增加和刪除功能，需要轉型的操作，否則不能使用
             ((ISonFolder)myfolder).Add( new NextFolder()) ; // 成功增加文件或者文件夾
            ((ISonFolder)myfolder).Remove( new NextFolder()); // 成功刪除文件或者文件夾
 
             Console.Read();
         }
     }
}
```
``` c#
namespace DotNetDesignPatternDemos.Structural.Composite.GeometricShapes
{
  public abstract class GraphicObject
  {
    public virtual string Name { get; set; } = "Group";
    public string Color;
    private Lazy<List<GraphicObject>> children = new Lazy<List<GraphicObject>>();
    public List<GraphicObject> Children => children.Value;
    //統一操作
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
  //圓和方都實作GraphicObject
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

### 組合模式的實現要點：

- Composite模式採用樹形結構來實現普遍存在的對象容器，從而將“一對多”的關係轉化為“一對一”的關係，使得客戶代碼可以**一致地處理對象和對象容器**，無需關心處理的是單個的對象，還是組合的對象容器。

- 將“客戶代碼與復雜的對象容器結構”解耦是Composite模式的核心思想，解耦之後，客戶代碼將與純粹的抽象接口——而非對象容器的複雜內部實現結構——發生依賴關係，從而更能“應對變化”。

- Composite模式中，是將“Add和Remove等和對象容器相關的方法”定義在“表示抽像對象的Component類”中，還是將其定義在“表示對象容器的Composite類”中，是一個關乎“透明性”和“安全性”的兩難問題，需要仔細權衡。這裡有可能違背面向對象的“單一職責原則”，但是對於這種特殊結構，這又是必須付出的代價。ASP.Net控件的實現在這方面為我們提供了一個很好的示範。

- Composite模式在具體實現中，可以讓父對像中的子對象反向追朔；如果父對像有頻繁的遍歷需求，可使用緩存技巧來改善效率。[文章來源](https://www.cnblogs.com/PatrickLiu/p/7743118.html)



