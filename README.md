# study-notes

# 상속과 업캐스팅 (Inheritance & Upcasting)

## 1. 개념 정리

| 용어 | 의미 | 예시 |
|---|---|---|
| 상속 (inheritance) | 자식 클래스가 부모 클래스의 필드와 메서드를 물려받는 것 | `class Child extends Parent` |
| 업캐스팅 (upcasting) | 자식 객체를 부모 타입 참조변수로 참조하는 것 | `Parent p = new Child();` |
| 다운캐스팅 (downcasting) | 부모 타입 참조를 다시 자식 타입으로 변환하는 것 | `Child c = (Child)p;` |

---

## 2. 업캐스팅 시 멤버 접근 범위

```java
Parent pc = new Child();
```

- 참조 변수 `pc`의 **타입은 Parent**
- 실제 객체는 Child 이지만,
- **자바에서는 "참조 변수의 타입"이 접근 가능한 멤버를 결정한다.**

즉,

| 접근 대상 | 가능 여부 | 이유 |
|---|---|---|
| Parent로부터 물려받은 필드/메서드 | ✅ 가능 | Parent 타입이 알고 있는 멤버이므로 |
| Child에서 새로 추가한 필드/메서드 | ❌ 불가능 | Parent 타입 입장에서 존재하지 않는 멤버로 취급됨 |

예)
```java
Parent pc = new Child();
pc.method1(); // 가능 (Parent에 존재)
pc.method2(); // 불가능 (Child 고유 메서드)
```

---

## 3. 핵심 정리 문장

> **자바에서는 실행 시 실제 객체가 무엇인지와 상관없이,  
> 컴파일 시 참조 변수의 타입 기준으로 접근 가능한 멤버가 결정된다.**

---

## 4. 비유로 이해하기

```
Parent (부모 타입의 안경)
└─ method1(), a

Child
└─ method2(), b, method1(), a
```

`Parent pc = new Child();` 은  
**Parent 안경으로 Child를 보는 것과 같다.**  
Child 안에 더 많은 기능(method2, b)가 있어도 **안경에는 안 보인다.**

---

## 5. 예제 코드

```java
class Parent {
    int a = 10;
    void method1() {
        System.out.println("Parent - method1()");
    }
}

class Child extends Parent {
    int b = 20;
    void method2() {
        System.out.println("Child - method2()");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent pc = new Child();
        pc.method1(); // OK
        // pc.method2(); // 컴파일 오류
    }
}
```
