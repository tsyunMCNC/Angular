# Routing
---

Tour of Heroes 앱에 새로운 요구사항이 있습니다:

	* Dashboard 화면을 추가합니다.
	* 히어로와 Dashboard 화면 사이를 탐색할 수 있는 기능을 추가합니다.
	* 사용자가 다른 화면에 있는 히어로 이름을 클릭 했을 때, 선택된 히어로의 상세 화면을 탐색합니다.
	* 사용자가 이메일 링크를 클릭 했을 때, 특정 히어로의 상세 화면이 열립니다.

위의 작업들을 했을때, 사용자들은 다음과 같은 탐색이 가능할 것입니다:  
![routing-done](https://angular.io/generated/images/guide/toh/nav-diagram.png)  

## *AppRoutingModule*를 추가
Angular 최고의 연습은 별도의 라우터를 로드하고 구성하는 것입니다. 라우터를 로드하고 구성하는 것은 top-level 모듈인 ```AppModule```에서 작업합니다.  

관례적으로 모듈 클래스 이름은 ```AppRoutingModule```이라고 하고 ```app-routing.module.ts``` 파일로 작성하여 ```src/app``` 폴더에 있습니다.  

이 모듈 클래스는 CLI를 통해 생성이 가능합니다.  
```
ng generate module app-routing --flat --module=app
```

>
```--flat``` 은 그 파일을 소유한 폴더 대신에 ```src/app```에 파일을 집어 넣습니다.  
```--module=app``` 은 ```AppModule```에 등록하라는 옵션입니다.

생성된 파일은 아래와 같습니다:  

### src/app/app-routing.module.ts (generated)
```

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: []
})
export class AppRoutingModule { }
```

일반적으로 라우팅 모듈에 컴포넌트를 정의하지 못합니다.  
```@NgModule.declarations``` 배열에 있는 ```CommonModule```을 지우고 import 부분의 ```CommonModule```도 지워줍니다.  

```RouterModule```과 ```Routes```를 설정하기 위해 ```@angular/router``` 라이브러리를 가져옵니다.  

```@NgModule.exports```를 배열로 추가하고 거기에 ```RouterModule```을 추가합니다.  
추출된 ```RouterModule```은 ```AppModule```의 컴포넌트들이 라이터 지시자(directives)를 사용할 수 있도록 만들어줍니다.  

### src/app/app-routing.module.ts (v1)
```

import { NgModule }             from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

@NgModule({
  exports: [ RouterModule ]
})
export class AppRoutingModule {}
```
---

## Routes 추가
*Routes*는 사용자가 링크를 클릭하거나 URL주소를 브라우저 주소창에 복사했을 때 표시하기 위해 라우터에 알려줍니다.  

Angular에서 ```Route```를 정의하기 위한 두 가지의 키 값을 소개합니다:  

| ```path``` | 브라우저 주소창에 URL로 매칭될 문자열입니다. |
|--|--|
| ```component``` | 특정 라우터가 탐색이 되면 라우터가 생성할 컴포넌트입니다. |
  

이제 ```localhost:4200/heroes```와 같은 URL이 매칭 됐을 때 ```HeroesComponent```를 탐색하도록 작업할 것입니다.  

```Route```와 ```HeroesComponent```의 관계를 맺을 수 있습니다. 그러려면 1차원 배열의 ```route```변수를 정의해야 합니다.  

```
import { HeroesComponent }      from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
```
  

셋팅이 끝났습니다. 라우터는 ```path: 'heores'``` URL에 매칭될 것이고 ```HeroesComponent```를 보여줄 것입니다.  


## *RouterModule.forRoot()*
처음에 반드시 라우터를 초기화 시켜야 하고 라우터가 브라우저 주소 변화를 감지할 수 있도록 시작해야 합니다.  

```@NgModule.imports```에 ```RouterModule```를 추가합니다. 그리고 ```RouterModule.forRoot()```를 호출하고 ```routes```집어 넣습니다:  
```
imports: [ RouterModule.forRoot(routes) ],
```
>
라우터를 어플리케이션의 루트 레벨에서 구성하기 때문에 forRoot()를 호출한 것입니다.  
forRoot() 메소드는 라우팅이 필요한 서비스 제공자(service providers)와 지시자(directive)를 지원합니다. 그리고 현재 브라우저 URL에 기반한 초기 네비게이션을 수행합니다.(?)
  

## *RouterOutlet* 추가
```AppComponent```템플릿을 열고 ```<app-heroes>```를 ```<router-outlet>```로 변경합니다.  

### src/app/app.component.html (router-outlet)
```

<h1>{{title}}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
```
  

사용자가 방문했을 때 오직 ```HeroesComponent```를 보여주기 위해 ```<app-heroes>```를 제거했습니다.  

```<router-outlet>```는 라우트될 화면이 표시될 장소를 라우터에 알려줍니다.  

>
```RouterOutlet```은 ```AppComponent```에서 사용할 수 있는 라우터 지시자(directive)중 하나입니다.
  

### 시도해봅시다
우리는 여전히 CLI 명령어로 실행해야 합니다.  

```
ng serve
```
  

브라우저가 새로고침됩니다. 앱 제목은 보이지만 히어로 목록은 보이지 않을 것입니다.  

브라우저 주소를 봐주세요. URL끝에 /가 있습니다. ```HeroesComponent```의 라우트 경로는 ```/heroes``` 입니다.  

```/heroes```를 브라우저 주소창에 붙입니다. 히어로 master/detail 화면과 유사한 화면을 볼 수 있습니다.  


## 네비게이션 링크(*routerLink*) 추가
사용자는 주소창에 라우트 URL을 붙여넣을 수 없습니다. 그들이 클릭해서 링크를 탐색하도록 해야합니다.  

```nav``` 태그를 추가하고 그 안에 ```<a>```태그를 추가합니다. ```AppComponent```의 템플릿이 다음과 같이 보이면 됩니다:  


### src/app/app.component.html (heroes RouterLink)
```

<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```
  
routerLink 속성(https://angular.io/tutorial/toh-pt5#routerlink) 은  ```HeroesComponent```의 라우트에 라우터가 매칭되기 위해 ```"/heroes"```로 설정합니다. ```routerLink```는 RouterLink 지시자(https://angular.io/tutorial/toh-pt5#routerlink) 의 선택자(selector)입니다. ```routerLink```는 ```RouterModule```에 있는 공용 지시자중 하나입니다.  

브라우저가 새로고침 되고 앱 타이틀과 히어로 링크를 보여줍니다. 하지만, 히어로 리스트는 없습니다.  

링크를 클릭하세요. 주소창이 ```/heores```로 바뀌고 히어로 목록이 보이게 됩니다.  

> 이번 링크와 앞으로의 링크가 더욱 잘 보이도록 ```app.component.css```에 CSS 스타일을 추가해주세요. 아래 최종 코드(https://angular.io/tutorial/toh-pt5#appcomponent) 를 참고하여 추가해주세요.
  

## 데시보드화면 추가
라우팅은 다중 화면이 있을 때 적합합니다. 그래서 히어로 화면만을 볼 수 있던 것보다 더 멀리 나가게 될 수 있습니다.

CLI를 사용해서 ```DashboardComponent```를 추가합니다:  
```
ng generate component dashboard
```
  

CLI가 ```DashboardComponent```가 정의될 파일을 생성하고 ```AppModule```에 ```DashboardComponent```가 정의됩니다.  

세 가지 기본 파일의 내용을 다음과 같이 변경합니다. 약간의 토론거리가 있으니 돌아오세요:(?)  


### src/app/dashboard/dashboard.component.html
```

<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <a *ngFor="let hero of heroes" class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>
```


### src/app/dashboard/dashboard.component.ts
```

import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';
 
@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];
 
  constructor(private heroService: HeroService) { }
 
  ngOnInit() {
    this.getHeroes();
  }
 
  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```


### src/app/dashboard/dashboard.component.css
```

/* DashboardComponent's private CSS styles */
[class*='col-'] {
  float: left;
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
a {
  text-decoration: none;
}
*, *:after, *:before {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
h4 {
  position: relative;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
  padding: 20px;
  text-align: center;
  color: #eee;
  max-height: 120px;
  min-width: 120px;
  background-color: #607D8B;
  border-radius: 2px;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
  .module {
    font-size: 10px;
    max-height: 75px; }
}
@media (max-width: 1024px) {
  .grid {
    margin: 0;
  }
  .module {
    min-width: 60px;
  }
}
```
  

템플릿은 히어로 이름 링크의 그리드를 보여주고 있습니다.  

* ```*ngFor```로 컴포넌트에 있는 히어로 배열과 같은 많은 링크를 생성합니다.
* 링크는 ```dashboard.component.css```에서 색으로 구분됩니다.
* 링크는 아직 어디로도 이동하지 않지만 곧 이동할 것입니다.
  

이번 *클래스(class)*는 HeroesComponent 클래스와 유사합니다.  

* ```heroes``` 배열을 정의합니다.
* 생성자(constructor)는 private 변수의 heroService(```HeroService```)가 주입될 것입니다.
* 라이프사이클 훅(hook)인 ```ngOnInit()```은 ```getHeroes```를 호출합니다.
  

```getHeroes```는 화면에 보여질 영웅의 수를 네 번 줄여줍니다.(두 번째, 세 번째, 네 번째, 다섯 번째를 순차적으로).  
```
getHeroes(): void {
  this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes.slice(1, 5));
}
```
  

## 데시보드 라우트를 추가
데시보드 탐색을 위해서 라우터는 적절한 라우트가 필요합니다.  

```AppRoutingModule```에 ```DashboardComponent```를 포함시킵니다.  


### src/app/app-routing.module.ts (import DashboardComponent)
```

import { DashboardComponent }   from './dashboard/dashboard.component';
```
  

```AppRoutingModule.routes```배열에 ```DashboardComponent```가 매칭되는 경로의 라우트를 추가합니다.  
```

{ path: 'dashboard', component: DashboardComponent },

```

## 기본 라우트를 추가
앱이 가동될 때 브라우저 주소창은 웹 사이트의 루트를 가리킵니다. 따라서 존재하는 어떤 라우터에도 매칭되지 않고 탐색하지도 않습니다. 그 공간은 ```<router-outlet>```에 공백으로 존재하게 됩니다.  

데시보드에 자동으로 찾아가게 하려면 ```AppRoutingModule.Routes```에 다음과 같은 라우트를 추가해야 합니다.  

```

{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },

```
  

이 라우트는 빈 경로('')가 완전히 매칭 됐을 때 ```'/dashboard'``` 경로로 리다이렉트(재전송) 합니다.  

브라우저가 새로고침 되고 난 후, 라우터는 ```DashboardComponent```를 로드하고 브라우저 주소창은 ```/dashboard``` URL을 보여주게 됩니다.  

## 쉘(shell)에 데시보드 링크를 추가
사용자는 화면의 위에 있는 탐색영역의 링크를 클릭해서 ```DashboardComponent```와 ```HeroesComponent``` 앞뒤로 갈 수 있어야 합니다.  

```AppComponent``` 쉘(shell) 템플릿에 데시보드 링크를 추가합니다. 단, *히어로* 링크위에 추가해주세요.  

### src/app/app.component.html
```

<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```
  
브라우저가 새로고침 됐습니다. 이제 두 개의 링크를 클릭하여 자유롭게 두 화면을 탐색할 수 있습니다.  


## 히어로 상세 탐색
```HeroDetailsComponent```는 선택된 영웅의 상세정보를 보여줍니다. 그 순간 ```HeroDetailsComponent```는 ```HeroesComponent```의 아래서 보여집니다.  

사용자는 세 가지의 방법으로 이 정보를 얻을 수 있습니다.  

1. 데시보드에 있는 히어로를 클릭했을 경우
2. 히어로 목록에 있는 히어로를 클릭했을 경우
3. 브라우저 주소창에 히어로의 고유 아이디가 있는 링크를 붙여넣기하여 이동했을 경우
  
이번 단계에서는 ```HeroDetailsComponent```로 탐색이 가능하고 ```HeroesComponent```로부터 분리시킬 작업을 할 것입니다.  

## *HeroesComponent*에서 히어로 상세를 제거
사용자가 ```HeroesComponent```에서 히어로 항목을 클릭했을 때, 앱은 히어로 목록 화면 대신 히어로 상세 화면이 있는 ```HeroDetailComponent```를 탐색합니다. 이제 히어로 목록 화면은 더 이상 영웅의 상세 정보가 보이면 안 됩니다.  

```HeroesComponent``` 템플릿(```heroes/heroes.component.html```)을 열고 ```<app-hero-detail>```태그를 제거합니다.  

이제 히어로 항목 클릭은 동작하지 않습니다. 이 문제를 간단하게 고치고(https://angular.io/tutorial/toh-pt5#heroes-component-links) 난 후 ```HeroDetailComponent```로 라우팅이 가능하도록 할 것입니다.  

## *히어로 상세 정보* 라우트 추가
```~/detail/11```과 같은 URL은 사용자가 11이라는 아이디를 가진 *히어로 상세 정보*로 가기 위한 좋은 URL입니다.  

```AppRoutingModule```을 열고 ```HeroDetailComponent```를 가져옵니다.  

### src/app/app-routing.module.ts (import HeroDetailComponent)
```

import { HeroDetailComponent }  from './hero-detail/hero-detail.component';

```
  

그리고 ```AppRoutingModule.routes```에 *히어로 상세 정보* 화면의 경로 패턴이 매칭한 파라미터화된 라우터를 추가합니다.  

```

{ path: 'detail/:id', component: HeroDetailComponent },

```
  
콜론(:)은 특정 히어로 아이디를 나타내는 표시자입니다.  

이 지점에서, 모든 어플리케이션 라우트는 제자리에 있습니다.(?)  


### src/app/app-routing.module.ts (all routes)
```

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes', component: HeroesComponent }
];

```
  

## *DashboardComponent*의 히어로 연결
```DashboardComponent``` 히어로 연결은 지금 하지 않습니다.  

지금은 라우터가 ```HeroDetailComponent```로 가는 라우트를 가지고 있습니다. 데시보드 히어로 링크를 *파리미터화된* 데시보드 라우트로 통하도록 고쳐야합니다.  


### src/app/dashboard/dashboard.component.html (hero links)
```

<a *ngFor="let hero of heroes" class="col-1-4"
    routerLink="/detail/{{hero.id}}">

```
  
당신은 현재 각각의 routerLink(https://angular.io/tutorial/toh-pt5#routerlink) 에 ```hero.id```를 ```*ngFor``` 반복자에 넣어 사용합니다.  

## *HeroesComponent*의 히어로 연결
```HeroesComponent```에 있는 히어로 항목들은 클릭 이벤트가 컴포넌트의 onSelect 메소드로 바인드된 ```<li>``` 태그입니다.  



### src/app/heroes/heroes.component.html (list with onSelect)
```

<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

```
  

Strip the <li> back to just its ```*ngFor```, wrap the badge and name in an anchor element (<a>), and add a routerLink attribute to the anchor that is the same as in the dashboard template


### src/app/heroes/heroes.component.html (list with links)
```

<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>

```
  

목록이 이전에 보였던 것 처럼 만들기 위해서 ```heroes.component.css```를 수정해야합니다. 수정된 스타일은 이 가이드의 아래에 있는 최종 코드 리뷰(https://angular.io/tutorial/toh-pt5#heroescomponent) 에서 확인할 수 있습니다.  

## 죽은 코드를 제거(선택사항)
```HeroesComponent``` 객체가 여전히 동작하는 동안 ```onSelect()``` 메서드와 ```selectedHero``` 맴버 변수는 더 이상 사용하지 않습니다.  

나중을 위해서 정리하는 것이 정말 좋습니다. 이후의 클래스는 죽은코드가 정리된 클래스입니다.  

### src/app/heroes/heroes.component.ts (cleaned up)
```

export class HeroesComponent implements OnInit {
  heroes: Hero[];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}

```

## 라우팅 가능한 *HeroDetailComponent*
이전에 부모 ```HeroesComponent```는 ```HeroDetailComponent.hero``` 맴버 변수를 설정했고 ```HeroDetailComponent```에 히어로를 보여줬습니다.  

```HeroesComponent```는 더이상 어떤것도 하지 않습니다. 지금 라우터는 ```~/detail/11```의 URL에 반응하는 ```HeroDetailComponent```를 만듭니다.  

```HeroDetailComponent```는 *hero-to-display*를 획득 하기 위한 새로운 방법이 필요합니다.  


* 생성된 라우트를 가져오고,
* 라우트에서 아이디를 추출
* ```HeroService```를 통에서 서버에서 히어로를 얻어옵니다
  
다음처럼 추가하세요:  


### src/app/hero-detail/hero-detail.component.ts
```

import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService }  from '../hero.service';

```
  

```ActivatedRoute```, ```HeroService```와 ```Location``` 생성자에 주입합니다:
```

constructor(
  private route: ActivatedRoute,
  private heroService: HeroService,
  private location: Location
) {}

```
  

ActivatedRoute(https://angular.io/api/router/ActivatedRoute) 는 객체화된 ```HeroDetailComponent```의 라우트에 관한 정보를 쥐고 있습니다. 이 컴포넌트는 흥미롭게도 URL로부터 추출된 파라미터를 가지고 있는 라우트 가방입니다. *"id"* 파라미터는 표시할 히어로의 아이디입니다.  

HeroService(https://angular.io/tutorial/toh-pt4) 는 원격 서버로 부터 히어로 데이터를 가져오고 컴포넌트가 그 데이터를 사용하여 *히어로를 표시*할 것입니다.  

로케이션(https://angular.io/api/common/Location) 은 브라우저와 교류하기 위한 Angular 서비스입니다. 나중에 여기로 오기전 화면으로 돌아가기 위해 사용할 것입니다.  


## 라우터 파리미터에서 *id*를 추출
라이프사이클 훅(hook)인 ```ngOnInit()```에서 ```getHero()```를 호출하고 다음과 같이 정의합니다.

```

ngOnInit(): void {
  this.getHero();
}

getHero(): void {
  const id = +this.route.snapshot.paramMap.get('id');
  this.heroService.getHero(id)
    .subscribe(hero => this.hero = hero);
}

```
  
```route.snapshot```은 컴포넌트가 만들어진 후 라우트 정보의 정적인 이미지입니다.  

```paramMap```은 URL로 부터 추출된 라우터 파리미터 값의 모음집입니다. 키 ```"id"```는 가져올 히어로의 아이디를 반환합니다.  

라우터 파리미터는 항상 문자열(string)입니다. JavaScript (+) 연산자는 히어로 아이디가 되어야 하는 문자열 파라미터를 숫자로 변경합니다.  

브라우저가 새로고침되고 앱은 컴파일 에러를 뱉어냅니다. ```HeroService```가 ```getHero()``` 메소드를 가지고 있지 않기 때문입니다. 지금 바로 추가해보죠.  

## *HeroService.getHero()* 추가
```HeroService```열고 ```getHero()``` 메소드를 추가합니다.  


### src/app/hero.service.ts (getHero)
```

getHero(id: number): Observable<Hero> {
  // Todo: send the message _after_ fetching the hero
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(HEROES.find(hero => hero.id === id));
}

```

> ```id```를 임베딩하기 위한 JavaScript *템플릿 리터럴*(https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 을 정의하는 backticks( \` )를 사용했습니다.
  

getHeroes()(https://angular.io/tutorial/toh-pt4#observable-heroservice) 처럼 ```getHero()```는 비동기 함수입니다. RxJS의 ```of()``` 함수를 사용하여 ```Observable```로 *mock(목)* 히어로를 반환합니다.  

당신은 ```getHero()```를 실제 ```Http``` 요청으로 ```getHero()```를 호출하는 ```HeroDetailComponent```를 변경하지 않고 다시 구현이 가능합니다.  


## 시도해봅시다
브라우저가 새로고침 되고 앱은 다시 작동할 것입니다. 데시보드나 히어로 목록에 있는 히어로를 클릭할 수 있고 히어로의 상세 화면을 탐색할 수 있습니다.  

만약 ```localhost:4200/detail/11```을 브라우저 주소창에 붙여 넣는다면 라우터는 ```id: 11```, "Mr. Nice"의 정보를 가진 히어로의 상세 정보 화면으로 이동할 것입니다.  


## 뒤로 돌아갈 길을 찾자
브라우저에서 뒤로가기 버튼을 클릭해서 상세 정보 화면으로 이동한 경로에 따라 히어로의 목록이나 데시보드 화면으로 이동할 수 있습니다.  

```HeroDetail``` 화면에 뒤로 가기가 가능한 버튼이 있다면 좋을 것 같습니다.  

컴포넌트 템플릿의 아래에 *뒤로가기*버튼을 추가하고 컴포넌트의 ```goBack()```에 바인딩 시킵니다.  


### src/app/hero-detail/hero-detail.component.html (back button)
```

<button (click)="goBack()">go back</button>

```
  
컴포넌트 객체에 ```goBack()``` 메소드를 추가합니다. ```goBack()```는 아까전에 주입했던(https://angular.io/tutorial/toh-pt5#hero-detail-ctor) ```Location``` 서비스를 사용하여 브라우저 히스토리 스택에 있는 한 단계 이전으로 탐색하도록 합니다.  


### src/app/hero-detail/hero-detail.component.ts (goBack)
```

goBack(): void {
  this.location.back();
}

```
  

브라우저가 새로고침되고 클릭을 시작합시다. 사용자들은 앱 곳곳을 탐색할 수 있습니다.  

이번 단계에서 탐색에 관한 모든 것을 만나보았습니다.  


## 최종 코드 리뷰
여기에는 이번 과정에서 의견을 나눈 코드 파일들이 있습니다. 그리고 이번 단계에 진행한 앱과 같은(혹은 비슷한) live example(https://angular.io/generated/live-examples/toh-pt5/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt5/toh-pt5.zip) 이 있습니다.

### *AppRoutingModule*과 *AppModule*


### src/app/app-routing.module.ts
```

import { NgModule }             from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { DashboardComponent }   from './dashboard/dashboard.component';
import { HeroesComponent }      from './heroes/heroes.component';
import { HeroDetailComponent }  from './hero-detail/hero-detail.component';

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule {}

```
---

### src/app/app.module.ts
```

import { NgModule }       from '@angular/core';
import { BrowserModule }  from '@angular/platform-browser';
import { FormsModule }    from '@angular/forms';

import { AppComponent }         from './app.component';
import { DashboardComponent }   from './dashboard/dashboard.component';
import { HeroDetailComponent }  from './hero-detail/hero-detail.component';
import { HeroesComponent }      from './heroes/heroes.component';
import { HeroService }          from './hero.service';
import { MessageService }       from './message.service';
import { MessagesComponent }    from './messages/messages.component';

import { AppRoutingModule }     from './app-routing.module';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    AppRoutingModule
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroesComponent,
    HeroDetailComponent,
    MessagesComponent
  ],
  providers: [ HeroService, MessageService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }

```
---

### *AppComponent*


### src/app/app.component.html
```

<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>

```
---

### src/app/app.component.css
```

/* AppComponent's private CSS styles */
h1 {
  font-size: 1.2em;
  color: #999;
  margin-bottom: 0;
}
h2 {
  font-size: 2em;
  margin-top: 0;
  padding-top: 0;
}
nav a {
  padding: 5px 10px;
  text-decoration: none;
  margin-top: 10px;
  display: inline-block;
  background-color: #eee;
  border-radius: 4px;
}
nav a:visited, a:link {
  color: #607D8B;
}
nav a:hover {
  color: #039be5;
  background-color: #CFD8DC;
}
nav a.active {
  color: #039be5;
}

```
---

### *DashboardComponent*


### src/app/dashboard/dashboard.component.html
```

<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <a *ngFor="let hero of heroes" class="col-1-4"
      routerLink="/detail/{{hero.id}}">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>

```
---

### src/app/dashboard/dashboard.component.ts
```

import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}

```
---

### src/app/dashboard/dashboard.component.css
```

/* DashboardComponent's private CSS styles */
[class*='col-'] {
  float: left;
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
a {
  text-decoration: none;
}
*, *:after, *:before {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
h4 {
  position: relative;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
  padding: 20px;
  text-align: center;
  color: #eee;
  max-height: 120px;
  min-width: 120px;
  background-color: #607D8B;
  border-radius: 2px;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
  .module {
    font-size: 10px;
    max-height: 75px; }
}
@media (max-width: 1024px) {
  .grid {
    margin: 0;
  }
  .module {
    min-width: 60px;
  }
}

```

### *HeroesComponent*


### src/app/heroes/heroes.component.html
```

<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>

```
---

### src/app/heroes/heroes.component.ts
```

import { Component, OnInit } from '@angular/core';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes: Hero[];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}

```
---

### src/app/heroes/heroes.component.css
```

/* HeroesComponent's private CSS styles */
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  position: relative;
  cursor: pointer;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}

.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}

.heroes a {
  color: #888;
  text-decoration: none;
  position: relative;
  display: block;
  width: 250px;
}

.heroes a:hover {
  color:#607D8B;
}

.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #607D8B;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  min-width: 16px;
  text-align: right;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}

```
---

### *HeroDetailComponent*


### src/app/hero-detail/hero-detail.component.html
```

<div *ngIf="hero">
  <h2>{{ hero.name | uppercase }} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name"/>
    </label>
  </div>
  <button (click)="goBack()">go back</button>
</div>

```
---

### src/app/hero-detail/hero-detail.component.ts
```

import { Component, OnInit, Input } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { Hero }         from '../hero';
import { HeroService }  from '../hero.service';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: [ './hero-detail.component.css' ]
})
export class HeroDetailComponent implements OnInit {
  @Input() hero: Hero;

  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private location: Location
  ) {}

  ngOnInit(): void {
    this.getHero();
  }

  getHero(): void {
    const id = +this.route.snapshot.paramMap.get('id');
    this.heroService.getHero(id)
      .subscribe(hero => this.hero = hero);
  }

  goBack(): void {
    this.location.back();
  }
}

```
---

### src/app/hero-detail/hero-detail.component.css
```

/* HeroDetailComponent's private CSS styles */
label {
  display: inline-block;
  width: 3em;
  margin: .5em 0;
  color: #607D8B;
  font-weight: bold;
}
input {
  height: 2em;
  font-size: 1em;
  padding-left: .4em;
}
button {
  margin-top: 20px;
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer; cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}

```
  

## 요약
* 당신은 다른 컴포넌트 사이를 탐색하기 위해 Angular 라우터를 추가했습니다.
* 당신은 ```AppComponent```에 있는 ```<a>``` 링크를 ```<router-outlet>```로 변경했습니다.
* 당신은 ```AppRoutingModule```에 라우터를 설정했습니다.
* 당신은 재전송 라우트인 심플 라우트를 정의했고 파리미터화된 라우트를 정의했습니다.
* 당신은 링크 연결에 ```routerLink``` 자시자(directive)를 사용했습니다.
* 당신은 강하게 결합된 마스터/상세 화면을 라우트된 상세화면으로 리팩토링 했습니다.
* 당신은 사용자가 선택한 히어로의 상세 정보 화면을 탐색하기 위해 라우터 링크 파라미터를 사용했습니다.
* 당신은 여러 컴포넌트 사이에 ```HeroService```를 공유했습니다.
