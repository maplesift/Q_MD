## 資料表調整與 PHP 代碼更新

### 1. 在資料表新增 `grand_total` 欄位
```sql
ALTER TABLE `Total` ADD `grand_total` INT NOT NULL DEFAULT 0;
```

### 2. 修改 PHP 代碼
```php
if (!isset($_SESSION['total'])) {
    $today = date("Y-m-d");
    $total = $Total->find(['date' => $today]);
    if ($total) {
        // 更新今日瀏覽數
        $total['total']++;
        $Total->save($total);
    } else {
        // 新增當天的紀錄
        $Total->save(['date' => $today, 'total' => 1]);
    }
        // 更新累積瀏覽數
        $grandTotal = $Total->find(['id' => 1]); // 假設只存一筆累積數據
        $grandTotal['grand_total']++;
        $Total->save($grandTotal);
    $_SESSION['total'] = 1;
}
```

### 3. 修改顯示部分
```php
今日瀏覽: <?=$Total->find(['date' => date("Y-m-d")])['total'];?>
| 累積瀏覽: <?=$Total->find(['id' => 1])['grand_total'];?>
