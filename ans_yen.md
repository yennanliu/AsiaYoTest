## 綜合應用測驗

### 題目一：找出 `words.txt` 中出現次數最多的單字（不區分大小寫）

**解法：Python 實作**
```python
from collections import Counter
import re

with open('words.txt', 'r') as f:
    text = f.read().lower()
    words = re.findall(r'\b\w+\b', text)
    counter = Counter(words)
    most_common = counter.most_common(1)[0]
    print(f"{most_common[1]} {most_common[0]}")
```

---

### 題目二：Terraform 建立 AWS EKS 並撰寫 K8s manifest

**1. Terraform 建立 EKS 範例主體**
```hcl
resource "aws_eks_cluster" "example" {
  name     = "example-cluster"
  role_arn = aws_iam_role.eks_role.arn

  vpc_config {
    subnet_ids = [aws_subnet.public1.id, aws_subnet.public2.id]
  }

  depends_on = [
    aws_iam_role_policy_attachment.eks_AmazonEKSClusterPolicy,
  ]
}
```

**2. Kubernetes Deployment Manifest 範例**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: your-registry/my-app:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-app-svc
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 80
```

---

### 題目三：SQL 查詢成績第二名學生的班級

**SQL：**
```sql
SELECT c.class
FROM student.score s
JOIN student.class c ON s.name = c.name
ORDER BY s.score DESC
LIMIT 1 OFFSET 1;
```

---

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

