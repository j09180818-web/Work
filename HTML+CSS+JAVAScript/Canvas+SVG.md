# Canvas = 用 JavaScript 畫圖的畫布
是螢幕上的像素繪圖區，可以呈現接近照片等級的顏色與細節。
Canvas 可以理解成一個「像素圖區」。
| 來源        | 說明                              |
| --------- | ------------------------------- |
| 瀏覽器       | Chrome、Edge、Firefox、Safari 限制不同 |
| 顯示卡 / GPU | 太大的 Canvas 會吃顯示記憶體              |
| 裝置效能      | 手機比桌機更容易受限制                     |
| 記憶體       | Canvas 越大越耗 RAM / VRAM          |
Canvas 實際解析度 = 是受到HTML的顯示設定<canvas width height>
不是 CSS 的 width height
HTML width / height
= Canvas 內部真正可繪製的像素範圍
CSS width / height
= Canvas 在網頁畫面上實際顯示的大小
Canvas 內部像素區
800 x 600

┌──────────────────────────────┐
│                              │
│      真正畫圖的解析度          │
│                              │
└──────────────────────────────┘
              ↓ CSS 縮小顯示

網頁上看到的大小
400 x 300

┌───────────────┐
│  顯示出來的畫面 │
└───────────────┘
後台 / 後端 Backend
= 提供資料、圖片、影片、影像串流、API
JavaScript
= 向後端拿資料，控制 Canvas 要畫什麼
Canvas
= 前端畫面上的像素繪圖區，只負責顯示/繪製
1. Canvas 是「像素層」，不是 DOM 元件層
2. 事件要自己算座標
3. Canvas 畫面要靠「重繪」維持
4. Canvas 很適合影像處理
5. Canvas 可以輸出成圖片
但不是適合
Canvas 不適合大量一般 UI 元件。
圖像、動畫、遊戲、影像、波形、地圖、圖表、即時畫面

# SVG = 用 HTML 標籤組成的向量圖形
Scalable Vector Graphics
可縮放向量圖形
SVG = 圖形元件集合
每個圖形都是 DOM 元素
HTML
<svg width="300" height="150">
    <rect id="box" x="20" y="20" width="100" height="50" fill="red" />
</svg>
這個 <rect> 是真的元素，可以被 JavaScript 抓到：
JAVASCRIPT
const box = document.getElementById("box");
box.setAttribute("fill", "blue");
SVG 基本框架
<svg width="400" height="300">
    <!-- 圖形放這裡 -->
</svg>
<svg> = 一個向量圖形世界
裡面的 rect、circle、line = 圖形元件

適合放:
設備狀態圖
流程圖
平面配置圖
icon
儀表板圖示
可點擊圖形
簡單動畫

1. 每個圖形是 DOM 元素
2. 可以用 CSS 控制外觀
3. 可以用 JavaScript 綁事件
4. 放大縮小不容易失真
5. 很適合設備圖、流程圖、icon、互動式圖形
Canvas = 畫面型，適合大量即時繪製。
SVG = 物件型，適合可控制的圖形元件。
| 項目       | Canvas | SVG       |
| -------- | ------ | --------- |
| 本質       | 像素畫布   | 圖形物件      |
| 畫完後      | 只剩像素   | 保留物件      |
| 控制方式     | 重新繪製   | 修改指定元素    |
| 點擊事件     | 要自己算座標 | 可直接綁定圖形   |
| 放大縮小     | 可能失真   | 不容易失真     |
| 適合影像處理   | 適合     | 不適合       |
| 適合儀表圖    | 可做但較麻煩 | 適合        |
| 適合大量即時畫面 | 適合     | 不適合大量高頻更新 |
| 適合單一物件控制 | 較麻煩    | 很適合       |

