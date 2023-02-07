---
title: "[JAVA] 메소드 오버로딩"
excerpt: "자바에서 메소드 오버로딩(method overloading)에 대해"

categories:
  - Java
tags:
  - [Java]

permalink: /java/overloading

toc: true
toc_sticky: true

date: 2023-01-27
last_modified_at: 2023-01-27
---

# 메소드 시그니처 (method signature)

메소드 시그니처는 메소드 오버로딩의 핵심으로 메소드의 선언부에 명시되는 매개변수의 리스트를 가리킨다. 
만약 두 메소드가 매개변수의 개수와 타입, 순서까지 모두 같으면 두 메소드의 시그니처는 같다고 할 수 있다.

# 메소드 오버로딩 (method overloading)

메소드 오버로딩은 같은 이름의 메소드를 중복하여 정의하는 것을 의미한다. 원래 한 클래스 내에서 같은 이름의 메소드를 정의할 수 없다. 하지만 매개변수의 개수나 타입을 다르게 주면 같은 이름의 메소드를 작성할 수 있다.
즉, 메소드 오버로딩은 서로 다른 시그니처를 갖는 여러 메소드를 같은 이름으로 정의하는 것이다.

메소드 오버로딩을 사용함으로써 메소드에 사용되는 이름을 절약할 수 있다. 그리고 메소드를 호출할 때 전달해야 할 매개변수의 타입이나 개수에 대해 신경쓰지 않고 호출할 수 있다.

메소드 오버로딩은 객체 지향 프로그래밍(OOP)의 특징 중 하나인 다형화(polymorphism)을 구현하는 방법 중 하나이다.
> 객체 지향 프로그래밍 특징 : 캡슐화, 추상화, 상속화, 다형화

대표적인 예로 `println`이 있다.

## 메소드 오버로딩의 조건

자바에서 메소드 오버로딩이 성립하기 위해서는 두가지 조건을 만족해야한다.

1. 메소드의 이름이 같아야한다.
2. 메소드의 시그니처, 즉 매개변수의 개수나 타입이 달라야한다.

## 메소드 오버로딩 예시

```java
public class Overloading {
    public static int getPower(int number) {
        int result = number * number;
        return result;
    }

    public static int getPower (String strNumber) {
        int number = Integer.parseInt(strNumber);
        return number * number;
    }

    public static int getPower (int number, int exponent) {
        int result = 1;
        for ( int i = 0; i < exponent; i++ ) {
            result *= number;
        }
        return result;
    }

    public static void main(String[] args) {
        // Method Overloading
        System.out.println(getPower(3)); // 3 * 3 = 9
        System.out.println(getPower("4")); // 4 * 4 = 16
        System.out.println(getPower(4, 5)); // 4 * 4 * 4 * 4 * 4 = 1024
    }
}
```

# 참고

[TCP SCHOOL](http://www.tcpschool.com/java/java_usingMethod_overloading)