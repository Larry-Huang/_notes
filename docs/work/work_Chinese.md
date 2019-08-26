---
layout: default
title: work_Chinese
parent: work
nav_order: 2
---

# 開發系統簡介資料


## 1.憑證驗證 
為了整合國軍智慧卡驗證機制，針對國軍客製化智慧卡進行驗證程序設計，用戶端將智慧卡插入讀卡機後，透過憑證驗證程式，輸入PinCode後進行第一階段驗證，當PinCode驗證完成後，進入卡片讀取簽章憑證資訊並驗證其校期，再來由驗證伺服至LDAP取出身份識別，並將憑證序號送入MCA進行驗證，檢查該張憑證是否有效。
![](https://i.imgur.com/Bu0yTL1.png)

## 2.Reserving and Pentesting
針對反組譯及ShellCode撰寫如圖2-1，藉此熟悉組合語言並僚解底層運作原理，後續並利用編碼方式測試防毒軟體偵測及ShellCode規避能力。
![](https://i.imgur.com/g1iq8bX.png)
## 3.員額報表系統
配合人事部門開發，使用WPF、MVC及Restful架構，並結合OneTimeToken產製、憑證驗證。開發工項包含報表60種、帳號管理、角色權限管理，輸入及參數設定，開發時間約1年，現為人事核心系統。
其中在每日員額統計使用樹狀圖顯示所有單位，並提供使用者勾選後，查找子單位及併計單位總量的功能，在開發時初期使用遞歸方式，在後續分析後，發現大量查找時出現效能瓶頸，遞歸過程中出現大量重複運算，後續利用動態規劃減少計算量後，成功解決問題。
![](https://i.imgur.com/ndpzIIo.png)

## 4.電腦資產管控及視覺化
配合單位重新佈署作業系統，於作業系統內部署回報程式，使單位可掌握目前作業系統安裝狀況，並結合資產、網管及回報系統資料庫同步及排程回饋，利用圖表視覺化顯示相關資訊。
![](https://i.imgur.com/78DU9Rp.png)
## 5.災害防救資訊系統發展
因救災任務順遂，105年時開發「災害防救資訊系統」，整合內政部TGOS救災圖層資訊、GoogleMapAPI、SignalR，Client使用Android行動裝置。
![](https://i.imgur.com/GEP3wKY.jpg)
![](https://i.imgur.com/qToUv4k.jpg)
## 6.線上歸鄉報到
撰寫離營歸鄉報到系統，107年時完成與內政部資訊系統構連，節省離退人員往返市區公所報到時間。
![](https://i.imgur.com/L5YmVBT.png)
## 7.補繳年資系統
整合公務人員退休撫卹基金會，107年時開發國軍購買年資系統，完成8萬人補繳工程...整個開發及測試時間嚴重壓縮，幸運的最後平安結束，自己也被扣了4萬多Ｏrz。
![](https://i.imgur.com/1eFLvb9.jpg)
## 8.其他開發項目
由於單位關係...很多東西連畫面都沒辦法截...只能列表


:closed_book: 系統開發項目
* 軍人保險計算查詢系統
* 志願役士兵起役系統
* 單位門戶網站含後台
* 代碼查詢系統
* SyBase拓樸查詢系統
* 國軍軟體管理資訊系統
* 門戶網站仿WebDesktop
* 退除役資料交換Web服務
* 驗證碼產生WebService
* 問卷調查系統

:closed_book: 系統維護項目
* 人事資訊系統(7個子系統)
* 人事查詢系統(5個子系統)
* 人事線傳系統
* 出國管制系統
* 考核系統
* StorePreduce（大量數千行）、CronJob多隻
* 體能鑑測、CA憑證、人員查詢、門禁WebService
* 財產抽籤系統

## 自我學習與受訓課程（ＭＯＯＣ）
對於新知識充滿熱情，強迫症般的利用下班時間不斷學習，加強自身不足，相信很快能適應或符合團隊需求。

| 類別|名稱|課程來源|講師|備考|
|-|-|-|-|-|
|Data Science|Neural Networks and Deep Learning|Coursera|Deeplearning.ai Andrew Ng|自行學習|
||Improving Deep Neural Networks|Coursera|Deeplearning.ai Andrew Ng|自行學習|
||Structured Machine Learning Projects|Coursera|Deeplearning.ai Andrew Ng|自行學習|
||Convolutional Neural Networks|Coursera|Deeplearning.ai Andrew Ng|自行學習|
||Sequence Models |Coursera|Deeplearning.ai Andrew Ng|自行學習|
||Machine Learning|Coursera|Standford Andrew Ng|自行學習|
||Artificial Intelligence A-Z™: Learn How To Build An AI|Udemy|Hadelin de Ponteves|自行學習|
||Deep Learning A-Z™: Hands-On Artificial Neural Networks|Udemy|Kirill Eremenko|自行學習|
||Apache Hadoop之開發者訓練課程|恆毅資訊|潘家羲 Sparrow Pan|公司派訓|
||机器学习 A-Z (Machine Learning A-Z in Chinese)|Udemy|Hadelin de Ponteves|自行學習|
||Data Science and Machine Learning for Infosec|Pentester Academy|Sinan Ozdemir|自行學習|
||D3.js資料視覺化|國發會|巨匠電腦講師|公司派訓|
||Building Recommender Systems with Machine Learning and AI||Sundog Education by Frank Kane|自行學習|
||Machine Learning with Javascript||Stephen Grider, Engineering Architect|自行學習|
||Artificial Intelligence 2018: Build the Most Powerful AI||Hadelin de Ponteves, AI Entrepreneur|自行學習|
||Machine Learning Practical: 6 Real-World Applications||Kirill Eremenko, Data Scientist|自行學習|
||Deep Learning and Computer Vision A-Z™: OpenCV, SSD & GANs||Hadelin de Ponteves, AI Entrepreneur|自行學習|
||Tableau 10 A-Z: Hands-On Tableau Training For Data Science||Kirill Eremenko, Data Scientist|自行學習|
|Security|Python for Pentesters|Pentester Academy|Vivek Ramachandran|自行學習|
||x86 Assembly Language and Shellcoding on Linux|Pentester Academy|Vivek Ramachandran|自行學習|
||Javascript for Pentesters|Pentester Academy|Vivek Ramachandran|自行學習|
||Pentesting with Metasploit|Pentester Academy|Vivek Ramachandran|自行學習|
||Wi-Fi Security and Pentesting|Pentester Academy|Vivek Ramachandran|自行學習|
||Exploiting Simple Buffer Overflows on Win32|Pentester Academy|Vivek Ramachandran|自行學習|
||GNU Debugger Megaprimer|Pentester Academy|Vivek Ramachandran|自行學習|
||Web Application Pentesting|Pentester Academy|Vivek Ramachandran|自行學習|
||Network Pentesting|Pentester Academy|Vivek Ramachandran|自行學習|
||Reverse Engineering and Exploit Development|Udemy|Infinite Skills Dr.Philip Polstra|自行學習|
||Kali Linux:Learn The Complete Hacking Operating System|Udemy |Sunil K. Gupta|自行學習|
||Advanced Ethical Hacking|Udemy|VTCSoftware Training|自行學習|
||The Complete Cyber Security Course : Hackers Exposed|Udemy|Nathan House|自行學習|
||The Complete Nmap Ethical Hacking Course : Network Security|Udemy|Nathan House|自行學習|
||The Complete Cyber Security Course : Network Security|Udemy|Nathan House|自行學習|
||The Complete Cyber Security Course : End Point Protection|Udemy|Nathan House|自行學習|
||Learn Social Engineering From Scratch|Udemy|Zaid Sabih|自行學習|
||Website Hacking in Practice|Udemy|Hacking School|自行學習|
||The Complete Cyber Security Course : Anonymous Browsing|Udemy|Nathan House|自行學習|
||SSCP資安專業人員認證課程|恆毅資訊|唐任威 Vincent_tang|公司派訓|
|程式開發|JavaScript 全攻略：克服 JS 的奇怪部分|Udemy|慕課|自行學習|
||JavaScript Design Patterns: 20 Patterns for Expert Code|Udemy|Packt Publishing|自行學習|
||A Simple Node.js Mongo Restify API in Less Than 3 Hours|Udemy|Jim Hlad|自行學習|
||Learning Windows PowerShell|Udemy|Infinite Skills|自行學習|
||C# Developers: Learn the Art of Writing Clean Code|Udemy|Mosh Hamedani|自行學習|
||Red Hat程式設計|恆毅資訊|王俊城Anderson Wang|公司派訓|
||Andriod程式設計|恆毅資訊|何孟翰 Mark Ho|公司派訓|
||Linux系統管理|國發會|林國龍 Bill Lin|公司派訓|
||Vue.js Essentials - 3 Course Bundle||Anthony Gore, Vue Community Partner|自行學習|
||動畫互動網頁特效入門（JS/CANVAS||hahow 吳哲宇|自行學習|
|其他|Number Theory|Udemy|Miran Fattah|自行學習|
||微積分-導函數篇、極限篇、基礎數學篇|Udemy|Lee Bor-Jian|自行學習|
||Python for Data Structures, Algorithms, and Interviews|Udemy|Jose Portilla|自行學習|
||BlockchainBasics:Practical Approach|Udemy|Toshendra Sharma|自行學習|
||算法基礎|Coursera|Peking Universary|自行學習|
||數據結構基礎|Coursera|Peking Universary|自行學習|
||工程數學-線性代數|台大開放式課程|蘇柏青|自行學習|
||JavaScript Algorithms and Data Structures Masterclass||Colt Steele|自行學習|
||Elasticsearch 6 and Elastic Stack - In Depth and Hands On||Sundog Education by Frank Kane|自行學習|
||Blockchain A-Z™: Learn How To Build Your First Blockchain||Hadelin de Ponteves, AI Entrepreneur|自行學習|
||用 Python 理財：打造小資族選股策略||hahow FinLab|自行學習|





