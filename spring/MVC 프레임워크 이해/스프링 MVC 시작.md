@RequestMapping
- RequestMappingHandlerMapping
- RequestMappingHandlerAdapter

```java
package hello.servlet.web.springmvc.v1;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.servlet.ModelAndView;  
  
@Controller  
public class SpringMemberFormControllerV1 {  
  
    @RequestMapping("/springmvc/v1/members/new-form")  
    public ModelAndView process() {  
        return new ModelAndView("new-form");  
    }  
}
```

@Controller : 스프링이 자동으로 스프링 빈으로 등록한다. (내부에 @Component 애노테이션이 있어서 컴포넌트 스캔의 대상이 됨). 
- 스프링 MVC에서 애노테이션 기반 컨트롤러로 인식 -> 스프링 빈 중에서 @RequestMapping 또는 @Controller 가 **클래스 레벨**에 붙어 있는 경우에 매핑 정보로 인식

컴포넌트 스캔 대상
`@Component` : 컴포넌트 스캔에서 사용  
`@Controller` : 스프링 MVC 컨트롤러에서 사용  
`@Service` : 스프링 비즈니스 로직에서 사용
`@Repository` : 스프링 데이터 접근 계층에서 사용 
`@Configuration` : 스프링 설정 정보에서 사용

#컴포넌트스캔 #RequestMapping #Controller #Component #Service #Repository #Configuration 

