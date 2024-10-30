  

### 교수님 오더

1. 간단한 3D 탄소성 J2 시스템에 대해 Doghri가 초창기에 homogenization 에 대해 논문을 냈는데, 이걸 보고 유맷 제작
2. Eshelby tensor를 새로 구해야하는지 아닌지를 검증.
3. homogenization scheme을 제대로 이해한건지 확인

  

### 교수님 2차 오더

1. 변수들 정리하고 안쓰는 FUNCTION과 subroutine 내부 변수들도 정리
2. 변수 이름 정리
3. 인수들 순서 정리
4. out 붙은 값들 필요한지?
5. 밖에서 계산된 값들 인수로 받는 것 고려
6. CP_STRAIN 2번 사용됨
7. deldel multi 식 확인? Cbar CMAT 잘 사용된건지?
8. loop 밖에서 Cbar 다시 계산 필요함
9. FVF=0으로 해서 테스트

  

### 교수님 3차 오더

1. stress strain 변환 고려해서 코드 수정 완료함
2. 그렇게 해서 DDSDDE를 제대로 계산하는 것 까지 완료했고, 이 DDSDDE를 이용했을 때 stress update가 잘 된다고 함.
3. 이제 이 DDSDDE를 이용해서 균질화 코드에 적용해볼 것.

  

### 교수님 4차 오더

1. 위의 코드 결과를 보았을 떄 균질화에 대해서는 우리가 이해를 잘못하고 있는 부분이 없다고 하심
    1. 이에 대해서는 몇가지 예시를 더 확인해보고 확실히 할 것
2. 그럼 이제 여기에 점소성을 붙일 것
    1. 제욱이형 코드 참고
    2. DDSDDE 교수님이 쓰신 과정 보고 따라가볼 것
    3. 구한 DDSDDE를 이전에 적용한 stress와 strain에서의 sqrt(2) 연산 적용해서 제대로 구한 1st, 2nd derivative를 적용해 구해볼 것.
    4. 이 때 이후에 Drucker Prager로 구했을 때에도 subroutine만 바꾸면 되도록 subroutine 외부에서 추가적인 연산을 적용할 것.
3. 코드 검증
    1. digimat을 현우형에게 부탁하거나
    2. 아바쿠스 내장 사용

  

### Tensor notation

  

### 확인 필요 부분

1. statev 제대로 들어가게 되는지
2. CMAT
3. TEMP들 정리

  

### 확인 필요 부분

1. hardening law는 어떤걸 해야하는지?
    1. power law isotropic hardening (R(p)=kp^m) , f에 사용될 때는 R+Sig_Y
        
        ![[Untitled 68.png|Untitled 68.png]]
        
2. 나중에 Drucker Prager부분 yield function J2 관련해서 제대로 들어간건지 확인 필요
    1. D2_Yield에서도 DJ2DS에 계수 확인 필요. 이건 J2에서도 영향을 주는 값이 됨
        
        ![[Untitled 1 32.png|Untitled 1 32.png]]
        
3. D2_yield 식 맞는지 확인 필요. 일단 DK 관련 term을 다 삭제함
4. DDSDDE도 식이 같은지?

  

### 수정 필요한 부분

1. J2 plasticity 로 변경
    1. 점탄성 부분 삭제
    2. Drucker Prager에서 변형하면 J2 될거같음
    3. yield function과 plastic corrector 위주로 수정이 필요함
    4. **D2_YIELD도 Von Mises에 맞춰 수정 필요**
2. 불필요한 subroutine 삭제
3. EMINV 계산하는 코드 누락됨
4. 변수명 바꾸면서 줄 벗어나는 부분 확인

### Property

property 입력은 UMAT 내에서 바로 입력

  

### Eshelby tensor calculation

별도의 ot 입력 없이 1방향으로 사용

AR = (/ASPECT, ONE, ONE/)

  

### Homogenization

1. Initialization
2. Fiber와 Matrix stiffness matrix 계산
    1. VEM 대신 다른 값으로 수정 필요
3. homogenization loop(Nl_2) 진입 전 initializing
    1. fiber 기준으로 수정하자
4. **Iteration** (*Doghri 논문 기준 scheme*)
    
    1. fiber의 constitutive box 호출, fiber의 평균 strain과 평균 strain 변화량을 인수로 받음  
        → fiber의 algorithmic moduli 계산, fiber의 reference moduli로 사용함  
        
    2. matrix에서의 average strain 계산
        
        $$
        
    3. matrix의 constitutive box 호출, matrix의 평균 strain과 평균 strain 변화량을 인수로 받음  
        → matrix의 algorithmic moduli 계산, matrix의 reference moduli로 사용함  
        
    4. reference matrix moduli로부터 isotropic part 추출
        
        1. 코드에 이미 해당 부분이 있음 확인, elastic 부분을 거쳤더라도 이 곳으로 도달해야한다.  
            지금까지 결과에서는 (visco)elastic에선 문제가 없긴 했음. 근데 현재 scheme에선 elastic 에서 이 과정을 거치는 것 같지 않음  
            
        
        $c^{i s o}\equiv(I^{\mathrm{vol}}:: c^{\mathrm{ani}})I^{\mathrm{vol}}+\frac{1}{5}(I^{\mathrm{dev}}::c^{\mathrm{ani}})I^{\mathrm{dev}}$
        
    5. isotropic moduli로부터 Eshelby tensor 계산
        1. 아래 식으로부터 poisson ratio를 계산해서 사용
        2. 경모형은 그냥 stiffness tensor에서 바로 poisson ratio를 계산함
            
            $c^{iso}=3\kappa_{t}I^{\mathrm{{vol}}}+2\mu_{t}I^{\mathrm{{dev}}} $
            
            $\nu_{t}=\frac{3\kappa_{t}-2\mu_{t}}{2(3\kappa_{t}+\mu_{t})} $
            
    6. Doghri는 mid point rule을 이용해서 n+1/2 일 때의 moduli를 계산해서 사용했음.
        1. 이 alpha = 1/2, 2/3 일 때가 robust and accurate임을 언급
    7. 구한 Eshelby tensor를 이용해서 strain concentration tensor 계산
    8. Residual로 compatibility check
    
      
    
      
    
      
    

### Isotropization 코드 부분 ← 확인 완료. C_ani가 그냥 C_mat 쓰면 되는지만 확인

  

C_Id, C_Jd, C_Kd 3개의 6*6 2nd order tensor가 있고,

C_Id는 identity tensor

$I=\left[\begin{array}{c c c c c c c}{{1}}&{{0}}&{{0}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{1}}&{{0}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{1}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{1}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{1}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{0}}&{{1}}\end{array}\right] $

C_Jd는 3 * 3까지의 element가 1/3인 행렬, **Volumetric operator,** I^vol

$J=\left[\begin{array}{c c c c c c c}{{1/3}}&{{1/3}}&{{1/3}}&{{0}}&{{0}}&{{0}}\\ {{1/3}}&{{1/3}}&{{1/3}}&{{0}}&{{0}}&{{0}}\\ {{1/3}}&{{1/3}}&{{1/3}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{0}}&{{0}}\end{array}\right] $

C_Kd는 C_Id-C_Jd , **Deviatoric operator**, I^dev

$K=\left[\begin{array}{c c c c c c c}{{2/3}}&{{-1/3}}&{{-1/3}}&{{0}}&{{0}}&{{0}}\\ {{-1/3}}&{{2/3}}&{{-1/3}}&{{0}}&{{0}}&{{0}}\\ {{-1/3}}&{{-1/3}}&{{2/3}}&{{0}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{1}}&{{0}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{1}}&{{0}}\\ {{0}}&{{0}}&{{0}}&{{0}}&{{0}}&{{1}}\end{array}\right] $

  

C_J_iso, C_K_iso는 scalar값 **←이거 위에서 선언되지 않음**

C_J_iso = I^vol::C_mat

C_K_iso = I^dev::C_mat

아래 식과 관련된 계산으로 보임

$c^{i s o}\equiv(I^{\mathrm{vol}}:: c^{\mathrm{ani}})I^{\mathrm{vol}}+\frac{1}{5}(I^{\mathrm{dev}}::c^{\mathrm{ani}})I^{\mathrm{dev}} $

$c^{iso}=3\kappa_{t}I^{\mathrm{{vol}}}+2\mu_{t}I^{\mathrm{{dev}}} $

K_t = C_J_iso/3

MU_t = C_K_iso/10

V_mat 계산

$\nu_{t}=\frac{3\kappa_{t}-2\mu_{t}}{2(3\kappa_{t}+\mu_{t})} $

  

  

### Corrector function

1. 변수들 initialization
2. Yield function
3. Theta 계산 후 이를 고려한 XVAL
4. loop 시작
    1. residual 계산 → R norms
    2. D2YIELD로 ddfdsds 계산 → C bar 계산에 필요
    3. viscoplastic multiplier 계산을 위한 DFDS_C_DFDS 등 계산
    4. DELDEL_VPLAMULTI → DEL_VPLAMULTI update
    5. update stress properties
        1. trial stress update, 이를 위해 DEL_STRESS를 계산.. 계산 식은 확인 필요. ← 맞는 것 같음
            
            1. Del_T_Stress 를 계산하는데 원래 안썼던거 같긴 함. 여기서도 따로 쓰이진 않은 것으로 보임. 일단 삭제
            
            ![[Untitled 2 30.png|Untitled 2 30.png]]
            
            ![[Untitled 3 27.png|Untitled 3 27.png]]
            
        2. corrected plastic strain update
        3. DEL_E_STRAN ← **필요없을거같은데 코드 상에서 쓰이고 있음. 확인 필요**
        4. EPS update
        5. Backstress udpate (필요없음)
    6. 다시 yield check using updated T_STRESS, T_EVPS
        1. tolerance 이하로 떨어지지 않으면 다시 a로
        2. tolerance 이하면 바로 나가지 않고 DRDS를 계산함?
5. loop 바깥
    
    1. algorithmic stiffness 계산 (→ 여기에 DRDS를 사용하고 있음)  
        여기서 R은 residual이 아니라 Cm(C_bar) * C_mat임  
        
        ![[Untitled 4 21.png|Untitled 4 21.png]]
        
    
    ![[Untitled 5 18.png|Untitled 5 18.png]]
    

  

### Compute C bar matrix

다음 식을 계산 (Q*C_inv)inv

![[Untitled 6 16.png|Untitled 6 16.png]]