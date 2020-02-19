---
layout: default
title: SMT MES
parent: mes
nav_order: 1
---


# MES


## 在WIP 的再製品管理模式
### SMT打板 
1. 物料正確與否(BOM)
2. 上料操作正確性(Feeder編號與物料是否在Picking List 一致，Feeder置於slot或者機台中的位置和順序是否與Program一致)
3. 機台運行中的拋料率問題
4. 鋼板/Feeder/slot 等耗材的版本和日常維護管理
  
### PCB DIP
1. 關注人工流程
2. 大量測試功能以檢測手工插件和回焊成效
3. 不能跳站、漏站、閉環

### Assembly 組裝
1. 關注成品序列碼、關鍵零組件(Key component)關聯和組裝後的測試及燒機
2. 支援客戶Issue release 後的產品及組件的回朔
3. 回朔要能加上DIP及SMT的資訊，一直追朔到IC電子元件的供應商和批號，組成完成的產品問題追朔功能
