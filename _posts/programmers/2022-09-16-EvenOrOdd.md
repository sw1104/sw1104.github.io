---
title: "짝수와 홀수"
excerpt: "정수 num이 짝수일 경우 "Even"을 반환하고 홀수인 경우 ..."

categories:
  - programmers
tags:
  - [programmers, Algorithm]

permalink: /programmers/even-odd/

toc: true
toc_sticky: true

date: 2022-09-16
last_modified_at: 2022-09-16
---
# 문제 설명
정수 num이 짝수일 경우 "Even"을 반환하고 홀수인 경우 "Odd"를 반환하는 함수, solution을 완성해주세요.

# 제한 조건
- num은 int 범위의 정수입니다.
- 0은 짝수입니다.

# 입출력 예 
|num|return|
|---|---|
|3|"Odd"|
|4|"Even"|

# 문제 풀이
```javascript
function solution(num) {
    return num % 2 ? "Odd" : "Even";
}
```

삼항연산자를 통해 풀이하였다.

주어진 인자 num을 2로 나누었을 때 나머지가 1이면 True 아니면 False를 이용하였다.

- True : Odd
- False : Even
