---
title: 楕円フーリエ解析
layout: post
author: Kaede Konrai
---

## 楕円フーリエ解析とは

輪郭 contour の解析のために用いられる解析方法です。
形態計測学 Morphometricsで用いられます。

- 輪郭がそれぞれのサンプルでどのくらい似ているのか
- サンプル同士の輪郭形状の違いがどのようなものか

といったことを定量化および可視化することができます。
研究例としては、以下のような研究が挙げられます。
本ページでは、理論からRによる実装、解析を含めた説明を試みます。

## チェインコード

文献によって方向や開始点などが若干異なりますが、方向に数字を結びつけたものをチェインと呼び、その数字を並べたベクトルをチェインコードと呼びます。

$$
V = a_1 a_2 a_3 \cdots a_K
$$

ここで、$$V$$がチェインコードで$$ a_1 a_2 a_3 \cdots a_K $$がそれぞれのチェインになります。
おそらく最初に提唱されたのはFreeman(1974)だそうです。
ここでは以下の図のように、右方向から時計回りに8方向に伸びる矢印にそれぞれ0~8までの数字を当てはめます。



## 楕円フーリエ係数の算出



## 説明力

## 標準化

$$
\left[
\begin{array}{rr}
a_n^{**} & b_n^{**} \\
c_n^{**} & d_n^{**} \\
\end{array}
\right]
=
\left[
\begin{array}{rr}
\cos \psi & \sin \psi \\
-\sin \psi & \cos \psi \\
\end{array}
\right]
\left[
\begin{array}{rr}
a_n & b_n \\
c_n & d_n \\
\end{array}
\right]
\left[
\begin{array}{rr}
\cos \frac{2 \pi n t}{T} &  -\sin \frac{2 \pi n t}{T} \\
\sin \frac{2 \pi n t}{T} &\cos \frac{2 \pi n t}{T} \\
\end{array}
\right]
$$

## 主成分分析

## 逆フーリエ変換