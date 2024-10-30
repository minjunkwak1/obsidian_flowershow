  

- 요청 필요한 샘플 : PA-GF30

  

## 조사할 부분

- puck criterion에서 쓰이는 parameter
    - 해당 parameter 추출 위한 실험 방법
    - SFRP에서는 다를 수 있기 때문에, SFRP에서 해당 parameter 추출을 위해 쓰이는 ASTM 방법 조사
    - 적합한 시편의 모양과 방향
- SFRP에서 특화된 파괴 기준이 있는지 조사

  

## Puck criterion

![[Untitled 47.png|Untitled 47.png]]

- 필요 파라미터
    - 인장/압축 파단 변형률 $\varepsilon_{1T}, \varepsilon_{1C}$﻿
    - 단일 방향 레이어 변형률$\varepsilon_{1} $﻿
    - 섬유 포아송비 $\nu_f$﻿
    - 탄성계수 $E_f $﻿
    - 섬유 수직방향 섬유의 평균응력 확대 상수 $m _ { \sigma f }$﻿
    - 섬유 수직방향 수직 응력성분$\sigma_2$﻿
    - 단일방향 레이어 전단병형률 및 전단응력$\gamma _ { 21 },\tau _ { 21 }$﻿
    - 섬유방향과 수직 빛 평행한 단일 레이어 shear strength$S_{21}$﻿
    - 파단면 각도 의존 파라미터$p _ { v p } ^ { + }, p _ { v p } ^ { - }, p _ { v v } ^ { - }$﻿
        - $p _ { v p } ^ { + } = - ( \frac { d\tau _ { 21 } } { d \sigma _ { 2 } } )$﻿
    - 인장 및 압축 파단강도$Y_T, Y_C$﻿
    - linear degradation에 의한 응력값$\sigma_{1D}$﻿
    

  

  

## Tsai-Wu

![[Untitled 1 16.png|Untitled 1 16.png]]

![[Untitled 2 15.png|Untitled 2 15.png]]

![[Untitled 3 13.png|Untitled 3 13.png]]

![[Untitled 4 11.png|Untitled 4 11.png]]

![[Untitled 5 10.png|Untitled 5 10.png]]

- X는 tensile과 compression에서의 strength, S는 shear strength
- 각 각도에서 0 45 90
- tensile, compression, shear
    - astm 찾기

  

## 17년 과제

- 인장시험
    - ASTM D3039
    - 축방향 및 횡방향
        - Elastic modulus (E1, E2)
        - tensile strength (Xt, Yt)
        - poisson’s ratio (v)
- 압축시험
    - ASTM D3410
    - 축방향 및 횡방향
        - compression strength (Xc, Yc)
- 전단 시험
    - In-plane shear test (ASTM D3518), Iosipescu shear test (ASTM D5379), Modified rail shear test (ASTM D7078)
        - 직교 이방성 재료 (orthotropic material) 의 특징을 갖는 섬유강화 복합재의 경우 균일한 전단력을 가해주기 매우 어렵다는 사실이 알려져 있기 때문에, 측정된 전단강성 및 전단강도 (G12 및 S) 를 상호 검증하고자 → 추후 D7078로 사용
            - 시험 결과 분석을 통해 가장 안정적인 측정값을 사용하여 정의함
        - shear modulus (G12)
        - shear strength (S)
- Off-axis 인장/압축 시험
    
    - 축방향(0도), 횡방향(90도) 외의 각도
        - 추후 개발된 거동 예측 모델의 유효성을 검증하기 위하여
    - 인장시험
        - (15°, 30°, 45°, 60°, 75°)
    - 압축시험
        - (30°, 45°, 60°)
    
    ![[Untitled 6 10.png|Untitled 6 10.png]]
    
      
    
      
    
    ## matrix
    
    - matrix-  
        점탄성 dma - TTSP (온도 속도 반영 되고 파라미터 구할 수 있음)  
        
    
    점소성 속도별 (인장/압축) 속도, 온도 3~5 개 필요
    
    - SFRP-  
        파괴모델  
        
    
    인장/압축 (각도별로 해야된다 적어도3개)