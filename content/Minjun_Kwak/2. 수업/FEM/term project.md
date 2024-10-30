  

2022-21559 곽민준

# Problem 1.

  

## (1) 20 element, t=1일 때 각 방법에서의 결과

  

1. Newmark method
    
    ![[Untitled 22.png|Untitled 22.png]]
    
    ![[Untitled 1 10.png|Untitled 1 10.png]]
    
2. Central differential method without lumped mass matrix
    
    ![[Untitled 2 9.png|Untitled 2 9.png]]
    
    ![[Untitled 1 10.png|Untitled 1 10.png]]
    
3. Central differential method with lumped mass matrix
    
    ![[Untitled 3 8.png|Untitled 3 8.png]]
    
    ![[Untitled 4 7.png|Untitled 4 7.png]]
    
      
    
      
    
    두 방법을 사용했을 때 CDM의 경우 lumped mass matrix를 사용하지 않으면 Newmark 방식과 같은 결과를 보였고, analytical solution과 유사한 결과를 보였다. 한편 lumped mass matrix를 사용할 경우 다소 부정확한 결과를 보이는 것을 확인할 수 있었다.
    
      
    
    시간 증분은 0.0001으로 설정했다. 두 경우 모두 Courant-Friedrichs-Lewy (CFL) 조건을 만족해야 하므로, 아래와 같은 조건을 만족해야한다.
    
    $\Delta t <L/c$
    
    L은 가장 작은 요소의 길이이고,c는 파동 속도이다. 모든 요소가 1/(element 수)의 길이를 가지고, c=1으로 가정했으므로 1/20 이하의 시간 증분을 가져야 한다.
    
    CDM은 추가적으로 안정성을 보장하기 위해서 아래의 조건을 만족해야 한다.
    
    $\Delta t <2/\omega_{max}$
    
    해석을 진행했을 떄 omega의 최댓값은 7.8 정도의 값을 보였고, 따라서 해당 조건도 시간 증분을 0.0001으로 설정했을 때 무리 없이 만족했다.
    
      
    
    계산 시간의 경우 같은 결과를 보인 newmark과 CDM without lumped mass 기준으로, 시간 증분이 같은 경우 두 경우 모두 비슷하게 계산되어 8초대를 보였다. 그러나 newmark은 CDM과 다르게 implicit 방식이어서 시간 증분을 비교적 크게 설정할 수 있었고, 시간 증분을 0.1로 설정한 경우에도 유사한 결과를 보이는 한편 계산시간은 1/10으로 줄어든 0.8초를 보였다.
    
    ![[Untitled 5 6.png|Untitled 5 6.png]]
    
      
    
    ![[Untitled 6 6.png|Untitled 6 6.png]]
    
    한편 CDM의 경우 시간증분이 과하게 커지면 심하게 발산하여 거의 무의미한 결과가 나타났다.
    
      
    
    ## (2) CDM에서 element가 많을 때의 결과
    
    ![[Untitled 7 4.png|Untitled 7 4.png]]
    
    시간에 따른 결과
    
    ![[Untitled 8 4.png|Untitled 8 4.png]]
    
    element 수가 40일 때
    
    ![[Untitled 9 4.png|Untitled 9 4.png]]
    
    element 수가 80일 때
    
    ![[Untitled 10 3.png|Untitled 10 3.png]]
    
    element 수가 160일 때
    

비교는 time increment에 의한 정확도 감소의 영향을 없애기 위해 1/160 이하인 0.001으로 설정했다. element 수가 증가할 수록 정확도가 크게 상승하는 것을 확인할 수 있었으며, 20 element에서 관측되었던 파동이 지나간 후 잔파동이 남는듯한 현상은 나타나지 않았다.

  

  

# Problem 2.

![[Untitled 11 2.png|Untitled 11 2.png]]

![[Untitled 12 2.png|Untitled 12 2.png]]

10*10*2 element 케이스에서 두 경우에서의 element 배치 결과는 위와 같다.

  

![[Untitled 13 2.png|Untitled 13 2.png]]

analytic solution 결과는 위와 같다. 그러나 해당 결과를 얻기 위해 알아야 할 포아송비, 두께 등의 필수 조건을 알지 못해 완전히 똑같이 구현하지는 못했다.

  

  

![[Untitled 14 2.png|Untitled 14 2.png]]

![[Untitled 15 2.png|Untitled 15 2.png]]

![[Untitled 16 2.png|Untitled 16 2.png]]

![[Untitled 17 2.png|Untitled 17 2.png]]

![[Untitled 18 2.png|Untitled 18 2.png]]

![[Untitled 19 2.png|Untitled 19 2.png]]

시간 증분의 경우 CDM에서의 해석 안정성을 보장하기 위해 1번 문제에서 설명한 것과 같이 critical time increment와 CFL 조건을 만족할 수 있도록 설정하고자 했고, 0.01으로 설정하였다. 다만 element가 80*80*2인 케이스는 컴퓨터의 하드웨어가 허용가능한 계산 수준을 넘어서 계산이 어려웠다. 해석 결과를 보았을 때 대각선이 왼쪽 위에서 아래로 떨어지는 2번째 케이스가 더 정확한 결과를 얻었고, 이는 파동 진행 방향으로 element 밀도가 더 높기 때문이라고 생각된다. 또한 element간 boundary가 파동 방향으로 배열된 1번째 케이스는 boundary에서의 불안정성이 더 컸을 것이라고 생각된다.

  

# Problem 3.

![[Untitled 20 2.png|Untitled 20 2.png]]

![[Untitled 21 2.png|Untitled 21 2.png]]

위와 같이 가운데가 비어있는 plane에 대해 파동 전파 해석을 진행할 수 있었다. 시간 증분은 1/40보다 작은 0.01으로 설정해 CFL 조건을 충족하도록 설정하였다.

해석의 정확도를 높이기 위해선, mesh density를 증가시키고 가능하면 필요한 부분을 제외하면 삼각형보다 사각형 element를 사용하는 것이 유리할 것이다. 또한 time step size도 충분히 작게 설정하여 numerical stability를 충족시킬 수 있어야 한다. 다만 앞의 mesh density와 time size의 경우 accuracy와 efficiency 측면에서 trade off가 있기 때문에 적절한 조절이 필요하다. CDM의 경우 lumped mass matrix를 사용하면 해석의 efficiency가 크게 향상될 수 있다.