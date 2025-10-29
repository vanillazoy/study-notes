# 10-5. 메서드 오버로딩 & 접근제어자 (Overloading & Access Modifiers)

## 1) 이것이 오버로딩인가?
```java
class Printer{
    void print() {}
    void print(String msg) {}
}
```
**예, 오버로딩(Overloading)입니다.**  
- 같은 이름의 메서드 `print`가 **매개변수 목록이 다르게** 정의되어 있음  
- 반환 타입/접근지정자와 상관없이 **매개변수 시그니처(개수, 타입, 순서)** 가 다르면 오버로딩

> ⚠️ 단, **반환 타입만 다르게** 정의하는 것은 오버로딩이 아닙니다.

---

## 2) 오버로딩 규칙 & 해석 우선순위
### 규칙
- 메서드 이름 동일
- **매개변수 목록**이 달라야 함 (개수/타입/순서 중 하나 이상 다름)
- 반환 타입은 무관
- 접근지정자, `static`, `final` 여부도 무관 (단, 같은 시그니처로는 중복 정의 불가)

### 호출 해석(Overload resolution) 우선순위 (간단 요약)
1. **정확히 일치하는 시그니처**
2. **기본형의 자동형변환(widening)**: `int → long → float → double`
3. **박싱/언박싱**: `int ↔ Integer`
4. **가변인자(varargs)**: `print(String... s)`

> 같은 우선순위에서 애매하면 **모호성 에러**가 발생합니다.

### 예시
```java
void f(int x) {}
void f(long x) {}
void f(Integer x) {}
void f(int... x) {}

f(10); // f(int) → 정확히 일치
f(10L); // f(long) 선택 (widening)
Integer i = 10;
f(i); // f(Integer) 선택 (boxing 일치)
```

---

## 3) 오버로딩 vs 오버라이딩 비교
| 구분 | 오버로딩(Overloading) | 오버라이딩(Overriding) |
|---|---|---|
| 위치 | 동일 클래스(또는 상속관계 포함 가능) | 상속 관계(자식이 부모 메서드를 재정의) |
| 기준 | **매개변수 목록이 달라야 함** | **메서드 시그니처 동일**(이름/매개변수/반환타입) |
| 접근제어자 | 무관 | **축소 불가**(가시성은 같거나 더 넓게) |
| 반환 타입 | 무관 | **동일**(또는 공변 반환 타입) |
| static 여부 | 무관 | `static` 은 오버라이딩이 아니라 **숨김(hiding)** |
| 바인딩 | 컴파일 타임 결정 | **런타임 동적 바인딩** |
| 목적 | 같은 동작 이름으로 **입력 형태를 다양화** | 상속받은 동작을 **새 의미로 재정의** |

---

## 4) 접근제어자(Access Modifiers) 총정리
| 한글 | 키워드 | 적용 가능 대상(대표) | 접근 범위 |
|---|---|---|---|
| 공용 | `public` | 클래스, 생성자, 필드, 메서드 | **모든 곳**에서 접근 가능 |
| 보호 | `protected` | 필드, 메서드, 생성자 | **같은 패키지 + 다른 패키지의 자식 클래스** |
| (기본) | *(없음, default)* | 클래스(최상위 클래스 불가), 필드, 메서드 | **같은 패키지** 에서만 |
| 비공개 | `private` | 필드, 메서드, 생성자 | **같은 클래스 내부**에서만 |

### 주의 포인트
- **최상위 클래스(top-level class)** 는 `public` 또는 **default** 만 가능 (`protected`, `private` 불가)
- 오버라이딩 시 **가시성 축소 금지**  
  - 부모가 `public` → 자식도 `public` 이어야 함 (또는 불가능)
  - 부모가 `protected` → 자식은 `protected` 또는 `public`
  - 부모가 default → 자식은 default/`protected`/`public` 가능(단, 패키지 이동 시 주의)
- 오버로딩은 접근제어자와 무관하나, **같은 시그니처**면 접근자만 바꿔서 **중복 선언 불가**

---

## 5) 실전 예제: 오버로딩 + 접근제어자
```java
class Printer {
    public void print() {
        System.out.println("[no-arg]");
    }
    // 오버로딩: 매개변수 타입 다름
    public void print(String msg) {
        System.out.println("[String] " + msg);
    }
    // 오버로딩: 매개변수 개수 다름
    void print(String msg, int copies) {
        for (int i = 0; i < copies; i++) System.out.println(msg);
    }
    // 오버로딩: 매개변수 순서 다름
    protected void print(int copies, String msg) {
        for (int i = 0; i < copies; i++) System.out.println("[seq] " + msg);
    }
    // 가변 인자
    private void printAll(String... msgs) {
        for (String m : msgs) System.out.println(m);
    }
}
```

---

## 6) 오버로딩에서 자주 하는 실수
- **반환 타입만 다르게 선언** → 오버로딩 아님 (컴파일 에러)
- `Integer`/`Long` 등 **래퍼 타입 우선순위**를 잘못 예측 → 의도와 다른 메서드가 선택될 수 있음
- `varargs` 가 있으면 **너무 폭넓게 매치**되어 모호성 발생 가능

```java
void g(Integer x, int... y) {}
void g(int x, Integer... y) {}
// g(1, 2); // 모호성 발생 가능
```

---

## 7) 체크리스트
- 같은 이름? → **오버로딩 후보**
- 매개변수 목록(개수/타입/순서) 변경? → **예, 오버로딩**
- 반환 타입만 변경? → **아님**
- 접근제어자 변경만? → **아님**
- 호출 시 어떤 메서드가 선택되는지 애매하면 → **시그니처를 더 구체화**하거나 **명시적 캐스팅** 고려

---

## 8) 요약 한 줄
> **오버로딩 = “같은 이름, 다른 매개변수”** / **오버라이딩 = “같은 시그니처, 실행은 실제 객체 기준(동적 바인딩)”**  
> 접근제어자는 **가시성**을 결정하고, 오버라이딩 시 **축소 불가** 원칙을 기억하자.
