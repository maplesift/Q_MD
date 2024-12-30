
# RWD 網頁設計與 CSS 關鍵代碼

### **問題與解答**
**用戶問題**: 我想做RWD的網頁 CSS的關鍵代碼有哪些？  
**回答**: 以下是一些響應式網頁設計（RWD）的關鍵代碼與技術：

---

### **1. 使用視口設定**
在 HTML 中設置 `<meta>` 標籤：
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
這可以確保頁面在不同設備上的正確縮放。

---

### **2. 流式佈局（百分比寬度）**
使用百分比代替固定像素值，使元素隨屏幕寬度變化：
```css
.container {
  width: 100%;
  max-width: 1200px; /* 設置最大寬度 */
  margin: 0 auto; /* 置中容器 */
}

img {
  max-width: 100%; /* 圖片不超過容器寬度 */
  height: auto; /* 保持圖片比例 */
}
```

---

### **3. 使用媒體查詢**
媒體查詢是 RWD 的核心，可以根據屏幕寬度調整樣式：
```css
/* 手機 */
@media (max-width: 600px) {
  .nav-menu {
    display: none; /* 隱藏導航選單 */
  }

  .header {
    font-size: 16px; /* 調整字體大小 */
  }
}

/* 平板 */
@media (min-width: 601px) and (max-width: 1024px) {
  .nav-menu {
    display: block;
    font-size: 18px;
  }
}

/* 桌面 */
@media (min-width: 1025px) {
  .nav-menu {
    display: flex;
    justify-content: space-between;
  }
}
```

---

### **4. 彈性佈局（Flexbox 和 Grid）**
#### **Flexbox**：適合一維佈局
```css
.container {
  display: flex;
  flex-wrap: wrap; /* 自動換行 */
}

.item {
  flex: 1 1 300px; /* 基礎寬度300px，自適應填充 */
}
```

#### **Grid**：適合二維佈局
```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); /* 自動調整列數 */
  gap: 20px; /* 元素間距 */
}
```

---

### **5. 響應式文字大小**
使用 `clamp()` 動態設置文字大小：
```css
h1 {
  font-size: clamp(1.5rem, 2vw, 3rem); /* 最小1.5rem，最大3rem，根據視口調整 */
}
```

---

### **6. 隱藏不必要的內容**
針對小屏幕隱藏次要內容：
```css
@media (max-width: 600px) {
  .sidebar {
    display: none; /* 小屏幕隱藏側邊欄 */
  }
}
```

---

### **7. 響應式圖片**
使用 `picture` 元素或 `srcset` 提供不同分辨率的圖片：
```html
<picture>
  <source srcset="image-large.jpg" media="(min-width: 1024px)">
  <source srcset="image-small.jpg" media="(max-width: 600px)">
  <img src="image-default.jpg" alt="示例圖片">
</picture>
```

---

### **8. 設定可滾動的內容**
讓小屏幕上橫向滾動內容更方便：
```css
.table-container {
  overflow-x: auto; /* 允許橫向滾動 */
}
```

---

### **9. 使用 CSS 變數方便調整**
定義全局斷點和其他樣式變量：
```css
:root {
  --mobile: 600px;
  --tablet: 1024px;
}

@media (max-width: var(--mobile)) {
  body {
    background-color: lightgray;
  }
}
```

---

### **結論**
掌握以上技術能讓你創建高效的 RWD 網頁，針對不同設備提供良好的用戶體驗！
