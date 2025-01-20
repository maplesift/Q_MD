
# PHP: isset() vs !empty()

## isset() 與 !empty() 的差異
在 PHP 中，`isset()` 和 `!empty()` 都用於檢查變量，但用途和行為不同：

### 1. isset()
- **作用**：檢查變量是否已設定（存在）且不為 `null`。
- **返回值**：
  - 存在且不為 `null`：返回 `true`。
  - 不存在或為 `null`：返回 `false`。
- **範例**：
  ```php
  $var1 = "Hello";
  $var2 = null;

  echo isset($var1); // true
  echo isset($var2); // false
  echo isset($var3); // false (未定義的變量)
  ```

### 2. !empty()
- **作用**：檢查變量是否為「空值」。
  - 「空值」包括：`null`、`false`、空字符串 `""`、數字 `0`、`"0"`、空陣列 `[]` 等。
- **返回值**：
  - 非空：返回 `true`。
  - 空值：返回 `false`。
- **範例**：
  ```php
  $var1 = "Hello";
  $var2 = 0;
  $var3 = null;

  echo !empty($var1); // true
  echo !empty($var2); // false
  echo !empty($var3); // false
  ```

### 差異總結表
| 功能             | isset()                     | !empty()                         |
|------------------|-----------------------------|----------------------------------|
| **檢查存在性**   | 檢查變量是否存在且不為 `null` | 檢查變量是否有「實際內容」         |
| **對未定義變量** | 返回 `false`，可能引發警告  | 返回 `false`，不引發警告          |

---

## 在 `save()` 函式中的應用
### 原始程式碼
```php
function save($array){
    if (isset($array['id'])) {
        // update
        $id = $array['id'];
        unset($array['id']);
        $set = $this->a2s($array);
        $sql = "UPDATE $this->table SET ".join(',', $set)." where `id`='$id'";
    } else {
        // insert
        $cols = array_keys($array);
        $sql = "INSERT INTO $this->table (`".join("`,`", $cols)."`) VALUES('".join("','", $array)."')";
    }
    return $this->pdo->exec($sql);
}
```

### 如果改成 `!empty($array['id'])` 的影響
1. **行為差異**：
   - `isset($array['id'])`：檢查 `'id'` 是否存在且不為 `null`。
   - `!empty($array['id'])`：檢查 `'id'` 是否存在且「有實際內容」。

2. **可能的影響**：當 `$array['id']` 為以下值時：

| `$array['id']` 值 | `isset()` 結果 | `!empty()` 結果 | 結果行為 |
|-------------------|----------------|-----------------|----------|
| `123`             | true           | true            | 更新      |
| `"123"`           | true           | true            | 更新      |
| `0`               | true           | false           | 插入      |
| `"0"`             | true           | false           | 插入      |
| `""`              | true           | false           | 插入      |
| `null`            | false          | false           | 插入      |
| 未定義            | false          | false           | 插入      |

3. **結論建議**：
   - 使用 `isset($array['id'])` 更為合理，因為數字 `0` 可能是合法的主鍵值。
   - 如果希望更嚴格檢查：
     ```php
     if (isset($array['id']) && !empty($array['id'])) {
         // 更新操作
     }
     ```
