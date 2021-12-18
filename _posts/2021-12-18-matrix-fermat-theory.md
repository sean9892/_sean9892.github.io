---
layout: post
title: 'Extension of FLT to Matrix base'
author: yubin.choi
comments: true
date: 2021-12-18 12:00
use_math: true
tags: [math]
---

###### 페르마 소정리의 행렬로의 확장

## 1. 다항식의 곱셈과 행렬

다항식은 수 사이의 관계를 표현하며, 다항식의 계수만을 적어 다항식을 벡터로 표현할 수 있습니다. 다항식을 곱하면 새로운 다항식으로 전이되며 벡터에 행렬을 곱하면 새로운 벡터로 전이된다는 사실을 생각해보면 다항식의 곱셈을 행렬로 표현하는 것이 새로운 관찰을 제공할 수 있을 듯 합니다.

우선 다음과 같이 다항식 $f$와 벡터 $v$를 대응시켜봅시다.


$$
f(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_0 \\
v = \begin{bmatrix}
a_0 \\
a_1 \\
\vdots \\
a_n
\end{bmatrix}
$$



다항식 사이의 곱셈에서 $x^c$의 계수는 $a+b=c$를 만족하는 음이 아닌 정수 $a,b$에 대해 두 다항식에서 $a$차항, $b$차항의 계수를 곱한 후 모두 더한 값이 됩니다. 따라서 '**다항식 $f$를 곱한다**'는 연산에 해당하는 행렬은 다음과 같이 대각선 방향으로 동일한 값이 위치하게 됩니다. $n$차 다항식은 $(n+1)\times1$꼴의 벡터로 표현되므로 이러한 행렬의 꼴은 $(2n+1)\times(n+1)$이 된다는 사실도 알 수 있습니다.


$$
\begin{bmatrix}
a_0 & 0 & \cdots & 0 \\
a_1 & a_0 & \cdots & 0 \\
a_2 & a_1 & \cdots & 0 \\
\vdots & \vdots & & \vdots \\
0 & 0 & \cdots & a_n
\end{bmatrix}
$$


## 2. $\mathbb{Z/nZ}$에서의 다항식 곱셈

다음 식은 페르마 소정리입니다. 이는 소수 $p$에 대해 $k$의 값에 무관하게 항상 성립하는 등식입니다.


$$
k \equiv k^p \text{ (mod }p\text{)}
$$


또한 $k$가 0이 아니라면, 다음이 성립합니다.


$$
k^{p-1} \equiv 1 \text{ (mod }p\text{)}
$$


따라서 1 이상 $p$ 미만의 정수 집합을 정의역으로 갖는 다항식 $f$는 법 $p$에서 항상 $p-2$차 이하임을 알 수 있습니다. 여기서 **1.**을 다시 떠올려 본다면, 법 $p$에서의 다항식과 그 곱셈은 항상 일정한 크기의 벡터와 행렬로 표현할 수 있겠다는 생각을 할 수 있습니다. 다음은 페르마 소정리를 이용해 정리한, 법 $p$에서 '**다항식 $f$를 곱한다**'에 해당하는 행렬입니다.


$$
\begin{bmatrix}
a_0 & a_n & \cdots & a_1 \\
a_1 & a_0 & \cdots & a_2 \\
a_2 & a_1 & \cdots & a_3 \\
\vdots & \vdots & & \vdots \\
a_n & a_{n-1} & \cdots & a_0
\end{bmatrix}
$$


그럼 다항식 $g(x)=1$에 여러 다항식 $a(x), b(x)$를 곱하는 상황을 생각해봅시다. 두 다항식을 곱하는 연산에 대응되는 행렬이 각각 $A,B$라면 다음과 같은 식으로 표현할 수 있습니다.


$$
a(x)b(x)g(x) \Leftrightarrow AB \begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix} \\
b(x) \Leftrightarrow B \begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix}
$$


위 수식의 두번째 줄을 보면, 결국 $a(x)b(x)g(x)$가 $a(x)b(x)$와 같다는, 우리가 이미 잘 알던 대수적 지식을 이끌어 낼 수 있음을 확인하였습니다. 잠깐, 어차피 $g(x)=1$을 곱하여 다항식에 해당하는 벡터를 얻을 수 있다면, 이 행렬은 '**다항식을 곱하는 연산**'이 아니라 '**다항식**' 그 자체로 보아도 괜찮지 않을까요? 다시 말해, 다음과 같이 대응시켜도 된다는 이야기입니다.


$$
a_n x^n + a_{n-1} x^{n-1} + \cdots + a_0 \Leftrightarrow \begin{bmatrix}
a_0 & a_n & \cdots & a_1 \\
a_1 & a_0 & \cdots & a_2 \\
a_2 & a_1 & \cdots & a_3 \\
\vdots & \vdots & & \vdots \\
a_n & a_{n-1} & \cdots & a_0
\end{bmatrix}
$$


## 3. Screw Matrix에 대한 페르마 소정리

그렇다면 위와 같은 대응관계에서 우리가 얻을 수 있는 것은 무엇인지 생각해봅시다. 위의 대응관계는 다항식을 행렬로 변환하는 방법을 묘사하지만, 동시에 특수한 꼴의 행렬을 다항식으로 변환하는 방법을 묘사합니다. 행렬보다는 다항식이 분석하기 용이한 면이 여럿 있기 때문에 이러한 변환은 몇 가지 사실을 알려줄 수 있을 듯 합니다. 예를 들면, 행렬에서의 페르마 소정리죠.

우선은 **2.**에서 발견한 대응관계를 통해 다항식으로 변환할 수 있는 특수한 행렬을 **Screw Matrix**라고 정의합시다. (같은 수들이 나열되어 있는 꼴이 왠지 나사 같아 보이지 않나요?) 조금 더 엄밀하게 정의하자면, Screw Matrix는 다음과 같은 명제를 만족하는 행렬 $M$입니다.


$$
\forall i,j,k \in \{0,1,\cdots,p-2\}\\
A_{ij} = A_{xy} \\
\text{ where }x= (i+k)\text{ mod }(p-2), y= (j+k)\text{ mod }(p-2)
$$


법 $p$에서 $x\neq0$일 때 다항식 $f(x)$의 $p$제곱은 $f(x)$와 합동이므로 임의의 Screw Matrix에 대응하는 다항식으로부터 다음과 같이 Screw Matrix $M$에 대한 페르마 소정리가 성립함을 알 수 있습니다.


$$
M \equiv M^p \text{ (mod }p\text{)}
$$
