# 📝 Lazy Loading (지연 로딩) 정리

## 1. 개념

JPA에서 연관 엔티티를 **실제 사용 시점까지 조회를 미루는 전략**

- 연관 엔티티가 필요할 때만 DB에서 조회
- 초기 조회 시 불필요한 데이터를 가져오지 않아 성능 최적화
- 반대 개념: **Eager Loading** (즉시 로딩, 연관 엔티티를 항상 같이 조회)
- N+1 문제 발생 가능

---

## 2. 적용
[지연로딩 예시_깃허브](https://github.com/Yimin2/intagram_clone/commit/0ab35badde6a5bec7aa0788c0c8ea52f6c64b136)
- ManyToOne / OneToMany / OneToOne 등 연관 관계 필드에 지정
- fetch = FetchType.LAZY 


