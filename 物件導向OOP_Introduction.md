
# 物件導向程式設計（Object-Oriented Programming, OOP）

**物件導向程式設計** 是一種程式設計的範式，將程式設計的重心放在「物件（Object）」上，並使用物件來模擬現實世界中的事物或概念。每個物件都是由 **屬性（Properties）** 和 **方法（Methods）** 組成，屬性用來儲存物件的狀態，方法則用來執行行為或操作。

## 物件導向程式設計的四大特性

1. **封裝（Encapsulation）**
    - 將資料和操作包裝在物件中，對外只提供有限的介面（例如：方法）來操作物件，隱藏內部實現的細節，確保資料的安全性。
    - 例如：將物件的屬性設為「私有（private）」並透過「公開（public）」的方法存取。
    
2. **繼承（Inheritance）**
    - 子類別（Subclass）可以繼承父類別（Superclass）的屬性和方法，從而避免重複程式碼並促進程式碼的重用。
    - 例如：`Car` 類別繼承自 `Vehicle` 類別，`Car` 可以擁有 `Vehicle` 的所有功能。
    
3. **多型（Polymorphism）**
    - 不同類別的物件可以以相同的方式被操作（例如，呼叫相同的方法），但表現出的行為可以不同。
    - 例如：`Bird` 和 `Plane` 都有 `fly()` 方法，但 `Bird` 的 `fly()` 是拍動翅膀，而 `Plane` 的 `fly()` 是啟動引擎。
    
4. **抽象（Abstraction）**
    - 將物件的關鍵功能抽象化，並隱藏不必要的細節，讓程式更簡潔、專注於解決問題。
    - 例如：定義一個抽象類別（`Abstract Class`）或介面（`Interface`），只描述必須實現的功能而不提供具體的實現。

---

## OOP 的關鍵概念

1. **類別（Class）：**
    - 類別是物件的藍圖，定義了物件的屬性和行為。
    - 例如：

    ```php
    class Car {
        public $brand;
        public $color;

        public function drive() {
            echo "The car is driving.";
        }
    }
    ```

2. **物件（Object）：**
    - 根據類別創建的實體，是可以操作的具體個體。
    - 例如：

    ```php
    $myCar = new Car();
    $myCar->brand = "Toyota";
    $myCar->color = "Red";
    $myCar->drive(); // Output: The car is driving.
    ```

3. **建構子（Constructor）：**
    - 在創建物件時自動執行的特殊方法，用於初始化物件屬性。
    - 例如：

    ```php
    class Car {
        public $brand;
        public $color;

        public function __construct($brand, $color) {
            $this->brand = $brand;
            $this->color = $color;
        }
    }

    $myCar = new Car("Honda", "Blue");
    ```

4. **介面和抽象類別：**
    - **介面（Interface）：** 定義類別必須實現的方法，沒有具體實現。
    - **抽象類別（Abstract Class）：** 可以有部分實現，也可以包含抽象方法。

---

## 為什麼學習 OOP？

- **模組化**：程式可以分解成更小的部分，易於理解和維護。
- **重用性**：使用繼承和多型，減少重複程式碼。
- **擴充性**：當需求改變時，方便擴展程式功能。
- **真實世界的模擬**：可以更自然地描述現實問題並提供解決方案。

如果你正開始學習 OOP，建議從 PHP 的基本類別與物件開始，然後逐步理解繼承和多型的應用！
