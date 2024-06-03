---
title: '[선형대수] 행렬식'
categories: [수학, 선형대수]
tags: [math, linear-algebra, determinants]
---

# 행렬식

앞에서 배웠던 행렬식에 대해 복습해보자. 

$$
A = 
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

위 행렬$$A$$가 invertable하기 위한 필요충분 조건은 $$ad - bc \neq 0$$이다. 
행렬식은 $$det(A) = ad - bc$$ 또는 $$\begin{vmatrix} a & b \\ c & d \end{vmatrix} = ad - bc$$ 로 표기한다. 
또한 $$A^{-1} = \frac{1}{det(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$ 로 행렬 $$A$$의 역행렬을 구할 수 있다.

## 여인수와 행렬식

## 소행렬식과 여인수

$$
A = 
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
$$

행렬 $$A$$가 정방행렬이라면 행렬의 i번째 행과 j번째 열을 제거해서 소행렬식을 만들 수 있다. 소행렬식은 $$M_{ij}$$로 표기한다. 
위 행렬 $$A$$의 $$M_{12}$$를 구하면 다음과 같다.

$$
M_{12} = 
\begin{vmatrix}
b & c \\
h & i
\end{vmatrix} =
bi - ch
$$

여인수는 $$M_{ij}$$에 $$(-1)^{i + j}$$를 곱한 값이다. $$C_{ij}$$로 표기한다. 
위 소행렬식 $$M_{12}$$를 이용해 여인수를 구하면 다음과 같다.

$$
C_{12} = (-1)^{i + j}M_{ij} = 
-(bi - ch)
$$

## 여인수 전개

여인수 전개를 이용해 행렬식을 구할 수 있다.

$$
A = \begin{bmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{bmatrix} \\
det(a) = \begin{vmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{vmatrix} = \\
a_{11}C_{11} + a_{12}C_{12} =\\
a_{11}C_{11} + a_{21}C_{22} =\\
a_{21}C_{21} + a_{22}C_{22} =\\
a_{12}C_{12} + a_{22}C_{22}
$$

이처럼 여인수 전개는 행과 열을 선택하여 전개가 가능하다. 
그래서 여인수 전개를 할 때는 0을 가장 많이 포함하고있는 행과열을 선택하는게 계산하기 편하다.

# 행렬식 특성

* 행렬 $$A$$가 정방행렬이며 영행 또는 영열을 가지면 $$det(A) = 0$$이다.
* 행렬 $$A$$가 정방행렬이라면 $$det(A) = det(A^{T})$$ 이다.

## 기본 행연산

행렬 $$A$$를 정방행렬이라고 가정한다.

$$
\begin{vmatrix}
ka_{11} & ka_{12} \\
a_{21} & a_{22}
\end{vmatrix} = k
\begin{vmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{vmatrix}
$$

* 행렬 $$B$$가 행렬 $$A$$에 한 행/열에 스칼라 $$k$$를 곱해서 얻어진 행렬이라면 $$det(B) = kdet(A)$$이다.

$$
\begin{vmatrix}
a_{21} & a_{22} & a_{23} \\
a_{11} & a_{12} & a_{13} \\
a_{31} & a_{32} & a_{33}
\end{vmatrix} = -
\begin{vmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{vmatrix}
$$

* 행렬 $$B$$가 행렬 $$A$$에 두 행/열을 교환해서 얻어진 행렬이라면 $$det(B) = -det(A)$$이다.

$$
\begin{vmatrix}
a_{11} + ka_{21} & a_{12} + ka_{22} & a_{13} + ka_{23} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{vmatrix} = 
\begin{vmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{vmatrix}
$$

* 행렬 $$B$$가 행렬 $$A$$에 한 행/열에 스칼라 $$k$$를 곱한 후 다른 행/열을 더해서 얻어진 행렬이라면 $$det(B) = det(A)$$이다.

$$
\begin{vmatrix}
a_{11} & a_{12} & a_{13} \\
2a_{11} & 2a_{12} & 2a_{13} \\
-4a_{11} & -4a_{12} & -4a_{13}
\end{vmatrix} = 0
$$

* 행렬 $$A$$가 비례하는 행을 가진 행렬이라면 $$det(A) = 0$$이다.

## 기본행렬

행렬 $$E$$를 정방행렬이라고 가정한다.

$$
\begin{vmatrix}
1 & 0 & 0 \\
0 & k & 0 \\
0 & 0 & 1
\end{vmatrix} = k
$$

* 행렬 $$E$$가 행렬 $$I_{n}$$에 한 행에 0이 아닌 스칼라 $$k$$를 곱해서 얻어진 행렬이라면 $$det(E) = k$$이다.

$$
\begin{vmatrix}
0 & 1 & 0 \\
1 & 0 & 0 \\
0 & 0 & 1
\end{vmatrix} = -1
$$

* 행렬 $$E$$가 행렬 $$I_{n}$$의 두 행/열을 교환해서 얻어진 행렬이라면 $$det(E) = -1$$이다.

$$
\begin{vmatrix}
1 & 0 & k \\
0 & 1 & 0 \\
0 & 0 & 1
\end{vmatrix} = 1
$$

* 행렬 $$E$$가 행렬 $$I_{n}$$의 한 행의 배수를 다른 행에 더해서 얻어진 행렬이라면 $$det(E) = 1$$이다.

# 행 축소에 의한 행렬식 계산

기본 행연산을 이용하여 행렬식 계산 과정을 단축시킬 수 있다.

$$A = 
\begin{bmatrix}
0 & 1 & 5 \\
3 & -6 & 9 \\
2 & 6 & 1
\end{bmatrix}
$$

$$det(A) = 
\begin{vmatrix}
0 & 1 & 5 \\
3 & -6 & 9 \\
2 & 6 & 1
\end{vmatrix} \\ = 3
\begin{vmatrix}
0 & 1 & 5 \\
1 & -2 & 3 \\
2 & 6 & 1
\end{vmatrix} \\ = -3
\begin{vmatrix}
1 & -2 & 3 \\
0 & 1 & 5 \\
2 & 6 & 1
\end{vmatrix} \\ = -3
\begin{vmatrix}
1 & -2 & 3 \\
0 & 1 & 5 \\
0 & 10 & -5
\end{vmatrix} \\ = -15
\begin{vmatrix}
1 & -2 & 3 \\
0 & 1 & 5 \\
0 & 2 & -1
\end{vmatrix} \\ = -15
\begin{vmatrix}
1 & 5 \\
2 & -1
\end{vmatrix} = 165
$$

