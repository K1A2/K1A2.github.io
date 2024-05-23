---
title: '[선형대수] 역행렬과 형렬 성질'
categories: [수학, 선형대수]
tags: [math, linear-algebra, inverse-matrix]
---

# Unit Matrix

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0  \\
   0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

$$
\begin{equation}
   \begin{bmatrix} 
   1 & 0 & 0  \\
   0 & 1 & 0  \\
   0 & 0 & 1  \\
   \end{bmatrix} 
\end{equation}
$$

main diagonal(주대각성분) 원소들이 모두 1이고 나머지 원소가 모두 0이면 **unit matirx**(단위 행렬) 또는 **identity matrix**(항등 행렬)라고 한다. 
표기는 $$\mathit{I}$$, 크기와 함께 표기할 땐 $$\mathit{I}_n$$으로 표기한다.

$$\mathit{I}_2A =
\begin{equation}
   \begin{bmatrix}
      1 & 0\\
      0 & 1\\
   \end{bmatrix}

   \begin{bmatrix}
      a_{11}&a_{12}&a_{13}\\
      a_{21}&a_{21}&a_{23}\\
   \end{bmatrix} =
   \begin{bmatrix}
      a_{11}&a_{12}&a_{13}\\
      a_{21}&a_{21}&a_{23}\\
   \end{bmatrix}
\end{equation} = A$$

identity matrix는 다른 행렬을 곱하면 그 행렬이 결과값으로 나온다. 실수 연산의 1과 같은 역할.

# Inverse Matrix

정방행렬 A와 B가 $$AB = BA = \mathit{I}$$ 를 만족할 때, A는 **invertable**(가역) 또는 **nonsingular**(정칙)이라 하고, 
B는 A의 **inverse matrix**(역행렬)이라고 한다. 이때 A의 역행렬은 $$A^{-1}$$로 표기한다.
A에 대한 B가 존재하지 않는다면 A는 **singular matirx**(특이 행렬)이라고 한다.  
  
어떤 행렬에 대한 역행렬은 유일하다.

## 성질 1

$$A =
\begin{bmatrix}
   a&b\\
   c&d
\end{bmatrix}\\

A^{-1} = \frac{1}{ad-bc}
\begin{bmatrix}
   d&-b\\
   -c&a
\end{bmatrix}$$

위 예제에서 행렬 A가 invertable인지 확인하는 방법은 $$ad - bc \neq 0$$ 이다.

## 성질 2

A와 B가 크기가 같은 invertable matrix 라면 $$(AB)^{-1} = B^{-1}A^{-1}$$ 이다.

## 행렬 거듭제곱

A가 정방행렬일 때, $$A^{0} = \mathit{I}, A^{n} = AAA \cdots A$$  
A가 가역일 때, $$A^{-n} = (A^{-1})^{n} = A^{-1}A^{-1}A^{-1} \cdots A^{-1}$$  
지수가 음이 아닐 떄, $$A^{r}A^{s} = A^{r + s}, (A^{r})^{s} = A^{rs}$$  
  
지수가 음일 때, A가 가역, n이 양수
* $$A^{-1}$$ 도 가역, $$(A^{-1})^{-1} = A$$
* $$A^{n}$$ 도 가역, $$(A^{n})^{-1} = A^{-n} = (A^{-1})^{n}$$
* 0이 아닌 스칼라 k에 대해서 $$kA$$ 도 가역, $$(kA)^{-1} = k^{-1}A^{-1}$$

## 행렬 다항식

$$p(x) = a_{0}x^{0} + a_{1}x^{1} + \cdots + a_{n}x^{n}$$

정방행렬 A를 대입하면

$$p(A) = a_{0}A^{0} + a_{1}A^{1} + \cdots + a_{n}A^{n} = a_{0}\mathit{I} + a_{1}A^{1} + \cdots + a_{n}A^{n}$$

## 전치행렬

행렬 A가 가역이면 $$(A^{T})^{-1} = (A^{-1})^{T}$$
