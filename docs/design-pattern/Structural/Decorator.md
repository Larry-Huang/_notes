---
layout: default
title: Decorator
nav_order: 1
parent: Structural
grand_parent: design_pattern
---

文章來源
[Decorator Pattern](https://ithelp.ithome.com.tw/articles/10198235)


Decorator的特性是採用組合、而非繼承，實現運行時動態地擴展物件功能的能力，而且可以
根據需要擴展多個功能。避免單獨使用繼承而產生靈活性差和"多子類別衍生問題。但它最根本的作用是在於裝飾。

例如開發一個報表功能可能只有顯示和下載的兩個功能，
之後又要加入顯示時先檢查是否有權限、下載前也要檢查權限、權限檢查後，判斷系統日期是否為月底。
所以為了要解決新增功能問題，不要修改原本的程式碼，又可以擴充，Decorator就是符合開發封閉原則，對修改封閉，對擴充開放，
藉以降低系統可能的錯誤風險，使用Decorator模式的組合特性來達到動態擴充相關功能。

在某一物件動態加上功能。
一層一層的將功能套上去，每一層執行的是不同的物件。