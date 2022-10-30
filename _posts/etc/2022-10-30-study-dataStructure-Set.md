---
title: "취업스터디 대비 - 자료구조 - Set"
excerpt: "자료구조 Set에 대해서"

categories:
    - etc
tags:
    - [data-structure, Set]

permalink: /etc/data-structure/Set

toc: true
toc_sticky: true

date: 2022-10-30 11:45:30
last_modified_at: 2022-10-30 11:45:30
---

# Set
Set은 Array나 List 처럼 순열 자료구조 이다. 하지만 Set은 순서라는 개념이 존재하지 않다.

# Set의 특징
- 데이터를 비순차적(unordered)으로 저장할 수 있는 순열 자료구조이다.
- 삽입 순서대로 저장되지 않는다. 즉, 특정한 순서를 기대할 수 없는 자료구조이다.
- 수정이 가능하다.
- 동일한 값을 여러번 삽입 불가능 하다. 
```python
// set 예시
>>> mySet = {1, 2, 3, 4, 5, 4, 3, 2, 1} // {1, 2, 3, 4, 5}
>>> for i in mySet:
        print(i)
1
2
3
4
5

>>> mySet.append(7) 
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    AttributeError: 'set' object has no attribute 'append'
>>> mySet[0]
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    TypeError: 'set' object does not support indexing
```

# Set의 구조
Set에서 요소들이 저장될 때 순서는 다음과 같다.
    1. 저장할 요소의 값의 hash 값을 구한다.
    2. hash 값에 해당하는 공간(bucket)에 값을 저장한다.
이렇게 Set은 저장하고자 하는 값의 hash 값에 해당하는 bucket에 값을 저장하기 때문에 순서가 없고 그렇기 때문에 indexing도 없다. 그리고 hash 값을 bucket에 저장하기 때문에 중복된 값을 저장할 수 없다. 그러나 hash 값을 기반으로 저장하기 때문에 Look up이 굉장히 빠르다.
    - Look up: 특정 값을 포함하고 있는지를 확인 하는 것 => `3 in mySet`
    - Set의 총 길이와 상관없이 hash 값을 계산 후 해당 bucket을 확인한다.

> hash는 단방향(one way) 암호화 이다. 단방향이란 한번 암호화 하면 복호화가 안된다는 뜻이다. 실제 값의 길이와 상관없이 hash 값은 일정한 길이를 가진다. 동일한 값에 대한 hash 값은 항상 일정한 값이 나오므로, 특정 요소를 가르키는 index 역할이 가능하다.

# 언제 Set을 사용하는가
- 중복된 값을 골라내야 할 때
- 특정 요소(element)의 존재 여부에 관해 빠른 Look up을 해야 할 때
- 순서가 상관 없을 때
- 합집합(Union), 교집합(Intersection), 차집합(Difference) 등을 구해야 할 때