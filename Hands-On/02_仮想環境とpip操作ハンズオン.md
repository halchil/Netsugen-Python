# Module04：仮想環境とpip操作ハンズオン

## 実践演習：環境構築とパッケージ管理

### 演習の準備

まず、現在のPython環境を確認しましょう。

```bash
# Pythonのバージョンを確認
python --version

# pipのバージョンを確認
pip --version

# 現在のディレクトリを確認
pwd

# 現在のディレクトリの内容を確認
ls -la
```

### 演習1：仮想環境の作成と管理

#### 1.1 基本的な仮想環境の作成

```bash
# プロジェクトディレクトリを作成
mkdir data-analysis-project
cd data-analysis-project

# 仮想環境を作成
python -m venv venv

# 仮想環境の構造を確認
ls -la venv/

# 仮想環境を有効化
# Windowsの場合
venv\Scripts\activate

# macOS/Linuxの場合
source venv/bin/activate

# 仮想環境が有効化されたことを確認
which python
pip --version
```

#### 1.2 仮想環境の状態確認

```bash
# 仮想環境内のPythonパスを確認
python -c "import sys; print(sys.executable)"

# 仮想環境内のパッケージ一覧を確認
pip list

# 仮想環境の詳細情報を確認
pip show pip
```

#### 1.3 仮想環境の無効化と再有効化

```bash
# 仮想環境を無効化
deactivate

# 無効化後のPythonパスを確認
which python

# 再度仮想環境を有効化
source venv/bin/activate  # macOS/Linux
# venv\Scripts\activate   # Windows
```

### 演習2：基本的なパッケージ管理

#### 2.1 パッケージのインストール

```bash
# 仮想環境が有効化されていることを確認
which python

# 基本的なパッケージをインストール
pip install numpy
pip install pandas
pip install matplotlib

# インストールされたパッケージを確認
pip list

# 特定のパッケージの詳細情報を確認
pip show numpy
pip show pandas
```

#### 2.2 バージョン指定でのインストール

```bash
# 特定バージョンのインストール
pip install numpy==1.21.0

# バージョン範囲でのインストール
pip install "pandas>=1.3.0,<1.4.0"

# 最新版へのアップグレード
pip install --upgrade matplotlib

# インストール済みパッケージの確認
pip list
```

#### 2.3 パッケージのアンインストール

```bash
# パッケージの削除
pip uninstall matplotlib

# 確認プロンプトなしで削除
pip uninstall matplotlib --yes

# 削除後のパッケージ一覧を確認
pip list
```

### 演習3：依存関係の管理

#### 3.1 requirements.txtの作成

```bash
# 現在の環境のパッケージ一覧を出力
pip freeze > requirements.txt

# requirements.txtの内容を確認
cat requirements.txt

# 特定のパッケージのみを抽出
pip freeze | grep numpy > numpy_requirements.txt
cat numpy_requirements.txt
```

#### 3.2 requirements.txtからのインストール

```bash
# 新しい仮想環境を作成してテスト
python -m venv test_env
source test_env/bin/activate  # macOS/Linux
# test_env\Scripts\activate   # Windows

# 空の環境であることを確認
pip list

# requirements.txtからパッケージをインストール
pip install -r requirements.txt

# インストール結果を確認
pip list

# テスト環境を削除
deactivate
rm -rf test_env
```

### 演習4：pyproject.tomlによるプロジェクト管理

#### 4.1 基本的なpyproject.tomlの作成

```bash
# メインの仮想環境に戻る
source venv/bin/activate

# pyproject.tomlファイルを作成
cat > pyproject.toml << 'EOF'
[build-system]
requires = ["setuptools>=45", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "data-analysis-project"
version = "0.1.0"
description = "データ分析プロジェクト"
authors = [
    {name = "研修参加者", email = "participant@example.com"}
]
requires-python = ">=3.8"
dependencies = [
    "numpy>=1.21.0",
    "pandas>=1.3.0",
    "matplotlib>=3.4.0",
    "jupyter>=1.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=6.0",
    "black>=21.0",
    "flake8>=3.8",
]
analysis = [
    "scikit-learn>=1.0.0",
    "seaborn>=0.11.0",
    "plotly>=5.0.0",
]
EOF

# pyproject.tomlの内容を確認
cat pyproject.toml
```

#### 4.2 pyproject.tomlからのインストール

```bash
# プロジェクトを開発モードでインストール
pip install -e .

# 開発用依存関係も含めてインストール
pip install -e ".[dev]"

# 分析用依存関係も含めてインストール
pip install -e ".[analysis]"

# インストールされたパッケージを確認
pip list
```

### 演習5：実践的なプロジェクト構造

#### 5.1 プロジェクトディレクトリ構造の作成

```bash
# プロジェクトの基本構造を作成
mkdir -p src/data_analysis
mkdir -p tests
mkdir -p data
mkdir -p notebooks
mkdir -p docs

# ディレクトリ構造を確認
tree . || ls -R

# 基本的なPythonファイルを作成
cat > src/data_analysis/__init__.py << 'EOF'
"""
データ分析プロジェクト
"""

__version__ = "0.1.0"
EOF

cat > src/data_analysis/utils.py << 'EOF'
"""
ユーティリティ関数
"""

import pandas as pd
import numpy as np

def load_sample_data():
    """サンプルデータを生成"""
    np.random.seed(42)
    data = {
        'id': range(1, 101),
        'value': np.random.normal(100, 15, 100),
        'category': np.random.choice(['A', 'B', 'C'], 100),
        'date': pd.date_range('2024-01-01', periods=100, freq='D')
    }
    return pd.DataFrame(data)

def calculate_statistics(df):
    """基本的な統計情報を計算"""
    stats = {
        'count': len(df),
        'mean': df['value'].mean(),
        'std': df['value'].std(),
        'min': df['value'].min(),
        'max': df['value'].max()
    }
    return stats
EOF

cat > tests/test_utils.py << 'EOF'
"""
ユーティリティ関数のテスト
"""

import pytest
import pandas as pd
from data_analysis.utils import load_sample_data, calculate_statistics

def test_load_sample_data():
    """サンプルデータ生成のテスト"""
    df = load_sample_data()
    
    assert isinstance(df, pd.DataFrame)
    assert len(df) == 100
    assert 'id' in df.columns
    assert 'value' in df.columns
    assert 'category' in df.columns
    assert 'date' in df.columns

def test_calculate_statistics():
    """統計計算のテスト"""
    df = load_sample_data()
    stats = calculate_statistics(df)
    
    assert 'count' in stats
    assert 'mean' in stats
    assert 'std' in stats
    assert 'min' in stats
    assert 'max' in stats
    assert stats['count'] == 100
EOF

cat > README.md << 'EOF'
# データ分析プロジェクト

## 概要
このプロジェクトは、Pythonを使ったデータ分析の学習用プロジェクトです。

## セットアップ
```bash
# 仮想環境を作成
python -m venv venv
source venv/bin/activate  # macOS/Linux
# venv\Scripts\activate   # Windows

# 依存関係をインストール
pip install -e .
pip install -e ".[dev]"
```

## 使用方法
```python
from data_analysis.utils import load_sample_data, calculate_statistics

# サンプルデータを読み込み
df = load_sample_data()

# 統計情報を計算
stats = calculate_statistics(df)
print(stats)
```

## テスト
```bash
pytest tests/
```
EOF
```

#### 5.2 プロジェクトの動作確認

```bash
# Pythonでプロジェクトをテスト
python -c "
from data_analysis.utils import load_sample_data, calculate_statistics

# サンプルデータを読み込み
df = load_sample_data()
print('データ形状:', df.shape)
print('カラム:', df.columns.tolist())

# 統計情報を計算
stats = calculate_statistics(df)
print('統計情報:', stats)
"

# テストを実行
pytest tests/ -v
```

### 演習6：高度なパッケージ管理

#### 6.1 パッケージ情報の詳細確認

```bash
# 特定パッケージの詳細情報
pip show numpy

# 依存関係ツリーの表示
pip install pipdeptree
pipdeptree

# 特定パッケージの依存関係
pipdeptree -p numpy

# 競合するパッケージの確認
pip check
```

#### 6.2 パッケージの検索と比較

```bash
# パッケージの検索（PyPIから）
pip search numpy  # 注意: この機能は現在制限されている場合があります

# 利用可能なバージョンの確認
pip index versions numpy

# パッケージの比較
pip show numpy pandas matplotlib
```

#### 6.3 キャッシュの管理

```bash
# pipキャッシュの情報を確認
pip cache info

# キャッシュの一覧を表示
pip cache list

# キャッシュをクリア
pip cache purge

# 特定パッケージのキャッシュを削除
pip cache remove numpy
```

### 演習7：トラブルシューティング

#### 7.1 よくある問題の解決

```bash
# pipのアップグレード
pip install --upgrade pip

# 破損したパッケージの修復
pip install --force-reinstall numpy

# 依存関係の競合解決
pip install --upgrade --force-reinstall pandas

# 特定バージョンでのインストール（競合回避）
pip install "numpy==1.21.0" "pandas==1.3.0"
```

#### 7.2 環境のクリーンアップ

```bash
# 不要なパッケージの削除
pip uninstall -y $(pip list | grep -v "Package\|---" | awk '{print $1}')

# 必要なパッケージのみ再インストール
pip install -e .

# 環境の状態確認
pip list
pip check
```

### 演習8：実践的なワークフロー

#### 8.1 開発ワークフローの実践

```bash
# 開発用パッケージのインストール
pip install -e ".[dev]"

# コードフォーマッターの実行
black src/

# リンターの実行
flake8 src/

# テストの実行
pytest tests/ -v

# カバレッジの確認
pip install pytest-cov
pytest tests/ --cov=src/ --cov-report=html
```

#### 8.2 本番環境の準備

```bash
# 本番用requirements.txtの作成
pip freeze > requirements-prod.txt

# 開発用requirements.txtの作成
pip freeze | grep -E "(pytest|black|flake8)" > requirements-dev.txt

# 最小限のrequirements.txtの作成
pip install pip-tools
pip-compile pyproject.toml --output-file requirements-minimal.txt
```

### 演習のまとめ

この演習を通じて、以下のスキルを身につけました：

1. **仮想環境の管理**
   - 仮想環境の作成・有効化・無効化
   - 複数の仮想環境の管理
   - 環境の独立性の確保

2. **パッケージ管理**
   - pipを使ったパッケージのインストール・削除
   - バージョン指定でのインストール
   - 依存関係の管理

3. **プロジェクト構造**
   - pyproject.tomlによる現代的なプロジェクト管理
   - 適切なディレクトリ構造の作成
   - テスト環境の構築

4. **トラブルシューティング**
   - よくある問題の解決方法
   - 環境のクリーンアップ
   - 依存関係の競合解決

これらのスキルは、今後のデータ分析プロジェクトにおいて、安定した開発環境を構築し、効率的なワークフローを実現するための重要な基盤となります。

