---
title: "[TypeScript] any와 unknown"
excerpt: "TypeScript any와 unknown에 대해"

categories:
  - TypeScript
tags:
  - [TypeScript]

permalink: /typescript/any&unknown

toc: true
toc_sticky: true

date: 2023-02-26
last_modified_at: 2023-02-26
---

# any & unknown

`unknown` 타입은 typescript 3.0 버전에서 나온 타입이다. `any` 처럼 `unknown` 타입에도 어떤 타입의 값이든 할당할 수 있다. 하지만 `any` 타입의 값은 어떤 타입의 변수에든 할당이 되지만 `unknown` 타입의 값은 `any` 타입과 `unknown` 타입에만 할당할 수 있다.

간단히 예시를 보면

```javascript 
let aUnknown: unknown;
let bAny: any;

aUnknown = 123;
bAny = 123;

aUnknown = '123';
bAny = '123';

let cNum: number;
cNum = bAny;
cNum = aUnknown; // Type 'unknown' is not assignable to type 'number'.
```

위에 예시를 살펴보면 `any`나 `unknown` 둘 다 어느 타입의 값이든 할당이 되지만 `number` 타입에 할당 할 시 `any`는 잘 들어가지만 `unknown`은 컴파일 에러가 난다. 

`unknown`가 등장한 이유는 `any`처럼 어떤 타입의 값이든 할당할 수 있지만 실제로 사용할 때는 개발자에게 타입을 체킹하도록 만들기 위한 타입이 필요하여 등장하게 되었다. 예를 들어 사용자로부터 입력을 받거나 잘 알려지지 않은 외부 api를 사용하는 등 실제 어떤 값이 올 지 모를 상황에서 처음 어떠한 값이든 할당할 수 있지만 그 이후에 그 값을 사용할 때에는 타입을 체크해서 안전하게 사용하기 위해 쓰기 위함이다.

# 참고
[eunji Blog : 타입스크립트 any, unknown](https://0119eunji.tistory.com/114)
[Roseline Blog : [Typescript] unknown vs any type](https://roseline.oopy.io/dev/typescript-unknown-vs-any-type)