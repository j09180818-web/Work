1. 前端檔案架構
前端檔案架構就是：
網頁專案中，各種檔案如何分類、放在哪裡、彼此如何引用。
| 檔案類型       | 副檔名     | 主要功能         |
| ---------- | ------- | ------------ |
| HTML       | `.html` | 畫面結構         |
| CSS        | `.css`  | 畫面樣式         |
| JavaScript | `.js`   | 畫面行為、事件、資料處理 |
my-web-project
│
├─ index.html(通常是網頁入口)
├─ style.css(負責畫面樣式)
└─ script.js(負責事件與邏輯)
HTML = 物件與畫面架構
CSS = 外觀設定
JavaScript = 事件與邏輯

比較完整的架構可能會是
my-web-project
│
├─ index.html
│
├─ css
│   └─ style.css
│
├─ js
│   └─ script.js
│
└─ images
    └─ logo.png


/      從網站根目錄開始找
./     從目前 HTML 所在位置開始找
../    從目前 HTML 所在位置回上一層找


2. HTML / CSS / JavaScript 載入關係
瀏覽器取得 HTML
↓
從上到下解析 HTML
↓
遇到 CSS 連結
↓
下載 CSS，準備外觀規則
↓
繼續建立 HTML 結構與 DOM
↓
遇到 JavaScript 連結
↓
下載並執行 JavaScript
↓
JavaScript 操作 DOM、事件、資料
↓
網頁完成顯示與互動

故載入順序通常會先在讀取HTML最上層載入所需的CSS可以多個載入在<head>
<head>
    <link rel="stylesheet" href="css/style.css">
</head>
依序讀取架構建立DOM後最後在</body>前載入JAVAScript可以多個
<body>
    <button id="startBtn">啟動</button>
         ..........最後>
    <script src="js/script.js"></script>
</body>
CSS:
CSS 載入一次
↓
瀏覽器比對 HTML / DOM 元素
↓
符合選擇器的元素就套用樣式
JavaScript:
程式邏輯它會主動取得 DOM 元素並操作。
完整範例
<!DOCTYPE html>
<html>
<head>
    <title>設備監控</title>

    <!-- 載入 CSS -->
    <link rel="stylesheet" href="css/style.css">
</head>

<body>
    <h1>設備監控系統</h1>

    <button id="startBtn">啟動</button>

    <!-- 載入 JavaScript -->
    <script src="js/script.js"></script>
</body>
</html>
CSS 用 <link>
JavaScript 用 <script>
<>
而
JavaScript 會照順序執行。
所以如果 main.js 需要用到 config.js 的資料，config.js 就要放前面。
同個標籤在不同 CSS 檔案被修改會怎樣？
看後載入的 CSS，以及選擇器優先權。
<button id="startBtn" class="btn">啟動</button>
.btn {
    color: blue;
}
.btn {
    color: red;
}
載入時候若這樣寫
<link rel="stylesheet" href="css/base.css">
<link rel="stylesheet" href="css/machine.css">
則最後會顯示read
因為 machine.css 比較後面載入。
但如果選擇器優先權不同，不一定後面贏
#startBtn 是 ID 選擇器
.btn 是 class 選擇器
ID 優先權比 class 高
所以如果一天就算最後執行.btn也會是#startBtn修改到
優先權關西
先看優先權：
ID > class > 標籤
如果優先權一樣：
後載入的 CSS 贏
<>

CSS：
看選擇器優先權
優先權一樣時，看誰後載入
JavaScript：
看執行順序
後執行的修改通常會蓋掉前面的結果
CSS 是規則互相競爭
JavaScript 是程式照順序執行
而通常javaScript會蓋掉Css

3. DOM 操作完整流程
Document Object Model
可以先理解成：
瀏覽器把 HTML 解析後，建立出來的一棵物件樹。
JavaScript 要操作畫面，不是直接操作 HTML 原始碼，而是操作 DOM。
<body>
    <h1>設備監控</h1>

    <p id="status">停止</p>

    <button id="startBtn">啟動</button>
</body>
會變成類似
document
└─ html
   └─ body
      ├─ h1
      │  └─ 設備監控
      ├─ p#status
      │  └─ 停止
      └─ button#startBtn
         └─ 啟動
這種架構DOM
1. HTML 建立畫面元素
2. JavaScript 取得 DOM 元素
3. JavaScript 綁定事件
4. 使用者觸發事件
5. JavaScript 修改 DOM，畫面跟著改變
<完整範例>
Html:
<!DOCTYPE html>
<html>
<head>
    <title>DOM 操作範例</title>
</head>
<body>

    <h1>設備監控系統</h1>

    <p>目前狀態：<span id="statusText">停止</span></p>

    <button id="startBtn">啟動</button>
    <button id="stopBtn">停止</button>

    <script src="js/script.js"></script>
</body>
</html>
JavaScript:
const statusText = document.getElementById("statusText");
const startBtn = document.getElementById("startBtn");
const stopBtn = document.getElementById("stopBtn");

startBtn.addEventListener("click", function () {
    statusText.textContent = "運轉中";
    statusText.style.color = "green";
});

stopBtn.addEventListener("click", function () {
    statusText.textContent = "停止";
    statusText.style.color = "red";
});

#1流程拆解
<span id="statusText">停止</span>
<button id="startBtn">啟動</button>
<button id="stopBtn">停止</button>
這些會被瀏覽器建立成 DOM 元素。
#2JavaScript 取得 DOM 元素
const statusText = document.getElementById("statusText");
const startBtn = document.getElementById("startBtn");
const stopBtn = document.getElementById("stopBtn");
從整份網頁 document 裡
找到指定 id 的元素
然後存到變數
startBtn.addEventListener("click", function () {
    statusText.textContent = "運轉中";
});
綁定事件
等待使用者觸發事件
statusText.textContent = "運轉中";
statusText.style.color = "green";
把畫面文字改成「運轉中」
把文字顏色改成綠色
畫面立刻修改
比較好的寫法：JS 改 class，CSS 負責樣式
使用者開啟網頁
        │
        ▼
瀏覽器讀取 HTML
        │
        ▼
建立 DOM 樹
        │
        ▼
載入 CSS 並套用樣式
        │
        ▼
載入 JavaScript
        │
        ▼
JS 取得 DOM 元素
        │
        ▼
JS 綁定事件
        │
        ▼
使用者操作畫面
        │
        ▼
事件被觸發
        │
        ▼
JS 修改 DOM
        │
        ▼
畫面更新
因此:JavaScript 先取得 DOM 元素，再綁定事件，最後在事件中修改 DOM。
瀏覽器讀取 HTML
↓
一邊解析 HTML，一邊建立 DOM
↓
遇到 CSS，就載入 CSS 規則
↓
CSS 規則套用到已建立 / 之後建立的 DOM 元素
↓
遇到 JavaScript，就執行 JS
↓
JS 主動操作 DOM
CSS 不是直接修改 HTML 原始碼
CSS 是被瀏覽器解析成樣式規則
再套用到 DOM 元素上
CSS：
被動規則
瀏覽器自動把樣式套到符合條件的 DOM 元素
JavaScript：
主動程式
執行後主動查找、修改、控制 DOM 元素



4. 表單資料收集
使用者在畫面輸入資料
↓
JavaScript 從 input / select / textarea 取值
↓
整理成物件或 JSON
↓
之後可以送給後端 API

常見的元素有
| HTML 元素      | 用途   |
| ------------ | ---- |
| `<input>`    | 單行輸入 |
| `<select>`   | 下拉選單 |
| `<textarea>` | 多行文字 |
| `<button>`   | 按鈕   |
| `<form>`     | 表單容器 |
<form id="machineForm">
    <label>機台名稱：</label>
    <input type="text" id="machineName">

    <label>機台狀態：</label>
    <select id="machineStatus">
        <option value="stop">停止</option>
        <option value="running">運轉中</option>
        <option value="alarm">異常</option>
    </select>

    <label>備註：</label>
    <textarea id="remark"></textarea>

    <button type="submit">送出</button>
</form>
機台名稱：[          ]
機台狀態：[停止 ▼]
備註：    [          ]
         [          ]

[送出]
const form = document.getElementById("machineForm");

form.addEventListener("submit", function (event) {
    event.preventDefault();

    const machineName = document.getElementById("machineName").value;
    const machineStatus = document.getElementById("machineStatus").value;
    const remark = document.getElementById("remark").value;

    const data = {
        machineName: machineName,
        machineStatus: machineStatus,
        remark: remark
    };

    console.log(data);
});
取得視窗後會收集相關資料與事件觸發後的所有資訊整理成一個資料物件是JSON的架構
之後這個物件就可以轉成 JSON，送給後端 API。
1.HTML 建立表單欄位
2.JavaScript 監聽 submit 事件
3.用 .value 取得欄位資料
4.整理成物件 / JSON
DOM 元素本身不是資料
.value 才是使用者輸入的資料document.getElementById("欄位ID").value;



5. fetch 呼叫 API
前端 JavaScript
↓
用 fetch 向後端 API 發送請求
↓
後端回傳資料，通常是 JSON
↓
前端接收資料
↓
顯示到畫面或做後續處理
用C#理解大概是
HttpClient client = new HttpClient();
var response = await client.GetAsync("https://example.com/api/machines");

JavaScript則是
fetch("https://example.com/api/machines")

Fetch常見的Get範例
fetch("https://example.com/api/machines")
    .then(function (response) {
        return response.json();
    })
    .then(function (data) {
        console.log(data);
    });
fetch 呼叫 API
↓
伺服器回傳 response
↓
response.json() 把 JSON 轉成 JavaScript 物件
↓
data 就是前端可使用的資料
實務上常看到非同步的async / await
async function loadMachines() {
    const response = await fetch("https://example.com/api/machines");
    const data = await response.json();

    console.log(data);
}

loadMachines();

假設 API 回傳資料
Json
[
    {
        "id": 1,
        "name": "機台A",
        "status": "running"
    },
    {
        "id": 2,
        "name": "機台B",
        "status": "stop"
    }
]
JavaScript取得後
[
    {
        id: 1,
        name: "機台A",
        status: "running"
    },
    {
        id: 2,
        name: "機台B",
        status: "stop"
    }
]
Html代碼
<h1>機台清單</h1>

<div id="machineList"></div>

<script src="js/script.js"></script>
JavaScript
async function loadMachines() {
    const response = await fetch("https://example.com/api/machines");
    const machines = await response.json();

    const machineList = document.getElementById("machineList");

    machines.forEach(function (machine) {
        const item = document.createElement("div");

        item.textContent = machine.name + "：" + machine.status;

        machineList.appendChild(item);
    });
}

loadMachines();
顯示
機台A：running
機台B：stop

# POST 送資料給 API
async function addMachine() {
    const machineData = {
        name: "機台C",
        status: "stop"
    };

    const response = await fetch("https://example.com/api/machines", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(machineData)
    });

    const result = await response.json();

    console.log(result);
}

addMachine();


把 JavaScript 物件轉成 JSON 字串。
因為 HTTP 傳輸時不能直接送 JavaScript 物件，要轉成文字格式。

# 結合表單資料送 API
<form id="machineForm">
    <input type="text" id="machineName" placeholder="機台名稱">

    <select id="machineStatus">
        <option value="running">運轉中</option>
        <option value="stop">停止</option>
        <option value="alarm">異常</option>
    </select>

    <button type="submit">送出</button>
</form>
<script src="js/script.js"></script>

JavaScript

const form = document.getElementById("machineForm");
form.addEventListener("submit", async function (event) {
    event.preventDefault();

    const machineData = {
        name: document.getElementById("machineName").value,
        status: document.getElementById("machineStatus").value
    };

    const response = await fetch("https://example.com/api/machines", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(machineData)
    });

    const result = await response.json();

    console.log(result);
});

使用者輸入資料
↓
按下送出
↓
JavaScript 收集 input / select 的 value
↓
整理成 machineData
↓
JSON.stringify() 轉成 JSON 字串
↓
fetch POST 給 API
↓
後端接收資料
↓
後端回傳結果

#fetch 完整流程圖
前端 JavaScript
        │
        ▼
fetch 呼叫 API URL
        │
        ▼
瀏覽器送出 HTTP Request
        │
        ▼
後端 API 接收請求
        │
        ▼
後端處理資料 / 查資料庫
        │
        ▼
後端回傳 HTTP Response
        │
        ▼
前端收到 response
        │
        ▼
response.json() 轉成 JS 物件
        │
        ▼
JavaScript 使用資料
        │
        ▼
顯示到 DOM 畫面

| 方法     | 用途     | C# / 資料庫概念     |
| ------ | ------ | -------------- |
| GET    | 取得資料   | Read           |
| POST   | 新增資料   | Create         |
| PUT    | 修改整筆資料 | Update         |
| PATCH  | 修改部分資料 | Partial Update |
| DELETE | 刪除資料   | Delete         |



# Http狀態碼
const response = await fetch("https://example.com/api/machines");
會回傳一個數字與結果
404 Not Found
500 Internal Server Error
這些就是HTTP的狀態碼
大致分為
200 ~ 299 = 成功
300 ~ 399 = 重新導向
400 ~ 499 = 前端請求錯誤
500 ~ 599 = 後端伺服器錯誤
| 狀態碼 | 意思     | 常見情境           |
| --- | ------ | -------------- |
| 200 | 成功     | GET 成功取得資料     |
| 201 | 建立成功   | POST 新增資料成功    |
| 400 | 請求格式錯誤 | 送出的 JSON 格式不對  |
| 401 | 未登入    | 沒有登入或 Token 錯誤 |
| 403 | 無權限    | 已登入但沒有權限       |
| 404 | 找不到    | API 網址錯誤或資料不存在 |
| 500 | 伺服器錯誤  | 後端程式發生例外       |

| 動作    | 用法                                   | 意思             |
| ----- | ------------------------------------ | -------------- |
| 前端送資料 | `JSON.stringify(data)`               | JS 物件轉 JSON 字串 |
| 前端收資料 | `response.json()`                    | JSON 回應轉 JS 物件 |
| 設定格式  | `"Content-Type": "application/json"` | 告訴後端我送的是 JSON  |

HTTP Response
├─ Status Code：400 / 200 / 500 ...
├─ Headers：Content-Type: application/json
└─ Body：JSON 內容
C#回傳的方法
return Ok(...);              // 200
return Created(...);         // 201
return BadRequest(...);      // 400
return Unauthorized(...);    // 401
return Forbid();             // 403
return NotFound(...);        // 404
return Conflict(...);        // 409
return StatusCode(500, ...); // 500


6. JSON 資料顯示到畫面

7. 瀏覽器開發者工具 DevTools

8. 前後端整體資料流