# Builder 패턴 (Builder Pattern)

## 1. 정의

- 객체 생성 과정을 **단계별로 설정**하고, 최종적으로 **완성된 객체를 반환**하는 디자인 패턴
- 새로운 객체를 생성할 때 사용하고, 수정 시 setter 사용
- 주로 생성자가 복잡하거나 매개변수가 많은 객체를 만들 때 사용


## 2. 특징

- **가독성 향상**: 매개변수가 많은 생성자보다 직관적
- **유연성**: 일부 필드만 선택적으로 설정 가능
- **불변 객체 생성**: `final` 필드를 가진 객체 생성 시 유용
- **메서드 체인 지원**: 연속 호출 가능

## 3. 구조 예시

```java
// Product
public class User {
    private final String name;
    private final int age;
    private final String email;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
    }

    // Builder
    public static class Builder {
        private String name;
        private int age;
        private String email;

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}

// 사용 예
User user = new User.Builder()
        .name("namsu")
        .age(30)
        .email("namsu@example.com")
        .build();
```

## 사용 시점

### 엔티티
- 엔티티 안의 새 객체 생성용 생성자에만 Builder 사용, 조회/수정에는 Builder 필요 없음.
### DTO
- 대부분 Builder 사용, 필요한 필드만 설정 가능.