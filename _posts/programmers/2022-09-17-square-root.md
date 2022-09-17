---
title: "정수 제곱근 판별"
excerpt: "임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 ..."

categories:
  - programmers
tags:
  - [programmers, Algorithm]

permalink: /programmers/square-root/

toc: true
toc_sticky: true

date: 2022-09-17
last_modified_at: 2022-09-17
---
# 문제 설명
임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

# 제한 사항
n은 1이상, 50000000000000 이하인 양의 정수입니다.

# 입출력 예
|n|	return|
|---|---|
|121|	144|
|3|	-1|

# 입출력 예 설명
## 입출력 예1
121은 양의 정수 11의 제곱이므로, (11+1)를 제곱한 144를 리턴합니다.

## 입출력 예2
3은 양의 정수의 제곱이 아니므로, -1을 리턴합니다.


# 문제 풀이
```javascript
function solution(n) {
   let a = Math.sqrt(n)
   let b = a+1;
  let c = Math.pow(b, 2)
  
  if(!a === Number.isInteger(a)){
    return -1
  }
  return c
}
```

인자로 받은 n의 제곱근을 `Math.sqrt` 메소드를 이용하여 제곱근을 구해준 다음 +1을 해줘서 `pow` 메소드로 제곱을 한 값을 구해줬다. 그리고 나서 조건문을 이용해서 제곱근이 정수가 아니면 -1을 반환하게 하고 아니면 제곱값을 리턴하게 만들었다.

# 가독성은 떨어지지만 한번 응용?

```javascript
function solution(n){
  return  (!Math.sqrt(n) === Number.isInteger(Math.sqrt(n))) ? -1 :  Math.pow(Math.sqrt(n)+1 ,2)
}
```