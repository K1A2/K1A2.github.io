---
title: '[C] 구조체란 무엇일까?'
categories: [개발, c]
tags: [c]
---

# 구조체란?

구조체는 **타입이 다른 데이터를 하나로 묶어버리는 방법입니다.** 프로그래밍을 하다 보면 여러 자료형을 사용할 때가 있는데, 이때 사용하면 변수들을 조금 더 편리하게 사용이 가능합니다.

```c
//사람을 표현하는 방법
//방법 1 따로따로 변수 선언하기
char[100] name;
int age;
int height;
float weight;

//방법 2 구조체로 묶기
struct Person {
    char[100] name;
    int age;
    int height;
    float weight;
};
```

사람에 관한 변수들을 사용해야 할 때, 한눈에 봐도 관련 변수들을 하나로 묶어주는 방법 2가 한눈에 보기 쉽죠?

# 구조체 사용 방법

## 구조체 선언

```c
struct Person {
    char[100] name;
    int age;
    int height;
    float weight;
};
```

**struct** 이라는 키워드를 쓰고, 구조체 이름을 쓴 다음 대괄호로 묶어줄 변수들을 선언해 주면 됩니다.

## 구조체 초기화 방법

```c
struct Person {
    char[100] name;
    int age;
    int height;
    float weight;
}; //구조체 선언

int main() {
    //구조체 변수 선언 및 초기화
    struct Person p1 = {"Name1", 12, 135, 47.6};
    
    //하나씩 초기화도 가능하다.
    struct Person p2;
    strcpy(p2.name, "Name2"); // <string.h>
    p2.age = 23;
    p2.height = 175;
    p2.weight = 65.7;
    
    return 0;
}
```

구조체 변수를 선언할때는 **struct** 키워드를 사용합니다. 선언 한 뒤 바로 대괄호에 멤버들을 위에서 아래 순서대로 적어주면 바로 초기화가 가능합니다.