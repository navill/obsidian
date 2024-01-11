- JPQL에서 엔티티를 직접 사용하면 SQL에서 해당 엔티티의 기본 키 값을 사용
```java
//JPQL
select count(m) from Member m //엔티티를 직접 사용

//SQL
select count(m.id) as cnt from Member m
```
- 조인 객체(Team)을 직접 사용할 경우 외래 키를 사용

---
# Named Query
![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020231221203642.png)
- 동적 쿼리 불가
- 어노테이션, XML 정의가능
- 어플리케이션 로딩 시점에 초기화 후 재사용
	- 로딩 시점에 쿼리를 검증함

---
# 벌크 연산
query.executeUpdate() 
- 벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 직접 쿼리(문제가 될 수 있음)
- 해결책
	- 벌크 연산을 먼저 실행
	- 벌크 연산 후 영속성 컨택스트 초기화