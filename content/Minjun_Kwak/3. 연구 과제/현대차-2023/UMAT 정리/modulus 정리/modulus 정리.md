![[Untitled 64.png|Untitled 64.png]]

![[Untitled 1 28.png|Untitled 1 28.png]]

![[Untitled 2 26.png|Untitled 2 26.png]]

여기서 말하는 E0가 matrix 형태 stiffness인 C_inf를 말하는 것 같음. 왼쪽이 heredity integral sum이 더해진 VEM(viscoelastic stiffness matrix)

  

![[Untitled 4 17.png|Untitled 4 17.png]]

![[Untitled 5 15.png|Untitled 5 15.png]]

이게 C_inf / 여기서 mu는 shear modulus와 같다.

## 코드 내부

$E_\mu =E_{ratio}*E_{mat} \\ E_\infty=E_0-\Sigma E_\mu $

  

sum of Emu

STATEV(1) = T_EVPS;  
STATEV(2) = C_VP_STRAIN(1);  
STATEV(3) = C_VP_STRAIN(2);  
STATEV(4) = C_VP_STRAIN(3);  
STATEV(5) = C_VP_STRAIN(4);  
STATEV(6) = C_VP_STRAIN(5);  
STATEV(7) = C_VP_STRAIN(6);  
STATEV(8) = BSTRESS(1);  
STATEV(9) = BSTRESS(2);  
STATEV(10) = BSTRESS(3);  
STATEV(11) = BSTRESS(4);  
STATEV(12) = BSTRESS(5);  
STATEV(13) = BSTRESS(6);  

```Plain
STATEV(15) = STRESS0(1);
STATEV(16) = STRESS0(2);
STATEV(17) = STRESS0(3);
STATEV(18) = STRESS0(4);
STATEV(19) = STRESS0(5);
STATEV(20) = STRESS0(6);

for K1 = 1:NTENS
    STATEV(20+K1) = T_STRESS(K1);
end

for K1 = 1:NTENS
    STATEV(26+K1) = STRAN0(K1);
end

for K1 = 1:N
    for K2 = 1:NTENS
        STATEV(50+6*K1+K2) = DI(K2, K1);
    end
end

STATEV(6*N+57) = DEL_VPLAMULTI;
```

## 흐름

물성 입력 → elastic, viscoelastic stiffness 계산 → trial stress → yield 판단 후 소성보정

  

## 확인 필요사항

TRIAL stress 계산할 때 EPS 사용했는지

![[Untitled 3 23.png|Untitled 3 23.png]]

  

소성 보정 후 DDSDDE

  

vpcorrect 부터

  

viscoplastic multiplier increment 계산 시 값에 차이 있음.(vpcorrect 112)

gamma 계산 시 E_MAT0 쓰지 않고 inf값 사용 (main, 99,140). 이후에도 E0가 맞을거같은데 inf로 사용함.

초기 elastic 추측 시 yield 판단에서 x값이 아닌 f값을 사용함.

deldel_multiplier 계산 시 dxdvp 이상함

FCT yield에서 yield function 다시 확인. 논문이랑 다름

DJ2DS, DJ3DS → D2yield와 yield에서 다름