---
layout: default
title: Flyweight
nav_order: 1
parent: Structural
grand_parent: Design_pattern
---

## Flyweight Pattern

在軟件系統中，採用純粹對象方案的問題在於大量細粒度的對象會很快充斥在系統中，從而帶來很高的運行時代價——主要指內存需求方面的代價。如何在避免大量細粒度對象問題的同時，讓外部客戶程序仍然能夠透明地使用面向對象的方式來進行操作？

**運用共享技術有效地支持大量細粒度的對象。——《設計模式》GoF**

![](https://images2017.cnblogs.com/blog/1048776/201711/1048776-20171106142034559-130872114.png)

1. 抽象共享單元角色（Flyweight） :此角色是所有的具體享元類的基類，為這些類規定出需要實現的公共接口。那些需要外部狀態的操作可以通過調用方法以參數形式傳入。
2. 具體享元角色（ConcreteFlyweight）：實現抽象享元角色所規定的接口。如果有內部狀態的話，可以在類內部定義。
3. 共享單元工廠角色（FlyweightFactory）：本角色負責創建和管理享元角色。本角色必須保證享元對象可以被系統適當地共享，當一個客戶端對象調用一個享元對象的時候，享元工廠角色檢查系統中是否已經有一個符合要求的享元對象，如果已經存在，享元工廠角色就提供已存在的享元對象，如果系統中沒有一個符合的享元對象的話，享元工廠角色就應當創建一個合適的享元對象。
4. 客戶端角色（Client）：本角色需要存儲所有享元對象的外部狀態。

``` c#
namespace 享元模式的實現
     {
         ///  <summary> 
         /// 享元模式不是很難，但是有些狀態需要單獨處理，以下就是該模式的C#實現，有些輔助類，大家應該看得出吧，別混了。
         ///  </summary> 
         class Client
         {
             static  void Main( string [] args)
             {
                 // 比如，我們現在需要10000個一般士兵，只需這樣
                 SoldierFactory factory = new SoldierFactory();
                 AK47 ak47 = new AK47();
                for ( int i = 0 ; i < 100 ; i++ )
                 {
                     Soldier soldier = null ;
                     if (i <= 20 )
                     {
                         soldier = factory.GetSoldier( " 士兵" + (i + 1 ), ak47, SoldierType.Normal);
                     }
                     else 
                     {
                         soldier = factory.GetSoldier(" 士兵" + (i + 1 ), ak47, SoldierType.Water);
                     }     
                     soldier.Fight();
                 }
                 // 我們有這麼多的士兵，但是使用的內存不是很多，因為我們緩存了。
                 Console.Read();
             }
         }
 
         // 這些是輔助類型
         public  enum SoldierType
         {
             Normal,
             Water
         }
 
        // 該類型就是抽象戰士Soldier--該類型相當於抽象享元角色
         public  abstract  class Soldier
         {
             // 通過構造函數初始化士兵的名稱
             protected Soldier( string name)
             {
                 this .Name = name ;
             }
 
             // 士兵的名字
             public  string Name { get ; private  set ; }
 
             // 可以傳入不同的武器就用不同的活力---該方法相當於抽象Flyweight的Operation方法
            public  abstract  void Fight();
 
             public Weapen WeapenInstance { get ; set ; }
         }
 
         // 一般類型的戰士，武器就是步槍---相當於具體的Flyweight角色
         public  sealed  class NormalSoldier : Soldier
         {
             // 通過構造函數初始化士兵的名稱
             public NormalSoldier( string name) : base (name) { }
 
             // 執行享元的方法---就是Flyweight類型的Operation方法
             public override  void Fight()
             {
                 WeapenInstance.Fire( " 士兵：" +Name+ " 在陸地執行擊斃任務" );
             }
         }
 
         // 這是海軍陸戰隊隊員，武器精良----相當於具體的Flyweight角色
         public  sealed  class WaterSoldier : Soldier
         {
             // 通過構造函數初始化士兵的名稱
             public WaterSoldier( string name) : base (name) { }
 
            // 執行享元的方法---就是Flyweight類型的Operation方法
             public  override  void Fight()
             {
                 WeapenInstance.Fire( " 士兵：" +Name+ " 在海中執行擊斃任務" );
             }
         }
 
         // 此類型和享元沒太大關係，可以算是享元對象的狀態吧，需要從外部定義
         public  abstract  class Weapen
         {
             public  abstract  void Fire( string jobName);
        }
 
         // 此類型和享元沒太大關係，可以算是享元對象的狀態吧，需要從外部定義
         public  sealed  class AK47:Weapen
         {
             public  override  void Fire( string jobName)
             {
                 Console .WriteLine(jobName);
             }
         }
 
         // 該類型相當於是享元的工廠---相當於FlyweightFactory類型
         public  sealed  class SoldierFactory
         {
             private static IList<Soldier> soldiers;
 
             static SoldierFactory()
             {
                 soldiers = new List<Soldier>();
             }
 
             Soldier mySoldier = null ;
             // 因為我這裡有兩種士兵，所以在這裡可以增加另外一個參數，士兵類型，原模式裡面沒有，
             public Soldier GetSoldier( string name, Weapen weapen, SoldierType soldierType)
             {
                 foreach (Soldier soldier in soldiers)
                 {
                     if ( string.Compare(soldier.Name, name, true ) == 0 )
                     {
                         mySoldier = soldier;
                         return mySoldier;
                     }
                 }
                 // 我們這裡就任務名稱是唯一的
                 if ( soldierType == SoldierType.Normal)
                 {
                     mySoldier = new NormalSoldier(name);
                }
                 else 
                 {
                     mySoldier = new WaterSoldier(name);
                 }
                 mySoldier.WeapenInstance = weapen;
 
                 soldiers.Add(mySoldier);
                 return mySoldier;
             }
         }
     }
```