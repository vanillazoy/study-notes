# 10-6. 인스턴스 필드 vs 정적 필드 (Instance Field vs Static Field)

| 구분 | 인스턴스 필드 | 정적 필드 (static 필드) |
|---|---|---|
| 저장 위치 | **객체(인스턴스)** 내부에 저장됨 | **클래스(설계도)** 자체에 저장됨 |
| 생성 시점 | `new` 로 객체 생성 시 메모리에 생성됨 | 클래스가 **메모리에 로딩되는 순간** 이미 생성됨 |
| 소유 주체 | **각 객체가 각각 따로 가짐** | **클래스가 단 하나만 가짐 (공유)** |
| 값 변경 영향 | 변경 시 **해당 객체에만 영향** | 변경 시 **모든 객체가 같은 값 공유** |
| 접근 방식 | `instance.field` | `ClassName.field` (또는 instance 가능하나 권장 X) |
| 예시 비유 | 사람마다 이름은 다름 | 사람 모두가 동일한 국적을 공유 |

---

## ✅ 핵심 요약

- **인스턴스 필드**는 객체마다 각각 다르게 존재한다.  
  → 객체가 여러 개이면 값도 여러 개.

- **정적(static) 필드**는 클래스가 하나만 소유한다.  
  → 모든 객체가 같은 값을 공유한다.

---

## 예제 코드

```java
class Person {
    String name;                    // 인스턴스 필드
    static String nation = "Korea"; // 정적 필드 (공유)
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.name = "민수";

        Person p2 = new Person();
        p2.name = "지수";

        System.out.println(p1.name);   // 민수
        System.out.println(p2.name);   // 지수

        System.out.println(Person.nation); // Korea (클래스명으로 접근)
    }
}
```

---

## 한 줄 정리
> **인스턴스 필드 = 객체마다 따로 존재**  
> **정적(static) 필드 = 클래스가 하나만 공유**
