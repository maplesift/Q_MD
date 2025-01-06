# PHP 方法解析與 SQL 語法拼接

## 1. `a2s` 方法的功能解析

### 方法功能
`a2s` 方法的主要功能是將一個關聯陣列轉換為格式化的 SQL 條件語法。

```php
function a2s($array){
    $tmp=[];
    foreach($array as $key => $value){
        $tmp[]="`$key`='$value'";
    }
    return $tmp;
}
```

### 方法的執行步驟
1. **輸入**
   - 方法接受一個關聯陣列，例如：`['type' => 'news', 'status' => 'published']`。
2. **邏輯**
   - 使用 `foreach` 遍歷陣列，將每個鍵值組合為 SQL 條件：
     - 鍵 (欄位名稱)：用反引號包裹，`$key` => `` `key` ``。
     - 值：用單引號包裹，`$value` => `'value'`。
     - 鍵值對組合為 SQL 條件，例如：`` `key`='value' ``。
   - 將這些條件存入陣列並回傳。
3. **輸出**
   - 回傳值是一個陣列，內容為 SQL 條件語法的字串。

### 使用範例
```php
$array = ['type' => 'news', 'status' => 'published'];
$result = $this->a2s($array);
```
執行後的結果：
```php
["`type`='news'", "`status`='published'"]
```

---

## 2. `all` 方法結合 `a2s` 方法的 SQL 拼接解析

### 方法功能
`all` 方法用於查詢資料庫並返回結果，接受多個參數來組合 SQL 語法。

```php
function all(...$arg){
    $sql="SELECT * FROM $this->table ";
    if(!empty($arg[0])){
        if(is_array($arg[0])){
            $where=$this->a2s($arg[0]);
            $sql=$sql . " WHERE ". join(" && ",$where);
        }else{
            $sql .= $arg[0];
        }
    }
    if(!empty($arg[1])){
        $sql=$sql . $arg[1];
    }
    return $this->fetchAll($sql);
}
```

### SQL 語法拼接流程
1. 初始 SQL：
   ```sql
   SELECT * FROM table_name
   ```
2. 判斷參數是否存在：
   - 若第一個參數是陣列，調用 `a2s` 方法處理，並在 SQL 語法中加入 `WHERE` 條件。
   - 若有第二個參數，直接拼接到 SQL 語法的末尾。

### 使用範例
#### 呼叫
```php
$rows = $News->all(['type' => 'news', 'status' => 'published']);
```
#### 執行流程
1. 調用 `a2s(['type' => 'news', 'status' => 'published'])`，返回：
   ```php
   ["`type`='news'", "`status`='published'"]
   ```
2. 使用 `join(" && ", $where)` 組合條件，結果：
   ```sql
   `type`='news' && `status`='published'
   ```
3. 拼接完整 SQL 語法：
   ```sql
   SELECT * FROM table_name WHERE `type`='news' && `status`='published';
   ```

---

## 3. 總結

### 方法 `a2s`
- 將關聯陣列轉換為 SQL 條件語法的陣列。
- 每個條件格式為 `` `key`='value' ``，並回傳條件陣列。

### 方法 `all`
- 組合 SQL 語法以查詢資料庫。
- 支援多種輸入形式，包括陣列 (透過 `a2s`) 與直接的 SQL 字串。
- 結果用於執行資料庫查詢並返回結果。
