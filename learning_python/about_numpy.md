<!-- パンくずリスト -->
[top](../index.md) > [Numpy](./learning_numpy.md)

<!-- タイトル -->
# Numpy

<!-- 目次 -->
### Contents

- [準備](#準備)
- [Numpy配列の基礎](#NumPy配列の基礎)


<!-- 基礎 -->
## 0. 準備
```python
import numpy as np

%precision 3  # jupyter: 少数第3位まで表示
```

## 1. NumPy配列の基礎

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

# 足し算
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

### 最大 / 最小 / 合計 / 累積

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


```

### データの標本

```python
data = np.array([9, 2, 3, 4, 10, 6, 7, 8, 1, 5])

# 10回の復元抽出(重複あり)
np.random.choice(data, 10)

# 10回の非復元抽出(重複なし)
np.random.choice(data, 10, replace=False)
```
