# HTML 專案執行、瀏覽器解析、伺服器通訊與渲染筆記

## 1. HTML 需要編譯嗎？

HTML **不需要編譯**。

它不像 C# 程式需要經過：

```text
C# 原始碼
↓
編譯
↓
.exe / .dll
↓
執行
```

HTML 的流程是：

```text
HTML 檔案
↓
瀏覽器讀取
↓
瀏覽器解析
↓
建立 DOM
↓
渲染畫面
```

所以 HTML 檔案寫好後，可以直接用瀏覽器開啟。

例如：

```text
index.html
style.css
script.js
```

可以直接點兩下 `index.html`，或使用瀏覽器開啟。

---

## 2. 本機開啟 HTML 的方式

如果在本機直接開啟 HTML，網址列可能會顯示：

```text
file:///C:/WebProject/index.html
```

代表瀏覽器是直接讀取本機檔案。

流程如下：

```text
本機 index.html
↓
瀏覽器讀取檔案
↓
解析 HTML
↓
建立 DOM
↓
載入 CSS
↓
執行 JavaScript
↓
渲染畫面
```

這種方式不一定需要網路，也不一定需要伺服器。

---

## 3. 為什麼不同瀏覽器都能開 HTML？

因為 HTML、CSS、JavaScript 都有共同規範。

例如：

```html
<h1>Hello</h1>
<button>啟動</button>
```

Chrome、Edge、Firefox 都知道：

```text
<h1> 是標題
<button> 是按鈕
```

所以不同瀏覽器都可以解析 HTML 並顯示網頁。

但是要注意：

不同瀏覽器、不同版本，對某些新語法或新功能的支援可能不同。

例如：

```css
.container {
    display: grid;
}
```

新版瀏覽器通常支援良好，但過舊的瀏覽器可能支援不完整。

所以網頁開發要注意：

```text
瀏覽器類型
瀏覽器版本
HTML / CSS / JavaScript 支援程度
```

---

## 4. 使用者訪問網站時的流程

當使用者輸入網址：

```text
https://example.com/index.html
```

流程是：

```text
使用者瀏覽器
↓
發送 HTTP Request 請求
↓
網頁伺服器 Web Server
↓
回傳 HTTP Response 回應
↓
瀏覽器收到 HTML / CSS / JavaScript
↓
瀏覽器解析並顯示畫面
```

重點是：

伺服器不是直接把「畫面」傳給使用者。

伺服器主要是傳：

```text
HTML
CSS
JavaScript
圖片
JSON 資料
其他資源
```

瀏覽器收到這些資料後，自己負責解析、建立 DOM、套用 CSS、執行 JavaScript，最後把畫面渲染出來。

---

## 5. 網頁伺服器會回傳哪些資料？

伺服器可能回傳：

```text
index.html
style.css
script.js
image.png
data.json
```

例如第一次請求：

```text
瀏覽器請求 index.html
```

伺服器回傳 HTML：

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>設備狀態</h1>
    <script src="script.js"></script>
</body>
</html>
```

瀏覽器看到這行：

```html
<link rel="stylesheet" href="style.css">
```

就會再向伺服器請求：

```text
style.css
```

瀏覽器看到這行：

```html
<script src="script.js"></script>
```

就會再向伺服器請求：

```text
script.js
```

所以網站載入時，不一定只請求一個檔案。

瀏覽器會先拿 HTML，再根據 HTML 內容繼續請求 CSS、JavaScript、圖片等資源。

---

## 6. DOM 是什麼？

DOM，全名是 Document Object Model。

可以理解成：

瀏覽器根據 HTML 建立出來的「畫面物件樹」。

例如 HTML：

```html
<body>
    <h1>設備狀態</h1>
    <button id="btnStart">啟動</button>
</body>
```

瀏覽器會建立成類似：

```text
document
└─ html
   └─ body
      ├─ h1
      └─ button id="btnStart"
```

這個結構就是 DOM。

精準說法：

```text
瀏覽器載入 HTML 時，會一邊解析 HTML，一邊建立 DOM。
```

HTML 本身不是程式執行主體，而是讓瀏覽器建立 DOM 的結構描述。

---

## 7. CSS 和 JavaScript 如何與 DOM 連結？

HTML：

```html
<button id="btnStart">啟動</button>
```

CSS：

```css
#btnStart {
    background-color: green;
}
```

JavaScript：

```javascript
const btn = document.getElementById("btnStart");

btn.addEventListener("click", function () {
    alert("啟動");
});
```

流程是：

```text
HTML 建立 button DOM
↓
CSS 用 #btnStart 找到這個 DOM，套用樣式
↓
JavaScript 用 id="btnStart" 找到這個 DOM，綁定 click 事件
```

所以可以理解成：

```text
HTML 負責建立結構
CSS 負責外觀樣式
JavaScript 負責事件與互動邏輯
DOM 是三者共同操作的畫面物件結構
```

---

## 8. 渲染 Render 是什麼？

渲染英文是 Render。

在網頁中，渲染意思是：

```text
瀏覽器把 HTML 結構、CSS 樣式、DOM 物件計算後，真正畫到螢幕上。
```

例如：

```html
<h1 style="color:red;">警報</h1>
```

瀏覽器會做：

```text
知道這是 h1 標題
↓
知道文字內容是「警報」
↓
知道顏色是紅色
↓
計算它的位置、大小、字型、顏色
↓
把紅色標題畫到螢幕上
```

這個「畫出畫面」的過程，就叫做渲染。

---

## 9. 本機 HTML 與網站 HTML 的差異

### 本機開啟 HTML

```text
index.html
↓
瀏覽器直接讀取本機檔案
↓
解析 HTML
↓
建立 DOM
↓
套用 CSS
↓
執行 JavaScript
↓
渲染畫面
```

### 使用者訪問網站

```text
使用者輸入網址
↓
瀏覽器發送 HTTP Request
↓
Web Server 回傳 HTML
↓
瀏覽器根據 HTML 再請求 CSS / JS / 圖片
↓
建立 DOM
↓
套用 CSS
↓
執行 JavaScript
↓
渲染畫面
```

兩者差異主要在於：

```text
本機開啟：瀏覽器直接讀取本機檔案
網站開啟：瀏覽器透過網路向伺服器請求檔案
```

但是最後都一樣：

```text
都由瀏覽器解析 HTML、建立 DOM、套用 CSS、執行 JavaScript、渲染畫面。
```

---

## 10. 用 C# 工程師角度理解

```text
HTML
≈ UI 結構描述檔

CSS
≈ UI 樣式設定

JavaScript
≈ UI 事件與互動邏輯

瀏覽器
≈ 執行環境

DOM
≈ 瀏覽器根據 HTML 建立出來的畫面物件樹

Render 渲染
≈ 把結構、樣式與內容真正畫到螢幕上

Web Server
≈ 提供 HTML / CSS / JS / 圖片 / API 資料的伺服器

HTTP Request
≈ 瀏覽器向伺服器提出請求

HTTP Response
≈ 伺服器回傳資料給瀏覽器
```

---

## 11. 重點總結

HTML 專案寫好後，不需要像 C# 一樣編譯。

```text
HTML 不是被編譯執行，而是被瀏覽器解析。
```

本機可以直接開：

```text
index.html
↓
瀏覽器解析
↓
建立 DOM
↓
渲染畫面
```

網站訪問流程是：

```text
瀏覽器請求伺服器
↓
伺服器回傳 HTML / CSS / JavaScript / 圖片
↓
瀏覽器解析 HTML
↓
建立 DOM
↓
套用 CSS
↓
執行 JavaScript
↓
渲染畫面
```

最核心觀念：

```text
HTML 是結構
CSS 是外觀
JavaScript 是互動邏輯
DOM 是瀏覽器建立出的畫面物件樹
Render 是瀏覽器把畫面畫出來
Web Server 是提供網頁檔案與資料的來源
```
