---
title: '[자료구조] C로 동적배열 만들기'
categories: [개발, c]
tags: [c, data-structure, dynamic-array]
---

소프트마에스트로 면접때 옆 사람에게 자료구조에 대한 질문이 들어왔습니다.
그때 면접관님꼐서 질문 하시면서 덧붙인 말씀이 생각 생각납니다.

> 비전공자라 **기초**를 잘 알고 있는지 확인하고싶어서 물어봤어요.

만약 그때 그 질문이 전공자인 저에게 왔다면 저는 **"기초적인"** 대답을 하지 못하고, 결국 면접에서 탈락했을 겁니다.  
그때 문득 파이썬, 자바, 다트와 같은 언어를 사용하면서 클래스로 만들어져 있는 자료구조를 가져다 쓰기만 해봤지 동작 원리나 과정을 자세히 공부해 본 적이 없다는 생각이 들었습니다. 그래서 다양한 자료구조를 gpt와 구글의 도움을 받아 C언어로 직접 구현해 보려고 합니다. 오늘은 첫 번째로 동적배열에 대해 알아보고 구현해볼 겁니다.


# 동적 배열?

C언어를 하다보면 그냥 배열은 꽤 친숙하실 겁니다. 자료형 뒤에 대괄호만 붙이면 고정된 크기의 배열은 쉽게 만들고 사용할 수 있으니까요. 하지만 파이썬, 자바와 같은 다른 언어를 보면 선언 시 배열의 크기를 지정하지 않고, 마음대로 데이터를 넣고 삭제할 수 있는 자료형이 있습니다. 이렇게 지정된 크기 없이 배열의 크기를 동적으로 조절할 수 있는 배열을 **동적 배열**이라고 합니다.


# 동작 원리

동적 배열은 내부적으로 **배열**을 생성해서 데이터를 저장하고 불러옵니다. 
일정한 크기의 배열을 미리 선언해두고, 선언한 배열의 크기를 모두 사용하면 추가적으로 늘려 데이터를 계속 추가하는 원리입니다. 

![da1](/assets/img/2024-04-06-dynamic-array/da1.webp){: width="250px"}

![da2](/assets/img/2024-04-06-dynamic-array/da2.webp){: width="250px"}

![da3](/assets/img/2024-04-06-dynamic-array/da3.webp){: width="450px"}

위 예제는 4 크기로 선언된 배열에 데이터를 추가하다가 4칸을 모두 사용하게 되었을때, 8로 리사이즈해서 데이터를 추가적으로 넣는 예제입니다. 
배열의 크기를 리사이즈하며 바꾸는것 외에는 대부분의 동작이 배열과 동일합니다. 자세한 동작 원리는 코드를 보며 설명하겠습니다.

## 구조체 선언

```c
typedef struct {
    int *array; // 내부적으로 데이터를 저장 할 배열
    size_t used; // 현재 사용중인 배열 개수
    size_t size; // 현재 할당된 배열 개수
} Array;
```

int *array **포인터**를 활용하여 동적 배열을 표현합니다. 
현재 사용중인 크기와 선언된 배열의 크기는 **size_t** 자료형을 이용하여 표현합니다.

### size_t?

size_t 자료형은 주로 **배열의 크기, 메모리, 문자열 길이** 등 음수가 아닌 값을 표현할때 사용합니다. 
그렇다면 왜 크기 표현에 int나 unsigned int를 사용하지 않고 sized_t를 사용할까요?  

size_t는 **항상 모든 타입 사이즈에 상응하는 용량을 가진다는 것을 보장**합니다. 
unsinged int를 예로 들자면, unsinged int는 32비트 운영체재에서 32비트 정수, 64비트 운영체제에서는 64비트 정수라고 **항상 보장하지 않습니다.** 
이는 예기치 않은 오류로 이어질 수 있습니다. 
그래서 메모리 크기, idx와 같은 정보를 표현할 때 size_t 자료형을 사용합니다.

## 배열 초기화

```c
void initArray(Array *a, size_t initialSize) {
    // init size 만큼 배열 할당
    a->array = (int*)malloc(initialSize * sizeof(int));
    // 현재 사용중인 배열 크기 0으로 초기화
    a->used = 0;
    // 할당된 사이즈 개수 초기화
    a->size = initialSize;
}
```

배열을 선언하는 함수입니다. 
구조체 Array와 초기 배열 사이즈를 파라미터로 받아 동적 배열의 기본 정보를 초기화 합니다.

## 데이터 맨 뒤에 추가

```c
void insertArray(Array *a, int element) {
    // 할당된 배열의 크기를 모두 사용중일 때
    if (a->used == a->size) {
        // 현재 배열 사이즈 * 2 사이즈로 늘림
        a->size *= 2;
        a->array = (int*)realloc(a->array, a->size * sizeof(int));
    }
    // 맨 뒤에 값 추가
    a->array[a->used++] = element;
}
```

배열에 값을 추가하는 함수입니다. 
할당된 배열의 크기를 모두 사용중인지 검사 후, 이미 모두 사용했다면 **realloc**을 이용하여 기존 사이즈 * 2배로 크기를 늘립니다. 
그리고 현재 사용중인 사이즈+1 위치에 데이터를 추가합니다.

## 데이터를 중간에 삽입

```c
void insertAtIndexArray(Array *a, size_t index, int element) {
    // 현재 사용중인 사이즈보다 타겟 idx가 크면 함수 종료
    if (index > a->used) return;

    // 할당된 배열의 크기를 모두 사용중일 때
    if (a->used == a->size) {
        // 현재 배열 사이즈 * 2 사이즈로 늘림
        a->size *= 2;
        a->array = (int*)realloc(a->array, a->size * sizeof(int));
    }
    // 타겟 idx의 값 부터 한 칸씩 뒤로 미룸
    memmove(&a->array[index + 1], &a->array[index], (a->used - index) * sizeof(int));
    // 타겟 idx에 값 추가
    a->array[index] = element;
    a->used++;
}
```

배열 중간에 값을 삽입하는 함수입니다. 
사용중인 배열 크기와 할당 크기를 검사하는 부분까지는 데이터 추가 함수와 같지만, 원하는 idx에 값을 넣는 부분이 다릅니다. 
**memove** 함수를 사용해서 값을 넣으려는 idx부터 모두 뒤로 한 칸씩 미루고, idx에 값을 삽입합니다.

## 데이터 삭제

```c
void removeAtIndexArray(Array *a, size_t index) {
    // 현재 사용중인 사이즈보다 타겟 idx가 크면 함수 종료
    if (index >= a->used) return;

    // 타겟 idx + 1 부터 한 칸씩 앞으로 당겨 타겟 idx 값 삭제
    memmove(&a->array[index], &a->array[index + 1], (a->used - index - 1) * sizeof(int));
    a->used--;
}
```

원하는 idx의 값을 삭제하는 함수입니다. 
타겟 idx + 1 번째 값 부터 한 칸씩 앞으로 당겨 데이터를 삭제합니다. 
좀 더 정확히는 idx의 값을 idx + 1의 값으로 덮어쓰기 합니다. 

# 전체 코드

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int *array;
    size_t used;
    size_t size;
} Array;

void initArray(Array *a, size_t initialSize) {
    a->array = (int*)malloc(initialSize * sizeof(int));
    a->used = 0;
    a->size = initialSize;
}

void insertArray(Array *a, int element) {
    if (a->used == a->size) {
        a->size *= 2;
        a->array = (int*)realloc(a->array, a->size * sizeof(int));
    }
    a->array[a->used++] = element;
}

void insertAtIndexArray(Array *a, size_t index, int element) {
    if (index > a->used) return;

    if (a->used == a->size) {
        a->size *= 2;
        a->array = (int*)realloc(a->array, a->size * sizeof(int));
    }
    memmove(&a->array[index + 1], &a->array[index], (a->used - index) * sizeof(int));
    a->array[index] = element;
    a->used++;
}

void removeAtIndexArray(Array *a, size_t index) {
    if (index >= a->used) return;

    memmove(&a->array[index], &a->array[index + 1], (a->used - index - 1) * sizeof(int));
    a->used--;
}

void printArray(Array *a) {
    for (size_t i = 0; i < a->used; i++) {
        printf("%d ", a->array[i]);
    }
    printf("\n");
}

void freeArray(Array *a) {
    free(a->array);
    a->array = NULL;
    a->used = a->size = 0;
}

int main() {
    Array array;

    char command[10];
    int number;
    size_t index;

    initArray(&array, 5);

    printf("Commands: add <num>, insert <index> <num>, del <index>, print, exit\n");
    while (1) {
        printf("> ");
        scanf("%s", command);

        if (strcmp(command, "add") == 0) {
            scanf("%d", &number);
            insertArray(&array, number);
        } else if (strcmp(command, "insert") == 0) {
            scanf("%zu %d", &index, &number);
            insertAtIndexArray(&array, index, number);
        } else if (strcmp(command, "del") == 0) {
            scanf("%zu", &index);
            removeAtIndexArray(&array, index);
        } else if (strcmp(command, "print") == 0) {
            printArray(&array);
        } else if (strcmp(command, "exit") == 0) {
            break;
        } else {
            printf("Invalid command.\n");
        }
    }

    freeArray(&array);

    return 0;
}
```
