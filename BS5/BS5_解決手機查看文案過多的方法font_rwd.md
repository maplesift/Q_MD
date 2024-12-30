# 解決手機查看文案過多的方法

為了解決文案在手機上過多的問題，可以利用 **Bootstrap 5 的響應式設計工具**，例如 `d-none` 和 `d-md-block` 等類別，來根據螢幕大小顯示不同的內容。

---

## 方法 1: 利用 `d-none` 和 `d-*` 類別

使用兩段不同的文字版本，一段顯示完整文案，另一段顯示精簡版本，然後根據螢幕尺寸顯示適合的版本。

```html
<div class="d-none d-md-block">
    <!-- 桌面版完整文案 -->
    <p>這是一段詳細的說明文字，包含所有的資訊與細節，適合使用電腦查看。</p>
</div>
<div class="d-block d-md-none">
    <!-- 手機版重點文案 -->
    <p>這是一段簡短的重點文字，方便手機用戶快速瀏覽。</p>
</div>
```

---

## 方法 2: 使用 `data-bs-toggle` 折疊文字

如果希望用戶能在手機上展開查看詳細內容，可以使用 Bootstrap 的折疊功能。

```html
<p class="d-md-none">
    這是一段簡短的重點文字，
    <a class="text-decoration-underline" data-bs-toggle="collapse" href="#moreDetails" role="button" aria-expanded="false" aria-controls="moreDetails">
        查看詳細
    </a>
</p>
<div class="collapse" id="moreDetails">
    <p>這是一段詳細的說明文字，包含所有的資訊與細節。</p>
</div>
<div class="d-none d-md-block">
    <p>這是一段詳細的說明文字，包含所有的資訊與細節，適合使用電腦查看。</p>
</div>
```

---

## 方法 3: 動態裁切文字與工具提示

在手機版顯示前幾行文字，並加上工具提示（tooltip）供用戶查看完整內容。

```html
<div class="text-truncate d-md-none" style="max-width: 200px;" title="這是一段詳細的說明文字，包含所有的資訊與細節。">
    這是一段詳細的說明文字...
</div>
<div class="d-none d-md-block">
    <p>這是一段詳細的說明文字，包含所有的資訊與細節，適合使用電腦查看。</p>
</div>
```

---

## 方法 4: 利用 JavaScript 動態更新

如果需求更複雜，可以使用 JavaScript 偵測螢幕寬度，並根據條件替換文字內容。

```html
<p id="description"></p>

<script>
    const description = document.getElementById('description');
    if (window.innerWidth >= 768) {
        // 電腦版
        description.textContent = '這是一段詳細的說明文字，包含所有的資訊與細節。';
    } else {
        // 手機版
        description.textContent = '這是一段簡短的重點文字。';
    }
</script>
```

---

## 建議

- **視覺效果最佳化**：搭配適合的排版（如字體大小、行高）提升閱讀體驗。
- **測試與調整**：確保在各種裝置上都能正常運作。

你可以根據專案需求試試看上述方法，選擇最適合的解決方案！
