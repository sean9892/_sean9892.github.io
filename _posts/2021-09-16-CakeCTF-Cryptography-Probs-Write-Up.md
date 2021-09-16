---
layout: post
title: "CakeCTF Cryptography Probs Write-Up"
author: yubin.choi
comments: true
date: 2021-09-16 21:00
use_math: true
tags: [crypto,contest]
---

### 서론

굉장한 버스를 탔던 대회이다. 그럼에도 뉴비 키워준다는 느낌으로 가이딩을 많이 해주셔서 혼자서라면 꽤 많이 삽질했을 법한 부분도 수월하게 넘길 수 있던 것 같다.

물론 내가 참가한 부분은 crypto이므로, 이에 해당하는 5가지 문제를 업솔빙하며 풀이를 적어보려고 한다.



### discrete-log

문제에서 주어진 코드와 실행 결과는 다음과 같다.

```python
from Crypto.Util.number import getPrime, isPrime, getRandomRange

def getSafePrime(bits):
    while True:
        p = getPrime(bits - 1)
        q = 2*p + 1
        if isPrime(q):
            return q

with open("flag.txt", "rb") as f:
    flag = f.read().strip()

p = getSafePrime(512)
g = getRandomRange(2, p)
r = getRandomRange(2, p)

cs = []
for m in flag:
    cs.append(pow(g, r*m, p))

print(p)
print(g)
print(cs)
```

```
10577926960839937947442162797370864980541285292536671603546595533193324977125572190720609448828374782284663027664894813711243894320697692129630847705557539
9947724104164898694903023872711663896409433873530762235716749042436185304062119886390357927264325412355223958396239523671881766361219889894069645084522127
[122928806618818844614057... (생략)
```

생각보다 어렵지 않게 풀 수 있다.

flag의 각 문자의 아스키코드 $m$에 대해 $g^{rm}\text{ mod }p$를 cs에 담아 출력하는데, $g^{rm}=(g^r)^m$이므로 $g^r\text{ mod }p$를 안다면 flag의 각 문자를 100번(printable character의 수) 이내의 bruteforcing으로 구할 수 있을 것이다.

이때 $g^r\text{ mod }p$를 어떻게 구하느냐가 문제인데, 모든 flag의 공통접두사인 **CakeCTF{**를 활용하면 된다. **a**와 **e**는 ascii 코드 값이 고작 4만큼 차이난다. 이는 $g^{97r}\text{ mod }p$와 $g^{101r}\text{ mod }p$을 알 수 있다는 얘기이고, 따라서 $(g^r)^4\text{ mod }p$를 알 수 있다는 의미이다.

이 값을 가지고 tonelli-shanks 알고리즘을 두 번 사용하면 최대 4개의 $g^r \text{ mod }p$ 값 후보를 얻을 수 있다. 4개의 값 모두에 대해 위에서 언급한 bruteforcing을 하여 플래그가 정상적으로 구해는 것을 찾으면 된다.

<details>
    <summary>[discrete-log] flag 확인하기</summary>
    <p>
        flag: CakeCTF{ba37a0f409ef3ec23a6cffbc474a1cef}
    </p>
</details>



### improvisation

다음과 같은 점화식을 갖는 간단한 LFSR의 취약점을 분석하는 문제이다.
$$
R_{n+64} = R_{n}\oplus R_{n+1}\oplus R_{n+8}\oplus R_{n+16}
$$


64bit LFSR인데, flag 공통접두사인 **CakeCTF{**가 마침 64bit이므로 seed를 쉽게 구할 수 있다. 주의할 것은 flag 맨 앞글자인 **C**를 ascii 코드 값으로 변환했을 때 leading zero가 존재한다는 점이다. 이를 유의하여 LFSR의 seed를 구하고 LFSR을 구현하여 flag를 간단하게 얻을 수 있다.

<details>
    <summary>[improvisation] flag 확인하기</summary>
    <p>
        flag: CakeCTF{d0n't_3xp3c7_s3cur17y_2_LSFR}
    </p>
</details>



### matrixcipher

추후 추가 예정



### party ticket

추후 추가 예정



### together as one

lunatic 난이도라고 서술된 문제이지만, lunatic이라 할 만큼 어렵진 않다.(그렇다고 나 같은 뉴비 기준으로도 마냥 쉬운 건 아니다.)

RSA puzzle인데, 이게 대체 crypto인지 그냥 정수론 문제인지 잘 모르겠다.

$n$, $c$, $x=(p+q)^r \text{ mod }n$과 $y=(p+qr)^r\text{ mod }n$이 주어질 때 $c$를 복호화하는 문제이다. 이런 문제 유형에서는 $p$, $q$, $r$을 구하여 $\phi(n)$를 구하는 것이 풀이일게 뻔하므로 $n$을 소인수분해해야 한다.

이항정리를 이용하여 $|y-x|$를 계산해보면 $q$의 배수임을 알 수 있다. $\gcd(|y-x|,n)$을 계산하면 $q$를 알 수 있다.

$y-x$를 전개한 것을 잘 살펴보면, $q^{r}r^r-q^{r}$인데, 페르마 소정리에 의해 $r$로 나눈 나머지가 $q$임을 알 수 있다. 따라서 $\gcd(y-x-q,{n\over q})$를 계산해주면 $r$을 알 수 있다.

$p$, $q$, $r$의 값을 모두 구했으면 $\phi(pqr)$은 쉽게 계산할 수 있고, 이를 이용해 $c$를 복호화해주면 flag를 알 수 있다.

<details>
    <summary>[together as one] flag 확인하기</summary>
    <p>
        flag: CakeCTF{This_chall_is_inspired_by_this_music__Check_out!__https://www.youtube.com/watch?v=vLadkYLi8YE_cf49dcb6a31f}
    </p>
</details>

