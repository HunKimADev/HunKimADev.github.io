---
layout: post
title: "DDD를 적용한 Java Spring Boot API 서버의 구조"
categories: Framework
---


이번에는 DDD(Domain Driven Design)를 적용한 Java Sprint Boot API Server의 구조에 대해 알아보려고 한다.


### 1. Java Spring Boot의 기본구조

![스프링구조](https://3827551924-files.gitbook.io/~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-M26jG1r8fNWiAQrZyfg%2F-M4hJIiL_CbaMtV-iVnw%2F-M4hJL1xuvndUsln6RlS%2Fimage-20200412132447872.png?generation=1586676989907156&alt=media)

1-1. 스프링의 흐름 - [출처](https://dahye-jeong.gitbook.io/spring/spring/2020-04-12-layer)  


자바 스프링의 기본구조는 Client -> Controller -> Service -> Repository -> DB 이다.

![프로젝트구조](https://sanghun-blog-asset.s3.ap-northeast-2.amazonaws.com/spring_basic.png)
1-2. 프로젝트 구조


Client -> Controller -> Service 구간은 1)**Dto**객체로 넘겨주며 Service -> Repository( 2)**DAO** ) -> DB간에는 Entity(Model)객체로 데이터가 오고간다.

#### 1) DTO란?
Data Transfer Object의 약자이며, API에 대한 요청과 응답을 주고받을 때에 필요한 정보만 담을 수 있도록 만들어진 객체이다.

<br>

#### 2) DAO란?
Data Access Object의 약자이며, Database의 CRUD를 담당하는 로직이 담긴 객체이다.

<br>



#### **Client**: 
API 요청을 보내는 Client이다. 요청은 **Request DTO**에 담겨 **Controller**로 전달된다.

#### **Controller**: 
각 요청에 맞게 **Service**의 알맞은 함수를 호출해 **Request DTO**를 전달하고, 반환된 결과값을 **Response DTO**에 담아 **Client**로 반환하는 Layer이다.

#### **Service**: 
모든 API요청의 처리에 대한 비즈니스 로직이 작성되는 부분이다. **Request DTO**에 담긴 정보를 가공/사용하여 **Repository**를 통해 원하는 결과 값을 얻어낸 뒤,
가공을 거쳐 **Response DTO**에 담아 Controller에 반환한다.

#### **Repository**:
데이터베이스와 관련한 CRUD로직을 구현하는 부분이다. **Entity Class(Model)**들을 이용해서 Database와 통신한다.

<br>
<br>

### **2. DDD를 적용한 Java Spring boot**

![DDD](https://miro.medium.com/max/1400/1*F82KYklUqew6ycZEq9niAw.png)
2-1. DDD - [출처](https://medium.com/riiid-teamblog-kr/gradle%EA%B3%BC-%ED%95%A8%EA%BB%98%ED%95%98%EB%8A%94-backend-layered-architecture-97117b344ba8)

#### **Domain Driven Design 이란?**

DDD란 코드를 설계함에 있어서 각 코드의 역할(e.g. Controller, Model, Veiw)별로 구분하는 것이 아니라 현실의 비즈니스 도메인(e.g. 유저, 카탈로그, 쇼핑카트, 결제)별로 구분하여 설계하는 디자인 패턴이다.

<br>

서비스의 규모가 작을때에는 1-2의 이미지의 기본구조로도 충분히 관리가 편하지만, 서비스의 규모가 점점 커지면서 다양한 서비스의 로직이 한군데에 몰리게 되면 의존성 관리도 어려울 뿐더러 새로운 기능의 추가나 유지 보수가 어려워 질 수 있다. 이럴 때에 DDD를 적용하면 좋다.

![ddd](https://sanghun-blog-asset.s3.ap-northeast-2.amazonaws.com/spring_ddd.png)
2-2. DDD를 적용한 프로젝트 구조

#### **Application**:
**Controller**와 **Dto**등 클라이언트와 직접적으로 통신하는 인터페이스를 포함하는 패키지.

#### **domain**:
비즈니스 도메인별로 나누어진, 컨트롤러로부터 호출되어 내부로직을 수행하는 **Service**, **Repository**, **Model**등을 포함하는 패키지

<br>

여기서 서브도메인의 규모가 더욱 커진다면 **Service**의 각 기능들을 **Usecase**로 분리하여 아래와 같이 더욱 세분화 할 수 있습니다.

![ddd_detail](https://sanghun-blog-asset.s3.ap-northeast-2.amazonaws.com/spinrg_ddd_detail.png)

2-3. 더 세분화한 DDD구조

#### **Command**: 
**Repository**에서 DB에 쿼리할때 사용하기위한 조건들을 담아놓은 Class. **Request Dto**를 통해 들어오는 데이터를 Validation하는데 사용된다.

#### **Result**:
마찬가지로 **Repository**에서 반환하는 결과를 담기위한 Class이다

#### ***Usecase**:
**Service**를 도메인의 각 기능별로 나눠놓은 Class들이다.




