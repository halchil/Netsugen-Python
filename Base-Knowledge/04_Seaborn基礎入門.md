# Seaborn基礎入門 - 本当に初歩から始めよう

## 1. Seabornとは？

Seabornは、Matplotlibをベースにした統計データの可視化ライブラリです。
より美しく、統計的に意味のあるグラフを簡単に作成できます。

## 2. まずはライブラリをインポートしてみよう

```python
# Seabornをインポートする
import seaborn as sns

# これで sns. を使ってSeabornの機能を使えるようになります
print("Seabornをインポートしました！")
```

**実行してみてください：**
- エラーが出なければ成功です
- エラーが出た場合は、Seabornがインストールされていない可能性があります

## 3. 最初の一歩：簡単なグラフを作ってみよう

```python
import seaborn as sns
import matplotlib.pyplot as plt

# サンプルデータを作成
data = [1, 2, 2, 3, 3, 3, 4, 4, 5, 5, 5, 5, 6, 6, 7]

# ヒストグラムを作成
sns.histplot(data=data, bins=10)

# タイトルを追加
plt.title('簡単なヒストグラム')

# グラフを表示
plt.show()

print("Seabornでグラフが作成されました！")
```

**実行結果：**
- 美しいヒストグラムが表示されます
- Matplotlibよりも見た目が良いです

## 4. 基本的なグラフの種類

### 4.1 ヒストグラム（Histogram）

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# サンプルデータを作成
data = np.random.normal(0, 1, 1000)  # 正規分布のデータ

# ヒストグラムを作成
sns.histplot(data=data, bins=30, color='skyblue', edgecolor='black')

# タイトルとラベルを追加
plt.title('正規分布のヒストグラム')
plt.xlabel('値')
plt.ylabel('頻度')

# グラフを表示
plt.show()
```

### 4.2 散布図（Scatter Plot）

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# サンプルデータを作成
np.random.seed(42)
x = np.random.randn(50)
y = 2 * x + np.random.randn(50) * 0.5

# 散布図を作成
sns.scatterplot(x=x, y=y, color='red', s=100)

# タイトルとラベルを追加
plt.title('散布図の例')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グラフを表示
plt.show()
```

### 4.3 箱ひげ図（Box Plot）

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# サンプルデータを作成
np.random.seed(42)
group1 = np.random.normal(0, 1, 100)
group2 = np.random.normal(2, 1.5, 100)
group3 = np.random.normal(1, 0.8, 100)

# データをリストにまとめる
data = [group1, group2, group3]
labels = ['グループA', 'グループB', 'グループC']

# 箱ひげ図を作成
sns.boxplot(data=data)

# ラベルを設定
plt.xticks(range(len(labels)), labels)

# タイトルを追加
plt.title('箱ひげ図の例')
plt.ylabel('値')

# グラフを表示
plt.show()
```

## 5. データフレームを使った可視化

### 5.1 PandasとSeabornの組み合わせ

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# サンプルデータフレームを作成
np.random.seed(42)
data = {
    '年齢': np.random.normal(30, 8, 100),
    '給与': np.random.normal(400000, 80000, 100),
    '部署': np.random.choice(['営業部', '開発部', '人事部'], 100)
}
df = pd.DataFrame(data)

# 年齢の分布を可視化
sns.histplot(data=df, x='年齢', bins=20, color='lightblue')

# タイトルを追加
plt.title('年齢分布')
plt.xlabel('年齢')
plt.ylabel('人数')

# グラフを表示
plt.show()
```

### 5.2 部署別の給与分布

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# サンプルデータフレームを作成
np.random.seed(42)
data = {
    '年齢': np.random.normal(30, 8, 100),
    '給与': np.random.normal(400000, 80000, 100),
    '部署': np.random.choice(['営業部', '開発部', '人事部'], 100)
}
df = pd.DataFrame(data)

# 部署別の給与分布を箱ひげ図で表示
sns.boxplot(data=df, x='部署', y='給与')

# タイトルを追加
plt.title('部署別給与分布')
plt.xlabel('部署')
plt.ylabel('給与（円）')

# グラフを表示
plt.show()
```

## 6. 美しいスタイルの設定

### 6.1 スタイルの変更

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# スタイルを設定
sns.set_style("whitegrid")  # 白い背景にグリッド

# サンプルデータを作成
x = np.linspace(0, 10, 100)
y = np.sin(x)

# 線グラフを作成
sns.lineplot(x=x, y=y, color='blue', linewidth=2)

# タイトルを追加
plt.title('美しい線グラフ')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グラフを表示
plt.show()
```

### 6.2 色のパレット設定

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# 色のパレットを設定
sns.set_palette("husl")  # 美しい色のパレット

# サンプルデータを作成
categories = ['A', 'B', 'C', 'D', 'E']
values = [10, 25, 15, 30, 20]

# 棒グラフを作成
sns.barplot(x=categories, y=values)

# タイトルを追加
plt.title('美しい棒グラフ')
plt.xlabel('カテゴリ')
plt.ylabel('値')

# グラフを表示
plt.show()
```

## 7. 統計的な可視化

### 7.1 回帰直線付き散布図

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# サンプルデータを作成
np.random.seed(42)
x = np.random.randn(50)
y = 2 * x + np.random.randn(50) * 0.5

# 回帰直線付き散布図を作成
sns.regplot(x=x, y=y, scatter_kws={'alpha':0.6})

# タイトルを追加
plt.title('回帰直線付き散布図')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グラフを表示
plt.show()
```

### 7.2 ヒートマップ

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# サンプルデータを作成
np.random.seed(42)
data = np.random.randn(10, 10)

# ヒートマップを作成
sns.heatmap(data, annot=True, cmap='coolwarm', center=0)

# タイトルを追加
plt.title('ヒートマップの例')

# グラフを表示
plt.show()
```

## 8. 練習問題

### 練習1: 自分のデータでヒストグラムを作ってみよう

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# あなたの好きなデータでヒストグラムを作ってください
my_data = np.random.normal(0, 1, 200)  # ここにあなたのデータを入れてください

# ヒストグラムを作成
sns.histplot(data=my_data, bins=20, color='lightgreen')

# タイトルを追加
plt.title('私のヒストグラム')
plt.xlabel('値')
plt.ylabel('頻度')

# グラフを表示
plt.show()
```

### 練習2: 散布図を作ってみよう

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# あなたの好きなデータで散布図を作ってください
my_x = np.random.randn(50)  # ここにあなたのX軸データを入れてください
my_y = np.random.randn(50)  # ここにあなたのY軸データを入れてください

# 散布図を作成
sns.scatterplot(x=my_x, y=my_y, color='red', s=80)

# タイトルを追加
plt.title('私の散布図')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グラフを表示
plt.show()
```

## 9. よくあるエラーと対処法

### エラー1: ModuleNotFoundError
```
ModuleNotFoundError: No module named 'seaborn'
```
**対処法：** Seabornをインストールしてください
```bash
pip install seaborn
```

### エラー2: データの形式エラー
```python
import seaborn as sns
sns.histplot(data="文字列")  # エラー！データは数値である必要があります
```
**対処法：** 正しいデータ形式を使用
```python
import numpy as np
data = np.array([1, 2, 3, 4, 5])
sns.histplot(data=data)
```

### エラー3: 日本語が表示されない
```python
plt.title('日本語タイトル')  # 文字化けする
```
**対処法：** フォント設定を追加
```python
plt.rcParams['font.family'] = 'MS Gothic'  # Windows
plt.title('日本語タイトル')
```

## 10. まとめ

この章で学んだこと：
- Seabornのインポート方法
- 基本的なグラフの作成（ヒストグラム、散布図、箱ひげ図）
- データフレームを使った可視化
- 美しいスタイルの設定
- 統計的な可視化
- エラーの対処法

次のステップ：
- より複雑な統計グラフ
- データの相関分析
- 時系列データの可視化
- インタラクティブなグラフ

**重要：** 実際にコードを実行してみることが大切です！ 