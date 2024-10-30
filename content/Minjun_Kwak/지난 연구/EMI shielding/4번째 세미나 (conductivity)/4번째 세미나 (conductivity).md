EMI 관련  
interphase와 관련하여 conductivity 부분 조사  

- interphase의 conductivity가 다르면 percolation theory 쪽에도 영향을 줄 것.  
    --> percolation theory의 formula 유도 등 살펴볼 것. interphase나 interfacial resistance를 고려한 case가 있는지  
    —> 찾아보니까 좀 있는 것 같음. 여기서는 interphase conductivity 달라지는 메커니즘 어떻게 설명하는지 조사 필요할 것 같음  
    

- 앞선 조사 내용에서는 ohmic contact 등에 의해 conductivity 차이 발생할 수 있었음 / 혹은 roughness의 영향 조사  
    --> 두께, conductivity drop, modeling case,  
    -앞선 내용은 case가 한정적. 더 확장된 case가 있는지?  
    -->  
    
- conductivity가 dielectric permittivity에도 영향을 줄 수 있을 것 같은데  
    --> 앞선 조사 내용에서는 particle이 아닌 polymer matrix쪽에서 영향을 받았지만, 이 경우라면 conductive particle쪽에서 interphase가 나타날 것. MWS effect를 생각해보면 interface쪽 전하 밀도가 달라지면 polarization 정도도 달라질 것으로 예상. 이건 시뮬레이션으로도 확인 가능할지도?  
    
- conductivity가 어떻게 달라지는지에 대한 추가 조사 필요성?
    
    - 어떤 경우, CNT와의 연결성 강화, defect 감소 등으로 인해 interphase 에서의 conductivity가 향상될 수 있음.
    - 혹은 insulating layer의 생성, defect의 존재로 인해 저하될 수 있음
    - 위의 변화는 여러 factor에 의해 나타나는 것으로 보임 —> 사용된 polymer의 종류, processing temperature.
    
      
    
- interphase에서 저항이 달라지는 방식이 단순히 interfacial resistance를 넣는 것과는 뭐가 다를까?
    - permittivity는 MWS effect가 관여하기 때문에 차이 있을것 같음
    - interphase를 적용하면 두께의 요소가 개입하게 됨
        - interphase저항+두께 / interfacial 저항 의 차이점?
            - CNT가 포함된 경우 CNT사이의 거리가 달라지는 효과 → penetration 차이 → conductive path가 달라질 가능성, percolation threshold에도 차이 발생할지도

  

이번 세미나에선 여기까지

- particle random distribution 조사  
    --> 실제 model 제작 위한 선행조사.  
    
- 조사된 interface 부근 property 차이가 실제로 어느정도 영향을 주는지  
    --> 선행연구나 계산된 property data를 비교했을 때 실제로 영향을 줄 수 있는 정도인지 판단 필요. 그렇지 않다면 interphase property 영향이 극대화된, nanoparticle case에서도 무의미한지 확인.  
    
- RVE 단계에서 interphase간의 중첩은 어떻게 표현할지

  

  

## 조사 정리

1. GEM model
    1. percolation theory와 effective medium theory를 결합한 형태
    2. pp와 cnf간 electrical contact을 무시, CNF끼리는 서로 contact 유지 가정
    3. particle geometry, aggregate, interphase등은 고려하지 않음
    
    5. interphase를 고려해 modified의 경우 더 실험값과 잘 맞는 결과
        1. interphase conductivity와 interaction 정도에 차이가 발생
        2. interphase 물성은 frequency에 영향을 받음 → hopping mechanism이 영향을 받기 때문
2. permittivity and conductivity
    1. 상호 연관성
        
        ![[Untitled 13.png|Untitled 13.png]]
        
        ![[Untitled 1 3.png|Untitled 1 3.png]]
        

  

## 내용 구성

1. 지난 세미나 내용
    1. EMI shielding에 conductivity와 permittivity가 중요
    2. 그 중에서 permittivity를 조사했고, interphase 두께는 AFM, permittivity는 curve fitting등으로 얻을 수 있음
    3. 코멘트 없었음
2. 이번 세미나 내용
    1. conductivity
    2. RVE 구성 이론적 내용
3. Conductivity of interphase
    1. interphase의 conductivity는 달라지는지?
    2. 어떻게 구할건지
        1. 우선 constant가 일반적이라는 레퍼런스
        2. permittivity때와 같은 전략으로 접근
            1. GEM modified model
                1. 얻은 interphase 물성은 matrix와 다름
        3. AC conductivity의 경우 frequency에 따라 달라진다는 보고
            1. 이건 제대로 조사 안되면 우선 내용에서 제거
    3. 어떻게 영향이 있을지
        1. percolation 에서 tunneling이 일어나기 위해 energy barrier를 지나야 하는데, 이 energy barrier가 달라지게 되면 exponential probablity도 달라지게 될 것
        2. 저번 permittivity 때도 그렇고, interphase 에서 property가 달라진다는 보고가 있지만 이 interphase property 변화가 EMI shielding에 영향을 얼마나 주는지에 대해선 알려져 있지 않아 일단 이걸 연구해보는 것도 괜찮을 것 같음
    
4. RVE construction
    1. 전략 설명으로만 마무리
        1. matlab 연동 with comsol
        2. pseudorandom number generator
        3. pbc
        4. size of RVE
            1. test를 통해 통계적 오차가 적은 크기
5. Further works
    
    1. RVE 구성
    2. 적용할만한 system 조사
    3. 이미 구축된 model 기반으로 신뢰도 평가?