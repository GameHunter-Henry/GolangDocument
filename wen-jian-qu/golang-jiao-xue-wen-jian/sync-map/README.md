---
cover: >-
  https://images.unsplash.com/photo-1548839140-29a749e1cf4d?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw1fHxzeW5jfGVufDB8fHx8MTY2MTc1OTA1MA&ixlib=rb-1.2.1&q=80
coverY: 0
---

# sync Map

golang map在執行緒中進行操作是有風險的(兩個執行續同時對同個map進行讀合寫)，正常來說我們有兩種方法：

1. 在map裡增加一把大鎖
2. 把一個map分成很多個小map，對key進行hash

但第一點會造成粒度較大，影響效率，第二點複雜度較高，容易出錯

> 於是乎，sync.map出來了

sync.map使用空間換時間的方式，創建read和dirty兩個map來進行讀寫分離，有以下優勢：

* 多個goroutine的併發是安全的
* 大多數代碼使用原生map，已獲得更好類型的安全性和維護性
