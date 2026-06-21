首先說明以前學過類似的格式ini格式
| 格式          | 名稱               |
| ----------- | ---------------- |
| `[Section]` | 區段（Section）      |
| `Key`       | 鍵（Key）           |
| `=`         | 指派符號（Assignment） |
| `Value`     | 值（Value）         |
[Section]
Key1=Value1
Key2=Value2
Key3=Value3

JSON資料格式
{
    "Key1": "Value1",
    "Key2": "Value2"
}
| 格式         | 名稱               |
| ---------- | ---------------- |
| `,`        | 項目分隔符（Delimiter） |
| `"Key1"`   | 鍵                |
| `"Value1"` | 值                |
| `"Key2"`   | 鍵                |
| `"Value2"` | 值                |
具有巢狀與陣列
巢狀
{
    "User": {
        "Name": "安"
    }
}
| 格式       | 名稱                 |
| -------- | ------------------ |
| `"User"` | 鍵                  |
| `{ }`    | 子物件（Nested Object） |
陣列
{
    "Users": [
        {},
        {}
    ]
}
| 格式    | 名稱             |
| ----- | -------------- |
| `[ ]` | 陣列（Array）      |
| `{ }` | 陣列中的物件（Object） |
結構
Object
├─ Key : Value
├─ Key : Value
└─ Key : Object

Array
├─ Object
├─ Object
└─ Object
JSON 是一種具有物件結構的資料交換格式。

它以 Key-Value 方式表示資料。

Value 可以是多種資料型別，
例如 string、number、boolean、null、object、array。

因為 JSON 支援物件與陣列，
所以可以用陣列存放多筆物件資料。

陣列中的每一個物件，
通常可以視為一筆完整資料。
| JSON 型別 | 範例                |
| ------- | ----------------- |
| string  | `"安"`             |
| number  | `30`、`36.5`       |
| boolean | `true`、`false`    |
| null    | `null`            |
| object  | `{ "Name": "安" }` |
| array   | `[1, 2, 3]`       |
範例
{
    "Machines": [
        {
            "Name": "M1",
            "Status": "Run",
            "Speed": 100,
            "IsAlarm": false
        },
        {
            "Name": "M2",
            "Status": "Stop",
            "Speed": 0,
            "IsAlarm": true
        }
    ]
}
可以理解為
Machines = 一組機台資料

Machine[0]
├─ Name = M1
├─ Status = Run
├─ Speed = 100
└─ IsAlarm = false

Machine[1]
├─ Name = M2
├─ Status = Stop
├─ Speed = 0
└─ IsAlarm = true

JSON 是一種具有物件結構的資料交換格式。

JSON 使用 Key-Value 表示資料。

Key 通常是字串。

Value 可以是 string、number、boolean、null、object、array。

JSON 可以用 object 表示一筆資料。

JSON 可以用 array 存放多筆 object。

因此 array 裡的每個 object，
通常可以視為一筆完整資料。

JSON 只有 number。
C# 必須指定 int、double、decimal、long 等明確型別。
反序列化時，C# 會根據 Class 屬性型別轉換。
如果 JSON 數字格式跟 C# 型別不符合，就可能轉換失敗。
實務上會靠 API 文件或 DTO Class 先定義好型別。

