# HTTP
---
이번 튜토리얼은 Angular의 ```HttpClient```의 도움을 받아 데이터 지속 기능을 추가할 것입니다.  

* ```HeroService```는 HTTP 리퀘스트로 히어로 데이터를 얻을 것입니다.
* 사용자는 히어로를 추가, 편집, 삭제할 수 있고 이러한 변경 작업을 HTTP로 저장할 수 있습니다.
* 사용자는 이름으로 히어로를 검색할 수 있습니다.

이번 단계가 끝났을 때, 앱은 live example(https://angular.io/generated/live-examples/toh-pt6/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt6/toh-pt6.zip) 과 같아야 합니다.  


## HTTP 서비스 활성화
```HttpClient```는 HTTP로 원격 서버와 커뮤니케이팅 하기 위한 Angular의 메커니즘입니다.  

앱 내에서 ```HttpClient```를 활성화 하기 위해서는,

* ```AppModule```을 여세요,
* ```@angular/common/http```에서 ```HttpClientModule```를 가져오세요,
* 가져온 것을 ```@NgModule.imports``` 배열에 넣으세요.
  
---

## 데이터 서버 시뮬레이트
이번 튜토리얼은 *In-memory Web API*(https://github.com/angular/in-memory-web-api) 모듈을 사용하여 원격 데이터 서버를 흉내낼 샘플을 다룰 것입니다.  

모듈을 설치한 후, 앱은 ```HttpClient```로부터 요청과 응답 받는 것을 작업할 것입니다. 하지만, 보통 아는 것과는 다르게 *In-memory Web API*가 데이터 리퀘스트를 전송해주고 in-memory 데이터 스토어에 적용하고 시뮬레이트 된 응답을 반환해 줄 것입니다.  

이러한 시설은 튜토리얼을 위해 아주 편리한 것입니다. 우리는 서버를 별도로 설치하지 않고 ```HttpClient```에 대해 배울 것입니다.  

이런 작업은 서버의 웹 api가 아직 정해지 않았거나 작업되지 않았을 때 앱 개발을 쉽고 편리하게 만들어 줍니다.  

> 중요사항: *In-memory Web API* 모듈은 Angular에 있는 HTTP 동작이 아닙니다.  
만약 이번 튜토리얼에서 ```HttpClient```만을 배우고 싶다면, 이번 단계를 스킵하여 넘어가세요.(https://angular.io/tutorial/toh-pt6#import-heroes) 이번 튜토리얼을 따라 코딩 하고 싶다면 여기서 머물고 *In-memory Web API*를 추가하세요.
  
npm으로 *In-memory Web API* 패키지를 설치합니다.  
```
npm install angular-in-memory-web-api --save
```
  

```InMemoryWebApiModule```과 ```InMemoryDataService``` 클래스를 추가하세요.  

#### src/app/app.module.ts (In-memory Web API imports)
```
import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';
```
  
```InMemoryWebApiModule```을 ```@NgModule.imports``` 배열에 추가(가져온 ```HttpClient``` 아래에)하고 거기에 ```InMemoryDataService```을 설정합니다.  

```
HttpClientModule,

// The HttpClientInMemoryWebApiModule module intercepts HTTP requests
// and returns simulated server responses.
// Remove it when a real server is ready to receive requests.
HttpClientInMemoryWebApiModule.forRoot(
  InMemoryDataService, { dataEncapsulation: false }
)
```
  
```forRoot()``` 설정 메소드는 ```InMemoryDataService``` 클래스가 준비한 in-memory DB를 가져옵니다.  

*Tour of Heroes* 샘플은 각각의 다음과 같은 소스 코드를 가진 ```src/app/in-memory-data.service.ts```를 생성합니다.  

#### src/app/in-memory-data.service.ts
```
import { InMemoryDbService } from 'angular-in-memory-web-api';

export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
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
    return {heroes};
  }
}
```
  
이 파일은 ```mock-heroes.ts``` 대체합니다. 그리고 이제 ```mock-heroes.ts```는 삭제하는 것이 안전합니다.  

*In-memory Web API*가 붙을 서버를 준비하고 앱이 서버로 요청을 보낼 것입니다.  

이제 다시 ```HttpClient``` 이야기로 돌아오죠.  

---

## 히어로와 HTTP
앞으로 필요할 몇 가지 HTTP 심볼을 가져옵니다:  

#### src/app/hero.service.ts (import HTTP symbols)
```
import { HttpClient, HttpHeaders } from '@angular/common/http';
```
  
```HttpClient```를 constructor에 넣습니다. 비공개 맴버변수인 http로 사용할 것입니다.  

```
constructor(
  private http: HttpClient,
  private messageService: MessageService) { }
```

```MessageService```는 유지하세요. ```MessageService```는 비공개 로그 메소드로 빈번하게 호출할 것입니다.  

```
/** Log a HeroService message with the MessageService */
private log(message: string) {
  this.messageService.add('HeroService: ' + message);
}
```
  
서버에서 히어로 자원을 가져올 주소인 ```heroesUrl```을 정의하세요.  

```
private heroesUrl = 'api/heroes';  // URL to web api
```
  

### *HttpClient*로 히어로 가져오기
현재 ```HeroService.getHeroes()```는 RxJS의 ```of()``` 함수를 사용하여 목(mock) 히어로들을 ```Observable<Hero[]>```로 반환해주고 있습니다.  

#### src/app/hero.service.ts (getHeroes with RxJs 'of()')
```
getHeroes(): Observable<Hero[]> {
  return of(HEROES);
}
```
  
```HttpClient```를 사용하는 메소드로 변경합니다.  

```
/** GET heroes from the server */
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
}
```
  
브라우저가 새로고침 됩니다. 히어로 데이터는 목(mock) 서버로부터 성공적으로 불러와야 합니다.  

함수의 리턴이 ```Observable<Hero[]>```로 앱이 작동하는 것이 유지 되야하기 때문에 ```http.get```의 타입을 강제로 변환해야합니다.  


### Http 메소드는 하나의 값을 반환
모든 ```HttpClient``` 메소드는 RxJS에 있는 ```Observable```의 무언가를 반환합니다.  

HTTP는 리퀘스트/리스폰스 프로토콜 입니다. 리퀘스트는 오직 하나의 리스폰스만 반환합니다.  

보통, ```Observable```는 여러 값들을 *반환 할 수 있습니다.* ```HttpClient```의 ```Observable```는 항상 하나의 값만 배출하고 끝이 납니다. 절대 다시 배출 하지 않습니다.  

이번에는 특이하게 ```HttpClient.get``` 호출이 ```Observable<Hero[]>```를 반환하는데, 문자 그대로 "*히어로 배열이 있는 하나의 observable*"입니다. 즉, 오직 하나의 히어로 배열만을 반환하는 것입니다.  


### *HttpClient.get*는 리스폰스 데이터를 반환
```HttpClient.get```는 기본적으로 JSON 객체의 리스폰스 바디(body)를 반환해줍니다. 특정 타입(```<Herop[]>``` 같은)을 선택적으로 적용하는 것은 타입이 있는 객체를 반환하기 위함입니다.  

JSON 데이터의 모양은 서버의 데이터 API에 의해 결정됩니다. *Tour of Heroes* 데이터 API는 히어로 데이터를 배열로 반환합니다.  

> 다른 API들은 데이터에 원하는 객체를 묻어 놓습니다. RxJS ```map``` 연산자로 ```Observable``` 결과를 처리하기 위해 데이터로 파고 들어가야할 수도 있습니다.  
여기서는 그런 부분에 대해서 논의하지 않았지만, ```getHeroNo404()``` 메소드안에 ```map```의 예제가 샘플 코드에 포함되어 있습니다. 참고하세요.
  

### 에러 핸들링
특히 원격 서버에서 데이터를 받아왔을 때 일이 꼬이는 경우가 발생합니다. ```HeroService.getHeroes()``` 메소드는 에러를 잡고 그런 것들을 적절히 처리해줘야 합니다.  

에러를 잡기 위해서, RxJS의 ```catchError()``` 연산자를 통해 ```http.get()```에서 observable 결과를 "파이프(pipe)" 합니다.  

```catchError```를 ```rxjs/operators```에서 가져옵니다. 같이 가져온 다른 연산자들은 나중에 필요하게 될 것입니다.  

```
import { catchError, map, tap } from 'rxjs/operators';
```
  
이제 observable 결과를 ```.pipe()``` 메소드로 확장 시킵니다. 그리고 거기에 ```catchError()``` 연산자를 부여합니다.  

```
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      catchError(this.handleError('getHeroes', []))
    );
}
```
  

```catchError()``` 연산자는 실패한 ```Observable```를 옮겨줍니다. 무슨 에러든 처리할 수 있는 에러 핸들러에게 에러를 건내줍니다.  

다음 ```handleError()``` 메소드는 에러를 보고하고 어플리케이션이 계속 작동하도록 무해한 결과를 반환합니다.  


### handleError
다음 ```errorHandler()``` 은 ```HeroService```에 있는 많은 메소드에 공유될 것입니다. ```errorHandler()```는 메소드들마다 다르 요구 사항을 충족시키기 위해서는 보편화 되었습니다.  

에러 핸들링을 직접적으로 하는 것 대신에, ```catchError```에 에러 핸들 함수를 반환합니다. 이러한 작업은 작동 이름에 실패와 안전한 결과 값을 둘다 설정해 놓습니다.(?)  

```
/**
 * Handle Http operation that failed.
 * Let the app continue.
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */
private handleError<T> (operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {

    // TODO: send the error to remote logging infrastructure
    console.error(error); // log to console instead

    // TODO: better job of transforming error for user consumption
    this.log(`${operation} failed: ${error.message}`);

    // Let the app keep running by returning an empty result.
    return of(result as T);
  };
}
```
  

콘솔에 에러가 보고된 후, 핸들러는 유저에게 친숙한 메세지를 구조화 시키고 앱이 계속 작동하도록 안전한 결과를 반환합니다.  

각 서비스 메소드이 다른 ```Observable``` 결과를 반환하기 때문에 ```errorHandler()```는 타입 파라미터를 가져오고 앱이 기대하는 타입으로 안전한 결과를 반환할 수 있습니다.  


### *Observable* 탭(Tab)
```HeroService``` 메소드들은 observable 값의 흐름을 탭(Tab) 메세지(log() 같은)를 화면 아래 메세지 영역에 보냅니다.  

observable 값을 바라보고, 그 값들로 무언가를 하고 그 값들을 따라가는 것들을 RxJS의 ```tab``` 연산자로 할 것입니다. ```tab```은 값 그 자신들이 닿을 수 없도록 뒤로 부릅니다.(?)  

다음은 작업을 로그로 남기는 ```tab```이 있는 ```getHeroes```의 최종 버전입니다.  

```
/** GET heroes from the server */
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      tap(heroes => this.log(`fetched heroes`)),
      catchError(this.handleError('getHeroes', []))
    );
}
```
  

### 아이디로 히어로 가져오기
대부분 웹 API들은 ```api/hero/:id```로부터 *아이디(id)* 요청으로 가져오는 것을 지원합니다.(```api/hero/11```와 같은). ```HeroService.getHero()```에 그런 요청을 만들기 위해 다음과 같이 추가합니다:  

#### src/app/hero.service.ts
```
/** GET hero by id. Will 404 if id not found */
getHero(id: number): Observable<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get<Hero>(url).pipe(
    tap(_ => this.log(`fetched hero id=${id}`)),
    catchError(this.handleError<Hero>(`getHero id=${id}`))
  );
}
```
  

```getHeroes()```에 세 가지 다른 부분이 있습니다.  

* 히어로의 id가 필요한 리퀘스트 URL 구조입니다.
* 서버는 히어로 배열보다는 단일 히어로로 리스폰스 해야 합니다.
* 그러므로 ```getHero```는 ```Observable<Hero>```를 반환합니다.(*히어로 객체인 하나의 observable*)

---

## 히어로 갱신
*히어로 상세 정보*에서 히어로의 이름을 편집할 수 있습니다. 입력할 때, 히어로 이름은 화면 상단 제목 부분에 갱신됩니다. 하지만 "뒤로가기 버튼"을 클릭하면 갱신한 이름은 없어집니다.  

만약 변경을 유지하고 싶다면 서버에 그 변경 사항을 남겨야만 합니다.  

히어로 상세 정보 템플릿의 끝에 저장 버튼을 추가하고 새로운 컴포넌트 메소드인 ```save()```가 바인딩 되도록 해주세요.  

#### src/app/hero-detail/hero-detail.component.html (save)
```
<button (click)="save()">save</button>
```
  
```save()``` 메소드를 다음과 같이 추가합니다. 그러면 히어로 이름 변경이 유지되고 히어로 서비스의 ```updateHero()```를 사용하여 변경하고 이전 화면으로 돌아가도록 합니다.  

#### src/app/hero-detail/hero-detail.component.ts (save)
```
save(): void {
   this.heroService.updateHero(this.hero)
     .subscribe(() => this.goBack());
}
```

### *HeroService.updateHero()* 추가
```updateHero()``` 메소드의 구조는 ```getHeroes()```와 유사합니다. 하지만 서버에 변경된 히어로를 유지하기 위해서 ```http.put()```을 사용합니다.  

#### src/app/hero.service.ts (update)
```
/** PUT: update the hero on the server */
updateHero (hero: Hero): Observable<any> {
  return this.http.put(this.heroesUrl, hero, httpOptions).pipe(
    tap(_ => this.log(`updated hero id=${hero.id}`)),
    catchError(this.handleError<any>('updateHero'))
  );
}
```
  

```HttpClient.put()``` 메소드는 세 가지의 파라미터를 전달합니다.  

* URL
* 갱신할 데이터(이번 케이스 처럼 변경된 히어로 정보)
* 옵션
  
URL은 변경하지 않습니다. 히어로 웹 API는 히어로의 아이디를 보고 업데이트 한 다는 것을 알고 있습니다.  

히어로 웹 API는 저장 리퀘스트 HTTP에 특별한 헤더를 기다리고 있습니다. 헤더는 ```HeroService```에 상수 ```httpOption```로 정의합니다.  

```
const httpOptions = {
  headers: new HttpHeaders({ 'Content-Type': 'application/json' })
};
```
  
브라우저가 새로고침 됩니다. 히어로 이름을 변경하고 저장하세요. 그리고 "뒤로가기" 버튼을 클릭하세요. 그 히어로는 지금 변경된 이름으로 목록에 보일 것입니다.  

---

## 새로운 히어로를 추가
히어로를 추가하기 위해서, 이번 앱에서는 이름만 필요하도록 할 것입니다. ```input``` 태그를 사용하고 짝으로 ```button```도 추가하세요.  

다음과 같이 ```HeroesComponent``` 템플릿에 삽입하세요(단 제목 바로 뒤에):  

#### src/app/heroes/heroes.component.html (add)
```
<div>
  <label>Hero name:
    <input #heroName />
  </label>
  <!-- (click) passes input value to add() and then clears the input -->
  <button (click)="add(heroName.value); heroName.value=''">
    add
  </button>
</div>
```
  
클릭 이벤트에 반응에 컴포넌트의 클릭 핸들러를 호출하고 그러고 난 다음 입력란이 지워지고 다른 이름이 들어갈 준비를 합니다.  

#### src/app/heroes/heroes.component.ts (add)
```
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
  this.heroService.addHero({ name } as Hero)
    .subscribe(hero => {
      this.heroes.push(hero);
    });
}
```
  
공백이 아닌 이름을 부여 했을 때, 핸들러는 이름을 가지고 유사-```Hero``` 객체를 생성하고(단, ```id```는 없습니다.) ```addHero()``` 메소드에 전달합니다.  

```addHero```가 성공적으로 저장한다면, ```subscribe``` 콜백은 새로운 히어로를 받고 히어로 목록에 보여주기 위해 그 히어로를 추가합니다.  

다음 단계에서는 ```HeroService.addHero```를 작성할 것입니다.  


### *HeroService.addHero()* 추가
```HeroService``` 클래스에 다음과 같이 ```addHero()``` 메소드를 추가합니다.  

#### src/app/hero.service.ts (addHero)
```
/** POST: add a new hero to the server */
addHero (hero: Hero): Observable<Hero> {
  return this.http.post<Hero>(this.heroesUrl, hero, httpOptions).pipe(
    tap((hero: Hero) => this.log(`added hero w/ id=${hero.id}`)),
    catchError(this.handleError<Hero>('addHero'))
  );
}
```
  
```HeroService.addHero()```는 ```updateHero```와 두 가지가 다릅니다.  

```put()``` 대신에 ```HttpClient.post()```을 호출합니다.  

새로운 히어로의 아이디가 서버에서 생성될 것을 예측해서, ```Observable<Hero>```를 반환해줍니다.  

브라우저가 새로고침되고 히어로를 추가해보죠.  

---

## 히어로 삭제
히어로 목록의 히어로들은 삭제 버튼을 가지고 있어햐 합니다.  

다음과 같이 ```HeroesComponent``` 템플릿에 ```button``` 태그를 추가하세요.(반복되는 ```<li>```의 히어로 이름 아래에)  

```
<button class="delete" title="delete hero"
(click)="delete(hero)">x</button>
```
  
히어로 목록 HTML은 다음과 같아야 합니다:  

#### src/app/heroes/heroes.component.html (list of heroes)
```
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
    <button class="delete" title="delete hero"
    (click)="delete(hero)">x</button>
  </li>
</ul>
```
  
히어로 엔트리의 보다 오른쪽에 삭제 버튼을 놓으려면 ```heroes.component.css```에 몇 가지 CSS를 추가해야 합니다. 최종 코드 리뷰를 참고하세요.  

컴포넌트에 ```delete()``` 핸들러를 추가합니다.  

#### src/app/heroes/heroes.component.ts (delete)
```
delete(hero: Hero): void {
  this.heroes = this.heroes.filter(h => h !== hero);
  this.heroService.deleteHero(hero).subscribe();
}
```
  
컴포넌트가 ```HeroService```의 히어로 삭제를 대신 하지만, 자체 히어로 목록을 업데이트 해야합니다. 컴포넌트의 ```delete()``` 메소드는 ```HeroService```가 서버에서 성공적으로 처리 될 것을 믿고 즉시 목록에서 *hero-to-delete*를 제거합니다.  

```heroService.delete()```에서 ```Observable```을 반환해주는 것과 컴포넌트는 정말 아무런 관계가 없습니다. 어떠한 방식으로든 ```subscribe```를 해야만 할 뿐입니다.(?)  

> ```subscribe()``` 사용이 귀찮다면 서비스는 서버에 삭제 요청을 보내지 않을 것입니다! 하나의 룰로서, ```Observable```는 어떤한 것이 ```subscribes``` 될 때 까지 아무것도 하지 못합니다!  
직접 임시로 ```subscribe()```를 제거하고 "Dashboard"를 클릭하고 "Heores"를 클릭하여 확인해보세요. 다시 모든 히어로 목록을 보게될 것입니다.
  

### *HeroService.deleteHero()* 추가
다음과 같이 ```HeroService```에 ```deleteHero()``` 메소드를 추가하세요.  

#### src/app/hero.service.ts (delete)
```
/** DELETE: delete the hero from the server */
deleteHero (hero: Hero | number): Observable<Hero> {
  const id = typeof hero === 'number' ? hero : hero.id;
  const url = `${this.heroesUrl}/${id}`;

  return this.http.delete<Hero>(url, httpOptions).pipe(
    tap(_ => this.log(`deleted hero id=${id}`)),
    catchError(this.handleError<Hero>('deleteHero'))
  );
}
```
  

적어두세요.  

* ```HttpClient.delete```를 호출합니다.
* URL은 히어로 리소스 URL 추가한 히어로의 ```id``` 부분을 제거한 것입니다.
* ```put```과 ```post```로 데이터를 보내지 마세요.
* 계속해서 ```httpOptions```을 보내야 합니다.
  
브라우저가 새로고침되고 새로운 기능인 삭제를 시도해보세요!  

---

## 이름으로 검색하기
이제 마지막 연습입니다. 체인 ```Observable``` 연산을 배우고 유사 HTTP 요청의 수를 최소화할 수 있고 경제적으로 네트워크 대역폭을 소비할 수 있습니다.  

*Dashboard*에 히어로 검색 기능을 추가할 것입니다. 사용자가 검색란에 이름을 입력했을 때, 이름으로 히어로를 필터링 하기 위해 HTTP 요청을 반복적으로 만들어 볼 것입니다. 우리의 목표는 필요한 만큼 많은 요청을 하는 것입니다.  

### *HeroService.searchHeroes*
```HeroService```에 ```searchHeroes``` 메소드를 추가하는 작업을 시작해봅시다.  

#### src/app/hero.service.ts
```
/* GET heroes whose name contains search term */
searchHeroes(term: string): Observable<Hero[]> {
  if (!term.trim()) {
    // if not search term, return empty hero array.
    return of([]);
  }
  return this.http.get<Hero[]>(`api/heroes/?name=${term}`).pipe(
    tap(_ => this.log(`found heroes matching "${term}"`)),
    catchError(this.handleError<Hero[]>('searchHeroes', []))
  );
}
```
  
메소드는 만약 빈 검색 결과가 있다면 즉시 빈 배열을 반환합니다. 나머지는 ```getHeroes()```와 아주 비슷합니다. 다른 점은 검색어가 들어갈 *query string*을 포함하는 URL일 뿐 입니다.  


### Dashboard에 검색을 추가
```DashboardComponent``` 템플릿을 열고 히어로 검색 태그인 ```<app-hero-search>```을 템플릿 맨 아래에 추가합니다.  

#### src/app/dashboard/dashboard.component.html
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

<app-hero-search></app-hero-search>
```

이번 템플릿은 ```HeroesComponent``` 템플릿에 있는 ```*ngFor``` 반복자와 많이 유사해 보입니다.  

불행하게도 ```<app-hero-search>```에 매칭되는 셀렉터를 가진 컴포넌트를 Angular가 찾을 수 없어서 앱이 깨지게 됩니다.  

```HeroSearchComponent```는 아직 없습니다. 고쳐보도록 하죠.  

### *HeroSearchComponent* 생성
*HeroSearchComponent*를 CLI를 통해 생성합니다.  

```
ng generate component hero-search
```
  
CLI가 세 가지 ```HeroSearchComponent```를 생성하고 ```AppModule```에 컴포넌트를 선언해 줍니다.  

생성된 ```HeroSearchComponent``` 템플릿을 다음과 같이 텍스트 박스와 검색 결과가 매칭된 목록이 있도록 바꿔주세요.  

####src/app/hero-search/hero-search.component.html
```
<div id="search-component">
  <h4>Hero Search</h4>

  <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />

  <ul class="search-result">
    <li *ngFor="let hero of heroes$ | async" >
      <a routerLink="/detail/{{hero.id}}">
        {{hero.name}}
      </a>
    </li>
  </ul>
</div>
```
  
아래에 최종 코드 리뷰(https://angular.io/tutorial/toh-pt6#herosearchcomponent )를 참고하여 ```hero-search.component.css```에 CSS 스타일을 추가하세요.  

사용자가 검색란에 입력했을 때, *키업(keyup)* 이벤트 바인딩을 컴포넌트의 ```search()``` 메소드와 새로운 검색어를 함께 호출하도록 합니다.  


### *AsyncPipe*
기대한 대로 ```*ngFor```은 히어로 객체를 반복해줍니다.  

자세히 보면 ```*ngFor```이 ```heroes```가 아닌 ```heroes$```를 목록에서 불러와 반복하는 것을 볼 수 있습니다.  

```
<li *ngFor="let hero of heroes$ | async" >
```
  
```$```은 ```heroes$```가 배열이 아닌 ```Observable```로 나타내도록 합의해줍니다.  

```*ngFor```은 ```Observable```에 대해서는 아무것도 할 수 없습니다. 하지만 파이프 문자(```|```) 다음에 오는 ```async```가 Angular의 ```AsyncPipe```로 분류합니다.  

```AsyncPipe```는 자동적으로 ```Observable```로 subscribes 합니다. 따라서 클래스에서 ```Observable```를 수행할 필요가 없습니다.(?)  

### *HeroSearchComponent*를 고치기
생성된 ```HeroSearchComponent``` 클래스와 메타데이터를 다음과 같이 바꾸세요.  

#### src/app/hero-search/hero-search.component.ts
```
import { Component, OnInit } from '@angular/core';

import { Observable } from 'rxjs/Observable';
import { Subject }    from 'rxjs/Subject';
import { of }         from 'rxjs/observable/of';

import {
  debounceTime, distinctUntilChanged, switchMap
} from 'rxjs/operators';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-search',
  templateUrl: './hero-search.component.html',
  styleUrls: [ './hero-search.component.css' ]
})
export class HeroSearchComponent implements OnInit {
  heroes$: Observable<Hero[]>;
  private searchTerms = new Subject<string>();

  constructor(private heroService: HeroService) {}

  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }

  ngOnInit(): void {
    this.heroes$ = this.searchTerms.pipe(
      // wait 300ms after each keystroke before considering the term
      debounceTime(300),

      // ignore new term if same as previous term
      distinctUntilChanged(),

      // switch to new search observable each time the term changes
      switchMap((term: string) => this.heroService.searchHeroes(term)),
    );
  }
}
```
  
```heroes$```의 선언은 ```Observable```로 합니다.  

```
heroes$: Observable<Hero[]>;
```
  
```ngOnInit()```(https://angular.io/tutorial/toh-pt6#search-pipe )을 설정할 것입니다. 작업을 하기 전에, ```searchTerms```의 정의를 보도록 하죠.  

### The *searchTerms* RxJS subject
맴버변수 ```searchTerms```은 RxJS ```Subject```로 정의했습니다.  

```
private searchTerms = new Subject<string>();

// Push a search term into the observable stream.
search(term: string): void {
  this.searchTerms.next(term);
}
```
  
```Subject```는 *observable* 값의 원천과 ```Observable``` 그 자체입니다. ```Observable```처럼 ```Subject```도 subscribe 할 수 있습니다.  

```search()``` 메소드에 넣을 때 ```next(value)``` 메소드를 ```Observable``` 로 호출 하여 값을 넣을 수 있습니다.  

```search()``` 메소드가 텍스트박스 키 입력 이벤트에 의해 호출 됩니다.  

```
<input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
```
  
사용자가 텍스트 박스에 입력할 때, 바인딩한 search()와 함께 텍스트박스 값을 호출합니다.  

```searchTerms```은 검색어를 꾸준히 방출하는 ```Observable```이 됐습니다.  


### RxJS 연사자 변경
검색어가 ```searchHeroes()```로 직접 전달 된 후 모든 사용자는 많은 양의 HTTP 리퀘스트를 만들어 냅니다. 서버 자원에 과부화와 셀룰러 네트워크 데이터를 태워먹을 수도 있습니다.  

Instead, the ngOnInit() method pipes the searchTerms observable through a sequence of RxJS operators that reduce the number of calls to the searchHeroes(), ultimately returning an observable of timely hero search results (each a Hero[]).

여기는 그 코드입니다.  

```
this.heroes$ = this.searchTerms.pipe(
  // wait 300ms after each keystroke before considering the term
  debounceTime(300),

  // ignore new term if same as previous term
  distinctUntilChanged(),

  // switch to new search observable each time the term changes
  switchMap((term: string) => this.heroService.searchHeroes(term)),
);
```
  

* ```debounceTime(300)```는 새로운 문자열 들어올 때 까지 0.3초를 기다리고 마지막 문자열을 전달해줍니다.
* ```distinctUntilChanged```은 요청이 텍스트를 걸러 안전하게 보내지도록 합니다.
* ```switchMap()```은 디바운스와 ```distinctUntilChanged```를 통해서 그것을 만든 각 검색어에 대한 검색 서비스를 호출한다.(?) ```switchMap()```은 이전 observable한 검색을 취소하고 버리며 오직 observable한 최신 검색 서비스 만을 반환합니다.(?)
  

> ```switchMap``` 연산자 처럼, 모든 유효 키 이벤트는 ```HttpClient.get()``` 메소드 호출을 할 수 있습니다. 요청 사이에 0.3초 일시 정지가 있어도 다중 HTTP 요청을 날릴 수 있고 전송한 순서대로 반환하지 않을 수 있습니다.  
```switchMap()```는 가장 최근의 HTTP 메소드를 호출한 observable한 값만 반환되는 동안 원본 요청 순서를 보존 합니다. 앞서 호출한 결과들은 취소되고 버려집니다.  
이전 ```searchHeroes()``` *Observable*을 취소한다고 해서 남아있는 HTTP 요청을 실제로 중단하지 않는 다는 것을 기억하세요. 원치 않는 결과가 어플리케이션 코드에 도달하기 전에 단순히 버려집니다.
  

컴포넌트 *클래스*가 ```heroes$``` *observable*을 subscribe하지 못한다는 것을 기억하세요. 템플릿에서 AsyncPipe(https://angular.io/tutorial/toh-pt6#asyncpipe) 역활은 단지 그것 뿐입니다.  

### 시도해보죠
앱을 재실행 합니다. *Dashboard*에서 검색 박스에 텍스트를 입력합니다. 히어로 이름이 존재하는 어떤 문자열이 들어있으면 다음과 같이 보일 것입니다.  
![toh-hero-search](https://angular.io/generated/images/guide/toh/toh-hero-search.png)
  
---

## 최종 코드 리뷰
앱은 이번 live example(https://angular.io/generated/live-examples/toh-pt6/eplnkr.html) / download example(https://angular.io/generated/zips/toh-pt6/toh-pt6.zip) 과 같아야 합니다.  

여기에는 이번 단계에서 논의한 파일들이 있습니다.(```src/app/``` 폴더에 있는 모든 파일)

소스는 링크를 참고하세요.  

https://angular.io/tutorial/toh-pt6#heroservice-inmemorydataservice-appmodule
