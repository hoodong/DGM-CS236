# DGM-CS236
- 가짜연구소 9기 프로젝트
- 생성모델 온라인 강의 (CS236) 공부
- 강의 슬라이드에 대한 요약과 질문을 간략히 정리 

## Things to learn (lecture no)
- Introduction & background (1,2)  
- Autoregressive models (3)    
- Maximum likelihood learning (4)  
- VAEs (5,6)  
- Normalizing flows (7,9)  
- GANs (8,10)     
- Energy based models (11,12,14)  
- Score based models (13)
- Evaluation of generative models (15)
- Score-based diffusion models (16) 
- Discrete-latent variable models (17) 
- Diffusion models for discrete data (18) 

## Lecture 1. Introduction 
- 생성모형에서 핵심질문
  - 표현: 많은 확률변수들의 결합분포를 어떻게 모델링할 것인가?
  - 학습: 확률분포를 비교하는 올바른 방법은 무엇인가?
  - 추론: 생성과정에서 어떻게 추론을 수행할 것인가?

## Lecture 2. Background
- 생성모형은 확률분포를 학습한다. 이 확률분포를 사용해 sampling, anomaly detection, feature extraction을 할 수 있다.
- 결합확률분포를 정확히 표현하려면 많은 파라미터가 필요하다. 파라미터 수를 줄이기 위해 모형을 사용해 근사한다.
- 연쇄율을 이용하면 결합확률분포를 조건부 확률들의 곱으로 표현할 수 있다. 이것을 Bayes net이라고 하고 유향 비순환 그래프로 기술된다.
- Bayes net에서 조건부 독립을 이용해 파라미터 수를 줄인다. 예를 들어 naive Bayes는 모든 feature가 독립이라고 가정한다.  
- 판별모형에서는 $P(Y|X)$, 생성모형에서는 $P(X,Y)=P(Y|X)P(X)$에 관심을 가진다.

## Lecture 3. Autoregressive models

## Lecture 4. Maximum likelihood learning
- 학습된 확률분포와 데이터 확률분포가 최대한 가까워지도록 학습해야 한다. 
- 두 분포의 거리를 측정하는데 KL divergence를 사용한다. KL divergence $D(p\parallel q)$는 $p$ 대신 $q$를 써서 데이터를 압축할 때 추가되는 비트수를 의미한다.
- 생성모형에서 KL divergence를 최대화하는 것은 log-likelihood의 기대값을 최대화하는 것과 같다.
- 데이터 분포를 모르니까 기대값 대신 표본평균을 사용한다. (expected log-likelihood $\approx$ empirical log-likelihood)
- 따라서 ML learning이 되고, 늘 그래왔듯이 stochastic gradient descent로 학습시킬 수 있다.

## Lecture 5. Latent variavle models
- 데이터에서 관측되지 않는 않는 숨겨진 변수가 있다. 이 잠재변수는 저차원 공간에서 데이터의 중요한 특징을 압축적으로 표현한다.
- 잠재변수 $z$를 도입하면 확률분포는 $p(x,z)=p(z)p(x|z)$로 표현되고 $p(z)$와 $p(x|z)$을 간단한 분포로 모델링할 수 있다.
- $x$만 관측되고 $z$는 관측되지 않은 변수이므로 잠재변수 모형을 학습려면 marginal likelihood $p(x)$를 최대화해야 한다.
- 베이지안 추론에서 marginal likelihood $p(x)$를 evidence라고 부른다. 여기서 marginal은 모든 잠재변수에 대해 적분함을 의미한다.
- 하지만 $p(x)$를 구하려면 $p(x,z)$를 잠재변수 $z$에 대해 적분해야 하는데 이 계산이 잘 풀리지 않는다.(intractable)
- ELBO는 evidence lower bound를 의미한다. evidence $p(x)$를 근사하는데 사용된다.
- 일반적으로 log-evidence = ELBO + KL divergence로 주어진다. 따라서 ELBO는 evidence의 하한이다.
  
