超文本標記語言
HTML架構寫錯時
瀏覽器可能自行修正
也可能造成顯示異常
因此開發者應依照HTML規範撰寫
避免不可預期的結果
1. HTML是什麼
<!DOCTYPE html>
<html>
<head>
<title>練習用網頁</title>
<!--這是一個網頁標題就像是網頁使用時頁籤顯示的部分-->
</head>
<body>
<!--body就是網頁的身體軀體內部就是我寫網頁框架的部分-->

<h1>My First Heading</h1>
<!--六種標題等級h1~h6就像是資料夾又或者專案的層級
h1 C#學習筆記
└─ h2 通訊
    └─ h3 Modbus
        └─ h4 TCP
            └─ h5 Socket
                ├─ h6 Connect
                ├─ h6 Disconnect
                ├─ h6 Send
                └─ h6 Receive-->

<p>My first paragraph.</p>
<p>文件 <br>使用br換行意思是break Line</p>

</body>
</html>
這是一個框架概念
href是連結指定目標不一定只有網址也可以是檔案
<a href"網址區域">顯示文字</a>
<img src="img_girl1.jpg"alt= "錯誤" width="500" height="600">
<img src="logo.png">時候圖片會放在哪?答案是與Html相同路徑的資料夾所以你要給的路徑就是你指標到的路徑必須給到能搜尋的範圍如果是在上一層資料夾則必須使用../這代表是上一層


在 HTML 中，屬性值通常使用雙引號括起來，但也可以使用單引號。
在某些情況下，當屬性值本身包含雙引號時，必須使用單引號：反之亦然
故當你需要表達''在文字中那外框必須"''"反之你要表達""則'""'
# h的層級概念
<!DOCTYPE html>
<html>
<head>
<title>專案資料夾架構</title>
</head>
<body>
<h1>專案資料夾</h1> <h2>控制檔案</h2> <h3>控制參數檔案</h3> <h2>系統檔案</h2> <h2>文本檔案區</h2> <h3>各類別文本資料分區</h3> <h4>PLC文本檔案</h4> <h5>各廠牌PLC</h5> <h6>三菱PLC資料文本</h6> <h6>三菱PLC技術手冊文本</h6> <h4>技術手冊文本</h4> <h5>馬達技術手冊</h5>
</body>
</html>
大概層級概念如下
專案資料夾 h1
│
├─ 控制檔案 h2
│   └─ 控制參數檔案 h3
│
├─ 系統檔案 h2
│
└─ 文本檔案區 h2
    └─ 各類別文本資料分區 h3
        ├─ PLC文本檔案 h4
        │   └─ 各廠牌PLC h5
        │       ├─ 三菱PLC資料文本 h6
        │       └─ 三菱PLC技術手冊文本 h6
        │
        └─ 技術手冊文本 h4
            └─ 馬達技術手冊 h5
h1~h6 的層級概念
不是用來直接顯示樹狀圖

而是用來描述文章、文件、網頁內容的重要層級
所以顯示上你可能看起來只是標題文字大小不同實際再運用時候依定會有層級概念
<hr>：水平分隔線會在畫面上出現一條水平線切割
<pre>文章段落</pre>會按照你的文章保留格式簡單說你的空白換行等等的都會相同
Html框架中的文字
<h1 style="font-family:verdana;">This is a heading</h1>
<p  style="font-family:courier;">This is a paragraph.</p>
或者
<h1 style="color:red;">This is a heading</h1>
<p  style="color:red;">This is a paragraph.</p>
可以設定字形或是文字顏色
以下是文字修改的特殊字眼
| 標籤         | 中文意思 | 主要用途       |
| ---------- | ---- | ---------- |
| `<b>`      | 粗體   | 只改外觀，讓文字變粗 |
| `<strong>` | 重要文字 | 表示內容重要     |
| `<i>`      | 斜體   | 只改外觀，讓文字變斜 |
| `<em>`     | 強調文字 | 表示語氣強調     |
| `<mark>`   | 標記文字 | 重點標示、搜尋結果  |
| `<small>`  | 小字   | 補充說明、版權、註解 |
| `<del>`    | 刪除文字 | 表示舊資料或已刪除  |
| `<ins>`    | 插入文字 | 表示新增資料     |
| `<sub>`    | 下標   | 化學式、下標     |
| `<sup>`    | 上標   | 次方、註腳、上標   |


# div 包覆區塊 主要用於提供CSS 或者javaScript運用
例如
<div> <h2>設備狀態</h2> <p>目前狀態：運轉中</p> </div>
這就會視為是個小區塊
而真正包覆區塊如何搭配使用就必須提到Id class
| 屬性      | 概念   | 適合用途     |
| ------- | ---- | -------- |
| `id`    | 唯一代號 | 單一指定元素   |
| `class` | 分類名稱 | 多個元素共用樣式 |
<div class="card active warning"></div> 
card
active
warning
多個CLass

沒有class id 可以單純透過div來找區塊但這會使的所有區塊被搜尋到就像是最基底的物件一樣具有共通的性質
Class假設我取名子叫做 PLC 可以重複例如我很多機器都是PLC所以Class用PLC去找到這些機器
而ID就像身分證我設定ID在這Html唯一性所以一定能找到指定的這個區塊

# C#
#machineA {
  background-color: lightgray;
}
#machineA {
  background-color: lightgray;
}
# javascript
const cards = document.querySelectorAll(".machine-card");
#machineA {
  background-color: lightgray;
}
# <span> 與 div相似但範圍更小，他是以段落小區域標記為主
例如今天若要修改某區域參數離如溫度
Html:
<p>
  目前溫度：
  <span id="temperatureValue">28</span>
  °C
</p>
Javascript:
document.getElementById("temperatureValue").innerText = "30";
| 項目   | `<div>`            | `<span>`            |
| ---- | ------------------ | ------------------- |
| 類型   | block element 區塊元素 | inline element 行內元素 |
| 是否換行 | 會換行                | 不會換行                |
| 用途   | 包一整個區塊             | 包一小段文字              |
| 常見用途 | 卡片、區域、版面容器         | 文字上色、標記狀態、局部修改      |
| 本身語意 | 沒語意                | 沒語意                 |
<div class="machine-card">
  <h2>設備 A</h2>
  <p>
    狀態：
    <span id="statusText" class="running">運轉中</span>
  </p>
</div>
div 負責整張設備卡片
p 負責一段文字
span 負責這段文字中的「狀態值」
分工明確


# block/inline 
block / inline 是 CSS display 顯示方式的概念
因為 block / inline 是顯示方式，所以 CSS 可以改它。
例如把 <span> 改成像 block 一樣：
<span class="item">設備 A</span>
<span class="item">設備 B</span>
原本顯示時不會換行但CSS使用
.item {
  display: block;
}
就會換行
HTML 決定「這是什麼元素」
CSS display 決定「它怎麼排列」

常用屬性表
| 屬性               | 中文理解   | 是否可重複 | 主要用途                        | 範例                                 |
| ---------------- | ------ | ----: | --------------------------- | ---------------------------------- |
| `id="..."`       | 唯一名稱   |  不應重複 | 給 CSS / JavaScript 精準找到單一元素 | `<main id="mainContent">`          |
| `class="..."`    | 分類名稱   |   可重複 | 給多個元素共用樣式或群組控制              | `<article class="machine-card">`   |
| `style="..."`    | 直接寫樣式  |   可使用 | 直接在元素上寫 CSS，不建議大量使用         | `<p style="color:red;">異常</p>`     |
| `data-xxx="..."` | 自訂資料   |   可使用 | 存放給 JavaScript 使用的額外資料      | `<button data-machine-id="M01">`   |
| `title="..."`    | 提示文字   |   可使用 | 滑鼠移到元素上時顯示提示                | `<span title="設備正常運轉">運轉中</span>`  |
| `hidden`         | 隱藏元素   |   可使用 | 讓元素不顯示                      | `<section hidden>`                 |
| `lang="..."`     | 語言設定   |   可使用 | 指定內容語言                      | `<p lang="en">Hello</p>`           |
| `tabindex="..."` | 鍵盤焦點順序 |   可使用 | 控制鍵盤 Tab 是否可選到              | `<button tabindex="0">送出</button>` |


# 語意化標籤 semantic tags
| 標籤             | 英文             | 中文理解      | 常見用途            | 類似 div 嗎  |
| -------------- | -------------- | --------- | --------------- | --------- |
| `<header>`     | Header         | 頁首 / 區塊開頭 | 網站標題、Logo、上方資訊  | 像有語意的 div |
| `<nav>`        | Navigation     | 導覽列       | 選單、頁面連結區        | 像有語意的 div |
| `<main>`       | Main Content   | 主要內容      | 一頁中最核心內容        | 像有語意的 div |
| `<section>`    | Section        | 主題區段      | 一個主題區塊          | 像有語意的 div |
| `<article>`    | Article        | 獨立內容      | 文章、卡片、報表項目、設備項目 | 像有語意的 div |
| `<aside>`      | Aside          | 側邊 / 補充內容 | 側邊欄、備註、警報摘要     | 像有語意的 div |
| `<footer>`     | Footer         | 頁尾 / 區塊結尾 | 版權、聯絡資訊、底部資訊    | 像有語意的 div |
| `<figure>`     | Figure         | 圖片或圖表區    | 圖片、圖表、示意圖       | 不是單純 div  |
| `<figcaption>` | Figure Caption | 圖片說明      | 圖片或圖表標題說明       | 搭配 figure |
| `<time>`       | Time           | 時間        | 日期、時間、紀錄時間      | 不是 div    |
| `<mark>`       | Mark           | 標記重點      | 高亮文字            | 不是 div    |

語意化標籤是為了讓架構更容易區分與讀寫，可以更直白的讓人讀懂整個架構，但如果沒有明確的使用語意化標籤，例如把 footer 的內容放在 main 區塊中，瀏覽器通常不會報錯，
但看代碼的人會很痛苦，語意化標籤可以搭配上屬性讓CSS與JAVASCRIPT去控制



# 清單
ul = unordered list，無序清單
ol =     list，有序清單
li = list item，清單項目
#############################
ul 或 ol = 清單容器
li = 清單裡面的每一筆項目 Item

1. ol 
<ol>
  <li>開啟電源</li>
  <li>確認急停按鈕復歸</li>
  <li>啟動設備</li>
</ol>1
畫面會呈現
1. 開啟電源
2. 確認急停按鈕復歸
3. 啟動設備
具有序號的清單

2. ul
<ul>
  <li>PLC-01</li>
  <li>PLC-02</li>
  <li>PLC-03</li>
</ul>
畫面呈現
• PLC-01
• PLC-02
• PLC-03

而li=項目就是用法中每個項目的項目列
而項目中也可以放很多元素Html非常自由
也可組合使用巢狀結構
搭配CSS
<ul class="machine-list">
  <li class="machine-item">PLC-01：運轉中</li>
  <li class="machine-item">PLC-02：停止</li>
  <li class="machine-item">PLC-03：異常</li>
</ul>
而CSS也可以為他拿掉圓點
.machine-list {
  list-style: none;
  padding-left: 0;
}
像很多網站導覽列
<nav>
  <ul>
    <li><a href="/dashboard">首頁</a></li>
    <li><a href="/machines">設備</a></li>
    <li><a href="/reports">報表</a></li>
  </ul>
</nav>
nav = 導覽區
ul = 導覽項目清單
li = 每一個選單項目
a = 可點擊連結



# 表單容器form與button
<form action="/search" method="get">
  <input name="deviceName" type="text">
  <button type="submit">查詢</button>
</form>
假設輸入PLC01按下按鈕後
GET /search?deviceName=PLC01
假設目前網站在http://localhost:3000
請求位置http://localhost:3000/search?deviceName=PLC01
收到的是localhost:3000 這個伺服器
| 語法                  | 作用                 |
| ------------------- | ------------------ |
| `action="/search"`  | 告訴瀏覽器送去哪個路徑        |
| `method="get"`      | 告訴瀏覽器資料放在網址後面      |
| `name="deviceName"` | 告訴瀏覽器這個輸入框送出時的參數名稱 |
| 使用者輸入 `PLC01`       | 變成參數值              |
| `type="submit"`     | 按下後觸發送出 form       |
form的屬性表

| 屬性                                            | 中文理解      |   是否常用 | 主要用途                        | 範例                                                   |
| --------------------------------------------- | --------- | -----: | --------------------------- | ---------------------------------------------------- |
| `action="..."`                                | 送出目標      |     常用 | 指定表單送出後要送到哪個路徑 / 網址         | `<form action="/search">`                            |
| `method="get"`                                | GET 方式送出  |     常用 | 把資料放在網址後面，常用於查詢、搜尋、篩選       | `<form method="get">`                                |
| `method="post"`                               | POST 方式送出 |     常用 | 把資料放在 HTTP Body，不直接顯示在網址上   | `<form method="post">`                               |
| `id="..."`                                    | 表單唯一名稱    |     常用 | 給 JavaScript / CSS 精準找到這個表單 | `<form id="searchForm">`                             |
| `class="..."`                                 | 表單分類名稱    |     常用 | 給 CSS 套樣式，或 JS 群組控制         | `<form class="search-form">`                         |
| `name="..."`                                  | 表單名稱      |    較少用 | 給表單本身取名稱，舊式 JS 可能會用到        | `<form name="deviceForm">`                           |
| `target="_self"`                              | 在目前頁開啟    |    較少用 | 表單送出結果在目前頁面開啟，預設值           | `<form target="_self">`                              |
| `target="_blank"`                             | 在新分頁開啟    |    較少用 | 表單送出結果在新分頁開啟                | `<form target="_blank">`                             |
| `autocomplete="on"`                           | 自動完成開啟    |     常用 | 允許瀏覽器自動填入曾輸入過的資料            | `<form autocomplete="on">`                           |
| `autocomplete="off"`                          | 自動完成關閉    |     常用 | 不讓瀏覽器自動填入資料                 | `<form autocomplete="off">`                          |
| `novalidate`                                  | 不檢查欄位     |    較少用 | 送出時不使用瀏覽器內建驗證               | `<form novalidate>`                                  |
| `enctype="application/x-www-form-urlencoded"` | 預設編碼      |   常用預設 | 一般表單預設格式                    | `<form enctype="application/x-www-form-urlencoded">` |
| `enctype="multipart/form-data"`               | 檔案上傳格式    | 上傳檔案常用 | 表單有檔案上傳時使用                  | `<form method="post" enctype="multipart/form-data">` |
| `enctype="text/plain"`                        | 純文字格式     |     少用 | 用純文字格式送出資料                  | `<form enctype="text/plain">`                        |
| `accept-charset="UTF-8"`                      | 字元編碼      |     少用 | 指定表單送出時的文字編碼                | `<form accept-charset="UTF-8">`                      |




沒有表單時候
<label for="deviceName">設備名稱：</label>
<input id="deviceName" type="text">

<button type="button" onclick="searchDevice()">查詢</button>

<script>
  function searchDevice() {
    const deviceName = document.getElementById("deviceName").value;
    alert("查詢設備：" + deviceName);
  }
</script>


重點加強
1. HTML 元素有父子關係 我自己角度更想說是框架關西
這個很重要。
多種階層框架無論是框架或是大框架 CSS 跟JAVASCRIPT都可以透過框架去更改內部或調用調整內部動作
例如框架名稱 ID A 內部全部的button需要修改CSS只要叫這個框架名稱即可
app 大框架
├─ areaA 框架
│  ├─ h2 元件
│  ├─ machine-card 框架
│  │  ├─ h3 元件
│  │  ├─ p 元件
│  │  │  └─ status 小元件
│  │  ├─ button 元件
│  │  └─ button 元件
│  │
│  └─ machine-card 框架
│     ├─ h3 元件
│     ├─ p 元件
│     │  └─ status 小元件
│     ├─ button 元件
│     └─ button 元件
│
└─ areaB 框架
   ├─ h2 元件
   ├─ machine-card 框架
   │  ├─ h3 元件
   │  ├─ p 元件
   │  │  └─ status 小元件
   │  ├─ button 元件
   │  └─ button 元件
   │
   └─ machine-card 框架
      ├─ h3 元件
      ├─ p 元件
      │  └─ status 小元件
      ├─ button 元件
      └─ button 元件
CSS
#areaA button {
  background-color: lightblue;
}
Java
const areaA = document.getElementById("areaA");
const buttonsInAreaA = areaA.querySelectorAll("button");




2. HTML 本身不負責互動邏輯
3. 表單送出資料的概念要再穩一點form
| 技術         | 負責    |
| ---------- | ----- |
| HTML       | 畫面結構  |
| CSS        | 外觀樣式  |
| JavaScript | 互動與事件 |

4. name 跟 id 不一樣，name表單的是資料欄位名稱
| 項目       | `id`                         | `name`      |
| -------- | ---------------------------- | ----------- |
| 中文理解     | 元素唯一代號                       | 表單資料欄位名稱    |
| 主要用途     | CSS / JavaScript / label 找元素 | form 送資料給後端 |
| 是否應唯一    | 是，同一頁不應重複                    | 不一定         |
| CSS 怎麼選  | `#deviceNameInput`           | 通常不用它選      |
| JS 怎麼找   | `getElementById()`           | 表單資料或特殊情況使用 |
| 送出表單是否需要 | 不一定                          | 需要          |


5. HTML的路徑
| 標籤         | 屬性     | 用途              |
| ---------- | ------ | --------------- |
| `<img>`    | `src`  | 圖片位置            |
| `<a>`      | `href` | 連結位置            |
| `<link>`   | `href` | CSS 檔案位置        |
| `<script>` | `src`  | JavaScript 檔案位置 |
<img src="images/logo.png">
<link rel="stylesheet" href="style.css">
<script src="app.js"></script>
<a href="about.html">關於我們</a>
從 index.html 所在資料夾開始找
style.css      → 跟 index.html 同一層
app.js         → 跟 index.html 同一層
about.html     → 跟 index.html 同一層
images/logo.png → 進入 images 資料夾找 logo.png
./ 代表目前資料夾
../ 代表上一層資料夾
/ 開頭代表網站根目錄
logo.png
→ 同一層找 logo.png

images/logo.png
→ 進入 images 資料夾找 logo.png

./images/logo.png
→ 從目前資料夾進入 images 找 logo.png

../images/logo.png
→ 回上一層，再進入 images 找 logo.png

/images/logo.png
→ 從網站根目錄開始找 images/logo.png

https://example.com/logo.png
→ 從外部網站找


#areaA
→ 找 id="areaA" 的框架

.start-button
→ 找 class="start-button" 的元素

#areaA .start-button
→ 找 id="areaA" 框架裡面，class="start-button" 的元素



下方為以後查閱用的
# 1. HTML 常用屬性表格

## 1-1. 全域屬性 Global Attributes

| 屬性                       | 中文理解   | 是否可重複 | 常用於   | 主要用途                                | 範例                                      |
| ------------------------ | ------ | ----: | ----- | ----------------------------------- | --------------------------------------- |
| `id="..."`               | 唯一名稱   |  不應重複 | 大多數標籤 | 給 CSS / JavaScript / label 精準找到單一元素 | `<input id="deviceName">`               |
| `class="..."`            | 分類名稱   |   可重複 | 大多數標籤 | 給多個元素共用 CSS 樣式或 JS 群組控制             | `<div class="card">`                    |
| `style="..."`            | 行內樣式   |   可使用 | 大多數標籤 | 直接在元素上寫 CSS，不建議大量使用                 | `<p style="color:red;">異常</p>`          |
| `title="..."`            | 提示文字   |   可使用 | 大多數標籤 | 滑鼠移到元素上時顯示提示                        | `<span title="設備正常">運轉中</span>`         |
| `hidden`                 | 隱藏元素   |   可使用 | 大多數標籤 | 讓元素不顯示                              | `<section hidden>`                      |
| `lang="..."`             | 語言設定   |   可使用 | 大多數標籤 | 指定內容語言                              | `<p lang="en">Hello</p>`                |
| `data-xxx="..."`         | 自訂資料   |   可使用 | 大多數標籤 | 存放給 JavaScript 使用的額外資料              | `<button data-id="M01">查詢</button>`     |
| `tabindex="..."`         | 鍵盤焦點順序 |   可使用 | 可互動元素 | 控制鍵盤 Tab 是否可選到元素                    | `<button tabindex="0">查詢</button>`      |
| `contenteditable="true"` | 可編輯內容  |   可使用 | 文字區塊  | 讓使用者可直接編輯內容                         | `<div contenteditable="true">可編輯</div>` |
| `draggable="true"`       | 可拖曳    |   可使用 | 大多數標籤 | 讓元素可以被拖曳                            | `<div draggable="true">拖曳我</div>`       |

---

## 1-2. 表單 Form 常用屬性

| 屬性                  | 中文理解    | 常用於                                   | 主要用途                     | 範例                                     |
| ------------------- | ------- | ------------------------------------- | ------------------------ | -------------------------------------- |
| `action="..."`      | 送出目標    | `<form>`                              | 指定表單資料送到哪個網址             | `<form action="/search">`              |
| `method="get"`      | GET 送出  | `<form>`                              | 資料放在網址後面送出               | `<form method="get">`                  |
| `method="post"`     | POST 送出 | `<form>`                              | 資料放在請求內容中送出              | `<form method="post">`                 |
| `for="..."`         | 對應欄位    | `<label>`                             | 對應到 input 的 `id`         | `<label for="deviceName">設備名稱</label>` |
| `name="..."`        | 欄位名稱    | `<input>` / `<select>` / `<textarea>` | 表單送出時的資料名稱               | `<input name="deviceName">`            |
| `type="text"`       | 文字輸入    | `<input>`                             | 一般文字輸入框                  | `<input type="text">`                  |
| `type="password"`   | 密碼輸入    | `<input>`                             | 密碼輸入框                    | `<input type="password">`              |
| `type="number"`     | 數字輸入    | `<input>`                             | 數字輸入框                    | `<input type="number">`                |
| `type="checkbox"`   | 勾選框     | `<input>`                             | 多選勾選                     | `<input type="checkbox">`              |
| `type="radio"`      | 單選按鈕    | `<input>`                             | 多個選項只能選一個                | `<input type="radio" name="mode">`     |
| `type="submit"`     | 送出表單    | `<button>` / `<input>`                | 觸發 form 送出               | `<button type="submit">查詢</button>`    |
| `type="button"`     | 普通按鈕    | `<button>`                            | 不會自動送出表單，通常搭配 JavaScript | `<button type="button">測試</button>`    |
| `value="..."`       | 欄位值     | `<input>` / `<option>` / `<button>`   | 設定欄位或選項的值                | `<input value="PLC01">`                |
| `placeholder="..."` | 輸入提示    | `<input>` / `<textarea>`              | 欄位內提示文字                  | `<input placeholder="請輸入設備名稱">`        |
| `required`          | 必填      | `<input>` / `<select>` / `<textarea>` | 欄位不可空白                   | `<input required>`                     |
| `readonly`          | 唯讀      | `<input>` / `<textarea>`              | 可顯示但不可修改，會送出資料           | `<input readonly>`                     |
| `disabled`          | 停用      | `<input>` / `<button>` / `<select>`   | 不可使用，通常不會送出資料            | `<button disabled>查詢</button>`         |
| `checked`           | 預設勾選    | `<input>`                             | checkbox / radio 預設選取    | `<input type="checkbox" checked>`      |
| `selected`          | 預設選項    | `<option>`                            | 下拉選單預設選取                 | `<option selected>PLC01</option>`      |
| `multiple`          | 可多選     | `<select>` / `<input type="file">`    | 允許選多個項目或檔案               | `<select multiple>`                    |
| `min="..."`         | 最小值     | `<input>`                             | 限制最小數值或日期                | `<input type="number" min="0">`        |
| `max="..."`         | 最大值     | `<input>`                             | 限制最大數值或日期                | `<input type="number" max="100">`      |
| `maxlength="..."`   | 最大字數    | `<input>` / `<textarea>`              | 限制輸入字數                   | `<input maxlength="20">`               |

---

## 1-3. 連結與圖片常用屬性

| 屬性                | 中文理解  | 常用於                  | 主要用途       | 範例                                                     |
| ----------------- | ----- | -------------------- | ---------- | ------------------------------------------------------ |
| `href="..."`      | 連結目標  | `<a>`                | 指定連結要前往的位置 | `<a href="/device">設備頁</a>`                            |
| `target="_blank"` | 新分頁開啟 | `<a>`                | 點擊後用新分頁開啟  | `<a href="https://example.com" target="_blank">網站</a>` |
| `src="..."`       | 資源來源  | `<img>` / `<script>` | 指定圖片或檔案來源  | `<img src="plc.png">`                                  |
| `alt="..."`       | 替代文字  | `<img>`              | 圖片無法顯示時的文字 | `<img src="plc.png" alt="PLC 圖片">`                     |
| `width="..."`     | 寬度    | `<img>`              | 指定圖片寬度     | `<img src="plc.png" width="300">`                      |
| `height="..."`    | 高度    | `<img>`              | 指定圖片高度     | `<img src="plc.png" height="200">`                     |
| `loading="lazy"`  | 延遲載入  | `<img>`              | 圖片靠近畫面時才載入 | `<img src="machine.png" loading="lazy">`               |

---

# 2. HTML 常用標籤表格

## 2-1. 語意化標籤 Semantic Tags

| 類別   | 標籤              | 中文理解      | 主要用途             | 範例                                         |
| ---- | --------------- | --------- | ---------------- | ------------------------------------------ |
| 頁面結構 | `<header>`      | 頁首 / 區塊開頭 | 放標題、Logo、開頭資訊    | `<header>網站標題</header>`                    |
| 頁面結構 | `<nav>`         | 導覽列       | 放選單、連結           | `<nav><a href="/home">首頁</a></nav>`        |
| 頁面結構 | `<main>`        | 主要內容      | 放網頁主要內容          | `<main>主要內容</main>`                        |
| 頁面結構 | `<section>`     | 區段        | 有主題的一塊內容         | `<section><h2>設備清單</h2></section>`         |
| 頁面結構 | `<article>`     | 文章 / 獨立內容 | 可獨立存在的內容         | `<article>公告內容</article>`                  |
| 頁面結構 | `<aside>`       | 側邊內容      | 側欄、補充資訊          | `<aside>相關連結</aside>`                      |
| 頁面結構 | `<footer>`      | 頁尾 / 區塊結尾 | 放版權、結尾資訊         | `<footer>Copyright</footer>`               |
| 文字內容 | `<h1>` ~ `<h6>` | 標題        | 建立標題階層           | `<h1>設備監控系統</h1>`                          |
| 文字內容 | `<p>`           | 段落        | 顯示一段文字           | `<p>目前設備正常。</p>`                           |
| 文字內容 | `<strong>`      | 重要文字      | 表示重要內容           | `<strong>警告</strong>`                      |
| 文字內容 | `<em>`          | 強調語氣      | 表示語氣強調           | `<em>請注意</em>`                             |
| 文字內容 | `<hr>`          | 分隔線       | 分隔不同主題           | `<hr>`                                     |
| 連結圖片 | `<a>`           | 連結        | 建立超連結            | `<a href="/device">設備頁</a>`                |
| 連結圖片 | `<img>`         | 圖片        | 顯示圖片             | `<img src="plc.png" alt="PLC">`            |
| 連結圖片 | `<figure>`      | 圖文區塊      | 包住圖片、圖表、程式碼等獨立內容 | `<figure><img src="machine.png"></figure>` |
| 連結圖片 | `<figcaption>`  | 圖片說明      | 說明 figure 內容     | `<figcaption>圖 1：機台</figcaption>`          |
| 表單   | `<form>`        | 表單容器      | 包住一組可送出的資料       | `<form action="/search"></form>`           |
| 表單   | `<label>`       | 欄位標籤      | 說明輸入欄位           | `<label for="deviceName">設備名稱</label>`     |
| 表單   | `<input>`       | 輸入欄位      | 文字、密碼、數字、勾選等輸入   | `<input type="text">`                      |
| 表單   | `<button>`      | 按鈕        | 送出或執行動作          | `<button type="submit">查詢</button>`        |
| 表單   | `<textarea>`    | 多行輸入      | 輸入多行文字           | `<textarea></textarea>`                    |
| 表單   | `<select>`      | 下拉選單      | 建立下拉選擇           | `<select></select>`                        |
| 表單   | `<option>`      | 選項        | 下拉選單中的選項         | `<option>PLC01</option>`                   |
| 表格   | `<table>`       | 表格        | 建立表格資料           | `<table></table>`                          |
| 表格   | `<thead>`       | 表格標題區     | 表格上方標題列區域        | `<thead></thead>`                          |
| 表格   | `<tbody>`       | 表格內容區     | 表格主要資料區          | `<tbody></tbody>`                          |
| 表格   | `<tr>`          | 表格列       | 一列資料             | `<tr></tr>`                                |
| 表格   | `<th>`          | 標題儲存格     | 表格欄位標題           | `<th>設備名稱</th>`                            |
| 表格   | `<td>`          | 資料儲存格     | 表格中的一格資料         | `<td>PLC01</td>`                           |
| 清單   | `<ul>`          | 無序清單      | 沒有順序的清單          | `<ul><li>PLC01</li></ul>`                  |
| 清單   | `<ol>`          | 有序清單      | 有順序的清單           | `<ol><li>第一步</li></ol>`                    |
| 清單   | `<li>`          | 清單項目      | 清單中的一項           | `<li>PLC01</li>`                           |

---

## 2-2. 普通標籤 / 非語意化標籤 Non-semantic Tags

| 類別   | 標籤       | 中文理解 | 主要用途                  | 範例                                |
| ---- | -------- | ---- | --------------------- | --------------------------------- |
| 容器   | `<div>`  | 區塊容器 | 最常用的萬用區塊，常用於排版、包區塊    | `<div class="card">內容</div>`      |
| 容器   | `<span>` | 行內容器 | 包住小範圍文字，常用於套樣式或 JS 控制 | `<span class="status">運轉中</span>` |
| 文字控制 | `<br>`   | 換行   | 強制換行，沒有內容語意           | `第一行<br>第二行`                      |

---

## 2-3. 文件基礎標籤 Document Tags

| 類別    | 標籤                | 中文理解          | 主要用途               | 範例                                         |
| ----- | ----------------- | ------------- | ------------------ | ------------------------------------------ |
| 文件宣告  | `<!DOCTYPE html>` | HTML5 文件宣告    | 告訴瀏覽器使用 HTML5 解析   | `<!DOCTYPE html>`                          |
| 文件根元素 | `<html>`          | HTML 根元素      | 包住整份 HTML 文件       | `<html lang="zh-Hant"></html>`             |
| 文件資訊  | `<head>`          | 文件資訊區         | 放設定、標題、CSS、meta 資訊 | `<head></head>`                            |
| 文件標題  | `<title>`         | 網頁標題          | 顯示在瀏覽器分頁上的標題       | `<title>設備監控</title>`                      |
| 編碼資訊  | `<meta>`          | 網頁資訊          | 設定編碼、描述、響應式畫面等     | `<meta charset="UTF-8">`                   |
| 顯示內容  | `<body>`          | 網頁畫面內容        | 放使用者看得到的內容         | `<body></body>`                            |
| 外部資源  | `<link>`          | 連接外部資源        | 載入 CSS、icon 等      | `<link rel="stylesheet" href="style.css">` |
| 程式腳本  | `<script>`        | JavaScript 腳本 | 撰寫或載入 JavaScript   | `<script src="app.js"></script>`           |

---

## 2-4. 表單標籤 Form Tags

| 類別   | 標籤           | 中文理解  | 主要用途            | 範例                                            |
| ---- | ------------ | ----- | --------------- | --------------------------------------------- |
| 表單容器 | `<form>`     | 表單    | 包住一組可送出的輸入資料    | `<form action="/search" method="get"></form>` |
| 欄位說明 | `<label>`    | 標籤文字  | 說明輸入欄位          | `<label for="deviceName">設備名稱</label>`        |
| 單行輸入 | `<input>`    | 輸入框   | 文字、密碼、數字、勾選等    | `<input type="text" name="deviceName">`       |
| 多行輸入 | `<textarea>` | 多行文字框 | 輸入較長文字          | `<textarea name="memo"></textarea>`           |
| 按鈕   | `<button>`   | 按鈕    | 送出或觸發事件         | `<button type="submit">送出</button>`           |
| 下拉選單 | `<select>`   | 下拉選單  | 建立選項清單          | `<select name="device"></select>`             |
| 選項   | `<option>`   | 選項    | 下拉選單內的項目        | `<option value="PLC01">PLC01</option>`        |
| 欄位群組 | `<fieldset>` | 表單群組  | 把相關欄位包成一組       | `<fieldset></fieldset>`                       |
| 群組標題 | `<legend>`   | 群組標題  | 說明 fieldset 的標題 | `<legend>設備資料</legend>`                       |

---

## 2-5. 表格標籤 Table Tags

| 類別   | 標籤          | 中文理解  | 主要用途     | 範例                         |
| ---- | ----------- | ----- | -------- | -------------------------- |
| 表格容器 | `<table>`   | 表格    | 建立表格     | `<table></table>`          |
| 表格標題 | `<caption>` | 表格標題  | 說明整個表格   | `<caption>設備狀態表</caption>` |
| 表頭區  | `<thead>`   | 表格標題區 | 放欄位標題列   | `<thead></thead>`          |
| 內容區  | `<tbody>`   | 表格內容區 | 放主要資料列   | `<tbody></tbody>`          |
| 表尾區  | `<tfoot>`   | 表格結尾區 | 放合計或補充資訊 | `<tfoot></tfoot>`          |
| 表格列  | `<tr>`      | 一列    | 建立一列資料   | `<tr></tr>`                |
| 標題格  | `<th>`      | 標題儲存格 | 欄位標題     | `<th>設備名稱</th>`            |
| 資料格  | `<td>`      | 資料儲存格 | 一格資料     | `<td>PLC01</td>`           |

---

## 2-6. 清單標籤 List Tags

| 類別   | 標籤     | 中文理解 | 主要用途     | 範例                        |
| ---- | ------ | ---- | -------- | ------------------------- |
| 無序清單 | `<ul>` | 項目清單 | 沒有順序的清單  | `<ul><li>PLC01</li></ul>` |
| 有序清單 | `<ol>` | 編號清單 | 有順序的清單   | `<ol><li>第一步</li></ol>`   |
| 清單項目 | `<li>` | 清單項目 | 清單中的一個項目 | `<li>PLC01</li>`          |
| 定義清單 | `<dl>` | 定義清單 | 名詞與說明的清單 | `<dl></dl>`               |
| 定義名稱 | `<dt>` | 名詞   | 定義清單中的名稱 | `<dt>PLC</dt>`            |
| 定義說明 | `<dd>` | 說明   | 定義清單中的說明 | `<dd>可程式控制器</dd>`         |

---

## 2-7. 連結與媒體標籤 Media Tags

| 類別   | 標籤             | 中文理解 | 主要用途                 | 範例                                         |
| ---- | -------------- | ---- | -------------------- | ------------------------------------------ |
| 連結   | `<a>`          | 超連結  | 連到其他頁面或位置            | `<a href="/home">首頁</a>`                   |
| 圖片   | `<img>`        | 圖片   | 顯示圖片                 | `<img src="plc.png" alt="PLC">`            |
| 圖文區塊 | `<figure>`     | 圖文內容 | 包住圖片、圖表、程式碼          | `<figure></figure>`                        |
| 圖文說明 | `<figcaption>` | 圖片說明 | 說明 figure 內容         | `<figcaption>設備照片</figcaption>`            |
| 影片   | `<video>`      | 影片   | 播放影片                 | `<video controls src="demo.mp4"></video>`  |
| 音訊   | `<audio>`      | 音訊   | 播放聲音                 | `<audio controls src="alarm.mp3"></audio>` |
| 來源   | `<source>`     | 媒體來源 | 提供 video / audio 的來源 | `<source src="demo.mp4" type="video/mp4">` |

---

## 2-8. 最常用優先記憶表

| 優先順序 | 標籤 / 屬性                              | 中文理解   | 先記原因                 |
| ---- | ------------------------------------ | ------ | -------------------- |
| 1    | `<div>`                              | 區塊容器   | 最常用的排版容器             |
| 2    | `<span>`                             | 行內容器   | 常用於小範圍文字控制           |
| 3    | `<h1>` ~ `<h6>`                      | 標題     | 建立內容階層               |
| 4    | `<p>`                                | 段落     | 顯示文字內容               |
| 5    | `<a href="">`                        | 連結     | 網頁跳轉必備               |
| 6    | `<img src="" alt="">`                | 圖片     | 顯示圖片必備               |
| 7    | `<form>`                             | 表單     | 表單送資料必備              |
| 8    | `<label for="">`                     | 欄位標籤   | 對應 input             |
| 9    | `<input id="" name="" type="">`      | 輸入欄位   | 使用者輸入資料              |
| 10   | `<button type="">`                   | 按鈕     | 送出或執行動作              |
| 11   | `<table>` / `<tr>` / `<th>` / `<td>` | 表格     | 顯示表格資料               |
| 12   | `id`                                 | 唯一名稱   | JS / CSS 精準選取        |
| 13   | `class`                              | 分類名稱   | CSS 共用樣式             |
| 14   | `name`                               | 表單欄位名稱 | 表單送資料時使用             |
| 15   | `type`                               | 類型     | 決定 input / button 行為 |
