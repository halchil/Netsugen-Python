# HadoopとKubernetesでの分散処理 - 類似技術の実装

## 1. Hadoopでの分散処理

### 1.1 Hadoopとは？

Hadoopは、大規模データの分散処理と分散ストレージを提供するオープンソースフレームワークです。

**主なコンポーネント：**
- **HDFS（Hadoop Distributed File System）**: 分散ファイルシステム
- **MapReduce**: 分散処理フレームワーク
- **YARN**: リソース管理システム

### 1.2 HDFSの基本概念

```bash
# HDFSの基本構造
HDFS
├── NameNode (マスターノード)
│   ├── メタデータ管理
│   ├── ファイルシステムの名前空間
│   └── ブロック配置情報
└── DataNode (ワーカーノード)
    ├── 実際のデータ保存
    ├── ブロックの読み書き
    └── ハートビート送信
```

### 1.3 HDFSでのファイル操作

```bash
# HDFSコマンドの基本
# ファイルをHDFSにアップロード
hdfs dfs -put local_file.txt /user/data/

# HDFSからファイルをダウンロード
hdfs dfs -get /user/data/file.txt local_file.txt

# HDFSのファイル一覧を表示
hdfs dfs -ls /user/data/

# ファイルの内容を表示
hdfs dfs -cat /user/data/file.txt

# ファイルを削除
hdfs dfs -rm /user/data/file.txt
```

### 1.4 MapReduceの基本概念

```python
# MapReduceの処理フロー
# 1. Map: データを分割して並列処理
def map_function(key, value):
    # 各マシンで実行
    words = value.split()
    for word in words:
        yield (word, 1)

# 2. Shuffle: 同じキーのデータを集約
# 自動的に実行される

# 3. Reduce: 結果を統合
def reduce_function(key, values):
    # マスターで実行
    return (key, sum(values))
```

### 1.5 HadoopでのWordCount実装

```python
# Hadoop Streamingを使用したWordCount
# mapper.py
#!/usr/bin/env python3
import sys

def mapper():
    for line in sys.stdin:
        # 行を単語に分割
        words = line.strip().split()
        for word in words:
            # 各単語を1回ずつ出力
            print(f"{word}\t1")

if __name__ == "__main__":
    mapper()

# reducer.py
#!/usr/bin/env python3
import sys
from collections import defaultdict

def reducer():
    word_count = defaultdict(int)
    
    for line in sys.stdin:
        word, count = line.strip().split('\t')
        word_count[word] += int(count)
    
    # 結果を出力
    for word, count in word_count.items():
        print(f"{word}\t{count}")

if __name__ == "__main__":
    reducer()
```

### 1.6 Hadoopジョブの実行

```bash
# Hadoop Streamingでジョブを実行
hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
    -input /user/input/text_files \
    -output /user/output/word_count \
    -mapper mapper.py \
    -reducer reducer.py \
    -file mapper.py \
    -file reducer.py

# 結果を確認
hdfs dfs -cat /user/output/word_count/part-*
```

## 2. Kubernetesでの分散処理

### 2.1 Kubernetesとは？

Kubernetesは、コンテナ化されたアプリケーションの自動デプロイ、スケーリング、管理を行うオープンソースプラットフォームです。

**主な特徴：**
- **コンテナオーケストレーション**: 複数コンテナの管理
- **自動スケーリング**: 負荷に応じた自動拡張
- **自己修復**: 障害時の自動復旧
- **ロードバランシング**: トラフィックの分散

### 2.2 Kubernetesの基本構成

```yaml
# Kubernetesの基本構造
Kubernetes Cluster
├── Master Node (コントロールプレーン)
│   ├── API Server
│   ├── etcd
│   ├── Scheduler
│   └── Controller Manager
└── Worker Node (ワーカーノード)
    ├── kubelet
    ├── kube-proxy
    └── Container Runtime
```

### 2.3 データ処理用のPod定義

```yaml
# data-processor-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-processor
  labels:
    app: data-processor
spec:
  containers:
  - name: processor
    image: python:3.9
    command: ["python"]
    args: ["/app/process_data.py"]
    resources:
      requests:
        memory: "1Gi"
        cpu: "500m"
      limits:
        memory: "2Gi"
        cpu: "1000m"
    volumeMounts:
    - name: data-volume
      mountPath: /app/data
    - name: config-volume
      mountPath: /app/config
  volumes:
  - name: data-volume
    persistentVolumeClaim:
      claimName: data-pvc
  - name: config-volume
    configMap:
      name: processor-config
```

### 2.4 分散処理用のDeployment

```yaml
# distributed-processor-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: distributed-processor
spec:
  replicas: 5  # 5つのレプリカを作成
  selector:
    matchLabels:
      app: data-processor
  template:
    metadata:
      labels:
        app: data-processor
    spec:
      containers:
      - name: processor
        image: data-processor:latest
        env:
        - name: WORKER_ID
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: DATA_PARTITION
          value: "{{.Replicas}}"
        resources:
          requests:
            memory: "2Gi"
            cpu: "1000m"
          limits:
            memory: "4Gi"
            cpu: "2000m"
        volumeMounts:
        - name: shared-storage
          mountPath: /shared
      volumes:
      - name: shared-storage
        persistentVolumeClaim:
          claimName: shared-storage-pvc
```

### 2.5 Kubernetesでのデータ処理ジョブ

```yaml
# batch-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: data-processing-job
spec:
  parallelism: 10  # 10個のジョブを並列実行
  completions: 10  # 10個完了まで実行
  template:
    spec:
      containers:
      - name: processor
        image: data-processor:latest
        command: ["python"]
        args: ["/app/process_partition.py"]
        env:
        - name: PARTITION_ID
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        resources:
          requests:
            memory: "1Gi"
            cpu: "500m"
          limits:
            memory: "2Gi"
            cpu: "1000m"
        volumeMounts:
        - name: input-data
          mountPath: /input
        - name: output-data
          mountPath: /output
      volumes:
      - name: input-data
        persistentVolumeClaim:
          claimName: input-data-pvc
      - name: output-data
        persistentVolumeClaim:
          claimName: output-data-pvc
      restartPolicy: Never
```

## 3. 実際の分散処理実装例

### 3.1 Hadoopでのログ分析

```python
# log_analyzer_mapper.py
#!/usr/bin/env python3
import sys
import re
from datetime import datetime

def mapper():
    # ログのパターンを定義
    log_pattern = r'(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) - (\w+) - (\w+) - (.+)'
    
    for line in sys.stdin:
        match = re.match(log_pattern, line.strip())
        if match:
            timestamp, user, action, details = match.groups()
            # 時間帯別に集計
            hour = datetime.strptime(timestamp, '%Y-%m-%d %H:%M:%S').hour
            print(f"{hour}\t1")
            # ユーザー別に集計
            print(f"{user}\t1")
            # アクション別に集計
            print(f"{action}\t1")

if __name__ == "__main__":
    mapper()

# log_analyzer_reducer.py
#!/usr/bin/env python3
import sys
from collections import defaultdict

def reducer():
    current_key = None
    count = 0
    
    for line in sys.stdin:
        key, value = line.strip().split('\t')
        
        if current_key == key:
            count += int(value)
        else:
            if current_key:
                print(f"{current_key}\t{count}")
            current_key = key
            count = int(value)
    
    if current_key:
        print(f"{current_key}\t{count}")

if __name__ == "__main__":
    reducer()
```

### 3.2 Kubernetesでの並列データ処理

```python
# kubernetes_processor.py
import os
import sys
import json
from multiprocessing import Pool
import pandas as pd

def process_partition(partition_data):
    """データの一部を処理"""
    # データ処理ロジック
    result = {
        'partition_id': partition_data['id'],
        'processed_count': len(partition_data['data']),
        'summary': partition_data['data'].describe().to_dict()
    }
    return result

def main():
    # 環境変数から設定を取得
    worker_id = os.environ.get('WORKER_ID', 'unknown')
    partition_id = os.environ.get('PARTITION_ID', '0')
    
    print(f"Worker {worker_id} processing partition {partition_id}")
    
    # データを読み込み
    input_path = f"/input/partition_{partition_id}.csv"
    output_path = f"/output/result_{partition_id}.json"
    
    try:
        # データを処理
        df = pd.read_csv(input_path)
        result = process_partition({
            'id': partition_id,
            'data': df
        })
        
        # 結果を保存
        with open(output_path, 'w') as f:
            json.dump(result, f)
        
        print(f"Partition {partition_id} processed successfully")
        
    except Exception as e:
        print(f"Error processing partition {partition_id}: {e}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

### 3.3 分散処理の監視と管理

```yaml
# monitoring-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: monitoring-dashboard
spec:
  replicas: 1
  selector:
    matchLabels:
      app: monitoring
  template:
    metadata:
      labels:
        app: monitoring
    spec:
      containers:
      - name: dashboard
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
        env:
        - name: GF_SECURITY_ADMIN_PASSWORD
          value: "admin"
        volumeMounts:
        - name: grafana-storage
          mountPath: /var/lib/grafana
      volumes:
      - name: grafana-storage
        persistentVolumeClaim:
          claimName: grafana-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: monitoring-service
spec:
  selector:
    app: monitoring
  ports:
  - port: 3000
    targetPort: 3000
  type: LoadBalancer
```

## 4. パフォーマンス比較

### 4.1 処理時間の比較

```python
# パフォーマンス比較例
import time

# データサイズ: 1TB
# 処理内容: 集計処理

# Hadoop MapReduce
hadoop_time = 180  # 3時間

# Kubernetes分散処理
kubernetes_time = 90  # 1.5時間

# PySpark
pyspark_time = 60  # 1時間

print("=== 処理時間比較 ===")
print(f"Hadoop MapReduce: {hadoop_time}分")
print(f"Kubernetes分散処理: {kubernetes_time}分")
print(f"PySpark: {pyspark_time}分")
```

### 4.2 リソース使用量の比較

```python
# リソース使用量比較
# データサイズ: 1TB

# Hadoop
hadoop_resources = {
    'memory_per_node': '8GB',
    'cpu_per_node': '4 cores',
    'total_nodes': 20,
    'total_memory': '160GB',
    'total_cpu': '80 cores'
}

# Kubernetes
kubernetes_resources = {
    'memory_per_pod': '4GB',
    'cpu_per_pod': '2 cores',
    'total_pods': 25,
    'total_memory': '100GB',
    'total_cpu': '50 cores'
}

# PySpark
pyspark_resources = {
    'memory_per_executor': '4GB',
    'cpu_per_executor': '2 cores',
    'total_executors': 20,
    'total_memory': '80GB',
    'total_cpu': '40 cores'
}
```

## 5. 実践的な選択基準

### 5.1 技術選択のガイドライン

```python
def choose_technology(data_size, processing_type, team_expertise):
    """
    技術選択のガイドライン
    
    Args:
        data_size: データサイズ（GB）
        processing_type: 処理タイプ（'batch', 'streaming', 'interactive'）
        team_expertise: チームの技術レベル（'beginner', 'intermediate', 'expert'）
    
    Returns:
        recommended_technology: 推奨技術
    """
    
    if data_size < 100:  # 100GB未満
        if team_expertise == 'beginner':
            return "Pandas + Python"
        else:
            return "PySpark (ローカルモード)"
    
    elif data_size < 1000:  # 1TB未満
        if processing_type == 'batch':
            return "PySpark"
        elif processing_type == 'streaming':
            return "PySpark Streaming"
        else:
            return "PySpark"
    
    elif data_size < 10000:  # 10TB未満
        if team_expertise == 'expert':
            return "Hadoop + MapReduce"
        else:
            return "PySpark + YARN"
    
    else:  # 10TB以上
        if processing_type == 'batch':
            return "Hadoop + MapReduce"
        else:
            return "Kubernetes + カスタム分散処理"
```

### 5.2 実装例の比較

```python
# 同じ処理を異なる技術で実装

# 1. Pandas（小規模データ）
def pandas_implementation(data):
    df = pd.read_csv(data)
    result = df.groupby('category').agg({
        'value': ['sum', 'mean', 'count']
    })
    return result

# 2. PySpark（中規模データ）
def pyspark_implementation(data):
    df = spark.read.csv(data)
    result = df.groupBy('category').agg(
        sum('value').alias('sum'),
        avg('value').alias('mean'),
        count('*').alias('count')
    )
    return result

# 3. Hadoop MapReduce（大規模データ）
def hadoop_implementation(data):
    # mapper.py と reducer.py を使用
    # hadoop streaming で実行
    pass

# 4. Kubernetes分散処理（カスタム）
def kubernetes_implementation(data):
    # 複数のPodで並列処理
    # 結果を集約
    pass
```

## 6. 練習問題

### 練習1: 技術選択の判断

```python
# 以下の要件で最適な技術を選択してください
# - データサイズ: 500GB
# - 処理タイプ: バッチ処理
# - チームレベル: 中級者
# - 予算: 月30万円以内
# - 処理時間: 2時間以内

def select_technology():
    # 推奨技術と理由を説明してください
    pass
```

### 練習2: 分散処理の設計

```python
# 以下の要件で分散処理システムを設計してください
# - データサイズ: 5TB
# - 処理タイプ: ストリーミング処理
# - 可用性: 99.9%
# - スケーラビリティ: 自動スケーリング

def design_distributed_system():
    # アーキテクチャ設計
    # コンポーネント選択
    # リソース設定
    # 監視・運用設計
    pass
```

## 7. まとめ

この章で学んだこと：
- Hadoopの基本概念と実装
- Kubernetesでの分散処理
- 各技術の特徴と適用場面
- パフォーマンス比較
- 技術選択のガイドライン

次のステップ：
- より高度な分散処理パターン
- クラウド環境での実装
- セキュリティとコンプライアンス
- 運用・監視の自動化

**重要：** 各技術の特徴を理解し、適切な場面で選択することが大切です！ 