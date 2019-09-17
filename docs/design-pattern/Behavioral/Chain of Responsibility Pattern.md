---
layout: default
title: Chain of Responsibility Pattern
nav_order: 4
parent: Behavioral
grand_parent: Design_pattern
---

## Chain of Responsibility Pattern

 在軟件構建過程中，一個請求可能被多個對象處理，但是每個請求在運行時只能有一個接受者，如果顯示指定，將必不可少地帶來請求發送者與接受者的緊耦合。如何使請求的發送者不需要指定具體的接受者，讓請求的接受者自己在運行時決定來處理請求，從而使兩者解耦。

**避免請求發送者與接收者耦合在一起，讓多個對像都有可能接受請求，將這些對象連接成一條鏈，並且沿著這條鏈傳遞請求，知道有對象處理它為止。——《設計模式》GoF**

![](https://images2018.cnblogs.com/blog/1048776/201712/1048776-20171225140403040-197033143.png)
---
1. 抽象處理者角色（Handler）：抽象處理者定義了一個處理請求的接口，它一般設計為抽像類，由於不同的具體處理者處理請求的方式不同，因此在其中定義了抽象請求處理方法。因為每一個處理者的下家還是一個處理者，因此在抽象處理者中定義了一個自類型的對象，作為其對下家的引用。通過該引用，處理者可以連成一條鏈。
2. 具體處理者角色（ConcreteHandler）：具體處理者是抽象處理者的子類，它可以處理用戶請求，在具體處理者類中實現了抽象處理者中定義的抽象處理方法，在處理請求之前需要進行判斷，看是否有相應的處理權限，如果可以處理請求就處理它，否則將請求轉發給後繼者；在具體處理者中可以訪問鏈中下一個對象，以便請求的轉發。

### 職責鏈模式的實現要點：
1. Chain of Responsibility模式的應用場合在於“一個請求可能有多個接受者，但是最後真正的接受者只有一個”，只有這時候請求發送者與接受者的耦合才有可能出現“變化脆弱”的症狀，職責鏈的目的就是將二者解耦，從而更好地應對變化。
2. 應用了Chain of Responsibility模式後，對象的職責分派將更具靈活性。我們可以在運行時動態添加/修改請求的處理職責。
3. 當我們要新增一個DHandler處理請求，就不需再改原來的代碼了，遵從了開放封閉原則。這樣我們的程序就更賦予變化，更有變化的抵抗力。
4.  Handler類本身繼承自BaseHandler類型，又包含了一個BaseHandler類型的對象，這點類似Decorator模式。
5.  如果請求傳遞到職責鏈的末尾仍得不到處理，應該有一個合理的缺省機制。這也是每一個接受對象的責任，而不是發出請求的對象的責任。

### 職責鏈模式的主要優點有：

1. 降低耦合度：職責鏈模式使得一個對象無需知道是其他哪一個對象處理其請求。對象僅需知道該請求會被處理即可，接受者和發送者都沒有對方的明確信息，且鏈中的對像不需要知道鏈的結構，有客戶端負責鏈的創建。
2. 可簡化對象的相互連接：接受者對象僅需維持一個指向其後繼者的引用，而不需維持它對所有的候選處理者的引用。
3. 增強給對象指派職責的靈活性：在給對象分派職責時，職責鏈可以給我們帶來更多的靈活性。可以通過在運行時對該連進行動態的增加或修改處理一個請求的職責。
4. 增加新的請求處理類很方便：在系統中增加一個新的請求處理者無需修改原有系統的代碼，只需要在客戶端重新建鏈即可，從這一點看來是符合“開閉原則”的。

### 職責鏈模式的主要缺點有：
1. 在找到正確的處理對象之前，所有的條件判定都要執行一遍，當責任鏈過長時，可能會引起性能的問題。
2. 可能導致某個請求不被處理。
3. 客戶端需要組裝這個鏈條，耦合了客戶端和鏈條的組成結構，可以把這個在客戶端的組合動作提到外面，通過配置來做，會更好點。
### 在下面的情況下可以考慮使用職責鏈模式：

1. 一個系統的審批需要多個對象才能完成處理的情況下，例如請假系統等。
2. 代碼中存在多個if-else語句的情況下，此時可以考慮使用責任鏈模式來對代碼進行重構
3. 有多個對象可以處理同一個請求，具體哪個對象處理該請求有運行時刻自動確定。客戶端只需將請求提交到鏈上，無須關心請求的處理對像是誰以及它是如何處理的。
4. 不明確指定接受者的情況下，向多個對像中的一個提交一個請求。請求的發送者與請求者解耦，請求將沿著鏈進行傳遞，尋求響應的處理者。
5. 可動態指定一組對象處理請求。客戶端可以動態創建職責鏈來處理請求，還可以動態改變鏈中處理者之間的先後次序
```c#
namespace ChainOfResponsibility
 {
     // 採購請求
     public  sealed  class PurchaseRequest
     {
         // 金額
         public  double Amount { get ; set ; }
 
         // 產品名字
         public  string ProductName { get ; set ; }
 
         public PurchaseRequest( double amount, string productName)
         {
             Amount = amount;
             ProductName = productName;
         }
     }
  
     // 抽象審批人,Handler---相當於“抽象處理者角色” 
     public  abstract  class Approver
     {
         // 下一位審批人，由此形成一條鏈
         public Approver NextApprover { get ; set ; }
 
         // 審批人的名稱
         public  string Name { get ;set ; }
 
         public Approver( string name)
         {
             this .Name = name;
         }
 
         // 處理請求
         public  abstract  void ProcessRequest(PurchaseRequest request);
     }
  
     // 部門經理----相當於“具體處理者角色” ConcreteHandler 
     public  sealed  class Manager : Approver
     {
         public Manager( string name):base (name){ }
  
         public  override  void ProcessRequest(PurchaseRequest request)
         {
             if (request.Amount <= 10000.0 )
             {
                 Console.WriteLine( " {0}部門經理批准了對原材料{1}的採購計劃！" , this .Name, request.ProductName);
             }
             else  if (NextApprover != null )
             {
                 NextApprover.ProcessRequest(request);
             }
         }
     }
  
     // 財務經理---相當於“具體處理者角色”ConcreteHandler 
     public  sealed  class FinancialManager : Approver
     {
         public FinancialManager( string name): base (name){ }
 
         public  override  void ProcessRequest(PurchaseRequest request)
         {
             if (request.Amount > 10000.0 && request.Amount <= 50000.0 )
            {
                 Console.WriteLine( " {0}財務經理批准了對原材料{1}的採購計劃！" , this .Name, request.ProductName);
             }
             else  if (NextApprover != null )
             {
                 NextApprover. ProcessRequest(request);
             }
         }
     }
  
     // 總裁---相當於“具體處理者角色” ConcreteHandler 
     public  sealed  class CEO :Approver
     {
         public CEO( string name): base (name){ }
 
         public  override  void ProcessRequest(PurchaseRequest request)
         {
             if (request.Amount > 50000.0 && request.Amount < 300000.0 )
             {
                 Console.WriteLine( " { 0}總裁批准了對原材料{1}的採購計劃！" , this .Name, request.ProductName);
             }
             else 
             {
                Console.WriteLine( " 這個採購計劃的金額比較大，需要一次董事會會議討論才能決定！" );
             }
         }
     }
  
     class Program
     {
         static  void Main( string [] args)
         {
             PurchaseRequest requestDao = new PurchaseRequest( 8000.0 , " 單刀5把" );
             PurchaseRequest requestHuaJi = new PurchaseRequest( 10000.0, " 10把方天畫戟" );
             PurchaseRequest requestJian = new PurchaseRequest( 80000.0 , " 5把金絲龍鱗閃電劈" );
  
             Approver manager = new Manager( " 黃飛鴻" );
             Approver financial = new FinancialManager( " 黃麒英" );
             Approver ceo = new CEO( " 十三姨" );
  
             // 設置職責鏈
             manager.NextApprover = financial;
             financial.NextApprover = ceo;
  
             // 處理請求
             manager.ProcessRequest(requestDao);
             manager.ProcessRequest(requestHuaJi);
             manager.ProcessRequest(requestJian);
 
             Console.ReadLine( );
         }
     }
}
```

