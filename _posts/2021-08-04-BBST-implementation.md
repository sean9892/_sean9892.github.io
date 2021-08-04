---
layout: post
title: 'BBST 병목 및 경쟁 현상 완화를 위한 Skip List의 구현 및 성능 검증'
author: yubin.choi
comments: true
date: 2021-08-04 14:18
use_math: true
tags: [computer-science]
---

Linked List는 메모리를 동적으로 관리하며 데이터를 저장할 수 있는 자료구조 중 하나로, 가장 단순한 형태의 선형 컨테이너이다. 하지만 random access에 대해서 충분히 빠른 시간복잡도를 보장하지 못하기 때문에 다른 자료구조로 흔히 대체된다. Linked List를 대체하여 random access, insert, remove의 세 가지 연산을 빠르게 수행할 수 있는 자료구조 중 대표적으로 Balanced Binary Search Tree, 즉 균형 이진 트리가 존재한다. Red-Black Tree, AVL Tree 등 다양한 종류의 BBST가 존재하여 많이 활용되고 있으나, 멀티 스레드로 프로그램을 구현하는 경우가 잦아진 요즘 SW 개발에서는 BBST의 balancing 과정이 BBST에의 접근을 차단하고 진행되기 때문에 프로그램 전반의 병목 현상 혹은 경쟁현상을 발생시킨다. 이를 개선하는 방법이 존재하기는 한다고 하나, 구현의 난이도가 높아 작은 구현 실수도 줄이는게 좋은 실무 개발에서는 쉽사리 택하기 어려운 방안이다.

Linked List보다 빠르며 멀티 스레드 프로그램 작성 시 BBST와 달리 병목 현상이 덜 발생하는 대안책으로써 Skip List라는 자료구조가 존재하여 이를 구현하였다.



#### 탐구 동기

'탐색'의 가장 기본적인 자료구조는 Linked List이다. 이러한 Linked List는 구현이 간단하며 기능 및 효과가 명확하여 좋아하는 자료구조인데, 한 가지 흠인 성능을 개선시킬 방안에 대해 고민하게 되었다. 그러던 중 Sparse Table과 같이 참조 위치를 2의 거듭제곱만큼씩 이동할 수 있는 List를 구현하는 아이디어를 떠올렸고, 이것이 Skip List라는 자료구조로 존재한다는 것을 알게 되었다. 본 탐구에서는 $O(n \log n)$의 공간복잡도를 사용하여 random access, push_back, pop_back의 연산을 $O(\log n)$에 수행할 수 있게 하는 Skip List를 구현하는 것이 목표이다.



#### 이론적 배경

Skip List에서는 각 노드는 $1,2,4,\cdots,2^n$칸만큼 뒤로 떨어진 노드의 주소를 저장한다. 따라서 $n$번째 노드에 접근할 때는 모든 자연수 $n$이 $O(\log n)$개의 서로 다른 2의 거듭제곱 수의 합으로 표현될 수 있으므로 $O(\log n)$의 시간복잡도에 접근할 수 있음이 보장된다. 또한, 데이터의 삽입 및 삭제 시에는 $O(\log n)$개의 연결이 추가되거나 사라지므로 이 두 연산 역시 $O(\log n)$의 시간복잡도를 보장할 수 있다.



#### 구현 계획

C++ 언어에서 class template을 사용하여 다양한 데이터 타입에 적용할 수 있게 구현하며, 다음과 같은 기능을 구현한다. T는 저장할 데이터의 타입이며, 인덱스는 1-based index를 사용한다.

```cpp
int size() // 저장된 데이터의 수를 반환함.
T getNthValue(int n) // n번째 데이터의 값을 반환함.
T operator[](int n) // getNthValue(int n)와 동일.
void push_back(T data) // data의 값을 가진 노드를 가장 뒤에 추가함.
T pop_back() // 가장 뒤의 노드를 삭제하고 그 노드에 저장되어있던 데이터를 반환함.
```

**size()** 연산은 $O(1)$의 시간복잡도가 보장되며 나머지 연산은 모두 $O(\log n)$의 시간복잡도가 보장되도록 구현한다.



#### 코드 구현

```cpp
template<typename T>
class SkipList{
private:
    struct Node{
        T data;
        vector<Node*> p;
    };
    Node head;
    int sz;
 
    Node* getNth(int n){
        assert(n>=0&&n<=sz);
        Node* p = &head;
        while(n){
            int msb = __builtin_clz(n);
            int k=31-msb;
            p = p->p[k];
            n^=(1<<k);
        }
        return p;
    }
public:
    SkipList() : sz(0){}
    ~SkipList(){
        if(sz==0)return;
        Node* p = &head;
        while(sz){
            pop_back();
        }
    }
 
    int size(){
        return sz;
    }
 
    T getNthValue(int n){
        assert(n>0&&n<=sz);
        return getNth(n)->data;
    }
    T operator[](int n){
        return getNthValue(n);
    }
 
    void push_back(T data){
        Node* nd = new Node;
        nd->data = data;
        sz++;
        for(int i=1;sz-i>=0;i*=2){
            getNth(sz-i)->p.push_back(nd);
        }
    }
 
    T pop_back(){
        assert(sz>0);
        Node* p = getNth(sz);
        for(int i=1;sz-i>=0;i*=2){
            Node* k=getNth(sz-i);
            k->p.pop_back();
        }
        T k = p->data;
        sz--;
        delete p;
        return k;
    }
};
```



#### 기존 해결책과의 대조

Linked List를 구현하여, 이와 성능을 비교하여 보았다. Linked List는 다음과 같이 구현하였다.

```cpp
template<typename T>
class LinkedList{
private:
    struct Node{
        T data;
        Node* nxt = nullptr;
    };
    int sz;
    Node head;
    
    Node* getNth(int n){
        Node* p = &head;
        for(int i=0;i<n;i++)p=p->nxt;
        return p;
    }
public:
    LinkedList():sz(0){}
    ~LinkedList(){
        while(sz!=0){
            pop_back();
        }
    }
 
    int size(){
        return sz;
    }
 
    T getNthValue(int n){
        assert(n>0&&n<=sz);
        return getNth(n)->data;
    }
    T operator[](int n){
        return getNthValue(n);
    }
 
    void push_back(T data){
        Node* p = getNth(sz);
        sz++;
        p->nxt = new Node;
        p->nxt->data = data;
    }
    
    T pop_back(){
        Node * p = getNth(sz-1);
        T r = p->nxt->data;
        delete p->nxt;
        p->nxt = nullptr;
        sz--;
        return r;
    }
};
```

다음과 같은 테스트 함수를 작성하여 총 10회 호출하였고, 아래 이미지와 같은 테스트 결과를 얻었다. Push Back Test는 총 10,000개의 난수를 삽입하는 시간을 측정한 것이고, Random Access Test는 임의의 인덱스를 100,000회 접근한 시간을 측정한 것이다. 또한 Pop Back Test는 삽입된 10,000개의 난수를 삭제하는 시간을 측정한 것이다. 

```cpp
const int SZ = 10000;
 
LinkedList<int> ll;
SkipList<int> sl;
 
void PU_ll(){
    int r = rand();
    ll.push_back(r);
}
 
void PU_sl(){
    int r = rand();
    sl.push_back(r);
}
 
void RA_ll(){
    int r,p;
    r = rand()%SZ+1;
    p = ll[r];
}
 
void RA_sl(){
    int r,p;
    r = rand()%SZ+1;
    p = sl[r];
}
 
void PO_ll(){
    ll.pop_back();
}
 
void PO_sl(){
    sl.pop_back();
}
 
void timeCheck(void (*fp)(),string name="X",int rec=10000){
    clock_t st,ed;
    st = clock();
    for(int i=0;i<rec;i++){
        fp();
    }
    ed = clock();
    printf("[%s] %dms\n",name.c_str(),(int)(ed-st));
}
```

![Result of Several tests](https://user-images.githubusercontent.com/46587635/128126685-a5bbf1e1-8a47-4946-840e-9ebffaf8bef0.png)

이 데이터를 정리하면, 다음과 같은 결과를 얻을 수 있다. 특히 Random Access Test의 경우가 차이가 큰데, 접근하는 인덱스에 따라 Linked List는 굉장히 큰 시간복잡도를 가지나, Skip List의 경우에는 매우 많은 데이터에 대해서도 굉장히 안정적이고 높은 성능을 보임을 알 수 있다.

![Result table of tests](https://user-images.githubusercontent.com/46587635/128126754-f4161d80-266d-410a-849c-59123dad6870.png)

#### 전체 코드(테스트 코드 포함)

```cpp
#include<cstdio>
#include<cstdlib>
#include<iostream>
#include<string>
#include<ctime>
 
#include<cassert>
#include<vector>
using namespace std;
 
template<typename T>
class SkipList{
private:
    struct Node{
        T data;
        vector<Node*> p;
    };
    Node head;
    int sz;
 
    Node* getNth(int n){
        assert(n>=0&&n<=sz);
        Node* p = &head;
        while(n){
            int msb = __builtin_clz(n);
            int k=31-msb;
            p = p->p[k];
            n^=(1<<k);
        }
        return p;
    }
public:
    SkipList() : sz(0){}
    ~SkipList(){
        if(sz==0)return;
        Node* p = &head;
        while(sz){
            pop_back();
        }
    }
 
    int size(){
        return sz;
    }
 
    T getNthValue(int n){
        assert(n>0&&n<=sz);
        return getNth(n)->data;
    }
    T operator[](int n){
        return getNthValue(n);
    }
 
    void push_back(T data){
        Node* nd = new Node;
        nd->data = data;
        sz++;
        for(int i=1;sz-i>=0;i*=2){
            getNth(sz-i)->p.push_back(nd);
        }
    }
 
    T pop_back(){
        assert(sz>0);
        Node* p = getNth(sz);
        for(int i=1;sz-i>=0;i*=2){
            Node* k=getNth(sz-i);
            k->p.pop_back();
        }
        T k = p->data;
        sz--;
        delete p;
        return k;
    }
};
 
template<typename T>
class LinkedList{
private:
    struct Node{
        T data;
        Node* nxt = nullptr;
    };
    int sz;
    Node head;
    
    Node* getNth(int n){
        Node* p = &head;
        for(int i=0;i<n;i++)p=p->nxt;
        return p;
    }
public:
    LinkedList():sz(0){}
    ~LinkedList(){
        while(sz!=0){
            pop_back();
        }
    }
 
    int size(){
        return sz;
    }
 
    T getNthValue(int n){
        assert(n>0&&n<=sz);
        return getNth(n)->data;
    }
    T operator[](int n){
        return getNthValue(n);
    }
 
    void push_back(T data){
        Node* p = getNth(sz);
        sz++;
        p->nxt = new Node;
        p->nxt->data = data;
    }
    
    T pop_back(){
        Node * p = getNth(sz-1);
        T r = p->nxt->data;
        delete p->nxt;
        p->nxt = nullptr;
        sz--;
        return r;
    }
};
 
const int SZ = 10000;
 
LinkedList<int> ll;
SkipList<int> sl;
 
void PU_ll(){
    int r = rand();
    ll.push_back(r);
}
 
void PU_sl(){
    int r = rand();
    sl.push_back(r);
}
 
void RA_ll(){
    int r,p;
    r = rand()%SZ+1;
    p = ll[r];
}
 
void RA_sl(){
    int r,p;
    r = rand()%SZ+1;
    p = sl[r];
}
 
void PO_ll(){
    ll.pop_back();
}
 
void PO_sl(){
    sl.pop_back();
}
 
void timeCheck(void (*fp)(),string name="X",int rec=10000){
    clock_t st,ed;
    st = clock();
    for(int i=0;i<rec;i++){
        fp();
    }
    ed = clock();
    printf("[%s] %dms\n",name.c_str(),(int)(ed-st));
}
 
void TEST(){
    puts("==============================");
    
    puts("Push Back Test");
    timeCheck(PU_ll,"Linked List",SZ);
    timeCheck(PU_sl,"Skip   List",SZ);
    puts("Random Access Test");
    timeCheck(RA_ll,"Linked List",100000);
    timeCheck(RA_sl,"Skip   List",100000);
    puts("Pop Back Test");
    timeCheck(PO_ll,"Linked List",SZ);
    timeCheck(PO_sl,"Skip   List",SZ);
 
    puts("==============================");
    puts("");
}
 
int main(void){
    for(int i=1;i<=10;i++){
        printf("Test %d\n",i);
        TEST();
    }
}
```

