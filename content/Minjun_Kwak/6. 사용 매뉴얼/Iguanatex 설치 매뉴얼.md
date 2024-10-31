---
quickshare-date: 2024-10-30 19:01:25
quickshare-url: https://noteshare.space/note/cm2vpimj9525501mwhcd1rroe#yRd2H8dOAbIiUS4rvZ7AacS/JdUPjgYWkUNEVeKVF7o
dg-publish: true
---
PPT등 자료를 구성할 때 수식을 많이 입력하는 경우 PPT 내장 수식 기능을 사용하면 수식이 길어지면서 PPT 구동이 매우 무거워지는 문제가 발생함. 연구실에서 구독해 사용중인 mathtype을 이용할 수도 있으나, 프로그램 자체의 자유도나 다른 운영체제와의 안정성 측면에서 단점이 존재함. 이를 해결하기 위한 방법으로 수식 입력에 최적화된 언어인 LaTeX를 이용한 입력방식이 있음. IguanaTeX는 LaTeX를 기반으로 PPT에 수식을 입력할 수 있도록 해주는 PowerPoint add-on임.


1. **LaTeX 설치** 
	1. https://miktex.org/download 에서 LaTeX 언어 축소판 설치
	2. LaTeX 편집기 설치 (Texstudio, https://www.texstudio.org/)
2. **Iguanatex 구동을 위한 보조프로그램 설치**
	1. 모든 프로그램은 설치 시 <u>설치 경로를 기억해둘 것</u>
		1. https://www.ghostscript.com/releases/gsdnld.html 에서 Ghostscript windows 다운로드 및 설치
		2. https://tex2img.tech/index.en.html 에서 tex2img windows용 다운로드 및 압축 해제
		3. https://imagemagick.org/script/download.php#windows 에서 windows용 ImageMagick 다운로드 및 설치
			1. 설치 시 additional task 선택 창에서 아래와 같이 선택함![[Pasted image 20241002124100.png]]
3. **Iguanatex 설치**
	1. https://github.com/Jonathan-LeRoux/IguanaTex/releases 에서 windows용 ppam파일을 다운로드
	2. 파워포인트 구동 후 파일-옵션-추가기능- 아래쪽 관리탭에서 PowerPoint 추가기능 선택 후 이동버튼 클릭 - 새로설치 선택 후 다운로드한 .ppam 파일 선택 후 설치
		1. 보안관련 경고는 무시해도 됨
	3. 설치 후 상단 메뉴 (파일, 홈, 삽입,...) 끝에 IguanaTeX 탭이 추가됨
4. **IganaTeX와 보조프로그램 연결**<br>![[Pasted image 20241002131821.png]]
	1. IguanaTeX 탭으로 이동 후 Main Setting 선택
	2. LaTeX 엔진은 pdflatex로 선택, Output은 Picture를 default로 설정함
	3. Ghostscript, ImageMagick, texstudio(1,2,3번째) 순으로 설치 경로 내에서 각각 gswin64c(64bit 일 경우), magick.exe, texstudiolive.exe 의 경로를 설정함.
	4. LaTeX source경로(5번째) 설정을 위해 MikTeX 설치 경로 내에서 pdftex.exe의 경로를 설정함. 
		1. 24.1 버전 64비트 기준 MiKTeX\miktex\bin\x64 경로 내에 있음.
	5. TeX2Img 경로를 설정함. TeX2Img 다운로드 후 압축 해제한 폴더 내에서 TeX2Img.exe의 경로로 설정
	6. 생성되는 latex 이미지 화질 조절을 위해 dpi를 추가로 조정 가능함.
5. **LaTeX 사용**
	1. 새 이미지 생성
		1. IguanaTeX 탭에서 New LaTeX display 선택 후 LaTeX 수식을 입력해 이미지 생성이 가능함. <br>![[Pasted image 20241002132812.png]]
		2.  "\begin{document}, \end{document}" 사이 공간에 수식 삽입이 가능함.  <br> 아래의 식을 복사 붙여넣기하여 테스트 가능
			1. $ r=\frac{\sum( X_{i}-\bar{X} ) ( Y_{i}-\bar{Y} )} {\sqrt{\sum( X_{i}-\bar{X} )^{2}} \sqrt{\sum( Y_{i}-\bar{Y} )^{2}}} $</br>
			   ![[Pasted image 20241002162700.png]]
			2. $\boldsymbol\sigma=\left[\begin{array}{c c c}{{\sigma_{1}}}&{{0}}&{{0}}\\ {{0}}&{{\sigma_{2}}}&{{0}}\\ {{0}}&{{0}}&{{\sigma_{3}}}\end{array}\right] $</br>  ![[Pasted image 20241002162708.png]]
		3. 수식 입력 후 generate를 누르면 이미지가 생성됨.
	2. 이미지 수정
		1. 생성된 이미지 내 수식을 수정하고자 할 경우 해당 이미지를 선택 후 IguanaTeX 탭에서 Edit LaTeX display를 선택하여 수식을 수정 가능함.
		2. 수식 수정은 IguanaTeX로 생성된 이미지만 수정이 가능하며, PPT기반이나Mathtype으로 생성된 수식은 IguanaTeX를 통해 수정이 불가능함. 해보면 에러가 뜸
6. **LaTeX 문법 및 사용 관련**
	1. LaTeX는 코딩에서 사용하는 것과 유사한 언어를 가지며, 사용하기 위해선 기초적인 문법을 익힐 필요가 있음. 
	2. LaTeX 지식 없이 사용하고자 할 경우 수식 OCR을 이용해 LaTeX로 변환해주는 기능을 활용하면 편리함. 아래는 몇 가지 유용한 무료 툴임.
		1.   Pix2Text (https://p2t.breezedeus.com/)
			1. 해당 사이트는 로그인 후 수식 사진을 복사 붙여넣기하면 사진 속 수식을 AI기반으로 자동 인식하여 LaTeX 식으로 변환해줌
		2. Simpletex (https://simpletex.cn/)
			1. 중국에서 개발된 무료 수식 snipping 툴이며, 마찬가지로 사진을 LaTeX로 변환해줌. Pix2Text와 다르게 프로그램 설치를 통해서도 사용이 가능함. 
	3. 위의 방법을 사용하더라도, 기초적인 수식 수정 등을 위해선 어느 정도 LaTeX를 알아 둘 필요는 있음. 
		1. $로 수식 앞 뒤를 감싸서 수식을 작성할 수 있음. 
		2. 기본적인 기호는 백슬래시(\\) 뒤에 이름을 넣어 사용 가능함
			1. \ \\alpha : α
			2. \ \\times : ×
			3. \ \\rightarrow : →
			4. \ \\partial : ∂
			5. 등등
		3. 윗첨자는 ^로, 아랫첨자는 \_로 입력함.
			1. \ \\alpha_1 : $\alpha_1$ 
			2. \ \\alpha^1 : $\alpha^1$ 
			3. \ \\alpha^{plastic} : $\alpha^{plastic}$ 
				1. 위와 같이 1문자가 아닌 여러 문자를 쓸 경우 중괄호로 묶어줘야함.
		4. https://velog.io/@jhjangjh/LaTex-%EC%88%98%EC%8B%9D-%EC%9E%91%EC%84%B1%EB%B2%95-%EC%A0%95%EB%A6%AC
			1. 기타 다른 문법은 위를 참고
7. **Word용 LaTeX 입력**
	1. IguanaTeX와 유사하게 texsword(https://sourceforge.net/projects/texsword/) 를 이용하면 Word에서도 사용이 가능함
	2. 설치는 Iguanatex에서와 마찬가지로 addon 방식으로 설치하면 됨.
	3. TEX버튼을 눌러 수식 입력이 가능하며, 수식 입력 후 Run LaTeX를 선택하면 이미지 형식으로 입력됨.</br>   ![[Pasted image 20241002162454.png]]
	4. 여기서 latex 수식을 넣을 땐 $를 앞뒤로 붙이지 않아야함. 
			 
