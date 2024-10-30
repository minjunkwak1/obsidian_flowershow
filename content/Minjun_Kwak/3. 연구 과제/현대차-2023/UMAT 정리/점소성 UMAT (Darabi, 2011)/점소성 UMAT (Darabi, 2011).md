  

우선 Von Mises에 대해서 점소성 구축한 뒤 Drucker-Prager로 변경

→ yield, D2yield subroutine을 바꿔서 적용 가능하도록 이 외의 연산은 subroutine바깥에서 이루어짐.

yield subroutine에서 FVAL은 theta 없이 뱉고, subroutine 밖에서 FVAL에 theta를 빼서 yield 판단하도록.

→ plastic corrector 부분만 변경하면 됨.

  

### Dynamic yield function 추가에 따라 변화하는 부분

- C_bar tensor

  

### 추가 파라미터

- Corrector function
    - THETA_VP
        - DTHDVP
    - ETA
    - VN
        - VNINV
    - DVPDS(6,6)
    - DTIME
        - initial yield Norton law 적용 필요
    - 기타 plastic 관련 parameter들 이름 변경

  

### subroutine 변경

1. **plastic corrector**
    1. Deldel_vpmultiplier
    2. DDSDDE
    3. 위의 두 값에 대해 다음이 필요함.
        1. 분모에 DTH_DVP 가 더해져야 함.
            
            ![[Untitled 70.png|Untitled 70.png]]
            
            ![[Untitled 1 34.png|Untitled 1 34.png]]
            
        2. C_bar 내부 식 변형
            1. 기존 식 (DRDS = C_bar)
                
                ![[Untitled 2 32.png|Untitled 2 32.png]]
                
            2. dynamic yield function 적용
            3. DRDS의 계산에서 viscplastic multiplier가 f의 함수이고, f는 sigma의 함수이기 때문에 이를 고려해야함. (아래 ddfdds 식은 drucker-prager 기준)
                
                ![[Untitled 3 29.png|Untitled 3 29.png]]
                
                ![[Untitled 4 22.png|Untitled 4 22.png]]
                
                ![[Untitled 5 19.png|Untitled 5 19.png]]
                
                ![[Untitled 6 17.png|Untitled 6 17.png]]
                
        3. 이 때 C_bar의 계산이 effective stress 종류와 yield function 형태에 영향을 받게 됨  
            → Drucker-Prager와 dynamic yield function에 대해 별도의 C_bar 계산 subroutine 작성  
            
        4. 다음의 새 parameter가 필요함.
            1. DVP_DS (6) ← dgds dfds가 cross product임. 이건 ddfdds랑 구분 필요
                
                ![[Untitled 7 13.png|Untitled 7 13.png]]
                
            2. DFDS
2. **Yield function**
    
    1. theta 추가
        
        ![[Untitled 8 10.png|Untitled 8 10.png]]
        
    
      
    
      
    
      
    

### Digimat 비교 조건

**조건 1.(Alfano 논문)**

elastoviscoplastic model  
J2 plasticity  

E : 2.1 * 10^5 MPa,  
Poisson's ratio = 0.3,  
yield stress = 240MPa  

viscoplastic exponent = 2Mpa  
viscoplastic coefficient (η) =2000 MPa s  
strain rate 1  

perfect viscoplastic  
(no hardening, hardening exponent =0.001)  

  

![[Untitled 9 10.png|Untitled 9 10.png]]

  

**조건2. (작년도 현대차 과제 viscoplastic parameter에서 hardening 차용)**

elastoviscoplastic model  
J2 plasticity  

E = 2.1 * 10^5 MPa,  
Poisson's ratio = 0.3,  
yield stress = 10Mpa  
viscoplastic coefficient (η) =5489MPa s  
strain rate 동일하게  
viscoplastic exponenent (m)=1.2  
hardening coefficient(A)=88.79 MPa  
hardening exponent(B)=0.4792  

![[Untitled 10 9.png|Untitled 10 9.png]]

  

  

### Corrector 계산 순서 확인 필요

1. Yield function → DFDS 계산
2. residual 계산
    1. 순서 맞나? 뒤에서 계산되어야 맞는 것 같음.
    2. 다만 초반에 residual은 계산되어야 multiplier update가 가능함.
    3. 근데 일반적으로 f와 r이 같이 norm 이하로 떨어지기 때문에 상관없을 것 같음.
3. DDFDDS 계산
4. DelDel_Vplamulti 계산 → multiplier update
5. stress, vp strain, effective vp strain update
6. DDSDDE update
7. yield check → loop 빠질지 확인

  

  

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