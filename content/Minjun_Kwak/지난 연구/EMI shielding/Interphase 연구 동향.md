## EMI shielding

conductivity 와 permittivity 겹치는 내용 같이 정리해도 좋을 것 같음

1. Conductivity
    1. interphase가 어떻게 영향을 미치는지
    2. interphase의 conductivity는 어떠한지
        1. probe 측정 방식은 전무
        2. model fitting 방식
        3. 사이값 가정
2. Permittivity
    1. interphase가 어떻게 영향을 미치는지
        1. MWS effect에 의해 interface 부근 거동 중요
        
3. EMI shielding 적용 사례
    1. 실험적 modification
    2. simulation
        1. model 적용한 effective medium
        2. 실제 model
            1. 거의 없음 → 파장과 object 사이의 관계성 떄문
            2. 대신 conductivity, permittivity 경우는 있음

  

수정 이후

1. EMI 관련 성질 중 interphase 영향 property

  

  

  

최종본 레퍼런스 확인

그림 설명

어느 쪽의 연구가 부족한지에 대한 고민

  

  

  

mechanical 관련 논문 정리

- interphase 관련
    
    mechanical 쪽에서 interphase의 영향을 다룬 논문은 주로 Carbon fiber나 glass fiber쪽에 있어서 nano inclusion 에 적용된 케이스가 많지 않았음. 그래서 carbon 계열 말고도 nanosize filler쪽으로 확대해서 찾아봄
    
    - Characterization of Polymer Nanocomposite Interphase and Its Impact on Mechanical Properties. Ciprari, D., Jacob, K. & Tannenbaum, R. _Macromolecules_ 39 6565–6573 (**2006**)
        
        → metal oxide와 polymer 간의 interphase로 인한 mechanical property를 다룬 논문
        
- IFSS(interfacial shear strength) 관련
    
    interphase와 관련 있을 수도 있고 없을 수도 있는데 일단 계면 특성이라 nano filler 케이스에 여러 논문이 많았음
    
    - A review of the interfacial characteristics of polymer nanocomposites containing carbon nanotubes. Chen, J., Liu, B., Gao, X. & Xu, D. _RSC Adv._ 8 28048–28085 (**2018**)
        
        → polymer-filler interaction 조절을 통해 물성 향상을 이뤄낸 케이스와, interaction 종류를 분광법으로 측정
        
    - Detachment of nanotubes from a polymer matrix. Cooper, C. A., Cohen, S. R., Barber, A. H. & Wagner, H. D. _Appl. Phys. Lett._ 81 3873–3875 (**2002**)  
        → SPM(scanning probe microscope) 데이터를 활용해 CNT를 pull out method를 쓰지 않고 IFSS 계산  
        
    - Determining the interphase thickness and properties in polymer matrix composites using phase imaging atomic force microscopy and nanoindentation. Downing, T. D., Kumar, R., Cross, W. M., Kjerengtroen, L. & Kellar, J. J. _Journal of Adhesion Science and Technology_ 14 1801–1812 (**2000**)  
        → nanoindentation과 afm을 활용해 기계적 물성 도출, 가장 대표적인 방법  
        
    - The effects of CNT waviness on interfacial stress transfer characteristics of CNT/polymer composites. Yazdchi, K. & Salehi, M. _Composites Part A: Applied Science and Manufacturing_ 42 1301–1309 (**2011**)  
        → CNT의 morphology가 변할 때 IFSS 영향  
        
    - Exploring the interface between single-walled carbon nanotubes and epoxy resin. Tsafack, T. _et al._ _Carbon_ 105 600–606 (**2016**)
        
        → 특이하게 CNT 도핑 및 defect 가 IFSS에 미치는 영향을 다룸
        
    - Pull-out simulations on interfacial properties of carbon nanotube-reinforced polymer nanocomposites. Li, Y. _et al._ _Computational Materials Science_ 50 1854–1860 (**2011**)  
        → CNT pull out을 MD 시뮬레이션해 IFSS 계산