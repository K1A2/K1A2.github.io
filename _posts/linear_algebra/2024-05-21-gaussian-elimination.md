---
title: '[선형대수] 가우스 소거법'
categories: [수학, 선형대수]
tags: [math, linear-algebra, gaussian-elimination]
---

# Row Echelon Form

## 예제

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 10 & 2  \\
   0 & 1 & 0  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 10 & 2 & 9 \\
   0 & 1 & 0 & 4 \\
   0 & 0 & 1 & 9 \\
   0 & 0 & 0 & 0 \\
   \end{bmatrix} 
\end{equation}
$$

## 조건

1. 행렬의 행벡터에서 맨 처음 0이 아닌 수는 1이어야 함. 이를 **Leading 1**이라고 함.
2. 모든 원소가 영인 행은 맨 아래에 있어야 함.
3. 0이 아닌 벡터가 연속해서 존재할 때, 아래행의 Leading 1은 위 행 보다 오른쪽에 있어야 함.

# Reduced Row Echelon Form

## 예제

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & 0  \\
   0 & 1 & 0  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & 0 & 9 \\
   0 & 1 & 0 & 4 \\
   0 & 0 & 1 & 9 \\
   0 & 0 & 0 & 0 \\
   \end{bmatrix} 
\end{equation}
$$

## 조건

Row Echelon Form의 조건에서 한 조건이 추가된다.

1. 행렬의 행벡터에서 맨 처음 0이 아닌 수는 1이어야 함. 이를 **Leading 1**이라고 함.
2. 모든 원소가 영인 행은 맨 아래에 있어야 함.
3. 0이 아닌 벡터가 연속해서 존재할 때, 아래행의 Leading 1은 위 행 보다 오른쪽에 있어야 함.
4. **Leading 1이 있는 열의 나머지 성분은 모두 0 이어야 함.**

# Gaussian Elimination

행렬의 기본 행 연산을 이용하여 Row Echelon Form을 구하는 연산.

## 예제

$$
\begin{equation}
   \begin{bmatrix} 
   2 & 3 & 1  \\
   3 & 2 & 2  \\
   4 & 6 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

위 행렬을 Gaussian Elimination을 적용해 Row Echelon Form을 구하는 과정이다.

1. 1행과 2행 교체  
$$
\begin{equation}
   \begin{bmatrix} 
   3 & 2 & 2  \\
   2 & 3 & 1  \\
   4 & 6 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

2. 1행에 2행 빼기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   2 & 3 & 1  \\
   4 & 6 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

3. 2행에 2를 곱한 1행 빼기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   0 & 5 & -1  \\
   4 & 6 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

4. 3행에 4를 곱한 1행 빼기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   0 & 5 & -1  \\
   0 & 10 & -3  \\
   \end{bmatrix} 
\end{equation}
$$

5. 3행에 2를 곱한 2행 빼기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   0 & 5 & -1  \\
   0 & 0 & -1  \\
   \end{bmatrix} 
\end{equation}
$$

6. 2행에 5 나누기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   0 & 1 & \frac{-1}{5}  \\
   0 & 0 & -1  \\
   \end{bmatrix} 
\end{equation}
$$

7. 3행에 -1 곱하기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & -1 & 1  \\
   0 & 1 & \frac{-1}{5}  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

# Gauss-Jordan Elimination

행렬의 기본 행 연산을 이용하여 Reduced Row Echelon Form을 구하는 연산. 
Gaussian Elimination이 선행된 후 진행된다.

## 예제

위 예제 결과에서 이어서 작성된다.

8. 1행에 2행 더하기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & \frac{4}{5}  \\
   0 & 1 & \frac{-1}{5}  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

9. 1행에 $$\frac{-4}{5}$$를 곱한 3행 더하기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & 0  \\
   0 & 1 & \frac{-1}{5}  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

10. 2행에 $$\frac{1}{5}$$를 곱한 3행 더하기  
$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & 0  \\
   0 & 1 & 0  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$
