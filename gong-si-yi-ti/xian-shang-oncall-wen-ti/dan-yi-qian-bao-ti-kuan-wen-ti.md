---
description: 當我方的注單寫入MQ，當錢包沒被核准超過五秒鐘，導致業主金額上有誤
---

# 單一錢包提款問題



![業主反應兩筆住單的 pf\_account和 round\_id](<../../.gitbook/assets/image (11).png>)

首先查詢訂單的資訊，到zio\_center\_record的single\_wallet\_transfer\_record中查詢該round\_id的狀態

發生異常將會在remark寫上err: 500

![500錯](<../../.gitbook/assets/image (20).png>)

此時詢問業主以下格式： &#x20;

<mark style="background-color:red;">你好，這二筆轉入我方時業主方回覆 500 錯誤，沒有產生注單，請問業主需要 rollback 嗎?</mark>

<mark style="background-color:red;"></mark>

如果業主回應需要，使用那筆資料的record\_id至TG輸入指令

/sw rollback {record\_id}, {record\_id},....

![兩筆單一錢包紀錄](<../../.gitbook/assets/image (8).png>)

![TG指令](<../../.gitbook/assets/image (18).png>)
