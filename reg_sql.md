
從你提供的程式碼和問題來看，你希望防止有人在你的練習網站上「亂搞」，比如用簡單密碼測試漏洞（例如剛剛提到的「PW:aaa'l」）
或進行其他惡意行為。我會針對你的前端HTML和後端PHP程式碼，給你一些具體建議來提升安全性並減少這種情況。
目前程式碼的潛在問題
前端問題：
沒有輸入驗證：前端表單（登入和註冊）沒有任何限制，比如密碼長度、特殊字符要求等，這讓「PW:aaa'l」這種簡單輸入可以直接提交。
表單直接依賴JavaScript函數（login() 和 reg()），但沒有顯示這些函數的實現。如果後端沒有進一步檢查，前端可能被繞過。
後端問題：
你直接把 $_POST 資料傳給 $Admin->save($_POST)，而且只用 unset($_POST['pw2']) 移除確認密碼。
這意味著如果有人用工具（比如Postman）直 接發送惡意資料，後端可能不會過濾就存進資料庫。
沒有提到資料庫存取是否用了參數化查詢（防止SQL注入）。
你的目標：
你提到「不想要看到對方亂搞」，可能是希望限制無意義的註冊、測試行為，或避免漏洞被利用。
解決方案
以下是一些實用的改進建議，分為前端和後端：

前端加強輸入驗證
在表單提交前，檢查輸入是否合理，防止簡單或惡意資料直接送出。

限制密碼規則：要求密碼至少8個字符，包含字母和數字。
JavaScript驗證：在 login() 和 reg() 函數中加入檢查。

範例改進（註冊表單的JavaScript）：
```php

include_once "db.php";

// 清理輸入，防止XSS或惡意資料
function cleanInput($data) {
    return htmlspecialchars(trim($data), ENT_QUOTES, 'UTF-8');
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $acc = cleanInput($_POST['acc'] ?? '');
    $pw = cleanInput($_POST['pw'] ?? '');

    // 再次檢查密碼規則
    if (!preg_match('/^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/', $pw)) {
        die("密碼不符合要求！");
    }

    // 使用參數化查詢儲存資料，防止SQL注入
    $stmt = $pdo->prepare("INSERT INTO admin (acc, pw) VALUES (?, ?)");
    $stmt->execute([$acc, $pw]);

    echo "註冊成功！";
} else {
    echo "無效請求！";
}
```
```js
function reg() {
    let acc = document.getElementById('reg_acc').value;
    let pw = document.getElementById('reg_pw').value;
    let pw2 = document.getElementById('reg_pw2').value;

    // 檢查帳號和密碼是否為空
    if (!acc || !pw || !pw2) {
        alert("請填寫所有欄位！");
        return;
    }

    // 密碼至少8位，包含字母和數字
    let pwRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/;
    if (!pwRegex.test(pw)) {
        alert("密碼需至少8位，且包含字母和數字！");
        return;
    }

    // 確認密碼是否一致
    if (pw !== pw2) {
        alert("兩次輸入的密碼不一致！");
        return;
    }

    // 通過驗證後再提交（假設用fetch發送到後端）
    fetch('register.php', {
        method: 'POST',
        body: JSON.stringify({ acc: acc, pw: pw }),
        headers: { 'Content-Type': 'application/json' }
    })
    .then(response => response.text())
    .then(data => alert(data));
}
```