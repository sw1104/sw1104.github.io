---
title: "Javascript"
excerpt: "Javascript에 대해"

categories:
  - Javascript
tags:
  - [Javascript]

permalink: /javascript/

toc: true
toc_sticky: true

date: 2022-06-13
last_modified_at: 2022-06-13
---

# Javascript
JavaScript는 웹 페이지에서 복잡한 기능들을 구현할 수 있도록 하는 프로그래밍 언어다.

# Javascript의 적용 예시
나이를 물어보는 버튼을 만들고 나이를 적으면 나이를 말하도록 만들어보자.
```html
<button>How old are you?</button>
```
![](https://velog.velcdn.com/images/sangwoo/post/27728e50-e532-4ae9-8252-266f868e90df/image.png)
나이를 물어보는 이 버튼은 아무런 기능이 없는 상태이기에 눌러도 반응이 없다.

여기에 자바스크립트를 이용해 기능을 만들어보자.
```html
<script>
    const age = document.querySelector("button");
    age.addEventListener("click", number);
    function number() {
    	const number = prompt("write your age");
		age.textContent = `I am ${number} years old`;
	}
</script>
```
이제 버튼을 클릭하면 나이를 적을 수 있는 입력창이 나오고 입력한 값으로 나타난다.

![](https://velog.velcdn.com/images/sangwoo/post/c8551e87-3481-4239-89f6-79602d02e871/image.gif)

이런 식으로 웹 페이지에 동적인 기능을 만들어 줄 수 있다.


## 이 외에도 아주아주 많고 많은 기능들을 할 수 있기에 앞으로 열심히 배우면서 블로그에 작성해 보자.