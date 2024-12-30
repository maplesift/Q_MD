# JavaScript 程式碼解析

以下是對控制「回到頂端」按鈕的 JavaScript 程式碼解析，幫助理解程式邏輯與功能。

---

## **主要功能**
此段程式碼的目的是控制「回到頂端」按鈕的顯示與動畫效果：
- 當頁面滾動超過 10% 時，按鈕從右下角彈出並顯示。
- 當滾動回到 10% 以下時，按鈕消失並滑回初始位置。

---

## **程式碼解析**

### 1. **監聽 DOM 加載完成**
```javascript
document.addEventListener('DOMContentLoaded', () => {
```
- `DOMContentLoaded` 事件確保程式碼在 HTML 結構加載完成後執行，避免操作尚未載入的 DOM 元素。

---

### 2. **取得按鈕元素**
```javascript
const backToTop = document.getElementById('back-to-top');
```
- 使用 `getElementById` 方法，取得具有 `id="back-to-top"` 的按鈕元素，方便後續操作。

---

### 3. **監聽滾動事件**
```javascript
window.addEventListener('scroll', () => {
```
- 當使用者在頁面上滾動時，觸發 `scroll` 事件，執行回調函數以更新按鈕的顯示狀態。

---

### 4. **計算滾動百分比**
```javascript
const scrollTop = window.scrollY || document.documentElement.scrollTop;
const windowHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
const scrollPercent = (scrollTop / windowHeight) * 100;
```
- **`window.scrollY`**：取得當前滾動的垂直像素數。
- **`document.documentElement.scrollTop`**：某些瀏覽器使用這個屬性獲取滾動位置。
- **`document.documentElement.scrollHeight`**：取得整個頁面的高度（包括未顯示的部分）。
- **`document.documentElement.clientHeight`**：取得可視區域的高度。
- **`(scrollTop / windowHeight) * 100`**：計算滾動的百分比，用於判斷顯示或隱藏按鈕。

---

### 5. **按鈕顯示與隱藏邏輯**
```javascript
if (scrollPercent > 10) {
    backToTop.style.opacity = '1';       // 設置完全不透明
    backToTop.style.visibility = 'visible'; // 顯示按鈕
    backToTop.style.transform = 'translateY(0)'; // 回到原始位置
} else {
    backToTop.style.opacity = '0';       // 設置完全透明
    backToTop.style.visibility = 'hidden'; // 隱藏按鈕
    backToTop.style.transform = 'translateY(50px)'; // 下移到初始位置
}
```

- **當滾動超過 10% 時**：
  - `opacity: 1`：按鈕完全不透明，顯示出來。
  - `visibility: visible`：設定為可見，確保按鈕可以被點擊。
  - `transform: translateY(0)`：按鈕從初始位置滑回到原位。

- **當滾動低於 10% 時**：
  - `opacity: 0`：按鈕完全透明，實現隱藏效果。
  - `visibility: hidden`：設定為不可見，防止透明按鈕仍能被點擊。
  - `transform: translateY(50px)`：將按鈕下移 50px，回到初始位置。

---

### **過渡效果依賴 CSS**
```css
transition: opacity 0.3s, transform 0.3s, visibility 0.3s;
```
- 利用 `transition` 屬性，讓按鈕的透明度（`opacity`）、位移（`transform`）、以及可見性（`visibility`）切換時產生平滑的動畫效果。

---

## **小結**
- 該代碼透過滾動事件動態控制按鈕的顯示與隱藏，結合 CSS 動畫效果提升用戶體驗。
- `opacity` 與 `transform` 控制動畫效果，`visibility` 確保透明按鈕不可被點擊。
- 使用百分比計算頁面滾動比例，實現精確的顯示與隱藏行為。

---

如果需要更深入的優化或其他功能實現，隨時提供問題！

