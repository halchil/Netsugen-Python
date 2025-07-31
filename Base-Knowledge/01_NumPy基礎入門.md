# NumPy基礎入門 - 本当に初歩から始めよう

## 1. NumPyとは？

NumPyは、Pythonで数値計算を行うための基本的なライブラリです。
配列（リストのようなもの）を効率的に扱うことができます。

## 2. まずはライブラリをインポートしてみよう

```python
# NumPyをインポートする
import numpy as np

# これで np. を使ってNumPyの機能を使えるようになります
print("NumPyをインポートしました！")
```

**実行してみてください：**
- エラーが出なければ成功です
- エラーが出た場合は、NumPyがインストールされていない可能性があります

## 3. 最初の一歩：簡単な配列を作ってみよう

```python
import numpy as np

# 簡単な配列を作成
numbers = np.array([1, 2, 3, 4, 5])

# 配列を表示してみよう
print("配列の内容:")
print(numbers)

# 配列の型を確認
print("配列の型:", type(numbers))
```

**実行結果例：**
```
配列の内容:
[1 2 3 4 5]
配列の型: <class 'numpy.ndarray'>
```

## 4. 配列の基本操作

### 4.1 配列の作成方法

```python
import numpy as np

# 方法1: リストから配列を作成
list_numbers = [10, 20, 30, 40, 50]
array1 = np.array(list_numbers)
print("方法1で作成した配列:", array1)

# 方法2: 直接配列を作成
array2 = np.array([100, 200, 300])
print("方法2で作成した配列:", array2)

# 方法3: 連続した数字の配列を作成
array3 = np.arange(1, 11)  # 1から10まで
print("方法3で作成した配列:", array3)
```

### 4.2 配列の要素にアクセス

```python
import numpy as np

# 配列を作成
numbers = np.array([10, 20, 30, 40, 50])

# 最初の要素（インデックス0）
print("最初の要素:", numbers[0])

# 2番目の要素（インデックス1）
print("2番目の要素:", numbers[1])

# 最後の要素
print("最後の要素:", numbers[-1])

# 最初の3つの要素
print("最初の3つの要素:", numbers[0:3])
```

## 5. 簡単な計算をしてみよう

```python
import numpy as np

# 配列を作成
numbers = np.array([1, 2, 3, 4, 5])

# 基本的な統計
print("配列:", numbers)
print("合計:", np.sum(numbers))
print("平均:", np.mean(numbers))
print("最大値:", np.max(numbers))
print("最小値:", np.min(numbers))
```

## 6. 練習問題

### 練習1: 自分の配列を作ってみよう

```python
import numpy as np

# あなたの好きな数字で配列を作ってください
my_array = np.array([ここに数字を入れてください])

print("私の配列:", my_array)
print("配列の合計:", np.sum(my_array))
```

### 練習2: 配列の要素を確認してみよう

```python
import numpy as np

# 配列を作成
test_array = np.array([15, 25, 35, 45, 55])

# 以下の要素を表示してください
print("3番目の要素:", test_array[2])  # インデックスは0から始まるので注意
print("最後の要素:", test_array[-1])
print("最初の2つの要素:", test_array[0:2])
```

## 7. よくあるエラーと対処法

### エラー1: ModuleNotFoundError
```
ModuleNotFoundError: No module named 'numpy'
```
**対処法：** NumPyをインストールしてください
```bash
pip install numpy
```

### エラー2: インデックスエラー
```python
import numpy as np
numbers = np.array([1, 2, 3])
print(numbers[5])  # エラー！配列の範囲外
```
**対処法：** 配列の長さを確認してからアクセス
```python
print("配列の長さ:", len(numbers))
print("有効なインデックス: 0から", len(numbers)-1)
```

## 8. まとめ

この章で学んだこと：
- NumPyのインポート方法
- 配列の作成方法
- 配列の要素へのアクセス
- 基本的な統計計算
- エラーの対処法

次のステップ：
- 2次元配列（行列）の操作
- より複雑な配列操作
- 条件付きフィルタリング

**重要：** 実際にコードを実行してみることが大切です！ 