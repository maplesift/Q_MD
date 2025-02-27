## Laravel Route 使用 .env 變數

### **問題描述**
在 Laravel 的 `.env` 檔案中，設定了一個變數：
```env
PWD = 'your_password'
```
並且在 `routes/web.php` 內使用：
```php
Route::get('index', function () {
    $pw = env("PWD");
    return view('index', $pw); // 這樣寫會出錯
});
```

### **錯誤原因**
Laravel 的 `view()` 方法要求第二個參數必須是**陣列**，所以不能直接傳遞 `$pw`。

### **正確寫法**
修改 `routes/web.php`，使用陣列來傳遞變數：
```php
Route::get('index', function () {
    $pw = env("PWD");
    return view('index', ['pw' => $pw]); // 正確傳遞變數
});
```

### **在 Blade 檔案 (`index.blade.php`) 內使用**
```blade
<p>密碼是：{{ $pw }}</p>
```

這樣就可以在 `index.blade.php` 內顯示 `.env` 檔案中的 `PWD` 變數了！🚀

