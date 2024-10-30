---
tags: []
parent: 'Machine learning-based constitutive model for J2- plasticity'
collections:
    - 'machine learning'
$version: 37329
$libraryID: 1
$itemKey: 5IBDKEWR

---
In this study, the computing time using the ANN was up to 50% lower, than in the case of the FEM simulation. However, the accuracy of the ANN depends strongly on the trained data. Interpolations between provided training data could be a reason between differences of the ANN and FEM solutions
viscoplastic쪽
<span class="highlight" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22pageLabel%22%3A%222%22%2C%22position%22%3A%7B%22pageIndex%22%3A1%2C%22rects%22%3A%5B%5B54.594%2C376.967%2C505.238%2C386.447%5D%2C%5B39.005%2C366.479%2C426.939%2C375.959%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%222%22%7D%7D" ztype="zhighlight"><a href="zotero://open-pdf/library/items/CN4SF6GA?page=2">“developed model replaces the nonlinear plastic correction procedure for the plastic loading path during the stress integration method while keeping the elastic linear and loading and unloading with a conventional physics-based scheme.”</a></span> <span class="citation" data-citation="%7B%22citationItems%22%3A%5B%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%222%22%7D%5D%2C%22properties%22%3A%7B%7D%7D" ztype="zcitation">(<span class="citation-item"><a href="zotero://select/library/items/XB8S2BEL">Jang 등, 2021, p. 2</a></span>)</span>
## 2.An artificial neural network-based constitutive model
## <span class="highlight" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22pageLabel%22%3A%222%22%2C%22position%22%3A%7B%22pageIndex%22%3A1%2C%22rects%22%3A%5B%5B39.005%2C94.469%2C216.206%2C103.949%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%222%22%7D%7D" ztype="zhighlight"><a href="zotero://open-pdf/library/items/CN4SF6GA?page=2">“2.1.Internal mechanism of artificial neural network”</a></span>
information은 nodal value의 weight 과 bias를 이용한 선형 조합으로 전달됨
delivery 이후 node는 ReLU로 activated
ANN 순서
![\<img alt="" data-attachment-key="RN7BG9VM" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22annotationKey%22%3A%22WUPWD6ZM%22%2C%22color%22%3A%22%23ffd400%22%2C%22pageLabel%22%3A%223%22%2C%22position%22%3A%7B%22pageIndex%22%3A2%2C%22rects%22%3A%5B%5B145.5%2C72.552%2C405.375%2C449.427%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%223%22%7D%7D" width="433" height="628" src="attachments/RN7BG9VM.png" ztype="zimage">](attachments/RN7BG9VM.png)\
<span class="citation" data-citation="%7B%22citationItems%22%3A%5B%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%223%22%7D%5D%2C%22properties%22%3A%7B%7D%7D" ztype="zcitation">(<span class="citation-item"><a href="zotero://select/library/items/XB8S2BEL">Jang 등, 2021, p. 3</a></span>)</span>
1.  input variable의 normalization --> input layer vector는 아래와 같게 됨\
    $I = [ x _ { 1 } , x _ { 2 } , \cdots , x _ { N ( i ) } ] ^ { T }$
2.  1st hidden layer는 weighted sum 을 받게 되고, ReLU로 activated.\
    activated nodal value H는 다음과 같음\
    $H _ { i } ^ { ( 1 ) } = f ( \sum _ { j = 1 } ^ { N ( i ) } W _ { i j } ^ { ( 1 ) } x _ { j } + b _ { i } ^ { ( 1 ) } )$, 여기서 f가 ReLU, W는 weight, b는 bias
3.  이 과정을 반복함. 이전 node에서의 nodal value를 weight를 곱해 sum을 받은 뒤 해당 node에서의 bias를 더함\
    $H _ { i } ^ { ( M ) } = R e l u \sum _ { j = 1 } ^ { N ( M-1 ) } W _ { ij } ( M ) H _ { j } ( M - 1 ) + b _ { i } ^ { ( M ) }$
4.  마지막 Mth hidden layer에서 output variable 도출\
    $y _ { i } = \sum _ { j = 1 } ^ { N ( M ) } W ^{(out)}_ { i j } H _ { j } ^{( M )} + b _ { i } ^ { ( out) }$
cost function을 줄이는 방법으로 ADAM optimizer가 사용되었고, cost function으로 MSE가 적용됨 -> MSE를 최소화하도록 trained, MSE 가 50 consecutive epoch 이상에서도 변화가 없으면 training stop
### 2.2.Solution algorithm of the artificial neural network-based constitutive model
Consistency condition을 활용해 given loading path가 plastic evolution을 야기하는지 확인함
$R=\bar{\sigma}\left(\sigma^{T R}\right)-\rho\left(\bar{\varepsilon}^{p, n}\right)$
R은 residual for consistency condition , $\rho$ 는 corresponding flow stress, $\bar\sigma$  는 equivalent stress
\--> 우리 과제에서는 \*\*<u>$\rho$ 는 Θ와 Κ 로 사용되었음, </u>\*\*원래는 hardening에 의한 term을 flow stress로 사용하는 것 같은데, 우리껀 여기에 dynamic term이 추가된 형태임.
이 값이 0 이하면, updated stress는 trial stress와 같음 -> plastic strain is not accumulated, plastic correction 없음
이 값이 0보다 크면, plastic corrector step이 필요함.
기존 방식에서 iterative scheme으로 NR method를 많이 사용함. -> plastic multiplier $\Delta \gamma$  를 계산하여 updated plane stress tensor$\boldsymbol{\sigma}^{n+1}$ 를 구함
$\boldsymbol{\sigma}^{n+1}=\boldsymbol{\sigma}^{T R}-\mathbf{C}^e \Delta \gamma \mathbf{P} \boldsymbol{\sigma}^{n+1}$
C는 plane stress elasticity matrix, $P = \frac { 1 } { 3 } \left[ \begin{array} { l l l } { 2 } & { - 1 } & { 0 } \\ { - 1 } & { 2 } & { 0 } \\ { 0 } & { 0 } & { 6 } \end{array} \right]$
$\boldsymbol s = \boldsymbol{P\sigma}$
deviatoric stress로 매핑하는 텐서, 왜 식을 이렇게 썼는지 모르겠음
deviatoric stress가 아니라 normal vector가 들어가야할 것 같은데
여기에 NN 구성방정식을 적용할경우
1.  trial stress를 spectral decomposition
2.  ANN으로 updated principal stress 도출
3.  effective plastic strain update
    1.  consistency 가 유지된다고 생각하여,  ![\<img alt="" data-attachment-key="YLZRT8PS" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22annotationKey%22%3A%22N2K6JAMY%22%2C%22color%22%3A%22%23ffd400%22%2C%22pageLabel%22%3A%225%22%2C%22position%22%3A%7B%22pageIndex%22%3A4%2C%22rects%22%3A%5B%5B54%2C532.302%2C134.25%2C550.302%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%225%22%7D%7D" width="134" height="30" src="attachments/YLZRT8PS.png" ztype="zimage">](attachments/YLZRT8PS.png)
    2.  $\bar{\varepsilon}^{p, n+1}$
          의 경우 따라서 $\bar{\varepsilon}^{p, n+1}= ( \frac { \rho } { K } ) ^ { \frac { 1 } { n } } - \varepsilon _ { 0 }$으로 계산됨\
        이 때 K, n, epsilon\_0는 constants of Swift law\
        \--> **effective plastic strain이 hardening law에 기반하여 계산됨**$\rho=K (\bar\varepsilon^{p,n+1}+\varepsilon_0)^n$\
        \
        ★ 정리하면, updated stress로 yield stress를 계산한 뒤($\bar\sigma(\boldsymbol\sigma^{n+1})$) consistency 조건을 가정해 이 값이 flow stress($\rho$)와 같다고 생각함\
        \--> 이걸로 effective plastic strain update값을 계산함
4.  material coordinate에 맞춰 rotation transformation\
    $\sigma^{n+1}=\sum_{A=1}^2 \sigma_A^{n+1} n^A \otimes n^A=\sum_{i=1}^2 \sum_{j=1}^2 \sigma_{i j}^{n+1} e^i \otimes e^j$
## <span class="highlight" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22pageLabel%22%3A%225%22%2C%22position%22%3A%7B%22pageIndex%22%3A4%2C%22rects%22%3A%5B%5B39.009%2C398.961%2C284.804%2C408.441%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%225%22%7D%7D" ztype="zhighlight"><a href="zotero://open-pdf/library/items/CN4SF6GA?page=5">“3.Training data and neural architecture with one element FEM”</a></span>
### 3.1.Review of plane stress return mapping method
$ \dot{\boldsymbol{\varepsilon}}=\dot{\boldsymbol{\varepsilon}}^e+\dot{\boldsymbol{\varepsilon}}^\rho \\$  - strain의 elastic part와 plastic part 분리
$\boldsymbol{\sigma}=\mathbf{C}^e \boldsymbol{\varepsilon}^e \\$  - elastic relationship
$ f=\frac{1}{2} \boldsymbol{\sigma}^T \mathbf{P} \boldsymbol{\sigma}-\frac{1}{3} \rho^2\left(\bar{\varepsilon}^p\right)$ 
*   consitency condition, 앞에서 R값
$\dot{\boldsymbol{\varepsilon}}^p=\dot{\gamma} \mathbf{P} \boldsymbol{\sigma}$
$ \dot{\varepsilon}^p=\dot{\gamma} \sqrt{\frac{2}{3} \boldsymbol{\sigma}^T \mathbf{P} \boldsymbol{\sigma}}$
리턴 매핑에서 일반적인 elastic predictor framework을 고려했을 때 elastic predictor state 는 다음과 같이 계산됨
$ \varepsilon^{n+1}=\varepsilon^n+\Delta \varepsilon \\$  strain을 일정 분율 증가시켜 예측값
$\boldsymbol{\sigma}^{n+1}=\mathbf{C}^e\left(\varepsilon^{n+1}-\varepsilon^p\right) \\$  해당 예측 strain을 이용해 stress 계산, elastic part만 사용하기 위해 plastic strain 빼주는 것 같음
$ f^{T R}=\frac{1}{2}\left(\boldsymbol{\sigma}^{T R}\right)^T \mathbf{P} \boldsymbol{\sigma}^{T R}-\frac{1}{3} \rho^2\left(\bar{\varepsilon}^{p, T R}\right)$
여기서 consistency condition이 0 이하면 process는 elastic
그러나 0보다 클 경우, plastic return mapping을 거치고, backward-Euler difference scheme (아래 식 풀어서)사용
$\boldsymbol\varepsilon^{p, n+1}=\boldsymbol{\varepsilon}^{p, n}+\Delta \gamma \mathbf{P} \boldsymbol{\sigma}^{n+1} \\$
$ \bar{\varepsilon}^{p, n+1}=\bar{\varepsilon}^{p, n}+\Delta \gamma \sqrt{\frac{2}{3}\left(\boldsymbol{\sigma}^{n+1}\right)^T \mathbf{P} \boldsymbol{\sigma}^{n+1}} \\$
$ \frac{1}{2}\left(\boldsymbol{\sigma}^{n+1}\right)^T \mathbf{P} \boldsymbol{\sigma}^{n+1}-\frac{1}{3} \rho^2\left(\bar{\varepsilon}^{p, n+1}\right)=0$
여기서 $\Delta \gamma$ 은 plastic Lagrange multiplier.
isotropic linear elasticity를 가정해서 계산되며 Cauchy stress vector는 다음과 같음
$ \boldsymbol{\sigma}^{n+1}=\boldsymbol{\sigma}^n+\mathbf{C}^e \Delta \boldsymbol{\varepsilon}^e \\$
        $=\boldsymbol{\sigma}^n+\mathbf{C}^e\left(\Delta \boldsymbol{\varepsilon}-\Delta \boldsymbol{\varepsilon}^p\right) \\$
$\because \boldsymbol{\sigma}^{T R}=\boldsymbol{\sigma}^n+\mathbf{C}^e \Delta \boldsymbol{\varepsilon} \\$
$\boldsymbol{\sigma}^{n+1}=\boldsymbol{\sigma}^{T R}-\mathbf{C}^e \Delta \gamma \mathbf{P} \boldsymbol{\sigma}^{n+1}$
$\Delta \gamma$ 은 consistency condition이 0이 되도록 NR을 통해 결정됨
$f(\Delta \gamma):=\frac{1}{2} \left(\sigma^{n+1}\right)^T \mathbf{P} \sigma^{n+1}(\Delta \gamma)-\frac{1}{3} \rho^2\left(\bar{\varepsilon}^{p, n}+\Delta \gamma \sqrt{\frac{2}{3} \left(\sigma^{n+1}\right)^T \mathbf{P} \sigma^{n+1}(\Delta \gamma)}\right)=0$
이 경우 plane stress를 가정했으므로 z축 strain은 이렇게 결정됨
$\varepsilon_{33}=-\frac{\nu}{E}\left(\sigma_{x x}+\sigma_{y y}\right)-\left(\varepsilon_{x x}^p+\varepsilon_{y y}^p\right)$
elastoplastic tangent modulus는 아래와 같이 정의됨 --> consistent modulus?
$d \boldsymbol{\sigma}^{n+1}=\mathbf{C}^{e p} d \varepsilon^{n+1}$
$\mathbf{C}^{e p}=\overline{\mathbf{C}}-\frac{\overline{\mathbf{C}} \mathbf{m}_{n+1} \otimes \overline{\mathbf{C}} \mathbf{m}_{n+1}}{\mathbf{m}_{n+1} \overline{\mathbf{C}} \mathbf{m}_{n+1}+h^{\prime}}$
$\overline{\mathbf{C}}=\left[\mathbf{C}^{e^{-1}}+\gamma \frac{\partial \mathbf{m}_{(n+1)}}{\partial \sigma_{(n+1)}}\right]^{-1}, \mathbf{m}=\frac{\partial \bar{\sigma}}{\partial \boldsymbol{\sigma}}, \mathbf{C}^e=\frac{E}{1-\nu^2}\left[\begin{array}{lll} 1 & \nu & 0 \\\nu & 1 & 0 \\0 & 0 & \frac{1-\nu}{2}\end{array}\right]$  
여기에 NN 구성방정식을 적용할경우, trial stress를 input으로 사용하기 전 회전변환을 통해 principal trial stress로 변환 후 input으로 사용함.
$[\sigma^{TR}_{xx},\sigma^{TR}_{yy},\sigma^{TR}_{xy}]^T \Rightarrow [\sigma^{TR}_{1},\sigma^{TR}_{2}]^T$
이후 출력된 n+1 step에서의 principal stress는 다시 xx, xy, yy에 대해 역회전변환이 이루어짐. (θ값이 회전변환 시 기억되어야함)
### 3.2.An investigation into a suitable training dataset
훈련 데이터는 기존의 theoretical return mapping scheme에 기반하여 numerically 생성됨
2개의 trial stress는 prescribed current flow stress와 2 updated stress를 활용하여 다음 식에 의해 계산됨
위에선 trial stress를 계산하고 principal stress로 변환한다고 했는데 여기선 반대로 기술하고 있음..
$\left[ \begin{array} { l } { \sigma _ { 1 } ^ { TR } } \\ { \sigma _ { 2 } ^ { TR } } \end{array} \right] = \left[ \begin{array} { l } { \sigma _ { 1 } } \\ { \sigma _ { 2 } } \end{array} \right]$  + $\Delta \gamma \frac { E } { 1 - v ^ { 2 } } \left[ \begin{array} { l } { 1 } & { v } \\ { v } & { 1 } \end{array} \right] \left[ \begin{array} { l } { ( 2 \sigma _ { 1 } -\sigma _ { 2 } ) / 2\bar{\sigma} } \\ { ( 2\sigma _ { 2 } - \sigma _ { 1 } ) / 2\bar{\sigma} } \end{array} \right]$  
training data size는 2개의 design variable에 의해 결정됨
1.  **density of data on hardening curve** --> input variable인 flow stress와 관련됨flow stress 상에서 effective plastic strain 으로 결정되는 flow stress값
2.  **density of data on the yield surface at the given hardening** --> output variable인 sigma 1,2와 관련됨\
    symmetry를 고려해서 범위 선정\
    density를 결정해야함.
equivalent plastic strain은 non-uniform으로 형성, grid interval이 결정되어야 density가 결정됨\
$( \bar{\varepsilon} _ { n + 1 } ^ { p } - \bar{\varepsilon}^{ p } _{ n } )$ 이 non uniform이며, n은 경화곡선 상 datapoint 순서를 나타냄\
Swift law를 이용한 경화곡선은 다음과 같이 나타남
$\rho (\bar{\varepsilon}^ { - p , n + 1 } ) = K ^ { * } ( {\varepsilon} _ { o } + { \bar{\varepsilon} } ^ { p , n + 1 } ) ^ { n } [ M P a ]$
여기서 K, ${\varepsilon}_0$ , n은 materials flow stress curve에 기반하여 계산됨..? 실험 결과 피팅한다는 말인듯\
maximum value 0.1로 해서 n개의 data point 설정, 밀도 order 다르게 설정해서 10 epoch 훈련 후 최대오차 비교\
\--> set2로, point 200개 사용함.
추후에 실제 dataset 사용할 때 는 maximum value 0.6으로 사용했음 (최종 point 수 1200개)
![\<img alt="" data-attachment-key="MUB83VUM" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22annotationKey%22%3A%22LEZ5U8YC%22%2C%22color%22%3A%22%23ffd400%22%2C%22pageLabel%22%3A%229%22%2C%22position%22%3A%7B%22pageIndex%22%3A8%2C%22rects%22%3A%5B%5B97.917%2C543.51%2C447.083%2C692.677%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%7D" width="582" height="249" src="attachments/MUB83VUM.png" ztype="zimage">](attachments/MUB83VUM.png)\
<span class="citation" data-citation="%7B%22citationItems%22%3A%5B%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%5D%2C%22properties%22%3A%7B%7D%7D" ztype="zcitation">(<span class="citation-item"><a href="zotero://select/library/items/XB8S2BEL">Jang 등, 2021, p. 9</a></span>)</span>
각각 2output sig1, sig2에 대해 MSE, set1의 경우가 overfitting 없음을 확인.
2nd data도 마찬가지로 200 epoch 훈련 진행 후 overfit 현상 확인함. -> set2가 overfitting없어서 사용함.
\--> dataset 개수 180개
![\<img alt="" data-attachment-key="5P74TJKU" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22annotationKey%22%3A%22HYJS639Y%22%2C%22color%22%3A%22%23ffd400%22%2C%22pageLabel%22%3A%229%22%2C%22position%22%3A%7B%22pageIndex%22%3A8%2C%22rects%22%3A%5B%5B92.5%2C244.344%2C443.333%2C398.51%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%7D" width="585" height="257" src="attachments/5P74TJKU.png" ztype="zimage">](attachments/5P74TJKU.png)\
<span class="citation" data-citation="%7B%22citationItems%22%3A%5B%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%5D%2C%22properties%22%3A%7B%7D%7D" ztype="zcitation">(<span class="citation-item"><a href="zotero://select/library/items/XB8S2BEL">Jang 등, 2021, p. 9</a></span>)</span>
large deformation regime을 고려하여 maximum eps를 0.6으로 설정
![\<img alt="" data-attachment-key="RVR5DBH6" data-annotation="%7B%22attachmentURI%22%3A%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FCN4SF6GA%22%2C%22annotationKey%22%3A%22TTWRAANP%22%2C%22color%22%3A%22%23ffd400%22%2C%22pageLabel%22%3A%229%22%2C%22position%22%3A%7B%22pageIndex%22%3A8%2C%22rects%22%3A%5B%5B53.333%2C117.677%2C494.167%2C186.427%5D%5D%7D%2C%22citationItem%22%3A%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%7D" width="1000" height="155" src="attachments/RVR5DBH6.png" ztype="zimage">](attachments/RVR5DBH6.png)\
<span class="citation" data-citation="%7B%22citationItems%22%3A%5B%7B%22uris%22%3A%5B%22http%3A%2F%2Fzotero.org%2Fusers%2F10857963%2Fitems%2FXB8S2BEL%22%5D%2C%22locator%22%3A%229%22%7D%5D%2C%22properties%22%3A%7B%7D%7D" ztype="zcitation">(<span class="citation-item"><a href="zotero://select/library/items/XB8S2BEL">Jang 등, 2021, p. 9</a></span>)</span>
이 때 너무 큰 lodaing path는 제외하였고, $\bar\sigma(\boldsymbol\sigma^{TR})-\rho(\bar\varepsilon^p)>1.2^{*}(initial\ yield\ stress)$ 즉 R값이 너무 큰 경우를 제외함.
최종 dataset size는 338182개로, 이 중 90퍼는 training에, 10퍼는 test에, 0.1퍼는 validation set (overfit 없도록 validation set의 cost function 모니터)으로 사용됨
50 epoch 이상 cost function for validation set의 차이가 발생하지 않는다면 training 멈춤
Referred in <a href="./-L3WINEIK.md" rel="noopener noreferrer nofollow" zhref="zotero://note/u/L3WINEIK/?ignore=1&#x26;line=-1" ztype="znotelink" class="internal-link">Workspace Note</a>
Referred in <a href="./-L3WINEIK.md" rel="noopener noreferrer nofollow" zhref="zotero://note/u/L3WINEIK/?ignore=1&#x26;line=-1" ztype="znotelink" class="internal-link">Workspace Note</a>
Referred in <a href="./-L3WINEIK.md" rel="noopener noreferrer nofollow" zhref="zotero://note/u/L3WINEIK/?ignore=1&#x26;line=-1" ztype="znotelink" class="internal-link">Workspace Note</a>
