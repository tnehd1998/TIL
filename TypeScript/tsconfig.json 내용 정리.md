# 📌 해당 내용을 학습하게된 계기

항상 create-react-app , create-next-app으로 프로젝트를 생성하여
기본으로 설정된 tsconfig.json을 사용하기만 했었다.

tsconfig.json에는 어떤 옵션들이 존재하는지 궁금해졌다.

## 기본 옵션

### incremental / tsBuildInfoFile

- 이전 컴파일 단계에서의 디스크의 파일로 정보를 읽거나 기록하는 기능
- 만약에 사용을 한다면 tsBuildInfoFile을 통해 저장할 위치를 지정할 수 있다.

### target

- 타입스크립트 파일을 어떤 버전의 자바스크립트로 바꿔줄지 정하는 부분

### module

- 자바스크립트 파일간의 import 문법을 구현할 때의 문법을 결정하는 부분
  - commonjs로 설정 : require 문법 사용
  - es2015, esnext로 설정 : import 문법 사용

### lib

- 타입스크립트 파일을 자바스크립트로 컴파일 할 때 포함될 라이브러리의 목록을 작성하는 부분

### allowJS

- 타입스크립트 컴파일 작업을 진행할 때 자바스크립트 파일도 포함될 수 있는지를 설정하는 부분
- js 파일을 ts에서 import하여 쓸 수 있게하는 기능
- 자바스크립트 프로젝트에서 타입스크립트를 적용할 때 사용하면 좋은 속성

### checkJS

- js 요소가 존재하는지 확인하는 기능
- allowJS를 설정하지 않으면 js를 허락하지 않는다는 의미이기에 allowJS가 true인 경우에만 설정을 해야 작동한다고 볼 수 있다.

### jsx

- preserve : jsx를 출력 일부로 유지하며 babel과 같은 다른 변환단계에서 추가로 사용 가능
- react : React.createElement를 출력하며 출력 파일 확장자는 .js
- react-native : preserve와 같이 모든 jsx를 유지하지만 출력 파일 확장자가 .js

### declaration / declarationMap

- declaration : 모든 .d.ts 파일을 프로젝트에 생성함
- declarationMap : 모든 .d.ts 파일에 맞는 source map을 생성함

### sourceMap

- 이해를 위한 배경 지식 : 프로젝트를 배포하게 되면 코드를 transpile된 해야하는데, 자바스크립트 파일은 압축, 컴파일, 최소화의 과정을 통해 사람이 직접 읽기 힘든 언어로 컴파일 되어버린다.
- 그렇기에 배포중에 에러가 발생한다면, transpile된 코드를 보고 버그를 찾아내는 것은 쉽지 않다.
- 해당 상황을 해결하기 위해 존재하는 요소가 map 파일이다.
- “.map”파일은 코드를 변역해주는 역할을 해준다고 생각하면 된다.
- 해당 요소는 타입스크립트와 제거된 자바스크립트 요소를 지니고 있다.
- 빌드된 결과물이 모두 .map이라는 파일로 생성된다.
- sourceMap을 통해 배포 단계의 버그를 더 쉽게 잡아낼 수 있다.

### outFile

- 단일 파일로 출력을 원하는 파일명을 입력하는 부분

### outDir

- 컴파일 후 생성되는 js파일이 저장될 폴더 명을 입력하는 부분

### composite

- 프로젝트가 컴파일 여부를 빠르게 확인하기 위한 설정

### removeComments

- 주석 제거의 유무를 설정

### noEmit

- 결과 파일의 저장 유무를 설정

### importHelpers

- tslib에서 helpers를 가져오는지 설정

### downlevelIteration

- target이 ES6이전의 ES3, ES5 일때도 spread, for...of, destructuring 문법을 지원하는지 설정

### isolatedModules

- 각 파일을 분리된 모듈로 트랜스파일링의 진행 여부를 결정

</br>

## 엄격한 타입 확인 옵션

### strict

- 모든 엄격한 타입 확인 옵션의 활성화 여부

### noImplicitAny / noImplicitThis

- any / this 타입을 사용하는 표현식 또는 정의 에러처리의 여부

### strictNullChecks / strictFunctionChecks / strictBindCallChecks

- 엄격한 null / function / bind, call, apply 메서드의 사용 여부를 확인

### strictPropertyInitialization

- 클래스 값 초기화에 대한 엄격한 확인 여부

### alwaysStrict

- 항상 모든 파일에 strict mode로 분석 여부를 결정 및 추가

</br>

## 추가적인 확인

### noUnusedLocals / noUnusedParameters

- 사용되지 않은 지역 변수 / 파라미터에 대한 에러 보고의 여부

### noImplicitReturns

- 함수에서 코드의 모든 경로가 값을 반환하지 않을 때의 에러 보고의 여부

### noFallthroughCasesInSwitch

- 잘못 작성된 switch 문에 대한 에러 보고의 여부

### noUncheckedIndexedAccess

- 선언하지 않은 값에 undefined를 할당하는 기능

### noImplicitOverride

- override 하지 않은 요소의 에러 보고의 여부

### noPropertyAccessFromIndexSignature

- 특정 인덱스 요소를 접근할 때 obj.key가 아닌 obj[”key”]로만 접근을 허용하는 기능

</br>

## 모듈 해석 옵션

### moduleResolution

- 모듈의 해석 방법을 설정
- node : Node.js
- classic: TypeScript pre-1.6

### baseUrl

- 모듈 이름을 처리할 기준 디렉토리를 지정

### paths

- baseUrl을 기준으로 모듈의 위치를 재지정

### rootDirs

- 프로젝트 구조를 나타내는 루트 폴더들의 목록을 나열

### typeRoots

- 타입 정의를 포함할 폴더 목록을 지정
- 기본적으로 ./node_modules/@types로 초기화됨

### types

- 컴파일 중 포함될 타입 정의의 파일 목록을 작성

### allowSyntheticDefaultImports

- default export가 아닌 모듈에서도 default import가 가능하게 할 지의 여부를 결정

### esModuleInterop

- 모든 imports에 대한 namespace 생성을 통해 CommonJS와 ES Modules 간의 상호작용의 가능 여부를 결정

### preserveSymlinks

- 심볼릭 링크 파일에서의 다른 모듈을 import를 할 때 기준이 되는 경로를 심볼릭 링크 경로로 할것인지를 결정

### allowUmdGlobalAccess

- UMD 전역을 모듈에서 접근 허용 여부를 결정

</br>

## 소스 맵 옵션

### sourceRoot

- 소스 위치 대신 디버거에게 제공할 TypeScript 파일의 위치를 지정

### mapRoot

- 생성된 위치 대신 디버거에서 제공할 맵 파일의 위치를 지정

### inlineSourceMap

- 분리된 파일을 가지고 있으며 단일 파일을 소스 맵과 함께 가지고 있을지를 결정

### inlineSources

- 소스와 소스 맵을 함께 단일 파일로 내보낼지를 결정

</br>

## 실험적 옵션

### experimentalDecorators

- ES7의 decorators의 포함 여부

### emitDecoratorMetadata

- decorator를 위한 타입 메타데이터를 내보내는 것에 대한 지원 여부

</br>

## 추가적 옵션

### skipLibCheck

- 정의 파일의 타입 확인을 할것인지를 결정

### forceConsistentCasingInFileNames

- 일관되지 않는 참조의 허용 유무

### [테크 블로그에 그림과 함께 더 쉽게 풀어쓴 글](https://velog.io/@tnehd1998/tsconfig.json-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%AC)
