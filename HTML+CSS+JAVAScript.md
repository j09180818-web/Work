# C# 工程師學習網頁基礎筆記：HTML / CSS / JavaScript

## 一、網頁整體架構先理解

一個網頁大致可以分成三層：

| 層級  | 技術         | 作用      |
| --- | ---------- | ------- |
| 結構層 | HTML       | 網頁有哪些內容 |
| 外觀層 | CSS        | 網頁長什麼樣子 |
| 行為層 | JavaScript | 網頁如何互動  |

簡單比喻：

HTML = 骨架
CSS = 外觀、衣服、排版
JavaScript = 動作、互動、邏輯

---

# 二、HTML 是什麼？

HTML 全名是 HyperText Markup Language。

它不是程式語言，而是「標記語言」。

主要用途是描述網頁內容結構。

例如：

```html
<h1>標題</h1>
<p>這是一段文字</p>
<button>按鈕</button>
```

## HTML 要學的大綱

### 1. 基本結構

```html
<!DOCTYPE html>
<html>
<head>
    <title>網頁標題</title>
</head>
<body>
    網頁內容
</body>
</html>
```

重點理解：

* `html`：整個網頁
* `head`：網頁設定，不直接顯示
* `body`：使用者看得到的內容

---

### 2. 常見標籤

| 標籤             | 作用   |
| -------------- | ---- |
| `h1 ~ h6`      | 標題   |
| `p`            | 段落   |
| `a`            | 超連結  |
| `img`          | 圖片   |
| `div`          | 區塊容器 |
| `span`         | 行內容器 |
| `ul / ol / li` | 清單   |
| `table`        | 表格   |
| `form`         | 表單   |
| `input`        | 輸入框  |
| `button`       | 按鈕   |

---

### 3. HTML 對 C# 工程師的重要觀念

HTML 比較像 WinForms 的畫面元件結構。

例如：

| WinForms     | HTML          |
| ------------ | ------------- |
| Form         | HTML 頁面       |
| Label        | `label` / `p` |
| TextBox      | `input`       |
| Button       | `button`      |
| Panel        | `div`         |
| DataGridView | `table`       |

---

# 三、CSS 是什麼？

CSS 全名是 Cascading Style Sheets。

主要用途是控制網頁外觀。

例如：

```css
button {
    background-color: blue;
    color: white;
    width: 100px;
}
```

## CSS 要學的大綱

### 1. 選擇器 Selector

CSS 需要先選到 HTML 元素，才能改樣式。

```css
p {
    color: red;
}
```

常見選擇器：

| 寫法       | 意義              |
| -------- | --------------- |
| `p`      | 選所有 p 標籤        |
| `.title` | 選 class="title" |
| `#main`  | 選 id="main"     |
| `div p`  | 選 div 裡面的 p     |

---

### 2. 常見樣式屬性

| 屬性                 | 作用   |
| ------------------ | ---- |
| `color`            | 文字顏色 |
| `background-color` | 背景顏色 |
| `font-size`        | 字體大小 |
| `width`            | 寬度   |
| `height`           | 高度   |
| `margin`           | 外距   |
| `padding`          | 內距   |
| `border`           | 邊框   |
| `display`          | 顯示模式 |
| `position`         | 定位方式 |

---

### 3. Box Model 盒模型

每個 HTML 元素都可以想成一個盒子：

```text
margin    外距
border    邊框
padding   內距
content   內容
```

這是 CSS 最重要觀念之一。

---

### 4. 排版系統

CSS 排版主要要知道：

| 技術         | 用途              |
| ---------- | --------------- |
| Flexbox    | 一維排版，常用         |
| Grid       | 二維排版，複雜版面       |
| Position   | 固定、絕對、相對位置      |
| Responsive | 響應式網頁，手機/電腦自動調整 |

---

# 四、JavaScript 是什麼？

JavaScript 是真正的程式語言。

主要用途是讓網頁有互動能力。

例如：

```javascript
document.querySelector("button").addEventListener("click", function () {
    alert("按鈕被按下了");
});
```

## JavaScript 要學的大綱

### 1. 基本語法

要理解：

* 變數
* if 判斷
* for 迴圈
* function 函式
* array 陣列
* object 物件

例如：

```javascript
let name = "Tom";

if (name === "Tom") {
    console.log("Hello Tom");
}
```

---

### 2. JavaScript 與 C# 對照

| C#                     | JavaScript          |
| ---------------------- | ------------------- |
| `string name = "Tom";` | `let name = "Tom";` |
| `int age = 20;`        | `let age = 20;`     |
| `Console.WriteLine()`  | `console.log()`     |
| `List<T>`              | `Array`             |
| `class`                | `class`             |
| `async/await`          | `async/await`       |

---

### 3. DOM 操作

DOM 是網頁被瀏覽器解析後形成的物件樹。

JavaScript 可以透過 DOM 操作 HTML。

例如：

```javascript
document.getElementById("title").innerText = "新標題";
```

常見操作：

| 動作   | 說明                 |
| ---- | ------------------ |
| 找元素  | `querySelector`    |
| 改文字  | `innerText`        |
| 改樣式  | `style.color`      |
| 新增元素 | `createElement`    |
| 刪除元素 | `remove`           |
| 綁定事件 | `addEventListener` |

---

### 4. 事件 Event

網頁互動大多靠事件。

例如：

| 事件        | 意義   |
| --------- | ---- |
| `click`   | 點擊   |
| `change`  | 值改變  |
| `input`   | 輸入中  |
| `submit`  | 表單送出 |
| `keydown` | 鍵盤按下 |

這跟 C# WinForms 的 Button Click 很像。

---

### 5. 非同步與 API

JavaScript 常用來呼叫後端 API。

例如：

```javascript
fetch("/api/data")
    .then(response => response.json())
    .then(data => console.log(data));
```

也可以寫成：

```javascript
async function loadData() {
    const response = await fetch("/api/data");
    const data = await response.json();
    console.log(data);
}
```

這跟 C# 的 `HttpClient`、`async/await` 很像。

---

# 五、前端與後端的關係

網頁系統通常分成：

```text
瀏覽器 Frontend
    ↓ HTTP Request
後端 Backend
    ↓
資料庫 Database
```

## 對 C# 工程師來說

你比較可能負責：

* ASP.NET Core Web API
* 資料庫
* 後端邏輯
* API 設計
* 資料格式 JSON
* 權限驗證
* 與前端串接

前端可能負責：

* HTML
* CSS
* JavaScript
* Vue / React / Angular
* 畫面互動

---

# 六、網頁重要名詞大綱

## 1. Browser 瀏覽器

例如 Chrome、Edge。

負責解析：

* HTML
* CSS
* JavaScript

---

## 2. HTTP / HTTPS

瀏覽器與伺服器溝通的協定。

常見方法：

| 方法     | 用途     |
| ------ | ------ |
| GET    | 讀取資料   |
| POST   | 新增資料   |
| PUT    | 修改整筆資料 |
| PATCH  | 修改部分資料 |
| DELETE | 刪除資料   |

---

## 3. URL

例如：

```text
https://example.com/products?id=10
```

組成：

| 部分            | 說明   |
| ------------- | ---- |
| `https`       | 協定   |
| `example.com` | 網域   |
| `/products`   | 路徑   |
| `?id=10`      | 查詢參數 |

---

## 4. JSON

前後端交換資料常用格式。

```json
{
    "id": 1,
    "name": "Motor",
    "status": true
}
```

C# 後端常把物件轉成 JSON 給前端。

---

## 5. API

API 是前端向後端要資料的接口。

例如：

```text
GET /api/devices
POST /api/devices
GET /api/devices/1
```

---

# 七、建議學習順序

## 第一階段：理解網頁三層

1. HTML 是結構
2. CSS 是外觀
3. JavaScript 是行為

---

## 第二階段：HTML 基礎

1. HTML 基本架構
2. 常用標籤
3. 表單
4. 表格
5. div / span
6. id / class

---

## 第三階段：CSS 基礎

1. 選擇器
2. 顏色、字體、大小
3. margin / padding
4. box model
5. flexbox
6. grid
7. 響應式設計

---

## 第四階段：JavaScript 基礎

1. 變數
2. 判斷式
3. 迴圈
4. 函式
5. 陣列
6. 物件
7. DOM
8. Event
9. fetch API
10. async / await

---

## 第五階段：前後端串接

1. HTTP
2. REST API
3. JSON
4. C# ASP.NET Core Web API
5. JavaScript fetch
6. 錯誤處理
7. 權限驗證

---

# 八、C# 工程師最需要知道的重點

你不一定要很會切版，但需要知道：

1. 前端畫面怎麼組成
2. HTML 元素怎麼對應資料
3. CSS 基本排版怎麼影響畫面
4. JavaScript 如何操作畫面
5. 前端如何呼叫後端 API
6. 後端如何回傳 JSON
7. HTTP Request / Response 是什麼
8. GET / POST / PUT / DELETE 差異
9. 網頁錯誤如何用瀏覽器開發者工具檢查
10. C# 後端與前端資料如何串起來

---

# 九、總結

網頁可以先用一句話理解：

HTML 負責「放什麼內容」
CSS 負責「長什麼樣子」
JavaScript 負責「按下去會發生什麼事」
C# 後端負責「資料、邏輯、API、資料庫」

對 C# 工程師來說，學網頁的核心不是變成美術切版工程師，而是要理解：

前端畫面如何產生
前端如何送資料給後端
後端如何提供 API
資料如何從資料庫一路顯示到網頁上
