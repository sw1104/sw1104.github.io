---
title: "정수 내림차순으로 배치하기"
excerpt: "함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 ..."

categories:
  - programmers
tags:
  - [programmers, Algorithm]

permalink: /programmers/descending/

toc: true
toc_sticky: true

date: 2022-09-18
last_modified_at: 2022-09-18
---

# 문제 설명

함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

# 제한 조건

n은 1이상 8000000000 이하인 자연수입니다.

# 입출력 예

|n|	return|
|---|---|
|118372|	873211|

# 문제 풀이

```javascript
function solution(n) {
  return  parseInt((n+'').split('').sort((x, y) => y - x).join(''))
}
```

한줄로 풀어보기가 재밌어서 어떻게 할까 요리 조리 고민하면서 해결했다. 인자로 받은 숫자를 문자로 바꾼뒤 배열로 나눠주고 `sort()`를 이용하여 내림차순으로 적용되게 한 뒤 `join`으로 배열을 풀어주고 마지막에 `parseInt`로 숫자로 바꾸어주었다.