# QuickStart
---

## Step 1. 개발 환경 설치
개발 환경을 설치하기 전에 확인해야할 부분들이 있습니다.  
Node.js와 npm이 설치 되어 있는지를 확인해야 합니다.  

> 터미널/윈도우 콘솔에서 node -v와 npm -v로 node 버전이 최소 6.9.x 이고 npm 버전은 3.x.x인지를 확인하세요. 저 버전들 보다 오래된 버전은 에러가 발생하지만 더 이 후 버전이면 괜찮습니다.

확인 했다면 Angular CLI를 설치한다.  
```
npm install -g @angular/cli
```


## Step 2. 새로운 프로젝트를 생성하기
터미널 창을 여세요.  
다음 커맨드를 따라 새로운 프로젝트와 뼈대 어플리케이션를 생성합니다.  

```
ng new my-app
```

> 제발 기다려주세요. 새로운 프로젝트를 셋업하는데는 시간이 걸립니다. 대부분 npm 페키지를 설치하는데 시간이 걸립니다.

## Step 3: 어플리케이션 실행
프로젝트 디렉토리로 이동하고 서버를 실행합니다.  
```ng serve``` 커맨드로 서버를 실행한다.(해당 명령은 당신의 파일이 변경된 것을 감지하고 리빌드 합니다.)  

```--open```(혹은 ```-o```)를 사용하면 http://localhost:4200 이 당신의 브라우저를 통해 자동으로 열릴 것 입니다.  
당신의 앱이 해당 메시지와 함께 인사합니다:  

![message](https://angular.io/generated/images/guide/cli-quickstart/app-works.png)

## Step 4: 당신의 첫 Angular 컴포넌트를 편집
당신의 첫 Angular 컴포넌트가 CLI로 생성됐습니다. 이것은 루트 컴포넌트이고 이름은 ```app-root``` 입니다. 당신은 ```./src/app/app.component.ts.``` 에서 그것을 찾을 수 있습니다.  

컴포넌트를 열고 ```title``` 변수를 *Welcome to app!!* 에서 *Welcome to My First Angular App!!*로 변경합니다:

> src/app/app.component.ts
    
    export class AppComponent {
        title = 'My First Angular App';
    }

브라우저는 title 변경과 함께 자동으로 새로고침 됩니다.  정말 멋있지만, 더욱 잘 보일 수 있습니다.(???)  

```src/app/app.component.css```를 열고 컴포넌트에 몇 가지 style을 줍니다.  

> src/app/app.component.css
    
    h1 {
        color: #369;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 250%;
    }

![step4.css](https://angular.io/generated/images/guide/cli-quickstart/my-first-app.png)  

잘 보이는 군요!


## 다음은 뭘까?
그것은 당신이 "Hello, World" 앱과 같은 것을 기대하고 있을 것입니다.

우리는 __Tour of Heroes Tutorial__ 를 준비했고 시연할 작은 어플리케이션을 빌드할 준비가 되어 있습니다.  

혹은 당신이 Angular 프로젝트의 파일들에 대해 알고 싶다면 여기서 조금더 머무르면 됩니다.  


## 프로젝트 파일 리뷰

### src 폴더
src 폴더에 당신의 앱이 살아있습니다. 모든 Angular 컴포넌트, 템플릿, 스타일, 이미지 그리고 앱에 필요한 모든 것들이 여기에 있습니다. 이 폴더 밖에 있는 파일들은 당신의 앱 빌딩 보조를 위해 필요한 파일들입니다.  
> 
* src
    * app
        * app.component.css
        * app.component.html
        * app.component.spec.ts
        * app.component.ts
        * app.module.ts
    * assets
        * .gitkeep
    * environments
        * environment.prod.ts
        * environment.ts
    * favicon.ico
    * index.html
    * main.ts
    * polyfills.ts
    * styles.css
    * test.ts
    * tsconfig.app.json
    * tsconfig.spec.json

| __File__ | __Purpose__ |
|----------|-------------|
| ```app/app.component.{ts,html,css,spec.ts}``` | ```AppComponent```에 대한 HTML템플릿, CSS 스타일시트, unit 테스트를 정의할 수 있습니다. 어플리케이션이 발전함으로서(?) 둥지(?) 컴포넌트의 나무가 될 __루트__ 컴포넌트입니다. |
| ```app/app.module.ts``` | ```AppModule```를 정의합니다. Angular에서 어떻게 어플리케이션이 조립되는지를 말하는 __root module__ 입니다. 지금은 ```AppComponent```만 정의되어 있습니다. 조만간 더 많은 컴포넌트가 정의될 것입니다. |
| ```assets/*``` | 이미지나 그 밖의 파일들이 놓여질 폴더입니다. 앱을 빌드할 때 해당 폴더를 모조리 복사합니다. |
| ```environments/*``` | 앱 환경변수를 설정할 수 있는 파일이 있습니다. |
| ```favicon.ico``` | 모든 웹 사이트는 보기 좋은 북마크를 갖고 싶어합니다. 아주 개성있는 Angular icon으로 시작하세요. |
| ```index.html``` | 당신이 사이트를 누군가가 방문했을 때 맞이하게 될 메인 HTML 페이지 입니다. 대부분의 경우 당신이 이 파일을 편집할 일은 거의 없을 것입니다. CLI로 당신의 앱을 빌드 할 때 자동적으로 모든 ```js```와 ```css``` 파일을 ```<script>```태그와 ```<link>``` 태그에 추가 시킵니다. |
| ```main.ts``` | 앱의 메인 진입 부분입니다. JIT컴파일러로 어플리케이션을 컴파일하고 브라우저에 루트 모듈(```AppModule```)을 실행시킵니다(Bootstrap AppModule). ```ng build```와 ```ng serve``` 커맨드에 ```--aot```를 추가하여 어떠한 코드 변환도 없이 AOT 컴파일러를 사용할 수도 있습니다. |
| ```polyfills.ts``` | 다른 레벨의 웹 표준을 지원하는 브라우저가 있습니다.(즉, 웹 표준에 뒤쳐진 브라우저) Polyfills는 이러한 다른 레벨의 정상화를 도와줍니다. ```core-js```와 ```zone.js```로 안정성을 보장 받지만, Browser Support guide를 통해 더 많은 정보를 숙지해야 합니다. |
| ```styles.css``` | 전역 스타일을 여기에 정의합니다. 대부분의 경우 지역 컴포넌트에서 쉽게 유지보수할 수 있습니다. 하지만 앱 전체의 스타일에 영향을 주고싶다면 여기서 편집하세요. |
| ```test.ts``` | 유닛 테스트의 메인 진입 부분입니다. 익숙치 않은 사용자 정의 설정이 있지만, 편집해야할 사항은 아닙니다. |
| ```tsconfig.{app or spec}.json``` | Angular앱(```tsconfig.app.json```)과 유닛 테스트(```tsconfig.spec.json```)를 위한 TypeScript 컴파일러 설정입니다. |

### 루트 폴더
src 폴더는 프로젝트의 루트 폴더에 속한 폴더중 하나입니다. 다른 파일들은 빌드, 테스트, 유지보수, 문서정리 그리고 앱 배포를 도와줍니다.  
> 
* my-app
    * e2e
       * app.e2e-spec.ts
       * app.po.ts
       * tsconfig.e2e.json
    * node_modules/...
    * src/...
    * .angular-cli.json
    * .editorconfig
    * .gitignore
    * karma.conf.js
    * package.json
    * protractor.conf.js
    * README.md
    * tsconfig.json
    * tslint.json

| __File__ | __Purpose__ |
|----------|-------------|
| ```e2e/``` | e2e 폴더 내부에는 end-to-end 테스트가 들어가 있습니다. 이 폴더가 src 폴더에 있지 않은 이유는 e2e테스트는 당신의 앱을 테스트할 별개의 앱입니다. ```tsconfig.e2e.json```이 왜 e2e에 들어가있는지가 그 증거입니다. |
| ```node_modules/``` | ```Node.js```가 생성한 폴더입니다. ```package.json```에 있는 모든 부분 모듈을 여기에 놓습니다. |
| ```.angular-cli.json``` | Angular CLI를 위한 설정입니다. 여기에는 여러 기본 값이 설정되어 있고 프로젝트가 빌드 됐을 때 포함될 파일들이 설정되어 있습니다. 더 많은 정보를 알고 싶다면 공식 문서를 참고하세요. |
| ```.editorconfig``` | 당신의 프로젝트를 사용하는 모든 누군가가 같은 편집 설정을 사용할 수 있게 단순화된 설정입니다. ```.editorconfig``` 파일에 대한 정보는 http://editorconfig.org 여기서 얻으실 수 있습니다. |
| ```.gitignore``` | Git에 커밋하지 않을 소스를 관리하는 파일입니다. |
| ```karma.conf.js``` | ```ng test```로 Karma 테스트를 실행( *https://karma-runner.github.io* )했을 때 유닛 테스트에 대한 설정 파일입니다. |
| ```package.json``` | 당신의 프로젝트에서 서드파티(The third party) 페키지를 사용하기 위한 ```npm``` 설정 파일입니다. 사용자 정의 설정( *https://docs.npmjs.com/misc/scripts* )을 추가로 할 수 있습니다. |
| ```protractor.conf.js``` | ```ng e2e```로 실행했을 때 Protractor를 위한 End-to-end 테스트 설정파일입니다. |
| ```README.md``` | 당신의 프로젝트의 기본 문서입니다. 레포지토리(repository)를 체크아웃 하는 누군가가 당신의 앱을 빌드할 수 있도록 프로젝트 문서와 함께 변경하세요! |
| ```tsconfig.json``` | IDE에서 사용할 TypeScript 컴파일러 설정파일입니다. |
| ```tslint.json``` | ```ng lint```를 실행 했을 때 TSLint( *https://palantir.github.io/tslint/* )와 함께 Codelyzer( *http://codelyzer.com/* )를 위한 린팅(Linting) 설정 파일입니다. 린팅(Linting)은 당신의 코드 스타일을 유지하도록 도와줍니다. |

> ## 다음 단계
만약 새롭게 Angular를 한다면 튜토리얼을 진행하세요. 당신은 이미 Angular CLI로 설치 했기 때문에 "설치" 단계를 건너 뛸 수 있습니다.
