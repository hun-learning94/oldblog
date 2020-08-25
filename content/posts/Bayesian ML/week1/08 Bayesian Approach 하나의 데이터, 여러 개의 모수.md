---
# Common-Defined params
title: "Bayesian Approach: 하나의 데이터, 임의의 모수"
date: "2020-07-20T05:30:00+09:00"
description: "Example article description"
categories:
  - "Probabilistic Machine Learning"
tags:
  - "Bayes Rule"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "cover/robotbayesian.jpg" # Thumbnail image
# lead: "" # Lead text
comments: true # Enable Disqus comments for specific page
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

## <br>0. 생각하는 로봇은 베이지안이다!

주변 환경을 인지하고 목적지를 찾는 로봇을 생각해보자. 목적지로 가는 경로에는 수많은 경우의 수가 있다. 이 경로에서 로봇은 시시각각 환경을 파악해서, 즉 데이터를 수집해서 가장 안전한 길을 택해야 한다. 전방에 위험징후를 포착했다. 로봇은 그 방향으로 가는 길이 위험하다고 판단해 경로를 변경해야 한다. 자 그러면 이걸 어떻게 코딩할까? 각각의 길이 위험할 확률 $p(road_i=unsafe)$과, 각각의 길에서 위험한 징후가 포착될 확률 $p(sign\mid road_i=unsafe)$ 을 고려하여, 위험할 확률 $p(road_i = unsafe \mid sign)$ 을 다시 계산해야한다. 사실 우리는 머릿 속으로 끊임없이 베이즈 정리를 쓰고 있지만 우리가 모르거나, 계산이 이상하거나, 착각을 하고 있을 뿐이다. 숫자로 생각하는 로봇은 철저한 베이지안이다!

## <br>1. Bayes Rule: "Inverse" Probability

<br>먼저 베이즈 정리에 대해 간략히 소개하고, 이게 왜 혁신적인 발상의 전환인지 느껴보자.

베이즈 정리 자체는 product rule과 조건부 분포의 정의를 알면 바로 나온다.

<br>

$$
\begin{align}
p(C|E) = \dfrac{p(C, E)}{p(E)} = \dfrac{p(C)p(E|C)}{p(E)} = \dfrac{p(C)p(E|C)}{\sum_{C'}p(C')p(E|C')}
\end{align}
$$
<br>여기서 분모의 $p(E)$ 개별 $C$에 의존하지 않는 상수이다. 때문에 다음과 같이 쓰기도 한다.

<br>


$$
\begin{align}
p(C|E) \propto p(C)p(E|C)
\end{align}
$$
<br>어떤 사건 E가 발생했을 때 그 원인이 C일 확률은, 애초에 C가 발생활 확률과, 그 C가 발생했을 때 E의 확률의 곱에 비례한다는 것. 만일 $p(C)$만 알고 있으면 **"사건 발생 후 원인의 확률을 묻는 문제"가 "원인이 주어졌을 때 사건의 확률를 묻는 문제"로 바뀐다는 것.** 인과관계가 역전된 것이 보이나? 이 때문에 이를 Inverse Probability라고도 한다. 

<br>이걸 가설 $\mathcal{H}$에 한번 적용해볼까? 모수공간의 분할 $\mathcal{H}_1$, $\mathcal{H}_2$, $\mathcal{H}_3$, ... 을 생각해보자. 그러면 $D$가 주어지면 다음과 같이 쓸 수 있다.

<br>


$$
\begin{align}
p(\mathcal{H}|D) = \dfrac{p(D|\mathcal{H})p(\mathcal{H})}{p(D)}
\end{align}
$$

* $p(\mathcal{H})$: 데이터를 보기 전에 모든 가설이 가지는 확률
* $p(D\mid\mathcal{H})$: 각각의 가설에서 데이터가 얼마나 Likely한지. 즉 데이터가 제공하는 가설의 Evidence
* $p(\mathcal{H}\mid D)$: 데이터를 바탕으로 업데이트된, 모든 가설 각각에 대한 확률
* $p(D)$: 모든 가능한 가설에서 주어진 데이터가 발생할 확률

<br>여기에서 논란이 되는게 $p(\mathcal{H})$, 즉 prior이다. prior가 존재하지 않으니 인위적으로 prior를 만들면 어떻게든 나의 주관이 들어가게 되는거고, 그 순간 확률보다는 분포로 나타낸 가설에 대한 믿음이라는 해석이 더 적절하다. 그럼에도 불구하고 prior를 만들어서 계속 베이즈 정리를 쓰겠다는 것이 베이지안이고, 데이터를 보기 이전에 인위적으로 prior를 만든다는 발상 자체에 거부감을 느껴, "어떻게 하면 prior가 없이 통계 분석을 할 수 있을까"라는 물음에서 시작되어 Likelihood만 쓰는 방법이 빈도주의이다. (Reference 5)



## <br>2. 베이즈 세계관 이해하기

<br>이제 아까 빈도론적 추론에서 본 것과 똑같은 세팅을 가져오자.

<br>


$$
\begin{align}
\text{Data (iid):}\quad &D = [x_1, x_2, ..., x_n]\\\\\\
\text{Sampling Density of $D$:}\quad& f(D|\theta) = \prod_{i=1}^nf(x_i|\theta) \quad (x_i\in \mathcal{X}, \theta \in \Omega)\\\\\\
\text{Likelihood of $\theta$:}\quad& L(\theta|D)
\end{align}
$$
<br>베이즈 추론에서 모수 $\theta$는 "unknown thus uncertain". 즉 모르니까 확률변수다.  $p(\theta)$는 데이터 $D$를 보기 전(prior) 나의 믿음이고, $p(\theta|D)$는 데이터를 보고 난 후(posterior)의 나의 믿음이다. 그렇다면 $p(\theta|D)$ 를 어떻게 얻는가? 베이즈 정리를 사용한다. **베이지안은 모든 통계분석을 베이즈정리로 한다. 베이즈정리가 알파이자 오메가이다!**

<br>


$$
\begin{align}
\text{(Prior) Belief in $\theta$} \quad & p(\theta) \\\\\\
\text{Likelihood of $D$ at each $\theta$} \quad & L(\theta|D)=p(D|\theta) \\\\\\
\text{Updated (Posterior) Belief in $\theta$} \quad & p(\theta | x ) = \dfrac{p(D|\theta)p(\theta)}{\int_{\theta} p(D|\theta)p(\theta) d\theta}\quad\\\\\\
\end{align}
$$
<br>빈도론 추론과 가장 큰 차이점은 $\theta$에 대한 추론의 결과를 분포로 제시한다는 것이다. 

<br>새로운 데이터 $\tilde{x}$에 대한 빈도론적 예측 분포는 다음과 같다.
$$
\tilde{x} \sim p(x_i|\hat{\theta})
$$
<br>새로운 데이터 $\tilde{x}$에 대한 베이지안 예측 분포는 다음과 같다.
$$
\begin{align}
\text{Prior Predictive:}\quad& p(\tilde{x}) = \int p(x_i|\theta)p(\theta)d\theta\\\\\\
\text{Posterior Predictive:}\quad& p(\tilde{x}) = \int p(x_i|\theta, D)p(\theta|D)d\theta\\
\end{align}
$$

<br>

Posterior Predictive를 잘 살펴보면, 새로운 데이터의 확률은, 모든 가능한 경우 $\theta$에 걸쳐 그 데이터의 확률(likelihood) $p(x \mid \theta, D))$을 모두 고려하되, 그 각각의 가능성에 대해 사후믿음 $p(\theta \mid D)$만큼의 가중치를 주어서 평균한 값이라는 말이다. 빈도론에서의 예측 분포는 오직  $p(x \mid \theta_{mle})$이다. <b>빈도론이 단 하나의 경우만을 고려한다면, 베이지안에서는 나의 믿음에 따라 모든 가능한 경우의 수를 모두 고려한다는 점이 가장 큰 차이점이다.</b>

## <br>3. 베이지안 논리의 직관적인 이해

이 부분은 이전에 만들어놓은 강의자료로 대체합니다!

![CH01-17](/image/CH01-17.png)

![CH01-18](/image/CH01-18.png)

![CH01-19](/image/CH01-19.png)

![CH01-20](/image/CH01-20.png)

![CH01-21](/image/CH01-21.png)

![CH01-22](/image/CH01-22.png)

![CH01-23](/image/CH01-23.png)

![CH01-24](/image/CH01-24.png)

![CH01-25](/image/CH01-25.png)











### 예시: 지금 신촌에 비가 올까?

**<br>(미국 Facebook 면접문제 변용) 오늘 아침 일기 예보를 보니 신촌에 비가 올 확률이 $25\%$이라고 합니다. 아침을 먹고 현관문을 나서기 전, 당신은 자취하는 친구 세 명에게 지금 비가 오는지 따로따로 물어봅니다. 모두 지금 비가 온다고 합니다. 하지만 당신의 친구들은 당신을 곯리기 위해 세 번 중 한 번은 거짓말을 합니다. 그렇다면 당신은 우산을 챙길 것인가요? 그 이유와 함께 설명해주세요.**

<br>인터넷에 떠도는 페이스북 면접 문제를 가져와서 변용하였다. 이 문제에서 친구들의 답변 $YYY$를 Data, 비가 내리는 여부를 모수 $\theta$로 생각하면, 다음과 같은 두 풀이가 가능하다.

<br>

1. **MLE 접근**. 비가 내려는 분포에서 YYY의 확률은 $p(YYY\mid rain) = (2/3)^3 = 8/27$, 비가 내리지 않는 분포에서 YYY의 확률은 $p(YYY\mid rain) = (1/3)^3 = 1/27$. 비가 내릴 때의 표본의 Likelihood가 더 크니 우산을 챙긴다.
   <br>

2. **Bayes Rule 접근**. 비가 내리는 사건의 사전 확률(믿음)은 $0.25$이다. 데이터를 바탕으로 이 믿음을 업데이트하면
   <br>
   $$
   \begin{align}
                  p(Rain|YYY) &= \dfrac{p(YYY|Rain)p(Rain)}{p(YYY|Rain)p(Rain) + p(YYY|NoRain)p(NoRain)}\\\\\\
                  &=\dfrac{8/27 \times 1/4}{8/27 \times 1/4 + 1/27 \times 3/4}\\\\\\
                  &= \dfrac{8 \times 1}{8 \times 1 + 1 \times 3} = 8/11 > 0.25
                  \end{align}
   $$
   <br>즉 친구들의 대답으로 인해 나의 믿음이 $0.25$에서 $0.72$로 올라갔으니 우산을 챙긴다는 것.

<br>결론은 똑같지만 사고 과정이 다르다.







## <br>References

1. Probability Theory and Statistical Inference: Econometric Modeling with Observational Data (Spanos, 1999)
2. Machine Learning: a Probabilistic Perspective (Murphy, 2012) 
3. Computer Age Statistical Inference (Efron, Hastie, 2016)
4. Calibration of p Values for Testing Precise Null Hypotheses (Sellke et al, 2001)
5. https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf

