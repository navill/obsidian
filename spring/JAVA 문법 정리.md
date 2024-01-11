
### 패키지
패키지 파일 이름: 도메인을 역순으로 만듬
```
filname: com.example.util


# Caculator.java
package com.example.util;

public class Calculator {
   public int plus(int x, int y){
	   return x + y
   }
   public int minus(int x, int y){
	   return x - y
   }
}

# 패키지 파일 생성
javac -d . Caculator.java
# 패키지 실행
java com.exmaple.util.Calculator
```