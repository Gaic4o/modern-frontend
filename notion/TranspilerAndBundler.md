# Transpiler Bundler 

## 트랜스파일러 (Transpiler) 

트랜스파일러는 소스 코드를 한 언어에서, 다은 언어로 변환하는 도구인데, 
프론트엔드 개발에서는 Javascript ES6, Typescript 같은 언어를 오래된 Javascript 버전(ES5)으로 변환하는 데 사용 됩니다. 

대표적 예시 : `Babel`
-> Babel 이 Javascript 대표적으로 사용되는 트랜스파일러 입니다. 
-> Javascript(ES6, EP7) 최신 문법으로 코드를 작성할 수 있게 해주며, 이 코드를 브라우저가 이해할 수 있는 ES5 버전의 Javascript 코드로 변환 해 줍니다. 

## 번들러 (Bundler) 

여러 개의 파일 (Javascript, CSS, Image 등) 모아서, 하나 또는 몇 개의 파일로 결합하는 도구 입니다. 
이는 웹 페이지의 로드 시간을 줄이거나, 모듈 관리를 용이하게 해주며, 파일 간의 의존성 관리를 해줌. 

대표적 예시 : `Webpack`
-> 모듈 : (모든 파일 Javascript, CSS, Image, Font 등) 모듈로 취급됩니다.
-> 의존성 그래프 : 프로젝트의 시작점 index.js 시작 해, 모듈 간의 의존성을 추적하여 의존성 그래프를 만듭니다. (모든 모듈과 관계를 나타냅니다.)
-> 로더 (Loader) : Webpack 은 기본적으로 Javascript, JSON 파일만 이해 가능, 다른 타입 파일 (CSS, Image, Font 등) 처리하기 위한 Webpack Loader 사용. 

1. 진입점 설정 : webpack.config.js 파일에서 지정된 진입점에서 시작 해, 애플리케이션의 모듈과 라이브러리를 로드 
2. 로더 처리 : webpack 은 설정된 규칙에 따라, 적절 로더를 사용 해 파일을 처리 style-loader, css-loader 는 CSS 파일 처리, babel-loader 는 ES6 이상의 Javascript 변환 
3. 플러그인 실행 :  모든 파일이 로드되고 변환된 후, Webpack 다양한 최적화 작업을 수행하기 위해 플러그인을 실행. 
4. 출력 : Webpack 은 모든 모듈을 포함한 결과물 bundle.js을 출력합니다. 


## 대표적인 트랜스파일러, 컴파일러 

1. Babel 
2. Typescript : 타입시스템을 가지고 컴파일을 하는 인기있는 컴파일러 
3. esbuild : Go 를 기반으로 빠른 속도와 번들링까지 가능한 차세대 대표 트랜스파일러.
4. SWC : Rust 를 기반으로 빠른 속도와 쉬운 사용을 목표로 인기를 얻고 있는 트랜스파일러 입니다. 



## Babel

### Babel 설정 파일 

`.babelrc.json, babel.config.json, .babelrc.js, babel.config.js` -> Babel 설정 파일. 
이 파일들은 Babel 이 코드를 어떻게 변환할 지 결정 합니다. 

### 플러그인 Plugin 

플러그인은 Babel 이 특정 Javascript 문법을 다른 문법으로 변환하는 규칙을 정의.
예를 들어, 화살표 함수(()=>{})를 ES5 함수 표현식(function(){})으로 변환하고 싶다면, @babel/plugin-transform-arrow-functions 플러그인을 설정 파일에 추가합니다.

### 프리셋 Preset 

프리셋은 여러 플러그인의 집합. (개별적으로 각각 플러그인을 추가하는 대신, 관련된 플러그인을 묶어 미리 정의된 설정을 사용할 수 있음.)
`@babel/preset-env` 다양한 Javascript 최신 기능을 지원하는 플러그인 모음 -> 이 프리셋은 설정에 따라 필요한 변환만을 적용, 최종 번들의 크기를 최적화하고, 대상 브라우저에서 필요한 기능만을 포함하게 합니다. 


대표 preset
 
@babel/preset-env (최신 ES 스펙을 지원, 필요한 변환만 적용하여 번들 크기를 최적화하고, `.browserlistrc 또는 package.json 정의된 대상 브라우저에 맞춰 코드 변환`)
@babel/preset-react. (리액트 개발에 필요, JSX 문법을 Javascript 변환, React 에서 사용되는 다른 기능들을 지원)
@babel/preset-typescript (Typescript 코드를 순수 Javascript 변환)

내 느낌점 : 단순히 구형 브라우저를 위한 ES 변환 도구라고 생각했으나, 실제로는 플러그인 설치 및 설정, 프리셋 활용 등 더 세밀한 조정이 가능하다는 것을 깨달았습니다. 특히, @babel/preset-env 사용으로 자동적으로 필요한 변환을 적용하며, .browserslistrc를 통해 대상 브라우저를 세밀하게 설정할 수 있다는 사실을 알게 되었습니다.



## Rollup 

Rollup 은 모듈 번들러 중 하나, 주로 Javascript 라이브러리와 애플리케이션을 번들링하기 위해 사용됩니다. 
Webpack 과 유사하지만, 특히 라이브러리 개발에 최적화된 몇 가지 특징을 가지고 있습니다. 

1. ES6 모듈 문법 지원 : 기본적으로 ES6 (import, export) 지원. 이는 모듈 간의 의존성을 효과적으로 관리, 필요한 코드만 번들링 됩니다.
2. 트리 쉐이킹 : 사용하는 가장 큰 이유가 트리 쉐이킹 입니다. -> 실제로 사용되지 않은 코드(데드 코드)를 최종 번들에서 제거하는 기능.
3. 간결한 번들 생성 : 간결하고, 효율적 번들 생성. -> 특히 라이브러리 개발에 유리, 최종 사용자에게 더 빠른 로딩 시간과 성능을 제공.
4. 플러그인 시스템 : 플러그인 시스템을 가지고 있음. 번들링 과정을 사용자 정의 가능. 

정리 : Rollup 은 주로 라이브러리를 제작 할 떄 주로 번들러로 사용 됩니다. Webpack, Vite 도 Rollup 특징을 대부분 가지고 있지만, 
Rollup 은 좀 더 라이브러리를 제작할 떄, ES6 모듈의 중심으로 트리 쉐이킹과 간결한 번들 생성에 특히 Webpack, Vite 보다 간결하고 효율적인 번들을 생성합니다. 

여기서 왜 ? Webpack, Vite, Rollup 모두 ES6 모듈을 중심으로 트리 쉐이킹을 사용하는 데, 굳이 Rollup 을 라이브러리를 제작 할 떄 주로 사용하는 이유가 무엇일까요? 
Webpack, Vite 은 어플리케이션 개발에 특화되어 있어, 다양한 기능 (코드 분할, 핫 모듈 교체, 다양한 파일 타입 지원) 을 제공해 주고, 이러한 기능들은 내부 추가적인 코드나, 런타임 처리가 필요할 수 있습니다. 
이 점을 봤을 떄, 어플리케이션 개발에 유리하지만, 최종 번들의 간결성 측면에서는 Rollup 보다 불리할 수 있습니다. 
Rollup 은 모듈을 하나의 스코프로 번들을 생성 함. 다른 도구들이 각 모둘을 별도의 함수나 스코프로 분리하여, 번들링하는 것과 달리, Rollup 은 필요한 코드만 포함하여 번들 크기를 줄이고, 런타임 성능도 향상시킵니다. 
