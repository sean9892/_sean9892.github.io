---
layout: post
title: "Introduction and Proof for Tonelli-Shanks Algorithm"
author: yubin.choi
comments: true
date: 2021-08-30 20:00
use_math: true
tags: [crypto]
---

## 서론

> 이 글은 이전에 작성한 Tonelli-Shanks 구현 포스팅에 Tonelli-Shanks에 대한 이론적 내용을 덧붙여 작성한 글입니다. 배우며 작성한 글이므로 틀린 내용이 있을 수 있으니, 발견하시면 댓글로 알려주시길 바랍니다.

> 본 글은 https://rkm0959.tistory.com/20를 토대로 학습하며 작성한 글입니다. 

암호학을 공부하다 보면 $\mathbb{Z}/p\mathbb{Z}$에서 이차방정식을 풀어야 할 필요가 있다. CTF에서 RSA 문제를 풀 때 $m^2\mod p$를 구했다거나, Diffie-Hellman 프로토콜 문제를 풀며 $g^2 \mod p$로부터 $g$를 찾아야 한다거나... 여러 상황에서 이차방정식을 해결할 수 있다면 편리할 것이다.

우선 $\mathbb{Z}/p\mathbb{Z}$가 아니라 $\mathbb{R}$에서 이차방정식을 풀 때를 생각해보자. 근의 공식이 너무 익숙하지만, 그 이전에는 한 변을 완전제곱식으로 변형하고 양변에 제곱근을 취하여 해를 구하였다.

따라서 $\mathbb{Z}/p\mathbb{Z}$에서도 $x^2 \equiv n \text{ (mod } p\text{)}$를 풀 수 있다면, 임의의 이차방정식에 대해 근을 구할 수 있음을 알 수 있다. 이러한 해결책으로는 Tonelli-Shanks 알고리즘을 사용하는 시간복잡도 $O(\log^2p)$의 솔루션이 존재한다.

우선 다음과 같은 Lemma를 확인해두자.

### Lemma 1

> $\mod p$에서 quadratic residue는 정확히 $p-1\over2$개 존재한다.

###### Proof

우선, $1\le k \le {p-1\over2}$일 때 $k^2\equiv(p-k)^2\text{ (mod } p\text{)}$이다.

$1\le a<b\le{p-1\over2}$이라고 하자. $a^2-b^2\equiv(a+b)(a-b)\text{ (mod } p\text{)}$인데, $2\le a+b \le p-1$이고 $a$와 $b$가 다른 수이므로 $a^2-b^2\mod p$는 0이 아니다. 따라서 $1,2,\cdots,{p-1\over2}$는 모두 제곱한 값이 서로 다르며, quadratic residue는 정확히 $p-1\over2$개 있음을 알 수 있다.

또한, Lemma 1에 의해 자명하게 quadratic non-residue 역시 $p-1\over2$개이다.

### Lemma 2

> $n$이 $\mod p$에서 quadratic residue $\Leftrightarrow$ $n^{p-1\over2}\equiv1\text{ (mod } p\text{)}$

###### Proof

quadratic residue라면 $x^2\equiv n\text{ (mod } p\text{)}$인 $x$가 존재하고, $n^{p-1\over2}\equiv x^{p-1}\equiv1\text{ (mod } p\text{)}$이다.
$x^{p-1}\equiv1\text{ (mod } p\text{)}$는 [페르마 소정리](https://ko.wikipedia.org/wiki/%ED%8E%98%EB%A5%B4%EB%A7%88%EC%9D%98_%EC%86%8C%EC%A0%95%EB%A6%AC)에 의해 성립한다.

$n^{p-1\over2}\equiv1\text{ (mod } p\text{)}$는 ${p-1\over2}$차 방정식이므로 최대 ${p-1\over2}$개의 근을 갖는다. 그러므로 quadratic non-residue는 이 방정식의 근이 될 수 없다.

참고로 $n$이 quadratic non-residue일 때 $n^{p-1\over2}\equiv-1\text{ (mod } p\text{)}$이다. $1\equiv n^{p-1}\equiv (n^{p-1\over2})^2 \text{ (mod } p\text{)}$이므로 $n^{p-1\over2}\equiv\pm1\text{ (mod } p\text{)}$인데, $n^{p-1\over2}\equiv1\text{ (mod } p\text{)}$은 아니므로 $n^{p-1\over2}\equiv-1\text{ (mod } p\text{)}$이다.

### Lemma 3

> $\sqrt{p}$ 이하의 자연수 중 quadratic non-residue가 적어도 하나 존재한다.

###### Proof

$\sqrt{p}$ 이하의 자연수가 모두 quadratic residue라고 가정하자. 이때 quadratic residue의 집합을 $Q$라고 하면, $a,b\in Q$일 때 $ab\in Q$이다. 따라서 $x\in\mathbb{Z}/p\mathbb{Z}$인 임의의 $x$에 대해 $x\in Q$를 보장할 수 있다. 하지만 Lemma 2에 의해 quadratic residue는 $p-1\over2$개뿐이므로 모순이 발생한다.

즉, 귀류법에 의해 Lemma 3이 성립한다.

## Tonelli-Shanks 알고리즘

Tonelli-Shanks 알고리즘은 다음과 같이 진행된다.

1. $n$이 $\text{mod } p$에서 quadratic residue인지 확인한다. (Lemma 2 활용)

2. $p-1=Q\cdot2^S$인 홀수 $Q$와 음 아닌 정수 $S$를 구한다.

3. $\text{mod } p$에서 quadratic non-residue인 정수 $z$를 하나 찾는다.
   Lemma 2에 의해 조사해야 하는 수의 개수의 기댓값이 2임을 알 수 있다.

   최악의 경우에도 Lemma 3에 의해 $O(\sqrt{p}\log{p})$에 찾을 수 있으나, 보통은 거의 상수 시간복잡도로 찾을 수 있다.

4. 다음과 같이 정의하자.

   - $M\equiv S\text{ (mod } p\text{)}$
   - $c\equiv z^Q \text{ (mod } p\text{)}$
   - $t\equiv n^Q\text{ (mod } p\text{)}$
   - $R\equiv n^{Q+1\over2}\text{ (mod } p\text{)}$

5. 다음 과정을 반복한다.

   - $t=0$이면 $0$을 출력한다.
   - $t=1$이면 $R$을 출력한다.
   - $0<i<M$과 $t^{2^i}\equiv1\text{ (mod } p\text{)}$를 만족하는 최소의 자연수 $i$를 찾는다.
   - $b$를 $b\equiv c^{2^{M-i-1}}\text{ (mod } p\text{)}$로 정의한다.
   - $M\leftarrow i$, $c\leftarrow b^2$, $t\leftarrow tb^2$, $R\leftarrow Rb\text{ (mod } p\text{)}$

6. 출력된 값이 $r$이라면 $\pm r$이 $x^2\equiv n\text{ (mod } p\text{)}$의 두 근이다.

## 알고리즘의 타당성 증명

초기의 정의를 다시 한 번 봐보자.

- $M=S$
-  $c\equiv z^Q \text{ (mod } p\text{)}$
- $t\equiv n^Q\text{ (mod } p\text{)}$
- $R\equiv n^{Q+1\over2}\text{ (mod } p\text{)}$

이때 초기값에 관해 다음과 같은 사실을 알 수 있다. $\cdots$ **(A)**

- $z$가 quadratic non-residue이므로 $c^{2^{M-1}}\equiv z^{Q\cdot2^{S-1}}\equiv z^{p-1\over2}\equiv-1\text{ (mod } p\text{)}$이다.
- $n$은 quadratic residue이므로 $t^{2^{M-1}}=n^{Q\cdot2^{S-1}}\equiv n^{p-1\over2}\equiv1\text{ (mod } p\text{)}$이다.
- $R^2\equiv n^{Q+1}\equiv nt\text{ (mod } p\text{)}$이다.

(5.)에서 $M,c,t,R$에 각각 $M^\prime,c^\prime,t^\prime,R^\prime$이 대입된다고 하자.

각각의 값이 어떻게 정의되는지 다시 한 번 확인하자.

- $b\equiv c^{2^{M-i-1}}\text{ (mod } p\text{)}$
- $M^\prime$는 $0<M^\prime<M$과 $t^{2^{M^\prime}}\equiv1\text{ (mod } p\text{)}$를 만족하는 최소의 자연수
- $c^\prime \equiv b^2 \text{ (mod } p\text{)}$
- $t^\prime\equiv tb^2 \text{ (mod } p\text{)}$
- $R^\prime\equiv Rb\text{ (mod } p\text{)}$

$b$의 정의에 따라 다음과 같은 명제가 성립한다.

- ${c^\prime}^{2^{M^\prime-1}}\equiv b^{2^i}\equiv c^{2^{M-1}}\equiv-1\text{ (mod } p\text{)}$

- ${t^\prime}^{2^{M^\prime-1}}\equiv t^{2^{M^\prime-1}}b^{2^{M^\prime}}\equiv(-1)\cdot(-1)\equiv1\text{ (mod } p\text{)}$
- ${R^\prime}^{2}\equiv R^2b^2\equiv ntb^2\equiv nt^\prime\text{ (mod } p\text{)}$

따라서 **(A)**에서 성립한 식은 (5.)를 몇 번 반복하더라도 여전히 성립함을 알 수 있다.

이 때 $t^{2^{M-1}}\equiv1\text{ (mod } p\text{)}$이므로 $0<M^\prime<M$인 $M^\prime$은 항상 존재하며, $M$의 값이 항상 감소하게 됨을 알 수 있다. 그렇게 계속 반복하며 $M=1$이 되는 순간 $1\equiv t^{2^{M-1}}\equiv t^1\text{ (mod } p\text{)}$이므로 $t\equiv1\text{ (mod } p\text{)}$임이 보장되며, $R^2\equiv nt\equiv n\text{ (mod } p\text{)}$이므로 $R$은 $x^2\equiv n\text{ (mod } p\text{)}$의 해가 된다.

## 알고리즘의 시간복잡도 증명

(1.), (2.), (4.)는 자명하게 $O(\log p)$에 동작함을 알 수 있다.

(3.) 역시 보통 상수 시간복잡도로 찾을 수 있다.

(5.)에서 $i$를 찾는 과정은 $O(M)$인데, $M$은 최악의 경우 $O(\log p)$이다. 따라서 $O(log p)$ 이내에 찾을 수 있다. (5.)의 이외의 과정도 $O(\log p)$에 계산할 수 있다.

(5.)는 최대 $O(\log p)$번 반복되므로 총 $O(\log^2 p)$에 동작한다.

따라서 전체 시간복잡도는 $O(\log p+1+\log^2 p)=O(\log^2 p)$가 됨을 알 수 있다.

## Python 구현

```python
def tonelli_shanks(n,p):
    def isQR(x,p):# check whether x is quadratic residue
        return pow(x,(p-1)//2,p)==1
    # (1.)
    if not isQR(n,p):
        return -1
    
    # (2.)
    Q,S=p-1,0
    while Q%2==0:
        S+=1
        Q//=2
    
    # (3.)
    z=None
    for x in range(2,p):
        if not isQR(x,p):
            z=x
            break
    # (4.)
    M,c,t,R=S,pow(z,Q,p),pow(n,Q,p),pow(n,(Q+1)//2,p)
    
    # (5.)
    while True:
        if t==0:
            return 0
        elif t==1:
            return R
        k=t*t%p
        ii=None
        for i in range(1,M):
            if k==1:
                ii=i
                break
            k*=k
            k%=p
        b=pow(c,2**(M-i-1),p)%p
        M=ii%p
        c=b*b%p
        t=t*c%p
        R=R*b%p

# test
if __name__=='__main__':
    n,p=map(int,input().split())
    print("STARTED")
    print(tonelli_shanks(n,p))
```

