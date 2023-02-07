---
title: "[JAVA] 메소드 오버라이딩"
excerpt: "자바에서 메소드 오버라이딩(method overriding)에 대해"

categories:
  - Java
tags:
  - [Java]

permalink: /java/overriding

toc: true
toc_sticky: true

date: 2023-02-02
last_modified_at: 2023-02-02
---

# 메소드 오버라이딩(method overriding)

메소드 오버라이딩은 상속 관계에 있는 부모 클래스에서 이미 정의된 메소드를 자식 클래스에서 재정의 하는것을 의미한다. 자식 클래스는 부모 클래스의 private 제어자를 제외한 모든 메소드를 상속받을 수 있다. 이렇게 상속받은 메소드를 그대로 사용해도 되지만, 필요한 동작을 위해서 재정의 하여 사용할 수 있다.

## 오버라이딩을 위한 조건

1. 메소드의 동작만을 재정의하므로 메소드의 선언부는 기존 메소드와 완전히 같아야한다. 단, 메소드의 반환 타입은 부모 클래스의 반환 타입으로 타입 변환할 수 있는 타입이라면 변경 가능하다.
2. 부모 클래스의 메소드보다 접근 제어자를 더  좁은 범위로 변경할 수 없다.
3. 부모 클래스의 메소드보다 더 큰 범위의 예외를 선언할 수 없다.

## 예제

```java
class Camera {
    public void takePicture() {
        System.out.println("사진 촬영한다.");
    }
}

class FactoryCam extends Camera {
    public void takePicture() {
        System.out.println("화재가 일어나면 사진 촬영한다.");
    }
}
```

# 참고

[TCP SCHOOL](http://www.tcpschool.com/java/java_inheritance_overriding)