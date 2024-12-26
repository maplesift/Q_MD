
# Bootstrap 5 Carousel 按鈕自適應位置

以下提供了一個方法，讓 Bootstrap 5 的輪播圖指示器按鈕隨螢幕寬度自動調整位置，始終位於圖片的下方，而無需額外切換版型。

## CSS 設計

```css
.carousel {
    position: relative; /* 為容器設定相對定位 */
}

.carousel-indicators {
    position: absolute; /* 絕對定位，相對於父容器 */
    bottom: 10px; /* 始終位於圖片下方 */
    left: 50%; /* 水平居中 */
    transform: translateX(-50%); /* 修正居中位置 */
    display: flex;
    justify-content: center;
    gap: 10px; /* 調整按鈕間距 */
}

.carousel-indicators .cas {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: #333; /* 指示器顏色 */
    margin: 0;
}

.carousel-inner img {
    display: block;
    width: 100%; /* 圖片全寬 */
    height: auto; /* 高度自適應 */
}
```

## HTML 結構

以下是完整的 HTML 範例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carousel Example</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        .carousel {
            position: relative;
        }

        .carousel-indicators {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .carousel-indicators .cas {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #333;
        }

        .carousel-inner img {
            display: block;
            width: 100%;
            height: auto;
        }
    </style>
</head>
<body>
    <div id="demo" class="carousel slide" data-bs-ride="carousel">
        <!-- Indicators/dots -->
        <div class="carousel-indicators">
            <button type="button" data-bs-target="#demo" data-bs-slide-to="0" class="active cas"></button>
            <button type="button" data-bs-target="#demo" data-bs-slide-to="1" class="cas"></button>
            <button type="button" data-bs-target="#demo" data-bs-slide-to="2" class="cas"></button>
        </div>

        <!-- The slideshow/carousel -->
        <div class="carousel-inner">
            <div class="carousel-item active">
                <img src="../001.jpg" alt="Los Angeles" class="d-block w-100">
            </div>
            <div class="carousel-item">
                <img src="../002.jpg" alt="Chicago" class="d-block w-100">
            </div>
            <div class="carousel-item">
                <img src="../003.jpg" alt="New York" class="d-block w-100">
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

## 說明

1. **指示器位置調整**
   - 使用 `position: absolute` 將指示器固定在輪播圖容器內，位置由 `bottom` 控制。
   - 透過 `left: 50%` 和 `transform: translateX(-50%)` 實現水平居中。

2. **圖片大小調整**
   - 圖片使用 `width: 100%` 和 `height: auto`，確保在任何螢幕上都能正確顯示。

3. **適用於所有螢幕尺寸**
   - 不需要媒體查詢，按鈕位置會自動根據圖片大小調整，適用於桌面和手機裝置。

此設計簡化了響應式樣式的處理，實現了一套通用的解決方案。
