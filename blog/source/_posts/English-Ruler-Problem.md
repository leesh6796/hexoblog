---
title: English Ruler Problem
date: 2018-02-08 02:04:30
categories:
- Algorithm
- DS
tags:
---

{% asset_img p1.png "English Ruler Problem" %}

DS 수업 시간에 배운 English Ruler Problem을 Recursive한 방법으로 풀어보자.

수업시간엔 Java로 풀었지만, 복습 과정에선 알고리즘 구현을 우선순위에 두고 있으므로 Python을 사용해 문제를 해결했다.

이 문제의 핵심은 길이 `L`인 눈금을 기준으로, 위 아래에 길이가 `L-1`인 Interval이 와야 한다는 것이다. 이 때 길이가 `L-1`인 Interval은 길이가 `L`인 두 눈금 사이에 그려져야 하는, 문제에서 요구하는 길이가 `L-1`인 눈금의 집합을 의미한다. 예를 들어 길이가 2인 Interval은 아래와 같다.

```
-
--
-

```


이제 recursive하게 문제를 해결하기 위해 base case를 잡자.

```
-
```

위와 같은 형태의 base를 잡을 수 있고, 이를 recursive하게 2번 반복한다고 생각하면

```
-
--
-
```

위와 같은 눈금을 그릴 수 있다. 이제 recursive step을 구상해보면, L이 1 이상일 때

```
Interval Drawing(length L-1)
LineDrawing(length L)
Interval Drawing(length L-1)
```

위와 같은 형태의 recursive step을 밟아나가면 된다.


전체 코드는 다음과 같다.

```python
def drawRuler(num_max_bar : int, max_inch : int): # n개의 max line을 가진다 하자
    # num_max_bar는 자에서 최대 눈금의 길이를, max_inch는 자에서 표시하는 최대 인치를 말한다.
    drawLabel(num_max_bar, 0) # 가장 처음 0을 그린다.
    for i in range(1, max_inch + 1):
        drawInterval(num_max_bar - 1) # n-1 크기의 구간을 그린 다음
        drawLabel(num_max_bar, i) # n개의 눈금을 그리고, 라벨을 붙인다.


def drawInterval(line_length : int):
    if line_length > 0:
        drawInterval(line_length - 1)
        drawLine(line_length)
        drawInterval(line_length - 1)


def drawLabel(length : int, label : int):
    # 라벨이 있는 눈금을 그린다.
    for i in range(length):
        print("-", end='')

    if label >= 0:
        # Python 3.6 이상에서만 동작하는 formatting이다.
        print(f" {label}", end='')

    print()


def drawLine(length : int): # without label
    # 라벨이 없는 눈금을 그린다.
    drawLabel(length, -1)


drawRuler(3, 2)
```


실행 결과는 다음과 같다.
```
--- 0
-
--
-
--- 1
-
--
-
--- 2
```