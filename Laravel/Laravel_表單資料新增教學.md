# Laravel 表單資料新增教學 (取代傳統 PHP 的 api/insert.php)

### 前言

- 在 Laravel 中，
- 你需要將 `api/insert.php` 的功能遷移到 Laravel 的 路由 (routes) 和 控制器 (Controller) 中，
- 而不再直接使用 PHP 文件處理請求。以下是遷移步驟：

## 1. 設定路由 (routes)
打開 `routes/web.php` (或 `routes/api.php` 如果是 API)，新增一個 `POST` 路由：

```php
use App\Http\Controllers\UserController;
use Illuminate\Support\Facades\Route;

Route::post('/insert', [UserController::class, 'insert']);
```

這樣 `POST /insert` 就會被送到 `UserController` 的 `insert` 方法處理。

---

## 2. 建立控制器 (Controller)

如果 `UserController` 尚未建立，可以用 Artisan 指令建立：

```sh
php artisan make:controller UserController
```

然後打開 `app/Http/Controllers/UserController.php`，在其中新增 `insert` 方法：

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;
use App\Models\User; // 假設有 User 模型

class UserController extends Controller
{
    public function insert(Request $request)
    {
        // 驗證輸入資料
        $request->validate([
            'acc' => 'required|string|unique:users,acc',
            'pw'  => 'required|string|min:6',
            'pw2' => 'required|string|same:pw',
        ]);

        // 建立新使用者
        User::create([
            'acc' => $request->acc,
            'password' => Hash::make($request->pw),
        ]);

        return redirect()->back()->with('success', '新增成功！');
    }
}
```

這裡使用 `Request` 來接收表單資料，並使用 Laravel 的驗證功能 (`validate()`) 確保輸入正確。

---

## 3. 更新前端表單

將原本 PHP 的 `action="api/insert.php"` 改成 Laravel 的路由：

```html
<form action="{{ url('/insert') }}" method="post">
    @csrf <!-- Laravel 需要 CSRF Token 保護 -->
    <table>
        <tr>
            <td class="title">帳號：</td>
            <td><input type="text" name="acc" class="form-control"></td>
        </tr>
        <tr>
            <td class="title">密碼：</td>
            <td><input type="password" name="pw" class="form-control"></td>
        </tr>
        <tr>
            <td class="title">確認密碼：</td>
            <td><input type="password" name="pw2" class="form-control"></td>
        </tr>
    </table>
    <div class="cent">
        <input type="submit" value="新增" class="btn btn-success">
        <input type="reset" value="重置" class="btn btn-info">
    </div>
</form>
```

> **注意**：Laravel 需要 `@csrf` 來保護表單，不然請求會被拒絕。

---

## 4. 設定 User 模型

假設你的資料庫有 `users` 表，你需要一個 `User` 模型來儲存資料：

```sh
php artisan make:model User
```

然後打開 `app/Models/User.php`，允許 `acc` 和 `password` 欄位可填充：

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use HasFactory;

    protected $fillable = ['acc', 'password'];
}
```

---

## 5. 遷移 (Migration)

如果 `users` 資料表尚未建立，執行：

```sh
php artisan make:migration create_users_table
```

在 `database/migrations/xxxx_xx_xx_xxxxxx_create_users_table.php`，新增：

```php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('acc')->unique();
        $table->string('password');
        $table->timestamps();
    });
}
```

然後執行：

```sh
php artisan migrate
```

這樣 `users` 表就會建立。

---

這樣，你就成功把 `api/insert.php` 遷移到 Laravel 的 **路由** 和 **控制器** 了！ 🎉

如有問題或細節需要調整，歡迎隨時提問！😊

