---
layout: default
title: Design_pattern
nav_order: 1
has_children: true
permalink: /docs/design_pattern
---


## 物件導向程式設計的 4 個重要特性
- 抽象 ( Abstraction )
- 將真實世界的需求轉換成為 OOP 中的類別
- 類別可以包含狀態(屬性)與行為(方法)。
- 封裝 ( Encapsulation )
- 隱藏/保護內部實作的細節，並可以對屬性或方法設定存取層級 (public, private, protected)。
- 繼承 ( Inheritance )
- 可讓您建立新類別以重複使用、擴充和修改其他類別中定義的行為。
- 多型 ( Polymorphism )
- 在相同的介面下，可以用不同的型別來實現。
- 多型有分成好幾種不同類型。

## 內聚力 Cohesion
>在一個 "模組" 內完成 "一件工作" 的度量指標

## 耦合力 Coupling
>模組與模組之間的關聯強度，模組之間相互依賴的程度

## SOLID
The S.O.L.I.D. Principles of Object Oriented Design
- 單一責任原則 SRP Single Responsibility Principle
  - <font color='red'>A class should have only one reason to change.</font>
  - 參考 YAGNI（You Ain't Gonna Need It）原則
  - 不用急於在第一時間就專注於分離責任
  - 尚未出現的需求 (未來的需求) 不需要預先分離責任
  - 當需求變更的時候，再進行類別分割即可！
  - SRP 是 SOLID 中最簡單的，但卻是最難做到的
  - 需要不斷提升你的開發經驗與重構技術
  - 如果你沒有足夠的經驗去定義一個物件的 Responsibility那麼建議你不要過早進行 SRP 規劃！

- 開放封閉原則 OCP Open Closed Principle
  - Software entities (classes, modules, functions, etc.)should be open for extension but closed for modification
  - 藉由增加新的程式碼來擴充系統的功能，而不是藉由修改原本已經存在的程式碼來擴充系統
  - 關於 OCP 的使用時機
    - 你既有的類別已經被清楚定義，處於一個強調穩定的狀態
    - 你需要擴充現有的類別，加入新需求的屬性或方法
    - 你擔心修改現有程式碼會破壞現有系統的運作
    - 系統剛開始設計時就決定要採用 OCP 模式
    - 可以透過 "介面" 或 "抽象類別" 進行實作
    - 
- 里氏替換原則 LSP Liskov Substitution Principle
  - Subtypes must be substitutable for their base types.
  - 子型別必需可替換為他的基底型別
  - 如果你的程式有採用繼承或介面，然後建立出幾個不同的衍生型別 (Subtypes)。在你的系統中只要是基底型別出現的地方，都可以用子型別來取代，而不會破壞程式原有的行為。
  - 關於 LSP 的基本精神
    - 當實作繼承時，必須確保型別轉換後還能得到正確的結果
    - 每個衍生類別都可以正確地替換為基底類別，且程式在執行時不會有異常的情況 (如發生執行時期例外)
    - 必須正確的實作 "繼承" 與 "多型"
  - 關於 LSP 的實作方式
    - 採用類別繼承方式來進行開發
    - 需注意繼承的實作方式
    - 利用 "介面" ( interface ) 來定義基底型別 ( base type )
- 介面隔離原則 ISP Interface Segregation Principle
  - Many client specific interfaces are better than one general purpose interface.
  - Clients should not be forced to depend upon interfaces that they don't use.
  - 針對不同需求的用戶端，僅開放其對應需求的介面就好
  - 關於 ISP 的實作方式
    - 依據用戶端需求，將介面進行分割或群組

- 相依反轉原則 DIP Dependency Inversion Principle 
  - High-level modules should not depend on low-level modules. Both should depend on abstractions.
  - Abstractions should not depend on details. Details
should depend on abstractions
  - 高階模組不應該依賴於低階模組，兩者都應相依於抽象
    - 高階模組 -> Caller (呼叫端)
    - 低階模組 -> Callee (被呼叫端)
    - 抽象不應該相依於細節。而細節則應該相依於抽象
  - 關於 DIP 的基本精神
    - 所有類別都要相依於抽象，而不是具體實作
    - 為了要達到類別間鬆散耦合的目的
    - 開發過程中，所有類別之間的耦合關係一律透過抽象介面
  - 關於 DIP 的實作方式
    - 型別全部都相依於抽象，而不是具體實作
    - 經過套用 DIP 之後，原來有相依於類別的程式碼
    - 都改成相依於抽象型別
    - 從緊密耦合關係變成鬆散耦合關係
    - 可以依據需求，隨時抽換具體實作類別

### Conclusion
1.找出程式中可能需要更動之處，把它們獨立出來，不要和那些不需要更動的程式碼混在一起。
2.寫程式是針對介面寫，而不是針對實踐方式而寫。
3.多用合成，少用繼承。



程式設計是思維具體化的一種方式，是思考如何解決問題的過程，設計模式是在解決問題的過程中，一些良好思路的經驗集成，最早講設計模式，人們總會提到 [Gof](http://www.amazon.com/exec/obidos/tg/detail/-/0201633612/ref=ase_portlandpatternrA/002-6454224-1040813?v=glance&s=books)的著作，它最早將經典的 23 種模式集合在一起說明，對後期學習程式設計，尤其是對從事物件導向程式設計的人們起了莫大的影響。後來設計模式一詞被廣泛的應用到各種經驗集成，甚至還有反模式 （AntiPattern），反模式教導您如何避開一些常犯且似是而非的程式設計思維。

