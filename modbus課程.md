今日課程內容整理:
RS232接腳235，RS232一定需要地線
| Pin | 名稱             | 功能   |
| --- | -------------- | ---- |
| 2   | RXD            | 接收資料 |
| 3   | TXD            | 發送資料 |
| 5   | GND            | 訊號地  |
| 其他  | RTS、CTS、DTR... | 流量控制 |
這只是 DTE標準定義（例如電腦）。
所以通常電腦是按照接腳規則RX在Pin2但是另一端設備Pin2可能是TX?這要看技術手冊我們只知道23會是TXRX但不知道他接頭到底是什麼規格如果兩端規格相同線材就要自己交叉對

RS485接腳125，RS485只需要D+D-
| Pin | 功能  |
| --- | --- |
| 1   | D-  |
| 2   | D+  |
| 5   | GND |
常見的接法是這樣
也有
| Pin | 功能  |
| --- | --- |
| 3   | D+  |
| 8   | D-  |
| 5   | GND |
甚至有USB轉485
| Pin | 功能 |
| --- | -- |
| 4   | D+ |
| 9   | D- |

因此485主要要看技術手冊
如果是 USB轉RS485接PLC
USB轉485
D+ → PLC D+
D- → PLC D-

很多設備的 COM Port 就是這種 DE-9（大家都叫 DB9）接頭。

而其中最常用的三個腳位就是：2RX 3TX 5GND

# 註DB9是因為接頭看起來像是英文字D 統稱D-Subminiature Connector（D-Sub 接頭）
早期DB是DB25後來改成9針序列埠

序列式傳輸:資料是按照一個bit一個bit依照順序傳送例如字母A=01000001一次只會走一個bit
常見序列式通訊
RS232
RS485
RS422
UART

為什麼 RTU 叫做序列式？
因為 Modbus RTU 本身不是實體線路。
它是一種：
「資料格式（Protocol）」
但是它規定：
必須跑在 Serial Port 上
通常使用 RS232 或 RS485

TCP傳輸又是什麼?
TCP是網路的通訊協定作用於Ethernet
Ethernet Frame
    ↓
IP
    ↓
TCP
    ↓
Modbus TCP
其實網路線最後硬體上還是算序列式

但工業通訊講的：

Serial Communication
通常專指：
RS232
RS485
COM Port

Ethernet Communication
通常專指：
TCP/IP
網路線
RJ45

另外說到序列式傳輸不外乎還有並列傳輸（Parallel Transmission）
D0 = 0
D1 = 1
D2 = 0
D3 = 0
D4 = 1
D5 = 1
D6 = 0
D7 = 1
八個腳位同時一時間傳送資料這就是並列式傳輸
早期的LPT Port（平行埠）印表機
IDE 排線也是並列 老的硬碟PATA（Parallel ATA）
後期因為同時傳輸但一定會有些誤差當速率越快就越容易出錯這叫做 Skew訊號偏移

因此後來改成少量線高速傳輸
例如：
USB
Ethernet
SATA
PCIe

全部都改成序列。

| 技術         | 類型   |
| ---------- | ---- |
| RS232      | 序列   |
| RS485      | 序列   |
| Modbus RTU | 序列   |
| Ethernet   | 序列   |
| Modbus TCP | 序列   |
| USB        | 序列   |
| SATA       | 序列   |
| PCIe       | 序列   |
| LPT印表機     | 並列   |
| IDE(PATA)  | 並列   |
| PLC數位IO    | 並列概念 |

# 遇到系統設備時候問什麼通訊? 如果說MOdbus 我則要繼續問 TCP RTU?
如果對方說RTU我則要說是485嗎?不是232吧?需要明確理解，通訊格式與通訊方式，通訊方式影響到配線

當你拿到一台設備時，可以依序問：
第一：支援什麼通訊協定？

例如：
Modbus RTU
Modbus TCP
Ethernet/IP
Profinet
CC-Link
MC Protocol
OPC UA
詢問：
請問這台設備支援什麼通訊協定？

第二：使用什麼實體介面？

如果對方回答：
Modbus RTU
不要立刻假設是 RS485。
繼續問：
請問 Modbus RTU 是走 RS232 還是 RS485？
因為 RTU 其實可以跑在：
協定	可能介面
Modbus RTU	RS232
Modbus RTU	RS485
Modbus RTU	RS422
只是現場 90% 以上都是 RS485。
例如：
變頻器
電表
溫控器
IO模組
幾乎都：
Modbus RTU over RS485

情境1

廠商：支援 Modbus RTU

你：請問是 RS485 還是 RS232？

廠商：RS485

你立刻知道：
D+
D-
GND(有些設備需要)

配線方式：
D+ ↔ D+
D- ↔ D-
GND ↔ GND

情境2
廠商：支援 Modbus TCP

你就直接問：IP位址如何設定？
因為你已經知道：
RJ45
網路線
TCP/IP
需要明確理解IP如何設定，很多設備可能有按鈕介面可以設定IP但如果沒有通常就廠商會供應相對應的軟體去搜尋，包含硬體驅動driver
在自動化、PLC、電腦領域：
Driver = 驅動程式
Device Driver = 裝置驅動程式

網路接頭TCP燈號:還沒查
ICMP設備PING沒回覆怎麼判斷?現在電腦可以關閉要注意
Ping的速度時間判斷怎麼看
