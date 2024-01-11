

```java
package hello.core.order;  
  
import hello.core.discount.DiscountPolicy;  
import hello.core.discount.FixDiscountPolicy;  
import hello.core.discount.RateDiscountPolicy;  
import hello.core.member.Member;  
import hello.core.member.MemberRepository;  
import hello.core.member.MemoryMemberRepository;  
  
public class OrderServiceImpl implements OrderService {  
  
    private final MemberRepository memberRepository = new MemoryMemberRepository();  
    //    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();  
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();  // dip 위반  
  
    @Override  
    public Order createOrder(Long memberId, String itemName, int itemPrice) {  
        Member member = memberRepository.findById(memberId);  // 멤버 조회  
        int discountPrice = discountPolicy.discount(member, itemPrice);  // 멤버에 따른 가격 할인 적용  
  
        return new Order(memberId, itemName, itemPrice, discountPrice);  // 주문 객체 생성  
    }  
}
```
- OrderServiceImpl에서 RateDiscountPolicy + DiscountPolicy 둘다 의존하고 있음 -> DIP 위반




#DIP #인터페이스

