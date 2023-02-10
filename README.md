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

# 2. 모듈이란?

- ECMAScript 2015부터 JavaScript에는 모듈 개념이 있었고, TypeScript는 이 개념을 공유한다.
- 모듈은 소프트웨어 개발에서 코드의 분리 및 재사용을 가능하게 하는 개념이다.
- 모듈은 코드, 데이터, 리소스 등을 하나의 단위로 구성하여 관리하고, 다른 부분에서 필요할 때 가져다 사용할 수 있도록한다.
- 이는 코드의 중복을 줄여주고, 개발 과정에서 코드의 관리 및 유지보수가 용이해진다.
- 모듈은 주로 프로그래밍 언어의 라이브러리와 프레임워크의 구성 단위로 사용된다.
- 모듈화는 TypeScript 코드를 모듈 단위로 구성하고, 코드의 공유와 재사용을 가능하게 하는 것이다.
- 각 모듈에서 정의된 객체, 함수 및 변수는 외부 모듈에서 사용할 수 있으며, 내부 모듈에서는 숨겨진다.

- 소스 코드를 여러개의 모듈로 분할하면 각각의 모듈에 어떠한 내용이 있는지 서로에게 `export`와 `import` 키워드를 이용해서 알려줘야한다.
- `export`는 기능을 제공하는 쪽에서, `import` 제공받은 기능을 이용하는 쪽에서 사용한다.

## 2.1 index.ts 파일 생성

```typescript
// src/index.ts

const MAX_AGE = 100;

interface IPerson {
  name: string;
  age: number;
}

class Person implements IPerson {
  constructor(public name: string, public age: number) {}
}

function makeRandomNumber(max: number = MAX_AGE): number {
  return Math.ceil(Math.random() * max);
}

const makePerson = (name: string, age: number = makeRandomNumber()) => ({
  name,
  age,
});

const testMakePerson = (): void => {
  let jane: IPerson = makePerson("Jane");
  let jack: IPerson = makePerson("Jack");
  console.log(jane, jack);
};

testMakePerson();
```

## 2.2 index.ts 모듈화하기

- `src/index.ts` 파일을 모듈화 하기 위해 src 디렉터리에 `person` 디렉터리를 생성 후 `Person.ts` 파일을 만듬.
- 그 후 `src/index.ts` 파일의 내용을 `src/person/Person.ts`로 옮긴다.

```typescript
// src/person/Person.ts

const MAX_AGE = 100;

interface IPerson {
  name: string;
  age: number;
}

class Person implements IPerson {
  constructor(public name: string, public age: number) {}
}

function makeRandomNumber(max: number = MAX_AGE): number {
  return Math.ceil(Math.random() * max);
}

const makePerson = (name: string, age: number = makeRandomNumber()) => ({
  name,
  age,
});
```

```typescript
// src/index.ts

const testMakePerson = (): void => {
  let jane: IPerson = makePerson("Jane");
  let jack: IPerson = makePerson("Jack");
  console.log(jane, jack);
};

testMakePerson();
```

- 코드를 실행하면 `IPerson`과 `makePerson`을 찾을 수 없다고 나온다.

```bash
npm run dev

> 01@1.0.0 dev
> ts-node src

/Users/user/Desktop/code/TypeScript/ts/node_modules/ts-node/src/index.ts:859
    return new TSError(diagnosticText, diagnosticCodes, diagnostics);
           ^
TSError: ⨯ Unable to compile TypeScript:
src/index.ts:2:13 - error TS2304: Cannot find name 'IPerson'.

2   let jane: IPerson = makePerson("Jane");
              ~~~~~~~
src/index.ts:2:23 - error TS2304: Cannot find name 'makePerson'.

2   let jane: IPerson = makePerson("Jane");
                        ~~~~~~~~~~
src/index.ts:3:13 - error TS2304: Cannot find name 'IPerson'.

3   let jack: IPerson = makePerson("Jack");
              ~~~~~~~
src/index.ts:3:23 - error TS2304: Cannot find name 'makePerson'.

3   let jack: IPerson = makePerson("Jack");

...


```

- 이를 해결하기 위해 `export`, `import` 키워드를 사용한다.

## 2.3 export

- 모듈의 내용을 외부에서 사용할 수 있도록 공개하는 기능
- `export` 키워드를 추가하여 모든 선언 (변수, 함수, 클래스, 타입 별칭, 인터페이스)를 `export` 할 수 있다.

```typescript
// src/person/Person.ts

const MAX_AGE = 100;

export interface IPerson {
  name: string;
  age: number;
}

class Person implements IPerson {
  constructor(public name: string, public age: number) {}
}

function makeRandomNumber(max: number = MAX_AGE): number {
  return Math.ceil(Math.random() * max);
}

export const makePerson = (name: string, age: number = makeRandomNumber()) => ({
  name,
  age,
});
```

## 2.4 import

> import { 심벌, 심벌... } from "file의 상대 경로"
> 타입스크립트 파일을 import할 때는 보통 확장자 .ts 를 생략

- 어떠한 파일이 `export` 키워드로 내보낸 심벌을 받아서 사용하려면 `import` 키워드로 해당 심벌을 불러와야 한다.

```typescript
// src/index.ts

import { IPerson, makePerson } from "./person/Person";

const testMakePerson = (): void => {
  let jane: IPerson = makePerson("Jane");
  let jack: IPerson = makePerson("Jack");
  console.log(jane, jack);
};

testMakePerson();
```

### 2.4.1 import \* as

> import \* as 심벌 from "file의 상대 경로"

- `src/utils` 디렉터리에 `makeRandomNumber.ts` 파일을 만들고 `Person.ts`에서 내용을 옮겨 적는다.

```typescript
// src/utils/makeRandomNumber.ts

export interface IPerson {
  name: string;
  age: number;
}

class Person implements IPerson {
  constructor(public name: string, public age: number) {}
}

export const makePerson = (name: string, age: number = makeRandomNumber()) => ({
  name,
  age,
});
```

- `Person.ts` 파일에는 `import * as` 구문 사용
  - `makeRandomNumber.ts`를 모듈로 분리했으며,
  - `Person.ts` 파일에서 `U`라는 심벌로 접근할 수 있도록 `import * as ` 구문 사용함.

```typescript
// src/person/Perso.ts

import * as U from "../utils/makeRandomNumber";

export interface IPerson {
  name: string;
  age: number;
}

class Person implements IPerson {
  constructor(public name: string, public age: number) {}
}

export const makePerson = (
  name: string,
  age: number = U.makeRandomNumber()
): IPerson => ({ name, age });
```

## 2.5 export default

- 타입스크립트는 자바스크립트와 호환하기 위해 `export default` 구문을 제공.
- `export default`는 타입스크립트에서 디폴트로 export하는 것을 의미.
- 하나의 모듈에서는 여러 개의 `export default`를 가질 수 없지만,
  하나의 모듈에서 여러 개의 export와 함께 export default를 가질 수 있다.
- `export default`가 붙은 기능은 `import`로 불러올 때 중괄호`{}` 없이 사용 가능.

- `person` 디렉터리에 `IPerson.ts` 파일을 만들고 아래와 같이 작성

```typescript
// src/person/IPerson.ts

export default interface IPerson {
  name: string;
  age: number;
}
```

- `Person.ts` 파일 아래와 같이 수정

```typescript
// src/person/Person.ts

import { makeRandomNumber } from "../utils/makeRandomNumber";
import IPerson from "./IPerson";

export default class Person implements IPerson {
  constructor(public name: string, public age: number = makeRandomNumber()) {}
}

export const makePerson = (
  name: string,
  age: number = makeRandomNumber()
): IPerson => ({ name, age });
```

- `export default`로 불러온 `IPerson`을 중괄호 없이 `import`에 지정하고 사용했다.
- `export default`를 `Person` 클래스에 사용해서 내보냈다.

- `Person` 클래스를 `index.ts`에서 아래와 같이 불러오기

```typescript
// src/index.ts

import IPerson from "./person/IPerson";
import Person, { makePerson } from "./person/Person";

const testMakePerson = (): void => {
  let jane: IPerson = makePerson("Jane");
  let jack: IPerson = makePerson("Jack");
  console.log(jane, jack);
};

testMakePerson();
```

- `IPerson`과 달리,
  `Person.ts` 파일은 `export default`로 지정된 `Person`과 `export`로 지정된 `makePerson` 두개의 심벌이 있는데 각각 중괄호의 유무가 다르다.

## 2.6 외부 패키지 import

- 외부 패키지를 `import`하기위해 패키지 설치

```bash
> npm i -S chance ramda

> npm i -D @types/chance @types/ramda
```

- -S 옵션으로 설치된 `chance`, `ramda`는 dependencies 항목
- -D 옵션으로 설치된 `@types/chance` `@types/ramda`는 devDependencies 항목
  - `chance` : 난수 생성기 패키지로, 다양한 형식의 랜덤 데이터를 생성하기 위한 간단한 API를 제공
    - 랜덤 문자열, 날짜, 숫자, 주소 등을 생성
    ```typescript
    // chance 사용 예
    import * as Chance from "chance";
    const chance = new Chance();
    console.log(chance.string()); // QpRKj0iokIOg8WzP
    ```
  - `ramda` : 함수형 프로그래밍을 위한 패키지로, 자바스크립트와 타입스크립트에서 함수형 프로그래밍에 필요한 여러 가지 함수를 제공
    - 배열의 매핑, 필터링, 리듀싱, 커링 등의 기능을 간편하게 사용
    ```typescript
    // ramda 사용 예
    import * as R from "ramda";
    const numbers = [1, 2, 3, 4, 5];
    const doubledNumbers = R.map((x) => x * 2, numbers);
    console.log(doubledNumbers); // [2, 4, 6, 8, 10]
    ```

```json
// package.json

...

"devDependencies": {
    "@types/chance": "^1.1.3",
    "@types/node": "^18.13.0",
    "@types/ramda": "^0.28.22",
    "ts-node": "^10.9.1",
    "typescript": "^4.9.5"
  },
  "dependencies": {
    "chance": "^1.1.9",
    "ramda": "^0.28.0"
  }
}
```

- `index.ts` 파일 수정

```typescript
// src/index.ts

import IPerson from "./person/IPerson";
import Person from "./person/Person";
import Chance from "chance";
import * as R from "ramda";

const chance = new Chance();

let persons: IPerson[] = R.range(0, 2).map(
  (n: number) => new Person(chance.name(), chance.age())
);

console.log(persons);
```

- `chance` 패키지는 `Chance` 클래스 하나면 `export default`로 제공하기 때문에 `import Chance from "chance";` 처럼 중괄호 없이 `import`
- 하지만 `ramda` 패키지는 다양한 기능을 제공하기 때문에 `import * as R from "ramda";` 처럼 모듈에서 `export`한 모든 것을 가져와 한 객체에 담아서 사용할 수 있도록 함.
- 또한 `chance`와 `ramda`는 외부 패키지이므로 `node_modules` 디렉터리에 있기 때문에 경로에서 `./`를 생략하고 사용한다.
