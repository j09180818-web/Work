OPC（Open Platform Communications）是一種工業自動化領域的通訊標準協定，主要用途是讓不同廠牌的設備、PLC、監控系統（SCADA）、MES、ERP 等軟體能夠彼此交換資料。

當多家設備都有自己協定的時候需要做軟體監控就會變得非常繁雜不易，
因此OPC的就是監控軟體與各家設備通訊的橋樑，監控者不需要知道PLC是何品牌只要會OPC即可取得資料
#備註:SCADA=資料採集監控系統
# OPC運作架構
1.OPC Server
負責與設備通訊
2.OPC Client
負責向OPC取資料
| 項目 | Modbus  | OPC       |
| -- | ------- | --------- |
| 類型 | 通訊協定    | 資料交換標準    |
| 對象 | 設備 ↔ 設備 | 軟體 ↔ 軟體   |
| 層級 | 底層      | 上層        |
| 用途 | 傳輸資料    | 統一資料格式    |
| 範例 | PLC→PLC | PLC→SCADA |


我詢問的GPT

我想問的是OPC Server的功能在我看來有這幾層 1.最基礎必須擁有各家通訊模式或者最廣泛的RTU TPC 2.在內部必須可以設定資料位子代表什麼資料例如D100=temp D200=bar 第二點問題是這是固定資料協定還是活性化設定畢竟誰也不確定今天讀取溫度位子放在哪 3.在一切整理後再將不同家廠商的資料讀取透過不同設定的資料位子紀錄檔案整理成相同的資料表格? 是我理解的這樣嗎
# 我的理解
OPC Server 的實際工作流程
1. 第一層：設備驅動 (Driver)
必須擁有各家通訊模式
完全正確。
例如 OPC Server 內部會有很多 Driver：
OPC Server
├─ Mitsubishi Driver
├─ Siemens Driver
├─ Modbus TCP Driver
├─ Modbus RTU Driver
├─ Allen-Bradley Driver
├─ Omron Driver
└─ BACnet Driver

2. 第二層：Tag Mapping（標籤映射）
在 OPC Server 中建立對應的資料表格
ex:
| Tag Name | Address |
| -------- | ------- |
| Temp     | D100    |
| Pressure | D101    |
| Alarm    | M100    |

3. 第三層：資料標準化
Line1.Temp → D100
Line2.Temp → DB1.DBW0
Line3.Temp → 40001
OPC Client 根本不用知道：
D100 是什麼
DB1.DBW0 是什麼
40001 是什麼
他只要去找他的表格資料
說整理成表格不算對而是整理成對應的資料樣貌
Tag
├─ Name
├─ Value
├─ Quality
├─ Timestamp

| Tag        | Value | Quality | Time     |
| ---------- | ----- | ------- | -------- |
| Line1.Temp | 25.6  | Good    | 10:00:00 |
| Line2.Temp | 28.1  | Good    | 10:00:00 |
| Line3.Temp | 30.2  | Good    | 10:00:00 |
