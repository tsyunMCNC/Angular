# 히어로 목록을 보여주기
---
이번 단계에서는 Tour of Heroes 앱에 히어로 목록을 보여주기 위해 확장하는 작업을 할 것입니다. 그리고 사용자가 히어로 선택할 수 있도록 허용 하고 선택한 히어로의 상세 정보를 보여줄 것입니다.  

## 목(mock) 히어로 생성
목록을 보여주기 위해서는 몇몇 히어로들이 필요합니다.  

결국에는 원격 서버로부터 데이터를 받아오게 되겠지만, 지금은 몇 가지 *목(mock) 히어로*를 생성하여 데이터를 서버에서 받아왔다고 가정한 체로 진행할 것입니다.  

> 목(Mock)?  
  목(mock)은 임의로 사용할 데이터를 의미합니다.
  
```src/app``` 폴더에 ```mock-heroes.ts```를 만들어주세요. ```HEORES```라는 상수값으로 정의합니다. 열 명의 히어로가 들어가도록 배열로 정의해주세요. 그리고 HEORES를 export합니다. 생성한 파일은 아래와 같습니다.  

#### src/app/mock-heroes.ts
```
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```
  

### 히어로들을 보여주자
```HeroesComponent```의 상단에 히어로 목록을 보여주도록 하죠.  

```HeroesComponent``` 클래스 파일을 열고 목 ```HEROES```를 가져옵니다.  

#### src/app/heroes/heroes.component.ts (import HEROES)
```
import { HEROES } from '../mock-heroes';
```
  
클래스에 ```heroes```라는 이름의 맴버변수를 추가합니다. ```heroes```는 히어로의 목록을 바인딩 하기 위해 필요합니다.  

```
heroes = HEROES;
```
  

### *\*ngFor*과 히어로 목록
```HeroesComponent``` 템플릿 파일을 열고 다음과 같이 수정해주세요:  

* ```<h2>```태그를 맨 위에 추가합니다.
* 그 아래에 비순차목록 태그인 ```<ul>```태그를 추가하세요.
* ```<ul>``` 안에 ```<li>```를 삽입합니다. ```<li>```는 히어로의 속성들이 표시됩니다.
* 스타일링을 위한 몇 몇 CSS 클래스들을 정의합니다.(곧 CSS 스타일에 추가할 것입니다.)
  
다음과 같이 만드세요:
#### heroes.component.html (heroes template)
```
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
  
지금 ```<li>```를 다음과 같이 변경해주세요:

```
<li *ngFor="let hero of heroes">
```
  
*\*ngFor*(https://angular.io/guide/template-syntax#ngFor) 은 Angular의 반복 지시자(directive)입니다. 목록에 있는 각각의 엘리먼트에 호스트 엘리먼트를 반복시켜줍니다.  

다음 예시를 보세요  

* ```<li>```는 호스트 엘리먼트입니다.
* ```heroes```는 ```HeroesComponent``` 클래스로부터 가져온 배열입니다.
* ```hero```는 배열을 통해 각각의 반복된 현재 히어로의 객체를 쥐고 있습니다.
  

> ```ngFor```앞에 아스트릭 기호(\*)를 붙이는 것을 잊지 마세요! 문법 에러에 치명적인 부분 중 하나입니다.
  
브라우저가 새로고침되고 나면 히어로 목록이 나타납니다.  


### 히어로 스타일
히어로 목록은 매력적이어야 하고 목록에 있는 히어로를 마우스 오버하고 선택 했을 때 외관상 반응해야 합니다.  

첫 번째 튜토리얼에서, 기본 스타일을 어플리케이션에 있는 ```styles.css```에 설정했습니다. 그 스타일시트에는 히어로 목록을 위한 스타일은 포함되어 있지 않습니다.  

```styles.css```에 더 많은 스타일을 추가할 수 있고 컴포넌트를 추가할 때 스타일시트를 계속 늘릴 수 있습니다.  

우리는 특정 컴포넌트에 대한 스타일을 정의하는 것과 하나의 컴포넌트에 필요한 모든 것(코드, HTML, CSS)을 한 곳에서 유지하는 것을 선호합니다.  

이러한 접근은 컴포넌트를 어디서든 재사용하고 컴포넌트의 의도된 화면을 전역 스타일이 다른 경우에도 마찬가지로 전달하기 쉽게 만듭니다.  

컴포넌트 전용 스타일을 ```@Component.styles``` 배열에 정의하거나 스타일시트 파일로 구분할 경우에는 ```@Component.styleUrls``` 배열에 정의합니다.  

CLI가 ```HeroesComponent```를 생성했을 때, 빈 ```heroes.component.css```를 생성했습니다. 그리고 다음과 같이 ```@Component.styleUrls```에 들어가 있습니다.  

#### src/app/heroes/heroes.component.ts (@Component)
```
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
```
  
```heroes.component.css``` 파일을 열고 ```HeroesComponent```에 적용할 전용 CSS 스타일을 붙여 넣습니다. 아래서 안내하고 있는 최종 코드 리뷰에 붙여넣을 파일을 찾을 수 있습니다.  

```@Component``` 메타데이터로 분류되는 스타일과 스타일시트는 영향 범위가 현재 작업중인 컴포넌트로 제한 됩니다. ```heroes.component.css``` 스타일은 오직 ```HeroesComponent```에만 적용되고 다른 바깥 HTML이나 다른 컴포넌트의 HTML에는 영향을 미치지 않습니다.  

---

## Master/Detail
사용자가 마스터 목록에서 히어로를 클릭했을 때, 컴포넌트는 해당 히어로의 상세 정보를 화면 아래에 보여줘야 합니다.  

이번 단계에서는 히어로 항목 클릭 이벤트 처리와 히어로 상세의 갱신작업에 관해 다룰 것입니다.  

### 클릭 이벤트 바인딩 추가
```<li>```에 다음과 같이 클릭 이벤트 바인딩을 추가합니다:  

#### heroes.component.html (template excerpt)
```
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
  
이번 예제는 Angular의 이벤트 바인딩(https://angular.io/guide/template-syntax#event-binding) 문법에 관한 것입니다.  

소괄호가 둘러 싸고 있는 ```click```은 ```<li>```의 ```click``` 이벤트를 Angular가 반응할 수 있도록 전달합니다. 사용자가 ```<li>```를 클릭했을 때, Angular는 구문안에 있는 ```onSelect(hero)```를 실행합니다.  

```onSelect()```는 ```HeroesComponent```에 있는 메소드입니다. Angular는 클릭된 ```<li>```에 표시되는 ```hero``` 객체를 호출 합니다. ```hero``` 객체는 이전에 ```*ngFor``` 구문식에서 사용한 ```hero```와 같습니다.  

### 클릭 이벤트 핸들러 추가
컴포넌트의 ```hero``` 맴버변수를 ```selectedHero```로 바꿔주세요. 하지만 값을 할당하지는 마세요. 왜냐하면 어플리케이션이 시작됐을 때는 *선택된 히어로*가 없으니까요.  

```onSelect()``` 메소드를 다음과 같이 추가하고 템플릿에서 클릭된 ```hero```를 ```selectedHero```에 할당해주세요.  

#### src/app/heroes/heroes.component.ts (onSelect)
```
selectedHero: Hero;

onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```
  

### details 템플릿을 갱신
여전히 템플릿은 더 이상 존재하지 않는 ```hero``` 맴버변수를 가리키고 있습니다. ```hero```를 ```selectedHero```로 바꿔주세요.  

#### heroes.component.html (selected hero details)
```
<h2>{{ selectedHero.name | uppercase }} Details</h2>
<div><span>id: </span>{{selectedHero.id}}</div>
<div>
  <label>name:
    <input [(ngModel)]="selectedHero.name" placeholder="name">
  </label>
</div>
```
---

## ```*ngIf```로 빈 상세 정보 숨기기
브라우저가 새로고침 되고 난 후 어플리케이션은 박살납니다.  

브라우저 개발자도구를 열고 콘솔탭에서 다음과 같은 애러 메세지를 봐주세요:  

```
HeroesComponent.html:3 ERROR TypeError: Cannot read property 'name' of undefined
```
  
지금 목록중 하나를 클릭 하세요. 앱은 다시 정상작동 하는 것처럼 보일 것입니다. 히어로들이 목록에 보일 것이고 상세에는 선택한 히어로가 화면 아래에 보일 것입니다.  

### 무엇이 문제였나?
앱이 시작됐을 때, ```selectedHero```는 undefined로 *설계*됐습니다.  

템플릿에 바인딩 문법은 ```selectedHero```의 속성(```{{selectedHero.name}}```)을 가리키고 있습니다. 선택된 히어로가 없으니 *에러가 당연히* 발생하게 됩니다.  

### 고쳐보죠
컴포넌트는 ```selectedHero```가 존재할 경우에만 선택된 히어로의 상세 정보를 보여줘야 합니다.  

```<div>``` 태그로 히어로 상세 정보 HTML을 감싸줍니다. 그리고 Angular의 지시자인 ```*ngIf```를 ```<div>```에 추가하고 거기에 ```selectedHero```를 대입합니다.  

> ```ngIf```앞에 아스티릭기호(\*)를 붙이는 것을 잊지 마세요! 문법 에러에 치명적인 부분 중 하나입니다.
  

#### src/app/heroes/heroes.component.html (\*ngIf)
```
<div *ngIf="selectedHero">

  <h2>{{ selectedHero.name | uppercase }} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="selectedHero.name" placeholder="name">
    </label>
  </div>

</div>
```
  
브라우저가 새로고침됩니다. 이름 목록이 다시 보일것입니다. 상세 영역은 빈 화면으로 나옵니다. 히어로를 클릭하면 상세 정보가 나타납니다.  


### 왜 작동할까
```selectedHero```가 undefined일 때, ```ngIf```는 히어로 상세 정보 DOM을 제거합니다. ```selectedHero```가 바인딩 되지 않아도 걱정없습니다.  

사용자가 히어로를 선택하면 ```selectedHero```가 값을 갖게 되고 ```ngIf```는 히어로 상세 정보 DOM을 갖다 놓습니다.  

### 선택된 히어로 스타일주기
모든 ```<li>```가 비슷하게 생겨서 목록에서 *선택된 히어로*를 분별하기 어렵습니다.  

만약 사용자가 "Magneta"를 클릭한다면 히어로는 특이하게 그려집니다. 다음과 같이 교묘한 배경색을 가지고 말이죠:  

![heroes-list-selected](https://angular.io/generated/images/guide/toh/heroes-list-selected.png)
  

*선택된 히어로*의 컬러링은 CSS 클래스인 ```.selected```가 적용되도록 일찍이(https://angular.io/tutorial/toh-pt2#styles) 추가 해뒀습니다. 이제 할 일은 단순히 ```<li>```를 사용자가 클릭했을 때 ```.selected```를 적용하는 것입니다.  

```Angular``` 클래스 바인딩(https://angular.io/guide/template-syntax#class-binding) 은 CSS 클래스를 조건부에 따라 쉽게 추가하고 제거하도록 합니다. ```[class.some-css-class]="some-condition"```를 스타일을 주고 싶은 태그에 추가하기만 하면 됩니다.  

다음과 같이 ```[class.selected]```를 ```HeroesComponent``` 템플릿에 있는 ```<li>``` 태그에 추가합니다:  

#### heroes.component.html (toggle the 'selected' CSS class)
```
[class.selected]="hero === selectedHero"
```
  
현재 선택한 히어로가 ```selectedHero```와 같을 때, Angular는 ```selected``` CSS 클래스를 추가해줍니다. 두 히어로 객체가 다르다면 클래스를 제거해줍니다.  

최종적으로 ```<li>```태그는 다음과 같습니다:  

#### heroes.component.html (list item hero)
```
<li *ngFor="let hero of heroes"
  [class.selected]="hero === selectedHero"
  (click)="onSelect(hero)">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
```
---

## 최종 코드 리뷰
이번에 작업한 앱과 같은 live example(https://angular.io/generated/live-examples/toh-pt2/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt2/toh-pt2.zip) 여기에 있습니다.  

여기에는 이번 단계에서 논의한 코드 파일들이 있습니다.(```HeroesComponent``` 스타일 포함)  

#### src/app/heroes/heroes.component.ts
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HEROES } from '../mock-heroes';
 
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
 
  heroes = HEROES;
 
  selectedHero: Hero;
 
 
  constructor() { }
 
  ngOnInit() {
  }
 
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
}
```
---

#### src/app/heroes/heroes.component.html
```
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

<div *ngIf="selectedHero">

  <h2>{{ selectedHero.name | uppercase }} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="selectedHero.name" placeholder="name">
    </label>
  </div>

</div>
```
---

#### src/app/heroes/heroes.component.css
```
/* HeroesComponent's private CSS styles */
.selected {
  background-color: #CFD8DC !important;
  color: white;
}
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes li.selected:hover {
  background-color: #BBD8DC !important;
  color: white;
}
.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.heroes .text {
  position: relative;
  top: -3px;
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
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
```
---

## 요약
* Tour of Heroes 앱은 Master/Detail 에서 히어로 목록을 보여줍니다.
* 사용자는 히어로를 선택할 수 있고 그 히어로의 상세 정보를 볼 수 있습니다.
* 당신은 ```*ngFor```를 사용하여 목록을 보여줬습니다.
* 당신은 ```*ngIf```를 사용하여 조건부로 HTML 블록을 추가/제거 하였습니다.
* 당신은 클래스 바인딩으로 CSS 스타일 클래스를 토글할 수 있습니다.
