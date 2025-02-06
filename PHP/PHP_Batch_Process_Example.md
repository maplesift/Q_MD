
# 範例說明：批量處理資料的 PHP 程式碼解析

此範例展示如何使用 PHP 從 HTML 表單接收資料，並對資料庫執行批量刪除或更新操作。

---

## HTML 表單範例

```html
<form method="POST" action="your_script.php">
    <input type="hidden" name="id[]" value="1">
    <input type="text" name="text[]" value="Home">
    <input type="text" name="href[]" value="/home">
    <input type="checkbox" name="del[]" value="1"> 刪除

    <input type="hidden" name="id[]" value="2">
    <input type="text" name="text[]" value="About">
    <input type="text" name="href[]" value="/about">
    <input type="checkbox" name="del[]" value="2"> 刪除

    <input type="hidden" name="id[]" value="3">
    <input type="text" name="text[]" value="Contact">
    <input type="text" name="href[]" value="/contact">
    <input type="checkbox" name="del[]" value="3"> 刪除

    <button type="submit">提交</button>
</form>
```

---

## 表單提交後的 `$_POST` 資料範例

假設使用者提交表單並選擇刪除 ID 為 `1` 的資料，PHP 接收到的 `$_POST` 資料結構如下：

```php
$_POST = [
    'id' => ['1', '2', '3'],                // ID 清單
    'text' => ['Home', 'About', 'Contact'], // 對應的文字
    'href' => ['/home', '/about', '/contact'], // 對應的連結
    'del' => ['1'],                         // 被選中要刪除的 ID
];
```

---

## PHP 程式碼解析

程式碼處理的流程如下：

```php
foreach ($_POST['id'] as $idx => $id) {
    if (isset($_POST['del']) && in_array($id, $_POST['del'])) {
        // 刪除操作
        $Menu->del($id);
    } else {
        // 更新操作
        $row = $Menu->find($id); // 假設找到對應資料
        $row['text'] = $_POST['text'][$idx]; // 更新文字
        $row['href'] = $_POST['href'][$idx]; // 更新連結
        $Menu->save($row); // 保存更新
    }
}
```

### 執行邏輯

1. 當 `$idx = 0` 時：
   - `$id = '1'`
   - 因為 `1` 在 `$_POST['del']` 中，執行刪除 `$Menu->del('1')`。

2. 當 `$idx = 1` 時：
   - `$id = '2'`
   - `2` 不在 `$_POST['del']` 中，執行更新操作：
     - `$_POST['text'][1] = "About"`
     - `$_POST['href'][1] = "/about"`
     - 更新到資料庫。

3. 當 `$idx = 2` 時：
   - `$id = '3'`
   - `3` 不在 `$_POST['del']` 中，執行更新操作：
     - `$_POST['text'][2] = "Contact"`
     - `$_POST['href'][2] = "/contact"`
     - 更新到資料庫。

---

## 資料庫操作結果

假設原始資料如下：

| ID  | Text      | Href       |
|-----|-----------|------------|
| 1   | Dashboard | /dashboard |
| 2   | Services  | /services  |
| 3   | Support   | /support   |

執行程式碼後，資料變為：

| ID  | Text      | Href       |
|-----|-----------|------------|
| 2   | About     | /about     |
| 3   | Contact   | /contact   |

---

## 小結

此範例展示如何根據提交的表單資料，實現批量刪除與更新操作。  

- **刪除操作**：如果某 `id` 在 `$_POST['del']` 陣列中，則執行刪除。
- **更新操作**：否則根據表單的 `text` 和 `href` 資料更新該記錄。

此方法適用於管理後台的批量操作，如菜單項目管理或資料維護。

---
