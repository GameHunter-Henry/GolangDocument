---
description: sync.map.store
---

# 💾 Store 方法

```go
func (m *Map) Store(key, value interface{}) {
    // 上半部：調用 Load 方法檢查 m.read 中是否存在這個元素。若存在，且沒有被標記爲刪除狀態，則嘗試存儲。
    read, _ := m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok && e.tryStore(&value) {
        return
    }
    
    // 下半部：若該元素不存在或已經被標記爲刪除狀態，則繼續走到下面流程
    m.mu.Lock()
    read, _ = m.read.Load().(readOnly)
    
    if e, ok := read.m[key]; ok {
        if e.unexpungeLocked() {
            m.dirty[key] = e
        }
        e.storeLocked(&value)
    } else if e, ok := m.dirty[key]; ok {
        e.storeLocked(&value)
    } else {
        if !read.amended {
            m.dirtyLocked()
            m.read.Store(readOnly{m: read.m, amended: true})
        }
        m.dirty[key] = newEntry(value)
    }
    m.mu.Unlock()
}   
```

{% tabs %}
{% tab title="上半部" %}
先將read進行載入並斷言成readOnly的struct

```go
read, _ := m.read.Load().(readOnly)
```

然後再將看read裡的map是否有該元素。若存在，且沒標記為刪除狀態，則嘗試儲存。

```go
if e, ok := read.m[key]; ok && e.tryStore(&value) {
    return
}
```

```go
func (e *entry) tryStore(i *interface{}) bool {
	for {
		// 先載入該value的pointer
		p := atomic.LoadPointer(&e.p)
		// 如果此pointer被標記刪除回傳 false
		if p == expunged {
			return false
		}
		// 這邊嘗試把entry的pointer指向新的值
		if atomic.CompareAndSwapPointer(&e.p, p, unsafe.Pointer(i)) {
			return true
		}
	}
}
```

其實tryStore就是滿常見的 for迴圈處理CAS操作來更新entry指標指向新的值
{% endtab %}

{% tab title="下半部" %}


從這裡就是走dirty的流程，開頭就毫不留情直接上互斥鎖來保證數據的安全，這也是導致性能變差的第一步。

#### 此時這邊會做三種處理判斷：

1. read底下的m(map)存在該元素，但被標記成已刪除
2. read不存在該元素，但dirty存在
3. read和dirty都不存在該元素

\--------------------------------------------------------------------------------------------_<mark style="background-color:green;">**read底下的m(map)存在該元素，但被標記成已刪除**</mark>_

這說明了dirty不等於nil(即為dirty中確定不存在該元素)，那我們該做的就是-- <mark style="color:yellow;">**將元素狀態從已刪除(expunged)改為nil**</mark>，並且將該元素插入dirty中，然後更新entry.p為此元素

```go
if e, ok := read.m[key]; ok {
    if e.unexpungeLocked() {
        m.dirty[key] = e
    }
    e.storeLocked(&value)
}
```

_<mark style="background-color:green;">**read不存在該元素，但dirty存在**</mark>_

那這樣就直接去更新entry.p為此元素即可(read與dirty都是共用同個entry)

```go
else if e, ok := m.dirty[key]; ok {
    e.storeLocked(&value)
}
```

_<mark style="background-color:green;">**read和dirty都不存在該元素**</mark>_

這已經是最後一步了，走到這一步基本上是排除大多數例外狀況，新增的這一元素也是從來沒寫進過這個map中，那就會依照正確的新增流程走。

1. 先確定read跟dirty是否資料一致，如果是，創建一個這個元素新的entry並指dirty，如果不是，走以下步驟
2. 在m.dirtyLocked()中，如果dirty為空，創建一個容量跟read相同的entry map，並判斷read中未被標記為刪除的元素並將元素複製進dirty中
3. 將read更新數據並將amended字符(是否與dirty相同)改成true

```go
else {
    if !read.amended {
        m.dirtyLocked()
        m.read.Store(readOnly{m: read.m, amended: true})
    }
    m.dirty[key] = newEntry(value)
}
 m.mu.Unlock()
```

最後在把互斥鎖解除，存入的動作就大功告成
{% endtab %}
{% endtabs %}
