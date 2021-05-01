---
title: QCQI Solution Problem 4.3
author: Xin Tong
date: 2021-03-03 08:38 +0800
categories: [Solution, QCQI]
tags: [Quantum Computing]
math: true
mermaid: flase
---

Solution to problem 4.3 on page 212

## Q1

Since $U$ is a unitary, all the eigenvalues have norm equal to $1$.

$$U = \sum_{k=0}^{2^n-1}{e^{-i\theta_k} \ket{k} \bra k}, \quad \theta_k \in[0,2\pi)$$

thus

$$H \equiv i\ln{(U)}=\sum{ \theta_k \ket{k} \bra{k} }$$

## Q2

### general idea

First, define

$$\mathbf{H}\equiv \{H\vert H:2^n\times 2^n \text{ Hermitian}\}$$

$$\mathbf{P_n} \equiv \{I,X,Y,Z\}^{\otimes n}$$

The idea is that $\mathbf{H}$ has $(2^n)^2$ degrees of freedom and $\mathbf{P_n}$ has $4^n = (2^n)^2$ number of elements. Then, if we can prove $\bf{H}$ is a vector space and $\bf{P_n}$ is a basis for $\bf{H}$, then it is obvious that for any $H \in \bf H$ it can be written as $H = \sum_g{h_g g},\quad g\in\bf{P_n}$.

### step 1

To prove $\bf{H}$ is a vector space, we can see that  $\bf H \subset \bf M$, where $\bf M$ is a vector space which contains all matrices. $\bf H$ is a subspace of $\bf M$. By checking that:
- $\bf 0\in H$
- closure under $+$: for $h_1, h_2\in \bf H$, $(h_1+h_2)^\dagger = h_1^\dagger+h_2^\dagger=(h_1+h_2), (h_1+h_2)\in \bf H$ by definition of Hermitian.
- closure under scalar $*$: $h\in \mathbf{H} \implies ah\in \mathbf H$

$\mathbf H$ is a vector space.

### step 2

To prove $\bf{P_n}$ is a basis, fortunately, we can easily prove that all the elements in $\bf{P_n}$ are linearly independent to each other by showing that they are orthogonal. 

we use the matrix inner prouct:

$$\left<A,B\right>\equiv tr(A^\dagger B)$$

When $n=1$, which is $$\mathbf{P_1}=\{I,X,Y,Z\}$$, for any two elements $\sigma_1,\sigma_2 \in \mathbf{P_1}$. we can confirm that $\left<\sigma_1,\sigma_2\right>=0$ for $\sigma_1\neq \sigma_2$.

When $n>1$, for $g_1,g_2\in \mathbf{P_n}$ we can write them as:
$$\begin{align}
g_1 & =\bigotimes_{i=0}^{n-1}{\sigma_{x_i}},\sigma_{x_i}\in\{I,X,Y,Z\} \\
g_2 & =\bigotimes_{i=0}^{n-1}{\sigma_{y_i}},\sigma_{y_i}\in\{I,X,Y,Z\}
\end{align}$$
Then 
$$\begin{align}
\left<g_1,g_2\right> & = \text{tr}(g_1^{\dagger} g_2) \\
  & = \text{tr}\left(\bigotimes_{i=0}^{n-1}{\sigma_{x_i}^\dagger}\bigotimes_{i=0}^{n-1}{\sigma_{y_i}}\right) \\
  & = \text{tr}\left(\bigotimes_{i=0}^{n-1}{\sigma_{x_i}^\dagger\sigma_{y_i}}\right) \\
  & = \prod_{i=0}^{n-1}{\left<\sigma_{x_i},\sigma_{y_i}\right>}
\end{align}$$

It is clear that, $\left<g_1,g_2\right>\neq0$ only when $\forall i, \sigma_{x_i}=\sigma_{y_i}$, which is $g_1=g_2$. Thus for any two distinctive elements in $\mathbf{P_n}$, their inner product is $0$. 

### finalizing the proof

Since $\mathbf{H}$ is a vector space with $\text{dim}(\mathbf{H})=\vert{\mathbf{P_n}}\vert=4^n$ , $\mathbf{P_n}\subset\mathbf{H}$ and all the elements in $\mathbf{P_n}$ are orthogonal  thus linearly independent, $\mathbf{P_n}$ is a basis for $\mathbf{H}$. For any $H \in \bf H$ it can be written as $H = \sum_g{h_g g},\quad h_g \in \Re, g\in\bf{P_n}$.

## Q3

$$\exp{(-ih_gg\Delta)}=\exp{(-i{h_g\over k}\bigotimes_{i=0}^{n-1}{\sigma_{i}})}$$

Based on the method and circuit proposed in section 4.7.3, we replace $\Delta t$ with $h_g/k$. Then for the $i$-th qubit, if $\sigma_i=I$, we remove the $\text{CNOT}$ gates; if $\sigma_i=X$, we add $H$ both before and after the $\text{CNOT}$ gates; if $\sigma_i=Y$, we add $U_3(\pi /2,\pi /2,\pi /2)$ before the $\text{CNOT}$s and $U_3(-\pi /2,\pi /2,\pi /2)$ after the $\text{CNOT}$s; if $\sigma_i=Z$, there should be only $\text{CNOT}$s. There're at most $4$ gates on a single wire. Thus the construction takes $\text{O}(n)$ number of one and two qubit gates.

## Q4

$$\begin{align}
\exp{(-iH\Delta)} & = \exp{(-i\sum_g{h_g g}\Delta)} \\
\end{align}$$

Using equation $(4.103)$ in chapter 4 and split the summation into $4^n$ terms:
$$\exp{(-i\sum_g{h_g g}\Delta)}=\prod_g{}\exp{(-ih_gg\Delta)}+\text{O}(4^n\Delta^2)$$

## Q5

Sicne $H \equiv i\ln{(U)}$,

$$\begin{align}
U=e^{-iH} & =\left(\exp{(-iH\Delta)}\right)^k \\
  & = \left(\exp{(-i\sum_g{h_g g}\Delta)}\right)^k \\
  & = \left(\exp{(-i\sum_g{h_g g}\Delta)}\right)^{k-1}\cdot\prod_g{}\exp{(-ih_gg\Delta)}+\text{O}(4^n\Delta^2) \\
  & = \left[\prod_g{}\exp{(-ih_gg\Delta)}\right]^k+k\cdot\text{O}(4^n\Delta^2) \\
  & = \left[\prod_g{}\exp{(-ih_gg\Delta)}\right]^k+\text{O}(4^n\Delta)
\end{align}$$

## Q6

To be within a distance $\epsilon>0$, $\text{O}(4^n\Delta)<\epsilon$, thus $k=\text{O}(4^n/\epsilon)$. We know that each $\exp{(-ih_gg\Delta)}$ requires $\text{O}{(n)}$ gatesto construct. Hence, $U$ requires $\vert \mathbf{P_n}\equiv\{g\}\vert\cdot k\cdot\text{O}(n)=\text{O}(n16^n/\epsilon)$