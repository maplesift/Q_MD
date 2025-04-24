# PHP htmlspecialchars 函數介紹

## 作用
`htmlspecialchars` 是 PHP 的內建函數，用於將特殊字元轉換為 HTML 實體，以防止**跨站腳本攻擊（XSS, Cross-Site Scripting）**並確保內容在網頁上正確顯示。它將可能影響 HTML 結構或具有安全風險的字元轉為對應的 HTML 實體，確保這些字元以純文字形式顯示，而不是被瀏覽器解析為 HTML 標籤或腳本。

轉換的特殊字元包括：
- `&` → `&amp;` （與符號）
- `"` → `&quot;` （雙引號）
- `'` → `&#039;` （單引號）
- `<` → `&lt;` （小於符號）
- `>` → `&gt;` （大於符號）

## 語法
```php
string htmlspecialchars(
    string $string,
    int $flags = ENT_QUOTES | ENT_SUBSTITUTE | ENT_HTML401,
    ?string $encoding = null,
    bool $double_encode = true
)
```

### 參數
- `$string`：要轉換的輸入字串。
- `$flags`：控制轉換行為的標誌，常用值包括：
  - `ENT_QUOTES`：轉換單引號和雙引號。
  - `ENT_NOQUOTES`：不轉換單引號和雙引號。
  - `ENT_HTML401`：使用 HTML 4.01 標準。
  - `ENT_SUBSTITUTE`：將無效字元替換為 Unicode 替換字元。
- `$encoding`：字元編碼（預設為 `ini_get("default_charset")`，通常是 UTF-8）。
- `$double_encode`：是否對已存在的 HTML 實體再次編碼（預設為 `true`，即不重複編碼）。

### 返回值
轉換後的字串，特殊字元被替換為 HTML 實體。

## 使用場景

### 1. 防止 XSS 攻擊
當顯示用戶輸入（例如表單資料）或外部 API 資料時，必須使用 `htmlspecialchars` 防止惡意腳本執行。例如：
```php
$userInput = "<script>alert('Hacked!');</script>";
echo htmlspecialchars($userInput); // 輸出: &lt;script&gt;alert('Hacked!');&lt;/script&gt;
```
網頁顯示為 `<script>alert('Hacked!');</script>`，腳本不會執行。

### 2. 確保 HTML 顯示正確
若資料包含 `<` 或 `>` 等字元，直接輸出可能被誤認為 HTML 標籤，導致顯示錯誤。例如：
```php
$text = "if (a < b)";
echo htmlspecialchars($text); // 輸出: if (a &lt; b)
```
網頁顯示為 `if (a < b)`。

### 3. 處理 API 資料
在顯示外部 API 回應（如天氣描述）時，使用 `htmlspecialchars` 確保安全。例如：
```php
<p>天氣描述：<?= htmlspecialchars($data['weather'][0]['description']) ?></p>
```

## 範例

### 範例 1：防止 XSS 攻擊
```php
$userInput = "<script>alert('Hacked!');</script>";
?>
<p>用戶輸入：<?= htmlspecialchars($userInput) ?></p>
```
- **網頁原始碼**：
  ```html
  <p>用戶輸入：&lt;script&gt;alert('Hacked!');&lt;/script&gt;</p>
  ```
- **網頁顯示**：`用戶輸入：<script>alert('Hacked!');</script>`

### 範例 2：處理特殊字元
```php
$text = "Copyright © 2023 & HTML <tags>";
?>
<p>文字：<?= htmlspecialchars($text) ?></p>
```
- **網頁原始碼**：
  ```html
  <p>文字：Copyright © 2023 &amp; HTML &lt;tags&gt;</p>
  ```
- **網頁顯示**：`文字：Copyright © 2023 & HTML <tags>`

## 注意事項
1. **何時使用**：
   - 只要資料輸出到 HTML 內容或屬性（例如 `<p><?= $data ?></p>` 或 `<input value="<?= $data ?>">`），都應使用 `htmlspecialchars`。
2. **何時不使用**：
   - 若資料需解析為 HTML（例如 `<b>文字</b>` 要顯示為粗體），則不使用，但需嚴格過濾輸入。
3. **編碼**：
   - 預設使用 UTF-8，若使用其他編碼，需指定 `$encoding`。
4. **屬性處理**：
   - 在 HTML 屬性中，建議使用 `ENT_QUOTES`：
     ```php
     <input value="<?= htmlspecialchars($value, ENT_QUOTES) ?>">
     ```

## 與其他函數的比較
- **`htmlentities`**：轉換所有可轉換的字元，範圍比 `htmlspecialchars` 廣，但通常 `htmlspecialchars` 足夠且效能更高。
- **`strip_tags`**：移除 HTML 標籤，適用於禁止標籤的場景。
- **`addslashes`**：用於 SQL 或 JavaScript 字串轉義，不適合 HTML 輸出。

## 總結
`htmlspecialchars` 是防止 XSS 攻擊和確保 HTML 內容正確顯示的重要工具，特別適用於處理用戶輸入或外部資料。透過將特殊字元轉為 HTML 實體，它增強了網頁應用程式的安全性，是 PHP 開發中的最佳實踐。