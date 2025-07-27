
## 実践演習：データ操作の基礎

### 演習の準備

まず、Pythonの実行環境を確認しましょう。

```python
# Pythonのバージョンを確認
import sys
print(f"Python version: {sys.version}")

# 基本的な計算
print("=== 基本的な計算 ===")
a = 10
b = 3
print(f"a = {a}, b = {b}")
print(f"加算: {a} + {b} = {a + b}")
print(f"減算: {a} - {b} = {a - b}")
print(f"乗算: {a} * {b} = {a * b}")
print(f"除算: {a} / {b} = {a / b}")
print(f"整数除算: {a} // {b} = {a // b}")
print(f"剰余: {a} % {b} = {a % b}")
print(f"べき乗: {a} ** {b} = {a ** b}")
```

### 演習1：文字列操作

#### 1.1 基本的な文字列操作

```python
print("\n=== 文字列操作演習 ===")

# 文字列の作成
name = "田中太郎"
company = "データ分析株式会社"
department = "IT部門"

print(f"名前: {name}")
print(f"会社: {company}")
print(f"部署: {department}")

# 文字列の連結
full_info = name + " - " + company + " - " + department
print(f"連結結果: {full_info}")

# f-stringを使った文字列フォーマット
formatted_info = f"{name}は{company}の{department}に所属しています。"
print(f"フォーマット結果: {formatted_info}")

# 文字列の長さ
print(f"名前の文字数: {len(name)}")
print(f"会社名の文字数: {len(company)}")

# 文字列の部分取得
print(f"名前の最初の文字: {name[0]}")
print(f"名前の最後の文字: {name[-1]}")
print(f"名前の最初の2文字: {name[:2]}")
print(f"名前の最後の2文字: {name[-2:]}")
```

#### 1.2 文字列メソッドの活用

```python
print("\n=== 文字列メソッド演習 ===")

# サンプルテキスト
text = "  Pythonデータ分析入門  "
print(f"元のテキスト: '{text}'")

# 空白の除去
cleaned_text = text.strip()
print(f"空白除去後: '{cleaned_text}'")

# 大文字・小文字の変換
sample_text = "Hello World"
print(f"元のテキスト: {sample_text}")
print(f"大文字: {sample_text.upper()}")
print(f"小文字: {sample_text.lower()}")

# 文字列の分割
csv_data = "田中,30,東京,エンジニア"
fields = csv_data.split(",")
print(f"分割前: {csv_data}")
print(f"分割後: {fields}")

# 文字列の結合
words = ["Python", "データ", "分析", "入門"]
joined_text = " ".join(words)
print(f"結合前: {words}")
print(f"結合後: {joined_text}")

# 文字列の置換
original = "Pythonは素晴らしい言語です。Pythonでデータ分析をしましょう。"
replaced = original.replace("Python", "Python3")
print(f"置換前: {original}")
print(f"置換後: {replaced}")
```

### 演習2：リスト操作

#### 2.1 基本的なリスト操作

```python
print("\n=== リスト操作演習 ===")

# リストの作成
numbers = [1, 2, 3, 4, 5]
names = ["田中", "佐藤", "鈴木", "高橋", "渡辺"]
mixed = [1, "Python", 3.14, True, [1, 2, 3]]

print(f"数値リスト: {numbers}")
print(f"名前リスト: {names}")
print(f"混合リスト: {mixed}")

# リストの長さ
print(f"数値リストの長さ: {len(numbers)}")
print(f"名前リストの長さ: {len(names)}")

# リストの要素アクセス
print(f"数値リストの最初の要素: {numbers[0]}")
print(f"数値リストの最後の要素: {numbers[-1]}")
print(f"数値リストの2番目から4番目: {numbers[1:4]}")

# リストの要素変更
numbers[0] = 10
print(f"変更後の数値リスト: {numbers}")

# リストへの要素追加
numbers.append(6)
print(f"append後の数値リスト: {numbers}")

numbers.insert(0, 0)
print(f"insert後の数値リスト: {numbers}")

# リストからの要素削除
removed = numbers.pop()
print(f"popで削除された要素: {removed}")
print(f"pop後の数値リスト: {numbers}")

numbers.remove(3)
print(f"remove後の数値リスト: {numbers}")
```

#### 2.2 リスト内包表記

```python
print("\n=== リスト内包表記演習 ===")

# 基本的なリスト内包表記
squares = [x**2 for x in range(10)]
print(f"0-9の二乗: {squares}")

# 条件付きリスト内包表記
even_squares = [x**2 for x in range(10) if x % 2 == 0]
print(f"偶数の二乗: {even_squares}")

# 文字列のリスト内包表記
words = ["python", "data", "analysis", "machine", "learning"]
upper_words = [word.upper() for word in words]
print(f"元の単語: {words}")
print(f"大文字変換後: {upper_words}")

# 長い単語のみ抽出
long_words = [word for word in words if len(word) > 5]
print(f"長い単語（5文字以上）: {long_words}")

# 複数の条件を組み合わせ
filtered_words = [word.upper() for word in words if len(word) > 4 and 'a' in word]
print(f"条件付きフィルタリング: {filtered_words}")
```

### 演習3：辞書操作

#### 3.1 基本的な辞書操作

```python
print("\n=== 辞書操作演習 ===")

# 辞書の作成
employee = {
    "name": "田中太郎",
    "age": 30,
    "department": "IT部門",
    "salary": 400000,
    "skills": ["Python", "SQL", "機械学習"]
}

print(f"従業員情報: {employee}")

# 値の取得
name = employee["name"]
age = employee.get("age", 0)
print(f"名前: {name}")
print(f"年齢: {age}")

# 存在しないキーの安全な取得
phone = employee.get("phone", "未設定")
print(f"電話番号: {phone}")

# 辞書の更新
employee["age"] = 31
employee["phone"] = "090-1234-5678"
print(f"更新後の従業員情報: {employee}")

# キーの存在確認
if "email" in employee:
    print("メールアドレスが設定されています")
else:
    print("メールアドレスが設定されていません")

# 辞書の反復処理
print("\n辞書の内容を表示:")
for key, value in employee.items():
    print(f"{key}: {value}")

# キーのみ取得
print(f"\nキーの一覧: {list(employee.keys())}")

# 値のみ取得
print(f"値の一覧: {list(employee.values())}")
```

#### 3.2 辞書の応用

```python
print("\n=== 辞書の応用演習 ===")

# 複数の従業員データ
employees = {
    "emp001": {
        "name": "田中太郎",
        "age": 30,
        "department": "IT部門",
        "salary": 400000
    },
    "emp002": {
        "name": "佐藤花子",
        "age": 28,
        "department": "営業部門",
        "salary": 350000
    },
    "emp003": {
        "name": "鈴木一郎",
        "age": 35,
        "department": "IT部門",
        "salary": 450000
    }
}

print("全従業員の情報:")
for emp_id, emp_info in employees.items():
    print(f"{emp_id}: {emp_info['name']} ({emp_info['department']})")

# IT部門の従業員のみ抽出
it_employees = {emp_id: emp_info for emp_id, emp_info in employees.items() 
                if emp_info['department'] == 'IT部門'}
print(f"\nIT部門の従業員: {it_employees}")

# 平均年齢の計算
total_age = sum(emp_info['age'] for emp_info in employees.values())
avg_age = total_age / len(employees)
print(f"平均年齢: {avg_age:.1f}歳")

# 部署別の人数集計
dept_count = {}
for emp_info in employees.values():
    dept = emp_info['department']
    dept_count[dept] = dept_count.get(dept, 0) + 1

print(f"部署別人数: {dept_count}")
```

### 演習4：制御構文の実践

#### 4.1 条件分岐の実践

```python
print("\n=== 条件分岐演習 ===")

# 成績判定システム
def evaluate_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    elif score >= 60:
        return "D"
    else:
        return "F"

# テスト
test_scores = [95, 85, 75, 65, 55]
for score in test_scores:
    grade = evaluate_grade(score)
    print(f"点数: {score} → 評価: {grade}")

# 年齢による料金計算
def calculate_fee(age, is_student=False):
    if age < 6:
        return 0
    elif age < 12:
        return 500
    elif age < 18:
        return 800 if not is_student else 600
    elif age < 65:
        return 1200
    else:
        return 800

# テスト
test_cases = [
    (5, False), (10, False), (15, False), (15, True),
    (25, False), (70, False)
]

for age, is_student in test_cases:
    fee = calculate_fee(age, is_student)
    student_text = "（学生）" if is_student else ""
    print(f"年齢: {age}歳{student_text} → 料金: {fee}円")
```

#### 4.2 繰り返し処理の実践

```python
print("\n=== 繰り返し処理演習 ===")

# 数値の合計計算
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
total = 0
for num in numbers:
    total += num
print(f"数値リスト: {numbers}")
print(f"合計: {total}")

# 偶数のみの合計
even_sum = sum(num for num in numbers if num % 2 == 0)
print(f"偶数の合計: {even_sum}")

# 九九の表（一部）
print("\n九九の表（3の段）:")
for i in range(1, 10):
    result = 3 * i
    print(f"3 × {i} = {result}")

# データの統計情報
data = [23, 45, 67, 89, 12, 34, 56, 78, 90, 11]
print(f"\nデータ: {data}")
print(f"データ数: {len(data)}")
print(f"最大値: {max(data)}")
print(f"最小値: {min(data)}")
print(f"平均値: {sum(data) / len(data):.2f}")

# データの分類
small_numbers = []
large_numbers = []
for num in data:
    if num < 50:
        small_numbers.append(num)
    else:
        large_numbers.append(num)

print(f"50未満: {small_numbers}")
print(f"50以上: {large_numbers}")
```

### 演習5：関数の作成と活用

#### 5.1 基本的な関数

```python
print("\n=== 関数作成演習 ===")

# 平均値を計算する関数
def calculate_average(numbers):
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)

# テスト
test_data = [10, 20, 30, 40, 50]
avg = calculate_average(test_data)
print(f"データ: {test_data}")
print(f"平均値: {avg}")

# 文字列を処理する関数
def process_text(text):
    # 空白を除去し、小文字に変換
    cleaned = text.strip().lower()
    # 単語に分割
    words = cleaned.split()
    # 単語数を返す
    return len(words)

# テスト
sample_texts = [
    "  Python データ分析入門  ",
    "Hello World",
    "  複数の  空白が  ある  テキスト  "
]

for text in sample_texts:
    word_count = process_text(text)
    print(f"テキスト: '{text}' → 単語数: {word_count}")

# データの統計情報を計算する関数
def analyze_data(data):
    if not data:
        return {
            "count": 0,
            "sum": 0,
            "average": 0,
            "min": None,
            "max": None
        }
    
    return {
        "count": len(data),
        "sum": sum(data),
        "average": sum(data) / len(data),
        "min": min(data),
        "max": max(data)
    }

# テスト
analysis = analyze_data(test_data)
print(f"\nデータ分析結果:")
for key, value in analysis.items():
    print(f"{key}: {value}")
```

### 演習6：エラーハンドリング

#### 6.1 基本的なエラーハンドリング

```python
print("\n=== エラーハンドリング演習 ===")

# 安全な数値変換関数
def safe_int_conversion(value):
    try:
        return int(value)
    except ValueError:
        print(f"エラー: '{value}' は数値に変換できません")
        return None

# テスト
test_values = ["123", "abc", "45.6", "789", "xyz"]
for value in test_values:
    result = safe_int_conversion(value)
    if result is not None:
        print(f"'{value}' → {result}")
    else:
        print(f"'{value}' → 変換失敗")

# 安全な除算関数
def safe_division(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        print("エラー: ゼロで割ることはできません")
        return None
    except TypeError:
        print("エラー: 数値以外の値が渡されました")
        return None

# テスト
test_cases = [
    (10, 2),
    (10, 0),
    (10, "abc"),
    (15, 3)
]

for a, b in test_cases:
    result = safe_division(a, b)
    if result is not None:
        print(f"{a} ÷ {b} = {result}")
    else:
        print(f"{a} ÷ {b} = 計算失敗")

# ファイル読み込みのエラーハンドリング
def read_file_safely(filename):
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            content = file.read()
            return content
    except FileNotFoundError:
        print(f"エラー: ファイル '{filename}' が見つかりません")
        return None
    except PermissionError:
        print(f"エラー: ファイル '{filename}' にアクセスする権限がありません")
        return None
    except Exception as e:
        print(f"予期しないエラーが発生しました: {e}")
        return None

# テスト（存在しないファイル）
result = read_file_safely("nonexistent_file.txt")
if result is None:
    print("ファイル読み込みに失敗しました")
```

### 演習のまとめ

この演習を通じて、以下のスキルを身につけました：

1. **基本的なデータ型の操作**
   - 文字列の加工と処理
   - リストの作成と操作
   - 辞書を使ったデータ構造化

2. **制御構文の活用**
   - 条件分岐によるデータの分類
   - 繰り返し処理による大量データの処理

3. **関数の作成と活用**
   - 再利用可能な処理の作成
   - パラメータと戻り値の活用

4. **エラーハンドリング**
   - 予期しない状況への対応
   - ロバストなプログラムの作成

これらの基礎スキルは、次のModuleで学ぶNumPyやPandasなどの高度なデータ処理ライブラリを理解するための重要な基盤となります。実践的な演習を通じて、理論的な知識を実際のスキルに変換することができました。
