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
如果 JavaScript 改的是 inline style，通常優先權會比一般 CSS 高。
但不是所有 JavaScript 都一定蓋掉 CSS。

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
瀏覽器讀取 HTML一邊讀取一邊建立 DOM 樹並載入 CSS 並套用樣式
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

# ###重點
如果 DOM 建立到一半就執行 JavaScript，
而 JavaScript 要找的元素在 HTML 後面還沒建立，
那就會找不到，也不會成功綁定事件。
所以通常前面再DOM建立的時候雖然會就載入CSS
但是JAVASCRIPT如果放在中間或前面後續才建立的DOM就會造成無法使用
因此JAVASCRIPT必須在DOM建立後才執行
CSS 規則會被瀏覽器保留，之後只要有符合選擇器的 DOM 元素出現，就會套用樣式。



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
fetch 本身一定是非同步。
不用 await，它會先送出請求，程式繼續往下跑。
用 await，只有目前 async 函式內會等結果回來後才往下跑。
fetch 本身是非同步，不會卡住主執行緒。
但因為它不會等待資料回來才繼續往下跑，
所以如果後面的程式需要使用 API 回傳資料，
就必須用 await 或 then 控制流程，
確保資料回來後再處理。

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

# 實務上常看到的async / await
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
# then的使用

fetch("/api/users")
    .then(response => response.json())
    .then(users => {
        console.log(users);
        showUsers(users);
    });
資料回來後，才執行 then 裡面的資料處理
    
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
前端最常見的流程
後端 API 回傳 JSON
↓
JavaScript 接收 JSON
↓
JavaScript 解析資料
↓
建立 HTML 元素
↓
顯示到畫面上

data/machines.json假設資料

[
    {
        "id": 1,
        "name": "機台A",
        "status": "running",
        "temperature": 32
    },
    {
        "id": 2,
        "name": "機台B",
        "status": "stop",
        "temperature": 25
    },
    {
        "id": 3,
        "name": "機台C",
        "status": "alarm",
        "temperature": 45
    }
]
# JSON可以讀取檔案也可以是後端傳過來的
1.存成 machines.json 檔案
2.由後端 API 直接回傳給前端
3.暫存在程式的字串裡
4.存在資料庫欄位裡

async function loadMachineData() {
    // 使用 fetch 讀取 JSON 檔案
    const response = await fetch("data/machines.json");

    // 把 JSON 轉成 JavaScript 可以操作的陣列或物件
    const machines = await response.json();

    // 測試看看資料有沒有成功進來
    console.log(machines);
}

// 呼叫函式，開始讀取資料
loadMachineData();
讀取近來後JAVAScript的格式就會是如下
[
    {
        id: 1,
        name: "機台A",
        status: "running",
        temperature: 32
    },
    {
        id: 2,
        name: "機台B",
        status: "stop",
        temperature: 25
    },
    {
        id: 3,
        name: "機台C",
        status: "alarm",
        temperature: 45
    }
]
兩種資料格式重點差距在於
JSON 是「文字格式」
JavaScript 物件是「程式裡的物件資料」所以KEY這邊不是文字格式


# 方式一-透過既有物件承接資料
Html
<h1>目前機台資訊</h1>
<p>名稱：<span id="machineName"></span></p>
<p>狀態：<span id="machineStatus"></span></p>
<p>溫度：<span id="machineTemperature"></span></p>
<script src="js/script.js"></script>
machineName
machineStatus
machineTemperature
有這三個放的欄位

JavaScript
async function loadMachineData() {
    // 讀取 JSON 檔案
    const response = await fetch("data/machines.json");

    // 把 JSON 轉成 JavaScript 陣列
    const machines = await response.json();

    // 先取第一筆機台資料做示範
    const machine = machines[0];

    // 把資料塞進已經存在的 HTML 元素
    document.getElementById("machineName").textContent = machine.name;
    document.getElementById("machineStatus").textContent = machine.status;
    document.getElementById("machineTemperature").textContent = machine.temperature + " °C";
}
// 執行資料載入
loadMachineData();
最後會呈現
名稱：機台A
狀態：running
溫度：32 °C
# 方式二：JavaScript 自己生成 HTML 元素
<h1>機台清單</h1>
<div id="machineList"></div>
<script src="js/script.js"></script>
等等 JavaScript 產生的機台卡片都放進這裡
CSS檔案
/* 每一張機台卡片的外觀 */
.machine-card {
    border: 1px solid black;
    padding: 10px;
    margin-bottom: 10px;
}

/* 機台名稱 */
.machine-title {
    font-size: 20px;
    font-weight: bold;
}

/* 狀態文字的基本格式 */
.machine-status {
    font-weight: bold;
}

/* running 狀態顯示綠色 */
.running {
    color: green;
}

/* stop 狀態顯示灰色 */
.stop {
    color: gray;
}

/* alarm 狀態顯示紅色 */
.alarm {
    color: red;
}
有了CSS控制屬性後透過JAVAScript去建立
async function loadMachineList() {
    // 讀取 JSON 檔案
    const response = await fetch("data/machines.json");

    // 把 JSON 轉成 JavaScript 陣列
    const machines = await response.json();

    // 取得畫面上的容器
    const machineList = document.getElementById("machineList");

    // 清空舊資料，避免重複載入時資料一直疊加
    machineList.innerHTML = "";

    // 逐筆讀取機台資料
    machines.forEach(function (machine) {
        // 建立外層卡片 div
        const card = document.createElement("div");

        // 加上固定 class，讓 CSS 可以控制外觀
        card.classList.add("machine-card");

        // 建立機台名稱 h2
        const title = document.createElement("h2");

        // 加上固定 class
        title.classList.add("machine-title");

        // 把 JSON 的 name 放進 h2
        title.textContent = machine.name;

        // 建立狀態 p
        const status = document.createElement("p");

        // 加上狀態基本 class
        status.classList.add("machine-status");

        // 再依照 JSON 的 status 加上 running / stop / alarm
        status.classList.add(machine.status);

        // 把狀態文字放進 p
        status.textContent = "狀態：" + machine.status;

        // 建立溫度 p
        const temperature = document.createElement("p");

        // 把溫度資料放進 p
        temperature.textContent = "溫度：" + machine.temperature + " °C";

        // 把 title 加進 card
        card.appendChild(title);

        // 把 status 加進 card
        card.appendChild(status);

        // 把 temperature 加進 card
        card.appendChild(temperature);

        // 最後把整張 card 加到畫面容器 machineList 裡
        machineList.appendChild(card);
    });
}
// 執行資料載入
loadMachineList();
JavaScript 不是亂生成畫面。
它是依照你寫好的 DOM 結構規則生成，
再交給 CSS class 決定外觀。

# 標準範例：script.js
// ===============================
// 1. API / 資料路徑設定區
// ===============================

// JSON 檔案路徑
const machineApiUrl = "data/machines.json";


// ===============================
// 2. DOM 元素取得區
// ===============================

// 取得畫面上用來放機台清單的容器
const machineList = document.getElementById("machineList");

// 取得重新載入按鈕
const reloadBtn = document.getElementById("reloadBtn");


// ===============================
// 3. 主功能 function 區
// ===============================

// 讀取機台資料
async function loadMachines() {
    // 呼叫 JSON / API
    const response = await fetch(machineApiUrl);

    // 如果讀取失敗，就停止
    if (!response.ok) {
        console.log("資料讀取失敗，狀態碼：", response.status);
        return;
    }

    // 將 JSON 轉成 JavaScript 陣列
    const machines = await response.json();

    // 把資料渲染到畫面
    renderMachineList(machines);
}


// 將機台資料顯示到畫面
function renderMachineList(machines) {
    // 先清空舊資料
    machineList.innerHTML = "";

    // 逐筆建立畫面元素
    machines.forEach(function (machine) {
        // 建立單張卡片
        const card = createMachineCard(machine);

        // 把卡片加入畫面容器
        machineList.appendChild(card);
    });
}


// 建立單張機台卡片
function createMachineCard(machine) {
    // 建立外層 div
    const card = document.createElement("div");
    card.classList.add("machine-card");

    // 建立機台名稱
    const title = document.createElement("h2");
    title.textContent = machine.name;

    // 建立機台狀態
    const status = document.createElement("p");
    status.textContent = "狀態：" + machine.status;
    status.classList.add(machine.status);

    // 建立機台溫度
    const temperature = document.createElement("p");
    temperature.textContent = "溫度：" + machine.temperature + " °C";

    // 組合卡片
    card.appendChild(title);
    card.appendChild(status);
    card.appendChild(temperature);

    // 回傳建立好的卡片
    return card;
}


// ===============================
// 4. 事件綁定區
// ===============================

// 當使用者按下重新載入按鈕時，重新讀取資料
reloadBtn.addEventListener("click", function () {
    loadMachines();
});


// ===============================
// 5. 初始化執行區
// ===============================

// 網頁第一次開啟時，先載入一次資料
loadMachines();
我問了GPT以下問題
所以我所想的架構應該是JAVASCRIPT的架構
1.前面會先取得物件就像是 C#宣告變數或new物件 
2.撰寫會需要使用的Function 
3.綁定事件那些事件會觸發Function在這邊綁定 
4.初始化區域某些開啟必須執行一次的功能在這邊 掃描到這邊就算完成了後面就剩下觸發事件了
另外 那我想問JavaScript有需要建立像是Timer或是Thread的重複掃描部分嗎 另外一樣式程式語言肯定也有死迴圈造成當機的機會對吧?
# JAVASCRIPT的架構大概會是如下
1.取得畫面物件
2.撰寫 function
3.綁定事件
4.初始化執行
5.script.js 掃描完成
6.後面等待事件 / Timer / API 回應觸發
另外JAVASCRIP沒有一直掃描只有在<script src="js/script.js"></script>讀取一次
script.js 載入
│
├─ 1. 取得 DOM 物件
│
├─ 2. 宣告 function
│
├─ 3. 綁定事件
│
├─ 4. 初始化執行一次
│
└─ 5. 載入完成，等待觸發
       │
       ├─ 使用者 click / input / submit
       ├─ setInterval 定時觸發
       ├─ setTimeout 延遲觸發
       └─ fetch API 回應後繼續執行

script.js 從上到下執行一次
初始化和事件綁定完成後
後面就不是一直掃描
而是等待事件、Timer、API 回應來觸發 function
重點
C# Timer      ≈ JavaScript setInterval
C# UI 卡死    ≈ JavaScript 死迴圈卡住瀏覽器
C# Thread     ≈ JavaScript Web Worker，但初期不用深入


7. 瀏覽器開發者工具 DevTools
DevTools 可以理解成：
瀏覽器內建的前端除錯工具。
你目前學前端，DevTools 主要用來看這幾件事：
. HTML / DOM 結構
. CSS 是否有套用成功
. JavaScript 有沒有錯誤
. fetch API 有沒有成功
. JSON 回傳資料長什麼樣
如何開啟DevTools
快捷F12
or
右鍵
↓
檢查 / Inspect
| 分頁          | 功能                                       |
| ----------- | ---------------------------------------- |
| Elements    | 看 HTML / DOM / CSS                       |
| Console     | 看 JavaScript 訊息與錯誤                       |
| Sources     | 看 JavaScript 檔案與中斷點                      |
| Network     | 看 API / JSON / CSS / JS 載入狀況             |
| Application | 看 localStorage / sessionStorage / cookie |

# Elements：看 HTML / DOM / CSS
1. 檢查某個 HTML 元素是否存在
2. 看 JavaScript 動態產生的元素有沒有成功加進來
3. 看 class / id 有沒有正確
4. 看 CSS 有沒有套用成功
5. 直接暫時修改 HTML / CSS 看效果
舉個例子
const card = document.createElement("div");
card.classList.add("machine-card");
card.textContent = "機台A";
machineList.appendChild(card);
我們就可以去Elements看
<div id="machineList">
    <div class="machine-card">機台A</div>
</div>
如果看不到代表有問題
JavaScript 沒執行
machineList 沒抓到
appendChild 沒執行
資料根本沒進來
就是可以除錯

# Console：看 JavaScript 訊息與錯誤
# Network：看 API / JSON / 檔案載入還有 API 有沒有被呼叫 OR HTTP 狀態碼是 200、404 還是 500 OR Request URL 是否正確 OR 後端回傳的 JSON 長什麼樣 OR CSS / JS 檔案有沒有載入失敗
# Sources：看 JS 檔案與中斷點
# Application：看瀏覽器儲存資料

總結
Elements：看 DOM 和 CSS
Console：看 JavaScript 錯誤與 log
Network：看 API / JSON / 檔案有沒有成功載入
Sources：下中斷點看 JS 執行流程
Application：看瀏覽器儲存資料

畫面沒出來
↓
先看 Console 有沒有錯
↓
再看 Network 有沒有載到資料
↓
再看 Elements 有沒有產生 DOM

DevTools 是前端的 Visual Studio Debugger。
它讓你看到 HTML、CSS、JavaScript、API 到底哪一層出問題。


8. 前後端整體資料流
前後端資料流的核心是：
前端不直接碰資料庫
前端透過 API 跟後端要資料或送資料
後端處理完後用 JSON 回傳
前端再把 JSON 轉成畫面


HTML        = 畫面結構 / 框架
CSS         = 樣式外觀
JavaScript  = 畫面互動、事件、資料請求
API         = 前後端資料交換入口
後端        = 商業邏輯、資料處理、資料庫存取
Database    = 真正資料儲存位置
前端不是直接操作後端資料，而是透過 API 這個指定出口交換資料。

# Request / Response
fetch 呼叫 API，但還要知道一次 API 溝通其實分兩段：
Request  = 前端送出去的請求
Response = 後端回傳的結果

# HTTP Method
GET    = 查詢資料
POST   = 新增資料
PUT    = 修改資料
DELETE = 刪除資料

# URL 路由
API 會有指定路徑這些路徑就是前端呼叫後端的「入口地址」。

# 狀態碼
例如
200 = 成功
404 = 找不到
500 = 後端錯誤

# 權限 / Token
正式系統通常不是每個 API 都能直接呼叫。
可能需要
登入
Token
權限驗證


如果不用非同步，或寫了耗時同步程式，
JavaScript 會卡住瀏覽器主執行緒。
結果不是只有某個事件卡住，
而是 UI 更新、點擊事件、輸入事件都可能沒反應。