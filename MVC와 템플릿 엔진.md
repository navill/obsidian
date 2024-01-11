MVC: model, view, controller

```
package com.example.hellospring.controller;  
  
import org.springframework.boot.Banner;  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.Model;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.bind.annotation.ResponseBody;  
  
@Controller  
public class HelloController {  
    @GetMapping("hello-api")  
    @ResponseBody  
    public Hello helloApi(@RequestParam("name") String name) {  
        Hello hello = new Hello();  
        hello.setName(name);  
        return hello;  
    }  
  
    static class Hello {  
        private String name;  
  
        public String getName() {  
            return name;  
        }  
  
        public void setName(String name) {  
            this.name = name;  
        }  
    }  
}
```
HttpMessageConverter
- JsonConverter(MappingJackson2HttpMessageConverter): 객체를 받았을 때 변환
- StringConverter(StringHttpMessageConverter): 기본 문자 변환
- byte 처리 등 기타 여러 HttpMessageConverter가 기본 등록되어 있음