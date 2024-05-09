# 📉 效能比對

現在我們來進行三種map的寫、讀、刪除效率比較

### 寫：

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

寫的部分 syncMap是最慢的，而最快的是互斥鎖的map

### 讀：

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

再讀的狀況下可以發現情況剛好相反過來，syncMap的讀寫速度遠遠少於其他兩種map

### 刪除：

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

刪除的也是，syncMap的速度也是很快，那從以上分析可得知

### sync.Map的特性是

* 在讀和刪的場景上性能是最佳，領先一倍以上
* 在寫入的場景上性能非常差
