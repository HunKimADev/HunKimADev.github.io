---
layout: post
title: "RDBMS와 NoSQL의 차이 - 2부 NoSQL"
categories: CS
---

저번 포스팅에 이어 이번에는 NoSQL에 대해 알아보도록 하겠습니다.

<br>

### **NoSQL이란?**
**N**ot **O**nly **SQL** (SQL만을 사용하지 않는) 혹은 **No**n **SQL** (비 SQL)의 약자로 알려져 있습니다.  
NoSQL은 이름 그대로 관계형 데이터베이스와는 다른 방식으로 데이터를 저장하는 데이터베이스를 말합니다.
대용량 데이터 처리가 중요해짐에 따라 RDBMS의 한계점인 스키마 문제와 수직적 확장(스케일업)의 한계를 극복하기 위해 등장한 데이터베이스입니다.

NoSQL의 종류는 데이터를 어떤 방식으로 저장하느냐에 따라 나누어지는데 아래에서 알아보도록 하겠습니다.

<br>

### **NoSQL의 종류**

#### **1. Key-Value Database : Redis**
![key-value database](https://upload.wikimedia.org/wikipedia/commons/5/5b/KeyValue.PNG)  
[이미지 출처(wikipedia)](https://en.wikipedia.org/wiki/Key%E2%80%93value_database)

가장 간단한 형태의 NoSQL 데이터베이스로 모든 데이터가 Key-Value Pair로 저장됩니다.
구조가 단순한만큼 데이터의 처리 속도가 빠릅니다.

<br>

#### **2. Document Database : MongoDB, CouchDB**
![document database](https://ravendb.net/wp-content/uploads/2021/04/beers-table-documents-1.png)  
[이미지 출처(ravendb.net)](https://ravendb.net/articles/nosql-document-oriented-databases-detailed-overview)

왼쪽은 데이터를 테이블 형태로 나타낸 것, 오른쪽은 문서형태로 나타낸 것입니다. Document Database는 보이는 것 과 같이 데이터를 JSON이나 XML 형태로 저장하는데 테이블 형태와 달리 각 기록(컬렉션)마다 다른 스키마를 가질 수 있습니다.

Document Database는 데이터를 Object형식으로 저장하기 때문에 SQL에 비해 어플리케이션과 데이터에비이스 사이의 translation비용이 적게 듭니다.

<br>

#### **3. Column-Oriented Database (Wide Column Database): Cassandra, Hbase**

![Columnar database](https://qph.fs.quoracdn.net/main-qimg-c29673c7da88bf898770ef130155e661)  
[이미지 출처(quora.com)](https://www.quora.com/What-is-a-columnar-database)

위 이미지에서와 같이 행마다 키와 값을 저장할때 각각 다른 값, 다른 수의 스키마를 가질수 있는 데이터베이스 형태입니다.
대량의 데이터에 대한 압축, 분석처리가 빠른 특징이 있습니다.

<br>

#### **4. Graph Database: Neo4J, AllegroGraph**

![Graph Database](https://venturebeat.com/wp-content/uploads/2021/02/graph-database-neo4j.png?w=1200&strip=all)
[이미지 출처(venturebeat.com)](https://venturebeat.com/2021/02/08/what-is-a-graph-database/)

Graph Database는 data elements간의 관계에 집중한 데이터베이스 형태입니다. 각 element는 node형태로 저장되며 노드 사이의 관계를 edge로 표현합니다.  
Graph Database에서의 '관계'는 elements 사이의 직접적 연결이기 때문에 복잡한 JOIN문이 필요한 SQL에 비해 처리 속도가 빠르고 유연하며 유지보수가 용이한 특징이 있으며, Social Networks에서 주로 사용됩니다.

<br>

#### **NoSQL의 장점**
- **`설계가 쉽다`**: 정해진 스키마가 없기 때문에 자유로운 데이터 구조를 가질 수 있다.
- **`구조의 변경이 쉽다`**: 데이터 구조의 변경이 필요할 떄 언제든 새로운 필드를 추가, 변경할 수 있다.
- **`읽기와 쓰기가 빠르다`**: 데이터가 어플리케이션에 맞는 형태로 저장되기 때문에 읽기와 쓰기 속도가 빠르다.
- **`데이터 분산`**이 쉬우며 **`수평적 확장`**이 가능하다.
- **`대용량의 데이터 처리`**에 유리하다.

<br>

#### **NoSQL의 단점**
- **`데이터 수정이 오래 걸린다`**: 중복된 데이터 변경 시, 모든 컬렉션에서 해당 데이터를 수정해주어야한다.
- **`일관성이 보장되지 않는다`**: ACID 트랜잭션을 지원하지 않기때문에 데이터의 일관성이 보장되지 않는다.


<br>

### References

> [zzory_bbong (2019, Apr 30). DB_RDB(관계형데이터베이스)와 NoSQL](https://medium.com/@duddk1551/db-rdb-%EA%B4%80%EA%B3%84%ED%98%95%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%99%80-nosql-adbd21f6f9f1)  
[MongoDB. What is NoSQL?](https://www.mongodb.com/en-us/nosql-explained)  
[jmkang (2018, Jul 09). NoSQL DB의 종류](http://www.incodom.kr/NoSQL_DB_%EC%9D%98_%EC%A2%85%EB%A5%98#h_5eb1cd481d082296550e9a66dfae32d1)

