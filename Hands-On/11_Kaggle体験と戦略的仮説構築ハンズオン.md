# Module22：Kaggle体験と戦略的仮説構築（ハンズオン）

## 1. Kaggleとは

- 世界最大級のデータ分析・機械学習コンペティションプラットフォーム
- 公開データセット・ノートブック・ディスカッションが充実
- 実務的なデータ分析・仮説検証の練習に最適

---

## 2. Kaggleのアカウント作成と環境準備

1. [Kaggle公式サイト](https://www.kaggle.com/)でアカウント作成
2. Kaggle Notebooks（クラウド上のJupyter環境）を利用
3. 必要に応じてkaggle APIのセットアップ（ローカルで作業する場合）

---

## 3. 公開データセットのダウンロード

### 3.1 Kaggle Notebooksでのデータ利用
- 「Datasets」から興味のあるデータを選択し、「New Notebook」で分析開始

### 3.2 kaggle APIでローカルにダウンロード
```bash
pip install kaggle
# APIトークンを取得し、~/.kaggle/kaggle.jsonに配置
kaggle datasets download -d zynicide/wine-reviews
unzip wine-reviews.zip
```

---

## 4. 仮説立案→分析→仮説検証の流れ

### 4.1 仮説例
- 「高評価ワインはどの国で多いのか？」
- 「価格と評価の関係は？」

### 4.2 データの読み込みと前処理
```python
import pandas as pd
df = pd.read_csv('winemag-data-130k-v2.csv')
print(df.head())
```

### 4.3 仮説検証のための分析
```python
# 国ごとの平均評価
country_score = df.groupby('country')['points'].mean().sort_values(ascending=False)
print(country_score.head())

# 価格と評価の関係
import matplotlib.pyplot as plt
plt.scatter(df['price'], df['points'], alpha=0.3)
plt.xlabel('Price')
plt.ylabel('Points')
plt.title('価格と評価の関係')
plt.show()
```

---

## 5. ノートブックでの実践例

- Kaggle Notebooks上で、データの可視化・前処理・特徴量エンジニアリング・モデル構築まで一連の流れを体験
- 例：ワインデータでランダムフォレストによる高評価ワインの特徴分析

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# シンプルな特徴量選択
df2 = df[['points', 'price', 'country']].dropna()
df2['high_score'] = (df2['points'] >= 90).astype(int)
X = pd.get_dummies(df2[['price', 'country']])
y = df2['high_score']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
clf = RandomForestClassifier()
clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)
print('精度:', accuracy_score(y_test, y_pred))
```

---

## 6. 分析結果のまとめ・考察

- 仮説がデータで支持されたかどうかを確認
- 新たな発見や今後の分析アイデアをまとめる
- Kaggleディスカッションで他者の分析例も参考に

---

## 7. まとめ

- Kaggleでの仮説立案・分析・検証の一連の流れを体験しました
- 実データを使った戦略的なデータ分析の重要性を学びました
