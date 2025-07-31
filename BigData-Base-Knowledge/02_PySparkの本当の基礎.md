# PySparkの本当の基礎 - 本当に初歩から始めよう

## 1. PySparkとは？

### 1.1 PySparkの定義

PySparkは、Apache SparkのPython版APIです。大規模データの分散処理をPythonで行うためのライブラリです。

**主な特徴：**
- **高速処理**: メモリ内処理で高速
- **分散処理**: 複数マシンで並列処理
- **多様な処理**: バッチ処理、ストリーミング、機械学習
- **Python API**: 親しみやすいPythonで記述

### 1.2 なぜPySparkが必要なのか？

```python
# 従来のPython（pandas）での問題
import pandas as pd

# 小さなデータなら問題ない
small_df = pd.read_csv('small_data.csv')  # 1GB程度
result = small_df.groupby('category').sum()
print("小さなデータは簡単に処理できます")

# 大きなデータだと...
# large_df = pd.read_csv('huge_data.csv')  # 100GB
# MemoryError: Unable to allocate array
# 処理時間: 数時間〜数日
```

**PySparkの解決策：**
```python
from pyspark.sql import SparkSession

# 同じ処理をPySparkで
spark = SparkSession.builder.getOrCreate()
large_df = spark.read.csv('huge_data.csv')
result = large_df.groupBy('category').sum()
# 処理時間: 数分〜数十分
# メモリ: 分散して使用
```

## 2. まずはPySparkをインストールしてみよう

### 2.1 基本的なインストール

```bash
# 仮想環境を作成
python -m venv pyspark_env

# 仮想環境を有効化
# Windowsの場合
pyspark_env\Scripts\activate
# Mac/Linuxの場合
source pyspark_env/bin/activate

# PySparkをインストール
pip install pyspark

# 確認
python -c "import pyspark; print('PySpark installed successfully!')"
```

### 2.2 SparkSessionとは何か？

SparkSessionは、PySparkアプリケーションの**エントリーポイント**（入り口）です。すべてのSpark機能にアクセスするための統一されたインターフェースを提供します。

#### 2.2.1 SparkSessionの役割

```python
# SparkSessionの基本構造
SparkSession
├── SparkContext: 分散処理の基本機能
├── SQLContext: SQLクエリの実行
├── HiveContext: Hiveテーブルの操作
└── StreamingContext: ストリーミング処理
```

**なぜSparkSessionが必要なのか？**

1. **統一されたインターフェース**
   ```python
   # 従来は複数のコンテキストが必要だった
   from pyspark import SparkContext
   from pyspark.sql import SQLContext
   from pyspark.sql import HiveContext
   
   sc = SparkContext()
   sqlContext = SQLContext(sc)
   hiveContext = HiveContext(sc)
   
   # SparkSessionなら一つで済む
   from pyspark.sql import SparkSession
   spark = SparkSession.builder.getOrCreate()
   ```

2. **設定の一元管理**
   ```python
   # アプリケーション全体の設定を一箇所で管理
   spark = SparkSession.builder \
       .appName("MyApp") \
       .config("spark.driver.memory", "2g") \
       .config("spark.executor.memory", "4g") \
       .getOrCreate()
   ```

3. **リソースの効率的な管理**
   ```python
   # セッションの開始
   spark = SparkSession.builder.getOrCreate()
   
   # 処理実行
   df = spark.read.csv('data.csv')
   result = df.groupBy('category').count()
   
   # セッションの終了（リソース解放）
   spark.stop()
   ```

#### 2.2.2 SparkSessionの作成方法

```python
from pyspark.sql import SparkSession

# 最も基本的な作成方法
spark = SparkSession.builder \
    .appName("MyFirstPySpark") \
    .getOrCreate()

# バージョン確認
print("Spark version:", spark.version)
print("SparkSession created successfully!")

# セッションを閉じる
spark.stop()
```

**各設定項目の説明：**

- `.builder`: SparkSessionのビルダーを開始
- `.appName("MyFirstPySpark")`: アプリケーション名を設定（WebUIで表示される）
- `.getOrCreate()`: 既存のセッションがあれば取得、なければ新規作成

#### 2.2.3 SparkSessionの詳細設定

```python
from pyspark.sql import SparkSession

# より詳細な設定
spark = SparkSession.builder \
    .appName("DetailedConfigApp") \
    .master("local[*]") \  # ローカルモードで全コア使用
    .config("spark.driver.memory", "2g") \  # ドライバーメモリ
    .config("spark.executor.memory", "4g") \  # エグゼキュータメモリ
    .config("spark.sql.adaptive.enabled", "true") \  # 適応的クエリ実行
    .getOrCreate()

print("詳細設定でSparkSessionを作成しました")
print("アプリケーション名:", spark.conf.get("spark.app.name"))
print("マスターモード:", spark.conf.get("spark.master"))
```

#### 2.2.4 SparkSessionの状態確認

```python
from pyspark.sql import SparkSession

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("StatusCheckApp") \
    .getOrCreate()

# セッション情報の確認
print("=== SparkSession情報 ===")
print("アプリケーション名:", spark.conf.get("spark.app.name"))
print("Sparkバージョン:", spark.version)
print("利用可能なコア数:", spark.sparkContext.defaultParallelism)

# リソース情報の確認
print("\n=== リソース情報 ===")
print("ドライバーメモリ:", spark.conf.get("spark.driver.memory"))
print("エグゼキュータメモリ:", spark.conf.get("spark.executor.memory"))

# セッションを閉じる
spark.stop()
```

**実行してみてください：**
- エラーが出なければ成功です
- エラーが出た場合は、インストールを確認してください

## 3. 最初の一歩：簡単なDataFrameを作ってみよう

### 3.1 基本的なDataFrameの作成

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("FirstDataFrame") \
    .getOrCreate()

# 簡単なデータを作成
data = [
    Row(name="田中", age=25, department="営業部"),
    Row(name="佐藤", age=30, department="開発部"),
    Row(name="鈴木", age=28, department="営業部")
]

# DataFrameを作成
df = spark.createDataFrame(data)

# DataFrameを表示
print("DataFrameの内容:")
df.show()

# スキーマ（データ構造）を確認
print("\nDataFrameのスキーマ:")
df.printSchema()

# セッションを閉じる
spark.stop()
```

**実行結果例：**
```
DataFrameの内容:
+----+---+----------+
|name|age|department|
+----+---+----------+
|田中| 25|    営業部|
|佐藤| 30|    開発部|
|鈴木| 28|    営業部|
+----+---+----------+

DataFrameのスキーマ:
root
 |-- name: string (nullable = true)
 |-- age: long (nullable = true)
 |-- department: string (nullable = true)
```

## 4. DataFrameの基本操作

### 4.1 データの表示と確認

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("DataFrameBasics") \
    .getOrCreate()

# サンプルデータを作成
data = [
    Row(id=1, name="田中", age=25, salary=300000),
    Row(id=2, name="佐藤", age=30, salary=400000),
    Row(id=3, name="鈴木", age=28, salary=350000),
    Row(id=4, name="高橋", age=35, salary=450000),
    Row(id=5, name="渡辺", age=27, salary=320000)
]

df = spark.createDataFrame(data)

# 基本的な情報を確認
print("DataFrameの行数:", df.count())
print("DataFrameの列数:", len(df.columns))
print("列名:", df.columns)

# 最初の3行を表示
print("\n最初の3行:")
df.show(3)

# 最後の2行を表示
print("\n最後の2行:")
df.tail(2)
```

### 4.2 列の選択とフィルタリング

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("DataSelection") \
    .getOrCreate()

# サンプルデータを作成
data = [
    Row(id=1, name="田中", age=25, salary=300000),
    Row(id=2, name="佐藤", age=30, salary=400000),
    Row(id=3, name="鈴木", age=28, salary=350000),
    Row(id=4, name="高橋", age=35, salary=450000),
    Row(id=5, name="渡辺", age=27, salary=320000)
]

df = spark.createDataFrame(data)

# 特定の列を選択
print("名前と年齢のみ表示:")
df.select("name", "age").show()

# 条件でフィルタリング
print("\n30歳以上の従業員:")
df.filter(df.age >= 30).show()

# 複数条件
print("\n30歳以上かつ給与35万円以上:")
df.filter((df.age >= 30) & (df.salary >= 350000)).show()
```

## 5. 基本的な集計操作

### 5.1 統計情報の取得

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("BasicAggregation") \
    .getOrCreate()

# サンプルデータを作成
data = [
    Row(id=1, name="田中", age=25, salary=300000),
    Row(id=2, name="佐藤", age=30, salary=400000),
    Row(id=3, name="鈴木", age=28, salary=350000),
    Row(id=4, name="高橋", age=35, salary=450000),
    Row(id=5, name="渡辺", age=27, salary=320000)
]

df = spark.createDataFrame(data)

# 基本的な統計
print("年齢の統計:")
df.describe("age").show()

print("\n給与の統計:")
df.describe("salary").show()

# 平均値の計算
from pyspark.sql.functions import avg, count, max, min

print("\n平均年齢:", df.select(avg("age")).collect()[0][0])
print("平均給与:", df.select(avg("salary")).collect()[0][0])
print("最高給与:", df.select(max("salary")).collect()[0][0])
print("最低給与:", df.select(min("salary")).collect()[0][0])
```

### 5.2 グループ化と集計

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql.functions import avg, count, sum

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("GroupByAggregation") \
    .getOrCreate()

# 部署情報を追加したデータを作成
data = [
    Row(name="田中", age=25, salary=300000, department="営業部"),
    Row(name="佐藤", age=30, salary=400000, department="開発部"),
    Row(name="鈴木", age=28, salary=350000, department="営業部"),
    Row(name="高橋", age=35, salary=450000, department="人事部"),
    Row(name="渡辺", age=27, salary=320000, department="開発部"),
    Row(name="伊藤", age=32, salary=380000, department="営業部")
]

df = spark.createDataFrame(data)

# 部署別の統計
print("部署別従業員数:")
df.groupBy("department").count().show()

print("\n部署別平均年齢:")
df.groupBy("department").avg("age").show()

print("\n部署別平均給与:")
df.groupBy("department").avg("salary").show()

# 複数の集計を一度に
print("\n部署別の詳細統計:")
df.groupBy("department").agg(
    count("*").alias("従業員数"),
    avg("age").alias("平均年齢"),
    avg("salary").alias("平均給与"),
    sum("salary").alias("給与合計")
).show()
```

## 6. データの読み込みと保存

### 6.1 CSVファイルの読み込み

```python
from pyspark.sql import SparkSession
import pandas as pd

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("CSVReading") \
    .getOrCreate()

# まず、サンプルCSVファイルを作成（pandasで）
sample_data = pd.DataFrame({
    'id': [1, 2, 3, 4, 5],
    'name': ['田中', '佐藤', '鈴木', '高橋', '渡辺'],
    'age': [25, 30, 28, 35, 27],
    'salary': [300000, 400000, 350000, 450000, 320000],
    'department': ['営業部', '開発部', '営業部', '人事部', '開発部']
})

# CSVファイルとして保存
sample_data.to_csv('employees.csv', index=False, encoding='utf-8')
print("サンプルCSVファイルを作成しました: employees.csv")

# PySparkでCSVファイルを読み込み
df = spark.read.csv('employees.csv', header=True, inferSchema=True)

print("\n読み込んだデータ:")
df.show()

print("\nスキーマ:")
df.printSchema()

# セッションを閉じる
spark.stop()
```

### 6.2 データの保存

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("DataSaving") \
    .getOrCreate()

# サンプルデータを作成
data = [
    Row(id=1, name="田中", age=25, salary=300000),
    Row(id=2, name="佐藤", age=30, salary=400000),
    Row(id=3, name="鈴木", age=28, salary=350000)
]

df = spark.createDataFrame(data)

# CSVファイルとして保存
df.write.csv('output_data', header=True, mode='overwrite')
print("データをCSVファイルとして保存しました: output_data/")

# Parquetファイルとして保存（より効率的）
df.write.parquet('output_data.parquet', mode='overwrite')
print("データをParquetファイルとして保存しました: output_data.parquet")

# セッションを閉じる
spark.stop()
```

## 7. 練習問題

### 練習1: 自分のDataFrameを作ってみよう

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("MyFirstDataFrame") \
    .getOrCreate()

# あなたの好きなデータでDataFrameを作ってください
my_data = [
    Row(name="あなたの名前", age=あなたの年齢, hobby="あなたの趣味"),
    # ここにデータを追加してください
]

my_df = spark.createDataFrame(my_data)
print("私のDataFrame:")
my_df.show()

# 基本的な統計を計算
print("\n年齢の統計:")
my_df.describe("age").show()

# セッションを閉じる
spark.stop()
```

### 練習2: データの分析をしてみよう

```python
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql.functions import avg, count

# SparkSessionを作成
spark = SparkSession.builder \
    .appName("DataAnalysis") \
    .getOrCreate()

# サンプルデータ
data = [
    Row(product="りんご", price=100, category="果物"),
    Row(product="バナナ", price=80, category="果物"),
    Row(product="トマト", price=120, category="野菜"),
    Row(product="キャベツ", price=150, category="野菜"),
    Row(product="オレンジ", price=90, category="果物")
]

df = spark.createDataFrame(data)

# 以下の分析を実行してください
print("1. 全商品の平均価格:")
# ここにコードを書いてください

print("\n2. カテゴリ別の商品数:")
# ここにコードを書いてください

print("\n3. カテゴリ別の平均価格:")
# ここにコードを書いてください

# セッションを閉じる
spark.stop()
```

## 8. よくあるエラーと対処法

### エラー1: ModuleNotFoundError
```
ModuleNotFoundError: No module named 'pyspark'
```
**対処法：** PySparkをインストールしてください
```bash
pip install pyspark
```

### エラー2: JavaNotFoundError
```
Java is not installed or not in PATH
```
**対処法：** Javaをインストールしてください
```bash
# Windows: https://adoptium.net/ からダウンロード
# Mac: brew install openjdk
# Linux: sudo apt-get install openjdk-11-jdk
```

### エラー3: メモリ不足
```
java.lang.OutOfMemoryError: Java heap space
```
**対処法：** メモリ設定を調整
```python
spark = SparkSession.builder \
    .appName("MyApp") \
    .config("spark.driver.memory", "2g") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()
```

### エラー4: ファイルが見つからない
```
java.io.FileNotFoundException
```
**対処法：** ファイルパスを確認
```python
# 絶対パスを使用
df = spark.read.csv('/full/path/to/file.csv', header=True)
```

## 9. まとめ

この章で学んだこと：
- PySparkの基本概念
- SparkSessionの作成と管理
- DataFrameの基本操作
- データの読み込み・保存
- 基本的な集計処理
- エラーの対処法

次のステップ：
- より複雑なデータ処理
- 分散処理の詳細
- パフォーマンス最適化
- 機械学習との組み合わせ

**重要：** 実際にコードを実行してみることが大切です！ 