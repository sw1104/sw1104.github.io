---
title: "자연수 뒤집어 배열로 만들기"
excerpt: "자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열..."

categories:
  - programmers
tags:
  - [programmers, Algorithm]

permalink: /programmers/array-reverse-int/

toc: true
toc_sticky: true

date: 2022-09-17
last_modified_at: 2022-09-17
---

# 문제 설명

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

# 제한 조건

n은 10,000,000,000이하인 자연수입니다.

# 입출력 예

|n|	return|
|---|---|
|12345|	[5,4,3,2,1]|

# 문제 풀이

```javascript
function solution(n) {
    return (n+'').split('').reverse().map(n => parseInt(n))
}
```

어제 문제를 풀면서 배운 연산자 `+`를 이용하여 인자 `n`을 문자열로 만들어준 뒤 `split('')`메소드를 써서 배열로 만들어준 다음 `reverse` 메소드로 반전을 준 다음 `map`과 `parseInt()`로 숫자형으로 만들어주었다.


# 다른 방법
`reverse` 없이 풀이하는 방법도 알기 위해 다음과 같이 풀었다.

```javascript
  let arr = [];
  let sp = String(n).split('')
  for(let i = 0; i < sp.length; i++){
    arr.unshift(Number(sp[i]))
  }
  return arr
```