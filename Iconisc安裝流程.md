# VMware 安裝 Windows 11 Pro 與 ICONICS 10.97.3 筆記

## 一、建立 VMware 虛擬機

### 1. 建立虛擬機系統版本

建立 VMware 虛擬機時，作業系統建議安裝：

```text
Windows 11 Pro
```

原因：

```text
Windows 11 Pro 比較適合安裝工控、SCADA、OPC、ICONICS 相關軟體
```

Windows 11 Home 可能會缺少部分管理功能，例如：

```text
本機使用者和群組
遠端桌面
部分系統管理功能
```

---

## 二、安裝 VMware Tools

Windows 11 Pro 安裝完成後，需要安裝：

```text
VMware Tools
```

VMware Tools 主要功能：

| 功能      | 說明            |
| ------- | ------------- |
| 顯示驅動    | 讓畫面解析度正常      |
| 滑鼠整合    | 滑鼠進出虛擬機比較順    |
| 複製貼上    | 主機與虛擬機可複製貼上   |
| 拖曳檔案    | 可在主機與虛擬機之間拖檔案 |
| 網路與硬體支援 | 提升虛擬機穩定性      |

---

## 三、啟用 Administrator 最高權限帳號

在 Windows 安裝或設定階段，可以開啟 CMD。

### 開啟 CMD

```text
Shift + F10
```

如果是筆電鍵盤，可能要按：

```text
Fn + Shift + F10
```

### 啟用內建 Administrator 帳號

在 CMD 輸入：

```cmd
net user Administrator /active:yes
```

意思：

```text
啟用 Windows 內建的 Administrator 系統管理員帳號
```

---

## 四、設定 Administrator 密碼

啟用 Administrator 後，需要設定密碼。

格式：

```cmd
net user Administrator 密碼
```

範例：

```cmd
net user Administrator 123456
```

如果密碼有空白，要加雙引號：

```cmd
net user Administrator "abc 123456"
```

如果不確定帳號名稱，可以先查詢：

```cmd
net user
```

確認有看到：

```text
Administrator
```

再設定密碼。

---

## 五、更換使用者登入

安裝完成後，使用：

```text
Administrator
```

登入 Windows。

登入後，如果原本建立的使用者帳號不需要，可以刪除原本的 User 帳號，只保留 Administrator 帳號使用。

### 注意

刪除原使用者前，要先確認：

```text
1. 重要檔案已經備份
2. Administrator 可以正常登入
3. Administrator 密碼已經記住
```

否則刪除後可能會找不到原本桌面、下載、文件內的檔案。

---

## 六、更新 Windows 系統

進入 Windows 後，需要先更新系統。

路徑：

```text
設定 → Windows Update → 檢查更新
```

將 Windows 更新到最新狀態後，再安裝主要軟體會比較穩定。

---

## 七、確認 VMware NAT 網路

如果 VMware 使用 NAT 模式，但虛擬機無法上網，或 IP 顯示：

```text
169.254.x.x
```

代表虛擬機沒有從 VMware DHCP 拿到正常 IP。

正常 NAT IP 通常會是：

```text
192.168.xxx.xxx
```

### 檢查 VMware 服務

在實體主機 Windows 操作：

```text
Win + R
```

輸入：

```text
services.msc
```

找到以下兩個服務：

```text
VMware DHCP Service
VMware NAT Service
```

確認兩個服務都是：

```text
正在執行
```

如果沒有執行：

```text
右鍵 → 啟動
```

如果已經執行但 NAT 還是不正常，可以：

```text
右鍵 → 重新啟動
```

### 服務用途

| 服務                  | 功能           |
| ------------------- | ------------ |
| VMware DHCP Service | 分配 IP 給虛擬機   |
| VMware NAT Service  | 讓虛擬機透過主機網路上網 |

---

## 八、安裝 ICONICS 10.97.3

Windows 更新完成後，開始安裝：

```text
ICONICS 10.97.3
```

### 安裝主程式

1. 掛載 ISO 檔案
2. 開啟 ISO 內容
3. 執行安裝檔
4. 安裝選項使用：

```text
Default
```

也就是預設安裝。

---

## 九、安裝 ICONICS 插件

主程式安裝完成後，再安裝插件。

### 插件安裝方式

1. 掛載插件 ISO 檔案
2. 開啟 ISO 內容
3. 找到：

```text
main
```

4. 進入 main 資料夾
5. 找到執行安裝檔
6. 執行安裝
7. 依照預設選項安裝即可

---

## 十、簡易安裝順序總結

```text
1. VMware 建立 Windows 11 Pro 虛擬機
2. 安裝 Windows 11 Pro
3. 安裝 VMware Tools
4. 開啟 CMD
5. 啟用 Administrator
6. 設定 Administrator 密碼
7. 使用 Administrator 登入
8. 刪除不需要的原本 User 帳號
9. 更新 Windows 系統
10. 確認 NAT 網路正常
11. 安裝 ICONICS 10.97.3
12. 安裝 ICONICS 插件
```

---

## 十一、常用指令整理

### 啟用 Administrator

```cmd
net user Administrator /active:yes
```

### 設定 Administrator 密碼

```cmd
net user Administrator 123456
```

### 查詢本機帳號

```cmd
net user
```

### NAT 網路重新取得 IP

```cmd
ipconfig /release
ipconfig /renew
ipconfig
```

---

## 十二、重點觀念

```text
169.254.x.x = 沒有拿到 DHCP IP
```

```text
VMnet8 = VMware NAT 使用的虛擬網卡
```

```text
VMware DHCP Service = 負責分配虛擬機 IP
```

```text
VMware NAT Service = 負責讓虛擬機透過主機上網
```

```text
Administrator = Windows 內建最高權限本機管理員帳號
```
