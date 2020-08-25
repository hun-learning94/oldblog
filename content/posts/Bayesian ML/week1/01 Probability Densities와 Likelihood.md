---
# Common-Defined params
title: "01 Probability Densities와 Likelihood"
date: "2020-07-20T03:43:58+09:00"
description: "Example article description"
categories:
  - "Statistics"
tags:
  - "Frequentist"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "cover/classical.jpg" # Thumbnail image
lead: "분포는 모른다. 우리에게는 오직 Likelihood만 있을 뿐이다." # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---

<br>어떤 확률 변수 $x_i$가 가질 수 있는 값들을 sample space $\mathcal{X}$라고 하고, 그 값들의 분포는 어떤 모수 $\theta$에 의해 완전히 결정되는 함수 $f(x\mid\theta)$라고 생각해봅시다(예컨대 이항분포나 분산이 주어진 정규분포 등을 생각해볼 수 있겠습니다). 모수 $\theta$가 가질 수 있는 값들은 parameter space $\mathcal{\Omega}$라고 합니다. (이때 모수 $\theta$는 스칼라가 아니라 벡터일 수도 있습니다.  여기서는 스칼라인 경우만 일단 생각해볼게요.) 



$$
\text{Sampling Density of $x_i$:}\quad f(x|\theta) \quad (x\in \mathcal{X}, \theta \in \Omega)
$$



<br>이때 함수 $f$를 probability density라고 합니다. ($x$가 이산형일때는 probability mass function이라고도 하는데, 여기서는 편의상 그냥 density라고 퉁치겠습니다.) 이와 똑같은 분포를 따르며 독립적으로 샘플링된(i.i.d라고 하지요) 샘플들이 $x_1, x_2, x_3, ...$ 이렇게 있으면 우리는 데이터의 벡터 $\mathbf{x}$를 생각해볼 수 있겠습니다. 



$$
\text{Joint Sampling Density of }\mathbf{x}=[x_1, x_2, ..., x_N]\quad p(\mathbf{x}|\theta)=\prod_{n=1}^Np(x_n|\theta)\\
$$

<br>앞서 말한 것처럼 density는 모수 $\theta$에 의해 전적으로 결정되는 함수입니다.  그런데 생각해보면, 우리는 모수 $\theta$를 모른 상태에서 오직 하나의 데이터셋 $\mathbf{x}$만 알고 있습니다. 이런 상황에서 density는 그 자체로는 아무 의미가 없어요. 모수의 값에 따라 수없이 많은 density가 가능하니까요. 즉 이처럼 데이터는 알고 모수를 모르는 상태에서는, 위 함수를 **모수에 대한 함수**로 바꿔쓸 수 있습니다. (사실 바꿔쓴다는 것도 아니고 그냥 그대로 똑같은 함수인데, 어떻게 해석하냐의 차이입니다.) 이를 데이터에 대한 모수 $\theta$의 Likelihood라고 합니다. 


$$
\text{Likelihood of}\;\theta \quad L(\theta|\mathbf{x})
$$


<br>해석을 해보자면, Likelihood가 말하는 바는 어떤 모수의 값 $\theta$가 우리에게 주어진 데이터 $\mathbf{x}$를 고려해보면 얼마나 "Likely"한지를 나타내는 것입니다. **이때 정말정말 헷갈리기 쉽지만 명확히 이해해야 할 것은 Likelihood는 $\theta$의 확률이 아니라는 것입니다!!** 함수 $f(x\mid\theta)$는 함수의 형태가 $\theta$에 의해 결정되는 확률밀도함수입니다. 즉 어느 $\theta$에 대해서도 $x$에 대해 적분을 하면 1이 되도록 만들어진 함수라는 것이죠. 그러나 Likelihood는 이렇게 인위적으로 고안된 함수를 $\theta$에 대해서 바라본 함수입니다. 때문에 주어진 데이터에 대해서 $\theta$에 대해서 적분을 한다해도 1이 되리라는 보장이 없습니다. (이 부분이 통계학에 대해서 가장 많이 오해를 하기 쉬운 부분이고, 대중적으로 빈도론적 통계추론, 특히 p-value가 크게 오해를 받고 있는 부분입니다. 나중에 자세히 설명하겠습니다.)<br>

<br>예를 들어 이항분포의 pdf를 생각해봅시다. 동전을 3번 던져서 앞면이 나온 횟수는 $B(3, p$)를 따릅니다. 실제로 3번 던져서 1번 앞면이 나왔다고 해봅시다. 이 사건의 확률은 다음과 같이 쓸 수 있습니다.


$$
\text{Probability Density of $x=1$ outcome:}\quad f(x=1|p) = {3 \choose 1}p^1(1-p)^2
$$


<br>그런데 모수 $p$를 모르면 위 함수는 뭐 써 먹을 수가 없죠. 때문에 위의 함수를 똑같이 쓰되 모수 $p$에 대한 함수로 생각해볼 수 있습니다.


$$
\text{Likelihood of $p$:}\quad L(p|x=1) = {3 \choose 1}p^1(1-p)^2
$$


<br>똑같은 식이에요. 근데 해석만 다를 뿐이죠. $p=1/2, 1/3, 1/4,...$인 경우에 대해서 Likelihood를 계산해보세요. 그 합이 모두 1이 될까요? 아닙니다! 핵심만 정리해보면,

* Likelihood는 Probability Density와 그냥 똑같은 함수인데, 해석만 다르다.
* Probability Density는 모수 $\theta$에 의해 결정되는 "확률"밀도 함수이다. Likehood는 데이터 $\mathbf{x}$에 의해 식이 결정되는 "그냥" 함수이다. 
* Likelihood는 $\theta$의 확률이 아니다! 즉 $L(\theta\mid\mathbf{x}) \neq p(\theta\mid\mathbf{x})$ 



## <br>References

1. Probability Theory and Statistical Inference: Econometric Modeling with Observational Data (Spanos, 1999)
2. Machine Learning: a Probabilistic Perspective (Murphy, 2012) 
3. Computer Age Statistical Inference (Efron, Hastie, 2016)
4. Calibration of p Values for Testing Precise Null Hypotheses (Sellke et al, 2001)
5. https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf

