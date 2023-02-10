# Practice-TypeScript

# 1. 타입스크립트 프로젝트 만들기

## 1.1 Install

```bash
> npm i -D typescript ts-node @types/node
```

> --save-dev (-D) <br>
> 프로젝트를 개발할 때만 필요한 패키지로 설치. <br>
> 패키지 정보가 package.json 파일의 "devDependencies"에 등록

- ts-node

  - Typescript로 작성된 앱을 브라우저 외부에서 실행할 수 있게 해주는 Node.js 패키지
  - Typescript는 코드작성을 마친후, javascript로 변환(컴파일)되는데,
  - Node.js에서는 javascript만 사용하므로, 변환 -> 실행해야 하는 번거로움이 있다.
  - ts-node는, 이 변환 -> 실행 작업을 한번에 해준다.

- @types/node
  - TypeScript를 사용할 때 Node.js 관련 정의 파일을 제공하는 패키지.
  - Node.js의 전역 변수, 모듈, 콜백 함수 등에 대한 타입 정보를 갖고 있어서 TypeScript를 사용하는 개발자들이 Node.js 관련 코드를 개발할 때 타입 정보를 제공받아, 오류를 예방하고 코드의 가독성을 높일 수 있다.

## 1.2 [tsconfig.json](https://www.typescriptlang.org/ko/docs/handbook/compiler-options.html) 만들기

- tsconfig.json은 타입스크립트를 자바스크립트로 변환할 때의 컴파일 설정을 정의 해놓는 파일.
- 프로젝트를 컴파일 하는데 필요한 루트 파일, 컴파일러 옵션 등을 상세히 설정할 수 있다.
- 보통 tsconfig.json 파일은 TypeScript 프로젝트의 루트 디렉토리(Root Directory)에 위치된다.
- tsconfig에서 옵션들을 정의해 놓으면, 더이상 컴파일 할때 대상 파일이나 옵션을 지정하지 않아도 된다.

```bash
> tsc --init
```

- tsconfig.json은 타입스크립트를 자바스크립트로 변환할 때의 컴파일 설정을 정의 해놓는 파일.
- 프로젝트를 컴파일 하는데 필요한 루트 파일, 컴파일러 옵션 등을 상세히 설정할 수 있다.
- 보통 tsconfig.json 파일은 TypeScript 프로젝트의 루트 디렉토리(Root Directory)에 위치된다.
- tsconfig에서 옵션들을 정의해 놓으면, 더이상 컴파일 할때 대상 파일이나 옵션을 지정하지 않아도 된다.

> 컴파일 대상 경로를 정의하는 속성의 우선 순위 files > include = exclude

```json
// tsconfig.json
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "target": "ES5",
    "moduleResolution": "node",
    "outDir": "dist",
    "baseUrl": ".",
    "sourceMap": true,
    "downlevelIteration": true,
    "noImplicitAny": false,
    "paths": { "*": ["node_modules"] }
  },
  "include": ["src/**/*"] // ./src와 ./src/utils 디렉터리에 프로젝트의 모든 타입스크립트 소스 파일이 있다는 뜻
}
```

## 1.3 source file 생성

```bash
> mkdir -p src/utils

> touch src/index.ts src/utils/makePerson.ts
```

> -p (--parents) 상위 경로도 함께 생성

```typescript
// src/utils/makePerson.ts

export function makePerson(name: string, age: number) {
  return { name: name, age: age };
}

export function testMakePerson() {
  console.log(makePerson("Son", 30), makePerson("Kane", 28));
}
```

```typescript
// src/index.ts
import { testMakePerson } from "./utils/makePerson";
testMakePerson();
```

## 1.4 package.json 수정

```json
// 수정 전

{
  ...

  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },

  ...

}
```

- 타입스크립트 프로젝트를 개발할 때는 `ts-node`를 사용하지만, 개발이 완료되면 타입스크립트 소스를 ES5 자바스크립트 코드로 변환해 Node.js로 실행해야한다.
- 이를 위해 아래와 같이 수정한다.

```json
// 수정 후

{
  ...

  "main": "src/index.js",
  "scripts": {
    "dev": "ts-node src",
    "bulid": "tsc && node dist"
  },

  ...

}
```

- `dev` 명령은 개발 중에 src 디렉터리에 있는 index.ts 파일을 샐힝하는 용도로 사용하며,
- `build` 명령은 개발이 완료된 후 프로그램을 배포하기 위해 dist 디렉터리에 ES5 자바스크립트 파일을 만들때 사용

## 1.5 컴파일 & run

```bash
> npm run dev

> 01@1.0.0 dev
> ts-node src ## dev 명령에 정의된 명렁

{ name: 'Son', age: 30 } { name: 'Kane', age: 28 }  ## 코드 실행 결과
```

## 1.6 build

```bash
> npm run build

> 01@1.0.0 build
> tsc && node dist ## dev 명령에 정의된 명렁

{ name: 'Son', age: 30 } { name: 'Kane', age: 28 }  ## 코드 실행 결과
```

- dist/index.js, dist/utils/makePerson.js로 컴파일 됨
