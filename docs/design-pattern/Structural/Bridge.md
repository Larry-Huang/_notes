---
layout: default
title: Bridge
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---

### Bridge模式
Bridge模式的目的，在於將抽象與實現分離，使兩者都可以獨立地演化。這邊所謂的抽象，指的是指應用程式行為定義的演化，而實現指的是應用程式實作時，所需使用的特定API或平台。
以UML來表示Bridge模式的結構：

![](https://openhome.cc/Gossip/DesignPattern/images/Bridge-4.jpg)

簡單地說，Bridge模式的重點在於，對Abstraction的實作，不應該依賴於特定API或平台，應 辨識出Implementor，透過Implementor來橋接特定API或平台實現。

盡量用組合取代繼承，因為繼承耦合性遠大於組合！

將抽象部分與實現部分分離，使它們都可以獨立地變化。--《設計模式》Gof

橋模式不能只是認為是抽象和實現的分離，它其實並不僅限於此。其實兩個都是抽象的部分，更確切的理解，應該是將一個事物中多個維度的變化分離。


每種數據庫都有自己的版本，但是每種數據庫在不同的平台上實現又是不一樣的。比如：微軟的SqlServer數據庫，該數據庫它有2000版本、2005版本、2006版本、2008版本，後面還會有更新的版本。並且這些版本都是運行在Windows操作系統下的，如果要提供Lunix操作系統下的SqlServer怎麼辦呢？如果又要提供IOS操作系統下的SqlServer數據庫該怎麼辦呢？這個情況就可以使用橋接模式，也就是Brige模式。
[資料來源](https://www.cnblogs.com/PatrickLiu/p/7699301.html)

``` c#
namespace 橋接模式的實現
 2  {
 3      ///  <summary> 
4      /// 該抽像類就是抽象接口的定義，該類型就相當於是Abstraction類型
 5      ///  </summary> 
6      public  abstract  class IDatabase
 7      {
 8          // 通過組合方式引用平台接口，此處就是橋樑，該類型相當於Implementor類型
9          protected PlatformImplementor _implementor;
 10  
11          // 通過構造器注入，初始化平台實現
12          protected Database(PlatformImplementor implementor)
 13          {
 14              this . _implementor =implementor;
 15          }
 16  
17          // 創建數據庫--該操作相當於Abstraction類型的Operation方法
18          public  abstract  void Create();
 19      }
 20  
21      ///  <summary> 
22      /// 該抽像類就是實現接口的定義，該類型就相當於是Implementor類型
 23      ///  </summary> 
24      public  abstract  class IPlatformImplementor
 25      {
 26          // 該方法就相當於Implementor類型的OperationImpl方法
27          public  abstract  void Process();
 28      }
 29 
30      ///  <summary> 
31      /// SqlServer2000版本的數據庫，相當於RefinedAbstraction類型
 32      ///  </summary> 
33      public  class SqlServer2000 : IDatabase
 34      {
 35          // 構造函數初始化
36          public SqlServer2000(IPlatformImplementor implementor) : base (implementor) { }
 37  
38          public  override  void Create()
 39          {
 40              this ._implementor.Process();
 41          }
 42      }
 43  
44      /// <summary> 
45      /// SqlServer2005版本的數據庫，相當於RefinedAbstraction類型
 46      ///  </summary> 
47      public  class SqlServer2005 : IDatabase
 48      {
 49          // 構造函數初始化
50          public SqlServer2005(IPlatformImplementor implementor) : base (implementor) { }
 51  
52          public  override  void Create()
 53          {
 54              this ._implementor.Process();
 55          }
 56      }
 57  
58      ///  <summary> 
59     /// SqlServer2000版本的數據庫針對Unix操作系統具體的實現，相當於ConcreteImplementorA類型
 60      ///  </summary> 
61      public  class SqlServer2000UnixImplementor : IPlatformImplementor
 62      {
 63          public  override  void Process()
 64          {
 65              Console.WriteLine( " SqlServer2000針對Unix的具體實現" );
 66          }
 67      }
 68  
69      ///  <summary> 
70      /// SqlServer2005版本的數據庫針對Unix操作系統的具體實現，相當於ConcreteImplementorB類型
 71      /// </summary> 
72      public  sealed  class SqlServer2005UnixImplementor : IPlatformImplementor
 73      {
 74          public  override  void Process()
 75          {
 76              Console.WriteLine( "SqlServer2005針對Unix的具體實現" );
 77          }
 78      }
 79  
80      public  class Program
 81      {
 82          static  void Main()
 83          {
 84              IPlatformImplementor SqlServer2000UnixImp =new SqlServer2000UnixImplementor()
 85              // 還可以針對不同平台進行擴展，也就是子類化，這個是獨立變化的
86  
87              IDatabase SqlServer2000Unix = new SqlServer2000(SqlServer2000UnixImp);
 88              // 數據庫版本也可以進行擴展和升級，也進行獨立的變化。
89  
90              // 以上就是兩個維度的變化。
91  
92           // 就可以針對Unix執行操作了
93           SqlServer2000Unix.Create();
 94          }
 95      }
 96 }

```