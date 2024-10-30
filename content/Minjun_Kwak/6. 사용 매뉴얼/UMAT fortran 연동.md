1. 아바쿠스 설치
    1. 아래 파일 참고. 연구실 클라우드 상의 크랙 파일은 설치하지 말 것.
    2. 메뉴얼엔 명시되어있지 않으나 **설치할 때 반드시** **CAA API****까지 설치 완료해야함**
        
        ![[2023_%EC%95%84%EB%B0%94%EC%BF%A0%EC%8A%A4_%EC%84%A4%EC%B9%98_%EB%A7%A4%EB%89%B4%EC%96%BC-%EC%95%9E%EC%9C%BC%EB%A1%9C%EB%8A%94_%EC%9D%B4%EA%B1%B0%EB%B3%B4%EA%B3%A0_%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94_(%EC%9C%A4%EB%AF%BC).pdf]]
        
2. 비주얼 스튜디오 설치
    1. community, professional, enterprise버전이 있으며, 이 중 학생 및 개인용 무료버전은 community버전임. 다른 버전을 설치하는 경우 trial 기간동안만 사용할 수 있으니 주의
    2. 2022는 포트란 설치가 잘 안됨. 시도해보고 안되면 2019버전으로 하거나 안전하게 **처음부터 2019를 사용**할 것  
          
        [https://my.visualstudio.com/Downloads?q=visual studio 2019&wt.mc_id=o~msft~vscom~older-downloads](https://my.visualstudio.com/Downloads?q=visual%20studio%202019&wt.mc_id=o~msft~vscom~older-downloads)
    3. 설치할 항목 선택 시 **C++ 를 사용한 데스크톱 개발**에 체크 후 설치 진행. 오래 걸리니 3번의 Fortran compiler 파일 먼저 다운로드해도 됨.
    4. 설치가 완료되면 VS 구동은 되지만, 아직 언어에서 fortran은 선택할 수 없는 상태로, fortran compiler 설치가 필요함  
          
        
3. Fortran compiler 설치
    1. 구글에 intel oneapi toolkit 검색 후 나오는 intel의 toolkit download 사이트에서, **oneAPI base toolkit**과 **one API HPC toolkit** 을 다운로드. VS 설치가 오래 걸리니 VS 설치 중에 미리 offline 버전으로 다운로드하면 좋음
    2. Base Toolkit 먼저 설치
        1. 구성요소를 모두 설치해도 되지만 용량이 크므로, custom installation에서 필요한 부분만 체크해서 설치
        2. abaqus 연동 없이 visual studio에 포트란만 사용한다면 **intel distribution for GDB**만 설치하면 되고, abaqus 연동까지 필요하다면 여기에 **DPC ++ compatibility tool**과 **oneAPI threading building blocks**, **DPC++/C++ compiler, DPC++ library**까지 설치해줌.
        3. integrating IDE 단계에서 설치된 visual studio 2019버전이 뜨고, 체크가 되어 있는지 확인해볼 것.
        4. 중간에 Intel graphic driver 관련해서 warning 나오는 경우도 있으나 무시해도 됨
    3. HPC toolkit 설치
        1. HPC toolkit은 그냥 recommended installation 그대로 설치. 용량 1GB 미만임  
              
            
4. 포트란 설치 테스트
    1. Visual studio를 키고, 새 프로젝트 만들기를 눌러 시작하면 템플릿을 선택할 수 있는데, 정상적으로 포트란 설치가 되었다면 fortran 언어에 대한 템플릿을 선택할 수 있다.(모든 언어 → Fortran)
    2. main program code 템플릿을 선택해 새로 만들어주면 Hello world 를 프린팅하는 샘플 프로그램 코드가 나온다. 이 코드가 제대로 동작하면 포트란이 정상적으로 설치된 것
    3. 코드 실행 전 테스트를 위해 print *, 'Hello World' 와 end program 사이에 Pause 라고 한 줄을 추가한 뒤 F5를 눌러 실행시킨다
    4. 정상적이라면 명령프롬프트가 뜬 뒤 Hello World라고 출력된다.
    5. 이제 포트란 언어는 사용할 수 있으나 아바쿠스 UMAT에 사용하기 위해선 추가적인 아바쿠스와의 연동 작업이 필요  
          
        
5. Abaqus와의 연동
    1. 아래의 파일을 참고해서 연동 진행할 것.
        https://launchtech.ae/blog/compiling-abaqus-with-fortran//
        ![[LinkingAbaqus2023InteloneAPIFortranCompilerVisualStudioinWindows10.pdf]]
        
        1. ABAQUS 및 포트란 컴파일러 버전에 따라 경로가 다를 수 있으니 반드시 경로를 정확히 확인 후 입력할 것.
        2. 파일에서 바로가기 수정하는 부분 경로 복붙할 때 특수기호 등에서 오류가 날 수 있으니 아래의 경로를 수정해 복붙해서 사용할 것.
            1. CAE 바로가기
                1. "C:\Program Files (x86)\Intel\oneAPI\compiler\2023.2.0\env\vars.bat" -arch intel64 vs2019 & abq2023 cae
            2. command 바로가기
                1. "C:\Program Files (x86)\Intel\oneAPI\compiler\2023.2.0\env\vars.bat" intel64 vs2019 & C:\WINDOWS\system32\cmd.exe /k
    
    3. ~~윈도우 검색에 시스템 환경 변수 편집 검색 → 고급 탭 아래 환경변수로 진입 → 아래쪽 시스템 변수 리스트 중 변수 명이 path인 것을 찾아 편집~~
    4. ~~두 가지 새로운 경로를 추가해야 함.  
        C:\Program Files (x86)\Intel\oneAPI\compiler\  
        ~~~~**2023.2.0**~~~~\env\vars.bat  
        C:\Program Files (x86)\Microsoft Visual Studio\2019\  
        ~~~~**Community**~~~~\VC\Auxiliary\Build\~~~~**vcvarsall**~~~~.bat  
        → API 설치 장소 및 버전에 따라 경로가 다를 수 있으니 경로 따라 들어가서 확인해본 뒤 본인 컴퓨터에 맞는 경로를 복사하여 환경변수 리스트에 새로 추가  
        완료되면 OK눌러 나간 뒤, 환경변수 새로만들기를 눌러 2가지를 추가  
        HOME, 변수명은 %UserProfile%  
        HOMEPATH 변수명은 마찬가지로 %UserProfile%  
        다 했음 확인 눌러 나감  
        ~~
    5. ~~아바쿠스가 설치된 드라이브에서 다음 경로로 이동  
        C:\SIMULIA\Commands  
        이 경로에서 ‘abq20**.bat’ 배치파일을 찾아 notepad로 편집  
        ~~
    6. ~~2 번째 줄 setlocal 아래에 다음을 추가  
        SET PATH=%PATH%;C:\Program Files (x86)\Intel\oneAPI\compiler\  
        ~~~~**2023.2.0**~~~~\windows\bin\intel64;  
        call "C:\Program Files (x86)\Intel\oneAPI\compiler\  
        ~~~~**2023.2.0**~~~~\env\vars.bat" intel64  
        이 때 반드시  
        ~~~~**본인이 설치한 버전에 맞게 수정하여 입력**~~~~할 것. (20**.*.*)~~
    7. ~~C:\SIMULIA\EstProducts\2023\win_b64\SMA\site  
        위 경로에서 abaqus_v6.env 파일을 찾아 notepad로 편집  
        ~~
    8. ~~가장 아래줄에 다음을 추가  
        compile_fortran += ['/names:lowercase',]  
        ~~
    9. ~~이후 abaqus_v6.env 파일을 복사하여 C:\Users\사용자명 경로에 복사~~
    10. ~~시작메뉴 abaqus command 우클릭하여 자세히→ 파일 위치 열기 누르면 바로가기가 모여있는 폴더(C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Dassault Systemes SIMULIA Established Products 2023)가 나옴~~
    11. ~~abaqus command 바로가기 우클릭해서 속성 진입하여 바로가기 탭에서 경로(대상)를 "C:\Program Files (x86)\Intel\oneAPI\compiler\~~~~**2023.2.0**~~~~\env\vars.bat" -arch intel64 vs2019 & C:\Windows\system32\cmd.exe /k으로 수정. API 버전 확인 후 수정하여 사용할 것.~~
    12. ~~abaqus CAE 바로가기도 같은 방식으로 수정하여 경로를 아래로 설정  
        "C:\Program Files (x86)\Intel\oneAPI\compiler\  
        ~~~~**2023.2.0**~~~~\env\vars.bat" -arch intel64 vs2019 & abq2023 cae~~
    13. ~~5번의 과정이 잘 안되면 아래 파일 참고~~
        
        ![[LinkingAbaqus2023InteloneAPIFortranCompilerVisualStudioinWindows10.pdf]]
        
6. UMAT 작동 테스트
    
    1. abaqus command를 실행시키고, abq2023 info=system 작성 후 엔터  
        → 잘 나오면 일단 OK  
        
    2. CAE 상에서도 Job edit을 통해 UMAT 입력 후 사용할 수 있으니 샘플 파일이 있다면 해보면 됨