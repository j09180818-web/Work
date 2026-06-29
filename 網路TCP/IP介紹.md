1. Network，網路
多種設備互相連接通訊就叫做網路
例如手機 電腦   印表機 等等互相通訊就叫做網路

2. LAN，Local Area Network，區域網路
只小範圍內的連結網路，而通常他們透過SWITCH連線
補充VLAN
| 名稱              | 說明                      |
| --------------- | ----------------------- |
| LAN             | 區域網路                    |
| VLAN            | 虛擬區域網路                  |
| LAN 重點          | 一個實際或邏輯上的區域網路           |
| VLAN 重點         | 在 Switch 上切出多個邏輯 LAN    |
| LAN 是否需要設定 VLAN | 不一定                     |
| VLAN 是否等於 LAN   | 可以把它理解成「被切出來的獨立 LAN」    |
| VLAN 之間能否直接通訊   | 通常不能                    |
| VLAN 之間通訊需要什麼   | Router 或 Layer 3 Switch |


3. WAN
WAN，Wide Area Network，廣域網路
指的是範圍較大的通訊例如廠對廠區對區，各種不同地區的LAN互相連接而成
台北工廠 LAN ─── Router ─── Internet/VPN ─── Router ─── 台中工廠 LAN

4. Internet 是什麼？
網際網路
觀念
Internet 是全球最大的網路。
可以把它想成：
很多很多 LAN 和 WAN 互相連接後形成的巨大網路。
你家裡的網路、公司網路、工廠網路、雲端伺服器，透過 ISP 連接在一起，就是 Internet。
ISP 是：
Internet Service Provider
網際網路服務供應商
例如中華電信、遠傳、台灣大哥大等。
| 名稱       | 中文   | 範圍  | 常見用途       |
| -------- | ---- | --- | ---------- |
| LAN      | 區域網路 | 小範圍 | 工廠內設備通訊    |
| WAN      | 廣域網路 | 大範圍 | 跨廠、跨地點連線   |
| Internet | 網際網路 | 全球  | 上網、雲端、遠端服務 |

# Switch   VS Hub

Hub 是很早期的網路設備屬於OSI的第一層實體層
他不負責判斷資料送給誰而是用廣播的方式將資料送到所有的Port口

SWitch 主要工作在OSI第二層:資料連結層
Switch會學習每台設備的MAC address,知道Port連接哪台設備所以可以指定傳輸
因此他不會直接廣播傳送資料也不容易被其他有心人士竊取資料
| Port   | MAC Address |
| ------ | ----------- |
| Port 1 | PC-A 的 MAC  |
| Port 2 | PC-B 的 MAC  |
| Port 3 | PC-C 的 MAC  |
| Port 4 | PC-D 的 MAC  |
記住SWITCH 是 Layer 2 設備透過MAC地址在通訊


| 項目           | Hub        | Switch             |
| ------------ | ---------- | ------------------ |
| 中文名稱         | 集線器        | 交換器                |
| OSI 層級       | 第 1 層 實體層  | 第 2 層 資料鏈結層        |
| 傳送方式         | 廣播給所有 Port | 依 MAC Address 指定傳送 |
| 效率           | 低          | 高                  |
| 安全性          | 較低         | 較高                 |
| 碰撞 Collision | 容易發生       | 大幅減少               |
| 是否學習 MAC     | 不會         | 會                  |
| 現代使用         | 幾乎淘汰       | 大量使用               |

另外資料碰撞Collosion:資料碰撞
在Hub中多台設備共用一個傳輸通道如果同時有設備發送訊號就可能發生資料碰撞，而SWitch因為都是獨立通道所以幾乎不會發生此問題。

Broadcast 廣播
Switch 雖然會依 MAC Address 傳送資料，但遇到某些封包仍然會廣播例如
ARP Request
DHCP Discover


# 名稱：ARP
ARP = Address Resolution Protocol
中文：位址解析協定
觀念
ARP 不負責判斷路由。
ARP 只負責：
已知某個 IP
查出那個 IP 的 MAC
什麼是ARP 當A電腦想知道C電腦的MAC Address就會使用廣播詢問雖然不知道目標設備MAC Address但Switch會將其告知所有設備等待回應
PC 要送 TCP 給 192.168.1.10
        ↓
PC 查自己的 ARP Table
        ↓
沒有 192.168.1.10 對應的 MAC
        ↓
PC 發 ARP Broadcast
        ↓
Switch 把 Broadcast 轉送給同 VLAN 其他 Port
        ↓
PLC 收到後回覆自己的 MAC
        ↓
PC 記到 ARP Table
        ↓
PC 開始送 TCP 封包
        ↓
Switch 根據目的 MAC 轉送到 PLC

同網段設備通訊方式
這是同網段 IP 通訊前，設備用來取得 MAC 的方式。
Switch 只是中間幫忙轉送 Layer 2 Broadcast。

| 重點                  | 說明                              |
| ------------------- | ------------------------------- |
| ARP 是誰發的？           | 發送端設備，例如 PC、PLC、HMI             |
| ARP 問誰？             | 同網段內，擁有該 IP 的設備                 |
| Switch 會不會主動問 ARP？  | 不會                              |
| Switch 做什麼？         | 看到 Broadcast 就轉送到同 VLAN 其他 Port |
| ARP 是不是針對 Switch？   | 不是                              |
| 沒有 Switch 會不會有 ARP？ | 會，只要是 Ethernet/IP 通訊仍可能需要       |
| ARP Table 在哪裡？      | 在 PC / PLC / HMI 這類設備裡          |
| MAC Table 在哪裡？      | 在 Switch 裡                      |


# Gateway（閘道器）
Default Gateway
預設閘道
Router
路由器

設定網路時候會看到下面這些
| 欄位名稱            | 英文    | 用途          |
| --------------- | ----- | ----------- |
| IP Address      | IP 位址 | 這台設備自己的地址   |
| Subnet Mask     | 子網路遮罩 | 判斷誰跟我同網段    |
| Default Gateway | 預設閘道  | 不同網段時，要先交給誰 |
例子假設
 IP          = 192.168.10.20
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.10.1
Gateway不能亂填寫必須同網段192.168.10.x
當設備要離開這個192.168.10網段的時候
必須透過跨網段設備上面可以設定多個介面網段去做跨網段的動作

誰可以跨網段
| 設備             | 能不能跨網段 | 原因                  |
| -------------- | -----: | ------------------- |
| Hub            |     不行 | 只處理實體訊號             |
| Layer 2 Switch |     不行 | 主要看 MAC             |
| Layer 3 Switch |     可以 | 可以看 IP 做 Routing    |
| Router         |     可以 | 專門做跨網段              |
| Firewall       |     可以 | 通常具備 Routing + 安全規則 |
| 工業路由器          |     可以 | 用於工業網段互通或遠端連線       |
只要Layer 3就可以跨網段

同網段通訊：靠 MAC 在本地網路內送到對方。
跨網段通訊：靠 IP 判斷目標網段，先送給 Gateway，再由 Router / Layer 3 設備轉送。



# Gateway 跨網段通訊範例筆記

有些時候手動修改還是能連出去是因為
Gateway 還是由 DHCP 保留或自動取得
有些系統的網路設定不是「全手動」，而是類似：
IP Address      = 手動
Subnet Mask     = 手動
Default Gateway = 自動 / 保留
DNS             = 自動 / 保留
所以你看到只填 IP 和 Mask，但系統其實還有 Gateway。

## 核心觀念

如果一台設備要連到「其他網段」，它自己不能直接送到目標設備。

它必須先把資料交給 Default Gateway。

Default Gateway 通常是一台可以跨網段的設備，例如：

- Router，路由器
- Layer 3 Switch，三層交換器
- Firewall，防火牆
- 工業路由器

這台 Gateway 設備本身要有多個網段的設定，或是有路由表 Routing Table，才知道不同網段要往哪裡送。

---

## 範例架構

現在有三個設備：

PLC 在網段 A：

PLC IP = 192.168.10.20  
Mask = 255.255.255.0  
Gateway = 192.168.10.1  

HMI 在網段 B：

HMI IP = 192.168.20.30  
Mask = 255.255.255.0  
Gateway = 192.168.20.1  

中間有一台 Router：

Router 介面 A = 192.168.10.1  
Router 介面 B = 192.168.20.1  

---

## 網路圖

PLC  
192.168.10.20  
│  
Router 介面 A  
192.168.10.1  
│  
Router / Gateway  
│  
Router 介面 B  
192.168.20.1  
│  
HMI  
192.168.20.30  

---

## Router 上的設定

Router 必須知道它連接了哪些網段。

Router 介面設定：

介面 A：
IP = 192.168.10.1  
Mask = 255.255.255.0  
連接網段 = 192.168.10.0/24  

介面 B：
IP = 192.168.20.1  
Mask = 255.255.255.0  
連接網段 = 192.168.20.0/24  

所以 Router 知道：

192.168.10.0/24 在介面 A  
192.168.20.0/24 在介面 B  

---

## PLC 要傳給 HMI 的流程

PLC 要傳資料給：

192.168.20.30

PLC 先判斷：

192.168.20.30 不在我的 192.168.10.0/24 網段。

所以 PLC 不會直接送給 HMI。

PLC 會先把資料交給自己的 Default Gateway：

192.168.10.1

也就是 Router 的介面 A。

Router 收到資料後，查看目標 IP：

目標 IP = 192.168.20.30

Router 查自己的路由資訊，發現：

192.168.20.0/24 在介面 B。

所以 Router 把資料從介面 B 送出去，最後送到 HMI。

---

## HMI 回覆 PLC 的流程

HMI 要回覆 PLC：

192.168.10.20

HMI 判斷：

192.168.10.20 不在我的 192.168.20.0/24 網段。

所以 HMI 也會先把資料交給自己的 Default Gateway：

192.168.20.1

也就是 Router 的介面 B。

Router 收到後，查目標 IP：

目標 IP = 192.168.10.20

Router 發現：

192.168.10.0/24 在介面 A。

所以 Router 把資料從介面 A 送回 PLC。

---

## 重點整理

設備如果要去其他網段，自己只需要知道 Default Gateway 是誰。

Gateway 設備必須知道多個網段，或是有 Routing Table。

Router / Gateway 會根據目標 IP 判斷資料要往哪個網段送。

跨網段通訊要成功，兩邊設備都要有正確的 Default Gateway。

Router / Gateway 也要有正確的網段設定或路由設定。

---

## 一句話記憶

設備要去其他網段時，先交給 Gateway；Gateway 因為知道多個網段，所以能幫設備把資料轉送到正確的網段。


Router 路由器：
負責不同網段之間轉送封包。

Gateway 閘道器：
某台設備離開自己網段時使用的出口。

路由器通常會擔任 Gateway：
例如 PC 的 Default Gateway 設成 Router 的 LAN IP。

Gateway 是一種角色：
是其他設備把某個 IP 當作出口。

Router 本身也可能需要 Gateway：
例如 Router 要往 Internet 送資料時，會設定 Default Route。

一句話：
Router 是會轉送封包的設備；
Gateway 是某個網段要出去時指定的出口。