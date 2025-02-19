在 Laravel 中，通常不會直接使用 `?do=title` 這種方式來傳遞 GET 參數，而是使用 **路由名稱** 或 **動態路由參數** 來處理。例如，你可以這樣設計路由：

### 1. **使用具名路由**
Laravel 提供了 **具名路由**，可以使用 `route()` 來生成 URL，這樣比較清晰、可維護：
```php
<a href="{{ route('admin.title') }}" class="highlight-hover del-underline">
    標題圖片管理
</a>
<a href="{{ route('admin.news') }}" class="highlight-hover del-underline">
    更新消息管理
</a>
<a href="{{ route('admin.comic') }}" class="highlight-hover del-underline">
    官方漫畫管理
</a>
```
然後在 `routes/web.php` 定義路由：
```php
Route::get('/admin/title', function () {
    return view('admin.title');
})->name('admin.title');

Route::get('/admin/news', function () {
    return view('admin.news');
})->name('admin.news');

Route::get('/admin/comic', function () {
    return view('admin.comic');
})->name('admin.comic');
```

### 2. **使用動態路由參數**
如果 `?do=` 參數只是用來選擇不同的頁面，你可以改用動態路由：
```php
<a href="{{ route('admin.page', ['page' => 'title']) }}" class="highlight-hover del-underline">
    標題圖片管理
</a>
<a href="{{ route('admin.page', ['page' => 'news']) }}" class="highlight-hover del-underline">
    更新消息管理
</a>
<a href="{{ route('admin.page', ['page' => 'comic']) }}" class="highlight-hover del-underline">
    官方漫畫管理
</a>
```
在 `routes/web.php` 定義：
```php
Route::get('/admin/{page}', function ($page) {
    return view("admin.$page");
})->name('admin.page');
```
這樣 Laravel 會自動解析 `/admin/title`、`/admin/news`、`/admin/comic`，並載入對應的 Blade 模板。

### 3. **保留 Query String 但用 Laravel 方式**
如果你仍然想保留 `?do=` 這種寫法，也可以這樣寫：
```php
<a href="{{ url('/admin') }}?do=title" class="highlight-hover del-underline">
    標題圖片管理
</a>
```
然後在 Controller 裡處理：
```php
Route::get('/admin', function (Request $request) {
    $do = $request->query('do', 'title'); // 預設 'title'
    return view("admin.$do");
});
```
這樣 `/admin?do=comic` 仍然可以運作，但這種方式比較少見，建議用 **具名路由** 或 **動態路由** 來管理。

---
### 結論：
如果你要轉換為 Laravel，建議使用 **具名路由** 或 **動態路由**，這樣程式碼會更清楚，避免直接使用 `?do=` 來控制頁面。

