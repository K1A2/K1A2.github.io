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
c_{21} & d_{22}
\end{bmatrix} \\
det(a) = \begin{vmatrix}
a_{11} & a_{12} \\
c_{21} & d_{22}
\end{vmatrix} = \\
a_{11}C_{11} + a_{12}C_{12} =\\
a_{11}C_{11} + a_{21}C_{22} =\\
a_{21}C_{21} + a_{22}C_{22} =\\
a_{12}C_{12} + a_{22}C_{22}
$$

이처럼 여인수 전개는 행과 열을 선택하여 전개가 가능하다. 
그래서 여인수 전개를 할 때는 0을 가장 많이 포함하고있는 행과열을 선택하는게 계산하기 편하다.
