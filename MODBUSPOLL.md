# MODbudPull主要功能當作MODBUS功能測試可以對點寫入無論是連續或是單點的讀寫

Modbuspoll軟體
此軟體主要可以做為測試Modbus的功能為主不是監控
首先設備分為

每個頁
還有整個Wordspace
可以建立設定多個頁儲存後再將多個頁存成一個Wordspace方便後續使用


主要設定Connection Setup
Serial Port
MODBUSTCP/IP
MODBUSUCP/IP
Modbus RTU Over TCP/IP
Modbus RTU Over OCP/IP

我使用了VxCom所以使用Serial Port
今天總共使用了兩種方式
TCP與Port

TCP參數中
除了目標IP以外
還須設定Server Port:在這通常是502
CommectTimout:3000 ms 

Port參數則有 速率 跟 n81等通訊參數設定

共同參數有
Responsp TimeOut: N ms (此參數設定的是指命令送出後等待回覆TimeOut的時間)
DelayBetweenPolls:N ms (此參數設定是指第一次通訊失敗後執行第二次通訊的間隔時間，有些設備還會有二次通訊的次數重複又失敗最多幾次這樣，有時候設備還會將此二次時間名稱改為RetryTime)

時間參數的概念如下
Master-------------->Slave
|
|
|
經過ResponspTimeOut後
確定TimerOut
等待DelayBetweenPolls 後
|
|
|
第二次發送


# Serial Port中
Advanced...
內可以設定
Flow Contorl是自資料流量控制
DSR:Data set Ready(232用)
CTS:Clear to Send配的是RTS CTS (232用)
DTR:Data Terminal Ready(也是232)
RTS:Toggle n [ms] RTS disable delay:資料送完後才關RTS
RTS ON  → 啟用發送器
      ↓
送資料
      ↓
資料送完
      ↓
等待 RTS Disable Delay
      ↓
RTS OFF → 切換接收模式
Remove ECHO勾選送出的Data避免自己收到自己的訊息由VXCOMME過濾掉


#資料表設定
Setup>>Read/Write Definition...
內部可以設定
Slave ID 設定ID如果是TCP則通常設定為1
Function通訊命令看需求設定
Adress設定的是Address表需要知道你要的記憶體位子在哪
Ouantity資料長度
ScanRate:輪詢時間就是完成一個輪詢後等待多久在執行下一次輪尋
關於PLCAddressBase1
| 不勾 Base1 | 勾 Base1 | 實際 Modbus 位址 |
| -------- | ------- | ------------ |
| 0        | 1       | 400001       |
| 1        | 2       | 400002       |
| 2        | 3       | 400003       |
| 3        | 4       | 400004       |
假設位子原本妳要41123=1122位子但因為你勾選了1為基礎所以你的位子變成1123但這指是換算顯示過程並不影響實際丟封包出去的結果

選項內有
Read/Write Once只執行一次
Read/Write Disabled停止輪巡
Disable on Error 通訊Error時候停止輪巡
View
可以設定顯示幾個列或者按照設定長度
Hide Alias Columns是否顯示別名
Address incell 顯示方式是否要對應記憶體位子顯示
例如不勾選數值為0 顯示0
勾選時候  數值為0=0001Address
Enron/Daniel Mode特出廠牌的延伸不要勾選

今天廠內側是TCP通訊MP850時候
V數值=Uint32=記憶體位子=41107+41108
KWh=Uint16=分三個Word以不同倍率加總呈現數值並非組合word~41051~41053
A=Uint32=41123+41124
總P=int32=41133+41134

另外可設定顯示方式
Signed
Unsigned
Hex-Acll
Binary
重點!!
32或64Bit可以做組合
例如Big-endian address 1100+1101=High+Low

例如Little-endian address 改成1101+1100
不要用
1100(H)+1101(L)
改成
1101(H)+1100(L)
(High/Low Word 順序交換)
還有byte Swap= 1100記憶體位子資料是十六進位 00 03 =>03 00
例如Big-endian byte Swap address十六進位 1100+1101=>00 03+ 00 20 =>03 00 + 20 00  High+Low
例如Little-endian byte Swap address十六進位 1101+1100=>=>00 20 + 00 03=>20 00 + 03 00  High+Low
