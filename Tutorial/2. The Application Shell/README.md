# 어플리케이션 쉘(Shell)
---

## Angular CLI 설치
Angular CLI(https://github.com/angular/angular-cli)를 설치합니다. 이미 설치 했다면 넘어가세요.

```

npm install -g @angular/cli

```


## 새로운 어플리케이션 생성
CLI 커맨드로 ```angular-tour-of-heroes```이름의 새로운 프로젝트를 생성합니다.  

```

ng new angular-tour-of-heroes

```
  

Angular CLI가 새로운 프로젝트를 생성했습니다. 기본 어플리케이션과 서포팅 파일들이 포함되어 있습니다.  


## 어플리케이션 구동
프로젝트 디렉토리로 이동하고 어플리케이션을 실행합니다.  

```

cd angular-tour-of-heroes
ng serve --open

```
  

>
__ng serve__ 커맨드는 앱을 빌드하고 개발 서버로 시작합니다. 수정된 소스파일들을 감지하고 앱을 리빌드를 하는 커맨드입니다.  
__--open__ 플래그는 브라우저에서 *http://localhost:4200/*를 열도록 하는 옵션입니다.
  

브라우저에서 앱이 구동된 것을 확인할 수 있습니다.  


## Angular 컴포넌트
현재 보고 계신 단계는 *어플리케이션 쉘(shell)*입니다. 쉘은 ```AppComponent```라는 Angular 컴포넌트에 의해 제어됩니다.  

*컴포넌트*는 Angular 어플리케이션의 기본적은 빌딩 블록입니다. 스크린에 데이터를 표시해주고 사용자의 입력에 반응하고 입력 기반의 동작을 시행합니다.  


## 어플리케이션 타이틀 변경
즐겨 쓰는 에디터나 IDE로 프로텍트를 열고 ```src/app``` 폴더로 들어갑니다.  

세 개의 파일에 분산된 쉘 ```AppComponent```를 확인하실 수 있습니다:  

1. app.component.ts— 컴포넌트 클래스 코드이며, TypeScript로 작성됐습니다.
2. app.component.html— 컴포넌트의 템플릿이며, HTML로 작성됐습니다.
3. app.component.css— 컴포넌트 전용의 CSS 스타일입니다.
  

컴포넌트 클래스 파일(```app.component.ts```)을 열고 맴버변수인 ```title```의 값을 'Tour of Heroes'로 변경합니다.

### app.component.ts (class title property)
```

title = 'Tour of Heroes';

```

컴포넌트 템플릿 파일(```app.component.html```)을 열고 Angular CLI가 기본으로 생성했던 템플릿을 제거합니다. 그리고 아래의 HTML로 바꿔주세요.

### app.component.html (template)
```

<h1>{{title}}</h1>

```
  

더블 중괄호는 Angular의 변수를 삽입하는 바인딩 문법입니다. 이 바인딩은 컴포넌트의 맴버변수로 있는 ```title```의 값을 HTML 헤더 태그의 내부에 집어 넣은 것입니다.  

브라우저가 새로고침 되고 새로운 어플리케이션 타이틀이 보여집니다.  


## 어플리케이션 스타일 추가
대부분의 앱은 전체적으로 일관된 모양을 가지기 위해 노력합니다. 이러한 목적으로 CLI는 빈 ```styles.css```를 생성했습니다. 이 파일에 어플리케이션 전역으로 스타일을 설정하세요.  

아래는 *Tour of Heroes* 앱에 있는 styles.css를 발췌한 것입니다.  

### src/styles.css (excerpt)
```

/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[text], button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}

```

## 최종 코드 리뷰
이번 튜토리얼에서 소스 코드와 완성된 *Tour of Heroes*의 전역 스타일은 live example(https://angular.io/generated/live-examples/toh-pt0/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt0/toh-pt0.zip) 에서 확인할 수 있습니다.  


아래는 이번 단계에서 논의한 코드 파일들입니다.  

### src/app/app.component.ts
```

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Tour of Heroes';
}

```
---

### src/app/app.component.html
```

<h1>{{title}}</h1>

```
---

### src/styles.css (excerpt)
```

/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[text], button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}

```



## 요약
* 당신은 Angular CLI로 초기 어플리케이션 구조를 생성했습니다.
* 당신은 Angular 컴포넌트가 어떻게 데이터를 표시하는지를 배웠습니다.
* 당신은 타이틀 보여주기위해 더블 중괄호를 사용하였습니다.
