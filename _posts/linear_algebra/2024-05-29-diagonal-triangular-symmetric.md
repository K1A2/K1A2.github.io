---
title: '[선형대수] 대각행렬, 삼각행렬, 대칭행렬'
categories: [수학, 선형대수]
tags: [math, linear-algebra, diagonal-matrix, triangular-matrix, symmetric-matrix]
date: 2024-05-27T10:23:00 Asia/Seoul
---

# Diagonal Matrix

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 5 & 0 \\
0 & 0 & 1
\end{bmatrix},
$$

Diagonal Matrix(대각행렬)은 main diagonal(주대각)원소 외에 모든 원소가 0인 행렬을 뜻한다.

$$
D = \begin{bmatrix}
d_{11} & 0 & 0 \\
0 & d_{22} & 0 \\
0 & 0 & d_{33}
\end{bmatrix} \\
D^{-1} = \begin{bmatrix}
\frac{1}{d_{11}} & 0 & 0 \\
0 & \frac{1}{d_{22}} & 0 \\
0 & 0 & \frac{1}{d_{33}}
\end{bmatrix} 
$$

대각행렬의 역행렬은 위와 같다.


# Triangular Matrix

$$
\begin{bmatrix}
1 & 3 & 4 \\
0 & 5 & 2 \\
0 & 0 & 1
\end{bmatrix},
\begin{bmatrix}
1 & 0 & 0 \\
3 & 5 & 0 \\
4 & 5 & 1
\end{bmatrix}
$$

Tirangular Matrix(삼각행렬)은 주대각선을 기준으로 위나 아래가 모두 0인 함수를 뜻한다. 
주대각선 기준으로 위쪽이 모두 0인 행렬을 Lower Tirangular Matrix(하삼각행렬), 아래쪽이 모두 0인 행렬을 Upper Tirangular Matrix(상삼각행렬)라고 한다. 두 행렬 모두 Tirangular Matrix(삼각행렬)이다.

## 성질

1. 하삼각행렬의 전치는 상삼각행렬이고, 상삼각행렬의 전치는 하삼각행렬이다.
2. 하삼각행렬의 곱은 하삼각행렬이고, 상삼각행렬의 곱은 상삼각행렬이다.
3. 삼각행렬이 invertable하다먄 주대각 원소 모두 0이 아니다.
4. invertable한 상삼각행렬의 역행렬은 상삼각행렬이고, 이는 하삼각행렬도 같다.


# Symmetric Matrix

$$
A = \begin{bmatrix}
1 & 3 & 4 \\
3 & 5 & 2 \\
4 & 2 & 1
\end{bmatrix},
A^{T} = \begin{bmatrix}
1 & 3 & 4 \\
3 & 5 & 2 \\
4 & 2 & 1
\end{bmatrix}
$$

Symmetric Matrix(대칭행렬)은 전치한 행렬과 원래 행렬이 같은 행렬이다. 
대칭행렬 $$A$$가 가역이면 $$A^{-1}$$, $$AA^{T}$$, $$A^{T}A$$도 가역이다.
