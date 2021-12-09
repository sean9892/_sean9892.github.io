---
layout: post
author: yubin.choi
date: 2021-12-09 00:00
use_math: true
comments: true
title: 'Efficient Calculation of Linear Homogeneous recurrence with Matrix power'
---

### 1. 선형 동차 점화식이란

###### What's the Linear Homogeneous recurrence

수열의 점화식이 아래와 같이 표현될 때 $g(n)=0$이면 선형 동차점화식, $g(n)\ne0$이면 선형 비동차점화식이라고 한다.
$$
a_{n+k} = g(n)+\sum_{i=0}^{k} c_ia_{n+i}
$$
이때 $k$를 점화식의 차수(order)라고 한다.

### 2. 간단한 경우의 계산

###### Calculation for a simple case

간단하게 $a_{n+2}=\alpha a_{n+1}+\beta a_n$인 경우를 생각해보자.

$a_{n+3} = a_{n+2} = \alpha^2a_{n+1}+(\alpha\beta+\beta)$이라고 표현할 수 있고, 다시 이때의 계수를 $\gamma$와 $\delta$로 정의하면 $a_{n+3}=\gamma a_{n+1}+\delta a_{n}$으로 표현할 수 있다.

즉, 계수가 달라졌을 뿐 수학적으로 근본적인 상황은 계산하고자 하는 index를 증가시켜도 달라지지 않음을 알 수 있다.

이때  차수가 $k$인 점화식은 $k$개의 초기항에 동일한 연산을 반복해 적용하면서 이후 항들을 계산할 수 있다는 사실에 집중해보자.

일정한 $k$개의 값은 $1\times k$의 열벡터로, 동일한 연산을 적용하는 것은 행렬로 대응시킬 수 있다는 발상을 할 수 있다.

### 3. 행렬을 활용한 표현

###### Expression of the recurrence using matrix

다음과 같은 열벡터를 떠올려보자.


$$
\begin{bmatrix}
a_k \\
a_{k-1} \\
\vdots \\
a_1
\end{bmatrix}
$$


우리의 목표는 다음과 같은 등식을 만족하는 $k\times k$ 행렬 $T$를 찾는 것이다.


$$
T\begin{bmatrix}
a_k \\
a_{k-1} \\
\vdots \\
a_1
\end{bmatrix}
=
\begin{bmatrix}
a_{k+1} \\
a_{k} \\
\vdots \\
a_2
\end{bmatrix}
$$


약간의 관찰을 하자면, $1$~$k-1$번 행의 값이 $2$~$k$번 행으로 이동했고, 1번 행에는 점화식에 따라 계산할 $a_{k+1}$이 들어간다.

따라서 다음과 같은 행렬은 위 등식을 만족한다.


$$
T = \begin{bmatrix}
c_k & c_{k-1} & \cdots & c_2 & c_1 \\
1 & 0 & \cdots & 0 & 0 \\
0 & 1 & \cdots & 0 & 0 \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}
$$


해의 유일성은 증명되지 않았으나, 특수한 해는 구할 수 있었다. 이러한 $T$는 다음 등식을 만족한다는 사실 역시 관찰할 수 있다.


$$
T\begin{bmatrix}
a_{n+k-1} \\
a_{n+k-2} \\
\vdots \\
a_{n}
\end{bmatrix}
=
\begin{bmatrix}
a_{n+k} \\
a_{n+k-1} \\
\vdots \\
a_{n+1}
\end{bmatrix}
$$


따라서 귀납적으로 다음의 사실도 증명할 수 있다.


$$
T^n\begin{bmatrix}
a_{k} \\
a_{k-1} \\
\vdots \\
a_{1}
\end{bmatrix}
=
\begin{bmatrix}
a_{n+k} \\
a_{n+k-1} \\
\vdots \\
a_{n+1}
\end{bmatrix}
$$

### 4. 행렬 표현의 장점

###### Adventage of Matrix Expression

이전에는 점화식을 통해 모든 항을 계산해나가며 $a_n$을 계산하려면 총 $O(nk)$번의 덧셈이 요구되었다.

하지만 행렬 표현의 경우 행렬 곱셈은 $O(k^3)$번의 덧셈이 필요하며 $n$의 이진수 전개를 통해 행렬 곱셈을 총 $O(\lg N)$번 진행해야 함을 익히 알고 있으므로, 총 $O(k^3\lg N)$번의 덧셈으로 $a_N$을 계산할 수 있다는 사실을 알 수 있다.

대부분의 실용적인 수열 활용 예시의 경우 $k$가 크지 않기 때문에 큰 $N$에 대하여 $a_N$을 빠르게 계산할 수 있다는 사실을 알 수 있었다.

또한 $k$가 커 계산 시간이 커지는 경우 스트라센 알고리즘 등을 활용하여 시간복잡도를 계산할 수 있으므로, 많은 경우에 유의미하게 수열 계산의 시간복잡도를 줄일 수 있다.

