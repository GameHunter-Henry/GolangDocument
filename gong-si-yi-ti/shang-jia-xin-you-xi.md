# 上架新遊戲

#### 先至 <mark style="background-color:red;">**Rich88\_遊戲問題回報**</mark>** 發訊息**

Rich88 測試站將於今日(8/30) 15:00\~16:00 停機更新&#x20;

Rich88 正式站將於明日(8/31) 09:00\~10:00 停機更新&#x20;

Rich88 DEMO站將於明日(8/31) 10:00\~11:00 停機更新&#x20;

更新項目:

1.新遊戲-魔龍傳奇&#x20;

2.跑馬燈優化-遊戲名稱若未翻譯顯示英文翻譯&#x20;

3.麻將王-BET調整(麻將王還是下架狀態)

4.海盜桶閃退問題修正

\----------------------------------------------------------------------------------

#### 先去後台關閉伺服器

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

\----------------------------------------------------------------------------------

#### 進入[http://gambling.baoxitech.com/Login/](http://gambling.baoxitech.com/Login/) 

1. 使用彩票公測站，帳：ze\_a3，密：123456
2. 檢查是否進入維護畫面

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

\----------------------------------------------------------------------------------

五分鐘後，進入機器端 ssh \[beta's account/offical's account]@ \[beta's IP / offical's IP]

```
ssh game@18.180.74.201
```

接著到目錄/home/game/projects，然後依照center -> engine -> cms順序做以下操作：

1. 先git pull
2. git log 觀察是否與lottery\_release的history一樣
3. 確定正確下 sudo make main

\----------------------------------------------------------------------------------

#### Telegram 群組中檢查上鎖玩家

1. /oncall checkmoneylock
2. /oncall unlock \[account, account]

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

\----------------------------------------------------------------------------------

#### 上架遊戲

到<mark style="background-color:green;">遊戲管理/遊戲列表</mark>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

除了正式站，其餘的去測試平台底下啟用該遊戲以進行測試

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

\----------------------------------------------------------------------------------

使用維護環境測試遊戲

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption><p>beta站的測試環境</p></figcaption></figure>

測試的項目有：

1. 共開
2. 快開
3. slot
4. 剛上的遊戲

\----------------------------------------------------------------------------------

#### 收尾

確認都完成後再去開啟伺服器

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

然後再去RD發大財通知後端更新完畢

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

等前端也通知後，到Rich88\_遊戲問題回報發通知

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><a href="http://nas.baoxitech.com:4999/web/#/22?page_id=1854">http://nas.baoxitech.com:4999/web/#/22?page_id=1854</a></summary>



</details>
