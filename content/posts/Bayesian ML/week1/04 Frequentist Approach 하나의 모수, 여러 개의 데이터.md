---
# Common-Defined params
title: "04 Frequentist Approach 하나의 모수, 여러 개의 데이터"
date: "2020-07-20T04:50:00+09:00"
description: "Example article description"
categories:
  - "Statistics"
tags:
  - "Frequentist"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "cover/classical.jpg" # Thumbnail image
lead: "수많은 가능한 표본 중에서 단 하나만 관측할 수 있는 것" # Lead text
comments: true # Enable Disqus comments for specific page
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

<br>이제 대략적인 소개는 했으니 수식을 사용해 좀 더 자세히 설명해볼게요. 일단 데이터 형성 과정에 대한 우리의 가정을 Likelihood로 표현해봅시다.

<br>


$$
\begin{align}
\text{Data (iid):}\quad &D = [x_1, x_2, ..., x_n]\\\\\\
\text{Sampling Density of $D$:}\quad& f(D|\theta) = \prod_{i=1}^nf(x_i|\theta) \quad (x_i\in \mathcal{X}, \theta \in \Omega)\\\\\\
\text{Likelihood of $\theta$:}\quad& L(\theta|D)
\end{align}
$$

## <br>1. 빈도론적 세계관 이해하기

<br>위의 가정은 빈도론과 베이즈 접근법에 상관없이 일반적으로 데이터의 형성과정을 확률 모델로 가정함과 동시에 성립하는 그냥 자명한 사실들입니다. 그러나 빈도론적 세계관에서는 이를 다음과 같이 다시 씁니다.

 <br>




$$
\begin{align}
\text{Ensemble of Data:}\quad & D_{s=1}^{\infty} = [x_1^{(s)}, x_2^{(s)}, ..., x_n^{(s)}]_{s=1}^{\infty}\\\\\\
\text{Sampling Density of $D^{(s)}$ (iid):}\quad& f(D^{(s)}|\theta) = \prod_{i=1}^nf(x_i^{(s)}|\theta) \quad (x_i^{(s)}\in \mathcal{X}, \theta \in \Omega)\\\\\\
\text{Likelihood of $\theta$ given $D^{(s)}$:}\quad& L(\theta|D^{(s)})
\end{align}
$$


<br>

이게 무슨 말일까요?

<br>The fundamental assumption of the frequentist approach is that the parameter is "a" fixed value, and the data is random. This assumption leads them to seek for a single value $\hat{\theta}$, so called estimator, which is a specific function of the data $X$ that best approximate the true value $\theta$ and an error bound of that estimator given the significance level, say 95%. Interestingly, the error bound itself is also a function of the data, so we end up with an interval $\hat{\theta}(X) \pm err(X)$ that is determined by the data $X$. 

<br>Since the frequentists believe the true parameter is a fixed value, they also conduct a hypothesis test to accept or reject a hypothesis, which is a belief that the specific single value $\theta_{null}$ is a true parameter $\theta$. The validation of any hypothesis relies entirely upon the p-value of $\hat{\theta}(X)$ under the the hypothesis in question. p-value quantifies how unlikely the data in hand (more precisely the value $\hat{\theta}(X)$) is under the assumption that $\theta=\theta_{null}$.

<br>The interpretation of the error bound and the p-value is what confounds people outside the circle the most. What the frequentists mean by believing that the data is random is that there exists an infinite array of possible data, $X_1, X_2, X_3, ...$, if only we can observe the outcome of the sampling process infinitely many time, all at the same time. A Sci-fi imagination helps here; envisage infinitely many parallel universes in which the same statistical experiment happens simultaneously. In our universe we observe only a single outcome $X_1$, but this result will vary across the different parallel universes and all those outcomes $\{X_i\}$ comprise a distribution of the data $X$, which is in turn dictated by the true parameter $\theta$. 

<br>Given the imagined distribution of $\{X_i\}$ and the choice of an estimator (i.e. a choice of a function) $\hat{\theta}(X_i)$, we get the distribution of the estimator $\{\hat{\theta}(X_i)\}$, that is, many different values of the estimator $\hat{\theta}$. In each universe we calculate the interval $\hat{\theta}(X_i) \pm err(X_i)$. 95% confidence interval means that in infinitely many universes, about 95 out of 100 of them has the interval containing the true value $\theta$. p-value of, say 3%, means that if the true distribution of estimators $\{\hat{\theta}(X_i)\}$ is indeed decided by $\theta_{null}$, the result in our universe is so rare that only 3 out of 100 universes can observe something rarer than this.

<br>As we can see, the core of the frequentist inference is to figure out the distribution of $\{\hat{\theta}(X_i)\}$ (**sampling distribution**) to make an inference on the distribution of $\{X\}$, which seems quite an oxymoron. Indeed, in a finite sample with $X_i=[x_1, x_2, ..., x_N]$, finding the exact distribution of $\{\hat{\theta}(X_i)\}$ is impossible unless we make some bold assumptions about the distribution of $\{X\}$. However, if $N \to \infty$, the distribution of  $\{\hat{\theta}(X_i)\}$ can be asymptotically determined. Thus the frequentist inference hinge on the asymptotic sampling distribution of $\{\hat{\theta}(X_i)\}$. 



## <br>2. 예시: 중심극한 정리를 이용한 모평균 추정

<br>중심극한정리를 한 번 다시 써보면 다음과 같습니다.


$$
\begin{align}
\text{Let } x \sim p(x). \text{ If $E(x)=\theta, V(x)=\sigma^2$ exist, then}\quad
\dfrac{\bar{x}-\theta}{\sqrt{\sigma^2/n}}\sim N(0,1) \quad as\quad n\to \infty\\
\end{align}
$$
<br>교수님에 따라서는 중심극한정리를 이렇게 쓰시는 분도 있긴 합니다.

$$
\bar{x} \sim^A N(\theta, \sigma^2/n)
$$
<br>즉 표본평균이 어심토틸컬하게 노말하다... 라는 건데, 사실 오해하기 쉬운 표현이긴 합니다. 중심극한정리가 표본평균이 분포수렴한다는 건 아니거든요. 표본평균은 대수의 법칙에 의해 하나의 값 모평균으로 확률수렴해요. 분포가 그냥 모평균 주위로 엄청 뾰족해지고 나머지는 다 density가 0이 되는 degenerate 분포가 됩니다. 정확히 말하면 "표본평균과 모평균의 차이에 $\sqrt{n}$을 곱해 스케일된 편차가 정규분포 $N(0, \sigma^2)$를 따르더라"입니다. 표본 수가 무한정으로 가면 표본평균의 오차는 0이 되겠지만, 거기에 표본 수를 곱한 편차는 정규분포를 따른다는 겁니다.

<br>그래도 이런 점만 주의하면 위의 표현이 좀더 직관적입니다. 중심극한정리가 말하는거는, 원래 분포가 뭐든 간에 상관이 없이, 어떤 조건만 만족하면 (대강 말하면) 표본평균의 근사적 분포를 알 수 있다는 것입니다. 그러니까 모수를 몰라도, 모수에 대한 추정량의 극한분포는 알 수 있으며, 때문에 빈도론적 추론이 가능한 것입니다. 어떻게 하는지 이제 자세히 살펴보겠습니다. 

<br>우리의 관심사가 $\theta$라고 할 때 이 모수의 추정량을 표본평균으로 $(\hat{\theta}(D)=\bar{x})$ 잡을 수 있겠습니다. *(사실 표본평균도 되고 표본 미디안도 되고, 그냥 뭐 아무 값이나 뽑아서 추정량으로 삼아도 됩니다. 어떤 추정량 $\hat{\theta}(D)$을 고를 것이냐의 문제는 frequentist optimality라고도 하는데, 나중에 자세히 볼게요. 결론만 말하면 그냥 MLE가 짱입니다. 표본평균도 MLE입니다.)* 그러나 중심극한정리를 보면 관심 없는데 끼어있는 다른 모수 $\sigma^2$가 있네요. 이런 애들을 nuisance parameter라고 합니다. 그래서 이런 모수들을 제거해주는 방법을 우리가 수통1 5단원에서 배웠죠. 이 nuisance parameter을 이에 확률 수렴하는 표본통계량으로 바꿔도 분포 수렴에 문제가 없다는 정리가 바로 Slutsky's Theorem이었습니다. 
$$
\begin{align}
\text{Sampling Distribtion}&\quad \dfrac{\bar{x}-\theta}{\sqrt{s^2/n}}\sim N(0,1) \quad as\quad n\to \infty\\\\\\
&(s=\dfrac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2)
\end{align}
$$
<br>자 이제 깔끔한 통계량의 극한분포가 나왔습니다. 이제부터 빈도통계학자들의 논리를 그대로 따라가보겠습니다.



### <br>1) 빈도통계학의 점 추정량과 신뢰구간

<br>빈도통계학의 대전제는 모수 $\theta$는 하나의 참값 $\theta^*$으로 고정되어있다는 것이었습니다. 그러니 자연스럽게 우리의 관심사는 데이터에서 하나의 추정치 $\hat{\theta}(D)$ (pointe estimate)을 정하고, 그 추정량이 얼마나 정확한지를 신뢰구간으로 나타내는 것입니다. 모평균 추정에서 점 추정량는 표본 평균 $\hat{\theta}(D) = \bar{x}$입니다. 신뢰구간을 구하는 방법은 다음과 같습니다. 먼저 sampling distribution으로부터 다음의 "확률"구간을 얻습니다.

<br>


$$
\begin{align}
p[-1.96 \leq \dfrac{\bar{x}-\theta}{\sqrt{s^2/n}} \leq 1.96] = 0.95
\end{align}
$$
<br>

위의 구간을 다시 써보면 다음과 같습니다.

<br>


$$
\begin{align}
p[\bar{x}-1.96\sqrt{s^2/n} \leq \theta \leq \bar{x}+1.96\sqrt{s^2/n}] = 0.95
\end{align}
$$
<br>이제 데이터로부터 $\bar{x}$를 계산하고 위의 구간에 대입합니다. 그러면 왼쪽과 오른쪽 구간의 값이 하나의 숫자로 계산이 되겠죠. 그런데 말입니다, 여기에서 확률분포를 가진 확률변수는 $\bar{x}$이며, $\theta$는 그 값이 하나로 고정된 상수입니다. 때문에 **데이터로부터 $\bar{x}$를 계산하고 위의 구간에 대입한 순간 위의 확률구간은 더이상 "확률"구간이 아닙니다.** 가운데 상수가 있고 양 옆에도 상수가 있는 그냥 "구간"이에요. 이 구간을 빈도통계학에서는 95% 신뢰구간이라고 합니다.

<br>


$$
\begin{align}
\text{95% Confidence Interval:}\quad [\bar{x}-1.96\sqrt{s^2/n} , \bar{x}+1.96\sqrt{s^2/n}]
\end{align}
$$
<br>이게 무슨 말일까요? 빈도통계학은 항상 "관측되지 않았지만 무수히 많이 존재할 것인 가상의 데이터의 앙상블"을 전제한다고 했습니다. 신뢰도 $(1-\alpha)\%$인 신뢰구간을 간단히 쓰면 $\hat{\theta}(D_s) \pm err(D_s, \alpha)$으로 나타낼 수 있습니다. 우리가 가진 데이터는 표본 크기 $n$개짜리 $D_1$ 하나이지만, 무수히 많은 데이터들 $\{D_s\}_{s=1}^\infty$들에서 서로 각기 다른 신뢰구간 $\hat{\theta}(D_s) \pm err(D_s, \alpha)$들을 계산할 수 있어요. 여기서 **신뢰도가 95%라는 것은 "대락 100개 중에서 95개 정도의 구간은 그 안에 참 모수를 포함하고 있을 것"**이라는 말입니다. 



### <br>2) 빈도통계학의 가설유의수준검정 (Null Hypothesis Significance Test)

<br>가설 검정은 통입 시간떄부터 정말 주구장창 배웠죠. 뭐라뭐라 말은 많이 하는데 결국 결론은 "p-value가 0.05보다 적으면 기각해야하는 구나"라고 기억할 것입니다. 잘 이해가 안 됐을 거에요. 왜냐면 애초에 논리가 직관적이지 않아서 그래요. 지금부터 한번 빈도론자들의 가설 검정에 대한 논리를 찬찬히 따라가볼게요.

<br>앞서 우리는 모수의 값이 몰라도 표본평균과 모수의 스케일된 편차는 극한적으로 표준정규분포를 따른다고 배웠습니다. **모수 $\theta$에 대한 점 추정과 신뢰구간을 만드는 작업은 극한분포를 바탕으로 "모수 $\theta$가 무엇이냐"**에 대한 물음을 해결하는 것이 목적이었습니다. 이와 달리 **빈도론적 가설검정은 그렇다면 과연 $\theta$가 0이냐, 즉 "모수가 이 값이 맞냐"**에 답을 하는 것이 목적입니다. 빈도론자들은 그 물음에 대한 답을 데이터에서 찾습니다. 

<br>가설이라는 것은 결국 모수 공간의 분할입니다. 귀무가설이 $\mathcal{H}_0$, 대립가설이 $\mathcal{H}_1$이라고 할 때 다음과 같이 쓸 수 있어요.

<br>


$$
\begin{align}
\Omega = \mathcal{H}_0 \cup \mathcal{H}_1 \quad (\mathcal{H_0}\cap\mathcal{H}_1=\emptyset)
\end{align}
$$

<br>예컨대 위에서 모평균 $\theta$를 추정하는 문제에서, 귀무가설과 대립가설을 다음과 같이 세울 수 있겠지요. (단측검정도 있지만 실제로 통계분석에서 관심이 있는 경우는 "두 변수가 독립인지", "이 변수의 회귀계수가 0인지" 등 양측검정인 경우가 많습니다. 단측검정에 대한 논의도 살짝 다르긴 한데 큰 흐름은 똑같습니다.)

<br>


$$
\begin{align}
\mathcal{H}_0: \theta = 0\\
\mathcal{H}_1: \theta \neq 0
\end{align}
$$
<br>이는 전체 모수공간을 하나의 점(귀무가설)과, 하나의 점을 제외한 모든 실수 구간(대립가설)로 나눈 것입니다. 때문에 $\mathcal{H}_0$을 귀무가설 영역으로, $\mathcal{H}_1$을 대립가설 영역이라고 부르겠습니다.

<br>빈도론적 가설검정이란 결국 두 영역 중 하나를 데이터를 바탕으로 선택하는 것입니다. 영역을 선택하는 알고리즘은 요약하면 다음과 같습니다.

<br>**빈도론적 가설 검정 알고리즘**

1. 일단 귀무가설 $\theta=0$ 이 맞다고 해보자.
2. 귀무가설이 맞다고 치면 검정통계량 $Z_{null}$의 (극한) sampling distribution은 $f(Z_{null}\mid\theta=0)$이 될거다.
3. 실제 분포가 $f(Z_{null}\mid\theta=0)$일때 내가 얻은 데이터에서 계산한 $z(D_s)$가 얼마나 "말이 안 되는지"를 보자.
4. 더 말이 안 되는 경우가 나올 확률이 $\alpha\%$보다 낮으면 귀무가설이 틀린거다.

<br>앞서 든 모평균 $\theta$에 대한 검정을 예로 들어봅시다. 먼저 귀무가설 하에서, 그러니까 진짜로 모수가 0일 때에 우리의 추정량 $\hat{\theta}(D_s) = \bar{x}$의 (극한) sampling distribution는 다음과 같을 것입니다.

<br>


$$
\begin{align}
\text{Sampling Distribtion of $\bar{x}$ (given any $\theta$):} &\quad Z=\dfrac{\bar{x}-\theta}{\sqrt{s^2/n}}\sim N(0,1)\\\\\\
\text{Sampling Distribtion if $H_0$ is true $(\theta=0)$:}&\quad Z_{null}=\dfrac{\bar{x}-0}{\sqrt{s^2/n}}\sim N(0,1)
\end{align}
$$


<br>

이처럼 **귀무가설이 맞다고 칠 때의 추정량 $\hat{\theta}(D_s)$의 함수를 검정통계량 $Z_{null}$**이라고 합니다.  여기서 실제 데이터를 넣고 ($\bar{x}, s^2$을 계산해서 넣으면 숫자가 나오겠죠) 검정통계량의 값 $z(D_s)$을 계산하고 나면 우리는 다음과 같이 p-value를 계산할 수 있어요.

<br>
$$
\begin{align}
\text{p-value}: \quad p(|Z_{null}| \geq z(D_s))
\end{align}
$$
<br>p-value에 대한 빈도통계학자들의 설명은, "만일 귀무가설이 맞다고 가정했을때, 내가 가진 데이터보다 더 귀무가설과 반대되는 방향으로 (귀무가설과 동떨어진) 데이터가 관측될 확률"입니다. 즉 **내 귀무가설에서 정말 상상할 수도 없는 일이 일어날 확률**이라고 생각할 수 있어요. 만일 실제로 귀무가설이 맞다면, 우리의 데이터가 그렇게 희박하지 않고 충분히 있을만하겠죠? 즉 p-value가 높을 것입니다. 그러나 실제로 귀무가설이 틀렸다면, 우리의 데이터가 "아 이건 우연이라고 보긴 힘들거같은데" 정도 수준으로 희박할 것입니다. 그 우연이라고 보기 힘들다는 수준을 유의수준 $\alpha\%$으로 결정합니다. 즉 귀무가설 하에서 내가 가진 데이터보다 더 희박한 데이터가 관측될 확률이 10%면 흠 뭐 충분히 그럴만하지 하고 넘어가는데, 5%도 안 되면, 이건 귀무가설이 잘못된거다! 라고 생각하는게 합리적이라는 것입니다. 이떄 유의수준 $\alpha\%$를 1종 오류의 확률이라고 얘기합니다. 

<br>


$$
\begin{align}
\text{Type I Error:}\quad\alpha = \text{$p[$Reject $\mathcal{H}_0$|$\theta^* \in \mathcal{H}_0]$}
\end{align}
$$
<br>$\theta=0$인지 아닌지를 알고 싶을 때, 가설검정 알고리즘을 따라서 결정을 내리면, 원래 $\theta=0$이 맞는데 하필 데이터가 이상하게 나와서 엉뚱하게 $\theta\neq0$이라고 결론내릴 확률은 5%으로 제한할 수 있다는 것입니다. 

*<br>(이거 말고도 검정력이라는 개념도 나오고, 기각역을 어떻게 잡아야 $\alpha$를 유지하면서 검정력 $\beta$가 제일 높냐는 논의도 있습니다. 만일 귀무가설과 대립가설이 각각 하나의 값일 때 Likelihood Ratio의 기각역을 어떻게 잡아야 "Best"하냐에 대한 논의가 수통2에서 Neymann-Pearson Theorem이라고 해서 자세히 나옵니다. 그리고 대립가설 영역이 하나의 점이 아닐 때에도 "uniformly best"한 기각역을 어떻게 잡을 수 있는가라는 논의도 UMP라고 나오는데, 양측검정는 해당이 안 됩니다. 나중에 설명할 이유 때문에 저는 중요하게 생각 안 해서 그냥 넘어갔는데, 수통2에서 자세히 배웁니다.)*

### <br>3) 빈도통계학의 가설유의수준검정: 써도 되는 건가?

<br>자 여기까지만 보면 그럴듯합니다. 정말 그럴듯해서 20세기 초중반 이 알고리즘이 나온 이후부터 2020년이 되는 지금까지 사람들이 계속 쓰고 있습니다. 그러나 완벽한 방법은 아닙니다. 빈도론적 가설검정을 쓰는 많은 연구에서 연구자들이 하고 싶은 것은 귀무가설의 기각입니다. 귀무가설이 대표하는 바는 "효과 없음", "의미 없음" 이런 것들인데, 연구자들이 비싼 돈 들여서 표본을 열심히 모았는데 귀무가설을 기각을 못했다는 것은 헛짓거리 했다는 것이니까요. 그런 연구자들에게 빈도론적 가설검정는 p-value 하나만 보면 "효과 있음"이라는 결론을 내릴 수 있게 해줍니다. 때문에 빈도론적 가설검정을 쓰는 연구자들은 다음과 같은 유혹과 오해에 빠지기 쉽습니다.

1. p-value가 낮을 때까지 데이터를 계속 수집하거나, p-value가 이쁘지 않은 데이터는 무시한다.
2. 하나의 데이터에 대해 p-value가 낮은 결론이 나올 때까지 이런 저런 가설 검정을 계속 한다.
3. p-value가 낮으면 낮을수록 $\theta \neq 0$의 확률이 (즉 약발이 좋을 확률이) 더 높은 것으로 착각한다.

<br>빈도론적 가설검정 자체는 문제가 없지만, 이런 유혹과 오해 때문에 빈도론적 가설검정 자체의 신뢰성에 큰 문제가 생기고 있는 실정입니다. (사실 제가 통계학 공부를 처음 시작한 이유도 이에 대한 의구심 때문이었습니다.) 통계적으로 유의한 결과를 보고한 논문들을 다시 재현해봤더니 보고된 대로 유의한 결과가 다시 나온 논문이 거의 없다고 하네요. 자세한 사항은 Replication Crisis를 한번 검색해보면 알 수 있습니다. 빈도론적 추론의 병폐에 대해서는 이후에 자세히 알아보겠습니다.

<br>*(제 생각이지만 이런 문제가 발생한 이유는 애초에 전제를 모 아니면 도, 즉 무적권 백퍼센트 모수가 $\mathcal{H}_0$이거나 $\mathcal{H}_1$이라고 전제를 했기 때문입니다. 사실 자료를 모으는 사람이 관심이 있는 거는 "신약의 효과가 없을 때 데이터가 얼마나 레어하나"같은 에둘른 말보다는 "그래서 신약 효과가 없을 확률이 얼마나 되냐"가 아닐까 싶습니다. 이를 확률로 쓰면 $p(\mathcal{H}_0\mid D)$로 쓸 수 있겠죠. 그러나 빈도론적 세계관에서는 이 확률을 구할 방법이 없습니다. 애초에 모수가 확률변수가 아니기 때문입니다. 이런 게 불가능하고 오로지 "채택", "기각" 같은 극단적인 선택만 해야하는데, 그 선택의 근거가 p-value이고, 결과에 따라 내 모가지가 왔다리갔다리하면, 당연히 p-value를 손보고 싶은 유혹이 생기지 않을까요? 이런 이유로 빈도론적 가설검정을 "trigger-happy"하다 (그러니까 깊이 고민 안 해보고 p-value 낮으면 무조건 무조건 응 기각~ 해버린다), 그리고 "p-hacking"이다라는 비판이 있습니다.)*

<br>지금까지 본 예시는 검정통계량을 중심극한정리로 얻은 경우였습니다. 모평균에 대한 추정만 예시를 들었는데, 이 외에도 모평균의 차이에 대한 검정에도 적용이 가능하고, 이항분포의 정규근사를 이용하면 모비율 차이 검정이나 분할표의 독립성 검정 문제에서도 중심극한정리로 검정통계량을 얻을 수 있습니다. 하지만 많은 경우 중심극한정리로는 검정통계량을 얻을 수 없는데, 다음에 살펴볼 MLE와 LRT는 데이터 형성 과정을 Likelihood로 세운 모든 경우에 해당하는 빈도론적 추정 방법입니다. 중심극한정리의 일반화라고 볼 수 있겠습니다.





## <br>References

1. Probability Theory and Statistical Inference: Econometric Modeling with Observational Data (Spanos, 1999)
2. Machine Learning: a Probabilistic Perspective (Murphy, 2012) 
3. Computer Age Statistical Inference (Efron, Hastie, 2016)
4. Calibration of p Values for Testing Precise Null Hypotheses (Sellke et al, 2001)
5. https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf

