# github下載

- git clone https://github.com/ xxxx 自行去github複製

### 加入函式庫
- composer install
### 建立env
-    cp .env.example .env
- OR copy .env.example .env
### 幫env加入key
- php artisan key:generate
### env設定資料庫
        DB_CONNECTION=mysql
        DB_HOST=127.0.0.1
        DB_PORT=3306
        資料庫名稱 需再去phpadmin建立同名資料庫
        DB_DATABASE=laravel_test
        DB_USERNAME=root
        DB_PASSWORD=
### 加入空資料表
-   php artisan migrate
-   php artisan db:seed
-   php artisan serve

===============================

# 檔案遷移(?)
 - route:
 - env放PWD
```php
Route::get('/', function () {
    $pw = env("PWD");
    // dd($pw);
    return view('index', ['pw' => $pw]);
})->name('index');
```
- 通常都放在 public/assets/css | and img  | and js
- 路徑ex:
```html
    <link rel="stylesheet" href="{{asset('assets\css\css.css')}}">
    <link rel="icon" href="{{asset('assets\img\Marie Antoinette (Summer) 1.png')}}" sizes="32x32" type="image/png">
```

===============================
# Controller 
- [官網:controller](https://laravel.com/docs/11.x/controllers)
-  終端機輸入: 
```cmd
<!-- (Photo)Controller (為自訂名稱) -->
php artisan make:controller PhotoController --resource
```
- 在routes/web輸入:
```php
use App\Http\Controllers\(Photo)Controller;
Route::resource('(photos)', (Photo)Controller::class);
```
- 在app\Http\Controllers\找到(Photo)Controller
- 可在function輸入:dd('(text)') 確認是否連到該網址
- 可在終端機輸入 php artisan route:list 查看route的網址
```php
    public function index()
    {
        $data=
        [   
            [
                'id'=>1,
                'name'=> 'apple',
            ],
            [
                'id'=>2,
                'name'=> 'babana',
            ],
            [
                'id'=>3,
                'name'=> 'cat',
            ]
        ];
        return view('student.index',['data'=>$data]);
    }
```
- 可在controller加上新的function
```php
    public function del()
    {
        dd('hi del');
    }
```
這就必須到routes\web去新增:
```php
Route::get('/students/del', [StudentController::class, 'del']);
```

# 0226 資料庫
## [官網](https://laravel.com/docs/11.x/migrations#generating-migrations)

======================================
<!-- 範例 -->
php artisan make:migration create_flights_table
<!-- 創造一個cars table -->
php artisan make:migration create_cars_table
<!-- 在資料庫創造資料表 (執行/前進一步) -->
- php artisan migrate
<!-- 創造資料表的動作:回到上一動 (退一步)   -->
- php artisan migrate:rollback

```php
        Schema::create('dogs', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name'); //可自創資料表欄位 名稱
            $table->string('address'); //可自創資料表欄位 名稱
        }); 
```

### 在資料庫增加資料表(create)
```php
    public function up(): void
    {           // create:創造資料表
        Schema::create('dogs', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->string('address');
            $table->string('love');
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('dogs');
    }
```

### 在資料表增加欄位(table)
```php
return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {         //需要增加欄位 而不是資料表
        Schema::table('dogs', function (Blueprint $table) {
            $table->integer('love');
        });
        Schema::table('cars', function (Blueprint $table) {
            $table->integer('love');
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::table('dogs', function (Blueprint $table) {
            $table->dropColumn('love');
        });
        Schema::table('cars', function (Blueprint $table) {
            $table->dropColumn('love');
        });
    }
};
```
### 從資料庫query

[官網:query](https://laravel.com/docs/11.x/queries#running-database-queries)
===============================
```php
// ex:
// controller內
// 引入db
use Illuminate\Support\Facades\DB;
// 去找到資料
$users = DB::table('users')->get();
// =================================================================


// 實際做:
$data = DB::table('students')->get();
// dd($data);
return view('student.index',['data'=>$data]);

// 在view輸入
@foreach ($data as $val)
<tr>
    <td>{{$val->id}}</td>
    <td>{{$val->name}} </td>
    <td>{{$val->mobile}} </td> 

</tr>

@endforeach

```
### 一次建立migrations/controller/resource
==================================
php artisan make:model Apple -mcr
php artisan make:model Student -mcr