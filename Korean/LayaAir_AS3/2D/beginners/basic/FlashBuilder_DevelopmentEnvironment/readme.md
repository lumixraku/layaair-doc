#Flash Builder 개발 환경 설정

###첫걸음: 레이어잉

####1.1 엔진 다운로드

LayaiairIDE 개발을 사용하면 LayairIDE 자체 엔진 가방을 다운로드합니다.개발자가 제3자 도구 개발을 사용하면 우선 엔진을 다운로드해야 한다.홈페이지나 개발자 중심 메뉴 중 엔진 다운로드된 링크 입구를 갖고 링크 를 켜면 엔진 각 버전의 다운로드 목록이 나타납니다. 1개 버전마다 AS3, TS, JS 3가지 개발 언어를 제공하고, 대응하는 개발언어 가방을 선택하여 바로 다운로드하면 됩니다.

####1.2 엔진 가방 디렉터리 구조 소개

우리는 AS3 버전의 엔진 가방을 다운로드한다.다운로드 해압 후 AS3 버전의 디렉터리 구조가 아래와 같이 표시됩니다:

![图1](img/1.png)< br / > (그림 1)

-'Layair Flash'디렉토리에는 플래쉬 버전이 나와야 필요한 엔진 라이브러리 코드.

-'libs'엔진 코드 디렉토리, Layaiair 엔진 라이브러리 코드 코드가 이 디렉터리에 처해 있는 src 카탈로그에 있습니다.

-'laya.js.exe'는 윈도우 시스템 아래 AS3 코드 디코딩을 JS 코드로 편집하는 데 사용됩니다.

-'LayaJSMac'는 애플MAC 시스템 아래 AS3 코드 디코딩을 JS 코드로 번역하는 데 사용된다.

-'playerglobal.swc'는 AS3 원생 편집기 SDK, 원생 API 문법 제시, 엔진 API 힌트 추가.

-'worker.js', worker.js 경로를 지정하여 WorkerLoader 를 사용하여 디코딩 이미지를 다운로드하는 기능을 사용하여 최적화된 효과를 강조하며 관련 문서를 구체적으로 사용합니다.

​

###2단계 설치 Google Chrome 브라우저

AS3 코드 컴파일을 실행한 후 기본값으로 Chrome 브라우저로 실행 프로젝트를 실행하려면 이 브라우저 설치 프로그램을 설치해야 합니다.설치된 것은 본 단계를 뛰어넘을 수 있습니다.



###3단계 플래쉬 Builder AS3 컴파일링

​**절차 1**Flash Builder 열기 '외부 도구 설정' 을 찾아 설정 창을 엽니다.

​![2.jpg](img/2.jpg)< br >>
그림 (2)


​**절차 2**설정 창에서 '프로그램' 을 오른쪽 단추 '를 누르면 새 설정 창을 엽니다.

​![3.jpg](img/3.jpg)< br >>
그림 (3)



**절차 3**：

우선 외부 프로그램 이름을 수정합니다.`LayaCompiler`… 이다

그리고 '파일 탐색 시스템' 을 누르십시오.`laya.js.exe`"아니면 직접 복사".`laya.js.exe`'의 경로가 '위치' 입력 표시줄에 붙었다.

마지막으로'자변수'란에 입력`"${project_loc}\.actionScriptProperties;iflash=false"`클릭 "응용을 누르면 이번 설정을 완성할 수 있습니다.

만약 우리가 매번 편집할 때마다 새로운 구글 브라우저 프로세스를 시작하지 않으려면, 현재 설정방식에 따라 외부 프로그램을 다시 만들기 위해서는 자변수 변수 변수 변수 변수를 바꾸어야 합니다`"${project_loc}\.actionScriptProperties;iflash=false;chromerun=false;"`그림 4, 도 5 개.

![4](img/4.png)< br / > (그림 4)

![图5](img/5.png)< br / > (그림 5)

** Tips: “D:레이보이스Layayaissu 1.7.3u betalaya.js.exe’는 laya.js.exexexexexe의 실제 경로가 되어야 합니다.MAC 시스템은 "D:LayaboxLayayairas3u1.7.3u betaLayajac *



이에 따라 프로젝트를 구성하기 전 Layair 엔진 다운로드, 브라우저 다운로드, Flash Builder 환경 아래 AS3 편집기 설정이 끝났습니다.개발자는 다른 장절에서 계속 공부하는 것을 환영합니다.