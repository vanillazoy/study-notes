# 10-8. Object의 핵심 메서드 요약 (toString, equals, hashCode)

> 컬렉션/로그/디버깅/키 비교의 기초가 되는 3대 메서드

---

## 1) toString()
- `System.out.println(obj)` → 내부적으로 `obj.toString()` 호출
- **오버라이딩 권장**: 디버깅/로그 가독성 향상
- 기본 동작(Object): `패키지.클래스명@16진수해시`

### 간단 규칙
- 사람이 읽기 쉬운 **주요 상태만** 노출 (민감정보 제외)
- 불변/핵심 필드 우선
- 예: `User{id=42, name='Kim'}`

```java
@Override
public String toString() {
    return "User{id=" + id + ", name='" + name + "'}";
}
```

---

## 2) equals(Object o)
- **동등성(equality)** 판정 로직을 정의
- 기본 동작(Object): **참조 동일성**(`this == o`)과 동일

### equals 계약(Contract)
1. **반사성**: `x.equals(x)` 는 true  
2. **대칭성**: `x.equals(y)` ↔ `y.equals(x)` 동일  
3. **추이성**: `x~y`, `y~z` ⇒ `x~z`  
4. **일관성**: 비교 대상 변경 없으면 결과가 변하지 않음  
5. **null 비교**: `x.equals(null)` 은 항상 false

### 구현 요령
- 클래스가 **논리적 동등성**을 가지면 오버라이딩
- 비교 순서: **== → instanceof → 필드 비교 → 반환**
- 비교 필드는 equals에 참여한 **모든 핵심 상태**

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof User)) return false;
    User that = (User) o;
    return id == that.id && Objects.equals(name, that.name);
}
```

---

## 3) hashCode()
- **해시 기반 컬렉션(HashMap/HashSet)** 성능에 사용
- 기본 동작(Object): **아이덴티티 기반** 해시(참조 기준, 구현 자유)

### equals·hashCode 불변식
- `x.equals(y) == true` ⇒ **`x.hashCode() == y.hashCode()` 반드시 동일**
- 역은 성립 X (해시 충돌 가능)
- **equals를 오버라이딩하면 반드시 hashCode도 함께 오버라이딩**

```java
@Override
public int hashCode() {
    return Objects.hash(id, name);
}
```

### 품질 팁
- equals에 사용한 **모든 필드**를 해시에 포함
- 변하는 필드로 **키를 만들지 말 것** (키는 가급적 불변)

---

## 4) 자주 하는 실수 & 체크리스트
- equals만/혹은 hashCode만 오버라이딩 ❌
- `getClass()` vs `instanceof` 선택: 상속 고려 시 `instanceof` 권장
- 배열 비교는 `Arrays.equals`, 중첩 객체 비교는 `Objects.equals`
- **민감정보**는 toString에 출력 금지
- 동등성 기준을 문서화(주석/JavaDoc)

---

## 5) 미니 예시 (레코드/롬복 참고)
- Java **record**는 equals/hashCode/toString 자동 생성
- Lombok `@EqualsAndHashCode`, `@ToString` 적극 활용 가능

```java
public record Point(int x, int y) {}
// toString: Point[x=..., y=...], equals/hashCode 자동
```

---

### 한 줄 요약
> **toString은 읽기 좋게, equals는 논리 동등성, hashCode는 컬렉션 성능.  
> equals를 바꾸면 hashCode도 반드시 함께 바꿔라.**
