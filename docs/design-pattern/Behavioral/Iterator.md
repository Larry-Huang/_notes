---
layout: default
title: Iterator
nav_order: 4
parent: Behavioral
grand_parent: Design_pattern
---

## Iterator Pattern 
在軟件構建過程中，集合對象內部結構常常變化各異。但對於這些集合對象，我們希望在不暴露其內部結構的同時，可以讓外部客戶代碼透明地訪問其中包含的元素；同時這種“透明遍歷”也為“同一種算法在多種集合對像上進行操作”提供了可能。
使用面向對象技術將這種遍歷機制抽象為“迭代器對象”為“應對變化中的集合對象”提供了一種優雅的方式。

**提供一種方法順序訪問一個聚合對像中的各個元素，而又不暴露該對象的內部表示。——《設計模式》GoF**

![](https://images2018.cnblogs.com/blog/1048776/201711/1048776-20171127130208487-1323685783.png)

從迭代器模式的結構圖可以看出，它涉及到四個角色，它們分別是：
1. 抽象迭代器(Iterator)：抽象迭代器定義了訪問和遍曆元素的接口，一般聲明如下方法：用於獲取第一個元素的first()，用於訪問下一個元素的next()，用於判斷是否還有下一個元素的hasNext()，用於獲取當前元素的currentItem()，在其子類中將實現這些方法。
2. 具體迭代器(ConcreteIterator)：具體迭代器實現了抽象迭代器接口，完成對集合對象的遍歷，同時在對聚合進行遍歷時跟踪其當前位置。
3. 抽象聚合類(Aggregate)：抽象聚合類用於存儲對象，並定義創建相應迭代器對象的接口，聲明一個createIterator()方法用於創建一個迭代器對象。
4. 具體聚合類(ConcreteAggregate)：具體聚合類實現了創建相應迭代器的接口，實現了在抽象聚合類中聲明的createIterator()方法，並返回一個與該具體聚合相對應的具體迭代器ConcreteIterator實例。

``` c#
namespace 迭代器模式的實現
 {
     // 部隊隊列的抽象聚合類--該類型相當於抽象聚合類Aggregate 
     public  interface ITroopQueue
     {
         Iterator GetIterator();
     }
     // 迭代器抽像類
     public  interface Iterator
     {
         bool MoveNext();
         Object GetCurrent();
         void Next();
         void Reset();
     }
    // 部隊隊列具體聚合類--相當於具體聚合類ConcreteAggregate 
     public  sealed  class ConcreteTroopQueue:ITroopQueue
     {
         private  string [] collection;
         public ConcreteTroopQueue()
         {
             collection = new  string [] { " 黃飛鴻" , " 方世玉" , " 洪熙官" , " 嚴詠春" };
         }
         publicIterator GetIterator()
         {
             return  new ConcreteIterator( this );
         }
         public  int Length
         {
             get { return collection.Length; }
         }
         public  string GetElement( int index)
         {
             return collection[index ];
         }
     }
     //具體迭代器類
     public  sealed  class ConcreteIterator : Iterator
     {
         // 迭代器要集合對象進行遍歷操作，自然就需要引用集合對象
         private ConcreteTroopQueue _list;
         private  int _index;
         public ConcreteIterator(ConcreteTroopQueue list)
         {
             _list = list;
             _index = 0 ;
         }
         public  bool MoveNext()
         {
             if (_index < _list.Length)
             {
                 return  true ;
             }
             return  false ;
         }
         public Object GetCurrent()
         {
             return _list.GetElement(_index);
         }
         public  void Reset()
         {
             _index = 0 ;
         }
        public  void Next()
         {
             if (_index < _list.Length)
             {
                 _index++ ;
             }
         }
     }
  
     // 客戶端（Client）
     class Program
     {
         static  void Main( string [] args)
         {
             Iterator iterator;
             ITroopQueue list =new ConcreteTroopQueue();
             iterator = list.GetIterator();
             while (iterator.MoveNext())
             {
                 string ren = ( string )iterator.GetCurrent();
                 Console.WriteLine(ren);
                 iterator. Next();
             }
             Console.Read();
         }
     }
}
```
- 迭代抽象：訪問一個聚合對象的內容而無需暴露它的內部表示。
- 迭代多態：為遍歷不同的集合結構提供一個統一的接口，從而支持同樣的算法在不同的集合結構上進行操作。
- 迭代器的健壯性考慮：遍歷的同時更改迭代器所在的集合結構，會導致問題。