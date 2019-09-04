---
layout: default
title: Template Method
nav_order: 4
parent: Behavioral
grand_parent: Design_pattern
---

## Template Method Pattern
 在軟件構建過程中，對於某一項任務，它常常有穩定的整體操作結構，但各個子步驟卻有很多改變的需求，或者由於固有的原因（比如框架與應用之間的關係）而無法和任務的整體結構同時實現。如何在確定穩定操作結構的前提下，來靈活應對各個子步驟的變化或者晚期實現需求？

 **定義一個操作中的算法的骨架，而將一些步驟延遲到子類中。Template Method使得子類可以不改變一個算法的結構即可重定義該算法的某些特定步驟。——《設計模式》GoF **

 ![](https://images2017.cnblogs.com/blog/1048776/201711/1048776-20171115103609171-1216271082.png)
### 模板方法模式參與者：
1. 抽像類角色（AbstractClass）：定義一個模板方法（TemplateMethod），在該方法中包含著一個算法的骨架，具體的算法步驟是PrimitiveOperation1方法和PrimitiveOperation2方法，該抽像類的子類將重定義PrimitiveOperation1和PrimitiveOperation2操作。
2. 具體類角色（ConcreteClass）：實現PrimitiveOperation1方法和PrimitiveOperation2方法以完成算法中與特定子類（Client）相關的內容。
3. 在模板方法模式中，AbstractClass中的TemplateMethod提供了一個標準模板，該模板包含PrimitiveOperation1和PrimitiveOperation2兩個方法，這兩個方法的內容Client可以根據自己的需要重寫。



``` c#
namespace 模板方法模式的實現
 {
     ///  <summary> 
     /// 好吃不如餃子，舒服不如倒著，我最喜歡吃我爸爸包的餃子，今天就拿吃餃子這件事來看看模板方法的實現吧
     ///  </summary> 
     class Client
     {
         static  void Main( string [] args)
         {
             // 現在想吃綠色面的，豬肉大蔥餡的餃子
             AbstractClass fan = new ConcreteClass();
             fan.EatDumplings();
 
             Console.WriteLine();
             // 過了段時間，我開始想吃橙色面的，韭菜雞蛋餡的餃子
             fan = new ConcreteClass2();
             fan.EatDumplings();
             Console.Read();
         }
     }
 
 
     // 該類型就是抽像類角色--AbstractClass，定義做餃子的算法骨架，這裡有三步驟，當然也可以有多個步驟，根據實際需要而定
     public  abstract  class AbstractClass
     {
         // 該方法就是模板方法，方法裡麵包含了做餃子的算法步驟，模板方法可以返回結果，也可以是void類型，視具體情況而定
         public  void EatDumplings()
        {
             // 和麵
             MakingDough();
             // 包餡
             MakeDumplings();
             // 煮餃子
             BoiledDumplings();
 
             Console.WriteLine( " 餃子真好吃！" );
         }
 
         // 要想吃餃子第一步肯定是“和麵”---該方法相當於算法中的某一步
         public  abstract  void MakingDough();
 
         // 要想吃餃子第二部是“包餃子”---該方法相當於算法中的某一步
         public  abstract void MakeDumplings();
 
         // 要想吃餃子第三部是“煮餃子”---該方法相當於算法中的某一步
         public  abstract  void BoiledDumplings();
     }
 
     // 該類型是具體類角色--ConcreteClass，我想吃綠色面皮，豬肉大蔥餡的餃子
     public  sealed  class ConcreteClass : AbstractClass
     {
         // 要想吃餃子第一步肯定是“和麵”---該方法相當於算法中的某一步
         public  override  void MakingDough()
         {
             // 我想要面是綠色的，綠色健康嘛，就可以在此步定制了
            Console.WriteLine( " 在和麵的時候加入芹菜汁，和好的面就是綠色的" );
         }
 
         // 要想吃餃子第二部是“包餃子”---該方法相當於算法中的某一步
         public  override  void MakeDumplings()
         {
             // 我想吃豬肉大蔥餡的，在此步就可以定制了
             Console.WriteLine( " 農家豬肉和農家大蔥，製作成餡" );
         }
 
         // 要想吃餃子第三部是“煮餃子”---該方法相當於算法中的某一步
         public  override  void BoiledDumplings()
         {
             // 我想吃大鐵鍋煮的餃子，有家的味道，在此步就可以定制了
             Console.WriteLine( " 用我家的大鐵鍋和大木材煮餃子" );
         }
     }
 
     // 該類型是具體類角色--ConcreteClass2，我想吃橙色面皮，韭菜雞蛋餡的餃子
     public  sealed  class ConcreteClass2 : AbstractClass
     {
         // 要想吃餃子第一步肯定是“和麵”- --該方法相當於算法中的某一步
         public  override  void MakingDough()
         {
             // 我想要面是橙色的，加入胡蘿蔔汁就可以。在此步定制就可以了。
            Console.WriteLine( " 在和麵的時候加入胡蘿蔔汁，和好的面就是橙色的" );
         }
 
         // 要想吃餃子第二部是“包餃子”---該方法相當於算法中的某一步
         public  override  void MakeDumplings()
         {
             // 我想吃韭菜雞蛋餡的，在此步就可以定制了
             Console.WriteLine( " 農家雞蛋和農家韭菜，製作成餡" );
         }
 
         // 要想吃餃子第三部是“煮餃子”---該方法相當於算法中的某一步
         public  override  void BoiledDumplings()
        {
             // 此處沒要求
             Console.WriteLine( " 可以用一般煤氣和不粘鍋煮就可以" );
         }
     }
}
```

### 模板方法模式適用情形：

1. 一次性實現一個算法的不變部分，並將可變的行為留給子類來實現。
2. 各子類中公共的行為應被提取出來並集中到一個公共父類中以避免代碼重複。
3. 控制子類擴展。模板方法只允許在特定點進行擴展，而模板部分則是穩定的。
---
### 模板方法模式特點：
1. TemplateMethod模式是一種非常基礎性的設計模式，在面向對象系統中大量應用。它用最簡潔的機制（基礎、多態）為很多應用程序框架提供了靈活的擴展點，是代碼復用方面的基本實現結構。
2. 在具體實現方面，被TemplateMethod調用的虛方法可以具有實現，也可以沒有任何實現（抽象方法或虛方法）。但一般推薦將它們設置為protected方法使得只有子類可以訪問它們。
3. 模板方法模式通過對子類的擴展增加新的行為，符合“開閉原則”。