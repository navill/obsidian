
```java
package com.example.hellospring.repository;  
  
import com.example.hellospring.domain.Member;  
  
import java.util.*;  
  
public class MemoryMemberRepository implements MemberRepository {  
    private static Map<Long, Member> store = new HashMap<>();  
    private static long sequence = 0L;  
  
    @Override  
    public Member save(Member member) {  
        member.setId(++sequence);  
        store.put(member.getId(), member);  
        return member;  
    }  
  
    @Override  
    public Optional<Member> findById(Long id) {  
        return Optional.ofNullable(store.get(id));  
    }  
  
    @Override  
    public Optional<Member> findByName(String name) {  
        return store.values().stream()  
                .filter(member -> member.getName().equals(name))  
                .findAny();  
    }  
  
    @Override  
    public List<Member> findAll() {  
        return new ArrayList<>(store.values());  
    }  
  
    public void clearStore(){  
        store.clear();  
    }  
}
```

```java
package com.example.hellospring.repository;  
  
import com.example.hellospring.domain.Member;  
import org.junit.jupiter.api.AfterEach;  
import org.junit.jupiter.api.Test;  
  
import java.util.List;  
  
import static org.assertj.core.api.Assertions.assertThat;  
  
public class MemoryMemberRepositoryTest {  
    MemoryMemberRepository repository = new MemoryMemberRepository();  
  
    @AfterEach  
    public void afterEach() {  
        repository.clearStore();  
    }  
  
    @Test  
    public void save() {  
        Member member = new Member();  
        member.setName("spring");  
        repository.save(member);  
        Member result = repository.findById(member.getId()).get();  
//        Assertions.assertEquals(member, result);  
        assertThat(member).isEqualTo(result);  
    }  
  
    @Test  
    public void findByname() {  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
  
        Member result = repository.findByName("spring1").get();  
        assertThat(result).isEqualTo(member1);  
    }  
  
    @Test  
    public void findAll() {  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
  
        List<Member> result = repository.findAll();  
        assertThat(result.size()).isEqualTo(2);  
    }  
}

```


#테스트코드
