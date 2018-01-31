# 히어로 편집기
---
지금 어플리케이션은 기본 타이틀을 가지고 있습니다. 다음으로 진행할 것은 히어로 정보를 보여주기 위한 새로운 컴포넌트를 만들고 그 컴포넌트를 어플리케이션 쉘에 넣는 작업을 진행할 것입니다.  


## heores 컴포넌트 생성
Angular CLI를 사용하여 ```heores```라는 이름의 새로운 컴포넌트를 생성합니다.  

```

ng generate component heroes

```
  

새로운 폴더 ```src/app/heroes/```가 생성됐습니다. 그리고 그 폴더 안에 ```HeroesComponent```에 관한 파일 세 개가 생성됐습니다.  

```HeroesComponent```클래스 파일은 다음과 같습니다:  

### app/heroes/heroes.component.ts (initial version)
```

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
  
컴포넌트 클래스는 항상 Angular 코어 라이브러리에서 ```Component```(https://angular.io/api/core/Component) 심볼을 가져오고 ```@Component```로 어노테이트(annotate) 시켜줍니다.

> 어노테이트(Annotate)는 사전적 의미로 주석을 붙이다 입니다. 이 개념은 Java의 Annotation에서 왔는데, TypeScript에서는 Decorator라고 칭합니다. 둘이 문법적으로는 @를 붙여서 사용한다는 점은 유사하지만 내부적으로 작동하는 방식은 차이가 있습니다. Decorator에 관한 설명은 http://www.typescriptlang.org/docs/handbook/decorators.html 를 참고하세요.

```@Component```는 데코레이터 함수입니다. Angular 메타데이터를 컴포넌트에 지정해줍니다.  

CLI로 생성한 세 가지 메타데이터의 속성들:  

1. selector— 컴포넌트 CSS 요소의 셀럭터와 동일
2. templateUrl— 컴포넌트 템플릿 파일의 경로
3. styleUrls— 컴포넌트 전용 CSS 스타일 파일의 경로

CSS 요소 셀럭터(https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors) 인 ```'app-heroes'```는 부모 컴포넌트 템플릿 내부에 있는 HTML 태그의 이름에 매칭 됩니다.  

```ngOnInit```은 라이프사이클 훅(hook)입니다. Angular가 컴포넌트를 생성한 직후 ngOnInit을 호출합니다. 초기화 로직을 넣기에 아주 좋은 장소입니다.  

```AppModule```과 같은 곳에서 ```import```하기 위해서는 컴포넌트 클래스를 ```export```해야합니다.  


## hero 맴버변수를 추가
```HeroesComponent```에 hero 맴버변수를 추가하고 "Windstorm"이라는 값을 넣습니다.  

### heroes.component.ts (hero property)
```

hero = 'Windstorm';

```
  

## 히어로 보여주기
```heroes.component.html```파일을 열어주세요. Angular CLI로 생성하여 만들어진 기본 텍스트는 지우고 다음과 같이 새로운 ```hero``` 맴버변수를 바인딩합니다.  

### heroes.component.html
```

{{hero}}

```
  

## *HeroesComponent*화면 보여주기
```HeroesComponent```를 보여주기 위해서 ```AppComponent```의 템플릿에 추가해야 되는 것이 있습니다.  

```HeroesComponent```를 사용하기 위해서 ```app-heroes``` 셀렉터(https://angular.io/tutorial/toh-pt1#selector) 가 필요하다는 것은 기억하실 겁니다. 그래서 ```AppComponent```의 템플릿 파일에 ```<app-hereos>``` 태그를 ```title``` 아래에 추가해주세요.  

### src/app/app.component.html
```

<h1>{{title}}</h1>
<app-heroes></app-heroes>

```
  
CLI에서 ```ng serve``` 커맨드가 여전히 실행되고 있다면 브라우저가 새로고침되고 어플리케이션 제목과 히어로 이름이 보이게 됩니다.  


## Hero 클래스 생성
진짜 히어로는 이름 이외의 더 많은 정보를 가지고 있습니다.  

```Hero```클래스를 ```src/app``` 폴더 안에 생성하세요. ```id```와 ```name```을 맴버변수로 선언해 주세요.  

### src/app/hero.ts
```

export class Hero {
  id: number;
  name: string;
}
```
  
```HeroesComponent``` 클래스로 돌아와서 ```Hero``` 클래스를 가져오세요.  

컴포넌트의 ```hero``` 맴버변수에 ```Hero```라는 타입을 부여하세요. ```id```는 ```1```로, ```name```은 ```Windstorm```으로 초기화해주세요.  

바뀐 ```HeroesComponent``` 클래스 파일은 다음과 같습니다:  

### src/app/heroes/heroes.component.ts
```

import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  constructor() { }

  ngOnInit() {
  }

}

```
  
```hero``` 맴버변수가 string에서 object로 바뀌었기 때문에 화면에서 더 이상 제대로 표시가 되지 않습니다.  


## 히어로 object 보여주기
hero의 ```name```을 알려주고 ```name```과 ```id``` 둘 다 보여주기 위해 다음과 같이 템플릿에 있는 바인딩을 바꿔주세요:  

### heroes.component.html (HeroesComponent's template)
```

<h2>{{ hero.name }} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>

```
  
브라우저가 새로고침되고 히어로의 정보가 표시됩니다.  

## *UppercasePipe* 포맷
```hero.name```을 다음과 같이 수정합니다.  

```
<h2>{{ hero.name | uppercase }} Details</h2>
```
  
브라우저가 새로고침되고 히어로의 이름이 대문자로 표시가 됩니다.  

바인딩 내에서 파이프 연산자인 ( | )의 오른쪽에 있는 ```uppercase```는 ```UppercasePipe``` 내장 파이프(pipe)로 동작합니다.  

파이프(https://angular.io/guide/pipes) 는 문자열, 통화량, 날짜 그리고 그밖에 데이터를 표시하기 위한 좋은 방법입니다. Angular는 여러가지 내장 파이프가 있고 만들어서 사용할 수 있습니다.  

## 히어로를 편집
사용자는 ```<input>```으로 히어로 이름을 편집할 수 있어야 합니다.  

텍스트박스는 히어로의 ```name```을 *보여주고* 사용자가 입력할 때 히어로의 ```name```이 갱신되야합니다. 이 동작은 데이터의 흐름이 컴포넌트 클래스에서 *화면 밖으로* 그리고 화면에서 다시 *클래스로 되돌아간다는* 것을 의미합니다.  

데이터 흐름이 자동적으로 이뤄지기 위해서는 ```<input>``` 태그와 ```hero.name``` 사이에 양방향(two-way) 속성을 추가해줘야 합니다.  


## 양방향(Two-way) 바인딩
```HeroesComponent``` 템플릿에 상세 영역을 다음과 같이 반영합니다:  

### src/app/heroes/heroes.component.html (HeroesComponent's template)
```

<div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
</div>

```
  
```[(ngModel)]```은 Angular의 양방향 데이터 바인딩 문법입니다.  

```hero.name```를 HTML 텍스트 박스에 바인딩하고 ```hero.name```는 양방향으로 흐를 수 있습니다(```hero.name```에서 텍스트 박스로, 텍스트 박스에서 다시 ```hero.name```으로)  

## *FormsModule*를 놓쳤습니다
```[(ngModel)]```을 추가했다면 앱이 멈췄다는 것을 알게 됩니다.  

에러가 보입니다. 브라우저 개발자 도구를 열고 콘솔탭에서 다음과 같은 메세지를 보세요  

```
Template parse errors:
Can't bind to 'ngModel' since it isn't a known property of 'input'.
```

```ngModel```은 유효한 Angular 지시자(directive)이지만, 기본적으로 사용할 수 있는 것은 아닙니다.  

```ngModel```은 ```FormsModule```에 선택적으로 속해있습니다. ```ngModel```을 사용하기 위해서는 옵트인(?) 해줘야 합니다.  

## *AppModule*
Angular는 어플리케이션의 조각이 어떻게 함께 맞춰지고 어떠한 파일과 라이브러리가 앱에 필요하지를 알아야 합니다. 이러한 정보를 *메타데이터(metadata)*라고 부릅니다.  

메타데이터중 일부는 컴포넌트 클래스에 추가 했던 ```@Component```(https://angular.io/api/core/Component) 데코레이터에 있습니다. 그 밖에 중요한 메타데이터는 @NgModule(https://angular.io/guide/ngmodules) 데코레이터에 있습니다.  

가장 중요한 ```@NgModule``` 데코레이터는 ```AppModule``` 클래스의 위에 붙어있습니다.  

Angular CLI는 프로젝트가 만들어졌을 때 ```src/app/app.module.ts```에 ```AppModule``` 클래스를 생성했습니다. 여기서 ```FormsModule```를 *옵트인(?)* 합니다.  


## *FormsModule* 가져오기
```AppModule```(```app.module.ts```)를 열고 ```@angular/forms```에 있는 ```FormsModule``` 심볼을 가져옵니다.  


### app.module.ts (FormsModule symbol import)
```

import { FormsModule } from '@angular/forms'; // <-- NgModel lives here

```
  
```@NgModule```가 가진 앱에 필요한 외부 모듈의 목록을 포함하는 ```imports``` 메타데이터의 배열에 ```FormsModule```을 추가힙니다.  


### app.module.ts ( @NgModule imports)
```

imports: [
  BrowserModule,
  FormsModule
],

```
  
브라우저가 새로고침 됐다면 앱이 다시 작동할 것입니다. 히어로의 이름을 편집할 수 있고 텍스트 박스위에 있는 ```<h2>```가 즉시 반영 됩니다.  


## *HeroesComponent* 선언
모든 컴포넌트는 반드시 하나의 ```NgModule```에만 선언되어야 합니다.  

우리는 아직 ```HeroesComponent```를 선언하지 않았습니다. 하지만 왜 어플리케이션이 정상적으로 작동했을까요?  

왜냐하면 Angular CLI가 컴포넌트를 생성했을 때 ```HeroesComponent```를 ```AppModule```에 선언했기 때문입니다.  

```src/app/app.module.ts```를 열고 ```HeroesComponent```가 포함된 것을 맨 위에서 찾아보세요.  

```

import { HeroesComponent } from './heroes/heroes.component';

```
  
```HeroesComponent```는 ```@NgModule.declarations``` 배열에 선언되어 있습니다.  
```

declarations: [
  AppComponent,
  HeroesComponent
],

```
  
```AppModule```이 ```AppComponent```와 ```HeroesComponent```를 선언했는지 확인하세요.  


## 최종 코드 리뷰
앱은 live example(https://angular.io/generated/live-examples/toh-pt1/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt1/toh-pt1.zip) 과 같을 것입니다. 여기에는 이번 단계에서 논의한 파일들이 있습니다.  


### src/app/heroes/heroes.component.ts
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
 
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };
 
  constructor() { }
 
  ngOnInit() {
  }
 
}
```
---

### src/app/heroes/heroes.component.html
```
<h2>{{ hero.name | uppercase }} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
</div>
```
---

### src/app/app.module.ts
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here

import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
---

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
<app-heroes></app-heroes>
```
---

### src/app/hero.ts
```
export class Hero {
  id: number;
  name: string;
}
```


## 요약
* 당신은 CLI로 두 번째 ```HeroesComponent```를 생성했습니다.
* 당신은 ```AppComponent``` 쉘에 ```HeroesComponent```를 추가하고 보여줬습니다.
* 당신은 이름의 포맷을 ```UppercasePipe```로 적용했습니다.
* 당신은 양방향(two-way) 데이터 바인딩인 ```ngModel``` 지시자(directive)를 사용했습니다.
* 당신은 ```AppModule```에 대한 것을 배웠습니다.
* 당신은 Angular가 인식하고 ```ngModel``` 지시자를 적용하기 위해 ```AppModule```에 ```FormsModule```를 포함시켰습니다.
* 당신은 ```AppModule```에 컴포넌트 선언의 중요성에 대해 배웠고 CLI가 당신을 위해 선언한 것에 대해 감사함을 느꼈습니다.
