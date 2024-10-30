  

## Memory caching

2개의 policy를 따라야 함.

1. spacial locality of data
    1. data access 시 메모리 근처에 있는 메모리가 다음에 사용되어야 함.
    2. UMAT에서는 double loop 에서, array의 경우 왼쪽 숫자가 먼저 증가하는 방향으로 저장됨  
        → inner loop이 왼쪽 dimension쪽에 적용되어야함.  
        
        ![[Untitled 73.png|Untitled 73.png]]
        
    3. dimension 선언 시 implicit 선언은 피해야 함. ←이건 안쓰고 있음.
2. temporal locality of data
    1. memory area에 엑세스 후, 다음 insturciton에서 다시 사용되기 쉬어야 함. 따라서 cache에 오래 남겨두고, 안쓸건 drop해야함.
    2. 이를 위해서, complier가 코드 분석 시 loop의 길이를 파악하고 cache에 저장하고 사용할 수 있도록 loop의 길이를 hard coding해야 함. array syntax를 쓰지 말 것.
    3. 아래와 같은 경우 Z가 A에서 쓰이고 마지막 줄에서 다시 쓰이는데, 이 경우에도 array syntax를 쓰지 말아야함.
        
        ![[Untitled 1 37.png|Untitled 1 37.png]]
        

  

## array initialization or copy

1. 필요한 경우가 아니면 initialization을 뺄 것.  
    → 불필요한 메모리 접근을 없애고, 캐시 미스가 발생할 가능성을 줄임. (앞에서 말한 것 처럼 캐시 용량이 작아서 생길 수 있는 문제를 최소화)  
    1. 아래의 경우 ZX는 initialization이 필요없고, ZY는 필요함.
        
        ![[Untitled 2 35.png|Untitled 2 35.png]]
        

  

## Vectorization

![[Untitled 3 32.png|Untitled 3 32.png]]

![[Untitled 4 25.png|Untitled 4 25.png]]

1. 코드의 루프 등에서 반복적으로 수행되는 동일한 작업을 병렬로 처리하는 방식으로, CPU의 벡터 연산 기능을 활용해 여러 데이터를 동시에 처리.
2. compiler가 vectorization을 할 수 있도록 inhibitor를 제거하고 enhancer를 넣는게 좋다.

  

  

  

  

참고 글

[https://stackoverflow.com/questions/15580572/fortran-matrix-multiplication-performance-in-different-optimization](https://stackoverflow.com/questions/15580572/fortran-matrix-multiplication-performance-in-different-optimization)