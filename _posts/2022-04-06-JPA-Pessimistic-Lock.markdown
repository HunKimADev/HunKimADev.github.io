---
layout: post
title: "[JPA] Pessimistic Lock for Database Concurrency Control"
categories: Framework
---

우리가 금융 혹은 거래 관련 서비스를 개발하다보면 한번에 여러개의 거래 요청이 들어오는 경우가 있다. 
예를 들어 한 계좌에 대한 입금요청이 동시에 여러개가 들어오는 경우엔  

1. 데이터베이스에서 해당 계좌의 정보를 읽어온다.
2. 읽어온 계좌의 현재금액에 입금하고자 하는 금액을 더해 업데이트한다.
3. 데이터베이스에 저장한다.

이렇게 세 단계를 거치게 되는데 한번에 여러 요청이 들어오는 경우 데이터의 정확성이 떨어질 수 있다.

예를들어, 아래와 같은 경우
![](https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/c0ab2664-f636-4e10-6b90-05943886f400/public)


첫번째 입금요청에서 100만원의 입금을 요청 하고 두번째 입금요청에서는 5000원의 입금을 요청했다. A.와 B.에서 두 요청 모두 읽어가는 시점의 현재 잔고는 만원이었기때문에, E에서 첫번째 요청에 대한 금액이 업데이트 되어 잔고가 101만원으로 업데이트 되었지만, 이후에 두번째 요청이 현재잔고를 15000원으로 업데이트 하게되면 첫번째 요청에대한 100만원은 사라져버리게 된다. 실제로 돈이 관련된 서비스에서 이런 일들이 일어난다면 어떨까? 안되겠지? 조지겠지?

그래서 조지는 상황을 피하기 위해 DB Lock이라는게 필요하다. 

<br>

## Lock의 종류
Lock에는 Optimistic Lock(낙관적 락)과 Pessimistic Lock(비관적 락) 두가지가 있는데 

낙관적 락은 
- **충돌이 일어나지 않을 것을 가정하고 충돌이 일어났을 때 알려주기 위한 방식** 이고

비관적 락은
- **충돌이 일어날 것을 미리 가정하고 락을 걸어놓는 방식**이다.

낙관적 락은 실제로 DB의 락 기능을 사용하는 것이 아니라 JPA의 버전 컨트롤 기능을 사용해 커밋할 때 읽어왔을 때와 버전정보가 다르다면 exception을 날려주는 방식으로 동작한다.

그에 비해 비관적락은 실제로 DB의 락 기능을 사용하며 (e.g. select for update) 항상 락을 걸어놓기때문에 성능이 비교적 떨어질 가능성이 있다는 문제가 있다.

하지만 위의 예시처럼 돈이 관련됐다면 성능이 중요하겠니, 안정성이 중요하겠니?

당연히 안정성이다 그러니 우리는 오늘 비관적 락에 대해 알아보도록 한다.

<br>

## 비관적 락 (Pessimistic Lock)

비관적 락이 작동하는 방식은 꽤나 간단하다.

우리가 update를 위한 select쿼리를 사용할 때에 뒤에 for update를 붙여주게 되면, 즉 JPA등에서 Pessimistic Lock을 사용하게 되면 (정확히는 JPA에서의 PESSIMISTIC_WRITE 옵션), select된 해당 로우에 업데이트가 일어날 때까지 해당 row는 Lock상태가 되어 다른 요청이 읽거나 쓰거나 업데이트를 할 수 없게 된다. Lock인 상태에서 해당 row로 다른 요청이 들어올 경우엔 해당 요청은 대기상태에 빠지게 되며, Lock이 걸린 로우에 업데이트가 완료 되면 Lock이 풀리고 대기중인 다음 요청이 처리된다.

![](https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/8625ced5-dbff-455c-a5ae-a7495502c400/public)

위의 예시에서 볼 수 있듯이, B에서 두번째 입금요청이 들어왔으나, 첫번째 입금요청으로 Lock이 걸려있기 때문에 대기상태에 빠지게 되고, 업데이트가 완료되어 Lock이 풀린후에 정상적으로 처리되어 우리의 소중한 100만원을 잃지 않고 1015000원이 정상적으로 입금된 걸 확인할 수 있다.

<br>

## JPA에서 비관적 락(Pessimistic Lock)의 적용

Repository클래스에서 Lock이 필요한 업데이트를 할 때 사용 할 select쿼리 위에 **@Lock** 어노테이션을 달아주면 된다.
```java

public interface AccountRepository extends JpaRepository<Account, Long> {

    @Lock(LockModeType.PESSIMISTIC_WRITE)
	Optional<Account> findById();
}
```

이게 다다 매우 간단하다.

<br>

## 주의할 점

주의할 점은 해당 쿼리를 사용하면 항상 Lock이 들어간다는 사실을 까먹지 말아야 한다는 것이다.

또, @Lock 어노테이션은 JpaRepository 내에서 auto-generated된 쿼리에만 적용이 된다.  

queryDsl로 Custom한 쿼리의 경우엔 Custom Interface에 붙여도, Impl에 붙여도 제대로 적용이 안되어 몇일동안 삽질을 했더랬다. 아마 JpaRepository가 auto-generate한 쿼리에서는 해당 어노테이션을 핸들링하는 내장 메소드가 있는 것 같은데 그건 다음 기회에 확인해 보도록 하겠다.