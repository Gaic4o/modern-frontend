# Polyrepo

큰 프로젝트나, 시스템을 여러 개의 작은 저장소로 나누어 관리하는 접근 방법입니다.
이 각 저장소는 독립적으로 운영되며, 서로 다른 부분들이 서로 영향을 미치지 않도록 해야 합니다.

## 주요특징

1. 독립성 : 각 저장소는 독립적으로 운영되며, 한 프로젝트의 변경사항이 다른 프로젝트에 직접적인 영향을 미치지 않습니다.
2. 유연성 : 프로젝트마다 다른 기술 스택을 사용할 수 있으며, 이는 특정 기술에 대한 의존성을 높여 줍니다.

### 장점

분리된 관리: 각 프로젝트가 독립적으로 개발, 빌드, 배포될 수 있어, 개별 프로젝트 관리가 용이합니다.
기술 스택의 다양성: 서로 다른 프로젝트에서 서로 다른 기술 스택을 자유롭게 선택하여 사용할 수 있습니다.
안정성과 보안: 프로젝트가 분리되어 있기 때문에, 한 프로젝트에서의 문제가 다른 프로젝트로 확산될 가능성이 낮습니다.
접근 제어 용이: 각 저장소별로 접근 권한을 설정하여, 보안을 강화할 수 있습니다.

### 단점

종속성 관리의 복잡성: 여러 저장소에 걸쳐 공통된 종속성을 일관되게 관리하는 것이 어려울 수 있습니다.
통합 및 테스팅의 복잡성: 독립적인 서비스나 애플리케이션들을 함께 통합하고 테스트하는 과정이 복잡해질 수 있습니다.
중복 코드: 공통 기능이나 구성 요소가 여러 저장소에 걸쳐 중복될 가능성이 있습니다.
프로젝트 관리의 어려움: 여러 저장소를 동시에 관리해야 하므로 프로젝트 관리가 더 복잡해질 수 있습니다.
