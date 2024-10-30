  

### Crisfield 책

Recent work has seen increasing use of the backward-Euler scheme without sub-incrementation (Sections 6.6.6 and 6.6.7). This method is popular because, for the von Mises yield criterion, it takes a particularly simple form and, in addition, it allows the generation of a ‘consistent tangent modular matrix’ (Section 6.7) which ensures quadratic convergence (see Section 1.2.3) for the overall structural iterations when the full Newton-Raphson method is adopted. 167 페이지

![[Untitled 67.png|Untitled 67.png]]

  

  

### Simo

![[Untitled 1 31.png|Untitled 1 31.png]]

→ 전산소성 역학에서도 마찬가지 notation

  

  

### Ju

![[Untitled 2 29.png|Untitled 2 29.png]]

  

  

### Alfano

![[Untitled 3 26.png|Untitled 3 26.png]]

![[Untitled 4 20.png|Untitled 4 20.png]]

  

  

### Miled

![[Untitled 5 17.png|Untitled 5 17.png]]

![[Untitled 6 15.png|Untitled 6 15.png]]

![[Untitled 7 12.png|Untitled 7 12.png]]

  

  

레포트

  

1. 지난 DDSDDE 변화의 경우 큰 차이 없음 ㅎ
2. Alfano 논문 Dynamic yield function
    
    1. f=0 대신 theta를 사용하라고 되어 있음. Phi의 경우 linear/nonlinear가 소개되어 있으나 제욱이형이 사용한 것과 같은게 없어 찾아봄 → Darabi 논문
        
        ![[Untitled 8 9.png|Untitled 8 9.png]]
        
        ![[Untitled 9 9.png|Untitled 9 9.png]]
        
        1. 다라비
            
            ![[Untitled 10 8.png|Untitled 10 8.png]]
            
            ![[Untitled 11 7.png|Untitled 11 7.png]]
            
            ![[Untitled 12 7.png|Untitled 12 7.png]]
            
        2. dfds와 dgds
    2. stress update 방식
        1. trial stress에서 구함
    3. DDSDDE 공부 내용