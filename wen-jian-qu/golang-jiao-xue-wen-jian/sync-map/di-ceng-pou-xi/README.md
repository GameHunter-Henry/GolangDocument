---
description: 在上一章節中，我們發現了syncMap在讀和刪除的情節中表現極佳，但在寫入時就變得慘不忍睹，究竟是什麼造成這部分的發生
---

# ☕ 底層剖析

我們在資料結構有稍微提過sync.Map的結構，那現在要來在更深入講，究竟是怎麼做到的

#### readOnly

```go
type readOnly struct {
    m    map[interface{}]*entry
    amended bool
}
```

sync.Map資料成員中read 實際使用的是readOnly，然後不管是dirty 還是read ，其組成map都是entry

```go
m map[interface{}] *entry
```

那entry究竟是什麼，看以下程式碼我們可以知道：

#### entry

```go
type entry struct {
    p unsafe.Pointer // *interface{}
}
```

看起來是一個指針，然後read跟dirty雖都擁有各自的key值，但其都指向同個value，也就是說，只要更動這個entry，read與dirty都會連動更改

<figure><img src="../../../../.gitbook/assets/image (23).png" alt=""><figcaption><p>sync.Map的設計設計圖</p></figcaption></figure>

{% content-ref url="store-fang-fa.md" %}
[store-fang-fa.md](store-fang-fa.md)
{% endcontent-ref %}
