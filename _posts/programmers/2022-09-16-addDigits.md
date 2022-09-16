---
title: "자릿수 더하기"
excerpt: "자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 ..."

categories:
  - programmers
tags:
  - [programmers, Algorithm]

permalink: /programmers/add-digits/

toc: true
toc_sticky: true

date: 2022-09-16
last_modified_at: 2022-09-16
---

# 문제 설명

자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.
예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

# 제한사항

N의 범위 : 100,000,000 이하의 자연수

# 입출력 예

|N	|answer|
|---|---|
|123	|6|
|987	|24|

# 입출력 예 설명

### 입출력 예 1

문제의 예시와 같습니다.

### 입출력 예 2

9 + 8 + 7 = 24이므로 24를 return 하면 됩니다.

# 문제 풀이
## 내 풀이

```javascript
function solution(n) {
    let answer = 0;
    let str = n.toString();
    let arr = str.split('');
    for(let i = 0; i < arr.length; i++){
        answer += parseInt(arr[i])
    }
     return answer;
}
```
초기값 0을 두고 인자 n을 문자로 만든 다음 split으로 쪼개서 배열로 담고 for문을 돌린다음 answer에 더한다.

## 다른 사람 풀이
```javascript
function solution(n){
    return String(n).split('').reduce((a,b) => a+b*1, 0);
}
```
인자 n을 문자로 만들고 split한 뒤 reduce를 써서 initial value에 0을 줘서 a = 0으로 만든 뒤 앞에서 만들어준 배열의 값을 더해준다.


# Feeling
동작하는 원리는 비슷하지만 reduce에 대한 지식이 부족하다보니 적용할 생각을 못했다.

## 다른 사람 풀이를 보고 응용

연산자`+`를 사용하면 피연산자 중 하나라도 문자열이면 문자가 아닌 피연산자도 문자열로 변환된다. 그래서 이것도 응용하고 `map`을 이용해서도 풀 수 있을거 같아서 `reduce`, `map`을 이용해서 한번 풀어봤다.
### reduce
```javascript
const solution = n => (n+'').split('').reduce((a,b) => a+parseInt(b) , 0);
```

### map
```javascript
function solution(n){
  let answer = 0;
  (n+'').split('').map(a => answer += parseInt(a))
  return answer
} 
```