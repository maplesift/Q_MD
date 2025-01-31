# 使用 PHP 與 MySQL 透過 foreach 生成頁面連結

## 介紹
本篇說明如何使用 PHP 與 MySQL 來從資料庫中讀取頁面資料，並透過 `foreach` 迴圈生成超連結，讓使用者點擊後能查看對應的頁面內容。

## **步驟**

### **1. 建立資料表**
首先，確保你的 MySQL 資料庫中有一個 `pages` 資料表，包含 `id`、`title` 和 `content` 欄位：

```sql
CREATE TABLE pages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL
);
```

然後，你可以插入一些範例資料：

```sql
INSERT INTO pages (title, content) VALUES ('Page One', 'This is page 1');
INSERT INTO pages (title, content) VALUES ('Page Two', 'This is page 2');
INSERT INTO pages (title, content) VALUES ('Page Three', 'This is page 3');
```

### **2. 建立 `index.php` 來顯示頁面列表**

```php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=your_database;charset=utf8", "root", "");

// 取得所有頁面的 id 和標題
$sql = "SELECT id, title FROM pages";
$stmt = $pdo->query($sql);
$pages = $stmt->fetchAll(PDO::FETCH_ASSOC);
?>

<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>選擇頁面</title>
</head>
<body>

    <h1>選擇一個頁面：</h1>
    <ul>
        <?php foreach ($pages as $page): ?>
            <li>
                <a href="index.php?id=<?= $page['id'] ?>">
                    <?= htmlspecialchars($page['title']) ?>
                </a>
            </li>
        <?php endforeach; ?>
    </ul>

    <hr>

    <?php
    // 讀取選取的頁面內容
    if (isset($_GET['id'])) {
        $id = $_GET['id'];
        $sql = "SELECT * FROM pages WHERE id = :id";
        $stmt = $pdo->prepare($sql);
        $stmt->execute([':id' => $id]);
        $page = $stmt->fetch(PDO::FETCH_ASSOC);

        if ($page) {
            echo "<h1>{$page['title']}</h1>";
            echo "<p>{$page['content']}</p>";
        } else {
            echo "<p>找不到此頁面。</p>";
        }
    }
    ?>

</body>
</html>
```

### **3. 運作方式解析**

1. 連接 MySQL 資料庫，並讀取 `pages` 表中的所有 `id` 和 `title`。
2. 使用 `foreach` 生成超連結列表，讓使用者點擊來選擇不同的頁面。
3. 當使用者點擊某個頁面後，透過 `$_GET['id']` 取得 `id`，然後從資料庫查詢對應內容並顯示。

### **4. 頁面輸出示例**
假設資料庫內有以下資料：

| id | title      | content         |
|----|-----------|----------------|
| 1  | Page One  | This is page 1  |
| 2  | Page Two  | This is page 2  |
| 3  | Page Three | This is page 3 |

那麼瀏覽器會顯示：
```
選擇一個頁面：
- Page One (點擊連結 index.php?id=1)
- Page Two (點擊連結 index.php?id=2)
- Page Three (點擊連結 index.php?id=3)
```
當使用者點擊 `"Page Two"`，畫面將顯示：
```
Page Two
This is page 2
```

### **結論**
此方法可以動態生成頁面列表，並允許使用者點擊後顯示對應的內容。這樣的架構適用於部落格、文件管理系統等應用。如果有更進一步的需求，可以擴充功能，如搜尋、分頁或分類過濾等。

