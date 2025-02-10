# PHP 中 if ($stmt) 的判斷條件說明

在 PHP 中，`if ($stmt) {}` 判斷條件的真假取決於 `$stmt` 的值是否為「**真值（truthy）**」或「**假值（falsy）**」。

## 何時為 `true`（真值）
只要 `$stmt` 不是空值或 `false`，就會被判斷為 `true`，例如：
- **非空陣列**（例如：`['id' => 1, 'name' => 'John']`）
- **非零數值**（例如：`1, -1, 0.5, -0.5`）
- **非空字串**（例如：`"hello"`）
- **物件（object）**
- **資源型態（resource）**

你的 `$stmt = $Introduction->find(['id'=>$id]);` 可能返回的是一個**關聯陣列**，所以只要 `$stmt` 不是 `null` 或空陣列，`if ($stmt)` 會進入條件執行 `{}` 內的程式碼。

---

## 何時為 `false`（假值）
PHP 會把以下值視為 `false`：
- `null`
- `false`
- 整數 `0`
- 浮點數 `0.0`
- 空字串 `""`
- 空陣列 `[]`
- `未初始化的變數`
- `unset()` 後的變數

如果 `$Introduction->find(['id'=>$id])` 找不到符合條件的資料，通常會回傳 `null` 或 `false`，這時候 `if ($stmt) {}` 內的程式碼就不會執行。

---

## 建議的判斷方式
如果你明確知道 `$stmt` 可能回傳 `false` 或 `null`，可以改用更嚴謹的比較，例如：
```php
if (!empty($stmt)) {
    // $stmt 不是空的，可以安全使用
    echo $stmt['name'];
}
```
或是：
```php
if ($stmt !== null) {
    echo $stmt['name'];
}
```
這樣可以避免 `false` 或 `null` 造成錯誤。

