# 10-9 ~ 10-14 요약: 추상 클래스 & 익명 이너 클래스 & 인터페이스 (default/static/접근제어)

> 10-8 이후에 배운 핵심만 간단 요약

---

## 10-9. 추상 클래스(abstract)의 역할
- **의미**: 미완성 설계도. **객체 직접 생성 불가**, 자식이 구현해야 할 **추상 메서드**를 선언.
- **목적**: 공통 규약(메서드 시그니처) **강제** + 부분 구현(공통 로직) **공유** 가능.
- **형태**
  ```java
  abstract class A { abstract void run(); }
  class B extends A { void run() { /* 구현 */ } }
  A a = new B(); // OK
  ```
- **포인트**: “무엇을 할지”를 정하고, “어떻게”는 자식이 채운다.

---

## 10-10. 자식 클래스 vs 익명 이너 클래스
- **자식 클래스 방식**: 재사용 가능, 클래스 이름 존재.
  ```java
  class B extends A { void run() { /*...*/ } }
  A a1 = new B();
  ```
- **익명 이너 클래스**: **일회성** 즉시 구현, 파일/이름 없이 간단.
  ```java
  A a2 = new A(){ void run(){ /*...*/ } };
  ```
- **선택 기준**
  - 여러 곳에서 쓰면 **자식 클래스**
  - 한 번 쓰면 **익명 이너 클래스**

---

## 10-11. 인터페이스의 개념 & 역할
- **역할**: “무엇을 할지(메서드 형식)”만 규정하는 **규칙/약속** → 다형성의 기반.
- **필드/메서드 기본 제어자**
  - 필드: `public static final` (상수, 반드시 초기화)
  - 메서드: `public abstract` (구현 강제)
  ```java
  interface X { int N = 3; void work(); } // 실제로는 public static final / public abstract
  class Y implements X { public void work(){ /*...*/ } } // public 유지 필수
  ```

---

## 10-12. 인터페이스 메서드 접근제어 규칙 (오버라이딩 시 가시성 축소 금지)
- **인터페이스 메서드는 public** → 구현(오버라이딩) 시 **반드시 public 유지**.
  ```java
  interface B { void bcd(); } // public abstract
  class D implements B {
    // void bcd() { }   // ❌ default(패키지)로는 컴파일 오류
    public void bcd() { } // ✅
  }
  ```

---

## 10-13. default / static 메서드
- **default 메서드**: 구현이 있는 인스턴스 메서드(접근은 **public**). 오버라이딩 시에도 **public** 유지.
  ```java
  interface A {
    default void hello(){ System.out.println("A.hello"); } // public default
  }
  class B implements A {
    @Override public void hello(){ System.out.println("B.hello"); } // public 유지
    void callParent(){ A.super.hello(); } // 부모 default 호출
  }
  ```
- **static 메서드**: **오버라이딩 불가**, 타입명으로 호출.
  ```java
  interface A { static void hi(){ System.out.println("A.hi"); } }
  A.hi();                // ✅
  // new B().hi();       // ❌ (권장 X, 정적은 타입 소속)
  ```

---

## 10-14. 인터페이스 객체 생성 방법 요약
1) **구현 클래스 사용**
   ```java
   class Impl implements A { public void abc(){ /*...*/ } }
   A a1 = new Impl();
   ```
2) **익명 이너 클래스**
   ```java
   A a2 = new A(){ public void abc(){ /*...*/ } };
   ```
> (참고) 함수형 인터페이스면 람다도 가능: `A a3 = () -> { /*...*/ };`

---

## 체크리스트
- 인터페이스 **필드 = 상수**(public static final) → 값 변경 불가, 반드시 초기화.
- 인터페이스 메서드 **구현 시 public 유지**(가시성 축소 금지).
- **default**: 인스턴스 메서드, `A.super.m()`로 부모 default 호출 가능.
- **static**: 타입 소속, **오버라이딩 불가**, `A.m()`으로 호출.
- 추상 클래스: 공통 로직 공유 + 미완성 메서드 강제, **객체 직접 생성 불가**.
- 익명 이너 클래스: 일회성 구현이 필요할 때 간결하게.

---

### 한 줄 요약
> **추상 클래스 = 미완성 설계 + 일부 구현 공유,  
> 인터페이스 = 규칙/다형성, default(인스턴스 구현)/static(타입 소속),  
> 구현 시 public 유지 & static은 오버라이딩 불가.**
