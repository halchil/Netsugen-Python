
## 1. PySparkのセットアップ

### 1.1 仮想環境とインストール

```bash
python -m venv venv
source venv/bin/activate  # Windowsの場合は venv\Scripts\activate
pip install pyspark pandas
```

---

## 2. PySparkの起動と基本操作

```python
from pyspark.sql import SparkSession

# SparkSessionの作成
spark = SparkSession.builder \
    .appName('PySpark入門') \
    .getOrCreate()

print(spark.version)
```

---

## 3. DataFrameの作成と表示

```python
from pyspark.sql import Row

data = [Row(id=1, name='Alice', score=85),
        Row(id=2, name='Bob', score=90),
        Row(id=3, name='Charlie', score=78)]
df = spark.createDataFrame(data)
df.show()
df.printSchema()
```

---

## 4. CSVデータの読み込み・保存

```python
# サンプルCSVの作成（pandasで）
import pandas as pd
sample = pd.DataFrame({
    'id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie'],
    'score': [85, 90, 78]
})
sample.to_csv('sample.csv', index=False)

# PySparkでCSV読み込み
csv_df = spark.read.csv('sample.csv', header=True, inferSchema=True)
csv_df.show()

# 保存
csv_df.write.csv('output_csv', header=True, mode='overwrite')
```

---

## 5. データの基本操作

```python
# 列の選択・フィルタ
csv_df.select('name', 'score').show()
csv_df.filter(csv_df['score'] > 80).show()

# 新しい列の追加
from pyspark.sql.functions import col
csv_df = csv_df.withColumn('passed', col('score') >= 80)
csv_df.show()
```

---

## 6. 集計・グループ化

```python
# グループごとの平均
csv_df.groupBy('passed').avg('score').show()

# 件数カウント
csv_df.groupBy('passed').count().show()
```

---

## 7. 欠損値処理

```python
from pyspark.sql.functions import mean
# 欠損値を含むデータを作成
from pyspark.sql import Row
data2 = [Row(id=1, score=85), Row(id=2, score=None), Row(id=3, score=78)]
df2 = spark.createDataFrame(data2)
df2.show()

# 欠損値の補完（平均値で）
mean_score = df2.select(mean('score')).collect()[0][0]
df2_filled = df2.na.fill({'score': mean_score})
df2_filled.show()
```

---

## 8. データの結合

```python
# 2つのDataFrameを結合
left = spark.createDataFrame([Row(id=1, city='Tokyo'), Row(id=2, city='Osaka')])
right = spark.createDataFrame([Row(id=1, name='Alice'), Row(id=2, name='Bob')])
joined = left.join(right, on='id', how='inner')
joined.show()
```

---

## 9. まとめ

- PySparkの基本操作（起動、DataFrame、CSV入出力、集計、結合、欠損値処理）を体験しました。
- ビッグデータ時代のデータ処理基盤の一端を実感できたと思います。
