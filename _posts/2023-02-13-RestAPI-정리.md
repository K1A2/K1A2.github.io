---
title: RestAPI 정리
date: 2023-02-12 18:10:00 +0900
categories: [web, api]
tags: [rest_api]  
---

# Rest API

## Rest?

### 정의

Rest는 REpresentational State Transfer의 줄임말.

자원을 이름으로 구분하여 자원의 상태를 주고받는것을 의미.

### 구성

1. 자원(Resource) - URI
2. 행위(Verb) - Method
3. 표현(Representation of Resource)

### 특징

1. Server-Client (서버-클라이언트 구조)

2. Stateless (무상태)
3. Cacheable (캐시 처리 기능)
4. Layered System (계층 구조)
5. Uniform Interface (인터페이스 일관성)
6. Self-Descriptivness (자체 표현)

### CRUD

- 자원에 대한 4가지 행위(Verb).
- HTTP Method(GET, POST, PUT, DELETE)로 표현.
    - GET - Read(정보 요청), URI가 가진 정보를 검색하기 위해 서버에 요청
    - POST - Create(정보 입력), 클라이언트에서 서버로 전달하려는 정보를 전송
    - PUT - Update(정보 업데이트), 내용을 갱신하기 위해 사용
    - DELETE - Delete(정보 삭제), 내용을 삭제
    - Patch
    - HEAD

## Rest API?

### 정의

- REST의 원리와 특징을 따르는 API.

### 중심 규칙

1. URI는 정보의 자원을 표현해야 한다.

    ```
    GET /members/delete/1 - 잘못된 예시
    GET /members/1
    ```

2. 자원의 대한 행위는 HTTP Method로 표현해야 한다.

    ```
    GET /members/1 - 잘못된 예시
    DELETE /members/1
    ```

### 설계 기본 규칙

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 이용해야 한다.

    ```
    GET /members/getAllUsers - 잘못된 예시
    get /members/users
    ```

2. / 는 계층 관계를 표현한다.

3. URI의 마지막에는 / 를 포함하지 않는다.

4. _ 보다는 - 을 사용한다.

5. URI는 소문자로만 구성된다.

6. URI에 파일 확장자는 포함하지 않는다.

7. HTTP 응답 상태 코드로 클라이언트는 요청에 대한 피드백을 받아야 한다.

## 레퍼런스

[[네트워크] REST API란? REST, RESTful이란?](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)

[[Network] REST란? REST API란? RESTful이란? - Heee's Development Blog](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

[REST란? REST API 와 RESTful API의 차이점](https://dev-coco.tistory.com/97)

[REST API 제대로 알고 사용하기 : NHN Cloud Meetup](https://meetup.toast.com/posts/92)

[[REST API] REST / REST API 개념과 적용 + 코드 예제 (SpringBoot 기반)](https://creamilk88.tistory.com/184)

[HTTP 상태 코드 정리](https://www.whatap.io/ko/blog/40/)