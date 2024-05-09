# 🔧 如何使用

直接上程式碼：

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var m sync.Map
    
    // 1. 寫入
    m.Store("Adam", 18)
    m.Store("Henry", 26)
    
    // 2. 讀取
    age, _ := m.Load("Adam")
    fmt.Println(age.(int))
    
    // 3.遍歷
    m.Range(func(key, value interface{}) bool {
        name := key.(string)
        age  := value.(int)
        fmt.Println(name, age)
        return true
    })
    
    // 4. 刪除
    m.Delete("Adam")
    age, ok := m.Load("Adam")
    fmt.Println(age, ok)
    
    // 5. 讀取或寫入
    m.LoadOrStore("Henry", 25)
    age, _ := m.Load("Henry")
    fmt.Println(age)
}
```

輸出：

```go
18
Henry 26
Adam 18
<nil> false
26
```

