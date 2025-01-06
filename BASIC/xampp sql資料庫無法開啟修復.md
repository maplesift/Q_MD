# XAMPP MySQL 強制關機後修復指南

## 問題描述

有些人可能有個壞習慣，就是 XAMPP 沒有正常關閉就直接進行關機；當然不只 XAMPP，許多視窗也可能因此被強制關閉。

### 強制關機的結果：資料庫意外關閉

實習日常的第一個動作，通常是開啟電腦、啟動 XAMPP 的 Apache 與 MySQL。但某一天，發現 MySQL 無法啟動，並出現以下錯誤訊息：

```
09:42:22 AM [mysql] Attempting to start MySQL app...
09:42:22 AM [mysql] Status change detected: running
09:42:22 AM [mysql] Status change detected: stopped
09:42:22 AM [mysql] Error: MySQL shutdown unexpectedly.
09:42:22 AM [mysql] This may be due to a blocked port, missing dependencies,
09:42:22 AM [mysql] improper privileges, a crash, or a shutdown by another method
09:42:22 AM [mysql] Press the Logs button to view error logs and check
09:42:22 AM [mysql] the Windows Event Viewer for more clues
09:42:22 AM [mysql] If you need more help, copy and post this
09:42:22 AM [mysql] entire log window on the forums
```

經過查詢，發現原因是未正常關閉 XAMPP 即關機，導致 MySQL 資料損毀。

---

## 解決方法

透過參考資料，成功修復此問題！以下是具體步驟：

> **提醒：請務必先進行備份！** 將 XAMPP 資料夾完整備份以防萬一。

### 修復步驟

1. **重新命名資料夾**
   將 `XAMPP\mysql\data` 資料夾重新命名為其他名稱（例如 `data_old`）。

2. **建立新資料夾**
   新建一個同名資料夾 `XAMPP\mysql\data`。

3. **複製備份內容**
   將 `XAMPP\mysql\backup` 資料夾內的所有內容複製到新建的 `XAMPP\mysql\data` 資料夾中。

4. **複製舊資料**
   從舊的 `XAMPP\mysql\data` 資料夾中，將除 `mysql`、`performance_schema` 和 `phpmyadmin` 資料夾外的所有檔案，複製到新的 `XAMPP\mysql\data` 資料夾中，尤其是 `ibdata1` 檔案，務必覆蓋至新資料夾內。

---

## 總結與心得

這次經驗證明，方便常伴隨風險。之前覺得直接關機很方便，但一次問題就讓人學會了。

請務必記得：
- 無論是 XAMPP 或其他應用程式，務必先正常關閉後再關機。
- 修復問題可能偶然成功，但不要仰賴運氣，養成良好習慣才是長久之計。

---

## 參考資料

- [XAMPP - MySQL shutdown unexpectedly - Stack Overflow](https://stackoverflow.com/questions/18022809/xampp-mysql-shutdown-unexpectedly)
