---
layout: post
title: "RDBMS와 NoSQL의 차이 - 1부 RDBMS"
categories: CS
---

우리는 흔히 데이터를 저장하기 위해서 두가지 유형의 데이터베이스를 사용합니다.

**RDBMS**와 **NoSQL**이죠.

이번 포스팅에서는 그 차이에 대해서 알아보도록 할텐데요.

먼저 오늘은 **RDBMS**에 대해 알아보도록 하겠습니다.

<br>

### **RDBMS란?**
**R**elational **D**ata**B**ase **M**anagement **S**ystem의 약자로 관계형 데이터베이스 관리 시스템을 말합니다.

관계형 데이터베이스는 각 역할 별로 Row와 Column으로 이루어진 2차원 테이블을 만들고 관련된 데이터를 정해진 규격에 따라 테이블에  저장하며, 여러 테이블들을 연결하기 위해 **'관계'**를 사용하는 데이터베이스입니다.

간단한 쇼핑몰 데이터베이스로 예를 들어 보겠습니다.

![rdb](https://sanghun-blog-asset.s3.ap-northeast-2.amazonaws.com/rdb.png)

위의 예시처럼 각 테이블의 기록(record)마다 고유한 아이디(Primary Key)를 가지고 있고 다른 테이블에서 해당 기록의 정보가 필요할 때에는 정보를 중복 입력하는 것이 아니라, 해당 기록의 Primary Key를 외래 키(Foreign Key)로써 참조하여 테이블과 테이블을 연결해주는 것이 특징입니다.

또한 데이터를 정해진 규격에 따라 저장하기 때문에 위 예시에서 각 테이블의 Field 옆에 명시 된 데이터 타입 이외의 정보는 저장할 수 없습니다.

RDBMS에서는 데이터의 생성, 수정, 삭제, 검색을 위해 SQL (**S**tructured **Q**uery **L**anguage)이라는 쿼리 언어를 사용합니다.

<br>

> SQL 예시

```sql
SELECT users.username, orders.id as order_number, orders.date as order_date, 
category.name as category, products.name as product_name, order_products.quantity
FROM users 
JOIN orders ON orders.user_id = users.id
JOIN order_products ON orders.id = order_products.order_id
JOIN products ON order_products.product_id = products.id
JOIN category ON products.category_id = category.id
WHERE users.username = 'sanghun'
ORDER BY orders.date DESC
```
<br>

> 결과 예시

|username|order_number|order_date|category|product_name|quantity|
|:---:|:---:|:---:|:---:|:---:|:---:|
|sanghun|9|2022-01-19|상의|무지 반팔티 - 화이트|3|
|sanghun|9|2022-01-19|하의|기본 와이드 슬랙스|1|
|sanghun|8|2021-01-11|아우터|양털 후리스|1|
|sanghun|8|2021-01-11|모자|캐릭터 비니|2|
|...|...|...|...|...|...|



<br>

#### **RDBMS의 장점**
- 데이터가 규격화 되어있고, 중복이 없기 때문에 **자주 변경**되더라도 **작업의 완전성**을 보장함.
- 데이터의 **분류, 정렬, 탐색 속도가 빠름**
- ACID 특성으로 데이터의 **일관성, 무결성, 안전성**을 보장함.

<br>

#### **RDBMS의 단점**
- **초기에 드는 설계 비용이 크다**: 데이터베이스의 스키마를 짤 때에 테이블의 **구조**와 **관계** 각 field의 **데이터타입까지** 세세하게 설계해줘야 한다.
- 데이터를 다룰 때 반드시 **스키마의 규격에 맞춰야한다**. (스키마에 맞지 않는다면 데이터의 저장이 불가능하다.)
- 구조를 한번 정한 뒤엔 데이터베이스의 **구조를 변경하기 어렵다**.
- 서비스의 규모가 커질 경우 **JOIN문이 복잡**해질 수 있다.
- RDBMS의 특성상 DB의 확장이 필요할 시 수평적 확장이 어렵고, **수직적 확장**만 가능하다. (확장 비용이 비싸다.)

<br>

***수직적 확장과 수평적 확장**
- 수직적 확장 (Scale Up): 서버의 하드웨어(CPU, RAM 등) 성능을 업그레이드하는 방식
- 수평적 확장 (Scale Out): 서버를 더 추가해서 분산처리하는 방식


<br>

### References

> [ghostzoominn (2020, Sep 14). RDBMS와 NoSQL의 차이점 완벽 정리.](https://universitytomorrow.com/26)  
[devuna (2020, Feb 13). [DB]SQL(관계형 데이터베이스)과 NoSQL(비 관계형 데이터베이스) 개념/비교/차이.](https://devuna.tistory.com/25)

