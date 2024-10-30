**Q**

- DSTRAN_** 정리
- INCLUDE ‘ABA_PARAM.INC’
- ‘EPS’ → 분모같은데 0이 들어가지 않도록 충분히 작은 값을 더해주는 개념
- 각 state variable 뭔지 알 수 있는지?
- 변수 선언 후 값 초기화를 해주는건 임의의 값으로 할당되기 때문?
- hardening model Voce와 hollomon 중 어떤건지
- F_val 식 왜 저렇게 되는지 잘 모르겠음
    - 그냥 EPS 삭제
- MT verification의 경우, 뭔가 보정에 대한 iteration이라고 들었음
    - strain increment가 fiber와 matrix에 대해서 MT 방법에 의해 나눠질 때 matrix와 fiber의 C가 사용됨
    - 근데 이 matrix strain increment로 delta stress와 C_mat이 도출됨
    - 따라서 실제 C값과 도출되는 C값에 차이가 있고? 이에대한 보정이 필요한가봄
- XX값이 뭐지?
    - 무시
- heredity integral and dstress
- regularization 부분
    - DDSDDE를 homogenization 고려하기 위해 만든 term. matrix만 쓰기 위해선 따로 DDSDDE 계산식을 추가해줘야함. DDSDDE는 일종의 수렴 판단 term인 것으로 추측되며 NN에서 출력값으로 계산할 수 있어야할듯
- P_TOTAL, T_TOTAL
- DEL_DVPDS
    - 제욱이형이 준 논문 참고
    

  

**공통변수**

|변수명|크기|값|의미||
|---|---|---|---|---|
|NDI|||number of direct stress component||
|NSHR|||number of shear stress component||
|KSTEP|||step||
|KINC|||increment number||
|TIME|2||TIME(1) states the value of step time and the second, i.e. TIME(2), is the value of total time, both at the beginning of the current increment.||
|DTIME|||current time increment||
|NOEL|||element number||
|NPT|||integration point number||
|DDSDDE|NTENS,NTENS||2D format of Jacobian matrix||
|NTENS||NDI + NSHR|size of stress(strain) component array  <br>6 for a general 3D, 4 for a plane stress model||
|STRESS|NTENS||vector form of stress tensor||
|STRAN|NTENS||vector form of strain tensor||
|DSTRAN|NTENS||increment of strain||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||

**Input parameter**

|변수명|크기|값|의미|
|---|---|---|---|
|N_FCINTER||PROPS(1)|1. STATIC, 2. CREEP, 3. FATIGUE, 4. CREEP-FATIGUE|
|N_FILE_format||PROPS(2)|섬유 배향 정보 파일 분류|
|E_FIB||PROPS(3)|Elastic modulus (E_f)|
|V_FIB||PROPS(4)|푸아송 비 (nu_f)|
|VOL_FIB||PROPS(5)|Aspect ratio|
|ASPECT||PROPS(6)|섬유의 부피 분율 (V_f)|
|E_MAT0||PROPS(7)|Instantaneous elastic modulus (E_0)|
|V_MAT||PROPS(8)|푸아송 비 (nu_m)|
|N||PROPS(9)|==**Generalized Maxwell model component 개수 (N)**==|
|TAU|1,N|PROPS(9+K1)|GMM 모델의 각 component의 relaxation time (tau_j)|
|E_RATIO|1,N|PROPS(9+N+K1)|GMM 모델의 각 component의 modulus ratio (p_j)|
|YIELD_0||PROPS(2*N+10)|항복 응력 yield strength (sigma^0_y)|
|BETA||PROPS(2*N+11)|Drucker-Prager effective stress hydrostatic stress 영향 파라미터 (beta)|
|DK||PROPS(2*N+12)|Drucker-Prager effective stress 파라미터 (K)|
|ETA||PROPS(2*N+13)|점소성 파라미터 viscoplastic coefficient (eta), viscosity parameter|
|VN||PROPS(2*N+14)|점소성 파라미터 viscoplastic exponent (VN)|
|B||PROPS(2*N+15)|Isotropic hardening parameter (B)|
|C||PROPS(2*N+16)|Isotropic hardening parameter (C)|
|CA||PROPS(2*N+17)|Kinematic hardening parameter (Ca)|
|CB||PROPS(2*N+18)|Kinematic hardening parameter (Cb)|
|CRITEMP||PROPS(2*N+19)|기준 온도 (T_ref)|
|ALPHA||PROPS(2*N+20)|열팽창계수 (alpha)|
|ATC1||PROPS(2*N+21)|Horizontal shift factor-WLF 계수 ( C1)|
|ATC2||PROPS(2*N+22)|Horizontal shift factor-WLF 계수 ( C2)|
|BTC1||PROPS(2*N+23)|Vertical shift factor (B1)|
|PBT1||PROPS(2*N+24)|Thermo-viscoplastic coefficient (theta_1)|
|Xt||PROPS(2*N+25)|Longitudinal tensile strength-Fiber 축 (X_t)|
|Yt||PROPS(2*N+26)|Transverse tensile strength–Fiber의 수직인 축 (Y_t)|
|Xc||PROPS(2*N+27)|Longitudinal compressive strength-Fiber 축 (X_c)|
|Yc||PROPS(2*N+28)|Transverse compressive strength-Fiber 수직인 축 (Y_c)|
|St||PROPS(2*N+29)|Shear strength (S)|
|F_a||PROPS(2*N+30)|Chaboche 식의 피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|F_BETA||PROPS(2*N+31)|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|F_M0||PROPS(2*N+32)|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|F_Sl0||PROPS(2*N+33)|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)  <br>FATIGUE LIMIT|
|F_lamda1||PROPS(2*N+34)|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|F_lamda2||PROPS(2*N+35)|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|C_A||PROPS(2*N+36)|Chaboche 식의 크립 파라미터 (순서대로 A,R,K)|
|C_R||PROPS(2*N+37)|크립 파라미터 (순서대로 A,R,K)|
|C_K||PROPS(2*N+38)|크립 파라미터 (순서대로 A,R,K)|
|RVE_static||PROPS(2*N+39)|정적 RVE 파괴 기준 (RVE_limit)  <br>90이면 각 요소에서 90% 부피 분율의 유사결정립이 파괴될 때 해당 요소가 파괴된다고 판단|
|HOLDTIME||PROPS(2*N+40)|블록 내 해석하고자 하는 크립 시간 (holdtime)|
|freq1||PROPS(2*N+41)|블록 내 1차 피로 주파수 (f_1)|
|freq2||PROPS(2*N+42)|블록 내 2차 피로 주파수 (f_2)|
|fat_cycle1||PROPS(2*N+43)|블록 내 1차 피로 cycle 수 (Nf_1)|
|fat_cycle2||PROPS(2*N+44)|블록 내 2차 피로 cycle 수 (Nf_2)|
|F_N_Sl0||1.00E+09||
|F_M_BETA||(1/F_M0)**(F_BETA)||
|VNINV||1/VN|1/점소성 파라미터 viscoplastic exponent (VN)|
|TIME|||피로 가하는 시간인듯|
|DTIME|||delta time?|
|HOLDTIME|||블록 내 크립 시간 (holdtime)|
|N_F1S||FLOOR((TIME)*freq1)|\#ofcycles1, floor는 버림|
|N_F1E||FLOOR((TIME+DTIME)*freq1)||
|TIME_F1||(TIME+DTIME)*freq1-N_F1S||
|N_C||FLOOR((TIME)/HOLDTIME)||
|TIME_C1||(TIME+DTIME)/HOLDTIME|creep-fatigue 해석일 경우 TIME_C1=TIME_C1-(3/freq1)/HOLDTIME|
|N_F2S||FLOOR((TIME-HOLDTIME-3/freq1)*freq2)|\#ofcycles2|
|N_F2E||FLOOR((TIME+DTIME-HOLDTIME-3/freq1)*freq2)||
|TIME_F2||(TIME+DTIME-HOLDTIME-3/freq1)*freq2-N_F2S||

|   |   |   |   |
|---|---|---|---|
|**PROPS**|**재료**|**특성**|**물성 파라미터 이름**|
|1|모드|관찰 모드|1. STATIC, 2. CREEP, 3. FATIGUE, 4. CREEP-FATIGUE|
|2|섬유|섬유 배향|섬유 배향 정보 파일 분류|
|3|섬유|탄성|Elastic modulus (E_f)|
|4|섬유|탄성|푸아송 비 (nu_f)|
|5|섬유|균질화 물성|Aspect ratio|
|6|섬유|균질화 물성|섬유의 부피 분율 (V_f)|
|7|수지|점탄성|Instantaneous elastic modulus (E_0)|
|8|수지|점탄성|푸아송 비 (nu_m)|
|9|수지|점탄성|Generalized Maxwell model component 개수 (N)|
|10~(N+9)|수지|점탄성|GMM 모델의 각 component의 relaxation time (tau_j)|
|(N+10)~(2N+9)|수지|점탄성|GMM 모델의 각 component의 modulus ratio (p_j)|
|2N+10|수지|점소성|항복 응력 yield strength (sigma^0_y)|
|2N+11|수지|점소성|Drucker-Prager effective stress hydrostatic stress 영향 파라미터 (beta)|
|2N+12|수지|점소성|Drucker-Prager effective stress 파라미터 (K)|
|2N+13|수지|점소성|점소성 파라미터 viscoplastic coefficient (eta)|
|2N+14|수지|점소성|점소성 파라미터 viscoplastic exponent (VN)|
|2N+15|수지|점소성|Isotropic hardening parameter (B)|
|2N+16|수지|점소성|Isotropic hardening parameter (C)|
|2N+17|수지|점소성|Kinematic hardening parameter (Ca)|
|2N+18|수지|점소성|Kinematic hardening parameter (Cb)|
|2N+19|수지|온도|기준 온도 (T_ref)|
|2N+20|수지|온도|열팽창계수 (alpha)|
|(2N+21)~ (2N+22)|수지|점탄성-온도|Horizontal shift factor-WLF 계수 (순서대로 C1, C2)|
|2N+23|수지|점탄성-온도|Vertical shift factor (B1)|
|2N+24|수지|점소성-온도|Thermo-viscoplastic coefficient (theta_1)|
|2N+25|SFRP|Tsai-Wu|Longitudinal tensile strength-Fiber 축 (X_t)|
|2N+26|SFRP|Tsai-Wu|Transverse tensile strength–Fiber의 수직인 축 (Y_t)|
|2N+27|SFRP|Tsai-Wu|Longitudinal compressive strength-Fiber 축 (X_c)|
|2N+28|SFRP|Tsai-Wu|Transverse compressive strength-Fiber 수직인 축 (Y_c)|
|2N+29|SFRP|Tsai-Wu|Shear strength (S)|
|2N+30 ~ 2N+35|SFRP|피로|피로 파라미터 (순서대로 a,β,M_(0,),S_l0,λ_1,λ_2)|
|2N+36 ~ 2N+38|SFRP|크립|크립 파라미터 (순서대로 A,R,K)|
|2N+39||정적|정적 RVE 파괴 기준 (RVE_limit)  <br>90이면 각 요소에서 90% 부피 분율의 유사결정립이 파괴될 때 해당 요소가 파괴된다고 판단|
|2N+40||경계 조건|블록 내 크립 시간 (holdtime)|
|2N+41||경계 조건|블록 내 1차 피로 주파수 (f_1)|
|2N+42||경계 조건|블록 내 2차 피로 주파수 (f_2)|
|2N+43||경계 조건|블록 내 1차 피로 cycle 수 (Nf_1)|
|2N+44||경계 조건|블록 내 2차 피로 cycle 수 (Nf_2)|

  

1. **정적/피로/크립/크립-피로 모델 선택, PROPS(1)**

물성 정보 창에 1, 2, 3, 4 중 하나의 숫자를 입력함. 단섬유강화 복합재료의 정적 파괴를 보기 위해서는 PROPS(1)에 1을, 크립 수명을 보기 위해서는 PROPS(1)에 2를, 피로 수명을 보기 위해서는 PROPS(1)에 3을, 크립-피로 수명을 보기 위해서는 PROPS(1)에 4를 입력해주면 됨.

  

1. **섬유 배향 정보, PROPS(2)**

섬유 배향 정보는 사출 시뮬레이션을 통해 얻을 수 있는데, MOLDEX3D에서 제공하는 섬유 배향 정보 파일의 확장자명은 o2d이며 MOLDFLOW에서 제공하는 섬유 배향 정보 파일의 확장자명은 xml임. 일관적인 UMAT 활용성을 위하여, **모든 섬유 배향 정보 파일의 파일명은 “OT.txt”로 변경해야 하고, ABAQUS 시뮬레이션의 working directory에 있어야 함**. **시뮬레이션에서 o2d 데이터를 사용할 경우 PROPS(2)에 1을, xml 데이터를 사용할 경우 PROPS(2)에 2를 입력해주면 됨**.

  

1. **점탄성 파라미터 도출 방법, PROPS(7)~ PROPS(2N+9)**

PROPS(7 ~ 2N+9)의 값은 수지의 점탄성 재료 물성을 나타냄. 수지의 점탄성 재료 물성은 DMA test 결과 (storage modulus–frequency 커브)를 Prony series 피팅하여 구함. 피팅 식은 아래와 같음.  
  

![[Untitled 63.png|Untitled 63.png]]

E’’(w)는 storage modulus, ω 는 frequency를 나타냄. 해당 식을 통해 E_0, τ_j 와 p_j 를 도출하여 각각 PROPS(7), PROPS(10~N+9), PROPS(N+10~2N+9)에 입력함. Maxwell component의 개수 N은 PROPS(9)에 입력하여 자유롭게 설정 가능함. PROPS(8)는 푸아송 비로 본 연구에서는 고분자의 보편적인 값인 0.3으로 가정함.

  

1. **점소성 파라미터 도출 방법, PROPS(2N+10)~ PROPS(2N+18)**
    - **Drucker-Prager 파라미터**

PROPS(2N+11, 2N+12) 의 값은 Drucker-Prager 유효 응력 파라미터임. 이중 PROPS(2N+11)은 압축 물성을 반영하는 파라미터로 아래 식의  와 같음.

는 다음과 같이 구할 수 있음.

1. 일축 인장, 일축 압축 시험을 진행하여야 함.
2. 인장 항복 응력 ()과 압축 항복 응력()을 구함

그 후 유효 응력을 아래와 같이 설정함.

본 연구에서는 해당 파라미터를 구할 수 있는 실험이 없기 때문에, PROS(2N+11)는 0, PROPS(2N+12)은 1으로 설정함 (Von Mises 유효 응력과 같음). 참고 문헌에서 보고된 PP의 PROPS(2N+11) 및 PROPS(2N+12)은 각각 0.1, 0.87 임.

- **속도 의존성 및 hardening 파라미터**

PROPS(2N+13~2N+16)의 값은 수지의 Perzyna flow rule (속도 의존성) 및 isotropic hardening을 나타내는 파라미터임. 이에 해당하는 수지의 점소성 물성은 아래 식에 대한 surface plot fitting을 통해 구할 수 있음.

위 식의 변수는  (true viscoplastic stress),  (viscoplastic strain),  (viscoplastic strain rate) 3 가지이고 surface plot fitting을 통해 구하는 파라미터는 yield strength (), hardening parameter (,) 및 속도의존성을 나타내는 파라미터 (,) 5 가지임. 이를 구하기 위해서는 아래와 같은 과정이 필요함.

1. 먼저 서로 다른 속도의 인장 시험을 진행하여야 함. 정확한 파라미터를 도출하기 위해서는 4가지 속도에 대해 인장 시험이 권장됨. 본 연구에서는 참고 논문의 실험 개수의 한계로 인하여 2가지 속도에 대한 인장 시험을 통해 구함.
2. Stress-strain 커브를 통해 항복 응력을 구함.
3. 각 인장 시험에 대한 stress-strain 커브를 true stress–viscoplastic strain 커브로 변환함.
4. X축-plastic strain, Y축-plastic strain rate, Z축-true stress 로 하여 위 식을 surface plot fitting을 진행함. 본 논문에서는 MATLAB을 통해 surface plot fitting을 진행하였고, 참고 논문을 통해 진행한 surface plot fitting 및 파라미터 값은 아래 그림과 같음.

  

**열효과**

|변수명|크기|값|의미|
|---|---|---|---|
|DTEMP|||delta temperature for thermal expansion|
|DTHERM|1,NTENS|ALPHA*DTEMP(NDI까지), 0(NTENS까지)|alpha는 열팽창계수. 열팽창 공식 deltaV/V_0 = alpha*deltaT  <br>열팽창에 의한 delta strain인듯|
|AT||AT=10**(-ATC1*(TEMP-CRITEMP)/(ATC2+TEMP-CRITEMP))|[[UMAT 파라미터]] shift factor, 수평 방향 이동 정도|
|VEDTIME||VEDTIME=DTIME/AT|viscoelastic delta time?|
|BT|||[[UMAT 파라미터]] shift factor, 수직 방향 이동 정도|
|E_MAT0||E_MAT0=E_MAT0/BT||
|PBT||PBT=EXP(-PBT1*(TEMP-CRITEMP))|thermo-viscoplastic parameter(theta)  <br>  <br>[[UMAT 파라미터]]|
|VPDTIME||DTIME/PBD|viscoplastic delta time?|
|C||C=C*PBT|Hardening parameter|
|YIELD_0||YIELD_0=YIELD_0*PBT|initial yield stress|
|ETA|||점소성 파라미터 ETA|
|XX||||

![[Untitled 1 27.png|Untitled 1 27.png]]

![[Untitled 2 25.png|Untitled 2 25.png]]

**Orientation tensor**

|변수명|크기|값|의미|
|---|---|---|---|
|Ori_OT|3,3|(/Ori_OT11, Ori_OT12, Ori_OT13, Ori_OT12, Ori_OT22, Ori_OT23, Ori_OT13, Ori_OT23, Ori_OT33/), (/3,3/)||
|Vec_OT|6|(/Ori_OT(1,1), Ori_OT(2,2), Ori_OT(3,3), Ori_OT(1,2), Ori_OT(1,3), Ori_OT(2,3)/)|3D orientation element, UMAT convention에 따라 순서 배치|
|PRI_VAL|NDI||principal value|
|DIR_COS|NDI,NDI||direction cosines of the principal direction|
|Trans_OT|3,3|((/PRI_VAL(1), ZERO, ZERO,  <br>ZERO, PRI_VAL(2), ZERO,  <br>ZERO, ZERO, PRI_VAL(3)/)|주응력 대각행렬|

**Pseudograin decomposition**

|변수명|크기|값|의미|
|---|---|---|---|
|PGNUM|||number of pseudograin|
|PG_angle|2,NumPG||각 유사결정립의 국소주축방향 정보 (Φ,θ)  <br>12개 데이터를 모두 담으며, 대칭성을 고려하여 대입됨|
|PG_weight|NumPG||각 pseudograin의 부피분율  <br>12개이므로 계산된 AB2W 값을 4로 나누어 1,4,7,10번/… PG에 적용|

**Eshelby tensor calculation**

|변수명|크기|값|의미|
|---|---|---|---|
|ESH_Ii|NDI||Eshelby integral|
|ESH_Iij|NDI,NDI||Eshelby integral|
|ESH_Tensor|6,6||Eshelby tensor|

**FCT_NN**

|변수명|크기|값|의미|
|---|---|---|---|
|NumL||5|hidden layer 수|
|NumI||3|input variable 개수 (a11,a22,a33)|
|NumO||2|output variable 개수(alpha, beta)|
|NumN||10|neuron 개수|
|A|NumI||input으로 들어가는 principal value|
|B|NumO||output으로 나오는 alpha, beta|
|b1~b5|NumN||hidden layer 각 neuron에 대응되는 값(?)|
|b6|NumO|||
|a1~a5|NumN|||
|a6|NumO|||
|LW1~LW6|||각 뉴런에서 다음 층 뉴런으로 이어질 때의 가중치로 추정|
|xst1~xst3||||
|yst1~yst3||||

**PG ROTATION TENSOR**

|변수명|크기|값|의미|
|---|---|---|---|
|BASIS1|3,12||각 PG에서의 주축 방향 정보를 cartesian coordinate로 변환시킨 값|
|BASIS2|3,12||NN_ORTHO에 BASIS1 대입 후 출력값|
|BASIS3|3,12||BASIS1, BASIS2를 cross한 값|
|BASIS_PG|3,3,12||각 Pseudograin의 basis1,basis2,basis3|
|||||

**1st homogenization (각 PG fiber+matrix)**

|변수명|크기|값|의미|
|---|---|---|---|
|VOL_MAT|real|1-VOL_FIB|matrix volume fraction|
|C_FIB|6,6|NDI*NDI까지  <br>비대각 성분은 LAM_FIB  <br>대각 성분은 2*MU_FIB+LAM_FIB  <br>NDI 이후 대각성분은 MU_FIB|섬유 강성 텐서|
|C_MAT|6,6|||
|C_I|6,6|||
|C_FRP|6,6|||
|C_VOIGT|6,6|||
|C_MATinf|NTENS,NTENS|NDI*NDI까지  <br>비대각 성분은 LAM_mat  <br>대각 성분은 2*MU_FIB+LAM_mat  <br>NDI 이후 대각성분은 MU_mat|elastic stiffness of matrix -infinite에서 equilibrium modulus|
|S_MATinf|NTENS,NTENS||C_MATinf의 역행렬|
|C_MAT_PGs|NTENS,NTENS,NumPG|||
|CV_TERM2|6|||
|MU_FIB|real|E_FIB/(2*(1+V_FIB))|Mu|
|LAM_FIB|real|V_FIB*E_FIB/((1+V_FIB)(1-2*V_FIB))|Lambda|
|E_FIB|real||elastic modulus|
|V_FIB|real||poisson’s ratio|

![[Untitled 5 15.png|Untitled 5 15.png]]

![[Untitled 4 17.png|Untitled 4 17.png]]

**Viscoelastic properties**

|변수명|크기|값|의미|
|---|---|---|---|
|N||PROPS(9)|Generalized Maxwell model component 개수 (N)|
|GAMMA||GAMMA=EMU(K1)/E_MATinf (for each N)|modulus ratio of maxwell model? E_j/E_inf|
|EMU||EMU(K1)=E_RATIO(K1)*E_MAT0||
|TERM1||TERM1=TERM1+GAMMA*(1-EXP(-VEDTIME/TAU(K1)))/(VEDTIME/TAU(K1))  <br>for each N||
|VEDTIME|||viscoelastic delta time(time increment)|
|TAU|||relaxation time of maxwell model|
|VEM||VEM(K1,K2)=(TERM1+1)*C_MATinf(K1,K2)|C^(n+1)_ve (visco elastic), C_MATinf는 E0|
|VEMINV|||VEM 역행렬|
|||||

Maxwell model

![[Untitled 3 22.png|Untitled 3 22.png]]

![[Untitled 4 18.png|Untitled 4 18.png]]

  

**Pseudograin matrix stiffness matrix**

|변수명|크기|값|의미|
|---|---|---|---|
|DSTRAN_PGs||||
|||||
|||||
|||||
|||||
|||||
|||||

  

**각 PG에서의 계산**

|변수명|크기|값|의미|
|---|---|---|---|
|N_STV_PG||N_STV_PG=(6_N+110)_(K3-1)+5  <br>K3은 각 PG의 i|number of state variables in each PG==  <br>왜 이렇게 계산되는지는 잘 모르겠음  <br>==|
|DSTRAN0|NTENS|DSTRAN0 = V_TEMP - VOL_FIB/VOL_MAT*V_TEMP2|PG axis에서의 strain|
|TERM2|NTENS|==TERM2(K2) = TERM2(K2)+STATEV(N_STV_PG+50+6*K1+K2)*(1-EXP(-VEDTIME/TAU(K1)))==||
|STATEV|NSTATV||array containing the solution-dependent state variables  <br>State variable can be used to store your data for each material point which can be used for the calculation purpose on next iteration/increment.  <br>These are passed in as the values at the beginning of the increment and must be returned as the values at the end of the increment.|
|NSTATV|||Number of solution-dependent state variables|
|==DSTRAN_IN==|NTENS||VEMINV dot TERM2  <br>여기에 DTHERM으로 열효과 반영  <br>INELASTIC VISCOELASTIC DSTRAN0 (PG axis)|
|P_TOTAL||||
|T_TOTAL||||
|||||
|||||
|||||
|DSTRAN0||||
|DSTRAN1||||

  

**INNER LOOP**

|변수명|크기|값|의미|
|---|---|---|---|
|NL_2|i|1~100|loop 순번|
|DSTRESS0|NTENS|FCT_Edotv에서 도출|PG axis에서의 stress|
|STRESS0||STATEV(N_STV_PG+14+K1)+DSTRESS0(K1)|trial stress|
|GAMMA||EMU(K1)/E_MATinf||
|VEM||||
|C_VP_STRAIN|NTENS|STATEV(N_STV_PG+1+K1)||
|P_VP_STRAIN|NTENS|STATEV(N_STV_PG+1+K1)||
|BSTRESS|NTENS|STATEV(N_STV_PG+7+K1)||
|DEL_VE_STRAN|NTENS|||
|DSTRAN_AF|NTENS|||
|T_EVPS||STATEV(N_STV_PG+1)||
|DEL_EVPS||||
|DEL_VPLAMULTI||||
|DELDEL_VPLAMULTI||||
|||||

  

**FCT_YIELD**

|변수명|크기|값|의미|
|---|---|---|---|
|SMEAN|NTENS|SMEAN(K1)=(T_STRESS(1)+T_STRESS(2)+T_STRESS(3))/3  <br>NDI까지, shear의 경우 0|즉, 3개의 같은 값과 3개의 0으로 구성|
|SDEV|NTENS|SDEV(K1)=T_STRESS(K1)-SMEAN(K1)-BSTRESS(K1)|deviatoric stress|
|BSTRESS|||state variable중 하나인데 뭔지 잘 모르겠음|
|CI1||CI1=T_STRESS(1)+T_STRESS(2)+T_STRESS(3)|invariants|
|CJ2||CJ2=1.5*(SDEV(1)**2+SDEV(2)**2+SDEV(3)**2+2_SDEV(4)**2  <br>+2  <br>_SDEV(5)**2+2*SDEV(6)**2)|invariants|
|CJ3||CJ3=4.5*(SDEV(1)_(SDEV(1)**2+SDEV(4)**2+SDEV(5)**2)  <br>+SDEV(2)  <br>_(SDEV(4)**2+SDEV(2)**2+SDEV(6)**2)  <br>+SDEV(3)  <br>_(SDEV(5)**2+SDEV(6)**2+SDEV(3)**2)  <br>+2  <br>_SDEV(4)*(SDEV(1)*SDEV(4)+SDEV(2)_SDEV(4)+SDEV(5)SDEV(6))  <br>+2SDEV(5)  <br>_(SDEV(1)*SDEV(5)+SDEV(4)_SDEV(6)+SDEV(3)SDEV(5))  <br>+2SDEV(6)  <br>_(SDEV(4)*SDEV(5)+SDEV(2)*SDEV(6)+SDEV(3)*SDEV(6)))|invariants|
|DJ2DS|||delta J2/delta stress|
|DJ3DS|||delta J3/delta stress|
|DP||(sqrt(CJ2))/2 * ( (1+1/DK) - (1-1/DK)* CJ3/(sqrt(CJ2**3)+EPS))|Deviatoric effective shear stress, Tau|
|DK|||pressure dependency, d|
|==EPS==||1.0E-6, 등|분모같은데 0이 들어가지 않도록 충분히 작은 값을 더해주는 개념|
|GVAL||GVAL=(DP+BETA*CI1/3)/(1-XX)|drucker prager effective stress|
|==XX==|||??, 앞에서 0으로 두었음|
|==FVAL==||FVAL=GVAL-(1/DK+BETA/3)*_(YIELD_0/EPS+(C*_T_EVPS**B))|B는 hardening parameter|
|==T_EVPS==|||effective viscoplastic strain?|
|==Hp==||Hp=C*_B*_(T_EVPS+EPS)**(B-1)||
|||||

**FCT_VP_CORRECT**

|변수명|크기|값|의미|
|---|---|---|---|
|T_STRESS|||Trial stress|
|T_EVPS|||effective viscoplastic strain|
|C_VP_STRAIN||||
|BSTRESS||||
|VNINV||1/VN|점소성 parameter VN의 역수|
|DEL_VPLAMULTI|||delta lambda, viscoplastic multiplier|
|DELDEL_VPLAMULTI|||delta(delta lambda), multiplier 값 update를 위한 delta값|
|DEL_EVPS||||
|XX||||
|DEL_T_STRESS||||
|DEL_VE_STRAN||||
|RESIDUAL||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||

  

|변수명|크기|값|의미|
|---|---|---|---|
|||||
|||||
|||||
|||||
|||||
|||||
|||||