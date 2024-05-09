# 📃 資料結構

在講解sync.map的資料結構前，先來聊聊在sync.map還未出來前，官方是怎麼提供解決方式的吧

#### 互斥鎖

```go
var MyMap = struct {
    sync.Mutex
    m map[string]int
}
```

就是如此暴力簡單，創建一個struct然後資料成員就是互斥鎖跟map。

#### 以及使用讀寫鎖的方式：

```go
var MyRwMap struct {
    sync.RWMutex
    m map[int]int
}
```

但這兩種方式在併發的同時會發生搶鎖的事情，於是go官方在1.9的時候發布了新的sync.map：

```go
type Map struct {
    mu Mutex
    read atomic.Value //readOnly
    dirty map[interface{}] *entry
    misses int
}
```

* mu：互斥鎖，用於保護 read 和 dirty。
* read：只讀數據，支持併發讀取（atomic.Value 類型）。如果涉及到更新操作，則只需要加鎖來保證數據安全。
* read 實際存儲的是 readOnly 結構體，內部也是一個原生 map，amended 屬性用於標記 read 和 dirty 的數據是否一致。
* dirty：讀寫數據，是一個原生 map，也就是非線程安全。操作 dirty 需要加鎖來保證數據安全。
* misses：統計有多少次讀取 read 沒有命中。每次 read 中讀取失敗後，misses 的計數值都會加 1。
