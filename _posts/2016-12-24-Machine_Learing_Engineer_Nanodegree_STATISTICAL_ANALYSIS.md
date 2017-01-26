---
published: true
title: Machine Learning - STATISTICAL ANALYSIS 01
layout: post
author: jungwook
category: Machine Learning
tag:
- machine learning
- statistical ANALYSIS
- mode
- mean
- median
---

## How to get a average?

어떻게 평균을 구할 것이냐? 일반적으로 해당 데이터 셋에 대해 평균을 구한다는 것은 아래와 같은 3가지 경우를 말한다.

## MODE (최빈값)

`Mode`는 최빈값이다. 일반적으로 데이터들이 나열되어 있는 경우 가장 많이 나오는 값. 예를 들면,

| 2  | 5  | 5  | 9  | 8  | 3 |

여기서 `mode`값은 두번 나온 5가 된다. 즉, `mode`의 정의는 가장 빈도가 높은 부분을 의미한다. 기본적인 상황에서 모드 값은 하나의 히스토그램내에서 한개만 존재할 수 있지만, 복합 형 히스토그램의 경우 두개 이상의 mode값을 가질 수도 있다.

## MEDIAN (중앙값)

`Median`은 중앙 값이다. 일련의 값들이 나열되어 있을 때, 해당 값들의 중앙에 위치한 값을 해당 히스토그램의 중앙 값이라고 한다. 예를 들어서

| 9 | 1 | 3 | 19 | 12 | 8 |

위와 같은 수들이 있다고 할 때, 이들을 정렬 하면,

| 1 | 3 | 8 | 9 | 12 | 19 |


위와 같이 되는데 해당 값의 중간값은 결국 8과 9의 사이인 `8.5` 정도가 된다.

`Median`의 장점은 상대적으로 범주 내에서 벗어나는 값이 들어오더라도 중간치의 변화자체는 크기가 않으로 상대적으로 중앙값의 경우 오차 범위가 큰 값이 들어올 경우 이에 대한 변동 폭이 실제 평균(`mean`)보다는 작다.

## MEAN (평균)

`Mean`은 산술적인 평균이라고 보면된다. 우리가 일반적으로 구하게 되는 X<sub>1</sub> ~ X<sub>n</sub>에 대한 평균을 구한다면, 


$$\frac{sum_{i=1}^{n} x_i}{n}$$


해당 값이 곧 산술적 평균이 된다. `Mean`은 산술적인 평균이므로, 정상적 범주를 벗어난 값이 들어올 경우 값의 이동이 크다.

## Outlier

`Outlier`는 현재 histogram에서 벗어나는 값을 말함. 예를 들어 데이터값이 1~10 사이에 있는데, 특정 값으로 100이 들엉는 경우에 100을 outliner라고 함.

이에 대한 명백한 정의는  Outliner < Q1 - 1.5(IQR), Outliner > Q3 + 1.5(IQR)

## What is Q1?

`Q1`은 분포의 25 %가 그 지점보다 아래에 있고 데이터의 75 %가 그 지점보다 위에있는 지점입니다.

## IQR = (Q3 - Q1)

사분면의 분포가 하위 25% 이상에서 상위 75%까지의 값을 말한다. 이를 IQR (Interquartile range)라고 함.


## 편차 구하기

![편차구하기](/images/20161228_234158.jpg "Machine learning")