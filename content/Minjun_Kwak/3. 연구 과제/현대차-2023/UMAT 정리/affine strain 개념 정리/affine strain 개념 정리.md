균질화에서 너무 stiff해지는 것을 방지하고자 도입한 개념

The main advantages of this promising approach is that a single formulation should enable to predict accurately the macroscopic response of a wide range of non linear materials (viscoelastic, elasto-(visco)plastic, etc.).

이 유망한 접근법의 주요 장점은 단일 포뮬레이션으로 광범위한 비선형 재료(점탄성, 엘라스토(점성) 플라스틱 등)의 거시적 반응을 정확하게 예측

  

nonlinear한 경우엔 다음이 성립하지 않음. → 유한 time 증분으로 algorithmic stiff 사용

![[Untitled 66.png|Untitled 66.png]]

Unfortunately, as observed in various simulations, such an approach gives too stiff responses and can even violate a rigorous upper bound.

안타깝게도 다양한 시뮬레이션에서 관찰된 바와 같이 이러한 접근 방식은 너무 딱딱한 응답을 제공하며 심지어 엄격한 상한을 위반할 수도 있습니다.

→ 증분 공식으로 정확한 예측을 얻으려면 일부 조정을 수행해야

도그리는 이 affine 방식을 사용할 때 문제를 고전적인 균질화 방식에 따라 균질화할 수 있는 가상의 선형 열탄성 문제로 변환

![[1-s2.0-S0749641905000549-gr5.jpg]]

  

  

Doghri group의 2010년 논문에서는 여러 constitutive model에 적용 가능한 generic format을 고안 → inelastic behavior를 V로 구현? inelastic behavior is described by a set of scalar and/or tensor variables V.

기존의 affine formulation은 Laplace-Carson 변환이 필요해 LC domain에서 계산이 이루어지지만 여기선 time domain에서 directly 구현

→ C는 그대로 구성방정식 모델로 구하고, affine strain inc만 구하면 적용 가능함.

![[Untitled 1 30.png|Untitled 1 30.png]]

One further remark: since the incremental elasticity relation (22)is also affine, one might wonder why not simply use it in thermoelastic-like homogenization. The answer is that – assuming that the inclusions are stiffer than the matrix – this would lead to extremely stiff predictions since the elastic Hooke’s operator cel is much stiffer than the algorithmic tangent calg; see Pierard and Doghri (2006b) for a related discussion in elasto-plasticity.

내포물이 행렬보다 강성이 높다고 가정하면 탄성 후크의 연산자 cel이 알고리즘 탄젠트 calg보다 훨씬 강성이 높기 때문에 매우 엄격한 예측으로 이어질 수 있다

  

### Homogeneous J2 탄점소성의 경우

![[Untitled 2 28.png|Untitled 2 28.png]]

![[Untitled 3 25.png|Untitled 3 25.png]]

  

  

도그리 논문에서a_eps 쓰여진건 아무래도 thermoelastic case의 식인 것 같음.

  

[[정리]]