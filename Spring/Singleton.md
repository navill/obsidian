
```java
package hello.core.singleton;  
  
import hello.core.AppConfig;  
import hello.core.member.MemberService;  
import org.junit.jupiter.api.DisplayName;  
import org.junit.jupiter.api.Test;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.AnnotationConfigApplicationContextExtensionsKt;  
  
import static org.assertj.core.api.Assertions.assertThat;  
  
public class SingletonTest {  
  
    @Test  
    @DisplayName("스프링 컨테이너와 싱글톤")  
    void springContainer() {  
//        AppConfig appConfig = new AppConfig();  
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);  
  
        MemberService memberService1 = ac.getBean("memberService", MemberService.class);  
        MemberService memberService2 = ac.getBean("memberService", MemberService.class);  
  
        // 참조값이 다른것을 확인  
        System.out.println("memberService1 = " + memberService1);  
        System.out.println("memberService2 = " + memberService2);  
  
        // memberService1 == memberService2  
        assertThat(memberService1).isSameAs(memberService2);  
    }  
}
```

# 싱글톤 방식의 주의점(중요)

- 싱글톤은 객체 상태를 유지하게 설계하면 안됨
	- 변경 시 참조하는 다른 로직에서 문제가 발생할 수 있음
	- only read
```java
package hello.core.singleton;  
  
public class StatefulService {  
    private int price; // 상태를 유지하는 필드  
  
    public void order(String name, int price) {  
        System.out.println("name = " + name + " price = " + price);  
        this.price = price;  
    }  
  
    public int getPrice() {  
        return price;  
    }  
}


package hello.core.singleton;  
  
import org.junit.jupiter.api.Test;  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.Bean;  
  
import static org.assertj.core.api.Assertions.assertThat;  
  
class StatefulServiceTest {  
  
    @Test  
    void statefulServiceSingletone() {  
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);  
  
        StatefulService statefulService1 = ac.getBean(StatefulService.class);  
        StatefulService statefulService2 = ac.getBean(StatefulService.class);  
  
        //ThreadA: 사용자A가 10000원을 주문  
        statefulService1.order("userA", 10000);  
        //ThreadB: 사용자B가 20000원을 주문  
        statefulService2.order("user2", 20000);  
  
        //ThreadA: 사용자A가 주문 금액 조회  
        int price = statefulService1.getPrice();  
        System.out.println("price = " + price);  
  
        assertThat(statefulService1.getPrice()).isEqualTo(20000);  //망함
    }  
  
    static class TestConfig {  
  
        @Bean  
        public StatefulService statefulService() {  
            return new StatefulService();  
        }  
    }  
}
```


### @Configuration
```java
@Configuration  
public class AppConfig {  
	// @Bean memberService -> new MemoryMemberRepository()
	// @Bean odrserService -> new MemoryMemberRepository() 싱글톤깨지나??
    @Bean  
    public MemberRepository memberRepository() {  
        return new MemoryMemberRepository();  
    }  
  
    @Bean  
    public MemberService memberService() {  
        return new MemberServiceImpl(memberRepository());  
    }  
  
    @Bean  
    public OrderService orderService() {  
        return new OrderServiceImpl(memberRepository(), discountPolicy());  
    }  
    
    ...
}
```

```java
@Bean
public MemberRepository memberRepository() {

	if (memoryMemberRepository가 이미 스프링 컨테이너에 등록되어 있으면?) { 
		return 스프링 컨테이너에서 찾아서 반환;

	} else { //스프링 컨테이너에 없으면  
		기존 로직을 호출해서 MemoryMemberRepository를 생성하고 스프링 컨테이너에 등록 
		return 반환

	}
}
```
- @Configuration에 의해 Appconfig@CGLIB 객체가 생성되고 이것을 스프링 컨테이너에서 사용
	- 내부 로직이 싱글톤을 보장함
- @Configuration을 제거하고 @Bean만 사용할 경우?
	- 스프링 빈에 등록은 되지만 싱글턴 보장 X










#싱글톤 #싱글톤주의사항 #test_price #configuration



