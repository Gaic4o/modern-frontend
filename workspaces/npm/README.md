# workspace

workspace : Mono 레포짓토리 프로젝트에서, 여러 패키지들을 관리 해 줄 수 있는 도구. -> `이를 통해 하나의 root project 에서 여러 sub project(workspace) 을 관리할 수 있습니다`

## npm workspace

공통 의존성 설치: npm install lodash와 같이 루트 프로젝트에서 의존성을 설치하면, 해당 패키지는 `루트 프로젝트의 node_modules에 설치되고`, 루트 프로젝트의 `package.json에 의존성(version, name, etc...)`으로 추가됩니다. 이를 통해 package-a, package-b와 같은 모든 `sub 프로젝트`에서 해당 의존성을 사용할 수 있게 됩니다.

```dtd
monorepo-workspace-npm/
├── package.json
├── node_modules/
└── packages/
    ├── package-a/
    │   ├── package.json
    │   └── index.js
    └── package-b/
        ├── package.json
        └── index.js
```

1. npm install lodash
2. lodash 는 root project 에 node_modules 에 설치가 되고, root project 의 package.json 에 lodash 를 `의존성(name, version)`을 추가합니다.
3. root project 서브 project package-a, package-b 에서 loadsh 를 사용할 수 있습니다.

### Script 실행

서브 프로젝트 스크립트 실행: `npm run build --workspace=package-a` 명령어를 사용하여 package-a의 build 스크립트를 실행할 수 있습니다. 마찬가지로, `npm run start --workspace=package-b`를 사용하여 package-b의 start 스크립트를 실행할 수 있습니다. 이를 통해 각 서브 프로젝트의 개별 스크립트를 간편하게 관리할 수 있습니다.

### 버전 관리 및 배포

버전 업데이트: `npm version 1.0.1 --workspace=package-a` 명령어를 사용하여 package-a의 버전을 업데이트할 수 있습니다. 이 명령어는 특정 워크스페이스의 package.json 파일에 새로운 버전 정보를 적용합니다.
배포:`npm publish --workspace=package-a `명령어를 사용하여 package-a를 npm 레지스트리에 배포할 수 있습니다. 이 과정은 각 서브 프로젝트를 개별적으로 npm에 배포하는 데 사용됩니다.

### 요약

`npm workspace` 를 사용하면, 모노레포 구조에서 project 관리가 수월 해 집니다.
여러 서브 프로젝트에서 종종 같은 패키지들을 사용할 떄가 있는데, 각 프로젝트는 자체 node_modules 디렉토리를 가지며, 같은 패키지의 복사본을 여러 번 다운로드하고 저장 할 수 있습니다. -> 불필요한 디스 공간 사용과 관리해야 할 의존성 총량 증가로 이어짐.
-> 워크스페이스를 사용 해 이 문제를 효과적으로 사용 가능.
모든 서브 프로젝트는 루트 프로젝트의 `node_modules` 디렉토리에 설치된 공통 의존성을 공유 가능.

## npm link

npm link : 개발 중인 패키지를 로컬에서 테스트하고, 다른 프로젝트에 쉽게 통합하기 위한 방법 입니다.

예시로) npm 에 올린 패키지를 workspace 환경에서, install 해서 import, require 으로 해당 A 패키지를 불러와서 사용하다가, B 패키지를 만들면서, A 패키지에서 특정 기능이 빠졌거나, 버그가 발생했을 떄, 직접 A 패키지를 다시 기능을 보완하고, 테스트, CI CD 를 거쳐서, A 패키지 버전 업데이트를 하고, B 패키지에서 버전이 업데이트 된 것을 가져와서 다시 사용하는 것이 아닌,
workspace 환경에서 A 패키지를 배포하지 않고, link 으로 불러올 수 있습니다.

- 로컬 중 개발 중인 패키지를 npm 저장소에 배포하기 않고, `다른 프로젝트에서 설치된 것 처럼 테스트를 할 수 있습니다.`
- 초기에 테스트 하면서, 버그나 문제를 발견하고, 수정 가능.
- npm link 은 보통 패키지의 동작을 검증할 수 있어, 배포 전 마지막 확인 단계로서의 역할을 합니다.

- `npm v7`부터 도입된 기능으로, 프로젝트 내 여러 패키지를 쉽게 관리할 수 있게 해줍니다.
- package.json의 `workspaces 속성을 통해 로컬 패키지들을 선언적으로 관리`합니다.
- npm install을 실행하면, `작업 공간에 정의된 패키지 간의 의존성을 자동으로 링크해줍니다.`

#### 사용 방법

1. npm init -y -w ./packages/a와 npm init -y -w ./packages/b를 통해 패키지 a와 b를 초기화합니다.
2. 패키지 a에 axios를 설치하려면, `npm i -w a axios`를 실행합니다.
3. 패키지 a를 시작하려면, `npm start --workspace a`를 사용합니다.

장점 :

1. 여러 패키지를 동시에 개발하고 관리하는 복잡한 프로젝트에서도, `각각의 패키지를 쉽게 추가, 삭제, 업데이트할 수 있습니다.`
2. 싱글 레포지토리 또는 다중 레포지토리 방식에 관계없이, npm workspaces는 프로젝트의 구조를 단순화하고 의존성 관리를 간편하게 해줍니다.

## node_modules 에 있는 패키지들을 수정해야 할 상황이 왔을 떄, link 을 사용하게 되면 좋은 점 case

### npm link 를 사용해서 로컬 패키지의 버그 수정 및 타입 문제를 해결 할 수 있음.

요약: 개발 중인 A 프로젝트에서 B 패키지를 사용하고 있습니다. B 패키지에서 타입 문제나 버그가 발견되었을 때, `이를 즉시 해결하기 위해 npm link를 사용할 수 있습니다.`

1. B 패키지 연결: B 패키지의 소스 코드 디렉토리에서 npm link를 실행하여 B 패키지를 글로벌 링크로 생성합니다.
2. A 패키지에서 B 링크: A 프로젝트 디렉토리에서 `npm link B`를 실행하여 B 패키지를 A 프로젝트의 node_modules에 링크합니다.
3. 문제 해결 및 테스트: B 패키지의 타입 문제나 버그를 수정하고, 수정된 코드를 빌드합니다. 이 변경사항은 A 프로젝트에서 바로 반영되어 테스트할 수 있습니다.
4. 실시간 반영: B 패키지의 수정사항이 A 프로젝트에 실시간으로 반영되므로, 즉시 테스트하고 결과를 확인할 수 있습니다.

## 팀 내 다른 패키지의 기능 개선을 위한 npm link 활용.

요약 : 팀에서 개발 중인 여러 패키지 중, 디자인 시스템의 버튼 컴포넌트에 새로운 기능을 추가해야 하는 상황입니다.

1. 디자인 시스템 컴포넌트 링크: 버튼 컴포넌트가 위치한 디자인 시스템 패키지에서 npm link를 실행하여 글로벌 링크를 생성합니다.
2. 기능 추가 및 테스트: 필요한 기능을 버튼 컴포넌트에 추가하고, 이를 로컬에서 바로 테스트합니다.
3. 팀 프로젝트에 적용: 기능 추가가 완료되고 테스트를 마친 후, 변경된 디자인 시스템 패키지를 팀 프로젝트에 npm link를 통해 연결하여, 모든 변경사항을 팀 프로젝트에 적용하고, 추가된 기능을 테스트합니다.

`만약에 이런식으로 npm link 을 사용하지 않는 다면 이런 문제가 발생할 수 있습니다.`

패키지를 `git clone` 하고, `필요한 변경을 수행한 뒤`, `변경 사항을 커밋하고`, `CI/CD 파이프라인을 통해 빌드 및 배포 과정을 거쳐야 합니다`. 그 후에야 업데이트된 패키지를 npm을 통해 설치할 수 있습니다. 시간 소모가 될 수 있음.
-> `실시간 반영으로 개발 속도를 크게 향상 시킬 수 있습니다.`

## 여러 패키지들을 동시에 개발해야 할 떄 npm link 좋은 점 case

여러 패키지들을 동시에 개발해야 할 상황이 있을 떄, 단일 레포지토리 또는 다중 레포지토리 방식으로 작업을 진행 하는 경우에도 큰 이점이 있음.

디자인 시스템과, Next.js Project 초기 환경을 구축하고, 이제 점차적으로 2개의 패키지를 만들어 간다고 가정, 그리고 서로 다른 싱글 레포짓토리에서 관리된다고 가정을 했을 떄,
Next.js Project 에서 디자인 시스템 패키지를 npm link 으로 가져온 다음,
로컬 환경에서 디자인 시스템 버튼 컴포넌트, token, theme 등을 구축 한 것들을 실시간으로 next.js 패키지에서 직접 가져와서 사용 하고, 유지 보수를 할 수 있습니다.
그러면 동시에 2개의 패키지를 개발 해 나갈 수 있는 장점이 있습니다.

## 의존성 보안 취약점

npm 을 사용할 떄, 프로젝트에서는 보통 NPM Package 에서 여러 패키지가 하위로 들어가, 의존성을 가지게 됩니다.

### 의존성 보안 취약점의 예시

가상의 상황: 내가 A라는 패키지를 프로젝트에 사용하고 있습니다. A는 B라는 다른 패키지에 의존하고, B는 또 다른 패키지 C에 의존합니다. 만약 C 패키지에 보안 취약점이 발견된다면, 이는 `A를 통해 당신의 프로젝트에도 영향을 미칠 수 있습니다.` C의 취약점이 공격자에게 악용될 경우, 데이터 유출이나 시스템 손상과 같은 심각한 보안 문제로 이어질 수 있습니다.

### npm audit 사용하기

`npm aduit` 은 프로젝트의 의존성 트리를 분석 해 알려준 보안 취약점을 식별 해 줍니다.
이 명령어를 실행하면, `npm 에 가진 모든 의존성들을 검사하게 되고, 각 의존성에 대한 보안 취약점 정보를 가진 데이터베이스를 조회합니다.`
만약에 취약점이 발견된다면, npm 은 이를 보고하고 가능한 해결 방안을 제시 해 줍니다. -> `이를 통해 개발자는 취약점을 신속히 파악후 적절 조치를 취할 수 있습니다.`

```bash
npm audit

=== npm audit security report ===

# Run  npm update react-scripts --depth 5  to resolve 1 vulnerability
SEMVER WARNING: Recommended action is a potentially breaking change

High            Arbitrary Code Execution
Package         react-scripts
Dependency of   react-scripts
Path            react-scripts > some-dependency > vulnerable-package
More info       https://npmjs.com/advisories/1234
```

- Vulnerability : 발견된 취약점의 심각도가 "High" 으로 표시되어 있음, `이는 취약점이 매우 심각함을 의미.`
- Package : 취약점이 발견된 패키지 이름은 `react-scripts` 입니다.
- Dependency of : `react-scripts` 는 직접 의존하는 패키지로, 프로젝트에 포함되어 있음.
- Path : 취약점이 있는 경로, `react-scripts > some-dependency > vulnerable-package` 통해 취약점이 발견 됐습니다.
- More info : 취약점에 대한 자세한 정보를 제공하는 링크, 이 링크를 통해 취약점에 대해 더 자세히 알아볼 수 있습니다.

결과에 제시된 해결 방법에 따라 취약점을 해결 할 수 있습니다. npm update react-scripts --depth 5 명령어를 실행하라고 권장하고 있습니다.

```bash
npm update react-scripts --depth 5
```
