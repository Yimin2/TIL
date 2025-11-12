# 컴포지션

## 개념

- 클래스가 다른 클래스의 객체를 포함해서 기능을 재사용하는 것
- has-a (가지고 있다) 의 관계 / is-a (상속)

```
// is-a 관계 (상속)
class Car extends Vehicle { // Car는 Vehicle이다
}

// has-a 관계 (컴포지션)
class Car {
    private Engine engine;  // Car는 Engine을 가지고 있다
    private Wheel[] wheels; // Car는 Wheel들을 가지고 있다
}
```

## 장점

1. 유연한 설계

- 결합도가 낮아짐
    - 한쪽을 수정해도 다른 쪽에 영향이 거의 없음
- 상속은 한 번밖에 못하지만 컴포지션은 여러 객체를 조합해 다양하게 구성 할 수 있음
  ```class Car {
  private Engine engine;
  private Navigation navigation;
  private AirConditioner airConditioner;
  }
  ```
- 코드 재사용성이 높음
   ```
  class Ship {private Engine engine;}
  class Airplane {private Engine engine;}
  ```  

- 런타임에 교체 가능 (유연한 의존성 주입)
    - 객체를 실행 중에 다른 것으로 바꿀 수 있음
- 테스트하기 쉬움
    - 객체를 mock으로 바꿔서 테스트 가능
