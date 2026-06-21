1. CSS 選擇器 Selector
CSS連結使用Html通常使用
標籤元素、class、ID
| 類型    | HTML 寫法        | CSS 寫法  | 用途     |
| ----- | -------------- | ------- | ------ |
| class | `class="card"` | `.card` | 可重複使用  |
| id    | `id="main"`    | `#main` | 頁面唯一元素 |

範例
<div id="areaA" class="machine-area">
  機台區 A
</div>
<div id="areaB" class="machine-area">
  機台區 B
</div>
在這之中有id與class，而CSS如何調用呢
.machine-area {
  border: 1px solid black;
}抓取Class調用
#areaA {
  background-color: lightblue;
}抓取個別ID
#areaB {
  background-color: lightyellow;
}抓取個別ID
也可以使用階層的h概念抓取
<div class="machine-area">
  <div class="machine-card">
    <h2>機台 A</h2>
    <p>狀態：運轉中</p>
  </div>
</div>
在CSS中
.machine-area h2 {
  color: green;
}Class中抓取h
# 子層選擇器 >
<div class="machine-area">
  <p>區域說明</p>

  <div class="machine-card">
    <p>機台狀態</p>
  </div>
</div>
.machine-area > p {
  color: red;
}這樣就是這個Class下的P抓取並調整 
當然也可以同時修改多種標籤
h1, h2, p {
  font-family: Arial;
}

而在CSS中也可以透過
button:hover {
  background-color: yellow;
}去設定物件滑鼠處碰到的行為變化
但CSS也有
<input type="text">
<input type="password">
CSS可以透過屬性中的type名稱去抓取
input[type="password"] {
  background-color: lightpink;
}

而CSS如何去選擇層級內的標籤或物件與框架等
<div class="area">
  <div class="card">
    <h2>
      機台 A
      <button>停止</button>
    </h2>
  </div>
</div>
.area .card h2 button {
  color: red;
}
這樣寫會抓到button
.area
└─ .card
   └─ h2
      └─ button
但如果你在h2省略不寫
也抓的到button這就是空白他會包含這層內所有指定Button的物件
如果要精準層CSS就必須使用.area > .card > h2 > button
.area > .card > h2 > button {
  color: red;
}
當然空白與>可以組合
在CSS寬鬆的層級內找到所有的.card下的button可以寬鬆又指定這Class下的按鈕反之也可以先嚴謹地說出>層級在寬鬆的給予範圍
.area .card > button {
  color: red;
}
CSS 可透過：
1. 標籤
2. class
3. id
4. 屬性
5. 階層關係
6. 狀態
7. 群組
8. 萬用選擇器
| 類型        | 意思                    | CSS 範例                               |
| --------- | --------------------- | ------------------------------------ |
| 標籤選擇器     | 用 HTML 標籤名稱選元素        | `p { color: red; }`                  |
| class 選擇器 | 用 `class` 名稱選元素，可重複使用 | `.card { color: red; }`              |
| id 選擇器    | 用 `id` 名稱選元素，通常唯一     | `#main { color: red; }`              |
| 屬性選擇器     | 用 HTML 屬性條件選元素        | `input[type="text"] { color: red; }` |
| 狀態選擇器     | 選元素目前狀態，例如滑鼠移上去       | `button:hover { color: red; }`       |
| 階層選擇器：空白  | 選某元素內部所有符合條件的後代       | `.area button { color: red; }`       |
| 階層選擇器：`>` | 只選直接下一層子元素            | `.area > button { color: red; }`     |
| 群組選擇器     | 多個選擇器共用同一組樣式          | `h1, h2, p { color: red; }`          |
| 萬用選擇器     | 選全部 HTML 元素           | `* { box-sizing: border-box; }`      |


2. CSS 排版 Layout+ Box Model 盒模型
CSS所指的HTML元素
| HTML 元素    | 你可以理解成    |
| ---------- | --------- |
| `<p>`      | 一段文字物件    |
| `<button>` | 按鈕物件      |
| `<input>`  | 輸入框物件     |
| `<img>`    | 圖片物件      |
| `<div>`    | 框架 / 容器物件 |
| `<h1>`     | 標題文字物件    |
Box Model 是什麼?
不管是什麼元素在CSS中都會有一個框架
margin 外距
┌────────────────────────┐
│ border 邊框            │
│  ┌──────────────────┐  │
│  │ padding 內距     │  │
│  │  ┌────────────┐  │  │
│  │  │ content    │  │  │
│  │  │ 內容本身   │  │  │
│  │  └────────────┘  │  │
│  └──────────────────┘  │
└────────────────────────┘
網頁排版時，元素佔多少空間，不只看 width。
假設
.card {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
200px + 左右 padding 40px + 左右 border 10px
= 250px
常用解法：box-sizing
* {
  box-sizing: border-box;
}
意思就是width 包含 content + padding + border
元素顯示方式 = 這個元素在畫面上怎麼排列。
主要是 display 這個 CSS 屬性。
例如
.card {
  display: block;
}
| display        | 意思             |
| -------------- | -------------- |
| `block`        | 區塊元素，自己佔一整行    |
| `inline`       | 行內元素，跟文字排在同一行  |
| `inline-block` | 可同一行，也能設定寬高    |
| `flex`         | 讓內部子元素用橫向/直向排列 |
| `grid`         | 讓內部子元素用格線排列    |
| `none`         | 不顯示            |


#1. 元素 = HTML 裡的一個物件，例如 div、p、button、input、img。
#2. Box Model = 每個元素在 CSS 排版裡都被當成盒子。
#3. display = 決定元素自己怎麼顯示，或決定它內部子元素怎麼排列。
| display        | 會不會換行 | 可否設定寬高 | 主要用途        |
| -------------- | ----: | -----: | ----------- |
| `block`        |     會 |     可以 | 大區塊、段落、框架   |
| `inline`       |    不會 |    不適合 | 文字中的小片段     |
| `inline-block` |    不會 |     可以 | 小按鈕、標籤、同排物件 |
| `flex`         |   看設定 |     可以 | 一排或一列排子元素   |
| `grid`         |   看設定 |     可以 | 多欄多列排版      |
| `none`         |   不顯示 |      無 | 隱藏元素        |





3. DOM 文件物件模型
Document Object Model
文件物件模型
DOM = 瀏覽器把 HTML 解析後，建立出來的一棵「物件樹」。

HTML 原始碼 = 文字結構
DOM = 瀏覽器轉成可操作的物件結構
JavaScript = 操作 DOM 的工具
<div class="card">
  <h2>機台 A</h2>
  <button>啟動</button>
</div>
這是我們所寫的代碼但經過編譯後
  document
└─ div.card
   ├─ h2
   │  └─ 文字：機台 A
   └─ button
      └─ 文字：啟動
就會建立如此的物件模型這樣javaScript就可以操作此物件
<button id="startBtn">啟動</button>
在JavaScript就可以透過向CSS一樣的方式連結物件進行操作
const btn = document.querySelector("#startBtn");
這時候btn就是物件就具有物件屬性可以操作
btn.textContent = "停止";
btn.style.backgroundColor = "red";
btn.disabled = true;
JavaScript-querySelector 跟 CSS Selector 關係

document.querySelector(".area .card button");
其實就是用Css的方式去找連結

const btn = document.querySelector("#startBtn");
btn.addEventListener("click", function () {
  btn.textContent = "已啟動";
  btn.style.backgroundColor = "green";
});
# HTML 會被瀏覽器解析成 DOM 物件樹
# CSS 透過 Selector 找 DOM 元素，套用樣式
# JavaScript 透過 Selector 或 DOM API 找 DOM 元素，操作內容、屬性、事件
| 項目   | CSS            | JavaScript               |
| ---- | -------------- | ------------------------ |
| 找誰   | 用 Selector 找元素 | 也可以用 Selector 找元素        |
| 找到後  | 套樣式            | 操作元素物件                   |
| 目的   | 外觀             | 行為、資料、事件                 |
| 常見語法 | `.card { }`    | `querySelector(".card")` |
CSS 和 JavaScript 都是在 DOM 物件樹上找元素，只是找到後做的事情不同。

4. Event 事件
Event 事件 =
瀏覽器或使用者在畫面上觸發的某個動作，
JavaScript 可以監聽這個動作，
當事件發生時執行指定程式。
HTML：提供畫面元素，例如 button、input、form
JavaScript：找到 HTML 元素，並綁定事件
Event：使用者或瀏覽器觸發的動作
以javaSCript來說
const btn = document.querySelector("#startBtn");
btn.addEventListener("click", function () {
  console.log("按鈕被點擊了");
});
| 事件          | 意思        |
| ----------- | --------- |
| `click`     | 點擊        |
| `input`     | 輸入內容改變    |
| `change`    | 值改變並確認    |
| `submit`    | 表單送出      |
| `keydown`   | 鍵盤按下      |
| `mouseover` | 滑鼠移到元素上   |
| `mouseout`  | 滑鼠離開元素    |
| `load`      | 頁面或資源載入完成 |
 HTML 上有一個元素
 JavaScript 找到這個元素
 JavaScript 綁定事件
 使用者觸發事件
 JavaScript 執行指定行為
Event 就是「觸發點」，JavaScript 負責「監聽它並執行後續動作」。



5. input 操作
HTML input 屬性 = 定義輸入框長什麼樣、預設規則
JavaScript input 操作 = 讀取或修改使用者輸入的值
HTML <input id="userName" type="text" placeholder="請輸入姓名"> 建立輸入框欄位
JavaScript 是在「操作這個輸入框」。
const input = document.querySelector("#userName");
console.log(input.value);
這邊就是紀錄寫入的內容
const name = input.value;
input.value = "安";
也可以紀錄值與修改
配合事件紀錄
const input = document.querySelector("#userName");

input.addEventListener("input", function () {
  console.log(input.value);
});
當我監控到輸入我就將輸入值寫入LOG這就是Java的主要功能
HTML + CSS 負責建立 UI，
JavaScript 負責讓 UI 具有互動、事件與資料處理能力。
HTML = 骨架
CSS = 外觀
JavaScript = 行為

6. class 切換
class 切換 = JavaScript 動態幫 HTML 元素加上或移除 class。

用途是：
不用直接一直改 style
而是透過切換 class
讓 CSS 決定畫面變化
const card = document.querySelector(".card");
card.classList.add("active");
另外說Class 可以同時存在多個
<div class="card active warning"></div> 
card
active
warning
多個CLass
另外CSS是可以先寫好屬性樣式的不會因此報錯，後續JavaScript修改載入就會自動更動了
1. 一個 HTML 元素可以有多個 class。
2. 多個 class 用空白分開。
3. CSS 可以先寫好尚未出現的狀態。
4. JavaScript 後續新增元素或切換 class，不會造成 CSS 異常。
5. CSS 會根據目前 DOM 狀態自動重新套用。

7. fetch API  
API 是應用程式與應用程式之間的介面。
它定義了雙方如何呼叫功能、
交換資料以及互相溝通的規則。
而實際通訊時，
通常會搭配特定的通訊協議與資料格式。

Fetch就是使用以下
HTTP
HTTPS
兩種通訊協議
API 跟 Fetch API 是兩個不同層級的東西。
瀏覽器提供給 JavaScript 的 API幫 JavaScript 發送 HTTP Request
JavaScript
    ↓
Fetch API
    ↓
HTTP
    ↓
後端 API
    ↓
資料庫
/////////////////////////////////////////
瀏覽器
↓
JavaScript

↓ HTTP

Web API(C#)

↓ Entity Framework(EF)是一套讓 C# 透過物件操作資料庫的 ORM 框架。

SQL Server
///ADO.NET////
我以前都是透過這種方式值對街對資料庫動作而Entity FrameworkEntity是包裝了以後提供我操作的物件

JavaScript 透過 Fetch API
向外部服務發送 HTTP Request，
並與後端 API 進行資料交換。
JavaScript 在瀏覽器中如果要透過 HTTP 與外部系統溝通，
通常會使用 Fetch API 發送 Request，
去呼叫外部提供的 Web API。

JavaScript 在瀏覽器中如果要透過 HTTP 與外部系統溝通，

通常會使用 Fetch API 發送 Request，
去呼叫外部提供的 Web API。

架構：

JavaScript
↓
Fetch API
↓
HTTP/HTTPS
↓
Web API
↓
其他系統

例如：

JavaScript
↓
Fetch
↓
ASP.NET Core API
↓
SQL Server

注意：

不是所有 JavaScript 操作都需要 Fetch。

例如：

button.textContent = "啟動";

這只是操作 DOM，不需要對外通訊。

只有以下情況通常需要 Fetch：

* 取得外部資料
* 傳送資料到伺服器
* 呼叫登入 API
* 查詢機台狀態
* 取得天氣資料

例如：

按下按鈕
↓
JavaScript
↓
Fetch
↓
機台 API
↓
取得狀態
↓
更新畫面

PLC 類比：

HTML
= 操作面板

JavaScript
= 操作邏輯

Fetch API
= 通訊模組

HTTP
= 通訊協議

Web API
= 對方設備提供的通訊介面

資料庫
= 資料儲存區

一句話：

JavaScript 在瀏覽器中若要透過 HTTP 與外部系統交換資料，
最常見的方式就是使用 Fetch API 去呼叫 Web API。




# 額外備註
C# 程式依照 API 規範呼叫功能。
API 定義了：

* 可以使用哪些功能
* 需要哪些參數
* 回傳哪些資料

當 C# 呼叫 API 時，
會透過指定的 Protocol（通訊協議）
以及 Format（資料格式）

將資料傳送到目標系統。

因此：

API = 功能介面

Protocol = 資料怎麼傳

Format = 資料長什麼樣子

目標系統收到符合規範的資料後，
執行對應功能並回傳結果。


HTML 會形成樹狀結構(DOM Tree)

CSS 與 JavaScript
透過 Selector 規則

(ID、Class、Tag、屬性、層級關係)

在這棵樹中找到目標元素。
樹狀架構
<div class="app">
    <div class="machine-card">
        <button>啟動</button>
    </div>
</div>

