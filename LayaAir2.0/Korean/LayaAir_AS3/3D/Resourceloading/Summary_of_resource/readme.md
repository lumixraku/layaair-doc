# 资源概述

###### *version :2.0.1beta   Update:2019-3-19*

레이이아르3D 세계에서 개발할 때 사용된 주요 자원 종류: 장면, 모형 격자, 소재구, 재질 스티커, 애니메이션 파일.각종 자원 유형은 뒤의 대응 과정에서 상세한 설명을 할 것이며 여기에서는 더 깊지 않다.이 절의 문서는 유닛에서 Layaiair에서 내보내는 도구 생성된 다양한 파일을 설명할 수 있으며, 대응하는 가재 방법입니다.

###자원 유형

`.ls`배경 파일을 위해 Scene 분류를 내보낼 때 생성을 선택하십시오.그 중에는 필요한 각종 데이터, 광샷, 모형, 위치 등이 포함되어 있다.사용**Sce3D**종류 가재.

`.lh`파일을 미리 설정하기 위해 Sprite3D 분류를 내보낼 때 생성합니다.그 중에는 장면 정보가 부족하고 다른 특징은 ls 파일과 같지만 사용할 필요가 있다.**Sprite3D**종류 가재.

`.lm`모형 데이터 파일을 위한 일반적으로 FBX 형식의 전환으로 변환됩니다.사용 가능**Messprite3D**종류 가재.

`.lmat`소재 데이터 파일로 유닛에서 모형을 위한 소재 정보.ls 나 lh 파일을 가재할 때 자동으로 lmat 파일이 재질이 생길 수 있습니다.사용 가능**Basematerial**종류 가재.

`.lani`애니메이션 데이터 파일.모형에 애니메이션이 있으면 내보내면 생성된 애니메이션 프로필 파일을 포함한다.다운로드 가능**애니메이션 클립**종류 가재.

`.jpg`,`.png`,`.ltc`,`.ktx`,`.pvr`등 스티커 파일.스티커에 사용할 경우 유닛이 내보내면 스티커 파일을 생성할 것입니다.사용 가능**Texture2D**종류 가재.