---
title: '[선형대수] 연립일차방정식과 역행렬'
categories: [수학, 선형대수]
tags: [math, linear-algebra, elementary-matrix]
date: 2024-05-27T10:23:00 Asia/Seoul
---

# inverse matrix와 연립일차방정식 관계

행렬 A가 정방행렬이면서 invertable 하다면 연립방정식 $$Ax=b$$는 $$x = A^{-1}b$$가 성립한다.

## 예제

$$
A = \begin{bmatrix}
1 & 2 & 3 \\
0 & 1 & 4 \\
5 & 6 & 0
\end{bmatrix},
x = \begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix},
b = \begin{bmatrix}
3 \\
7 \\
2
\end{bmatrix}
$$

### 1. $$A^{-1}$$ 구하기

$$
\left[
   \begin{array}{ccc|ccc}
   1 & 2 & 3 & 1 & 0 & 0\\
   0 & 1 & 4 & 0 & 1 & 0 \\
   5 & 6 & 0 & 0 & 0 & 1
   \end{array}
\right] \\
\left[
   \begin{array}{ccc|ccc}
   1 & 2 & 3 & 1 & 0 & 0\\
   0 & 1 & 4 & 0 & 1 & 0 \\
   0 & -4 & -15 & -5 & 0 & 1
   \end{array}
\right] \\
\left[
   \begin{array}{ccc|ccc}
   1 & 2 & 3 & 1 & 0 & 0\\
   0 & 1 & 4 & 0 & 1 & 0 \\
   0 & 0 & 1 & -5 & 4 & 1
   \end{array}
\right] \\
\left[
   \begin{array}{ccc|ccc}
   1 & 2 & 3 & 1 & 0 & 0\\
   0 & 1 & 0 & 20 & -15 & -4 \\
   0 & 0 & 1 & -5 & 4 & 1
   \end{array}
\right] \\
\left[
   \begin{array}{ccc|ccc}
   1 & 2 & 0 & 16 & -12 & -3\\
   0 & 1 & 0 & 20 & -15 & -4 \\
   0 & 0 & 1 & -5 & 4 & 1
   \end{array}
\right] \\
\left[
   \begin{array}{ccc|ccc}
   1 & 0 & 0 & -24 & 18 & 5\\
   0 & 1 & 0 & 20 & -15 & -4 \\
   0 & 0 & 1 & -5 & 4 & 1
   \end{array}
\right] \\
A^{-1} = \begin{bmatrix}
-24 & 18 & 5 \\
20 & -15 & -4 \\
-5 & 4 & 1
\end{bmatrix}
$$

### 2. $$x$$ 구하기

$$
A^{-1}b = 
\begin{bmatrix}
-24 & 18 & 5 \\
20 & -15 & -4 \\
-5 & 4 & 1
\end{bmatrix}
\begin{bmatrix}
3 \\
7 \\
2
\end{bmatrix} = 
\begin{bmatrix}
64 \\
-53 \\
15
\end{bmatrix}
$$

# 역행렬 성질

## 동등정리

이전 [[선형대수] 역행렬과 행렬 성질]({% post_url linear_algebra/2024-05-24-elementary-matrix %})에서 정리한 동등정리에 두 가지 명제가 더 추가될 수 있다.

1. A는 가역이다.
2. $$Ax= 0$$은 trivial solution(자명해)만을 갖는다.
3. $$A$$의 reduced row echelon form은 $$I_{n}$$이다.
4. $$A$$는 elementart matrix들의 곱으로 나타낼 수 있다.
5. $$Ax=b$$는 모든 $$n \times 1$$ 행렬 $$b$$에 대해서 일치한다.
6. $$Ax=b$$는 모든 $$n \times 1$$ 행렬 $$b$$에 대해 유일한 해를 갖는다.

## 정방행렬의 곱과 가역

행렬 $$A$$와 $$B$$가 크기가 같은 정방행렬일 때, $$AB$$가 가역이면 행렬 $$A$$와 $$B$$도 가역이다.

# 행렬이 정방행렬이나 가역이 아닐때 해 구하기

## 예제

$$
A = \begin{bmatrix}
1 & 2 & 3 \\
2 & 5 & 11 \\
2 & 3 & 1
\end{bmatrix},
x = \begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix},
b = \begin{bmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{bmatrix} \\
\begin{bmatrix}
1 & 2 & 3 & b_{1} \\
2 & 5 & 11 & b_{2} \\
2 & 3 & 1 & b_{3}
\end{bmatrix} \\
\begin{bmatrix}
1 & 2 & 3 & b_{1} \\
2 & 3 & 1 & b_{3} \\
0 & 1 & 5 & -2b_{1} + b_{2}
\end{bmatrix} \\
\begin{bmatrix}
1 & 2 & 3 & b_{1} \\
0 & -1 & -5 & -2b_{1} + b_{3} \\
0 & 1 & 5 & -2b_{1} + b_{2}
\end{bmatrix} \\
\begin{bmatrix}
1 & 2 & 3 & b_{1} \\
0 & 1 & 5 & -2b_{1} + b_{2} \\
0 & 0 & 0 & -4b_{1} + b_{3} + b_{2}
\end{bmatrix}
$$

이때 $$-4b_{1} + b_{3} + b_{2} = 0$$이므로 $$b_{3} = 4b_{1} + b_{2}$$가 성립한다. 
그러므로 이 연립방정식이 해를 갖기 위한 조건은 다음과 같다.

$$
b = \begin{bmatrix}
b_{1} \\
b_{2} \\
4b_{1} + b_{2}
\end{bmatrix}
$$
