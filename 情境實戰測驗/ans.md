## 情境實戰測驗

### 題目一：活動網頁高流量應對策略

- 使用 CDN 來緩存靜態內容
- 建立 Auto Scaling Group，自動擴容
- 加入負載平衡（如 ALB）
- 優化資料庫查詢，加快回應速度
- 實作快取機制（如 Redis）
- 預先壓力測試（Load Test）

---

### 題目二：單機異常排查方式

- 檢查異常主機的 CPU/Memory/Disk 使用率
- 查詢 application log / system log（dmesg、journalctl）
- 用 `top`, `netstat`, `iostat` 等工具排查資源瓶頸
- 假設與 k8s/ALB 配合，考慮將異常節點移出集群

---

### 題目三：AWS EC2 SSH 無法登入但服務正常

**排查步驟：**
- 嘗試 EC2 Instance Connect
- 使用 SSM Session Manager 登入
- 檢查 sshd 是否異常（可能設定錯誤或資源爆滿）
- 若 ssh key 損壞，可透過 AMI 另起一台來掛載 EBS 修復

**可能原因：**
- `~/.ssh/authorized_keys` 損壞
- `sshd` 設定錯誤或未啟動
- 系統資源用盡導致 SSH 無反應

---

### 題目四：如何將新服務串接至 ELK/EFK

- 新服務需輸出日誌至 STDOUT（適用於 Docker 容器）
- 使用 Fluentd / Fluentbit 收集並轉發至 Elasticsearch
- 確保有正確的 index pattern 讓 Kibana 可搜尋
- 若為非容器應用，使用 Filebeat 撈日誌
- 注意索引大小與 retention policy，避免 ES 爆滿
