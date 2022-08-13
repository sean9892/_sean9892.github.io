---
layout: post
title: 'Introduction and Proof of Baby-step Giant-step Algorithm'
author: yubin.choi
comments: true
date: 2021-09-14 14:00
use_math: true
tags: [crypto]
redirect: "https://sean9892.tistory.com/30"
---

### 서론

시작하기에 앞서 이산 로그 문제(**D**iscrete **L**ogarithm **P**roblem, 이하 DLP)가 무엇인지 알아봅시다.

소수 $p$와 $\mathbb{Z}/p\mathbb{Z}$의 원소 $b$, $n$이 주어질 때 다음 식을 만족하는 $0$ 이상 $p-1$ 미만의 정수 $x$를 찾는 문제가 DLP입니다.
$$
b^x\equiv n \text{ (mod }p\text{)}
$$
Baby-step Giant-step 알고리즘은 DLP을 $O(\sqrt{p}\lg{p})$에 해결할 수 있는 알고리즘입니다. 다양한 DLP 알고리즘 중 완전탐색을 제외한 가장 간단한 알고리즘에 해당합니다.

이 글에서는 Baby-step Giant-step 알고리즘이 DLP를 어떻게 해결하는지, 왜 $O(\sqrt{p}\lg{p})$라는 시간복잡도가 보장되는지에 관해 알아보겠습니다.

### Baby-step Giant-step Algorithm

이 알고리즘은다음과 같은 순서로 진행됩니다.

1. $1$ 이상 $\lceil\sqrt{p}\rceil$인 모든 정수 $c$에 대해 $b^c\text{ mod }p$를 집합 $S$에 삽입한다.
2. $1$ 이상 $\lceil\sqrt{p}\rceil$인 모든 정수 $k$에 대해 $b^{k\lceil\sqrt{p}\rceil}$을 계산한다.
3. 오일러 소정리에 따라 $b^{-k\lceil\sqrt{p}\rceil}$를 구하여 $b^{x-k\lceil\sqrt{p}\rceil}$를 계산하고, 이 값을 $r$이라 하자.
4. $S$에 $r\equiv b^c \text{ (mod } p\text{)}$을 만족하는 $b^c \text{ mod }p$가 존재한다면 $x\equiv k\lceil\sqrt{p}\rceil +c \text{ (mod }p-1\text{)}$임을 알 수 있습니다.

### 타당성 증명

$b^{x-k\lceil\sqrt{p}\rceil}\equiv b^c\text{ (mod } p\text{)}$라면, 합동식의 성질에 따라 양변에 $b^{k\lceil\sqrt{p}\rceil}$를 곱하여 $b^x\equiv b^{k\lceil\sqrt{p}\rceil +c}\text{ (mod } p\text{)}$임을 알 수 있습니다.

이때 $0$ 이상 $p-1$ 미만의 두 정수 $\alpha$, $\beta$에 대하여 $b^\alpha\equiv b^\beta\text{ (mod } p\text{)}$라면 $\alpha=\beta$이므로 $x\equiv k\lceil\sqrt{p}\rceil +c \text{ (mod }p-1\text{)}$입니다.

### 효율성 증명

$a^b \text{ mod }p$를 계산하는 것은 $O(\lg{b})$에 가능합니다. 또한, 잘 구현된 집합 구현체는 어떤 원소가 포함되어있는지 $O(\lg b)$에 판별하는 것이 가능합니다.

따라서 (1.)은  $\lceil\sqrt{p}\rceil$번 $O(\lg{b})$의 연산을 수행하므로, $O(\sqrt{p}\lg b)$의 시간복잡도를 갖습니다.

(2.), (3.), (4.)에서 모듈로 역원을 구하는 과정이나 거듭제곱을 하는 모든 과정은 $O(\lg b)$에 가능하고, 총 $\lceil\sqrt{p}\rceil$번 이 과정을 반복하므로 역시 $O(\sqrt{p}\lg b)$에 동작합니다.

따라서 전체 시간복잡도는 $O(\sqrt{p}\lg b)$가 됩니다.

### 결론

Baby-step Giant-step 알고리즘을 활용해 DLP를 해결하는 방법과, 그 타당성 및 효율성의 증명에 관해 이야기해봤습니다. 본 글에서는 $\mathbb{Z}/p\mathbb{Z}$ 상에서의 DLP만을 다루었지만, 일반적으로 다양한 군에서 DLP와 Baby-step Giant-step 알고리즘을 적용할 수도 있습니다.

더 효율적인 DLP 알고리즘에 대해 알아보고 싶으시다면, 다음 키워드들을 추천드립니다.

- Pollard-rho 알고리즘
- Pohlig-Hellman 알고리즘