# Matplotlib基礎入門 - 本当に初歩から始めよう

## 1. Matplotlibとは？

Matplotlibは、Pythonでグラフや図表を作成するためのライブラリです。
データを視覚的に表現するのに役立ちます。

## 2. まずはライブラリをインポートしてみよう

```python
# Matplotlibをインポートする
import matplotlib.pyplot as plt

# これで plt. を使ってMatplotlibの機能を使えるようになります
print("Matplotlibをインポートしました！")
```

**実行してみてください：**
- エラーが出なければ成功です
- エラーが出た場合は、Matplotlibがインストールされていない可能性があります

## 3. 最初の一歩：簡単なグラフを作ってみよう

```python
import matplotlib.pyplot as plt

# 簡単なデータを作成
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# 線グラフを作成
plt.plot(x, y)

# グラフにタイトルを付ける
plt.title('簡単な線グラフ')

# グラフを表示
plt.show()

print("グラフが表示されました！")
```

**実行結果：**
- 新しいウィンドウでグラフが表示されます
- グラフを閉じるまでプログラムは待機します

## 4. 基本的なグラフの種類

### 4.1 線グラフ（Line Plot）

```python
import matplotlib.pyplot as plt

# データを作成
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

# 線グラフを作成
plt.plot(x, y, 'b-o')  # 青い線と丸い点

# タイトルとラベルを追加
plt.title('線グラフの例')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グリッドを追加
plt.grid(True)

# グラフを表示
plt.show()
```

### 4.2 棒グラフ（Bar Plot）

```python
import matplotlib.pyplot as plt

# データを作成
categories = ['りんご', 'バナナ', 'オレンジ', 'ぶどう']
values = [10, 15, 8, 12]

# 棒グラフを作成
plt.bar(categories, values, color=['red', 'yellow', 'orange', 'purple'])

# タイトルとラベルを追加
plt.title('果物の売上数')
plt.xlabel('果物の種類')
plt.ylabel('売上数（個）')

# グラフを表示
plt.show()
```

### 4.3 散布図（Scatter Plot）

```python
import matplotlib.pyplot as plt

# データを作成
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
y = [2, 4, 1, 5, 3, 7, 4, 8, 6, 9]

# 散布図を作成
plt.scatter(x, y, color='red', s=100)  # sは点のサイズ

# タイトルとラベルを追加
plt.title('散布図の例')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# グラフを表示
plt.show()
```

## 5. グラフのカスタマイズ

### 5.1 色とスタイルの変更

```python
import matplotlib.pyplot as plt

# データを作成
x = [1, 2, 3, 4, 5]
y1 = [1, 4, 9, 16, 25]
y2 = [1, 2, 3, 4, 5]

# 複数の線を描画
plt.plot(x, y1, 'r-', label='y = x²', linewidth=2)  # 赤い線
plt.plot(x, y2, 'b--', label='y = x', linewidth=2)   # 青い破線

# タイトルとラベルを追加
plt.title('複数の線グラフ')
plt.xlabel('X軸')
plt.ylabel('Y軸')

# 凡例を表示
plt.legend()

# グリッドを追加
plt.grid(True, alpha=0.3)

# グラフを表示
plt.show()
```

### 5.2 グラフのサイズとレイアウト

```python
import matplotlib.pyplot as plt

# グラフのサイズを設定
plt.figure(figsize=(10, 6))  # 幅10、高さ6

# データを作成
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# グラフを作成
plt.plot(x, y, 'g-o', linewidth=3, markersize=8)

# タイトルとラベルを追加
plt.title('大きなグラフ', fontsize=16)
plt.xlabel('X軸', fontsize=12)
plt.ylabel('Y軸', fontsize=12)

# グリッドを追加
plt.grid(True, alpha=0.5)

# グラフを表示
plt.show()
```

## 6. 複数のグラフを並べて表示

```python
import matplotlib.pyplot as plt

# データを作成
x = [1, 2, 3, 4, 5]
y1 = [1, 4, 9, 16, 25]
y2 = [1, 2, 3, 4, 5]

# 2つのグラフを横に並べて表示
plt.figure(figsize=(12, 5))

# 1つ目のグラフ
plt.subplot(1, 2, 1)
plt.plot(x, y1, 'r-o')
plt.title('y = x²')
plt.xlabel('X軸')
plt.ylabel('Y軸')
plt.grid(True)

# 2つ目のグラフ
plt.subplot(1, 2, 2)
plt.plot(x, y2, 'b-s')
plt.title('y = x')
plt.xlabel('X軸')
plt.ylabel('Y軸')
plt.grid(True)

# レイアウトを調整
plt.tight_layout()

# グラフを表示
plt.show()
```

## 7. 日本語フォントの設定

```python
import matplotlib.pyplot as plt

# 日本語フォントの設定（Windowsの場合）
plt.rcParams['font.family'] = 'MS Gothic'
# Macの場合: plt.rcParams['font.family'] = 'Hiragino Sans'
# Linuxの場合: plt.rcParams['font.family'] = 'DejaVu Sans'

# データを作成
categories = ['りんご', 'バナナ', 'オレンジ', 'ぶどう']
values = [10, 15, 8, 12]

# 棒グラフを作成
plt.bar(categories, values, color=['red', 'yellow', 'orange', 'purple'])

# タイトルとラベルを追加
plt.title('果物の売上数')
plt.xlabel('果物の種類')
plt.ylabel('売上数（個）')

# グラフを表示
plt.show()
```

## 8. 練習問題

### 練習1: 自分のデータでグラフを作ってみよう

```python
import matplotlib.pyplot as plt

# あなたの好きなデータでグラフを作ってください
my_x = [1, 2, 3, 4, 5]  # ここにあなたのX軸データを入れてください
my_y = [1, 4, 9, 16, 25]  # ここにあなたのY軸データを入れてください

# グラフを作成
plt.plot(my_x, my_y, 'g-o')
plt.title('私のグラフ')
plt.xlabel('X軸')
plt.ylabel('Y軸')
plt.grid(True)
plt.show()
```

### 練習2: 棒グラフを作ってみよう

```python
import matplotlib.pyplot as plt

# あなたの好きなデータで棒グラフを作ってください
my_categories = ['A', 'B', 'C', 'D']  # ここにカテゴリ名を入れてください
my_values = [10, 20, 15, 25]  # ここに値を入れてください

# 棒グラフを作成
plt.bar(my_categories, my_values, color=['red', 'blue', 'green', 'orange'])
plt.title('私の棒グラフ')
plt.xlabel('カテゴリ')
plt.ylabel('値')
plt.show()
```

## 9. よくあるエラーと対処法

### エラー1: ModuleNotFoundError
```
ModuleNotFoundError: No module named 'matplotlib'
```
**対処法：** Matplotlibをインストールしてください
```bash
pip install matplotlib
```

### エラー2: グラフが表示されない
```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3], [1, 4, 9])
# plt.show() を忘れている！
```
**対処法：** `plt.show()` を必ず追加
```python
plt.plot([1, 2, 3], [1, 4, 9])
plt.show()  # これを追加
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
- Matplotlibのインポート方法
- 基本的なグラフの作成（線グラフ、棒グラフ、散布図）
- グラフのカスタマイズ（色、サイズ、タイトル）
- 複数グラフの表示
- 日本語フォントの設定
- エラーの対処法

次のステップ：
- より複雑なグラフ（ヒストグラム、箱ひげ図）
- データの可視化
- Seabornとの組み合わせ
- グラフの保存

**重要：** 実際にコードを実行してみることが大切です！ 