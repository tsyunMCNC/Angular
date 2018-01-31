# What changed?
---
그 순간, HeroesComponent는 heroes의 리스트와 선택된 hero의 detail을 동시에 보여줍니다.

어플리케이션이 커져감에 따라, 한 컴포넌트(구성요소)에서 모든 기능을 유지하는 것은 유지보수 할 수 없습니다.

당신은 큰 컴포넌트들을 작은 서브 컴포넌트로 분리하기를 원하고, 각각은 특별한 기능이나 workflow에 집중되어 있습니다.

이 페이지에서 당신은 hero details를 별도로, 재사용 할 수 있는 HeroDetailsComponent로 옮기는 방향으로의 첫번째 단계를 거칩니다.  

HeroesComponent는 지금 heros들의 목록입니다. HeroDetailsComponent는 지금 선택된 hero의 디테일이구요.


## Make the HeroDetailComponent
Angular CLI로 hero-detail이라는 새 컴포넌트를 만드세요.

이 명령은 HeroDetailComponent file의 발판이며, AppModule에서 컴포넌트를 선언합니다.


## Write the template
HeroesComponent template 아래에 있는 hero detail을 잘라 붙여넣어서, HeroDetailComponent template의 boilerplate를 만드세요

붙인 HTML은 selectedHero를 참고합니다. 새 HeroDetailComponent는 선택된 hero가 아닌 다른 어느 hero를 표현할 수 있습니다. 그러니 이 템플릿에 hero를 selectedHero로 대체하세요.

다했으면, the HeroDetailComponent template는 이렇게 보여줄거에요

#### src/app/hero-detail/hero-detail.component.html
```
<div *ngIf="hero">

  <h2>{{ hero.name | uppercase }} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name"/>
    </label>
  </div>

</div>
```


## Add the ```@Input()``` hero property
HeroDetailComponent template은 Hero 유형을 나타내는 컴포넌트의 hero 속성에 바인딩된다. 

HeroDetailComponent class file을 열고 Hero symbol을 import 하세요

#### src/app/hero-detail/hero-detail.component.ts (import Hero)
```
import { Hero } from '../hero';
```

hero 속성은 ```@Input()``` decorator로 주석이 달린 input 속성이어야 합니다. 외부의 HeroesComponent는 아래처럼 바인딩하기 때문에요.

```
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

```@angular/core``` import statement을 수정해서 input symbol을 포함하세요. 

#### src/app/hero-detail/hero-detail.component.ts (import Input)
```
import { Component, OnInit, Input } from '@angular/core';
```

```@Input()``` decorator 앞에 hero 속성을 더하세요.

```
@Input() hero: Hero;
```

이건 당신이 HeroDetailComponent class에 대한 유일한 변경사항이에요.

어느 속성도 없고, 어떤 로직도 없어요. 이 컴포넌트는 hero 속성을 통해 단순히 하나의 hero object를 받고, 보여줍니다.  


## Show the HeroDetailComponent
HeroesComponent는 여전히 master/detail 구조에요.

이건 당신이 템플릿의 해당부분을 자르기 전에는 어떤 hero detail를 독자적으로 표시해주도록 사용됩니다. 이제 HeroDetailComponent에 위임합니다.

두 컴포넌트는 부모/자식 관계를 갖습니다. 부모 HeroesComponent는 유저가 list의 어떤 hero를 선택할 때 마다 새 hero를 보내서 자식 HeroesDetailComponent을 컨트롤합니다. 

너는 HeroesComponent class를 변경하지는 않지만, 그 템플릿을 변경해야 합니다.

## Update the HeroesComponent template
HeroDetailComponent selector는 'app-hero-detail입니다. <app-hero-detail> 요소를 HeroesComponent template 아래에 붙이세요, hero detail view가 있어야 합니다. 

아래처럼 HeroesComponent.selectedHero를 각 요소의 hero 속성에 바인딩하세요

#### heroes.component.html (HeroDetail binding)
```
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```


[hero]="selectedHero"는 Angular 속성바인딩입니다.

HeroComponent의 selectedHero 속성으로부터 HeroDetialComponent의 hero속성에 매핑된 타겟 요소의 hero 속성으로의 단방향 데이터 바인딩입니다. 

이제 유저가 리스트의 hero를 클릭할 때, selectedHero는 바뀝니다. seletedHero가 바뀌면, 속성 바인딩은 영웅을 업데이트 하고, HeroDetailComponent는 새 hero를 보여줍니다.

수정된 HeroesComponent template은 다음과 같아야 합니다.

#### heroes.component.html
```
<h2>My Heroes</h2>

<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```


브라우저는 refresh되고, 앱은 이전에 그랬던 것처럼 다시 시작됩니다.


## What changed?
이전처럼, 유저가 hero이름을 클릭할 때마다, hero detail은 hero list 아래 나타납니다. 이제 HeroDetailComponent는 HeroesComponent 대신에 그 detail들을 보여줍니다.

원래의 HeroesComponent를 두개의 component로 재구성하면 얻는 이득은 현재와 미래입니다.

책임을 줄임으로써 HeroesComponent를 단순화했습니다.

부모 HeroesComponent를 만지지 않고 rich hero editor로 HeroDetailComponent를 개선할 수 있습니다.

hero detail view를 만지지 않고 HeroesComponent를 개선할 수 있습니다.

some future component의 템플릿에서 HeroDetailComponent를 재사용할 수 있습니다.

## Final code review
이 페이지에서 언급한 코드파일들이 있습니다. 그리고 당신의 앱은 이 예시와 같아야합니다. 


## Summary
* 당신은 분리된, 재사용 가능한 HeroDetailComponent를 만들었습니다.
* 당신은 속성바인딩을 사용했습니다. 부모 HeroesComponent control에서 자식 HeroDetailComponent을 컨트롤 할 수 있도록
* 당신은 ```@Input``` decorator를 사용했습니다. hero 속성을 외부의 heroesComponent가 바인딩해서 사용할 수 있도록