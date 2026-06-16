RTU在傳遞資料的時候
1. 如果多個ID並接的時候假設呼叫ID1則其餘ID收到此資料會直接無視，此時如果站1收到的資料異常也會回傳異常訊息
2. RTU常見的三個時間參數
很多PLC或HMI都有：
# 1.Response Timeout
等待回應時間
我發送後
最多等1秒
沒收到就失敗
# 2.Retry Count 
重送次數
# 3.Poll Delay / Scan Interval
兩次命令間隔

3. 超時後重送，剛好回覆回來怎麼辦？
這就是RTU最重要的時序問題。
例子
t=0
PLC → 01
PLC設定Timeout = 500ms
結果設備回應很慢
t=520ms
設備才回
此時PLC認定失敗
於是PLC再次發送請求
這時候在總線上
設備第一次回覆
+
PLC第二次請求


## 這叫做 Collision資料衝撞
RTU最重要的時序
PLC                Device

送出封包
 │----------------->│

 │                  │解析

 │<-----------------│回覆

等待結束
PLC                Device

送出
 │----------------->│

 │                  │

 │<----Timeout------│

重送
 │----------------->│

手冊常寫 3.5 Character Time
Frame 1      Frame 2      Frame 3
███████      ███████      ███████
       ↑            ↑
    3.5 char     3.5 char
RTU有個特殊規定：
總線必須安靜。

TUR 本身就是靠時序規範避免衝撞
真正常發生資料衝撞原因
1.超過一個MASTER掛在相同路徑上
2.相同ID



以下暫時紀錄尚未理解透徹


# Modbus RTU 與 RS485 筆記

## 一、Modbus RTU 是什麼？

Modbus RTU 是一種資料封包格式（通訊協定）。

常見封包：

01 03 00 00 00 02 CRC

意思：

* 01 = 站號(Slave ID)
* 03 = 功能碼(Function Code)
* 0000 = 起始位址
* 0002 = 讀取長度
* CRC = 錯誤檢查碼

---

## 二、RTU 與 RS485 的關係

Modbus RTU = 資料格式

RS485 = 實體傳輸線路

常見組合：

Modbus RTU + RS485

現場架構：

PLC (Master)
│
├── Slave 1
├── Slave 2
└── Slave 3

所有設備都收到封包。

但只有站號符合的設備會回覆。

例如：

PLC 發送：

01 03 00 00 00 02 CRC

Slave1：
發現站號=01
→ 回覆

Slave2：
發現站號≠01
→ 忽略

Slave3：
發現站號≠01
→ 忽略

---

## 三、RTU 的主從架構

Master：

* 主動發送命令
* 主動詢問設備

Slave：

* 不能主動說話
* 只能被問後回答

規則：

一問一答

Master 問
↓
Slave 回
↓
Master 再問下一台

---

## 四、RTU 為什麼不容易資料衝撞？

因為：

* 一個 Master
* 多個 Slave
* 一次只問一台

正常流程：

PLC → Slave1
等待
Slave1 → PLC

PLC → Slave2
等待
Slave2 → PLC

---

## 五、容易發生衝撞的情況

1. 站號重複

設備A = 01
設備B = 01

PLC 問 01

設備A 回
設備B 也回

發生衝撞

---

2. 多個 Master

電腦A
PLC B

同時發送資料

發生衝撞

---

3. Timeout 設定不合理

PLC：
等待500ms

設備：
520ms才回覆

PLC已重送

第一次回覆與第二次命令可能重疊

---

## 六、RTU 常見時間參數

### 1. Response Timeout

等待回應時間

例如：

1000ms

超過時間沒收到回覆

→ Timeout

---

### 2. Retry Count

重送次數

例如：

3次

超時後自動重送

---

### 3. Poll Interval / Scan Interval

掃描間隔

例如：

50ms

問完一台後等待50ms再問下一台

---

## 七、Frame 與 Character

### Frame

一整包 Modbus 資料

例如：

01 03 00 00 00 02 CRC

這叫一個 Frame

---

### Character

Frame 裡面的每一個 Byte

例如：

01
03
00
00

每一個都是一個 Character

---

## 八、UART 傳輸方式

Byte 不會直接送出去。

每個 Byte 都會被包裝：

Start + Data + Parity + Stop

例如：

01

實際變成：

Start
00000001
Stop

再送到 RS485 線路上。

---

## 九、3.5 Character Time

RTU 利用靜默時間判斷封包結束。

概念：

Frame1

安靜超過3.5 Character Time

Frame2

設備看到：

超過3.5 Character 沒資料

就知道：

上一包資料結束了

開始解析。

---

## 十、重要觀念

通訊分三層理解：

Modbus RTU
↓
UART (Start/Data/Parity/Stop)
↓
RS485 (D+、D-)

Modbus RTU 負責資料格式

UART 負責字元封裝

RS485 負責電氣傳輸
###################################################
是 UART 硬體本來就一次只能傳送一個 Character(Byte)
所以送資料來說
Modbus命令
01 03 00 00 00 02 CRC

↓拆開

01
03
00
00
00
02
CRC_H
CRC_L
然後UART會
送01
↓
送03
↓
送00
↓
送00
↓
送00
↓
送02
↓
送CRC
所以在RS485上是
1個Byte
接著
1個Byte
接著
1個Byte
...
一個UART是
8 Data Bit
No Parity
1 Stop Bit
Start + Data + Stop
UART Character

Start
 ↓
Data
 ↓
Parity(可有可無)
 ↓
Stop

Idle    Start      Data         Stop   Idle
 1 1 1    0    00000001          1    1 1 1
          ↑
       Start Bit
    
平常都維持在1

突然變0
↓
代表開始傳送

資料送完
↓
回到1
↓
代表結束


序列傳輸格式（Serial Communication）有UART但不代表所有序列都是UART
序列通訊
├─ UART
├─ SPI
├─ I2C
├─ CAN
├─ LIN
└─ 其他...

所以以後看到
RS485
9600
8N1
電氣介面
↓
RS485

傳輸格式
↓
UART

UART參數
↓
9600 8N1
而所謂Modbus RTU是在這之上的資料格式