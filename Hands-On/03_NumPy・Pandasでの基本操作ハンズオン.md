
## 実践演習：データ操作の基礎

### 演習の準備

まず、必要なライブラリをインポートし、サンプルデータを作成しましょう。

```python
# 必要なライブラリのインポート
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 日本語フォントの設定（必要に応じて）
plt.rcParams['font.family'] = 'DejaVu Sans'
# Windowsの場合: plt.rcParams['font.family'] = 'MS Gothic'

# サンプルデータの作成
np.random.seed(42)

# 従業員データの作成
employee_data = {
    'employee_id': range(1, 101),
    'name': [f'従業員{i:03d}' for i in range(1, 101)],
    'department': np.random.choice(['営業部', '開発部', '人事部', '経理部', 'マーケティング部'], 100),
    'age': np.random.normal(35, 8, 100).astype(int),
    'salary': np.random.normal(400000, 80000, 100).astype(int),
    'years_of_service': np.random.exponential(5, 100).astype(int),
    'performance_score': np.random.normal(75, 15, 100).round(1)
}

# DataFrameの作成
df = pd.DataFrame(employee_data)

# データの確認
print("データの基本情報:")
print(df.info())
print("\n最初の5行:")
print(df.head())
print("\n統計的概要:")
print(df.describe())
```

### 演習1：NumPy配列の操作

#### 1.1 基本的な配列操作

```python
print("=== NumPy配列の基本操作 ===")

# 年齢データをNumPy配列として取得
ages = df['age'].values
print(f"年齢データの型: {type(ages)}")
print(f"年齢データの形状: {ages.shape}")
print(f"年齢データ: {ages[:10]}")  # 最初の10個

# 基本的な統計計算
print(f"\n年齢の統計:")
print(f"平均年齢: {np.mean(ages):.1f}歳")
print(f"中央値: {np.median(ages):.1f}歳")
print(f"標準偏差: {np.std(ages):.1f}歳")
print(f"最小値: {np.min(ages)}歳")
print(f"最大値: {np.max(ages)}歳")

# 配列の操作
print(f"\n配列操作:")
print(f"最初の5要素: {ages[:5]}")
print(f"最後の5要素: {ages[-5:]}")
print(f"偶数インデックスの要素: {ages[::2][:5]}")
print(f"30歳以上の人数: {np.sum(ages >= 30)}人")
```

#### 1.2 2次元配列の操作

```python
print("\n=== 2次元配列の操作 ===")

# 年齢と給与の2次元配列を作成
age_salary = df[['age', 'salary']].values
print(f"年齢・給与データの形状: {age_salary.shape}")
print(f"最初の5行:\n{age_salary[:5]}")

# 列ごとの統計
print(f"\n年齢列の統計:")
print(f"平均: {np.mean(age_salary[:, 0]):.1f}")
print(f"標準偏差: {np.std(age_salary[:, 0]):.1f}")

print(f"\n給与列の統計:")
print(f"平均: {np.mean(age_salary[:, 1]):,.0f}円")
print(f"標準偏差: {np.std(age_salary[:, 1]):,.0f}円")

# 条件付きフィルタリング
high_performers = age_salary[df['performance_score'] > 80]
print(f"\n高パフォーマンス従業員（80点以上）の数: {len(high_performers)}人")
print(f"高パフォーマンス従業員の平均年齢: {np.mean(high_performers[:, 0]):.1f}歳")
print(f"高パフォーマンス従業員の平均給与: {np.mean(high_performers[:, 1]):,.0f}円")
```

### 演習2：Pandas DataFrameの操作

#### 2.1 データの表示と確認

```python
print("=== DataFrameの基本操作 ===")

# データの基本情報
print("データの形状:", df.shape)
print("列名:", df.columns.tolist())
print("データ型:\n", df.dtypes)

# 各列の基本統計
print("\n各列の基本統計:")
print(df.describe())

# 欠損値の確認
print("\n欠損値の確認:")
print(df.isnull().sum())

# 重複データの確認
print(f"\n重複データの数: {df.duplicated().sum()}")
```

#### 2.2 データの選択とフィルタリング

```python
print("\n=== データの選択とフィルタリング ===")

# 特定の列の選択
print("名前と年齢のみ表示:")
print(df[['name', 'age']].head())

# 条件付きフィルタリング
print("\n30歳未満の従業員:")
young_employees = df[df['age'] < 30]
print(young_employees[['name', 'age', 'department']].head())

print("\n給与が40万円以上の従業員:")
high_salary = df[df['salary'] >= 400000]
print(high_salary[['name', 'salary', 'department']].head())

# 複数条件のフィルタリング
print("\n開発部で30歳以上の従業員:")
dev_senior = df[(df['department'] == '開発部') & (df['age'] >= 30)]
print(dev_senior[['name', 'age', 'salary', 'years_of_service']].head())

# 文字列フィルタリング
print("\n名前が'従業員01'で始まる従業員:")
employee_01 = df[df['name'].str.startswith('従業員01')]
print(employee_01[['name', 'department', 'age']])
```

#### 2.3 データのソート

```python
print("\n=== データのソート ===")

# 年齢でソート（昇順）
print("年齢順（昇順）:")
age_sorted = df.sort_values('age')
print(age_sorted[['name', 'age', 'department']].head())

# 給与でソート（降順）
print("\n給与順（降順）:")
salary_sorted = df.sort_values('salary', ascending=False)
print(salary_sorted[['name', 'salary', 'department']].head())

# 複数列でのソート
print("\n部署、年齢、給与の順でソート:")
multi_sorted = df.sort_values(['department', 'age', 'salary'], 
                             ascending=[True, False, False])
print(multi_sorted[['name', 'department', 'age', 'salary']].head(10))
```

### 演習3：データの変換と新しい列の作成

#### 3.1 新しい列の作成

```python
print("=== 新しい列の作成 ===")

# 年齢グループの作成
df['age_group'] = df['age'].apply(lambda x: 
    '若年層' if x < 30 else '中年層' if x < 50 else '高年層')

# 給与レベルの作成
df['salary_level'] = np.where(df['salary'] >= 450000, '高給',
                              np.where(df['salary'] >= 350000, '中給', '低給'))

# パフォーマンス評価の作成
df['performance_grade'] = df['performance_score'].apply(lambda x:
    'A' if x >= 90 else 'B' if x >= 80 else 'C' if x >= 70 else 'D')

# 勤続年数グループの作成
df['service_group'] = df['years_of_service'].apply(lambda x:
    '新人' if x < 2 else '中堅' if x < 5 else 'ベテラン')

print("新しい列を追加後のデータ:")
print(df[['name', 'age_group', 'salary_level', 'performance_grade', 'service_group']].head())
```

#### 3.2 データ型の変換

```python
print("\n=== データ型の変換 ===")

# 部署をカテゴリ型に変換
df['department'] = df['department'].astype('category')

# 年齢グループをカテゴリ型に変換
df['age_group'] = df['age_group'].astype('category')

# データ型の確認
print("変換後のデータ型:")
print(df.dtypes)

# カテゴリ型の情報
print("\n部署のカテゴリ:")
print(df['department'].cat.categories)
print("\n年齢グループのカテゴリ:")
print(df['age_group'].cat.categories)
```

### 演習4：集計とグループ化

#### 4.1 基本的な集計

```python
print("=== 基本的な集計 ===")

# 部署別の統計
print("部署別従業員数:")
dept_counts = df['department'].value_counts()
print(dept_counts)

print("\n部署別平均年齢:")
dept_age = df.groupby('department')['age'].mean()
print(dept_age)

print("\n部署別平均給与:")
dept_salary = df.groupby('department')['salary'].mean()
print(dept_salary)

# 複数の統計量を一度に計算
print("\n部署別の詳細統計:")
dept_stats = df.groupby('department').agg({
    'age': ['mean', 'min', 'max', 'count'],
    'salary': ['mean', 'min', 'max'],
    'performance_score': ['mean', 'min', 'max']
})
print(dept_stats)
```

#### 4.2 複雑な集計

```python
print("\n=== 複雑な集計 ===")

# 部署・年齢グループ別の集計
print("部署・年齢グループ別の統計:")
dept_age_stats = df.groupby(['department', 'age_group']).agg({
    'salary': ['mean', 'count'],
    'performance_score': ['mean', 'min', 'max']
})
print(dept_age_stats)

# 給与レベル別の分析
print("\n給与レベル別の分析:")
salary_level_stats = df.groupby('salary_level').agg({
    'age': ['mean', 'count'],
    'years_of_service': ['mean', 'min', 'max'],
    'performance_score': ['mean', 'std']
})
print(salary_level_stats)

# パフォーマンス評価別の分析
print("\nパフォーマンス評価別の分析:")
performance_stats = df.groupby('performance_grade').agg({
    'age': ['mean', 'count'],
    'salary': ['mean', 'std'],
    'years_of_service': ['mean', 'min', 'max']
})
print(performance_stats)
```

### 演習5：データの結合とマージ

#### 5.1 新しいデータの作成と結合

```python
print("=== データの結合 ===")

# 部署情報のデータフレームを作成
dept_info = pd.DataFrame({
    'department': ['営業部', '開発部', '人事部', '経理部', 'マーケティング部'],
    'manager': ['田中マネージャー', '佐藤マネージャー', '鈴木マネージャー', '高橋マネージャー', '渡辺マネージャー'],
    'budget': [50000000, 80000000, 20000000, 15000000, 30000000],
    'location': ['1階', '2階', '3階', '1階', '2階']
})

print("部署情報:")
print(dept_info)

# メインデータと部署情報を結合
df_with_dept = pd.merge(df, dept_info, on='department', how='left')
print("\n結合後のデータ（最初の5行）:")
print(df_with_dept[['name', 'department', 'manager', 'budget']].head())
```

#### 5.2 条件付き結合

```python
print("\n=== 条件付き結合 ===")

# 年齢グループ別の目標設定データを作成
age_targets = pd.DataFrame({
    'age_group': ['若年層', '中年層', '高年層'],
    'target_performance': [70, 80, 75],
    'target_salary_increase': [0.05, 0.03, 0.02],
    'training_hours': [40, 20, 10]
})

print("年齢グループ別目標:")
print(age_targets)

# 従業員データと目標データを結合
df_with_targets = pd.merge(df, age_targets, on='age_group', how='left')
print("\n目標データを結合した結果（最初の5行）:")
print(df_with_targets[['name', 'age_group', 'performance_score', 'target_performance']].head())
```

### 演習6：データの可視化

#### 6.1 基本的な可視化

```python
print("=== データの可視化 ===")

# 図のサイズを設定
plt.figure(figsize=(15, 10))

# 1. 年齢分布のヒストグラム
plt.subplot(2, 3, 1)
plt.hist(df['age'], bins=20, alpha=0.7, color='skyblue', edgecolor='black')
plt.title('年齢分布')
plt.xlabel('年齢')
plt.ylabel('人数')

# 2. 給与分布のヒストグラム
plt.subplot(2, 3, 2)
plt.hist(df['salary'], bins=20, alpha=0.7, color='lightgreen', edgecolor='black')
plt.title('給与分布')
plt.xlabel('給与（円）')
plt.ylabel('人数')

# 3. 部署別従業員数の棒グラフ
plt.subplot(2, 3, 3)
dept_counts.plot(kind='bar', color='orange', alpha=0.7)
plt.title('部署別従業員数')
plt.xlabel('部署')
plt.ylabel('人数')
plt.xticks(rotation=45)

# 4. 年齢と給与の散布図
plt.subplot(2, 3, 4)
plt.scatter(df['age'], df['salary'], alpha=0.6, color='red')
plt.title('年齢と給与の関係')
plt.xlabel('年齢')
plt.ylabel('給与（円）')

# 5. パフォーマンススコアの分布
plt.subplot(2, 3, 5)
plt.hist(df['performance_score'], bins=15, alpha=0.7, color='purple', edgecolor='black')
plt.title('パフォーマンススコア分布')
plt.xlabel('スコア')
plt.ylabel('人数')

# 6. 勤続年数の分布
plt.subplot(2, 3, 6)
plt.hist(df['years_of_service'], bins=15, alpha=0.7, color='gold', edgecolor='black')
plt.title('勤続年数分布')
plt.xlabel('勤続年数')
plt.ylabel('人数')

plt.tight_layout()
plt.show()
```

#### 6.2 高度な可視化

```python
print("\n=== 高度な可視化 ===")

# 図のサイズを設定
plt.figure(figsize=(15, 10))

# 1. 部署別の箱ひげ図（年齢）
plt.subplot(2, 2, 1)
df.boxplot(column='age', by='department', ax=plt.gca())
plt.title('部署別年齢分布')
plt.suptitle('')  # デフォルトタイトルを削除

# 2. 部署別の箱ひげ図（給与）
plt.subplot(2, 2, 2)
df.boxplot(column='salary', by='department', ax=plt.gca())
plt.title('部署別給与分布')
plt.suptitle('')

# 3. 年齢グループ別のパフォーマンススコア
plt.subplot(2, 2, 3)
df.boxplot(column='performance_score', by='age_group', ax=plt.gca())
plt.title('年齢グループ別パフォーマンススコア')
plt.suptitle('')

# 4. 給与レベル別の勤続年数
plt.subplot(2, 2, 4)
df.boxplot(column='years_of_service', by='salary_level', ax=plt.gca())
plt.title('給与レベル別勤続年数')
plt.suptitle('')

plt.tight_layout()
plt.show()
```

### 演習7：データの保存と読み込み

#### 7.1 データの保存

```python
print("=== データの保存 ===")

# CSVファイルとして保存
df.to_csv('employee_data.csv', index=False, encoding='utf-8')
print("CSVファイルに保存しました: employee_data.csv")

# 処理済みデータも保存
df_with_dept.to_csv('employee_data_with_department.csv', index=False, encoding='utf-8')
print("部署情報付きデータを保存しました: employee_data_with_department.csv")

# 集計データの保存
dept_stats.to_csv('department_statistics.csv', encoding='utf-8')
print("部署別統計データを保存しました: department_statistics.csv")

# Excelファイルとして保存
with pd.ExcelWriter('employee_data.xlsx') as writer:
    df.to_excel(writer, sheet_name='基本データ', index=False)
    dept_stats.to_excel(writer, sheet_name='部署別統計')
    df_with_dept.to_excel(writer, sheet_name='部署情報付き', index=False)
print("Excelファイルに保存しました: employee_data.xlsx")
```

#### 7.2 データの読み込みと確認

```python
print("\n=== データの読み込みと確認 ===")

# 保存したCSVファイルを読み込み
df_loaded = pd.read_csv('employee_data.csv')
print("読み込んだデータの形状:", df_loaded.shape)
print("読み込んだデータの最初の5行:")
print(df_loaded.head())

# データの整合性確認
print("\nデータの整合性確認:")
print("元データと読み込みデータが同じ:", df.equals(df_loaded))
print("列名が同じ:", list(df.columns) == list(df_loaded.columns))
```

### 演習8：実践的な分析例

#### 8.1 従業員分析レポートの作成

```python
print("=== 従業員分析レポート ===")

# 1. 全体概要
print("【全体概要】")
print(f"総従業員数: {len(df)}人")
print(f"平均年齢: {df['age'].mean():.1f}歳")
print(f"平均給与: {df['salary'].mean():,.0f}円")
print(f"平均パフォーマンススコア: {df['performance_score'].mean():.1f}点")

# 2. 部署別分析
print("\n【部署別分析】")
dept_analysis = df.groupby('department').agg({
    'employee_id': 'count',
    'age': 'mean',
    'salary': 'mean',
    'performance_score': 'mean',
    'years_of_service': 'mean'
}).round(2)
dept_analysis.columns = ['従業員数', '平均年齢', '平均給与', '平均パフォーマンス', '平均勤続年数']
print(dept_analysis)

# 3. 高パフォーマンス従業員の分析
print("\n【高パフォーマンス従業員分析】")
high_perf = df[df['performance_score'] >= 80]
print(f"高パフォーマンス従業員数: {len(high_perf)}人 ({len(high_perf)/len(df)*100:.1f}%)")
print(f"高パフォーマンス従業員の平均年齢: {high_perf['age'].mean():.1f}歳")
print(f"高パフォーマンス従業員の平均給与: {high_perf['salary'].mean():,.0f}円")

# 4. 給与分析
print("\n【給与分析】")
salary_analysis = df.groupby('salary_level').agg({
    'employee_id': 'count',
    'age': 'mean',
    'performance_score': 'mean'
}).round(2)
salary_analysis.columns = ['従業員数', '平均年齢', '平均パフォーマンス']
print(salary_analysis)

# 5. 年齢グループ別分析
print("\n【年齢グループ別分析】")
age_analysis = df.groupby('age_group').agg({
    'employee_id': 'count',
    'salary': 'mean',
    'performance_score': 'mean',
    'years_of_service': 'mean'
}).round(2)
age_analysis.columns = ['従業員数', '平均給与', '平均パフォーマンス', '平均勤続年数']
print(age_analysis)
```

### 演習のまとめ

この演習を通じて、以下のスキルを身につけました：

1. **NumPy配列の操作**
   - 基本的な配列操作と統計計算
   - 条件付きフィルタリング
   - 2次元配列の操作

2. **Pandas DataFrameの操作**
   - データの選択とフィルタリング
   - データの変換と新しい列の作成
   - データ型の変換

3. **データの集計と分析**
   - グループ化による集計
   - 複雑な統計分析
   - データの結合

4. **データの可視化**
   - 基本的なグラフ作成
   - 分布の可視化
   - 関係性の分析

5. **データの保存と読み込み**
   - 各種ファイル形式での保存
   - データの整合性確認

6. **実践的な分析**
   - ビジネスレポートの作成
   - 多角的なデータ分析

これらのスキルは、実際のデータ分析プロジェクトにおいて、効率的で正確なデータ処理を実現するための重要な基盤となります。
