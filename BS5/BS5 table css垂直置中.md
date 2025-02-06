# 使用 CSS 置中表格內容的討論

## 問題描述

使用者希望在 Bootstrap 5 (BS5) 裝飾的表格中，將特定 `<td>` 元素 (`#td-test`) 的內容垂直置中。

### HTML 範例
```html
<table class="table">
    <thead>
        <tr>
            <th>Firstname</th>
            <th>Lastname</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        <tr class="tr-test">
            <td id='td-test'>John</td>
            <td>Doe</td>
            <td>john@example.com</td>
        </tr>
    </tbody>
</table>
```

### 初始 CSS
```css
#td-test {
    width: 300px;
    height: 100px;
    background-color: wheat;
    /* margin: auto; */
    text-align: center;
}
```

## 解決方案

### 解法 1：使用 `vertical-align`

保持 `table-cell` 特性，使用 `vertical-align: middle` 來垂直置中。

```css
#td-test {
    width: 300px;
    height: 100px;
    background-color: wheat;
    text-align: center; /* 水平置中 */
    vertical-align: middle; /* 垂直置中 */
}
```

### 解法 2：使用 Flexbox

如果使用 `display: flex` 會讓格式跑掉，應避免直接在 `td` 上使用 Flexbox。

**正確方式**：對 `tr` 或 `tbody` 使用 `flex`。

```css
tbody {
    display: flex;
    flex-direction: column;
}

.tr-test {
    display: flex;
}

#td-test {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: wheat;
    width: 300px;
    height: 100px;
}
```

## 結論

* 使用 `vertical-align: middle` 是最簡單且不會影響 `table` 結構的方法。
* 若使用 `flex`，應該在父層（如 `tr` 或 `tbody`）應用，而非直接在 `td`。

這樣可以確保內容置中且不會破壞表格原本的佈局。

