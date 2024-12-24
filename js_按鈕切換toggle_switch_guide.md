
# 使用切換按鈕 (Toggle Switch)

## 問題描述
使用者上傳了一張切換按鈕的圖片，並詢問如何實現這種效果。

## 解法一：使用 `<input>` 與 CSS 實現

### HTML 範例
```html
<div class="toggle-switch">
  <input type="checkbox" id="switch" />
  <label for="switch" class="slider">
    <span class="option">個人</span>
    <span class="option">商務版</span>
  </label>
</div>
```

### CSS 範例
```css
/* 基本樣式 */
.toggle-switch {
  position: relative;
  display: inline-block;
  width: 100px;
  height: 40px;
}

.toggle-switch input {
  display: none; /* 隱藏原生的 checkbox */
}

/* 滑塊背景 */
.slider {
  position: absolute;
  cursor: pointer;
  background-color: #ddd;
  border-radius: 20px;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  transition: background-color 0.4s;
}

/* 文字樣式 */
.slider .option {
  font-size: 14px;
  color: #aaa;
  transition: color 0.4s;
}

.slider .option:first-child {
  font-weight: bold;
}

/* 滑塊 */
.slider:before {
  content: "";
  position: absolute;
  height: 34px;
  width: 34px;
  left: 3px;
  bottom: 3px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.4s;
}

/* 切換狀態樣式 */
input:checked + .slider {
  background-color: #ccc;
}

input:checked + .slider:before {
  transform: translateX(60px);
}

input:checked + .slider .option:first-child {
  color: #aaa;
}

input:checked + .slider .option:last-child {
  color: #000;
  font-weight: bold;
}
```

## 解法二：使用 `<button>` 與 JavaScript 實現

### HTML 範例
```html
<div class="toggle-switch">
  <button id="toggleButton" class="slider" data-active="false">
    <span class="option active">個人</span>
    <span class="option">商務版</span>
  </button>
</div>
```

### CSS 範例
```css
/* 外層樣式 */
.toggle-switch {
  position: relative;
  display: inline-block;
  width: 100px;
  height: 40px;
}

.slider {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #ddd;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  border: none;
  cursor: pointer;
  outline: none;
  transition: background-color 0.4s;
}

/* 滑塊背景 */
.slider .option {
  font-size: 14px;
  color: #aaa;
  transition: color 0.4s;
}

.slider .option.active {
  color: #000;
  font-weight: bold;
}

/* 滑塊 */
.slider:before {
  content: "";
  position: absolute;
  height: 34px;
  width: 34px;
  left: 3px;
  bottom: 3px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.4s;
}

/* 切換狀態 */
.slider[data-active="true"] {
  background-color: #ccc;
}

.slider[data-active="true"]:before {
  transform: translateX(60px);
}

.slider[data-active="true"] .option:first-child {
  color: #aaa;
}

.slider[data-active="true"] .option:last-child {
  color: #000;
  font-weight: bold;
}
```

### JavaScript
```javascript
const toggleButton = document.getElementById("toggleButton");

toggleButton.addEventListener("click", () => {
  const isActive = toggleButton.getAttribute("data-active") === "true";
  toggleButton.setAttribute("data-active", !isActive);
});
```

## 結論
- 使用 `<input>` 和 `<button>` 兩種方法均可實現切換按鈕效果。
- 若使用 `<button>`，需要 JavaScript 控制切換邏輯。
- 可以根據需求選擇合適的方式實現。

需要更多幫助，隨時告訴我！
