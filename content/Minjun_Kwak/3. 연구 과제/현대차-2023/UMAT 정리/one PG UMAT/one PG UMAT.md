## 1ST STEP HOMOGENIZATION - COMMON PROPERTIES

### STIFFNESS MATRIX

Fiber의 elastic stiffness와 matrix의 viscoelastic stiffness 계산

C_FIB와 VEM

  

### DSTRAN OF PG MATRIX (DSTRAN0)

C_MAT=VEM

DSTRAN_PG2=DSTRAN

  

### INELASTIC VISCOELASTIC DSTRAN0 (PG AXIS)

viscoelastic inelastic term계산(DSTRAN_IN,DSTRAN_IN_VE) ← 지워야함.

  

  

  

## Dstran0 계산

  

CMAT = VEM

DSTRAN_PG2=DSTRAN (←~~**이거 위에서 따로 정의 안돼있음**~~ **유맷 인풋값.**)

  

### C INELASTIC VISCOELASTIC DSTRAN0 (PG AXIS)

TERM2 → PPT 상에서 점탄성 inelastic strain

![[Untitled 65.png|Untitled 65.png]]

DSTRAN_IN → VEMINV와 곱한 뒤 DTHERM을 더함..?

→ DSTRAN_IN_VE에 따로 저장해둠

  

### STRAIN CONCENTRATION TENSOR

STRAN_CON=T_TEMP= B tensor 0

Rev_T_TOTAL = A tensor 0

**근데 여기서 사용된 stiffness matrix는 점탄성 only임**

  

### DSTRAN1 (PG axis)  
MORI-TANAKA  

V_TEMP = fiber dstran

V_TEMP2 = 제욱이형이 정의한 inelastic term ← 점탄성만 들어감

  

  

  

  

  

  

  

  

  

  

## 수정이 필요한 부분

1. initializing 에서 DSTRAN0,1 계산
2. residual에서 DSTRAN 0,1 계산
3. 1st step homogenization
    
    1. DSTRAN1 계산에서 V_TEMP2
    
      
    
      
    

## 수정 로그

1. C INELASTIC VISCOELASTIC DSTRAN0 (PG AXIS), 삭제 611-655
    1. DSTRAN0= DSTRAN_PG2
2. elastic이어서 MT verification 코드 들어가지 않은 경우에도 homogenized stiffness를 계산할 수 있도록 goto 9000 이전에 B concentration tensor와 matrix의 A concentration tensor를 계산하는 코드를 추가(766줄)
    1. 이후 stress update에도 a_eps가 필요하기 때문에, 이 부분도 같이 추가함
3. MT verification 이전 inelastic term 계산부분 삭제 811-823
4. MT verification 부분 residual 수정
    
    ![[Untitled 1 29.png|Untitled 1 29.png]]
    
    ![[Untitled 2 27.png|Untitled 2 27.png]]
    
    1. 기존 코드 식
        
        ![[Untitled 3 24.png|Untitled 3 24.png]]
        
    2. A tensor 계산까지는 그대로 쓰이므로 유지함.
    3. **BETA를 새롭게 정의할 필요**
        
        ![[Untitled 4 19.png|Untitled 4 19.png]]
        
        1. beta_1과 beta_0이 필요 ←**위쪽에 dimension 정의 필요**
        2. ALPHA가 1개밖에 들어가있지 않으므로 우선 ALPHA를 matrix와 fiber에 따로 변수 정의하지 않고, 그냥 beta_1을 0으로 놓음.
        3. DTHERM 은 1*6 vector
            1. alpha, DTEMP는 scalar
            2. DTHERM=ALPHA로 만든 vector ←-179-190번째 줄에 있는 코드 수정
                1. DTHERM0과 DTHERM1로 분리해서 변수 변경 및 선언
                2. alpha도 마찬가지로 0과 1로 분리했으며, 우선 PROPS에서는 alpha0만 입력할 수 있도록 그대로 둠. 추후 섬유 팽창률을 사용하고자 할 경우 input props 변경 필요
            3. alpha를 새롭게 tensor 형태로 만들 필요가 있음. 혹은 DTHERM 계산할 때 tensor 형태로 변환
                1. 우선 기존 열팽창을 고려한 것과 마찬가지로 11,22,33방향에 대해서만 isotropic expansion을 고려함.
    4. **a_eps 를 새롭게 정의**
        
        ![[Untitled 5 16.png|Untitled 5 16.png]]
        
5. 루프 바깥쪽 수정
    
    1. 878번쨰 줄 DSTRESS0(K1)=DSTRESS0(K1)+DEL_T_STRESS(K1)/(TERM1+1) ← Trial stress에 대한 보정이 필요한지 잘 모르겠음. 버전 나눠서 테스트 해봐야 할지도?
    2. 9000 이후에 homogenized stiffness tensor를 계산하는 코드 추가(1087줄)
        
        ![[Untitled 6 14.png|Untitled 6 14.png]]
        
    3. STRESS0가 점탄성이 고려되지 않은 stress로 되어 있는데 최종 결과에는 영향을 주지 않아서 우선 그대로 두었음. state variable에는 저장되어서 다음 STEP에서 “탄성”에 대한 STRESS로 사용됨. 점탄성이 아님
    4. loop 바깥쪽 DSTRAN fiber와 matrix로 분리하는 mori tanaka 부분 삭제 (953-998줄)
    5. 1045번줄 FRP_TERM2 부분 삭제
    6. **stress update에 사용할 beta_bar 정의.** 아래 식에선 thermoelastic case여서 E로 elastic modulus가 주어졌지만 우리 케이스에선 matrix와 fiber의 stiffness를 사용해야함. 코드 상단에도 dimension을 정의해두었음
        
        ![[Untitled 7 11.png|Untitled 7 11.png]]
        
    
      
    
6. 변수명 수정
    1. REV_T_TOTAL → A_CON_m (A matrix strain concentration tensor)
    2. T_TEMP → B_CONCEN (B strain concentration tensor)
        1. UMAT 앞에 **B_CONCEN을 T_TEMP와 똑같이 차원 정의**
        2. B tensor로 T_TEMP가 사용된 경우만 수정함
7. RNORMS2 초기화 할 때 실수형을 사용하도록 지정.