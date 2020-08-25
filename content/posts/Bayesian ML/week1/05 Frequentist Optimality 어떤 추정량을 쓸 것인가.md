---
# Common-Defined params
title: "05 Frequentist Optimality: 어떤 추정량을 쓸 것인가?"
date: "2020-07-20T05:00:00+09:00"
description: "Example article description"
categories:
  - "Statistics"
tags:
  - "Frequentist"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "cover/classical.jpg" # Thumbnail image
lead: "가장 좋은 추정량 같은 것은 없다." # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: true # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---

<br><br>빈도론자들의 세계관을 다시 한번 복기해봅시다. 데이터의 sampling density를 모수 $\theta$로 결정되는 확률분포함수로 가정하였고, $\theta$를 모를 때 이 sampling density를 데이터에 의해 정해지는 $\theta$의 식인 Likelihood로 해석합니다. 비록 우리가 가진 샘플은 $D^{(s)}$ 하나이지만 내가 모르는 수많은 평행우주에 똑같은 확률실험의 결과들의 앙상블인 $\{D^{(s)}\}_{s=1}^{\infty}$가 있다고 믿어봅시다. 

<br>


$$
\begin{align}
\text{Ensemble of Data:}\quad & D_{s=1}^{\infty} = [x_1^{(s)}, x_2^{(s)}, ..., x_n^{(s)}]_{s=1}^{\infty}\\\\\\
\text{Sampling Density of $D^{(s)}$ (iid):}\quad& f(D^{(s)}|\theta) = \prod_{i=1}^nf(x_i^{(s)}|\theta) \quad (x_i^{(s)}\in \mathcal{X}, \theta \in \Omega)\\\\\\
\text{Likelihood of $\theta$ given $D^{(s)}$:}\quad& L(\theta|D^{(s)})
\end{align}
$$
<br>우리는 데이터를 보고 모수를 추정하고자 합니다. 앞서 배운 빈도론적 방식을 다시 쓰면 다음과 같습니다.

<br>


$$
\begin{align}
\text{True (fixed) Parameter:}& \quad \theta^*\\\\\\
\text{Estimator of $\theta^*$ given $D^{(s)}$:} &\quad \delta(D^{(s)})= \hat{\theta}(D^{(s)})\\\\\\
\text{(Limiting) Sampling Distribution of $\delta(D^{(s)})$:}&\quad  \delta(D^{(s)}) \sim p(.|\theta^*)
\end{align}
$$
<br>앞서 살펴본 예시는 관심 모수가 모평균이고 추정량이 표본평균인 경우였습니다. 중심극한정리 덕분에 모수를 몰라도 추정량의 극한분포, 즉 asymptotic sampling distribution을 알 수 있었고, 덕분에 모수에 대한 점 추정치와 신뢰구간을 제시할 수 있었으며, 모수가 귀무가설의 영역에 있을 때의 추정량의 극한분포 (즉 검정통계량의 극한분포)를 사용해 나름의 가설 검정도 할 수 있었습니다.

<br>그런데 왜 표본평균으로 했는지에 대한 설명은 따로 하지 않았습니다. 즉 $\delta(D^{(s)})$의 선택에 대한 이야기를 하지 않았습니다. 자 여러분이 $\delta(D^{(s)})$를 선택해야하는 빈도통계학자라고 생각해봅시다. 관찰은 못 하는데 분명 우주 어딘가에 존재하는 데이터들이 $\{D^{(s)}\}_{s=1}^{\infty}$이 있습니다. 각각의 데이터에서 추정량 $\delta$의 선택에 따라 하나의 모수에 대한 각기 다른 추정치를 얻겠죠. 그렇다면 나는 $\delta(D^{(s)})$를 결정할 때, 모든 데이터에 대해서 계산한 "오차"를 최대한 줄이는 방향으로 하고 싶습니다. 이때 **각각의 데이터에서 계산한 오차를 "Loss"라고 하며, 모든 데이터에 대해서 이 Loss를 계산하여 평균을 내린 값을 "Risk"라고 합니다. **Loss는 어떻게 정의하기 나름인데, 여기서는 예시로 squared loss를 쓰겠습니다.

<br>


$$
\begin{align}
\text{Loss of $\delta$ in $D^{(s)}$}: \quad & L[\theta^*, \delta(D^{(s)})] = (\theta^* - \delta(D^{(s)}))^2\\\\\\
\text{Risk of $\delta$ over $\{D^{(s)}\}_{s=1}^{\infty}$}:\quad &
R(\theta^*, \delta)\\\\\\
\quad &= \mathbb{E}_{D^{(s)}|\theta^*}\{L[\theta^*, \delta(D^{(s)})]\}\\\\\\
\quad &=\int L[\theta^*, \delta(D^{(s)})] p(D^{(s)}|\theta^*)dD^{(s)}
\end{align}
$$
<br>이 Risk를 가장 최소화하는 $\delta$가 가장 Optimal한 추정량이겠지요. 이걸 어떻게 구할 수 있을까요? 못 구해요. 일단 "무한 개의 평행우주에서의 데이터" 같은 것도 없고, 애초에 참 모수값 $\theta$를 모르니 $p(D^{(s)}\mid\theta)$ 이것도 몰라요. 그래서 빈도통계학에서 "최적의 추정량"같은 것은 없습니다. 그렇다고 아예 아무거나 고를 수는 없지요. 그래서 빈도통계론자들은 추정량이 가지면 참 좋을 것 같은 여러 기준을 제시하는데, 무한 데이터 $\{D^{(s)}\}_{s=1}^{\infty}$에 걸친 추정량 $\delta$의 "행태"에 대한 기준이라고 생각해보면 되겠습니다. (때문에 빈도주의보다는 행태주의(Behaviorism)가 더 어울리는 이름이라는 얘기도 있긴 합니다.) 

<br>일단 데이터의 크기가 엄청 늘어나면 추정량이 모수에 근접해야겠지요. 이를 **Consistency**라고 하는데, 가장 기본적인 성질입니다. 상식적으로 생각했을 때 확률실험을 무수히 반복했는데도 모수에 근접하지 않으면 그 추정량은 아무 소용이 없는 것입니다. 이렇게 기본적으로 일치는 해주는 예의를 갖춘 후에 고려할 사항은 **Unbiasedness**와 **Efficiency**입니다. 즉 모든 평행우주 데이터에 걸친 $\delta$의 분포가 가급적이면 모수를 중심으로 하면 좋겠고, 분포의 폭도 좁으면 좋겠다는 것입니다. 

<br>Bias도 없으면서 모든 추정량 중에서 분산도 가장 작으면 좋은 추정량이라고 할 수도 있습니다. 하지만 어느정도 편차가 생겨도 Unbiased한 추정량보다 더 좋을 수도 있습니다. 앞서 우리는 좋은 추정량을 Risk가 작은 추정량으로 이야기했습니다. 이때 squared loss를 가정하면 Risk를 다시 써보면 다음과 같이 쓸 수 있습니다. (squared loss으로 정의된 risk를 Mean Squared Error라고 합니다.) 아마 수통1 시간때 배우셨을 겁니다.

<br>


$$
\begin{align}
\mathbb{E}_{D^{(s)}|\theta^*}[\theta^*- \delta(D^{(s)})]^2 &= 
\mathbb{E}_{D^{(s)}|\theta^*}(\delta - \mathbb{E}_{D^{(s)}|\theta^*}[\delta])^2 + (\mathbb{E}_{D^{(s)}|\theta^*}[\delta]-\theta^*)^2\\\\\\
&= \mathbb{V}_{D^{(s)}|\theta^*}(\delta) + Bias^2(\delta)\\\\\\
\therefore MSE &= Variance + Bias^2
\end{align}
$$
<br>위 식을 보면 추정량을 정할 때 만일 Biased하더라고, 그 편차가 크지 않으면서 줄어든다면 오히려 Unbiased Estimator보다 MSE가 더 적을 수 있음을 알 수 있으며, 나중에 그런 예를 한번 살펴보겠습니다.

<br> *(지금까지 말한 논리는 Supervised Learning에도 그대로 적용됩니다. 어떤 연속형 확률변수 $t$의 값을 예측하는 설명변수들의 함수 $\hat{f}(\mathbf{x})$, 즉 예측 모델을 만드는 문제를 생각해봅시다. 이때 실제 함수는 $f$이겠지만 이를 알 수 없으니 회귀분석 등 갖가지 방법을 이용해 $\hat{f}$를 추정할 수 있습니다. 이 때 $f$와 $\hat{f}$의 관계도 위의 논의와 똑같습니다.)*





## <br>References

1. Probability Theory and Statistical Inference: Econometric Modeling with Observational Data (Spanos, 1999)
2. Machine Learning: a Probabilistic Perspective (Murphy, 2012) 
3. Computer Age Statistical Inference (Efron, Hastie, 2016)
4. Calibration of p Values for Testing Precise Null Hypotheses (Sellke et al, 2001)
5. https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf

