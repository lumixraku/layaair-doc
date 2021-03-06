#Layair 2.0 정식 버전 발표, 중요 특성 전면 소개

> author:charley Date:2019-1-19

9월 15일 처음으로 Layair 2.0 엔진 테스트판 발표 이래[点此查看2.0引擎新特性](http://mp.weixin.qq.com/s?__biz=MzAxMjI4NjA1OA==&mid=2650584322&idx=1&sn=375e3dceaaf2b405e728bcba8f174d1e&chksm=83bc3407b4cbbd11c76ea98a032c328e253b80163cd4e68f3ebe5ced75b36beeccf511e87132&scene=21%3Ch1%3Ewechat_redirect)옛날`4`개월 넘게 내놓다`4`2.0 beta 버전 중 BUG 몇 개, 2D 엔진과 IDE 최적화 및 새로운 기능`39`항목, 3D 엔진 및 플러그인 최적화 및 신규 기능`22`항목엔진팀의 꾸준한 노력으로 개발자 2.0의 안정판을 가져왔다.여기에 테스트와 BUG의 개발자에게 많은 참여를 해 주셔서 감사드립니다.

우선 이번 공식 버전의 가장 핵심적인 업데이트 사항을 소개합니다:

###1, 2D 엔진의 drawCall 최적화된 기능 (drawCallOptimize)

2D 엔진에서 DrawCall 수량이 많으면 성능이 떨어질 수밖에 없다.Layaiair 엔진은 사진의 보카시 측면에서 많은 최적화를 했다. 예를 들어 인접한 같은 그림이 렌더링을 할 때 자동으로 합병해 보일 수 있다. 이렇게 하면 DrawCall 수량을 줄일 수 있다.그러나 UI 가 사용할 때 다른 그림이나 텍스트 삽입에는 반드시 그래프를 차단할 수 있습니다.개발자가 부당한 사용으로 생기는 불필요한 성능 지출을 초래해 성능에 걸리는 카드가 생길 수 있다.

기존 LayairierIDE 최적화 방안에서 개발자가 같은 색상의 도집 자원을 인접한 위치에 배열하면 엔진이 자동으로 합병해 성능적으로 최적화된다.최적화 방식은 그림 1개와 같다.

![图1](img/1.png) 


(그림 1)

도집 자원은 색상 정렬에 따라 최적화 효과가 뚜렷하지만 소수의 복잡한 장면에서 최소한 양의 불가피한 텍스트 에피소드 현상이 나타나 더욱 극심한 성능을 추구하기 위해서다.Layaiair 2.0 공식 버전에서 IDE 내에서 drawCallOptimize 최상층으로 추가되었으며, 기본값은 false 2의 표시, drawCallOptimize 참수가 true 로 설정되었을 때 엔진은 자동으로 텍스트를 합쳐 최상층으로 추출할 수 있으며, 개발자는 도화자원과 텍스트의 순서를 다시 조정할 필요는 없다. 자동drawCall 최적화의 목표를 달성할 수 있으며, 또한 자동으로 drawCall 우화할 수 있다.최적화된 것은 더욱 깔끔하다.그래서 이번 최적화는 성능 최적화 목표뿐만 아니라 사용자의 이용성을 높이고 최적화의 조작문턱을 낮추고 있다.

![图2](img/2.png) 


(2)

""drawCallOptimize 최적화 방안이 자동으로 텍스트를 표시하는 단계를 표시하기 때문에 텍스트를 반으로 가려야 할 특수 수요에 적용되지 않습니다.물론 대다수의 경우 텍스트가 전문적으로 나타날 것이며, 만약 숨기는 상황이 있으면 속성을 직접 설정할 수 있다.그래서 개발자는 이 최적화 방안을 열어줄 것을 건의했다.

###2, 새로운 프로젝트 발표 기능 추가

Layaiair IDE 2.0 정식 버전에는 새로운 프로젝트의 발표 기능 3.0 버전도 새로 늘었다.압축, 버전 관리, 소규모 게임 추출 등 기능을 더욱 완벽하게 하고 유연성, 개발자는 모든 기능의 사용에 대해 사용자 정의 제어, 기능이 더 자유롭고, 제품 발표 기능의 기능을 대폭 높이는 기능을 높일 수 있다.

이 기능은 소개할 내용이 많기 때문에, 독립 문서를 미리 발표했다.[《LayaAir IDE项目发布3.0详解》](https://mp.weixin.qq.com/s/AMS7xEqVbLpbfo2F5li3vw)개발자는 본문을 읽을 수 있는 후에 문서 링크를 클릭하고, 상세한 읽기 항목 발표 3.0의 기능소개를 자세히 읽을 수 있습니다.

###3, 3D 성능 통계 패널 최적화

####새로 스펙트럼 통계 인자 RenderBatch

2D 성능은 대개 drawCall 수를 보는데, drwaCall 이 한 번의 비판이다.3D 성능이 drawCall 을 보면 정확하지 않다. 3D 엔진이 스펙트럼의 합병 처리를 하기 때문에 drawCall 수를 보면 성능 문제를 판단하기 어렵다.이에 따라 2.0버전을 시작으로 새로운 인자 레이더배틀을 선보여 더욱 전문적이고 정확하다.제시한 대로.이후 개발자는 렌더배트치의 수치를 보고 실제 렌더바이스를 제출하는 횟수이며, 수치는 업무 수요를 만족시키는 상황에서 낮을수록 좋다.

![图3](img/3.png) 


(그림 3)

####CPU 및 GPU 메모리

이전에 메모리 디스플레이는 통계에 포함되었다.사실 메모리 점용 문제의 조사는 물론 2.0버전을 시작으로 CPU 와 GPU 의 메모리 통계를 각각 나타낸다.CPUMEmorry와 GPUMemorry의 수치를 직접 살펴보면 된다.제시한 대로.



###4. GPU 텍스처 압축 증가

Layaiair 2.0 엔진 공식 버전에 GPU 텍스처 압축 기능을 증가시켜 스티커의 현존 점용은 최소 75% 에 이른다.만약 원래 100M 을 점용해야 한다면 지금은 20여 M만 차지하는 셈이다.이것은 프로그램관리 메모리 원가를 크게 줄이고 미술을 발휘할 수 있는 공간을 확대해 게임 화질을 더욱 정교하게 만든다.

또한 Layabox 추진을 통해 위신7.0버전부터 위신소신용 소규모 게임 밑부분도 GPU 텍스처 압축을 지지했다.개발자들의 작은 게임 화면은 품질이 더 좋아질 수 있다.



###5, 메시 파일 압축 증가

레이야아 2.0 엔진 본판에는 메시 파일의 압축 기능도 증가해 메쉬 파일 크기가 약 60% 감소하고 3D 모델 네트워크 다운로드 부담이 절반 이상에 이른다.동등한 품질의 3D 게임으로 게임의 속도를 가재할 수 있다는 얘기다.아시다시피 게임 가재 속도는 사용자의 전환 데이터에 직접적인 영향을 미치는 것으로 알려져 개발자들은 가능한 한 빨리 이 기능을 사용할 수 있다.



###6, 비활성 자원의 인터페이스 증가 destroyUnusedResourcess


이전의 엔진 버전에서 개발자는 목록을 통해 자원을 관리하고, 개발자가 골치 아픈 문제로, 특히 3D에서는 자원 종류가 많고 공유 문제에 관해 개발자가 안전하게 목록 관리를 위해 자원 관리를 하기 어렵다.Layaiair 2.0 엔진 공식 버전에 간단한 사용이 불가능한 자원을 방출하는 인터페이스를 늘렸다`Laya.Resource.destroyUnusedResources();`) 2D와 3D의 자원 관리를 대폭 향상시켰다.



###7, 신입 회원 기능

Layaiair 2.0 본판부터 일반 개발자의 일상 개발 기초에 영향을 주지 않고 회원 전속 엔진 기능을 내놓는다.1024위안은 엔진의 연비 회원이 될 수 있으며, 고단회원 전속기능을 즐길 수 있다(예를 들어 GPU 텍스처 압축과 메시 파일 압축은 회원 기능에 속한다), 엔진의 전속회원 기능도 일정한 주파수가 새로운 기능을 유지할 수 있지만 가격은 변함없다.또 엔진 전속 기능을 기반한 회원비 수입은 모두 Layaiair 엔진 자체의 발전에 쓰일 것이기 때문에 Layair 엔진의 자립점이 될 것이며 개발자의 지지를 통해 양성발전을 이어갈 수 있도록 엔진을 지속할 수 있도록 노력할 것이다.



이번 본판의 중점 증가 기능 외에.


####이 4개월여 동안 2D 엔진과 IDE 신증과 최적화된 기능이 있습니다:

1. 물리 엔진을 늘리는 보조선 설정
2. 물리 엔진 RigigidBody getWorldCenter 인터페이스를 늘려 강체 중심점을 얻기 편리하다
3. 물리 엔진 Physics 종류가 강체 증가, 관절 수량, 충돌 수량
4. 물리 엔진의 충돌 사건 증가 충돌 정보 취득 방법
5. 물리 엔진 변경 RigidBody linearVelocity 속성 object 형식
6. 엔진 Loader가 sk, ani 등 파일 접미사에 대한 자동식별 증가
7. 엔진 Scene open 방법 증가 parm 인자 증가
8. 엔진 Scene 종류 증가 단례의 지원
9. 핸드Q 가벼운 게임의 적절한 지원
10. 엔진 Byte 종류 증가 readArrayBuffer 방법
11. 엔진 골격 애니메이션 증가 수치 이상처리
12. 엔진 증가 바이두 소규모 게임 적용
13. 엔진 Scene 종류 증가 progress 리플레이 증가, 진행 정보 추가
14. 엔진 Scene loading 페이지 설정을 추가하여 setLoading Page 방법으로 페이지를 다운로드할 때 로ading 페이지를 표시하고, loading 페이지는 현재 장면의 progress 이벤트를 자동으로 받을 수 있습니다
15. 엔진 Scene 증가 showLoding Page와 하이드 LoadingPage 수동 제어 loading 페이지 표시
16. 엔진 Scene의 close 방법 증가 type 속성, 닫는 원인을 알기 쉽다
17. 엔진 Sprite loadImage 방법 증가 url
18. 엔진 SceneLoader sk 파일을 추가할 때 Png 파일을 자동으로 가재합니다.
19. 엔진 배합 라이브러리, 마이크로신과 바이두의 작은 게임 입력 상자의 정규 사용지원
20. 작은 게임에 맞추기 위해 IDE 는 장면 등 파일을 json 으로 내보낼 수 있다.
21. IDE graphics 속성 패널 증가 rendeertype 설정
22. 아이디에서 마이크로폰 개방 데이터 영역 전시 요소 추가
23. 아이디에서 마이크로폰 소규모 유량 공유 요소 증가
24. UI 라이브러리 동태에 피부의 구성 요소를 가재 완료 후 resize 이벤트 추가
25. IDE 에서 같은 종류 노드 를 새로 증가 하고, 이 종류 의 더 많은 속성 기능 보이기
26. IDE 에서 새로운 style 파일 (자원 기본 속성) 변화 기능을 발견하면, style 변화가 발생하면 IDE 가 자동으로 고칠 수 있으며, 효과적으로 효과가 잘못된 문제를 나타내는 것을 방지할 수 있습니다.
27. IDE 에서 구궁 칸 설정 인터페이스 입력 상자 tab 전환 기능
28. IDE 중 새로운 텍스처 이미지 변환 도구를 추가하여 안탁과 ios 파일 메모리 점용 크기를 크게 감소 (VIP 기능)
29.IDE 장면 페이지 오른쪽 키 증가 검색 참조 기능
30. IDE 재생 캐시 최적화, 파일은 변함없이 내보내며 번역 효율을 높이기
31. IDE 최적화 감청류 파일 수정, 컴파일을 수정하지 않고 컴파일링 효율을 높이기
32. IDE 블록 추가 프로젝트 (ETH, NEO, HPB)
33. 관련 물리 주석 최적화, 상세 소개 설명
34. 음효 석방 전략을 최적화시켜 더욱 합리적으로 만든다
35. drawCirl drawline 등 벡터 인터페이스를 최적화
36. 물리 조립 요소 최적화 대상 창설 비용
37. 물리 엔진을 최적화하는 마우스 관절, 설정 제어점이 선택되어 있으며, 설정하지 않으면 마우스 클릭 위치를 제어점으로 삼는다

####이 4개월여 동안 3D 엔진과 유닛 내보내기 플러그인과 최적화된 기능은:

1. CompoundColliderShape, clearchildShape 방법 증가
2. ShinnedMeshRenderer rootbone 연관 메커니즘 조정, 노트본은 골격 노드 노드 매트릭스
3. 애니메이터가 요정 active 속성 지원 증가
4. Rigidbody3D 구성 증가 수면 상태 속성 issleeping
5. Rigidbody3D 구성 요소 증가 sleepLinearVelocity sleepAngularVelocity 속성
6. 물리 구성 요소 activate () 방법을 제거하고 Rigidbody3D 구성 요소 wakeUp
7. TrailSprite3D 요정을 재구성하고 일부 BUG 를 복원해 API 문의 대부분을 자세히 볼 수 있다
8. PixelLineSprite3D 요정, 최적화 API 증강성, API 문서를 자세히 볼 수 있다.
9. Vector3 SetValue 방법 증가
10. TrailRender 신규 TransformZ 모드
11. 카메라 render 함수 증가replacementag 인자 증가
12. 1Shader 프레임 추가 SubShader 개념
13. 프로그램화 하늘 소재 증가
14.사용자 정의 Shader 설정 인자 변경
15. 애니메이터 애니메이션 지원 재생 기능
16. 모형 파일 압축 기능 증가
17. PrimitiveMesh 관련 자류를 메쉬 통용류로 조정하여 정적 공장식 Primitivemesh.createX () 방법, 간소화
18. 3D 모드 로타tionOverLifeTime 모듈 관련 기능
19. 유닛 플러그인 계정 관리 페이지 추가
20. 유닛 플러그인 메시 파일 압축 기능 증가
21. 유닛 플러그인 blinphong 소재 포인트
22. 유닛 플러그인 유닛 중 Layashader 출력 색상 범위
23. 유닛 플러그인 복구 법선 스티커 BUG 내보내기
24. 유닛 플러그 최적화 안탁플랫폼 텍스처 압축 속도
25. 유닛 플러그인 LayairRun 기능을 최적화시켜 cmd 창 제거
26. 유닛 플러그인 자원 내보내기 속도 대폭 최적화



###개발자가 Laya2.0에 대한 새로운 특성에 대해 잘 모른다.9월 15일 공측을 볼 때 엔진의 새로운 특성에 대한 전면 소개를 할 수 있습니다:

다음 링크:

[9月15日LayaAir 2.0 开始测试，引擎新特性全面介绍](http://mp.weixin.qq.com/s?__biz=MzAxMjI4NjA1OA==&mid=2650584322&idx=1&sn=375e3dceaaf2b405e728bcba8f174d1e&chksm=83bc3407b4cbbd11c76ea98a032c328e253b80163cd4e68f3ebe5ced75b36beeccf511e87132&scene=21%3Ch1%3Ewechat_redirect)

