<!-- パンくずリスト -->
[top](../index.md) > [Codility](./contents.md) > [Lesson1 - Iterations](./lesson1_iterations.md)

<!-- タイトル -->
# Lesson1 - Iterations

***

<!-- 目次 -->
### Contents

- [BinaryGap](#BinaryGap)

#### BinaryGap

正の整数を2進数変換した時に, 最長の1同士の距離を求める問題.

> A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.
>
>For example ...
number 9 has binary representation 1001 and contains a binary gap of length 2.
The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3.
The number 20 has binary representation 10100 and contains one binary gap of length 1.
The number 15 has binary representation 1111 and has no binary gaps.
The number 32 has binary representation 100000 and has no binary gaps.
>
> - N is an integer within the range [1..2,147,483,647].

#### answer1

```python
def solution(N):
    flag = False
    max_gap, gap = 0, 0

    while N != 0:
        rem = N%2  
        if flag and rem == 0:
            gap += 1
        elif rem == 1:
            max_gap = max(gap, max_gap)
            gap = 0
            flag = True
        N = N//2

    return max_gap
```

#### performance

|program|category|time complexity|spatial complexity|
|:--:|:--:|:--:|:--:|
|[answer1](#answer1)| x | O($\log_2{N}$) | O(1) |
