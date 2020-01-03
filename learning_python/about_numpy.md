#yuki_edy 参上
<!-- パンくずリスト -->
[top](../index.md) > [Learning Python](./contents.md) > [About NumPy](./about_numpy.md)

<!-- タイトル -->
# Numpy

***

<!-- 目次 -->
### Contents

1. [準備](#準備)
1. [NumPy配列の基礎](#NumPy配列の基礎)
  - [配列の作成](#配列の作成)
  - [データ型](#データ型)
  - [次元数と要素数](#次元数と要素数)
  - [基本演算](#基本演算)
  - [ソート](#ソート)
  - [最大/最小/合計/累積](#最大/最小/合計/累積)
  - [乱数](#乱数)
  - [データの標本](#データの標本)
  - [行列の生成](#行列の生成)
  - [特別な行列の生成](#特別な行列の生成)
  - [行列変換](#行列変換)
  - [行列の内積](#行列の内積)
1. [NumPy計算の応用](#NumPy計算の応用)
  - [配列のコピー](#配列のコピー)
  - [ブールインデックス参照](#ブールインデックス参照(マスク処理))
  - [条件制御](#条件制御)
  - [ユニバーサル関数](#ユニバーサル関数)

***

<!-- 基礎 -->
## 0. 準備
```python
import numpy as np

%precision 3  # jupyter: 少数第3位まで表示
```

***

## 1. Numpy配列の基礎

### 配列の作成

```python
# 基本的な配列の作成
data = np.array([9, 2, 3, 4, 10, 6, 7, 8, 1, 5])
```

### データ型

```python
data = np.array([1, 2, 3], dtype=np.int16)  # 型を指定して配列の作成
data.dtype  # 型の確認
data.astype(np.uint32)  # 型の変更
```

#### ``int``: 符号付き整数

|データ型|内容|数値範囲|
|:--:|:--|:--|
|`int8`|8ビット符号付き整数|-128 ~ 127|
|`int16`|16ビット符号付き整数|-32768 ~ 32767|
|`int32`|32ビット符号付き整数|-2147483648 ~ 2147483647|
|`int64`|64ビット符号付き整数|#|

#### `uint`: 符号なし整数

|データ型|内容|数値範囲|
|:--:|:--|:--|
|`uint8`|8ビット符号付き整数|0 ~ 255|
|`uint16`|16ビット符号付き整数|0 ~ 65535|
|`uint32`|32ビット符号付き整数|0 ~ 4294967295|
|`uint64`|64ビット符号付き整数|#|

#### `float`: 浮動小数点数

|データ型|内容|数値範囲|
|:--:|:--|:--|
|`float16`|16ビット浮動小数点数|-6.55040e+04 ~ 6.55040e+04|
|`float32`|32ビット浮動小数点数|-3.4028235e+38 ~ 3.4028235e+38|
|`float64`|64ビット浮動小数点数|#|
|`float128`|128ビット浮動小数点数|#|

#### `bool`: 真偽値

|データ型|内容|数値範囲|
|:--:|:--|:--|
|`bool`|`True`か`False`の真偽値|#|

### 次元数と要素数

```python
data = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.int16)

data.shape # 配列の形 => (2, 3)
data.ndim  # 配列の次元数 => 2
data.size  # 配列の要素数 => 6
```

### 基本演算

```python
data1 = np.array([1, 2, 3], dtype=np.float16)
data2 = np.array([3, 2, 1], dtype=np.float16)

# 足し算 (引き算も同様)
data1 + 2 # => array([3., 4., 5.])
data1 + data2 # => array([4., 4., 4.])

# 掛け算
data1 * 2 # => array([2., 4., 6.])
data1 * data2 # => array([3., 4., 3.])

# 累乗
data1 ** 2 # => array([1., 4., 9.])
data1 ** data2 # => array([1., 4., 9.])

# 割り算
data1 / 2 # => array([0.5, 1. , 1.5])
data1 / data2 # => array([0.333, 1., 3.])
```

### ソート

```python
data = np.array([9, 2, 3, 4, 10, 6, 7, 8, 1, 5])

# 昇順(小 => 大)でソート
data.sort()
data # => array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# 降順(大 => 小)でソート
data[::-1].sort()
data # => array([10, 9, 8, 7, 6, 5, 4, 3, 2, 1])
```

### 最大/最小/合計/累積

```python
data = np.array([9, 2, 3, 4, 10, 6, 7, 8, 1, 5])

data.min() # 最小値 => 1
data.max() # 最大値 => 10
data.sum() # 合計値 => 55
data.cumsum() # 累積
# => array([10, 19, 27, 34, 40, 45, 49, 52, 54, 55])
data.cumsum() / data.sum() # 累積比率
# => array([0.182, 0.345, 0.491, 0.618, 0.727, 0.818, 0.891, 0.945, 0.982, 1.])
```

### 乱数

```python
import numpy.random as random

# 乱数シードを指定
# シードが同じであれば, 発生する乱数も同じになる
random.seed(0)

# 一様分布
random.rand(10) # [0.0, 1.0) から10個
random.random_sample(10) # [0.0, 1.0) から10個
random.randint(0, 100, 10) # [0, 100) から10個の整数

# 正規分布
random.randn(10) # 平均0, 分散1の正規分布から10個
random.normal(50, 10, 10) # 平均50, 分散10の正規分布から10個

# 二項分布
random.binomial(5, 0.3, 10) # p=0.3 で成功する試行を5回した場合の成功数を10個

# ベータ分布, ガンマ分布, カイ二乗分布
random.beta(2, 2, 10) # a=2, b=2 のベータ分布から10個
random.gamma(2, 2, 10) # shape=2. scale=2. のガンマ分布から10個
random.chisquare(2, 10) # 自由度2のカイ二乗分布から10個
```

### データの標本

```python
data = np.array([9, 2, 3, 4, 10, 6, 7, 8, 1, 5])

# 10回の復元抽出(重複あり)
np.random.choice(data, 10)

# 10回の非復元抽出(重複なし)
np.random.choice(data, 10, replace=False)
```

### 行列の生成

```python
data = np.arange(9).reshape(3, 3)
# => array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])

# スライシング
data[1, :] # 2行目 => array([3, 4, 5])
data[:, 0] # 1列目 => array([0, 3, 6])
data[0, 1:3] # 1行目, 2,3列目 => array([1, 2])
```

### 特別な行列の作成

```python
# 全てを0で初期化した配列
np.zeros((2, 3), dtype=np.int8)
# => array([[0, 0, 0],
#           [0, 0, 0]])

# 全てを1で初期化した配列
np.ones((2, 3), dtype=np.float16)
# => array([[1., 1., 1.],
#           [1., 1., 1.]])

# 全てを任意の数で初期化した配列
np.full((2, 3), np.pi)
# => array([[3.142, 3.142, 3.142],
#           [3.142, 3.142, 3.142]])

# 単位行列
np.eye(3) # = np.identity(3)
# => array([[1., 0., 0.],
#           [0., 1., 0.],
#           [0., 0., 1.]])
```

### 行列変換

```python
data = np.arange(0, 9).reshape(3, 3)
# => array([[0, 3, 6],
#           [1, 4, 7],
#           [2, 5, 8]])

# 行列の転置
data.T
# =>
```

### 行列の内積

```python
array1 = np.arange(0, 9).reshape(3, 3)
# => array([[0, 1, 2],
#           [3, 4, 5],
#           [6, 7, 8]])
array2 = np.arange(9, 18).reshape(3, 3)
# => array([[9, 10, 11],
#           [12, 13, 14],
#           [15, 16, 17]])

array1.dot(array2) # 結果は同じ
np.dot(array1, array2)
# => array([[ 42,  45,  48],
#           [150, 162, 174],
#           [258, 279, 300]])
```

***

## 2. Numpy計算の応用

### 配列のコピー

```python
# numpy.array型の変数[data]は参照型の変数であるため,
# 別の変数[alt]に代入されて, 代入先の変数[alt]に変更があった時,
# もとの変数[data]にまでデータが反映されてしまう
data = np.array([1, 2, 3])
alt = data
alt[1] = 0
alt # => array([1, 0, 3])
data # => array([1, 0, 3])

# 対応策として, copy() を使う方法がある.
data = np.array([1, 2, 3])
alt = data.copy()
alt[1] = 0
alt # => array([1, 0, 3])
data # => array([1, 2, 3])
```

### ブールインデックス参照 (マスク処理)

```python
columns = np.array(['a', 'b', 'b'])
data = np.arange(9).reshape(3, 3)

# ブールインデックスの作成
columns == 'b'
# => array([False, True, True])
data > 4  
# => array([[False, False, False],
#           [False, False, True],
#           [True, True, True]])

# ブールインデックスを用いた参照
data[columns == 'b']
# => array([[3, 4, 5],
#           [6, 7, 8]])

data[data > 4]
# => array([5, 6, 7, 8])
```

### 条件制御

```python
# np.where(条件配列, Trueに対応する配列, Falseに対応する配列)
cond = np.array([True, False, False, True, False])
array1 = np.array([1, 2, 3, 4, 5])
array2 = np.array([-1, -2, -3, -4, -5])

np.where(cond, array1, array2)
# => array([1, -2, -3, 4, -5])
```

### ユニバーサル関数

全ての要素に適応される処理を行いたい時, ユニバーサル関数を使用する.

```python
# abs: 絶対値の例
data = np.array([-1, 2, -3])
np.abs(data)
# => array([1, 2, 3])
```

#### 三角関数

|名前|処理|
|:--|:--|
|sin|sin関数|
|cos|cos関数|
|tan|tan関数|
|arcsin|sinの逆関数|
|arccos|cosの逆関数|
|arctan|tanの逆関数|

#### 整数

|名前|処理|
|:--|:--|
|ceil|小数部の切り上げ(天井)|
|floor|小数部の切り下げ(床)|
|rint|整数に四捨五入する|
|modf|小数部と整数部に分ける|
|abs|絶対値を取る|
|sign|負: -1, 0: 0, 正: 1 を返す|


#### 指数/対数/累乗/平方根

|名前|処理|
|:--|:--|
|exp|ネイピア指数関数|
|log|底がネイピア数(e)の対数|
|log2|底が2の対数|
|log10|底が10の対数|
|square|平方|
|sqrt|平方根|

#### 判定

|名前|処理|
|:--|:--|
|isnan|nan かどうかを判定|
|isinf|infかどうかを判定|
|isfinite|nanでもinfでもなければTrue|
|isin|別の配列に含まれる要素かどうか|

### 最小/最大/平均/合計
