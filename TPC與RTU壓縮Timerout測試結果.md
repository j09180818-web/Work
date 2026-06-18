# Modbus TCP 為什麼比 RTU 容易 Timeout

## 現場架構

### RTU

電腦
↓
虛擬 COM Port
↓
PDS-755
↓
RS485
↓
電表

### TCP/IP

電腦
↓
公司內網
↓
Switch
↓
電表 Ethernet
Port

---

## 為什麼 TCP 比 RTU 容易 Timeout

### RTU 路徑較單純

資料路徑：

PC → PDS755 → RS485 → 電表

中間設備少。

通訊較固定。

延遲較穩定。

因此 Timeout 可以設定較短。

---

### TCP 路徑較複雜

資料路徑：

PC → 網卡 → Switch → 內網 → 電表

中間經過：

* TCP/IP 協定
* 網路交換器(Switch)
* 公司內網流量
* 電表網路模組

因此延遲變動較大。

---

## 共用公司網路的影響

Modbus TCP 與一般網路共用同一條實體網路：

* 網頁瀏覽
* 檔案傳輸
* NAS備份
* Teams
* 監視器串流

都可能經過同一個 Switch。

雖然 Modbus 封包很小，

但內網忙碌時仍可能產生額外延遲。

---

## 最終結論

TCP Timeout 較長不一定代表網路較慢。

真正原因通常是：

1. 經過內網與 Switch
2. 電表 TCP 模組處理時間較長
3. 延遲波動較 RTU 大

因此：

RTU Timeout 可設較短。

TCP Timeout 通常需要保留較大的安全餘裕。
