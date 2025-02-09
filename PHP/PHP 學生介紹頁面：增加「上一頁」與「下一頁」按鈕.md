# PHP 學生介紹頁面：增加「上一頁」與「下一頁」按鈕

## **功能說明**
本程式用於顯示學生介紹內容，並在頁面底部新增 **「上一頁」與「下一頁」** 按鈕，讓使用者可以方便地切換不同 `id` 的介紹內容。

## **程式碼**

```php
<?php
// 讀取選取的頁面內容
if (isset($_GET['id'])) {
    $id = $_GET['id'];

    // 查詢當前 ID 的資料
    $stmt = $Introduction->find(['id' => $id]);

    if ($stmt) {
        // 動態生成 title
        echo "<h2 class='title title-boder'><a href='?do=$do&p=$now#introduction-title' class='int-a' id='introduction-title'>學生介紹</a> <i class='bi bi-caret-right'></i> {$stmt['name']} </h2>";
        
        // 動態生成圖片
        echo "<img src='./upload/{$stmt['img']}' class='img-fluid mt-4 int-img'><hr>";

        // 內容
        echo "<pre><div class='int-text'>{$stmt['text']}</div></pre>";

        // 查詢上一個 ID（小於當前 ID 的最大值）
        $prev = $Introduction->find(['id[<]' => $id], ['ORDER' => ['id' => 'DESC']]);

        // 查詢下一個 ID（大於當前 ID 的最小值）
        $next = $Introduction->find(['id[>]' => $id], ['ORDER' => ['id' => 'ASC']]);

        // 底部返回按鈕
        echo "<a href='?do=$do&p=$now#introduction-title' class='int-back'>
                <img class='img-fluid' src='./icon/introduction.png'>
              </a>";

        // 如果有「上一個」ID，則顯示「上一頁」按鈕
        if ($prev) {
            echo "<a href='?do=$do&p=$now&id={$prev['id']}#introduction-title' class='int-prev'>⬅ 上一頁</a>";
        }

        // 如果有「下一個」ID，則顯示「下一頁」按鈕
        if ($next) {
            echo "<a href='?do=$do&p=$now&id={$next['id']}#introduction-title' class='int-next'>下一頁 ➡</a>";
        }
    } else {
        echo "<p>找不到此頁面。</p>";
    }
}
?>
```

## **解釋**
1. **查找上一個 (`prev`) 和下一個 (`next`) 的 `id`**
   - `id[<] $id`：查找比當前 `id` 小的最大值 (`DESC`)
   - `id[>] $id`：查找比當前 `id` 大的最小值 (`ASC`)

2. **顯示按鈕**
   - 如果 `$prev` 存在，則顯示 **「上一頁」** 按鈕。
   - 如果 `$next` 存在，則顯示 **「下一頁」** 按鈕。

## **效果**
當前頁面是 `id=5`：
- 若 `id=4` 存在，則顯示 `⬅ 上一頁`
- 若 `id=6` 存在，則顯示 `下一頁 ➡`

這樣你就可以在頁面底部 **增加「上一頁」與「下一頁」的按鈕** 來瀏覽不同的 `id` 內容了！ 🚀

