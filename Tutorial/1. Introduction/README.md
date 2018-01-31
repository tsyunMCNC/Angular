# 튜토리얼: Tour of Heroes
---
Tour of Heroes 튜로리얼은 Angular의 기본 사항을 다룹니다.  
이번 튜토리얼에서 당신은 안정적으로 히어로를 에이전시하도록 도와주는 앱을 빌드할 것입니다.  

이 기본 앱은 당신이 기대하는 data-driven 어플리케이션에서 찾을 수 있는 많은 기능을 가지고 있습니다. 히어로를 취득하고 보여주고 편집하고 다른 히어로 데이터의 견해를 탐색합니다.  

듀토리얼의 마지막까지 가기 위해 당신은 아래 사항을 따라야할 것입니다.:

* 히어로 데이터의 목록과 히어로 elements를 보여주고 가리기 위해 Angular 지시자(directives)를 사용합니다.
* 히어로의 상세 정보를 보여주고 히어로의 배열(array)을 보여주기 위해 Angular 컴포넌트를 생성합니다.
* read-only(읽기전용) 데이터를 위한 one-way(단방향) 데이터 바인딩을 사용합니다.
* two-way(양방향) 데이터 바인딩 모델을 갱신하기 위해 수정가능한 필드를 추가합니다.
* 키입력과 클릭과 같은 사용자 이벤트를 위한 methods를 컴포넌트에 바인딩 합니다.
* 유저를 마스터 목록으로부터 히어로를 선택하기 위해 활성화 하고 상세 보기에서 히어로를 편집합니다.
* pipe로 데이터를 포맷합니다.
* 히어로를 모으기 위해 공유된 서비스를 생성합니다.
* 다른 컴포넌트를 탐색하기 위해 라우팅(routing)을 사용합니다.

당신은 Angular를 시작하고 Angular에 대한 자신감을 얻기 위해 충분히 배울 것입니다.  

모든 튜토리얼을 완료한 후, 최종 앱은 live example( *https://angular.io/generated/live-examples/toh-pt6/eplnkr.html* ) / download example( *https://angular.io/generated/zips/toh-pt6/toh-pt6.zip* ) 같이 보일 것입니다.
  

## 당신이 빌드할 것은 무엇일까요
여기는 "Dashboard" 보기와 가장 영웅적인 영웅을 시작하는 이번 튜토리얼이 리드할 시각화 자료입니다.  	
![heroes-dashboard-1](https://angular.io/generated/images/guide/toh/heroes-dashboard-1.png)
  

Dashboard 위의 두 개의 링크("Dashboard"와 "Heores")를 클릭하여 Dashboard와 Heores 화면을 탐색할 수 있습니다.  

만약 데시보드에서 "Magneta"라는 히어로를 클릭했다면 라우터는 당신이 히어로의 이름을 변경할 수 있는 "Hero Details" 화면을 열 것입니다.  
![heroes-detail-1](https://angular.io/generated/images/guide/toh/hero-details-1.png)
  

뒤로가기 버튼을 누르면 Dashboard로 갑니다. 상단의 링크는 메인 화면중 하나로 안내합니다. 만약 "Heores"를 클릭 했다면 앱은 "Heroes" 마스터 목록 화면을 보여주게 됩니다.  
![heroes-list-2](https://angular.io/generated/images/guide/toh/heroes-list-2.png)
  

다른 히어로 이름을 클릭 했을 때, 새로 선택한 영웅으로 목록 아래에 있는 읽기 전용의 상세 정보에 반영합니다.  

"View Details" 버턴을 클릭 하면 선택한 영웅의 정보를 편집할 수 있습니다.  

다음 다이어그램은 모든 탐색 옵션을 담고 있습니다.  
![nav-diagram](https://angular.io/generated/images/guide/toh/nav-diagram.png)
  

다음은 앱의 액션입니다:  
![toh-anim](https://angular.io/generated/images/guide/toh/toh-anim.gif)