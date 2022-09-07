---
description: (已結案)
---

# 免費贈予活動修正

![](<../.gitbook/assets/image (1).png>)

免費贈予的已派發金額需有一個額外api，功能主要是傳送此活動各平台id以及派發金額這兩樣



```yaml
/free_spin/detail/list
```

```yaml
/free_spin/detail/record_id
```

這兩隻api發生500錯，寫log追蹤一下
