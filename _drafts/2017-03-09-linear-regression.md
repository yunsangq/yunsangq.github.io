---
layout: post
title: "Linear Regression"
date:   2017-03-09
excerpt: "수업시간에 배운 Linear Regression에 대해 정리한다."
categories: [Machine Learning]
tags: [Ubuntu, Caffe, OpenCV]
comments: true
---

Linear Regression 수업 내용에 대해 간략히 정리한다.

# Regression

Supervised Learning은 Dataset에 mapping된 input: X(vector)와 output: y를 이용해 학습을 한다.
$$ D = \{(X_i, y_i)\}_i^N $$

이때 y가 Categorical하면 classification, Real-valued라면 regression이라 할 수 있다. 
이를 function approximation 해서 보게되면 다음과 같이 생각할 수 있다.

$$ y = f(X) $$ 일때 estiamte 하면
$$ \hat y = \hat f(X) $$
이다.

이를 전제로 Regression의 종류에 대해 살펴보자.

# Linear Regression

Linear Regression은 D를 input: X의 dimensionality라 할때 다음과 같이 가정할 수 있다.

$$ \begin{align} 
	y(X) & = w^TX + \epsilon \\
	& = \sum_{j=1}^D w_jx_j + \epsilon
	\end{align}
$$
