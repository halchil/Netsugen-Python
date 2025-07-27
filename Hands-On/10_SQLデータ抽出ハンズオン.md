
## 1. 準備

### 1.1 仮想環境とパッケージインストール

```bash
python -m venv venv
source venv/bin/activate  # Windowsの場合は venv\Scripts\activate
pip install pandas sqlite3
```

---

## 2. データベースとテーブルの作成

```python
import sqlite3
conn = sqlite3.connect('sample.db')
cursor = conn.cursor()

# テーブル作成
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    age INTEGER,
    city TEXT
)
''')
cursor.execute('''
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    amount INTEGER,
    date TEXT,
    FOREIGN KEY(user_id) REFERENCES users(id)
)
''')
conn.commit()
```

---

## 3. データの挿入

```python
# サンプルデータ挿入
users = [
    (1, 'Alice', 25, 'Tokyo'),
    (2, 'Bob', 32, 'Osaka'),
    (3, 'Charlie', 29, 'Nagoya')
]
sales = [
    (1, 1, 1200, '2023-01-10'),
    (2, 2, 800, '2023-01-12'),
    (3, 1, 1500, '2023-02-05'),
    (4, 3, 700, '2023-02-10')
]
cursor.executemany('INSERT OR IGNORE INTO users VALUES (?, ?, ?, ?)', users)
cursor.executemany('INSERT OR IGNORE INTO sales VALUES (?, ?, ?, ?)', sales)
conn.commit()
```

---

## 4. SELECT文によるデータ抽出

```python
# 全件取得
for row in cursor.execute('SELECT * FROM users'):
    print(row)

# 条件指定
for row in cursor.execute('SELECT name, age FROM users WHERE age > 28'):
    print(row)
```

---

## 5. GROUP BY・集計

```python
for row in cursor.execute('SELECT city, COUNT(*) FROM users GROUP BY city'):
    print(row)
```

---

## 6. JOINによるテーブル結合

```python
for row in cursor.execute('''
SELECT u.name, s.amount, s.date
FROM users u
JOIN sales s ON u.id = s.user_id
'''):
    print(row)
```

---

## 7. pandasとの連携

```python
import pandas as pd
df = pd.read_sql_query('SELECT u.name, s.amount, s.date FROM users u JOIN sales s ON u.id = s.user_id', conn)
print(df)
```

---

## 8. まとめ

- sqlite3でDB作成・テーブル作成・データ挿入・抽出・集計・結合を体験しました。
- pandasと連携することで、SQL＋Pythonの強力なデータ分析基盤を構築できます。

# 後片付け
```python
conn.close()
``` 