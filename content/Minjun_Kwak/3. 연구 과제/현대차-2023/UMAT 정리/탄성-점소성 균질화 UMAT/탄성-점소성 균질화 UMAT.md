  

## 교수님 지시사항

- 점소성 코드를 탄소성 균질화 UMAT에 implementation
    - 테스트 완료
    - FVF 0에서 ddsdde로 했을 때와 trial stress로 했을 때 둘이 같은 결과
- 점소성에서 DDSDDE로 stiff하게 나온 부분을 time increment를 작게 해서 테스트
    - 완전히는 아니지만 어느 정도 비슷하게 나옴
    - 점소성이 increment size에 더 크게 영향을 받는 것 같음.
    
- 점소성 코드에서 ETA를 eps로 두었을 때 소성으로 적용 가능한지 확인  
    →이게 되면 점소성 균질화 코드로 소성 테스트 진행  
    - 계산은 abort 안뜸. 특이하게 DDSDDE 결과와 trial stress 결과가 유사하게 나옴.
- 도그리 교수의 점소성관련 논문 확인
    - dynamic yield function 자체의 한계점일 수 있다.
- stress, strain을 state variable에 넣을 경우엔 항상 ROTSIG를 사용할 수 있도록 할 것
- Drucker-Prager 2nd derivative 식 직접 유도해볼 것
- 탄소성 균질화 UMAT에서 Aspect ratio 반영해서 테스트
    - 우선 점소성 배제

  

## 240131 세미나

- isotropization scheme 다시 확인.
- VEVPT 균질화에서도 S22, S33 확인 가능한지?
- DDSDDE 큰 차이 없다고 했는데 fiber/matrix 분리해서도 digimat에서 확인 가능한지
- fiber orientation transformation에 대해서 더 알아볼 것
    - plastic 변형에 의한 방향 문제?
    - digimat 상에서 rotation과 관련된 내용을 확인 가능한지
    - 에셸비 텐서도 추출 가능한가?
- initialization 안해서 나타나는 문제 있는지 체크.
- d time 줄이고 continuum으로 넣어도 trial이랑 잘 맞는지
    - C_bar를 elastic moduli로 변경 후 stress update scheme 바꿔서 테스트
- DDSDDE의 확인 방법
    - Doghri의 DDSDDE와 general form 차이 있는지 확인

  

  

## 240207, 08 미팅

- tensor 미분을 하는 경우 basis가 변경된 형태이기 때문에, shear 성분에 대해 미분을 하는 경우 basis인 sqrt(2)가 분모로 감  
    → Mandel이 적용된 미분!  
    
- E-VP 코드 검증은 homogenization을 사용한 numerical 계산 논문을 찾아서 비교, digimat도 같이
- 인공신경망 another material case는 transfer learning으로 가능성을 보여줌.
- PBC 제대로 적용된건지 확인 필요
- DP에서 C_bar 확인 필요
- 점탄성 적용 시 E_bar를 E 대신 사용

  

  

## 240214 미팅

- von mises 점소성 논문 찾아서 검증

  

  

## 코드 확인 필요 사항

- 탄소성 균질화 tensile에서 volume fraction이 들어갔을 때 제대로 나오는지 확인
- 탄소성 균질화에서 AR을 2-5정도로 넣었을 때 digimat이랑 비교
- 1/3 같은 코드 구문 있는지 확인

  

## 코드 상으로 발견된 문제

- aspect ratio를 25로 설정 시 matrix의 trial stress가 감소하는 문제
    - matrix의 DSTRAN0, T_EPS는 정상적으로 증가함
        - y, z 방향 DSTRAN이 비정상적으로 크게 감소
    - matrix의 stress가 x, y, z 모든 방향 axial 방향으로 감소하고, inclusion의 경우 반대로 증가함.
        - plastic 시작 부터 문제 발생
    - time increment size의 문제는 아닌 것으로 확인됨.
    - shear 방향 성분이 해석 끝 부분에서 나타나고, DFDS 계산이 틀어짐

  

### AR=1일 때

total strain

![[Untitled 71.png|Untitled 71.png]]

total stress

![[Untitled 1 35.png|Untitled 1 35.png]]

fiber stress

![[Untitled 2 33.png|Untitled 2 33.png]]

matrix stress

![[Untitled 3 30.png|Untitled 3 30.png]]

fiber strain

![[Untitled 4 23.png|Untitled 4 23.png]]

matrix strain

![[Untitled 5 20.png|Untitled 5 20.png]]

  

  

### Aspect ratio 25 일 때

total strain

![[Untitled 6 18.png|Untitled 6 18.png]]

  

total stress

![[Untitled 7 14.png|Untitled 7 14.png]]

fiber stress

![[Untitled 8 11.png|Untitled 8 11.png]]

matrix stress

![[Untitled 9 11.png|Untitled 9 11.png]]

![[Untitled 10 10.png|Untitled 10 10.png]]

fiber strain

![[Untitled 11 8.png|Untitled 11 8.png]]

matrix strain

![[Untitled 12 8.png|Untitled 12 8.png]]

  

  

### Eshelby tensor

![[Untitled 13 7.png|Untitled 13 7.png]]

![[Untitled 14 7.png|Untitled 14 7.png]]

  

## Doghri’s VPT model

  

2011년 Miled 논문 기반, rate-dependent J2 점소성 모델 사용

  

### Viscoplastic system

- J2 viscoplastic with isotropic hardening
- associated flow rule
- viscoplastic function을 도입해 점소성 사용, Theta와 형태 동일함.

  

> **Darabi 논문**
> 
> ![[Untitled 15 7.png|Untitled 15 7.png]]
> 
> ![[Untitled 16 7.png|Untitled 16 7.png]]
> 
> ![[Untitled 17 6.png|Untitled 17 6.png]]
> 
> ![[Untitled 18 6.png|Untitled 18 6.png]]
> 
>   
> 
> **Miled 논문**
> 
> ![[Untitled 19 6.png|Untitled 19 6.png]]
> 
> P는 viscoplastic effective strain을 말하는 것 같음.
> 
> ![[Untitled 20 6.png|Untitled 20 6.png]]
> 
> Norton’s power law로 정의됨.
> 
>   
> 
>   
> 
>   

  

  

## 균질화 수정 관련

  

### 코드 상황

1. 균질화 없이, FVF=0으로 설정 시 점소성에 대한 물성은 제대로 나옴
2. 점소성 파라미터를 0,1로 두고 탄소성 균질화 테스트 시 spectral isotropization 도입으로 aspect ratio에 관계없이 잘 나옴
3. 점소성 균질화 모델 적용 시 aspect ratio에 관계없이(1,25) 결과가 stiff하게 나옴. 이건 spectral 적용 시에도 그러함.
4. 위와는 별개로 균질화가 제대로 들어가더라도, 중간에 abort가 발생하거나 불안정한 해석이 발생함. ← trigger point가 어디에서 나타나는지 파악하지 못했음.

  

![[Untitled 21 6.png|Untitled 21 6.png]]

### 확인 필요한 사항

1. 점소성 모델이 spectral isotropization과는 잘 맞지 않거나
    1. digimat mf manual에는 J2 viscoplasticity에 spectral method를 사용 가능하다고 써있지만
    2. 사용된 식의 경우 J2 plasticity model에 대해서만 나와있음. .
        1. Theta term 추가
2. 점소성에서의 DDSDDE가 균질화 모델과 맞지 않거나.

  

  

## Miled 학위논문 isotropization 내용 정리

DDSDDE를 regularization 이후 isotropization으로 이어짐

  

### Regularization