# 如何將 CSV 檔案轉換為 JSON 檔案（Shift-JIS 編碼）

以下是使用 Python 將 CSV 檔案轉換為 JSON 格式的完整教學，並確保設置 `Shift-JIS` 編碼。

---

## **準備工作**

1. **安裝 Python**
   - 如果尚未安裝 Python，請到 [Python 官網](https://www.python.org/) 下載並安裝。

2. **安裝必要套件**
   - 打開終端或命令提示字元，執行以下指令來安裝 `pandas`：
     ```bash
     pip install pandas
     ```

---

## **Python 程式碼範例**

以下是程式碼，會讀取 `Shift-JIS` 編碼的 CSV 檔案並轉換為 JSON 檔案：

```python
import pandas as pd

# 讀取 CSV 檔案，指定編碼為 Shift-JIS
csv_file_path = "你的檔案名稱.csv"  # 替換為你的 CSV 檔案名稱
output_json_path = "你的輸出檔案名稱.json"  # 輸出的 JSON 檔案名稱

try:
    # 使用 pandas 讀取 CSV
    data = pd.read_csv(csv_file_path, encoding="shift_jis")
    
    # 轉換為 JSON 格式
    data.to_json(output_json_path, orient="records", force_ascii=False, indent=4)
    
    print(f"轉換成功！JSON 檔案儲存於: {output_json_path}")
except Exception as e:
    print(f"發生錯誤: {e}")
```

---

## **執行步驟**

1. 打開文字編輯器（例如 VS Code 或 Notepad++）。
2. 將上面的程式碼複製並貼上。
3. 替換以下變數內容：
   - `你的檔案名稱.csv`：改成你的 CSV 檔案的完整路徑。
   - `你的輸出檔案名稱.json`：改成你希望儲存的 JSON 檔案名稱。
4. 將程式碼儲存為檔案，例如命名為 `convert_csv_to_json.py`。
5. 打開終端或命令提示字元，切換到程式碼所在的目錄，執行：
   ```bash
   python convert_csv_to_json.py
   ```

---

## **輸出結果**

執行後，程式會將 JSON 檔案儲存到指定的路徑。範例格式如下：

```json
[
    {
        "欄位1": "值1",
        "欄位2": "值2"
    },
    {
        "欄位1": "值3",
        "欄位2": "值4"
    }
]
```

---

## **注意事項**

1. **檢查 CSV 檔案格式**
   - 確保 CSV 文件的第一行是標題（欄位名稱）。
   - 如果 CSV 文件很大，請確保電腦有足夠的記憶體。

2. **指定正確的編碼**
   - 如果你的檔案不是 `Shift-JIS` 編碼，請修改 `encoding="shift_jis"` 為對應的編碼格式。

3. **安裝必要套件**
   - 如果執行時出現模組錯誤，請重新安裝 `pandas`：
     ```bash
     pip install pandas
     ```

---

如果有其他問題或需要協助，隨時告訴我！😊

