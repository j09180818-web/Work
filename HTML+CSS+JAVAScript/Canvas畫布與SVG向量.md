# Canvas 與 SVG 完整筆記

## 1. 核心概念

Canvas 與 SVG 都可以在網頁中呈現圖形，但本質完全不同。

```text
Canvas = 像素畫布 / 點陣圖概念
SVG    = 向量圖形 / 圖形物件集合
```

簡單理解：

```text
Canvas：
像是一塊像素畫布。
JavaScript 把圖片、線條、矩形、影片幀畫上去。
畫完後只剩像素結果。

SVG：
像是在圖面上放入一個個圖形物件。
每個圖形都還是 DOM 元素，可以被 CSS / JavaScript 控制。
```

---

## 2. Canvas 是什麼？

`<canvas>` 是 HTML 提供的一塊「像素繪圖區」。

它本身只是空白畫布，真正內容要靠 JavaScript 畫上去。

```html
<canvas id="myCanvas" width="800" height="600"></canvas>
```

```javascript
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");

ctx.fillStyle = "red";
ctx.fillRect(100, 100, 200, 100);
```

意思是：

```text
建立一個 800 x 600 的 Canvas 畫布
JavaScript 取得 Canvas 的 2D 繪圖工具 ctx
在 x=100, y=100 的位置畫一個紅色矩形
```

---

## 3. Canvas 的本質：像素 / 點陣圖

Canvas 可以理解成一張會被 JavaScript 改寫的圖片。

```text
Canvas 內部是像素資料。
例如：
寬 800、高 600
= 800 x 600 個像素
```

每個像素通常可以理解成：

```text
R 紅色
G 綠色
B 藍色
A 透明度
```

所以 Canvas 適合處理：

```text
照片
影片畫面
相機影像
像素濾鏡
波形
大量動畫
```

---

## 4. Canvas 畫完後只剩像素

例如你畫一個矩形：

```javascript
ctx.fillRect(100, 100, 200, 100);
```

畫完後，瀏覽器不會保留一個「矩形物件」。

它只知道：

```text
Canvas 這塊區域上有一些紅色像素
```

所以 Canvas 不是這樣：

```text
canvas
 └─ rect 物件
```

而是：

```text
canvas
 └─ 一整張像素畫面
```

---

## 5. Canvas 與 C# 的類比

Canvas 比較像 C# WinForms 的 Graphics 繪圖：

```csharp
Graphics g = e.Graphics;
g.DrawRectangle(pen, x, y, w, h);
g.DrawImage(image, x, y);
```

重點是：

```text
畫完是畫面結果
不是控制項
不是物件集合
更新畫面通常要重畫
事件要自己判斷座標
```

所以 Canvas 比較像：

```text
Graphics.DrawXXX()
```

不是像：

```text
Button
Panel
Label
PictureBox
```

---

## 6. Canvas 與圖片、影片的關係

Canvas 可以顯示圖片，也可以顯示影片畫面。

但它不是把圖片或影片「貼上去」，而是把圖片或影片目前畫面的像素「繪製」到 Canvas 上。

### 圖片

```javascript
ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
```

意思是：

```text
把 img 圖片的像素資料畫到 Canvas 上
```

### 影片

```javascript
ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
```

意思是：

```text
把 video 目前這一幀畫面畫到 Canvas 上
```

影片在 Canvas 中是逐幀繪製：

```text
video 第 1 幀 → 畫到 Canvas
video 第 2 幀 → 畫到 Canvas
video 第 3 幀 → 畫到 Canvas
快速連續重畫 → 看起來像影片
```

---

## 7. Canvas 尺寸觀念

Canvas 有兩種尺寸要分清楚。

### HTML width / height

```html
<canvas width="800" height="600"></canvas>
```

這是 Canvas 內部真正的繪圖解析度。

```text
內部繪圖範圍 = 800 x 600 像素
```

### CSS width / height

```css
canvas {
    width: 400px;
    height: 300px;
}
```

這是 Canvas 在網頁畫面上的顯示大小。

```text
畫面顯示大小 = 400 x 300
```

所以要記：

```text
HTML width / height = Canvas 內部繪圖像素大小
CSS width / height  = Canvas 外部顯示大小
```

如果比例一樣，只是縮放顯示：

```text
800 x 600 → 400 x 300
比例都是 4:3
不會變形
```

如果比例不同，畫面會被拉伸或壓扁：

```text
800 x 600 → 400 x 400
4:3 被拉成 1:1
畫面會變形
```

---

## 8. Canvas 點擊事件注意事項

Canvas 裡面的圖形不是 DOM 元素，所以不能直接對某個矩形或圓形綁事件。

錯誤理解：

```text
我畫了一個矩形，所以可以直接對矩形 click
```

實際上不行。

Canvas 只能對整個 `<canvas>` 綁事件，再自己計算滑鼠座標。

```javascript
canvas.addEventListener("click", function (event) {
    const rect = canvas.getBoundingClientRect();

    const mouseX = event.clientX - rect.left;
    const mouseY = event.clientY - rect.top;

    if (mouseX >= 100 && mouseX <= 300 &&
        mouseY >= 100 && mouseY <= 200) {
        console.log("點到矩形範圍");
    }
});
```

意思是：

```text
使用者點擊 Canvas
取得滑鼠在 Canvas 裡的位置
自己判斷這個位置有沒有落在某個圖形範圍內
```

這點對 C# 工程師很重要。

Canvas 裡面不像 WinForms 有 Button、Panel、Label 那種控制項物件。

Canvas 內部只是一整張像素畫面。

---

## 9. Canvas 適合使用的場合

Canvas 適合「像素型、即時重繪、大量變化」的畫面。

常見用途：

```text
1. 圖片處理
2. 影片逐幀顯示
3. 相機畫面顯示
4. 即時波形圖
5. 遊戲畫面
6. 大量動畫
7. 影像標記
8. 截圖輸出
9. 濾鏡處理
```

自動化產業中的例子：

```text
相機影像
↓
Canvas 顯示影像
↓
JavaScript 畫框線、標記異常位置
```

或：

```text
即時資料
↓
JavaScript 持續重繪
↓
Canvas 顯示波形圖
```

---

# 10. SVG 是什麼？

`SVG` 全名是：

```text
Scalable Vector Graphics
可縮放向量圖形
```

SVG 是用類似 HTML 的標籤來描述圖形。

例如：

```html
<svg width="400" height="200">
    <rect id="box" x="20" y="20" width="100" height="60" fill="red" />
    <circle id="light" cx="200" cy="50" r="30" fill="green" />
</svg>
```

這裡有兩個圖形元素：

```text
rect   = 矩形物件
circle = 圓形物件
```

---

## 11. SVG 的本質：向量圖形

SVG 是向量圖，不是像素圖。

向量圖的意思是：

```text
它不是記錄每個像素的顏色。
它是記錄圖形的數學描述。
```

例如一個圓形：

```html
<circle cx="100" cy="100" r="50" />
```

它記錄的是：

```text
圓心在 100,100
半徑是 50
```

不是記錄：

```text
第 1 個像素是什麼顏色
第 2 個像素是什麼顏色
第 3 個像素是什麼顏色
```

所以 SVG 放大縮小時不容易失真。

---

## 12. 向量圖與像素圖差異

### Canvas / 圖片類型

```text
像素圖 / 點陣圖
由很多像素組成
放大太多可能會模糊或鋸齒
```

例如：

```text
照片
影片
相機畫面
Canvas 繪製結果
```

### SVG

```text
向量圖
由座標、線條、形狀、路徑描述
放大縮小仍然清楚
```

例如：

```text
icon
流程圖
設備圖
儀表板
時速表
狀態圖
```

---

## 13. SVG 畫完後仍然保留物件

SVG 裡面的每個圖形都是 DOM 元素。

所以可以用 JavaScript 單獨控制某個圖形。

```javascript
const box = document.getElementById("box");
box.setAttribute("fill", "blue");
```

意思是：

```text
找到 id="box" 的矩形
只把這個矩形改成藍色
```

這跟 Canvas 不同。

```text
Canvas：
畫完只剩像素。

SVG：
畫完仍然保留圖形物件。
```

SVG 的結構比較像：

```text
svg
 ├─ rect 矩形物件
 ├─ circle 圓形物件
 ├─ line 線條物件
 └─ text 文字物件
```

---

## 14. SVG 常見圖形

### 矩形 rect

```html
<rect x="20" y="20" width="100" height="60" fill="red" />
```

```text
x, y = 左上角位置
width, height = 寬高
fill = 填滿顏色
```

---

### 圓形 circle

```html
<circle cx="100" cy="80" r="30" fill="blue" />
```

```text
cx, cy = 圓心位置
r = 半徑
fill = 填滿顏色
```

---

### 線條 line

```html
<line x1="20" y1="20" x2="200" y2="100" stroke="black" stroke-width="3" />
```

```text
x1, y1 = 起點
x2, y2 = 終點
stroke = 線條顏色
stroke-width = 線寬
```

---

### 文字 text

```html
<text x="50" y="100" font-size="20" fill="black">Motor A</text>
```

```text
x, y = 文字位置
font-size = 字體大小
fill = 文字顏色
```

---

### 路徑 path

`path` 是 SVG 裡最強大的圖形。

它可以畫複雜形狀，例如：

```text
曲線
箭頭
不規則圖形
icon
複雜機構外型
```

範例：

```html
<path d="M 20 100 L 100 20 L 180 100 Z" fill="orange" />
```

簡單理解：

```text
M = Move，移動到某個點
L = Line，畫線到某個點
Z = Close，封閉圖形
```

這段代表：

```text
移動到 20,100
畫線到 100,20
畫線到 180,100
封閉成三角形
```

`path` 常用在 SVG icon、複雜圖示、機台外型圖。

---

## 15. SVG 的 viewBox

`viewBox` 是 SVG 非常重要的觀念。

範例：

```html
<svg width="300" height="200" viewBox="0 0 300 200">
    <circle cx="150" cy="100" r="50" fill="blue" />
</svg>
```

`viewBox="0 0 300 200"` 的意思是：

```text
SVG 內部座標系統：
x 從 0 到 300
y 從 0 到 200
```

而：

```html
width="300" height="200"
```

代表：

```text
實際顯示大小是 300 x 200
```

所以：

```text
viewBox = 內部座標世界
width / height = 外部顯示大小
```

這點有點像 Canvas 的內部尺寸與 CSS 顯示尺寸概念，但 SVG 是向量圖，所以縮放後通常不會失真。

---

## 16. SVG 可以被 CSS 控制

SVG 圖形可以用 CSS 控制外觀。

```html
<svg width="300" height="150">
    <rect class="machine" x="20" y="20" width="100" height="60" />
</svg>
```

```css
.machine {
    fill: green;
    stroke: black;
    stroke-width: 2;
}
```

意思是：

```text
用 CSS 控制 SVG 矩形的填色、邊框、線寬
```

常見 SVG 外觀屬性：

```text
fill         填滿顏色
stroke       線條顏色
stroke-width 線條粗細
opacity      透明度
```

---

## 17. SVG 群組 g

SVG 可以用 `<g>` 把多個圖形組成一組。

```html
<svg width="400" height="200">
    <g id="machineA">
        <rect x="20" y="20" width="120" height="60" fill="gray" />
        <circle cx="40" cy="50" r="10" fill="green" />
        <text x="60" y="55" font-size="16">A機台</text>
    </g>
</svg>
```

這裡：

```text
rect   = 機台外框
circle = 狀態燈
text   = 機台名稱
g      = 把這三個圖形組成 machineA 這個群組
```

JavaScript 可以控制整組：

```javascript
const machineA = document.getElementById("machineA");

machineA.addEventListener("click", function () {
    console.log("點到 A 機台");
});
```

---

## 18. SVG 與 JavaScript 的關係

SVG 的圖形可以被 JavaScript 直接抓取、修改、綁事件。

例如：

```html
<rect id="machineA" x="20" y="20" width="100" height="60" fill="green" />
```

```javascript
const machineA = document.getElementById("machineA");

machineA.addEventListener("click", function () {
    machineA.setAttribute("fill", "red");
});
```

意思是：

```text
點擊 machineA 這個矩形
把顏色從綠色改成紅色
```

因為 `rect` 是 DOM 元素，所以可以直接綁事件。

---

## 19. SVG 與後端資料的關係

SVG 本身不直接連後端。

通常流程是：

```text
後端 API 提供資料
↓
JavaScript 取得資料
↓
JavaScript 修改 SVG 圖形
↓
畫面更新
```

例如後端回傳：

```json
{
    "machineId": "machineA",
    "status": "alarm"
}
```

JavaScript 根據資料改 SVG：

```javascript
const data = {
    machineId: "machineA",
    status: "alarm"
};

const machine = document.getElementById(data.machineId);

if (data.status === "alarm") {
    machine.setAttribute("fill", "red");
} else {
    machine.setAttribute("fill", "green");
}
```

意思是：

```text
後端提供機台狀態
JavaScript 找到對應 SVG 圖形
依照狀態改顏色
```

---

## 20. SVG 適合使用的場合

SVG 適合「物件型、可控制、可互動、可縮放」的圖形。

常見用途：

```text
1. 設備狀態圖
2. 流程圖
3. 儀表板
4. 時速表 / 壓力表 / 溫度表
5. icon
6. 可點擊平面圖
7. 機台配置圖
8. 狀態燈
9. 需要縮放不失真的圖示
```

自動化產業中的例子：

```text
後端回傳機台狀態
↓
JavaScript 找到 SVG 裡的 machineA
↓
把顏色改成紅色或綠色
```

或：

```text
後端回傳速度值
↓
JavaScript 計算角度
↓
控制 SVG 指針旋轉
```

---

## 21. SVG 做時速表範例

```html
<svg width="300" height="200" viewBox="0 0 300 200">
    <!-- 外框 -->
    <circle cx="150" cy="150" r="100" fill="none" stroke="black" stroke-width="4" />

    <!-- 指針 -->
    <line id="needle"
          x1="150"
          y1="150"
          x2="150"
          y2="70"
          stroke="red"
          stroke-width="5" />

    <!-- 中心點 -->
    <circle cx="150" cy="150" r="6" fill="black" />
</svg>
```

JavaScript 控制指針：

```javascript
const needle = document.getElementById("needle");

needle.setAttribute("transform", "rotate(45 150 150)");
```

意思是：

```text
只旋轉 id="needle" 的指針
旋轉中心是 150,150
角度是 45 度
```

這就是 SVG 的優點：

```text
整張圖是 svg 容器
指針是 line 物件
JavaScript 可以只控制指針
不用重畫整張圖
```

---

# 22. Canvas 與 SVG 比較表

| 項目       | Canvas        | SVG                  |
| -------- | ------------- | -------------------- |
| 本質       | 像素畫布 / 點陣圖    | 向量圖形 / 圖形物件          |
| 畫完後      | 只剩像素          | 保留圖形元素               |
| 是否進 DOM  | 只有 canvas 本身  | 每個圖形都是 DOM           |
| 控制方式     | JavaScript 重繪 | CSS / JavaScript 改元素 |
| 點擊事件     | 要自己算座標        | 可直接綁在圖形上             |
| 放大縮小     | 可能失真          | 不容易失真                |
| 適合影像處理   | 適合            | 不適合                  |
| 適合影片逐幀   | 適合            | 不適合                  |
| 適合儀表圖    | 可以，但較麻煩       | 適合                   |
| 適合大量動畫   | 適合            | 不一定適合                |
| 適合單一物件控制 | 較麻煩           | 很適合                  |
| 適合 icon  | 較不適合          | 適合                   |
| 適合流程圖    | 較不適合          | 適合                   |
| 適合相機畫面   | 適合            | 不適合                  |

---

## 23. C# 工程師理解方式

### Canvas 類似 C# 的 Graphics 繪圖

```csharp
Graphics g = e.Graphics;
g.DrawImage(image, x, y);
g.DrawRectangle(pen, x, y, w, h);
```

重點：

```text
畫完是畫面結果
不是控制項
事件要自己判斷座標
更新畫面通常要重畫
```

Canvas 比較像：

```text
Graphics.DrawXXX()
```

---

### SVG 類似畫面上有很多可控制圖形元件

SVG 裡面有：

```text
rect   矩形
circle 圓形
line   線條
text   文字
path   路徑
g      群組
```

每個都可以被 JavaScript 找到並控制。

這比較像 WinForms 裡有多個控制項：

```text
Panel
Label
Button
PictureBox
```

只是 SVG 的物件是圖形元素。

---

## 24. 選用規則

### 選 Canvas 的情況

```text
畫面一直變
資料量大
需要處理圖片、影片、像素
需要逐幀繪製
不需要單獨控制每個圖形
```

適合例子：

```text
相機影像
監控畫面
影片逐幀處理
即時波形
遊戲畫面
圖片濾鏡
影像標記
大量動畫
```

---

### 選 SVG 的情況

```text
圖形結構固定
只需要改某些物件狀態
需要點擊某個圖形
需要縮放不失真
需要單獨控制圖形
```

適合例子：

```text
時速表
壓力表
設備狀態圖
流程圖
機台配置圖
警示燈
儀表板 icon
可互動平面圖
```

---

## 25. 最終總結

```text
Canvas = 畫面型
適合像素、影像、動畫、即時重繪。

SVG = 物件型
適合向量圖、圖形元件、儀表、流程圖、設備狀態圖。
```

最重要記法：

```text
Canvas：畫完只剩像素。
SVG：畫完仍然是物件。
```

以 C# 工程師角度記：

```text
Canvas 比較像 Graphics.DrawXXX()
SVG 比較像畫面上有一堆可控制的圖形元件
```

實務選擇：

```text
相機影像 / 影片 / 即時波形 / 像素處理 → Canvas

時速表 / 設備圖 / 流程圖 / 狀態燈 / icon → SVG
```
