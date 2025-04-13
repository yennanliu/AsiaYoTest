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
