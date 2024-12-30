# Bootstrap 5 圖示使用教學

Bootstrap 5 本身不內建圖示功能，因此需要使用額外的圖示資源，例如官方提供的 **Bootstrap Icons**。

## 安裝 Bootstrap Icons

### 1. 使用 CDN
在你的 HTML 的 `<head>` 標籤中加入以下連結：
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap-icons/font/bootstrap-icons.css" rel="stylesheet">
```

### 2. 下載圖示檔案
前往 [Bootstrap Icons 官方網站](https://icons.getbootstrap.com/) 下載完整的圖示檔案，並在你的專案中引入 `bootstrap-icons.css` 和對應的字型檔案。

---

## 使用方法
引入圖示資源後，你可以使用 `<i>` 標籤，並加上對應的圖示類別名。

### 範例
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bootstrap Icons 範例</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons/font/bootstrap-icons.css" rel="stylesheet">
</head>
<body>
    <h1>Bootstrap Icons 範例</h1>
    
    <!-- 單一圖示 -->
    <i class="bi bi-alarm"></i> Alarm 圖示

    <!-- 搭配按鈕 -->
    <button class="btn btn-primary">
        <i class="bi bi-plus"></i> 新增
    </button>

    <!-- 搭配文字 -->
    <p><i class="bi bi-heart-fill text-danger"></i> 收藏</p>
</body>
</html>
```

---

## 調整圖示樣式

### 1. 大小
使用 Bootstrap 的文字大小工具調整圖示大小，例如：
```html
<i class="bi bi-alarm fs-1"></i> <!-- fs-1 是字體大小 -->
<i class="bi bi-alarm fs-3"></i> <!-- fs-3 比較小 -->
```

### 2. 顏色
可以使用 Bootstrap 的文字顏色工具：
```html
<i class="bi bi-heart-fill text-danger"></i> <!-- 紅色 -->
<i class="bi bi-heart-fill text-success"></i> <!-- 綠色 -->
```

### 3. 額外樣式
若需要旋轉或動畫效果，可以用 CSS：
```html
<i class="bi bi-arrow-clockwise" style="transform: rotate(90deg);"></i> <!-- 旋轉 -->
<i class="bi bi-arrow-repeat" style="animation: spin 2s linear infinite;"></i> <!-- 動畫 -->
<style>
    @keyframes spin {
        from { transform: rotate(0deg); }
        to { transform: rotate(360deg); }
    }
</style>
```

---

## 圖示庫參考
前往 [Bootstrap Icons](https://icons.getbootstrap.com/) 查看完整的圖示列表。你可以直接搜尋需要的圖示名稱，並將對應的類別名稱加入到 `<i>` 標籤中即可使用。

若有其他問題或需要更多範例，隨時與我聯繫！

