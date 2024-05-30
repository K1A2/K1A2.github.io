---
title: '[선형대수] 선형변환'
categories: [수학, 선형대수]
tags: [math, linear-algebra, linear-transformation]
---

# Matrix Transformation

행렬변환(matrix transformatioin)은 어떤 행렬을 다른 행렬로 변환하는것을 말한다.

$$
w_{1} = a_{11}x_{1} + a_{12}x_{2} + \cdots + a_{1n}x_{n}\\
w_{2} = a_{21}x_{1} + a_{22}x_{2} + \cdots + a_{2n}x_{n}\\
\vdots \\
w_{m} = a_{m1}x_{1} + a_{m2}x_{2} + \cdots + a_{mn}x_{n}
$$

위와같은 식은 $$w = Ax$$로 표기한다. 
$$R^{n}$$(정의역)에 속한 백터 $$x$$를 $$R^{m}$$(공역)에 속한 백터 $$w$$로 변환한다는 뜻이다. 
이때 $$n = m$$ 이라면 행렬 연산자(matrix operator)라고도 부른다.  
  
그밖에 $$T_{A} : R^{n} \rightarrow R^{m}$$, $$w=T_{A}(x)$$, $$x \xrightarrow[]{T_{A}} w$$라고도 표현 가능하다.

## 성질

모든 행렬변환 $$T_{A} : R^{n} \rightarrow R^{m}$$은 모든 백터 $$u, v$$와 스칼라 $$k$$에 대해 다음 성질을 만족한다.

1. $$T_{A}(0) = 0$$ - 0백터
2. $$T_{A}(ku) = kT_{A}(u)$$ - 동질성
3. $$T_{A}(u + v) = T_{A}(u) + T_{A}(v)$$ - 합의 성질
4. $$T_{A}(u - v) = T_{A}(u) - T_{A}(v)$$ - 

이중 2번과 3번 성질을 선형성 조건(linearity confition)이라고 하고, 이 조건을 만족하는 변환을 선형변환(linear transformation)이라고 한다. 
그러므로 모든 선형변환은 행렬변환이고, 행렬변환은 선형변환이다. 

$$T_{A} : R^{n} \rightarrow R^{m},
T_{B} : R^{n} \rightarrow R^{m} \\
T_{A}(x) = T_{B}(x), A=B$$

위 정리에 따라 행렬변환은 정확하게 하나의 행렬을 생성함을 알 수 있다. 이러한 행렬은 표준행렬(standard matrix)라고 한다.

## 표준행렬

표준행렬을 구하는 방법은 다음 예제와 같다.

$$
T: R^{2} \rightarrow R^{3} \\

T \left (
\begin{bmatrix}
x_{1} \\
x_{2}
\end{bmatrix}
\right ) = 
\begin{bmatrix}
2x_{1} + x_{2} \\
x_{1} - 3x_{2} \\ 
-x_{1} + x_{2}
\end{bmatrix} \\ 

T(e_{1}) = T \left (
\begin{bmatrix}
1 \\
0
\end{bmatrix}
\right ) = \begin{bmatrix}
2 \\
1 \\ 
-1
\end{bmatrix},
T(e_{2}) = T \left (
\begin{bmatrix}
0 \\
1
\end{bmatrix}
\right ) = \begin{bmatrix}
1 \\
-3 \\ 
1
\end{bmatrix} \\

A = \left[
\begin{array}{c|c}
T(e_{1}) & T(e_{2}) \\
\end{array}
\right] = \begin{bmatrix}
2 & 1 \\
1 & -3 \\ 
-1 & 1
\end{bmatrix} \\

A \begin{bmatrix}
x_{1} \\
x_{2} \\ 
\end{bmatrix} = \begin{bmatrix}
2 & 1 \\
1 & -3 \\ 
-1 & 1
\end{bmatrix} \begin{bmatrix}
x_{1} \\
x_{2} \\ 
\end{bmatrix} = \begin{bmatrix}
2x_{1} + x_{2} \\
x_{1} - 3x_{2} \\ 
-x_{1} + x_{2}
\end{bmatrix}
$$
