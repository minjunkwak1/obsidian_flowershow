![[Drucker_Prager_Yield_function_%EC%A0%95%EB%A6%AC_(1).pptx]]

![[J2J3_invariant_%EC%A0%95%EB%A6%AC.pdf]]

### Drucker-Prager effective stress

- 검증은 탄소성에서 ABAQUS built in을 활용해야 함.
    - 이후 yield, D2yield subroutine을 점소성 코드에 implement
    - 이 외에 corrector function에서 effective plastic strain update도 변화가 있음.
    - non associative flow rule도 사용하는게 맞는지 확인
- 우선 코드는 제욱이형이 쓴 코드를 활용
    - 관련한 이론적 식은 따로 정리 필요
    - (1/DK+BETA/3) 에 대한 이론적 근거가 있는지 확인
        - Abaqus manual D-P 설명 글에서 uniaxial tension case에서 찾을 수 있었음.
- FCT_YIELD 수정, FCT_D2YIELD 수정
    - 2nd derivative 검증 ← 현우형이 진행
- viscoplastic effective stress와 multiplier 사이 관계식 확인 필요
    - code에서는 아래와 같음
        
        $\Delta\bar\varepsilon^ p=\Delta \gamma \left ( \frac{1}{D}+\frac {\beta}{3} \right) \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ $
        
    - 논문 상으론 이렇게 나와있음. effective는 아님.
        
        ![[Untitled 72.png|Untitled 72.png]]
        
        ![[Untitled 1 36.png|Untitled 1 36.png]]
        
    - effective viscoplastic strain increment
        
        ![[Untitled 2 34.png|Untitled 2 34.png]]
        

  

### Plastic potential vs yield function

- plastic potential function
    - Du의 논문과 Darabi의 논문 모두 동일함. 아예 yield의 개념이 빠져있다.
        
        $g=\bar\tau-\beta\bar I_1$
        
    - DGDS도 동일함
- yield function
    - 이것도 두 논문에서 동일함. yield의 개념이 들어가있음
        
        $f=F ( \overline{{{\sigma}}}_{i j} )-\kappa( \varepsilon_{e}^{v p} )=\overline{{{\tau}}}-\alpha\bar{I}_{1}-\kappa( \varepsilon_{e}^{v p} )$
        
    - 다만 dfds의 계산에서는 kappa 가 stress에 무관.
        
        $$
        
- 그럼 associated flow rule일 경우에는?
    - g, f가 미분되지 않은 형태로 쓰이는 경우 확인
    - f 대신 ksi가 사용되는 경우
        - 이건 yield 판단 시에 사용함. (consistency condition)
    - flow rule에서 계수도 확인해야한다.
        - 이건 ver 2-2에서도 반영되어야 하는 사항같긴 함.

  

- 해볼 것
    - 2-2에서 flow rule 수정해서 비교

  

  

### Parameter (linear model)

![[Untitled 3 31.png|Untitled 3 31.png]]

![[cdruckprag-yield-merid.png]]

- Yield criterion
    
    $\begin{align*}$
    
    - q는 Von Mises effective stress
        
        $q=\sqrt{\frac{3}{2}(S:S)}$
        
    - r은 J3 invariants, 아바쿠스 상 정의는 **(1/3)이 적용되어 있음. t는 Darabi에서의 Tau와 동일
    - d는 ‘cohesion of material’
        
        - hardening이 어떻게 정의되는가에 따라 다르다고 함.
        - hardeining이 uniaxial compression yield stress로 정의된 경우, tension yield stress로 정의된 경우, cohesion(shear)으로 정의된 경우에 대해 다름.
        - 그래프 상으로 봤을 땐 p가 0일 때 yield stress인 것 같음. pi plane 상이라서 이렇게 나오는듯? yield stress가 compression/tension에 따라 정의되면 거기에 맞게 d가 결정됨
        
        $\begin{align*}d & =(1-\frac{1}{3}\tan\beta)\sigma_{c} \\ &d=(\frac{1}{K}+\frac{1}{3}\tan\beta)\sigma_{t}\end{align*} $
        
        ![[Untitled 4 24.png|Untitled 4 24.png]]
        
- flow potential eccentricity (ε)
    - defines the rate at which the function approaches the asymptote (the flow potential tends to a straight line as the eccentricity tends to zero).
    - For the general exponent model flow is always nonassociated in the _p_–_q_ plane. The default flow potential eccentricity is , which implies that the material has almost the same dilation angle over a wide range of confining pressure stress values.
- angle of friction (β)
    - yield function의 angle
    - abaqus에서의 tan β가 코드 상 β와 동일함.
- Flow stress ratio (K)
    - 0.778~1.0 사이 값을 가져야 convexity 보장 가능
        
        ![[Untitled 5 21.png|Untitled 5 21.png]]
        
- Dilation angle (ψ)
    - plastic potential의 angle
    - fully associated일 경우 beta와 동일함.
    - tangent psi가 3 이상이어야 한다.
- plastic flow
    
    ![[cdruckprag-lin-yield-p-t.png]]
    
    $G=t-p\ tan\ \psi $
    
      
    

## Digimat Drucker-Prager

- Yield function
    
    $$
    
    $\mathrm{M_{\phi} > 0, \ q > 0}\ ,\ 0 \leq\mathrm{H}_{\Phi} \leq1$
    
    - 각각 yield stress coefficient/ yield stress exponent/ yield pressure coefficient
    
    $\sigma_{m}=-\frac{1} {3} I_{1} ( \sigma)=-\frac{1} {3} ( \sigma_{1 1}+\sigma_{2 2}+\sigma_{3 3} ) $
    
- hardening mode
    
    ![[Untitled 6 19.png|Untitled 6 19.png]]
    
- Flow potential
    - D
        
        ![[Untitled 7 15.png|Untitled 7 15.png]]
        
- Digimat 입력 파라미터
    - yield stress coefficient M
    - yield pressure coefficient H (tangent beta)
    - hardening mode
    - Advanced paramet
        - Yield stress exponent q
        - eccentricity ξ
        - Dilation angle (in degree) Φ ← associated 면 H와 동일하게

  

  

  

  

## ABAQUS built in test

BC: x,y방향 -1

1. biaxial compression (J3>0)
    1. J2와 동일하게 설정, compression hardening/tensile hardening 동일
        1. mises응력 48.96
    2. ~~둘 다 -11.31~~
        1. abort 발생, The friction angle must be greater than or equal to zero.
    3. ~~alpha, beta 11.31, tensile hardening~~
        1. ~~mises응력 62.31~~
    4. alpha, beta 11.31, compression hardening
        1. mises응력 53.35
    5. alpha, beta 11.31, DK=0.8, compressive hardening
        1. mises응력 39.79
2. biaxial tension (J3<0)
    1. J2와 동일하게 설정, compression hardening/tensile hardening 동일
        1. mises응력 48.04
    2. ~~alpha, beta 11.31, compression hardening~~
        1. ~~mises응력 38.44~~
    3. alpha, beta 11.31, tensile hardening
        1. mises응력 44.8
    4. alpha, beta 11.31, DK=0.9, tensile hardening
        1. mises응력 50.22
    5. alpha, beta 11.31, DK=0.8, tensile hardening
        1. mises응력 57.11
3. uniaxial compression (J3<0)
    1. alpha, beta 11.31, DK=0.8, compressive hardening
        1. mises응력 43.66
    2. alpha, beta 11.31, DK=1, compressive hardening
        1. mises응력 43.66
4. uniaxial tension (J3>0)
    1. alpha, beta 11.31, DK=0.8, tensile hardening
        1. mises응력 43.02
    2. alpha, beta 11.31, DK=1, tensile hardening
        1. mises응력 43.02

  

### VPT composite ver3 test

1. biaxial compression(J3>0)
    1. ~~yield function 수정 전(FVAL=EFF_STRESS-ALPHA*CI1/3.)~~
        1. ~~alpha, beta=0~~
            1. ~~mises응력 48.95~~
        2. ~~alpha, beta=0.2~~
            1. ~~mises응력 42.1~~
        3. ~~alpha, beta=-0.2~~
            1. ~~mises응력 58.87~~
    2. yield function 수정 후(FVAL=EFF_STRESS+ALPHA*CI1/3.)
        1. alpha, beta=0
            1. mises응력 48.95
        2. alpha, beta=0.2, tensile hardening law
            1. **mises응력 62.31**
        3. alpha, beta=0.2, compressive hardening law
            1. **mises응력 53.35**
    3. DK 테스트
        1. alpha, beta=0.2, DK=0.8, compressive hardening law (CJ3/Cj2^3/2= 1)
            1. **mises응력 53.35**
        2. alpha, beta=0.2, DK=0.8, compressive hardening law, 식 변경
            1. **mises응력 39.43**
2. Uniaxial compression (J3<0)
    1. alpha, beta=0.2, DK=1, compressive hardening law
        1. **mises응력 43.66**
    2. alpha, beta=0.2, DK=0.8, compressive hardening law
        1. mises응력 32.28
        2. hardening 식 변경 시 43.44
    3. alpha, beta=0.2, DK=0.8, compressive hardening law, j3 부호 반대
        1. 42.88
3. biaxial tension (CJ3/Cj2^3/2= -1) (J3<0)
    1. alpha, beta=0.2, tensile hardening law
        1. **mises응력 44.8**
    2. DK 테스트
        1. alpha, beta=0.2, DK=0.8 tensile hardening law
            1. mises응력 44.71
        2. alpha, beta=0.2, DK=0.8 tensile hardening law, J3 부호 반대
            1. mises응력 52.56
        3. alpha, beta=0.2, DK=0.8 tensile hardening law, hardening 식 변경
            1. mises응력 35.08
        4. alpha, beta=0.2, DK=0.8 tensile hardening law, 식 변경
            1. mises응력 55.96
4. Uniaxial tension (CJ3/Cj2^3/2=1) (J3>0)
    1. alpha, beta=0.2, tensile hardening law
        1. **mises응력 43.02**
    2. DK 테스트  
        tensile hardeing이므로, hardening이 K값에 영향을 받음?  
        ABAQUS에선 결과가 달라지지 않음. 확인 필요함.  
        1. alpha, beta=0.2, DK=0.8 tensile hardening law, J3 반대 (CJ3/Cj2^3/2=-1)
            1. mises응력 47.09
        2. alpha, beta=0.2, DK=0.8 tensile hardening law, J3 부호 그대로 (CJ3/Cj2^3/2= 1)
            1. mises응력 53.42
        3. alpha, beta=0.2, DK=0.8 tensile hardening law, hardening 식 변경
            1. mises응력 43.11

  

  

### DP 코드 관련 이슈

1. J2에서 사용 가능했던 spectral isotropization을 사용 불가능
    1. 기본 가정
        
        ![[Untitled 8 12.png|Untitled 8 12.png]]
        

![[Untitled 9 12.png|Untitled 9 12.png]]

![[Untitled 10 11.png|Untitled 10 11.png]]

![[Untitled 11 9.png|Untitled 11 9.png]]

1. general method의 경우
    1. anisotropic operator 모든 케이스에 대해 적용 가능함.