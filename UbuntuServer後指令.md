whoami
sudo whoami
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
lsb_release -a
hostname -I
ping google.com
dpkg -l | grep openssh-server
systemctl status ssh


| 項目      | 指令                                   | 用途          |
| ------- | ------------------------------------ | ----------- |
| 確認帳號    | `whoami`                             | 看目前登入者      |
| 確認 sudo | `sudo whoami`                        | 看是否有管理員權限   |
| 更新清單    | `sudo apt update`                    | 更新套件清單      |
| 升級套件    | `sudo apt upgrade -y`                | 更新已安裝軟體     |
| 清理套件    | `sudo apt autoremove -y`             | 移除不需要套件     |
| 查版本     | `lsb_release -a`                     | 看 Ubuntu 版本 |
| 查 IP    | `hostname -I`                        | 看目前 IP      |
| 測網路     | `ping google.com`                    | 測試能否連外      |
| 查 SSH   | `systemctl status ssh`               | 確認 SSH 是否啟動 |
| 安裝 SSH  | `sudo apt install openssh-server -y` | 安裝遠端登入功能    |
| 重開機     | `sudo reboot`                        | 重新啟動系統      |
關機是sudo poweroff

| 指令                     | 用途   |
| ---------------------- | ---- |
| `sudo poweroff`        | 立即關機 |
| `sudo shutdown now`    | 立即關機 |
| `sudo shutdown -h now` | 立即關機 |
| `sudo reboot`          | 重新開機 |
