首先在LAN中
MAC Address 是 LAN / VLAN 內實際送 Frame 時使用的位址。
IP：我要送到哪個目標
MAC：目前這段 LAN 要交給哪張網卡

在LAN中
發送端無論是要用電腦IP或是用名稱找設備發送資料時都會遇到
ARP Table儲存IP對應的MAC
DNS Cache 儲存：名稱 → IP
這些表單如果沒有對應的
在使用IP發送時會ARP廣播詢問前提是同網段，而且 ARP Table 沒有紀錄。如果不同網段，不是 ARP 問目標設備，而是 ARP 問 Gateway。
用電腦名稱發送時會訪問DNS Server
然後本機端會儲存
| 項目    | DNS Cache      | ARP Table             |
| ----- | -------------- | --------------------- |
| 位置    | 電腦、IPC、HMI、伺服器 | 電腦、IPC、HMI、PLC、Router |
| 對應    | 名稱 → IP        | IP → MAC              |
| 用途    | 把設備名稱變成 IP     | 把 IP 變成目前 LAN 的 MAC   |
| 是否會過期 | 會              | 會                     |
| 查不到時  | 問 DNS Server   | 發 ARP 廣播              |
而Switch也有MAC 對應的表單叫做MAC TABLE他每次接收某PORT口進來的資料就會去看自己的TABLE表單上是否與紀錄相同如果不同就會修改
而DNS1. 手動建立 DNS Record或 自動註冊 Dynamic DNS / DDNS，在有設定 DDNS 的環境中，電腦自己或 DHCP Server 可能會把名稱與 IP 註冊到 DNS Server。

SWitch的MAC是針對區域網路內，但DNS是廣域也會使用所以他還是紀錄IP對應的電腦名稱

| 項目    | DNS Cache      | ARP Table             | Switch MAC Table  |
| ----- | -------------- | --------------------- | ----------------- |
| 位置    | 電腦、IPC、HMI、伺服器 | 電腦、IPC、HMI、PLC、Router | Switch            |
| 對應    | 名稱 → IP        | IP → MAC              | MAC → Port        |
| 用途    | 把名稱變成 IP       | 把 IP 變成目前 LAN 的 MAC   | 把 Frame 送到正確 Port |
| 查不到時  | 問 DNS Server   | 發 ARP 廣播              | Flood 到其他 Port    |
| 是否會過期 | 會              | 會                     | 會                 |


# DNS 筆記精簡版

## 1. DNS 是什麼？

DNS 全名是 **Domain Name System**，中文是 **網域名稱系統**。

DNS 的主要功能是：

```text
名稱 → IP Address
```

例如：

```text
www.google.com → 142.250.xxx.xxx
plc01.factory.local → 192.168.1.10
```

電腦真正通訊時使用的是 IP，不是名稱。
所以當我們輸入名稱時，電腦要先透過 DNS 查出 IP。

---

## 2. 網卡設定 DNS 是什麼意思？

在 Windows IPv4 設定中的 DNS 欄位，例如：

```text
Preferred DNS Server
Alternate DNS Server
```

意思是：

```text
我要查名稱時，要去問哪一台 DNS Server。
```

不是設定：

```text
我的電腦叫什麼名字
```

也不是設定：

```text
我的電腦自己是 DNS Server
```

---

## 3. DNS 查詢流程

假設電腦要連：

```text
plc01.factory.local
```

流程：

```text
1. 電腦發現目標是名稱，不是 IP
2. 先查自己的 DNS Cache
3. 如果沒有紀錄或紀錄過期，就問 DNS Server
4. DNS Server 回答 IP
5. 電腦取得 IP 後，才進入 ARP 流程
6. ARP 找到 MAC 後，才真正送出 Ethernet Frame
```

簡化：

```text
名稱 → DNS → IP → ARP → MAC → Switch 轉送
```

如果直接使用 IP：

```text
IP → ARP → MAC → Switch 轉送
```

直接使用 IP 時通常不需要 DNS。

---

## 4. DNS Cache 是什麼？

DNS Cache 是本機暫存的名稱解析結果。

它記錄：

```text
名稱 → IP
```

例如：

```text
plc01.factory.local → 192.168.1.10
```

所以不是每次使用名稱都一定要問 DNS Server。
如果 DNS Cache 有資料且未過期，就會直接使用。

DNS Cache 會過期，過期時間通常由 DNS Record 的 TTL 決定。

---

## 5. DNS 跟 ARP 的差異

| 項目      | DNS          | ARP              |
| ------- | ------------ | ---------------- |
| 對應關係    | 名稱 → IP      | IP → MAC         |
| 主要用途    | 把名稱轉成 IP     | 找目前 LAN 內下一站 MAC |
| 查不到時    | 問 DNS Server | 發 ARP 廣播         |
| 是否跨網段使用 | 可以           | 不跨 Router        |
| 常見快取    | DNS Cache    | ARP Table        |

最短記法：

```text
DNS：名稱找 IP
ARP：IP 找 MAC
Switch：MAC 找 Port
```

---

## 6. DNS 為什麼不是名稱對 MAC？

因為 MAC Address 只在同一個 LAN / VLAN 內有效。

跨 Router 時，每一段網路的 MAC 都會改變，但 IP 目的地仍然可以保持不變。

所以 DNS 必須對應 IP，而不是 MAC。

```text
DNS：找最終目標的 IP
ARP：找目前這段 LAN 的 MAC
```

---

## 7. DNS 跟 Gateway 可以是同一個 IP 嗎？

可以。

例如：

```text
Gateway：192.168.1.1
DNS：192.168.1.1
```

代表同一台設備同時提供：

```text
Gateway / Router 功能
DNS Forwarder / DNS Proxy 功能
```

但功能不同：

```text
Gateway：負責送封包到其他網段
DNS：負責查名稱對應 IP
```

一句話：

```text
Gateway 是送路的出口。
DNS 是查地址的電話簿。
```

---

## 8. DNS Server 怎麼知道名稱對應 IP？

主要有兩種方式。

### 方式一：手動建立 DNS Record

管理員手動建立：

```text
plc01.factory.local → 192.168.1.10
scada01.factory.local → 192.168.1.50
```

工業現場固定 IP 設備常用這種方式。

### 方式二：Dynamic DNS / DDNS

在公司網域或 DHCP 環境中，電腦取得 IP 後，電腦自己或 DHCP Server 可能會把：

```text
電腦名稱 → IP
```

註冊到 DNS Server。

例如：

```text
IPC01.factory.local → 192.168.1.100
```

但不是所有 DHCP 環境都一定會自動更新 DNS。

---

## 9. 工業現場使用建議

PLC、HMI 這類設備若追求穩定，通常建議直接使用固定 IP。

例如：

```text
PLC：192.168.1.10
HMI：192.168.1.20
```

如果要用名稱，例如：

```text
plc01.factory.local
```

就要確保：

```text
1. DNS Server 穩定
2. DNS Record 正確
3. 設備支援 DNS 查詢
4. DNS Cache 過期後仍能查到正確 IP
```

---

# 重點整理

```text
1. DNS 是名稱轉 IP。
2. DNS Server 是查名稱時要詢問的伺服器。
3. DNS Cache 是本機暫存的名稱 → IP。
4. 使用名稱連線時，會先查 DNS。
5. 直接使用 IP 連線時，通常不查 DNS。
6. DNS 查到 IP 後，才進入 ARP 流程。
7. DNS 對應 IP，不對應 MAC，因為 MAC 只在 LAN / VLAN 內有效。
8. Gateway 和 DNS 可以是同一台設備，但功能不同。
9. DNS Record 可以手動建立，也可能透過 DDNS 自動註冊。
```
