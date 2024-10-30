1. **INPUT**
    1. PROPS를 통해 parameter 입력  
          
        
2. **온도 영향**
    1. 열팽창
    2. 점탄성 및 점소성 온도 관련 파라미터 추출  
          
        
3. **O2D 파일 read**
    1. GETOUTDIR 호출
        1. GETOUTDIR( OUTDIR, LENOUTDIR )
        2. write the path to the input file (the .odb is stored in the same directory as the .inp) in the user-provided string OUTDIR, up to LENOUTDIR characters (the length of the string).
    2. OT.txt open
        1. 섬유 배향 정보 파일
        2. o2d / xml에 따라 각기 다른 방식으로 읽게 됨 (PROPS(2)에서 N_FILE_FORMAT으로 지정, 시뮬레이션에서 o2d 데이터를 사용할 경우 PROPS(2)에 1을, xml 데이터를 사용할 경우 PROPS(2)에 2를 입력)
        3. Ori_OT11,22,12,13,23,33 값이 입력됨  
            → 이후 Ori_OT (3,3 대칭행렬) & Vec_OT (1,6 벡터)로 reshaped  
              
            
4. **orientation tensor 후처리  
      
    **3개의 주응력 값으로 되어있어야 인공신경망에 넣을 수 있다.
    
    1. SPRIND 호출
        1. 주응력 값과 direction of a tensor 반환
        2. SPRIND(Vec_OT, PRI_VAL, DIR_COS, 1, NDI, NSHR)
            1. Vec_OT는 NDI의 direct component 와 NSR의 shear component로 구성
            2. PRI_VAL은 principal value
            3. DIR_COS는 direction cosines of the principal direction
            4. 1은 Vec_OT가 stress로 구성됨을 표시 (2면 strain)
    2. CROSS 호출 **==← 이거 왜하는건지는 잘 모르겠음. Sprind에서 출력되는 direction이 부정확한가?==**
        1. CROSS PRODUCT
            1. CROSS(DIR_COS(:,1), DIR_COS(:,2), DIR_COS(:,3), OK_FLAG)
            2. DIR_COS(:,3) (dimension 3,1)의 값 변경됨
    3. Principal value 로 이루어진 대각행렬 생성, a11>a22>a33으로 되도록 정렬
    
    ![[Untitled 62.png|Untitled 62.png]]
    
      
    
5. **Pseudo-grain decomposition**
    1. 5개 인공신경망 함수 호출
        1. 유사결정립 분해에 Kmeans 방법을 직접적으로 쓰지 않고 미리 학습된 인공신경망을 활용함
        2. CALL FCT_NN_OT2AB(PRI_VAL, OT2AB, OK_FLAG)  
            CALL FCT_NN_AB2PG1(OT2AB, AB2PG1, OK_FLAG)  
            CALL FCT_NN_AB2PG2(OT2AB, AB2PG2, OK_FLAG)  
            CALL FCT_NN_AB2PG3(OT2AB, AB2PG3, OK_FLAG)  
            CALL FCT_NN_AB2W(OT2AB, AB2W, OK_FLAG)  
              
            
            ![[Untitled 1 26.png|Untitled 1 26.png]]
            
        3. OT2AB는 섬유 배향 텐서의 세개의 주 성분으로부터 2변수-Bingham 분포함수의 두 파라미터인 α, β를 구하는 모델
        4. (AB2W)는 α, β로부터 3개의 유사결정립 각각의 부피 분율을 계산하는 모델
        5. (AB2PG1, AB2PG2 및 AB2PG3)은 α, β로부터 3개의 유사결정립의 국소 주축 방향을 계산하는 모델
    2. 유사결정립 4~12의 정보는 유사결정립 1~3을 x축 대칭, y축 대칭, 그리고 원점 대칭을 시켜 얻을 수 있음.
        1. 각 유사결정립의 각도(θ, φ)는 2*12 크기의 PG_angle에 저장됨
        2. 각 유사결정립의 weight는 PG_weight에 저장됨. 이 때 AB2W로 나온 weight을 1/4로 균등분배하여 들어감.
        3. 부피분율은 PG가 12개이므로 계산된 AB2W 값을 4로 나누어 1,4,7,10번/… PG에 적용
6. **Eshelby tensor calculation**
    1. 모리 타나카 homogenization 적용을 위해 계산
        
        ![[Untitled 2 24.png|Untitled 2 24.png]]
        
    2.   
          
        
7. **Pseudograin rotation tensor**
    1. FCT_SPH2CART 호출
        1. spherical coordinate에서 cartesian coordinate로 PG angle(각 pseudo grain 주축 방향(Φ,θ))을 변환시킴  
            → 3*12 크기의 Basis1 array에 벡터형태로 저장됨  
            
        2. 이걸 pseudograin principal 방향 1번째로 사용하는 것 같음  
            아래의 형태로 각 PG에 대해 적용됨  
            
            ![[Untitled 3 21.png|Untitled 3 21.png]]
            
    2. FCT_NN_ORTHO 호출
        1. **왜 쓰는건지는 모르겠는데 basis2의 선정에서 임의의 방향을 쓰지 않고 인공신경망을 통해 방향을 결정함**
    3. CROSS 호출
        1. basis3을 basis2와 basis1의 외적을 통해 결정
    4. BASIS_PG(3*3*12 크기)에 각 basis data를 저장함
8. **1st step homogenization**
    1. Fiber stiffness matrix
        1. 아래의 행렬을 만듦, 섬유는 탄성만 있음 가정함
            
            ![[Untitled 4 17.png|Untitled 4 17.png]]
            
            ![[Untitled 5 15.png|Untitled 5 15.png]]
            
    2. Matrix elastic stiffness
        
        1. 열효과 반영된 elastic modulus 계산
        2. 강성텐서 구축
        
        1. FCT_M66INV 호출 **←역행렬 만드는 routine인듯?  
              
            **C_MATinf 의 역행렬으로 S_MATinf 생성
    3. Matrix viscoelastic stiffness
        1. Viscoplastic의 경우 추후 iteration을 통해 계산함
    4. PG Grain matrix stiffness matrix  
          
        
9. **Viscoplastic stiffness**
    1. 각 PG에 대해서 계산
        1. 해당 PG의 **==state variable==** 수 계산
        2. 해당 PG의 property 호출
            1. basis 호출
        3. inelastic viscoelastic dstran0 계산
        4. strain concentration tensor (T?) for MT method 계산
            1. 문헌 보니 matrix 변형률과 fiber 변형률의 관계를 연결지어주는 것 같음
        5. delta strain을 global axis에서 PG axis로 변환
            1. global axis to principal axis
            2. principal axis to PG axis
        6. MT model에 기반해 섬유 및 수지 delta strain으로 분리
            1. **==DSTRAN0, DSTRAN1 이 섬유/수지 데이터 같은데 각각 어디에 해당하는지 잘 모르겠음. 이후 코드 진행 보면 0이 matrix인듯? → 이게 맞다 함==**
                
                ![[Untitled 6 13.png|Untitled 6 13.png]]
                
10. **INNER LOOP (1~100 loop)**
    
    ![[Untitled 7 10.png|Untitled 7 10.png]]
    
    1. viscoelastic assumption
        1. trial stress 계산
            1. FCT_Edotv 호출  
                1~NTENS에 대해 DSTRESS0=C_MATinf*DSTRAN0 → state variable에 저장된 ㄴtress에 더해 새로운  
                **STRESS0**(PG axis에서의 stress) 생성. trial stress인듯
                
                $\begin{aligned}$
                
            2. Heredity integrals (h_j) ← 점탄성에 대한 term인듯  
                여기서 j는 각 maxwell model component를 의미  
                
                $\begin{aligned}$
                
                1. STATEV(N_STV_PG+50+6*K1+K2) 는 h_j^n을 말하는 것 같음
                2. gamma는 E_j/E_inf
                3. 각 maxwell component에 대해 계산하여 N,NTENS dimension의 DI matrix 구성
            3. NTENS개의 각 DSTRESS0에 대해 계산 후 summation
        2. Stress 계산
            1. 1~NTENS에 대해 T_STRESS(K1)=STRESS0(K1)+DISUM(K1)으로 update
                
                $\begin{aligned}& \sigma^{n+1}=\sigma_0^{n+1}+\sum_{j=1}^N h_j^{n+1}\end{aligned}$
                
        3. Yield
            1. 이론적 내용====
                
                $f=(\bar\sigma-X)-\sigma_y^0-\kappa(\varepsilon^{vp}_{eq})$
                
                1. yield function이 위와 같이 구성됨
                2. sigma bar는 effective stress (Drucker Prager)  
                      
                    
                    ![[Untitled 8 8.png|Untitled 8 8.png]]
                    
                    $\tau=\frac{\sqrt{J_2}}{2} [(1+\frac{1}{K})+(1-\frac{1}{K})\frac{J_3}{\sqrt{J^3_2}}]\\ \bar \sigma$
                    
                3. X는 kinematic hardening term (Chaboche)
                    
                    1. Chaboche kinematic model 사용
                    2. viscoplastic multiplier increment(gamma), yield function(f), stress state(sigma_ij)에 따라 변화
                    
                    $\dot X=[a(\frac{\partial f}{\partial \sigma_{ij}})-bX]\dot\gamma$
                    
                4. 위의 gamma는 Perzyna flow rule에 기반하여 나옴
                    1. Perzyna flow rule은 고분자 수지의 속도 효과 반영
                    2. viscoplastic multiplier increment가 viscoplastic multiplier와 viscoplastic exponent(N)에 의해 계산됨
                        
                        ![[Untitled 9 8.png|Untitled 9 8.png]]
                        
                5. Kappa는 isotropic hardening term
                    1. effective plastic strain이 증가함에 따라 변화하는 effective stress 거동 묘사
                    2. 여기선 단순 power law를 사용함
                        
                        ![[Untitled 10 7.png|Untitled 10 7.png]]
                        
                6. 최종적인 dynamic yield function은 아래와 같음
                    
                    ![[Untitled 11 6.png|Untitled 11 6.png]]
                    
            2. FCT_YIELD 호출 → ==**delta yield function/delta stress 계산**==
                1. hydrostatic stress 계산
                    1. SMEAN(K1)=(T_STRESS(1)+T_STRESS(2)+T_STRESS(3))/3  
                        NDI까지, shear의 경우 0  
                        즉, 3개의 같은 값과 3개의 0으로 구성  
                        
                    2. TSTRESS와 SMEANS 둘 다 NTENS 개의 data 가짐
                2. deviatoric stress 계산
                    1. SDEV(K1)=T_STRESS(K1)-SMEAN(K1)-**==BSTRESS==**(K1)
                        
                        ![[Untitled 12 6.png|Untitled 12 6.png]]
                        
                3. invariants 계산
                    1. I1, J2, J3 계산  
                        Drucker-Prager effective stress에서 이 세 invariants가 필요함.  
                        
                        ![[Untitled 13 6.png|Untitled 13 6.png]]
                        
                    2. CI1=T_STRESS(1)+T_STRESS(2)+T_STRESS(3)  
                        평균값으로 쓰진 않고있음  
                        
                    3. CJ2=1.5*(SDEV(1)**2+SDEV(2)**2+SDEV(3)**2+2_SDEV(4)**2 +2_SDEV(5)**2+2*SDEV(6)**2)  
                        계수값이 알려진 값과 다른 것은 SHEAR 성분에 대해서 식이 아바쿠스에서 쓰는 형태가 다른가봄  
                        
                    4. CJ3=4.5*(SDEV(1)_(SDEV(1)**2+SDEV(4)**2+SDEV(5)**2)  
                        +SDEV(2)  
                        _(SDEV(4)**2+SDEV(2)**2+SDEV(6)**2)  
                        +SDEV(3)  
                        _(SDEV(5)**2+SDEV(6)**2+SDEV(3)**2)  
                        +2  
                        _SDEV(4)*(SDEV(1)*SDEV(4)+SDEV(2)_SDEV(4)+SDEV(5)SDEV(6))  
                        +2SDEV(5)  
                        _(SDEV(1)*SDEV(5)+SDEV(4)_SDEV(6)+SDEV(3)SDEV(5))  
                        +2SDEV(6)  
                        _(SDEV(4)*SDEV(5)+SDEV(2)*SDEV(6)+SDEV(3)*SDEV(6)))  
                        =det(dev)  
                          
                        **stress의 순서가 아바쿠스에서는 알려진 형태와 다름**
                        
                        ![[Untitled 14 6.png|Untitled 14 6.png]]
                        
                        ![[Untitled 15 6.png|Untitled 15 6.png]]
                        
                4. DELTA J2/DELTA STRESS
                    1. DJ2DS(i)=3*SDEV(i)
                5. DELTA J3/DELTA STRESS
                    1. DJ3DS(1)=27/2*(SDEV(1)*SDEV(1)+SDEV(4)*SDEV(4)+SDEV(5)_SDEV(5))  
                        -3  
                        _CJ2
                6. Effective shear stress
                    1. g값(Drucker-Prager effective stress ?) 및 Tau 값 계산  
                          
                        ***코드에서는 (beta*I1)/3 으로 3으로 나눈 값이 사용됨***
                        
                        ![[Untitled 8 8.png|Untitled 8 8.png]]
                        
                    2. f값 계산
                        1. 수렴 확인 위한 f값이며, yield function에 구하고자 하는 sigma 값 넣었을 때 0이 되어야 함  
                            * 여기서의 yield function은 dynamic이 아닌 yield function  
                            
                            $f= \tau +\beta I_1 -\kappa (\varepsilon^vp_e)=0$
                            
                        2. UMAT 상으로는 식이 좀 다른데 왜 그런지는 확인 필요  
                            (DK는 Drucker-Prager effective stress 파라미터 (K))  
                            
                            $f= \tau +\beta I_1 -(\frac{1}{d}+\frac{\beta}{3})(\frac{\sigma_0}{EPS }+\kappa (\varepsilon^vp_e))\\$ 
                            *d는 \ k와 \ 같음
                            
                    3. Hp값 계산
                        1. hardening function 미분값인것 같은데 나중에 viscoplastic multiplier 2계미분의 계산에서 사용됨
                7. Delta yield Function/Delta STRESS
                    1. yield function에서의 normal 방향
                        
                        $\tau=\frac{\sqrt{J_2}}{2}[(1+\frac{1}{d})+(1-\frac{1}{d}) \frac{J_3}{\sqrt{J_2^3}}]\$ 
                        
                    2. 왜 I1에 대해서는 stress 미분 term이 없는지 잘 모르겠다. J는 deviatoric stress invariant고 I1은 stress에 대한 invariant라 그런가?
                    3. ——FCT_YIELD 종료——
            3. ~~FVAL~~ XVAL값을 tolerance와 비교하여 elastic 구간일 경우 loop 종료(9000)  
                아닐 경우 다음 줄 plastic corrector로 이동  
                
        4. Plastic corrector (FCT_VP_CORRECT)
            1. FCT_VP_CORRECT 호출
                1. input값으로, stress, effective viscoplastic strain, viscoplastic strain, bstress
            2. FCT_YIELD 호출
                1. F값 도출
            3. Theta값 계산
                1. THETAVP=YIELD_0*(DEL_VPLAMULTI*ETA/VPDTIME)**(VNINV)
                    
                    $\Theta(\gamma) =\sigma^0_y(\frac{\dot\gamma \ \eta}{\Gamma})^{1/N}$
                    
                2. XVAL (= FVAL-THETAVP) 계산
                    
                    1. 이게 consistency condition으로 맞춰진 dynamic yield function인 듯
                    
                    $X=f-\Theta$
                    
                    $X(\boldsymbol \sigma_C)=0 \ 일 때 \ correction\ 종료\\ X= \tau +\beta I_1 -\kappa (\varepsilon^vp_e)-\sigma^0_y(\frac{\dot\gamma}{\Gamma})^{1/N}=0$
                    
            4. NR plastic corrector  
                maximum iteration을 1000000으로 설정  
                
                1. Residual 계산
                    1. 1,NTENS에 대해서 r vector 계산
                        
                        1. RESIDUAL(K1)=-C_VP_STRAIN(K1)+P_VP_STRAIN(K1) +DEL_VPLAMULTI*DFDS(K1)
                        2. P_VP_STRAIN이 correction 지점에서의 값, C_VP_STRAIN이 trial 지점 값인거 같은데 왜 strain이지?
                            
                            1. 이건 뒤의 multiplier쪽에서 C 안붙은거 보니까 residual도 **strain으로 계산**하는가봄.
                            
                            1. 이후 RNORM 계산 후 RTOL과 비교하게 됨.
                            
                        
                        $\boldsymbol\sigma_C=\boldsymbol\sigma_B-\Delta\lambda \boldsymbol C\boldsymbol a_C \\ \\\Rightarrow \boldsymbol r=\boldsymbol\sigma_C-(\boldsymbol\sigma_B-\Delta\lambda \boldsymbol C\boldsymbol a_C ) =0\\ $
                        
                
                1. RNORMS 계산
                    1. r벡터 크기, 이후 tolerance와 비교
                2. DFDS:DFDS 계산
                3. DFDSxDFDS 계산
                4. DDFDDS (2nd derivative) 계산
                    1. FCT_D2YIELD 호출
                    2. 아래 식 값
                        
                        ![[Untitled 16 6.png|Untitled 16 6.png]]
                        
                    3. g와 f
                        1. 이 모델에선 non associated flow rule을 써서 g와 f가 다르다.(즉, yield surface와 potential surface가 다르고, stress 증가 방향과 plastic strain 증가 방향이 서로 다르다.)
                            
                            ![[Untitled 17 5.png|Untitled 17 5.png]]
                            
                        2. 따라서 일반적인 flow rule 에서 사용하는 f값을 f(=g if associated flow rule)가 아닌 g에 대해 사용하는 것으로 보임
                            
                            ![[Untitled 18 5.png|Untitled 18 5.png]]
                            
                            ![[Untitled 19 5.png|Untitled 19 5.png]]
                            
                5. DEL_DVPDS 계산
                    1. 뭔지 잘 모르겠다. dr/ds 계산에 사용됨
                    2. 아래 식 값
                        
                        ![[Untitled 20 5.png|Untitled 20 5.png]]
                        
                    3. 여기서 viscoplastic effective strain increment값은 perzyna flow rule에 의한 것.
                        
                        $\dot \varepsilon _{ij} ^{vp,t}=\dot\gamma \frac{\partial f}{\partial \sigma_{ij}} \leftarrow perzyna \ flow \ rule \\ \Rightarrow \Delta \bar\varepsilon_{ij}^{vp}=\frac{\partial g}{\partial \bar \sigma_{ij}} \Delta \bar \gamma ^{vp,t}$
                        
                6. dr/ds (delta residual/delta stress)계산
                    1. drds=VEMINV+DEL_DVPDS
                        
                        ![[Untitled 21 5.png|Untitled 21 5.png]]
                        
                    2. perzyna flow rule과 R을 strain으로 생각해보면 이 계산이 맞음. (a_c=dgds)
                7. second derivative of VP multiplier 계산
                    
                    1. DRDS inv 계산 후
                    2. DTHDVP를 미리 계산한 후 DELDEL_VPLAMULTI 계산
                        1. DTHDVP=(YIELD_0)/(VN*DEL_VPLAMULTI)*(DEL_VPLAMULTI*ETA/VPDTIME)**(VNINV)
                        2. DTHDVP는 theta를 viscoplastic multiplier에 대해 미분한 항
                    3. DELDEL_VPLAMULTI=(XVAL-C_DFDS_R)/(DFDS_C_DFDS+Hp+DTHDVP)  
                          
                        
                    
                    ![[Untitled 22 4.png|Untitled 22 4.png]]
                    
                    ![[Untitled 23 4.png|Untitled 23 4.png]]
                    
                8. Update
                    1. DEL_VPLAMULTI=DEL_VPLAMULTI+DELDEL_VPLAMULTI
    
      
    
      
    
      
    
      
    
      
    
      
    
    DEL_EVPS=DEL_VPLAMULTI*(1/DK+BETA/3)/(1-XX)
    
    C_VP_STRAIN(K1)=C_VP_STRAIN(K1)+DEL_VP_STRAN(K1)