# 10-3. 메서드 오버라이딩 & 동적 바인딩 (Overriding & Dynamic Binding)

## 1) 오버라이딩의 기본 개념
자식 클래스가 부모 클래스로부터 상속받은 메서드를 **동일한 시그니처(메서드명 + 매개변수 + 반환타입)** 로 다시 정의하는 것.

```java
class A {
    public void abc() { System.out.println("A"); }
}
class B extends A {
    @Override
    public void abc() { System.out.println("B"); }
}
```

| 구분 | 기준 | 설명 |
|---|---|---|
| 메서드 이름 | 동일해야 함 | abc → abc |
| 매개변수 | 동일해야 함 | ( ) → ( ) |
| 반환타입 | 동일해야 함 | void → void |
| 접근지정자 | **부모보다 좁아질 수 없음** | public → public 가능 / public → default 불가능 |

---

## 2) private 메서드는 오버라이딩 불가
```java
class A {
    private void bcd() { System.out.println("A bcd"); }
}
class B extends A {
    // @Override  ← 오류! 부모 bcd()가 private 이므로 보이지 않음
    void bcd() { System.out.println("B bcd"); } // 이는 "새로운 메서드"임
}
```
- `private` 은 **상속은 되지만 자식에게 보이지 않음**
- 따라서, 오버라이딩 대상이 아니므로 `@Override` 사용 시 **컴파일 오류** 발생

---

## 3) 접근 지정자 규칙 (중요)
오버라이딩 시 **가시성 축소 금지 규칙**

| 부모 메서드 접근지정자 | 자식 클래스 오버라이딩 가능한 범위 |
|---|---|
| public | public |
| protected | protected, public |
| default | default, protected, public |
| private | ✅ 오버라이딩 불가능 (새 메서드 정의됨) |

---

## 4) 동적 바인딩 (실행 시점 결정)
참조 변수의 타입이 아닌 **실제 객체의 타입에 따라** 실행되는 메서드가 결정됨.

```java
A ab = new B();
ab.abc(); // B의 abc() 실행 (오버라이딩 + 동적 바인딩)
```

| 시점 | 결정 기준 |
|---|---|
| 컴파일 시 | 참조 변수 타입 기준으로 호출 가능한 메서드 목록 결정 |
| 실행 시 | **실제 객체 타입** 기준으로 실행될 메서드 선택 (동적 바인딩) |

---

## 5) 예제 코드 (정리 기반)
```java
class A{
    public void abc() { System.out.println("A 클래스의 abc()"); }
    private void bcd() { System.out.println("A 클래스의 bcd()"); }
}

class B extends A{
    @Override
    public void abc() { System.out.println("B 클래스의 abc()"); }

    // private 은 오버라이딩 불가 → 이는 "새로운 메서드"
    void bcd() { System.out.println("B 클래스의 bcd()"); }
}
```

### 실행 해석 요약
| 코드 | 실행 결과 | 이유 |
|---|---|---|
| `A aa = new A(); aa.abc();` | A 클래스의 abc() | A 객체 그대로 |
| `A ab = new B(); ab.abc();` | B 클래스의 abc() | **동적 바인딩** |
| `B bb = new B(); bb.abc();` | B 클래스의 abc() | 오버라이딩 사용 |
| `bb.bcd();` | B 클래스의 bcd() | B에서 새로 정의한 메서드 |
| `ab.bcd();` | 컴파일 오류 | A의 bcd()는 private → A 타입에서 호출 불가 |

---

## 6) 핵심 요약
> **오버라이딩은 “같은 시그니처 재정의”이며, 실행 시에는 실제 객체의 메서드가 실행된다.**  
> **private 메서드는 오버라이딩되지 않으며**, 오버라이딩 시 접근 지정자는 더 넓어질 수만 있다.
