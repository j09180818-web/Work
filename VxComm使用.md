VxComm utility軟體
介面按鈕 Sreach Servers可以搜尋現在已經接著的網路Port口
此時可以在左側按鈕 Add Servers(s)就可以建立所收尋到的設備 我這邊使用PDS-755
此時會開啟設定頁面
# Adding Servers
其中分為三個小頁籤
IP Range 有Start 跟End 但我現在只有一個設備所以我只有一個號碼如果你有多個設定IP範圍他會去搜尋有的設備建立
介面可以選擇
skip duplicated ip 跳過重複的IP
get name Automatically 獲得設備名稱
Includes the following Spcial Ip 
這邊選擇 GateWay254 Gateway 自己的管理站號（Management Address）
如果選擇255 Broadcast 這是廣播就是發送訊號同時給所有設備但是不允許回傳
在Virtuall comand IO設定時
起始的Com假設設定在 Com n 時候
設備有5個com的話 例如在PDS-755上

下方Virtual Com and I/O Port Mappings
設定虛擬ComPort: Com 10(以此設備為例有5個Com會自動向下計算所需數量)
兩個像選項
# Fixed baudrate use current settings of servers.
此選項設定是確認設備是否固定鮑率（Baud Rate）若沒有勾選此設定，在其他設備設定速率時設備會被更動到鮑率（Baud Rate）
# Maps vitual Com ports to "Port I/O" on servers
是否要顯示映射
ex Por1 <--->Com10
後面兩頁的設定在後續也可以選擇 包含Configure Server 和 Configure Port這幾個選項內都可以在設定

## Configure Server
1.Keep Alive time(sec):120
每當120秒確定TCP沒有斷開

2.Connection Broken:180sec
超過180秒沒有正常連線判斷為斷線

3.ConnectTimerOut:5
連線設備例如PDS755超過5秒就視為TimerOut

4.CommandPort TCP:10000 管理Tcp Port
5.Virtual:9999特殊功能的TCP Port
6.IP address:設備IP
7.Redundant IP[Secondary]備援IP

## Configure Port
首先點到哪一個就會設定哪一個
這側會顯示那些Com
如果剛剛建立的時候設定com以PDS755為例子
Port1~5因為設備有5個Com但還會多一個Port i/o
Port I/O 特殊設備使用 選擇UnMap即可 不映射
COM1
COM2
COM3
COM4
COM5


PC部分設定
Port Mapping(PC)
1.Re-assign Com number fot all Subsequrnt ports.
勾選後會一次幫忙修改映射的COm如果沒勾就是只改選擇的port

2.Fixed baud rate. use server current settings.Skip ...
一樣是依照VXCOM設定為主不讓其他設備去修改這樣多個設備連結實可以減少發生衝突問題

3.Disable Purge command {e-g.ModScan32}.Don't clear TX / RX Buffers
勾選會禁止使用者清空TXRX緩存區資料一般此項目不勾選

4.Enable Write-Buffer to collect small packets into big one. Auto-Flush Interval: n (ms,10~500,default=50)
勾選後資料會先存在緩存在設定n秒後一次送出