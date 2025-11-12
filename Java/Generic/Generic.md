# Generic
## Generic 필요성
- Object 범용 클래스의 타입 안정성 문제
    - 타입마다 형변환 필요
    - 잘못된 타입 넣어도 컴파일 시점에 문제점 발견 불가
## Generic
```
class Box2<T> {
    private T item;
}

public class WithGeneric {
    public static void main(String[] args) {
        Box2<String> b = new Box2<String>();
        // 타입 명시
    }
}
```
## 타입 매개변수 규칙

|문자 | 의미 | 사용 예 |
|--|--|--|
| T | Type | `class Box<T>` |
| E | Element | `class List<E>` |
| K | Key | `class Map<K,V>` |
| V | Value | `class Map<K,V>` |
| N | Number | `class Calculator<N>` |

## 타입 매개변수 제한
### extends
### 인터페이스 제한
