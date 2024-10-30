## prime objective

EMI shielding simulation 을 위한 RVE 구성

  

## main step

1. 자료 조사
    1. conductivity와 permittivity의 interphase 영향 가능성 확인
    2. **EMI shielding에 영향을 주는가?**
2. interphase 물성 조사
    1. permittivity —> checked(2212 세미나)
        1. interphase에 영향을 줄 수 있는 요인은 매우 다양함 —> 몇 가지 parameter 대입으로 얻기 어려움
        2. 선행 연구에서는, 유한요소해석을 이용한 fitting, fitting by trial and error 등의 방식. interphase 두께는 STM을 이용해 측정했음
    2. conductivity
        
3. RVE 구성
    
    1. 작은 RVE를 위한 boundary condition
        1. random distribution을 제대로 표현하지 않으면 오차가 클 수 밖에 없음. 특히 conductivity는 network 형성에 따라 크게 달라지게 됨
        2. RVE 시뮬레이션의 장점을 부각하기 위해, 이미 이론이 어느정도 정립된 CNT 외에도 다른 particle이나 hybrid composite에 대해서 고려해볼 수 있을 것 같음
            1. 3개 이상의 component가 혼합된 시스템
        3. 현대차 보고서에 CFRP의 랜덤배향이 구현된 케이스가 있는데 어떻게 구현된건지? DIGIMAT을 쓰는 것도 고려해보면 좋을듯
    
      
    
    1. CNT의 random distribution 방법
4. interphase를 반영한 각 물성 시뮬레이션
    1. conductivity
    2. permittivity
5. 물성을 반영한 EMI shielding simulation

  

## obstacles

1. interphase가 중요하려면, nanoscale filler여야 하는데, 이 경우 RVE 구성 방식이 매우 비효율적임
2. 작은 RVE의 경우 도파관 형성을 통한 시뮬레이션 자체가 어려울 수 있음
3. interphase 두께 알아내기 위한 STM 사용 어려움
4. interphase로 인한 작은 차이가 다른 오차 원인에 비해 유의미한 정도인지 잘 모르겠음

  

## Ideas

- 머신러닝 활용
    - 훈련 데이터 어디서 얻을지? 몇 만개 단위의 dataset이 필요할 것으로 예상됨
        - 개수보다는, data가 전체 시스템을 잘 표현하는지가 중요한 부분.
        - 각 case에서의 변수가 어떻게 달라지는지를 먼저 파악한 뒤에 (소성에서 본 stress mode, hardening rate 외에도 점탄성의 경우 달라지는 부분이 있는지 확인) dataset 범위를 정해야 할 것 같음
- RVE simulation
    - 방법은 google scholar 통해서 좀 더 찾아볼 것
    - multiple reflection같은데 영향을 줄 것 같음.
- RVE 시뮬레이션으로 interphase 물성을 알아낸 뒤, 이를 조성이 다르거나/형태가 다르거나/hybrid인 경우 등에 적용하기
- small RVE 시뮬레이션 결과를 통해 전체 물성을 반영할 수 있는 메커니즘