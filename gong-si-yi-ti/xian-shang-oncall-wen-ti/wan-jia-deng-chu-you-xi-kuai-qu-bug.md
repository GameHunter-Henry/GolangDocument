---
description: 玩家已登出遊戲，但在金流上會顯示 Player is playing game
---

# 玩家登出遊戲快取bug

![客服查詢到的玩家金流狀態](<../../.gitbook/assets/image (15).png>)

發生原因為，玩家在進行遊戲時因為某些原因，如：遊戲崩潰、非正當登出遊戲等，造成玩家在Redis的資料沒被刪除



解決辦法：

1. 先打api 查詢玩家快取：<mark style="background-color:yellow;">`GET`</mark> [<mark style="background-color:yellow;">`{{center}}`</mark>](http://3.115.35.142:8090)<mark style="background-color:yellow;">`/v1/trust/cache/player`</mark>
2. 刪除快取：

&#x20;     a. `DELETE` [`{{center}}`](http://3.115.35.142:8090)`/v1/trust/cache/delete`&#x20;

&#x20;     b. 主要刪除下列二前綴的鍵&#x20;

&#x20;     c. ppocp\_player\_playing\_oid\_&#x20;

&#x20;     d. pprncp\_player\_playing\_room\_no\_
