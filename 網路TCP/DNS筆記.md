# DNS、Gateway、IPv4 網卡設定與電腦名稱解析筆記

## 一、DNS 是什麼？

DNS 全名是 **Domain Name System**，中文叫做 **網域名稱系統**。

DNS 的主要功能是：

> 把「名稱」轉換成「IP 位址」。

例如：

```text
www.google.com → 142.250.xxx.xxx
```

電腦在網路上真正通訊時，使用的是 **IP 位址**，不是網址名稱。

所以當我們在瀏覽器輸入：

```text
www.google.com
```

電腦需要先透過 DNS 查詢：

```text
www.google.com 的 IP 是多少？
```

查到 IP 之後，電腦才會使用該 IP 去建立連線。

---

## 二、DNS 的核心觀念

可以把 DNS 想成網路世界的電話簿。

人類容易記：

```text
www.google.com
```

但電腦真正需要的是：

```text
142.250.xxx.xxx
```

所以 DNS 的作用就是：

```text
名稱 → IP 位址
```

例如：

```text
scada01.factory.local → 192.168.10.100
plc01.factory.local   → 192.168.10.20
nas01.company.local   → 192.168.10.200
```

---

## 三、設定網卡 DNS 是什麼意思？

在 Windows 網卡 IPv4 設定中，常見 DNS 欄位有：

```text
慣用 DNS 伺服器
其他 DNS 伺服器
```

英文是：

```text
Preferred DNS Server
Alternate DNS Server
```

這裡設定的意思是：

> 我的電腦要查名稱時，要去問哪一台 DNS Server。

不是設定：

```text
我的電腦叫什麼 DNS 名稱
```

也不是設定：

```text
我的電腦自己是 DNS Server
```

---

## 四、DNS 設定範例

假設電腦 IPv4 設定如下：

```text
IP Address：192.168.1.50
Subnet Mask：255.255.255.0
Default Gateway：192.168.1.1
DNS Server：8.8.8.8
```

代表意思是：

```text
我的 IP 是 192.168.1.50
我用 255.255.255.0 判斷同不同網段
我要去其他網段時，交給 192.168.1.1
我要查網址名稱時，去問 8.8.8.8
```

如果使用者輸入：

```text
www.google.com
```

流程如下：

```text
1. 電腦發現 www.google.com 是名稱，不是 IP

2. 電腦查看自己的 DNS 設定

3. 電腦向 DNS Server 8.8.8.8 詢問：
   www.google.com 的 IP 是多少？

4. DNS Server 回答對應的 IP

5. 電腦再使用該 IP 去連線
```

---

## 五、DNS 跟 Gateway 可以設定相同嗎？

可以。

例如：

```text
Gateway：192.168.1.1
DNS：192.168.1.1
```

這是很常見的設定。

---

## 六、為什麼 DNS 跟 Gateway 可以相同？

因為同一台設備可以同時提供多種功能。

例如家用路由器、工業路由器、防火牆、L3 Switch 可能同時具備：

```text
1. Gateway 功能
2. DNS Forwarder / DNS Proxy 功能
3. DHCP Server 功能
4. NAT 功能
```

所以 `192.168.1.1` 這台設備可能同時是：

```text
預設閘道 Gateway
DNS 查詢轉發器 DNS Forwarder
DHCP 發放 IP 的伺服器
```

---

## 七、DNS 與 Gateway 的角色差異

| 設定         | 功能          | 例子          |
| ---------- | ----------- | ----------- |
| Gateway    | 幫你把封包送到其他網段 | 192.168.1.1 |
| DNS Server | 幫你查名稱對應 IP  | 192.168.1.1 |

雖然 IP 一樣，但功能不同。

一句話記：

```text
Gateway 是負責送路。
DNS 是負責查地址。
```

---

## 八、DNS 與 Gateway 相同 IP 的流程範例

假設電腦設定：

```text
IP：192.168.1.50
Mask：255.255.255.0
Gateway：192.168.1.1
DNS：192.168.1.1
```

使用者輸入：

```text
www.google.com
```

流程如下：

```text
1. 電腦發現 www.google.com 是名稱，不是 IP

2. 電腦問 DNS Server：
   192.168.1.1，請問 www.google.com 的 IP 是多少？

3. 192.168.1.1 使用 DNS 功能回答，或轉問外部 DNS

4. 電腦取得 www.google.com 的 IP

5. 電腦判斷該 IP 不在自己的網段

6. 電腦把封包交給 Gateway：
   192.168.1.1

7. 192.168.1.1 使用 Gateway / Router 功能把封包送出去
```

同一個 IP，但前面使用的是 **DNS 功能**，後面使用的是 **Gateway 路由功能**。

---

## 九、DNS 跟 Gateway 不同的情況

公司或工廠網路常見設定：

```text
IP Address：192.168.10.50
Subnet Mask：255.255.255.0
Gateway：192.168.10.1
DNS Server：192.168.10.10
```

可能代表：

```text
192.168.10.1  = 防火牆 / Router / Gateway
192.168.10.10 = Windows Server / AD Server / DNS Server
```

公司內部 DNS 可能記錄：

```text
scada01.factory.local → 192.168.10.100
plc01.factory.local   → 192.168.10.20
nas01.company.local   → 192.168.10.200
```

外部 DNS，例如：

```text
8.8.8.8
1.1.1.1
```

不一定知道這些內部名稱。

所以在公司或工廠網域環境中，不建議隨便把 DNS 改成外部 DNS，否則可能造成：

```text
無法登入網域
找不到內部伺服器
找不到檔案伺服器
找不到授權伺服器
內部系統名稱解析失敗
```

---

## 十、Windows IPv4 網卡設定有哪些選項？

在 Windows IPv4 設定中，常見選項包含：

```text
1. 自動取得 IP 位址
2. 使用下列 IP 位址
3. IP 位址
4. 子網路遮罩
5. 預設閘道
6. 自動取得 DNS 伺服器位址
7. 使用下列 DNS 伺服器位址
8. 慣用 DNS 伺服器
9. 其他 DNS 伺服器
```

---

## 十一、自動取得 IP 位址

英文：

```text
Obtain an IP address automatically
```

意思是：

> 電腦向 DHCP Server 自動取得 IP 設定。

DHCP Server 可能會發給電腦：

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Time
```

也就是說，電腦不需要手動設定 IP，而是由 DHCP Server 分配。

---

## 十二、使用下列 IP 位址

英文：

```text
Use the following IP address
```

意思是：

> 手動固定 IP。

需要自己填：

```text
IP Address
Subnet Mask
Default Gateway
```

範例：

```text
IP Address：192.168.1.50
Subnet Mask：255.255.255.0
Default Gateway：192.168.1.1
```

---

## 十三、IP Address 是什麼？

中文：**IP 位址**

功能：

> 設定這台電腦在網路上的位址。

範例：

```text
192.168.1.50
```

意思是：

```text
這台電腦在 192.168.1.x 網段中的位置是 50
```

---

## 十四、Subnet Mask 是什麼？

中文：**子網路遮罩**

功能：

> 判斷目標 IP 是否跟自己在同一個網段。

範例：

```text
IP：192.168.1.50
Mask：255.255.255.0
```

代表這台電腦所在網段是：

```text
192.168.1.0/24
```

可用主機範圍大約是：

```text
192.168.1.1 ~ 192.168.1.254
```

---

## 十五、Default Gateway 是什麼？

中文：**預設閘道**

功能：

> 當目標 IP 不在同一個網段時，把封包交給 Gateway。

範例：

```text
Default Gateway：192.168.1.1
```

意思是：

```text
如果我要去不同網段或 Internet，就把封包交給 192.168.1.1
```

---

## 十六、自動取得 DNS 伺服器位址

英文：

```text
Obtain DNS server address automatically
```

意思是：

> DNS Server 的 IP 也由 DHCP Server 自動發給你。

例如 DHCP Server 可能發給電腦：

```text
IP：192.168.1.50
Mask：255.255.255.0
Gateway：192.168.1.1
DNS：192.168.1.1
```

---

## 十七、使用下列 DNS 伺服器位址

英文：

```text
Use the following DNS server addresses
```

意思是：

> 手動指定 DNS Server。

範例：

```text
Preferred DNS Server：8.8.8.8
Alternate DNS Server：1.1.1.1
```

意思是：

```text
查名稱時先問 8.8.8.8
如果 8.8.8.8 沒回應，再問 1.1.1.1
```

---

## 十八、IPv4 設定欄位總表

| 欄位                       | 中文         | 作用              |
| ------------------------ | ---------- | --------------- |
| IP Address               | IP 位址      | 設定自己是誰          |
| Subnet Mask              | 子網路遮罩      | 判斷同不同網段         |
| Default Gateway          | 預設閘道       | 離開本網段時交給誰       |
| Preferred DNS Server     | 慣用 DNS 伺服器 | 查名稱時優先問誰        |
| Alternate DNS Server     | 其他 DNS 伺服器 | 第一台 DNS 無回應時備用  |
| Obtain IP automatically  | 自動取得 IP    | 由 DHCP 發 IP 設定  |
| Obtain DNS automatically | 自動取得 DNS   | 由 DHCP 發 DNS 設定 |

---

## 十九、電腦名稱是什麼？

Windows 電腦會有一個 **Computer Name**。

中文：**電腦名稱**

例如：

```text
IPC01
SCADA01
ENG-PC
DESKTOP-A123
```

這是電腦自己的名稱。

可以用指令查看：

```cmd
hostname
```

Windows 中可以在這裡修改：

```text
設定 → 系統 → 關於 → 重新命名此電腦
```

---

## 二十、電腦名稱設定後，是誰記住這個名稱？

要分成兩層來看。

### 第一層：電腦自己記住

電腦名稱首先存在：

```text
這台電腦自己的作業系統裡
```

也就是 Windows 自己知道：

```text
我叫 IPC01
```

但是，只有自己知道還不夠。

其他電腦要用名稱找到它，還需要名稱解析機制。

---

### 第二層：讓其他電腦查得到

其他電腦如果要透過名稱找到這台電腦，通常需要以下其中一種機制：

```text
1. DNS Server 有紀錄
2. DHCP 幫忙註冊到 DNS
3. 電腦自己向 DNS Server 動態註冊
4. 區域網路廣播 / 多播名稱解析
```

---

## 二十一、DNS 是怎麼知道電腦名稱的？

常見方式有三種。

---

### 方式一：管理員手動在 DNS Server 建立紀錄

例如 IT 人員在 DNS Server 裡建立：

```text
IPC01.factory.local → 192.168.10.50
```

這種紀錄通常叫：

```text
A Record
```

意思是：

```text
主機名稱對應 IPv4 位址
```

所以其他電腦查：

```text
IPC01.factory.local
```

DNS Server 就會回答：

```text
192.168.10.50
```

---

### 方式二：DHCP 發 IP 時，幫忙更新 DNS

這在公司網域環境很常見。

流程如下：

```text
1. 電腦開機

2. 電腦向 DHCP Server 要 IP

3. 電腦在 DHCP Request 裡可能帶上自己的 Hostname
   例如：IPC01

4. DHCP Server 分配 IP
   例如：192.168.10.50

5. DHCP Server 可以把這個名稱與 IP 註冊到 DNS Server

6. DNS Server 產生紀錄：
   IPC01.factory.local → 192.168.10.50
```

所以不是單純「廣播給 DHCP」。

比較正確的說法是：

> 電腦向 DHCP 要 IP 時，可能會把自己的 Hostname 一起送給 DHCP Server，然後 DHCP Server 可以協助更新 DNS。

---

### 方式三：電腦自己向 DNS Server 動態註冊

在 Windows 網域環境中，電腦也可能自己向 DNS Server 註冊自己的名稱。

例如：

```text
電腦名稱：IPC01
IP：192.168.10.50
DNS Domain：factory.local
```

DNS Server 可能會有：

```text
IPC01.factory.local → 192.168.10.50
```

這種方式通常叫：

```text
Dynamic DNS Update
動態 DNS 更新
```

---

## 二十二、DNS 是靠廣播知道名稱嗎？

一般來說：

> DNS 不是靠廣播運作。

DNS 查詢通常是：

```text
Client → DNS Server
```

也就是電腦會問明確指定的 DNS Server IP。

例如：

```text
我的 DNS Server 是 192.168.10.10
所以我要查 IPC01.factory.local 時，就去問 192.168.10.10
```

---

## 二十三、那什麼東西會用廣播或多播？

在區域網路內，有些名稱解析機制可能使用廣播或多播。

例如：

```text
NetBIOS Name Resolution
LLMNR
mDNS
```

這些機制可能讓你在小型區域網路中用名稱找到設備。

例如：

```cmd
ping IPC01
```

有時候會成功，但不一定是 DNS 成功。

可能是靠：

```text
NetBIOS
LLMNR
mDNS
```

這類區域網路名稱解析機制。

---

## 二十四、DNS、DHCP、廣播、多播比較

| 機制      | 主要用途                        | 是否主要靠 DNS Server | 是否可能用廣播/多播         |
| ------- | --------------------------- | ---------------- | ------------------ |
| DNS     | 名稱轉 IP                      | 是                | 通常不是               |
| DHCP    | 自動發 IP 設定                   | 否                | DHCP Discover 會用廣播 |
| NetBIOS | 舊式 Windows 名稱解析             | 否                | 可能使用廣播             |
| LLMNR   | 區域名稱解析                      | 否                | 使用多播               |
| mDNS    | 區域名稱解析，常見於 .local、IoT、Apple | 否                | 使用多播               |

---

## 二十五、DHCP、DNS、電腦名稱三者關係

### DHCP 的主要工作

DHCP 主要負責：

```text
自動發 IP 設定
```

它可以發給電腦：

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Time
```

---

### DNS 的主要工作

DNS 主要負責：

```text
名稱 → IP
```

例如：

```text
scada01.factory.local → 192.168.10.100
```

---

### 電腦名稱的角色

電腦名稱是：

```text
這台電腦自己的名字
```

例如：

```text
IPC01
```

但只有自己知道還不夠。

如果要讓其他人查得到，需要：

```text
DNS Server 有紀錄
或
區域網路名稱解析機制能找到它
```

---

## 二十六、完整範例：公司網路

假設公司網路設定：

```text
電腦名稱：IPC01
IP：自動取得
DHCP Server：192.168.10.1
DNS Server：192.168.10.10
網域：factory.local
```

流程如下：

```text
1. IPC01 開機

2. IPC01 向 DHCP Server 要 IP

3. DHCP Server 發給 IPC01：
   IP：192.168.10.50
   Mask：255.255.255.0
   Gateway：192.168.10.1
   DNS：192.168.10.10
   Domain：factory.local

4. IPC01 或 DHCP Server 將名稱註冊到 DNS

5. DNS Server 記住：
   IPC01.factory.local → 192.168.10.50

6. 其他電腦查詢：
   IPC01.factory.local 的 IP 是多少？

7. DNS Server 回答：
   192.168.10.50
```

---

## 二十七、完整範例：工業現場固定 IP

假設現場設備：

```text
PLC01：192.168.0.10
HMI01：192.168.0.20
IPC01：192.168.0.30
Mask：255.255.255.0
```

如果 HMI 直接連 PLC：

```text
PLC IP：192.168.0.10
```

那麼：

```text
不需要 DNS
不需要電腦名稱解析
```

因為目標直接就是 IP。

---

## 二十八、如果工業現場想用名稱連線

例如希望 HMI 或 IPC 使用：

```text
PLC01.factory.local
```

那就需要 DNS Server 裡有紀錄：

```text
PLC01.factory.local → 192.168.0.10
```

否則電腦不知道：

```text
PLC01.factory.local 是哪個 IP
```

---

## 二十九、工業現場建議

### 1. PLC、HMI 通訊

建議使用固定 IP：

```text
PLC：192.168.0.10
HMI：192.168.0.20
```

原因：

```text
穩定
直覺
不依賴 DNS
不容易因名稱解析失敗導致斷線
```

---

### 2. IPC、SCADA、資料庫、伺服器

可以考慮使用 DNS 名稱：

```text
scada01.factory.local
sql01.factory.local
mqtt01.factory.local
```

原因：

```text
伺服器 IP 變更時，只要改 DNS 紀錄
Client 不需要全部重新設定
較適合大型系統管理
```

---

### 3. 公司網域環境

若有 Active Directory，DNS 通常很重要。

不要隨便把 DNS 改成：

```text
8.8.8.8
1.1.1.1
```

可能會造成：

```text
無法登入網域
找不到內部伺服器
找不到檔案伺服器
找不到授權伺服器
內部系統名稱解析失敗
```

---

## 三十、常用檢查指令

### 1. 查看 IP、Gateway、DNS 設定

```cmd
ipconfig /all
```

可以看到：

```text
IPv4 Address
Subnet Mask
Default Gateway
DNS Servers
Host Name
Connection-specific DNS Suffix
DHCP Enabled
DHCP Server
```

---

### 2. 查看電腦名稱

```cmd
hostname
```

---

### 3. 查 DNS 解析結果

```cmd
nslookup www.google.com
```

查內部主機：

```cmd
nslookup IPC01.factory.local
```

---

### 4. 測試能不能連 IP

```cmd
ping 8.8.8.8
```

---

### 5. 測試能不能解析名稱

```cmd
ping www.google.com
```

如果：

```text
ping 8.8.8.8 成功
ping www.google.com 失敗
```

通常表示：

```text
網路可能通
但 DNS 解析有問題
```

---

## 三十一、你輸入網址時的流程

```text
www.google.com
      ↓
電腦檢查這是名稱，不是 IP
      ↓
查詢自己設定的 DNS Server
      ↓
DNS Server 回答目標 IP
      ↓
電腦判斷目標 IP 是否同網段
      ↓
不同網段 → 交給 Gateway
      ↓
Gateway 幫你送出去
```

---

## 三十二、你直接輸入 IP 時的流程

```text
192.168.0.10
      ↓
已經是 IP
      ↓
不需要 DNS
      ↓
判斷是否同網段
      ↓
同網段 → ARP 查 MAC
不同網段 → 交給 Gateway
```

---

## 三十三、DNS、Gateway、DHCP、Computer Name 簡表

| 名稱            | 中文       | 功能                     |
| ------------- | -------- | ---------------------- |
| IP Address    | IP 位址    | 我自己的地址                 |
| Subnet Mask   | 子網路遮罩    | 判斷對方是否同網段              |
| Gateway       | 預設閘道     | 去其他網段時交給誰              |
| DNS Server    | DNS 伺服器  | 查名稱對應 IP 時問誰           |
| DHCP          | 動態主機設定協定 | 自動發 IP、Gateway、DNS     |
| Computer Name | 電腦名稱     | 這台電腦自己的名字              |
| DNS Record    | DNS 紀錄   | DNS Server 裡的名稱與 IP 對應 |

---

## 三十四、重點整理

```text
1. DNS 是把名稱轉成 IP 的系統。

2. 網卡裡設定 DNS Server，
   是設定「我要查名稱時問誰」。

3. 網卡裡設定 DNS，
   不是設定「我的電腦叫什麼名字」。

4. DNS 和 Gateway 可以設定成同一個 IP。

5. 原因是同一台設備可以同時提供：
   Gateway 路由功能
   DNS Forwarder 功能
   DHCP Server 功能

6. Gateway 負責：
   幫你把封包送到其他網段。

7. DNS 負責：
   幫你查名稱對應哪個 IP。

8. DHCP 負責：
   自動發 IP、Mask、Gateway、DNS 等設定。

9. 電腦名稱 Computer Name 是存在電腦自己系統裡。

10. 要讓別人用名稱找到這台電腦，
    需要 DNS Server 有對應紀錄，
    或靠區域網路名稱解析機制。

11. DNS 不是靠廣播運作。

12. DHCP 發 IP 時，
    電腦可能會把 Hostname 告訴 DHCP，
    DHCP Server 可以協助更新 DNS。

13. Windows 網域環境中，
    電腦也可能自己向 DNS Server 做 Dynamic DNS Update。

14. 工業現場 PLC/HMI 固定 IP 通訊，
    通常不一定需要 DNS。

15. IPC、SCADA、資料庫、MQTT、雲端連線，
    若使用主機名稱，就需要 DNS。
```

---

## 三十五、最簡短記憶版

```text
IP Address：
我自己的地址。

Subnet Mask：
我怎麼判斷對方是不是同網段。

Gateway：
我要去別的網段時，交給誰。

DNS Server：
我要查名字對應 IP 時，問誰。

Computer Name：
我這台電腦自己的名字。

DNS Record：
DNS Server 裡記錄的「名字 → IP」。

DHCP：
幫電腦自動發 IP、Gateway、DNS 等設定。
```

一句話記：

```text
Gateway 是送路的出口，DNS 是查地址的電話簿；
它們可以在同一台設備上，但功能完全不同。
```
