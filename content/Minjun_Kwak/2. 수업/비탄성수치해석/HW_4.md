
## Stress_Integration
### 필요한 parameter

[del_eps, del_epsilon_pl, sigma_new,iter]=Stress_Integration(E, del_epsilon, eps_old, sigma_old, yieldf)
input으로 E, del epsilon, 이전 step의 strain, 이전 step의 yield stress, yield furnction을 받음. 
output으로 effective plastic strain 증분(del_eps),  plastic strain 증분(del_epsilon_pl), updated stress, iteration 횟수

### input parameter
- del_epsilon
	- Nelem x Nload 사이즈로 저장 필요하며, input으로 사용 시 1개씩 call 
	- ~~del sigma 계산 후 역산~~
		- ~~sigma=F/A = E ε~~
		- ~~Δε=ΔF /(AE)~~
	- equilibrium iteration에 의해 sU가 update되므로, 이를 이용해 계산
- eps_old
	- Nelem x Nload 사이즈로 저장 필요하며, input으로 사용 시 1개씩 call 
- sigma_old
	- Nelem x Nload 사이즈로 저장 필요하며, input으로 사용 시 1개씩 call 

## 코드 구성

1. load의 경우 15+15+20+20=70 step 필요
2. stress-strain 관계 변경되었으므로 K, P 다시 계산





