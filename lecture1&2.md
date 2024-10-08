# Lecture 1. Introduction 
- 생성모형에서 핵심질문
  - 표현: 많은 확률변수들의 결합분포를 어떻게 모델링할 것인가?
  - 학습: 확률분포를 비교하는 올바른 방법은 무엇인가?
  - 추론: 생성과정으로부터 어떻게 추론을 수행할 것인가?

# Lecture 2. Background
- 훈련 데이터가 개 이미지이면, 생성모형에서 학습된 확률분포는
  - 새로운 개 이미지를 생성할 수 있어야 한다. (sampling)
  - 개이면 확률이 높고 아니면 확률이 낮아야 한다. (anomaly detection) 
  - 개 이미지의 특징을 학습해야 한다. (feature)
- Q.생성모형의 손실함수 $d(P_{data},P_{\theta})$에서 $P_{data}$는 모집단의 확률분포 or 데이터셋의 분포?
- 결합확률분포 예제
  - RGB 픽셀은 R,G,B 각 채널이 256개 값을 가질 수 있으므로, 결합확률분포 $p(R,G,B)$는 $256^3-1$개의 파라미터가 필요하다. 
  - n개의 픽셀의 흑백 이미지는 베르누이 결합확률분포 $P(X_1,...X_n)$로 나타낼 수 있고 $2^n-1$개의 파라미터가 필요하다.
- 확률분포가 독립이라고 가정하면
  - $X_1,...X_n$이 서로 독립이면, $P(X_1,...X_n)=P(X_1)P(X_2)...P(X_n)$
  - $2^n$개의 상태를 표현하는데 $n$개의 파라미터가 필요하다.
  - 가정이 너무 강해서 현실과 맞지 않다.(픽셀간의 상관관계를 고려하지 않는다)
- 두가지 중요한 확률 법칙
  - 연쇄 법칙: $P(X_1,X_2,...,X_n)=p(X_1)P(X_2|X_1)...P(X_n|X_1,X_2,...,X_{n-1})$
  - 베이즈 법칙: $p(X1|X2)=\frac{P(X1,X2)}{P(X2)}=\frac{P(X2|X1)P(X1)}{P(X2)}$
- 조건부 독립
  - 연쇄율은 파라미터수를 줄여주지 않는다.
  - 왜냐하면 $P(X_1,...,X_n)=P(X_1)P(X_2|X_1)P(X_3|X_1,X_2)...P(X_n|X_1,X_2,...,X_{n-1})$ 이므로 $1+2+...+2^{n-1}=2^n-1$개의 파라미터가 필요하다.
  - 만약 $X_{i+1}$과 $X_1,...,X_{i-1}|X_i$이 독립이면 $P(X_1,...,X_n)=P(X_1)P(X_2|X_1)P(X_3|X_2)...P(X_n|X_{n-1})$ 이므로 파라미터 수가 $1+2(n-1)=2n-1$가 된다.
- Bayes 네트워크  
  - 결합확률분포를 조건부확률로 표현 (연쇄율에서 조건부 독립을 가정)
    - $P(X_1,...,X_n)=\prod{P(X_i|X_{A_i})}$
    - eg. difficulty, intelligence, grade, SAT, letter
      $p(d,i,g,s,l)=p(d)p(i)p(g|i,d)p(s|i)p(l|g)$ 는 $D\perp I, S\perp {D,G}|I, L\perp {I,D,S}|G$를 의미
  - 유향 비순환 그래프 (directed acyclic graph; DAG)로 기술할 수 있다.
    - $G(V,E)$에서 노드 $V$는 확률변수를, 에지 $E$는 "조건부 종속"을 나타낸다.
    - 예를 들어 Sprinkler, Rain, Grass wet는 P(G,S,R) = P(G|S,R)P(S|R)P(R) 
  - 독립 (independent) vs. 조건부 독립 (conditional independent)
    - 독립: $A \perp B$ $\leftrightarrow$ $P(A|B) = P(A)$
    - 조건부 독립: $A \perp B|C$ $\leftrightarrow$ $P(A|B,C) = P(A|C)$
    - 독립이 아니면 종속
- naive Bayes
  - 모든 feature가 조건부 독립이라고 가정 (label이 주어졌을 때)  
    $X_i \perp X_{-i} | Y$   $\leftrightarrow$  $p(y,x_1,...,x_n) = p(y)\prod_{i=1}^{n}{p(x_i|y)}$      
  - classification 문제에 naive Bayes를 사용한다면
    - 데이터를 이용해 파라미터 학습: $P(Y), P(X_i|Y)$
    - Bayes rule을 이용해 예측  
      $P(Y|X) = \frac{P(X|Y)P(Y)}{\sum_{y}P(X|Y)P(Y)}$  
      where $P(X|Y) = \prod_{i=1}^{n}{p(x_i|Y)}$
- 판별(discrimitive) vs 생성(generative) 모형
  - 판별 모형은 $P(Y|X)$, 생성 모형은 $P(X|Y)$에 관심을 가짐
  - 생성 모형은 $P(Y)$도 필요함. $P(X,Y)=P(Y)P(X|Y)$  
    
     
    
