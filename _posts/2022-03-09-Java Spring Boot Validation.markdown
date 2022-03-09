---
layout: post
title: "Java Spring boot - @Valid를 이용한 유효성 검사"
categories: Framework
---

오늘은 Java Spring Boot에서의 @Valid를 이용한 Validation에 대해 간단하게 알아보도록 하겠습니다.

API서버를 구축할때에 들어오는 Request의 내용에대해 유효성 검사를 해야겠죠?

1. 그럴때엔 유효성 검증을 하고자하는 **매개변수 클래스**의 **필드**에 **constraints annotation**을 명시해준 뒤, 
2. 매개변수로 불러지는 곳에서 **@Valid annotation**을 사용해 유효성 검사를 진행할 수 있습니다.

<br>

### **Java.validation의 constraints annotation들**

```java
@NotNull            // Null 허용 안함. "", " " 허용
@Null	            // Null만 허용
@NotEmpty           // Null, "" 허용 안함. " " 허용
@NotBlank           // Null, "", " " 모두 허용 안함
@Size(min=,max=)    // 문자열, 배열 등의 크기가 min 이상 max이하 일때만 허용
@Pattern(regex=)    // 정규식에 매치될 경우만 허용
@Max(num)           // (정수) num 이하의 값만 하용
@Min(num)           // (정수) num 이상의 값만 허용
@Future	            // (시간) 현재보다 미래일 경우만 하용
@FutureOrPresent    // (시간) 현재 또는 미래일 경우만 허용
@Past	            // (시간) 현재보다 과거일 경우만 허용
@PastOrPresent      // (시간) 현재 또는 과거일 경우만 허용
@Positive         // 양수만 허용
@PositiveOrZero	    // 0 또는 양수만 허용
@Negative         // 음수만 허용
@NegativeOrZero	    // 0 또는 음수만 허용
@Email	            // 이메일 형식만 허용
@Digits(integer=, fraction = )	// (실수) 최대 integer 정수 자릿수, fraction 소수 자릿수 만큼 허용
@DecimalMax(value=) // (실수) value 이하의 실수만 허용
@DecimalMin(value=) // (실수) value 이상의 실수만 허용
@AssertFalse	    // false만 허용
@AssertTrue         // true만 허용
```
주로 쓰이는 annotation위주로 정리해 보았으며 더 자세한 내용은 [Java 8 validation doc](https://javaee.github.io/javaee-spec/javadocs/javax/validation/constraints/package-summary.html)에서 확인해 보실 수 있습니다.
각 어노테이션은 @Annotation(message="내용") 처럼 message속성을 통해서 에러 메세지를 지정해 줄 수 있습니다.


### **사용예시**

#### **매개변수 클래스의 필드에 constraints annotation과 message 지정**
```java

public class SignUpRequest {

    @NotNull(message = "username은 필수 입력 값입니다.")
    @Size(min = 4, max = 15)
    private String username;

    @Email(message = "이메일 형식이 맞지 않습니다")
    private String email;

    @Pattern(message = "비밀번호는 최소 8자 이상이어야 하며, 하나 이상의 소문자, 대문자, 숫자, 특수문자를 포함해야 합니다",regexp = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[*.!@$%^&(){}[]:;<>,.?/~_+-=|\]).{8,32}$")
    private String password;

    @Positive
    private String age;

    @NotBlank(message = "name은 필수 입력 값입니다.")
    private String name;
    
}

```

<br>

#### **매개변수 앞에서 @Valid annotation 사용**

```java
@ApiOperation("회원가입 API")
@PostMapping("/user")
public SignUpResponse signUp(@RequestBody @Valid SignUpRequest request){
    return signUp.execute(requeset);
}

```

이렇게 설정해 주었을 때에 잘못된 Request가 들어오게 되면,
@Valid 어노테이션을 통해 검증을 한 뒤 설정해 놓은 적절한 에러메세지와 함께 400 bad request 가 리턴됩니다 :)