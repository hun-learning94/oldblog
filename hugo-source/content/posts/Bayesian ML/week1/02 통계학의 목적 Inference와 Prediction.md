---
# Common-Defined params
title: "02 통계학의 목적: Inference와 Prediction"
date: "2020-07-20T04:00:00+09:00"
description: "Example article description"
categories:
  - "Statistics"
tags:
  - "Frequentist"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "cover/classical.jpg" # Thumbnail image
# lead: "(KOR) 확률밀도함수와 우도는 다르다" # Lead text
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

<br>이처럼 데이터는 알고 모수는 모르는 상황에서, 우리는 오직 하나의 Likelihood 함수만 관측할 수 있습니다. 이 Likelihood 함수를 가지고 통계학자들이 하고 싶은 일은 두 가지로 요약할 수 있습니다.

<br>

1. **Inference:** 데이터 $\mathbf{x}$를 바탕으로 모수 $\theta$에 대해 무엇을 말할 수 있는가?
2. **Prediction:** 데이터 $\mathbf{x}$를 바탕으로 새로운 데이터 $x_{new}$를 예측해보자.

<br>동전의 예를 생각해보면, inference는 이 동전이 과연 fair한가 아닌가, 즉 앞면이 나올 확률이 무엇인가에 대해 답하고자 하는 것이며, prediction은 그렇다면 다음 시행에서 앞면이 나올지 뒷면이 나올지 예측하는 것입니다. 

<br>여기서부터는 사견인데, inference는 전통적으로 통계학의 분야였고, 앞으로도 통계학만의 분야로 계속 남을 거라고 생각합니다. inference는 결국 어떤 모수에 대한 통계적인 추론이기 때문에, 애초에 시작부터 데이터의 형성 과정에 대한 모수를 동반한 확률적인 가정이 들어가야 하니까요. 데이터의 형성 과정에 대한 여러 가정을 반영해 Likelihood 모델을 세우고, 그 가정 하에서 이런 저런 이야기를 하는게 inference입니다. 그러나 Prediction만을 한다면 굳이 데이터 형성 과정에 확률적인 가정이 들어갈 필요가 없어요. inference에서 우리가 가정을 세운다는 것은 다시말해 데이터 형성 과정을 일부로 인간이 알아보기 좋게 단순화한다는 것입니다. 그래야 어떤 모수의 값이 변하면 결과가 어떻게 변하는지 파악하기 쉬우니까요. 하지만 단순히 예측만 잘 하면 된다면 이런 가정이 필요없고, 예측 오차만을 줄일 수 있다면 수많은 다양한 시도를 해볼 수 있습니다. 그 과정에서 우리 인간이 더이상 예측 모델을 해석할 수는 없겠지요. 

<br>회귀분석을 예로 들어보면, 회귀분석은 종속변수와 설명변수들 간의 관계를 일부로 선형으로 단순화해 해석을 용이하게 만든 확률 모형입니다. 오차항의 정규 가정을 통해 종속변수의 조건부 분포를 평균이 설명변수의 선형결합으로 단순화된 정규분포로 나타낸 거에요. 그게 제일 만만하니까. 그래서 베타가 0이냐 아니냐 얘기도 하고 베타 계수가 대충 이 정도 구간 안에 있지 않을까 얘기도 하고. 그러나 실제로 데이터의 조건부분포가 완벽히 선형인 경우는 극히 드믈죠. 그런데도 쓰는 이유는 "마케팅 예산을 이정도 늘리면 매출이 얼마나 오를까"와 같은 질문에 대충 답이라도 해볼 수 있기 때문입니다. 그런데 그게 아니라 최대한 정확히 예측을 하는게 목적이면 회귀분석 말고 SVM, Boosting, Neural Network 등 다양한 알고리즘을 쓸 수 있겠지요. 그러나 "아니 우버가 왜 트럭에다가 들이박았냐" 같은 질문을 해결을 할 수 없어요. 

## <br>References

1. Probability Theory and Statistical Inference: Econometric Modeling with Observational Data (Spanos, 1999)
2. Machine Learning: a Probabilistic Perspective (Murphy, 2012) 
3. Computer Age Statistical Inference (Efron, Hastie, 2016)
4. Calibration of p Values for Testing Precise Null Hypotheses (Sellke et al, 2001)
5. https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf

