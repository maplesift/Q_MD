### 問題：CSS `transition` 無效

```css
.s {
	display: none;
	/* background-color: greenyellow; */
	transition: 0.3s;
}

div.ww:hover>div.s {
	display: block;
	transition: 0.3s;
}
```

**原因：**
`display` 屬性無法進行 `transition`，因此 `display: none;` 與 `display: block;` 之間的變化無法有動畫效果。

---

## **解決方案 1：使用 `opacity` 和 `visibility`**
```css
.s {
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0.3s ease;
}

div.ww:hover > div.s {
    opacity: 1;
    visibility: visible;
}
```

**為什麼這樣有效？**
1. `opacity: 0;` 讓元素透明，而 `opacity: 1;` 讓元素顯示，可平滑過渡。
2. `visibility: hidden;` 確保 `opacity: 0;` 時，元素無法被點擊。
3. `transition` 讓 `opacity` 有動畫效果。

---

## **解決方案 2：使用 `max-height` 來動畫顯示內容**

- 如果你還想讓 .s 元素在 display: none; 時完全不佔空間，可以使用 height 或 max-height 來動畫化高度，例如：

```css
.s {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease-out;
}

div.ww:hover > div.s {
    max-height: 200px; /* 根據內容調整 */
}


```



### **需求：讓 `max-height` 根據項目數量自動調整**

#### **方法 1：PHP 計算 `max-height`**
```php
<?php
$items = ["項目1", "項目2", "項目3"]; // 模擬從資料庫獲取的項目
$itemCount = count($items);
$maxHeight = $itemCount * 200; // 每個項目 200px
?>

<style>
    .s {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.3s ease-out;
    }

    div.ww:hover > div.s {
        max-height: <?php echo $maxHeight; ?>px;
    }
</style>

<div class="ww">
    <div class="s">
        <?php foreach ($items as $item) : ?>
            <div><?php echo $item; ?></div>
        <?php endforeach; ?>
    </div>
</div>
```
**優點**：直接由 PHP 計算 `max-height`，適用於靜態內容。

---

#### **方法 2：使用 JavaScript 讓 `max-height` 自動適應內容高度**
```php
<?php
$items = ["項目1", "項目2", "項目3"]; // 假設這是從資料庫取出的
?>

<style>
    .s {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.3s ease-out;
    }
</style>

<div class="ww">
    <div class="s">
        <?php foreach ($items as $item) : ?>
            <div><?php echo $item; ?></div>
        <?php endforeach; ?>
    </div>
</div>

<script>
    document.addEventListener("DOMContentLoaded", function () {
        const sElement = document.querySelector(".s");

        document.querySelector(".ww").addEventListener("mouseenter", function () {
            sElement.style.maxHeight = sElement.scrollHeight + "px"; // 設為內容真實高度
        });

        document.querySelector(".ww").addEventListener("mouseleave", function () {
            sElement.style.maxHeight = "0"; // 隱藏時歸零
        });
    });
</script>
```

**優點**：適用於 AJAX 動態載入的內容，不需要 PHP 預先計算。

---

## **結論**
- **內容是 PHP 端固定的** → **用 PHP 計算 `max-height`**（方法 1）。
- **內容是前端動態變化的** → **用 JavaScript 設定 `max-height`**（方法 2）。

這樣就可以讓 `.s` 根據內容動態調整高度，並且保留平滑動畫效果！😊

