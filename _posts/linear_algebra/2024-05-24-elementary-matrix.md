---
title: '[선형대수] 기본행렬과 역행렬'
categories: [수학, 선형대수]
tags: [math, linear-algebra, elementary-matrix]
date: 2024-05-24T12:15:00 Asia/Seoul
---

# Elementary Matrix

$$
\mathit{I} =
\begin{bmatrix} 
   1 & 0  \\
   0 & 1  \\
\end{bmatrix}
,
\mathit{E} =
\begin{bmatrix} 
   1 & 0  \\
   0 & 3  \\
\end{bmatrix} 
$$

unit matrix(단위 행렬)에서 기본 행연산을 한 번만 수행한 행렬. 
$$\mathit{E}$$ 와 $$\mathit{I}$$ 는 기본 행연산을 통해 변환이 가능하므로 **row equivalent**(행동등)이라고 한다. 

$$
2행에 3을 곱한 뒤 \frac{1}{3} 곱하기 \\
\begin{bmatrix} 
   1 & 0  \\
   0 & 1  \\
\end{bmatrix}
\rightarrow
\begin{bmatrix} 
   1 & 0  \\
   0 & 3  \\
\end{bmatrix}
\rightarrow
\begin{bmatrix} 
   1 & 0  \\
   0 & 1  \\
\end{bmatrix}
$$

단위행렬을 기본 행연산을 통해 기본행렬을 얻고, 다시 단위 행렬로 바꿀 때, 이를 **inverse operation**(역연산)라고 한다. 
모든 기본행렬과 기본행렬의 역은 invertable(가역)이다.

## 동등 정리

$$A$$가 정방 행렬일 때, 아래 명제들은 전부 참이거나 거짓이다.

1. A는 가역이다.
2. $$Ax= 0$$은 trivial solution(자명해)만을 갖는다.
3. $$A$$의 reduced row echelon form은 $$I_{n}$$이다.
4. $$A$$는 elementart matrix들의 곱으로 나타낼 수 있다.

## 역행렬 구하기

행렬 $$A$$의 reducde row echelon form이 $$I_{n}$$이라고 할 때, $$A$$에 기본행렬 연산을 통해 $$I_{n}$$ 구하는 식은 다음과 같다.

$$E_{k} \cdots E_{2}E_{1}A = I_{n}$$

이때 $$A$$의 역행렬인 $$A^{-1}$$을 양변에 곱하면 다음과 같은 식을 얻을 수 있다.

$$
E_{k} \cdots E_{2}E_{1}AA^{-1} = I_{n}A^{-1} \\
E_{k} \cdots E_{2}E_{1}I_{n} = A^{-1}
$$

그러므로 $$A$$를 $$I_{n}$$으로 만들기 위한 연산이 $$I_{n}$$에서 $$A^{-1}$$로 만드는 연산에도 동일하게 적용됨을 알 수 있다.
