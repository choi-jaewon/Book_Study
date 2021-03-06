## ⚙ 엔터티 매니저 팩토리와 엔터티 매니저

데이터베이스를 하나만 사용하는 어플리케이션은 일반적으로 **EntityManagerFactory**를 하나만 생성한다.

```java
EntityManagerFactory emf = 
	Persistence.createEntityManagerFactory("jpabook");
```

- `Persistence.createEntityManagerFactory("jpabook")` 를 호출하면 `META-INF/persistence.xml`에 있는 정보를 바탕으로 **EntityManagerFactory**를 생성함
- 필요할 때마다 다음 코드와 같이 엔터티 매니저를 생성 가능

```java
EntityManager em = emf.createEntityManager();
```

> ***엔터티 매니저 팩토리 vs 엔터티 매니저***  

> **엔터티 매니저 팩토리**  
>  - 만드는 비용이 커서 한 개만 만들고 어플리케이션 전체에 공유  
>  - 서로 다른 스레드 간 공유 가능  
>  
> **엔터티 매니저**  
>  - 생성하는 비용이 거의 들지 않음  
>  - 스레드간 동시 접근하면 동시성 문제가 발생  

엔터티 매니저는 데이터베이스 연결이 꼭 필요한 시점까지 커넥션을 얻지 않음

*(보통 트랜잭션을 시작할 때, 커넥션을 획득)*

**하이버네이트**를 포함한 **JPA 구현체**들은 **EntityManagerFactory**를 생성할 때 **커넥션풀**도 만드는데 이건 ***J2SE*** 환경에서 사용하는 방법이다. (11장에서 자세히 알아볼 예정)

---

## 🧵 영속성 컨텍스트란?

***엔터티를 영구 저장하는 환경***이라는 뜻으로 엔터티 매니저로 엔터티를 저장하거나 조회하면 엔터티 매니저는 영속성 컨텍스트에 엔터티를 보관하고 관리한다.

영속성 컨텍스트는 논리적인 개념에 가깝고, 엔터티 매니저를 생성할 때 하나 만들어진다.

엔터티 매니저를 통해 영속성 컨텍스트에 접근할 수 있고, 관리할 수 있다.

```java
em.persist(member);
```

- `persist()` 메소드는 엔터티 매니저를 사용해서 회원 엔터티를 영속성 컨텍스트에 저장

---

## 🎠 엔터티의 생명주기

엔터티에는 ***비영속, 영속, 준영속, 삭제*** 로 4가지 상태가 존재한다.

### 1. 비영속 (new/transient)

*영속성 컨텍스트와 전혀 관계가 없는 상태*

엔터티 객체를 생성하고 저장하지 않으면 영속성 컨텍스트나 데이터베이스와 전혀 관련이 없게 된다. 이런 경우를 **비영속 상태**라고 한다.

```java
// 객체를 생성한 상태 (비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

### 2. 영속 (managed)

*영속성 컨텍스트에 저장된 상태*

엔터티 매니저를 통해 엔터티를 영속성 컨텍스트에 저장하면 영속성 컨텍스트에 의해 관리되는 상태가 되는데 이런 상태를 **영속 상태**라고 한다.

`em.find()` 나 ***JPQL***을 사용해서 조회한 엔터티도 영속성 컨텍스트가 관리하는 **영속 상태**다.

```java
// 객체를 저장한 상태 (영속)
em.persist(member);
```

### 3. 준영속 (detached)

*영속성 컨텍스트에 저장되었다가 분리된 상태*

영속성 컨텍스트가 관리하던 영속 상태의 엔터티를 관리하지 않으면 **준영속 상태**가 된다.

`em.detach()` 를 호출하면 **준영속 상태**로 만들 수 있다.

`em.close()` 를 호출해서 영속성 컨텍스트를 닫거나 `em.clear()` 호출해서 초기화하는 방법도 있다.

### 4. 삭제 (removed)

*삭제된 상태*

엔터티를 영속성 컨텍스트와 데이터베이스에서 **삭제**한다.

```java
em.remove(member)
```

---

## 📌 영속성 컨텍스트의 특징

- **영속성 컨텍스트와 식별자 값**
    
    엔터티를 식별자 값(@Id로 테이블의 기본 키와 매핑한 값)으로 구분한다.
    영속 상태는 식별자 값이 반드시 있어야 한다. (없으면 예외 발생)
    
- **영속성 컨텍스트와 데이터베이스 저장**
    
    *플러시(flush)*
    : JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔터티를 데이터베이스에 반영함
    
- **영속성 컨텍스트의 장점**
    - 1차 캐시
    - 동일성 보장
    - 트랜잭션을 지원하는 쓰기 지연
    - 변경 감지
    - 지연 로딩

### 1. 엔터티 조회

영속성 컨텍스트는 **1차 캐시**라는 내부 캐시를 가지고 있고, 영속 상태의 엔터티는 모두 이곳에 저장된다. (키가 @Id로 매핑한 식별자고, 값은 엔터티 인스턴스인 Map 형태)

```java
// 엔터티를 생성한 상태 (비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
// 엔터티를 저장한 상태 (영속)
em.persist(member);
```

위 코드를 실행하면 1차 캐시에 회원 엔터티를 저장할 수 있지만, 

아직 데이터베이스에는 저장되지 않았다.

```java
Member member = em.find(Member.class, "member1");
```

- 첫 번째 파라미터는 **엔터티 클래스 타입**이고, 두 번째는 조회할 **엔터티의 실별자 값**  
- `em.find()` 를 호출하면 먼저 **1차 캐시**에서 엔터티를 찾고, 만약 찾는 엔터티가 1차 캐시에 없으면 **데이터베이스**에서 조회한다.

**1차 캐시에서 조회**

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
//1차 캐시에 저장됨
em.persist(member);
//1차 캐시에서 조회
Member findMember = em.find(Member.class, "member1");
```

**데이터베이스에서 조회**

1. `em.find(Member.class, "member2")` 를 실행
2. member2가 1차 캐시에 없으므로 데이터베이스에서 조회
3. 조회한 데이터로 member2 엔터티를 생성해서 1차 캐시에 저장(영속 상태)
4. 조회한 엔터티를 반환

**영속 엔터티의 동일성 보장**

```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");

System.out.println(a == b); // 동일성 비교
```

⇒ 둘은 같은 인스턴스기 때문에, 영속성 컨텍스트는 성능상 이점과 엔터티의 동일성을 보장

### 2. 엔터티 등록

```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
// 엔터티 매니저는 데이터 변경 시 트랜잭션을 시작해야 한다.
transaction.begin(); // [트랜잭션] 시작

em.persist(memberA);
em.persist(memberB);
// 여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

- 엔터티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔터티를 저장하지 않고 내부 쿼리 저장소에 **INSERT SQL**을 모아둔다.
- 트랜잭션을 커밋할 때 모아둔 쿼리를 데이터베이스에 보내는데, 이를 **트랜잭션을 지원하는 쓰기 지연(transactional write-behind)** 라 함
- 트랜잭션을 커밋하면 엔터티 매니저는 영속성 컨텍스트를 **플러시**한다.
- 플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화하는 작업인데, 쓰기 지연 SQL 저장소에 모인 쿼리를 데이터베이스에 보낸다.

**트랜잭션을 지원하는 쓰기 지연이 가능한 이유**

등록 쿼리를 그때 그때 데이터베이스에 전달해도 트랜잭션을 커밋하지 않으면 소용이 없다.

커밋 직전에만 데이터베이스에 SQL을 전달하면 된다.

### 3. 엔터티 수정

**SQL 수정 쿼리의 문제점**

SQL을 사용하면 수정 쿼리를 작성해야 하는데 요구사항이 늘어나면 수정 쿼리도 추가된다.

이런 개발 방식의 문제점은 수정 쿼리가 많아지는 것은 물론이고 비즈니스 로직을 분석하기 위해 SQL을 계속 확인해야 하므로, 비즈니스 로직이 SQL에 의존하게 된다.

**변경 감지**

JPA로 엔터티를 수정할 때는 단순히 엔터티를 조회해서 데이터만 변경하면 된다. `update()` 메소드는 따로 없고, 데이터를 변경하기만 하면 데이터베이스에 자동으로 반영된다.

이런 기능을 **변경 감지(dirty checking)** 라 한다.

1. 트랜잭션을 커밋하면 엔터티 매니저 내부에서 먼저 플러시(flush())가 호출된다.
2. 엔터티와 스냅샷을 비교해서 변경된 엔터티를 찾는다.
3. 변경된 엔터티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.
4. 쓰기 지연 저장소의 SQL을 데이터베이스에 보낸다.
5. 데이터베이스 트랜잭션을 커밋한다.
- JPA는 엔터티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 저장해 두는데 이를 스냅샷이라 한다.
- 플러시 시점에 스냅샷과 엔터티를 비교해서 변경된 엔터티를 찾는다.
- 변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔터티에만 적용된다.
- 변경 감지로 실행된 UPDATE SQL은 엔터티의 모든 필드를 업데이트한다.
    - 수정 쿼리가 항상 같기 때문에 어플리케이션 로딩 시점에 수정 쿼리를 미리 생성하고 재사용 가능
    - 데이터베이스가 이전에 한 번 파싱된 쿼리를 재사용 가능
    
    @org.hibernate.annotations.DynamicUpdate 어노테이션을 사용하면 수정된 데이터만 사용해서 동적으로 UPDATE SQL 생성 가능
    

### 4. 엔터티 삭제

```java
Member memberA = em.find(Member.class, "member1");
em.remove(memberA);
```

- 먼저 삭제 대상 엔터티를 조회하고 `em.remove()` 에 삭제 대상 엔터티를 넘겨주면 삭제 쿼리를 쓰기 지연 SQL 저장소에 등록한다.
- 이후 트랜잭션을 커밋해서 플러시를 호출하면 실제 데이터베이스에 삭제 쿼리를 전달한다.
- `em.remove(memberA)` 를 호출하는 순간 memberA는 영속성 컨텍스트에서 제거된다.
- 이렇게 삭제된 엔터티는 재사용하지 말고 자연스럽게 가비지 컬렉션의 대상이 되도록 두는 것이 좋다.

---

## ✨ 플러시

플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.

1. 변경 감지가 동작해서 영속성 컨텍스트에 있는 모든 엔터티를 스냅샷과 비교해서 수정된 엔터티를 찾는다. 수정된 엔터티는 수정 쿼리를 만들어 쓰기 지연 SQL 저장소에 등록한다.
2. 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송한다.

### 1. **플러시 방법**

- **직접 호출**
    
    엔터티 매니저의 `flush()` 메소드를 직접 호출해서 영속성 컨텍스트를 강제로 플러시
    
    테스트나 다른 프레임워크와 JPA를 함께 사용할 때를 제외하고 거의 사용 X
    
- **트랜잭션 커밋 시 플러시 자동 호출**
    
    트랜잭션을 커밋하기 전에 꼭 플러시를 호출해서 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영해야한다.
    
- **JPQL 쿼리 실행 시 플러시 자동 호출**
    
    **JPQL**이나 **Criteria** 같은 객체지향 쿼리를 호출할 때도 플러시가 실행된다.
    
    **JPQL**은 SQL로 변환되어 데이터베이스에서 엔터티를 조회하기 때문에 쿼리를 실행하기 직전에 영속성 컨텍스트를 플러시해서 변경 내용을 데이터베이스에 반영해야 한다.
    
    *(식별자를 기준으로 조회하는 find() 메소드를 호출할 때는 플러시가 실행되지 않음)*
    

### 2. 플러시 모드 옵션

`javax.persistence.FlushModeType`을 사용해서 엔터티 매니저에 플러시 모드 지정 가능

- FlushModeType.AUTO : 커밋이나 쿼리를 실행할 때 플러시 (기본값)
- FlushModeType.COMMIT : 커밋할 때만 플러시

```java
em.setFlushMode(FlushModeType.COMMIT);
```

---

## 🎭 준영속

준영속 상태의 엔터티는 영속성 컨텍스트가 제공하는 기능을 사용할 수 없다.

**준영속 상태로 만드는 방법**

1. em.detach(entity) : 특정 엔터티만 준영속 상태로 전환
2. em.clear() : 영속성 컨텍스트를 완전히 초기화
3. em.close() : 영속성 컨텍스트를 종료

### 1. 엔터티를 준영속 상태로 전환: detach()

이 메소드를 호출하는 순간 1차 캐시부터 쓰기 지연 SQL 저장소까지 해당 엔터티를 관리하기 위한 모든 정보가 제거된다.

이미 준영속 상태이므로 영속성 컨텍스트가 지원하는 어떤 기능도 동작하지 않고, 쓰기 지연 SQL 저장소의 INSERT SQL도 제거되어서 데이터베이스에 저장되지 않는다.

### 2. 영속성 컨텍스트 초기화: clear()

detach()는 특정 엔터티 하나를 준영속 상태로 만들지만, clear()는 해당 영속성 컨텍스트의 모든 엔터티를 준영속 상태로 만든다.

준영속 상태이므로 영속성 컨텍스트가 지원하는 변경 감지는 동작하지 않는다.

### 3. 영속성 컨텍스트 종료: close()

영속성 컨텍스트를 종료하면 해당 영속성 컨텍스트가 관리하던 영속 상태의 엔터티가 모두 준영속 상태가 된다.

영속 상태의 엔터티는 주로 영속성 컨텍스트가 종료되면서 준영속 상태가 된다. 개발자가 직접 준영속 상태로 만드는 일은 드물다.

### 4. 준영속 상태의 특징

- **거의 비영속 상태에 가깝다**
    
    영속성 컨텍스트가 관리하지 않으므로 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작 X
    
- **식별자 값을 가지고 있다**
    
    비영속 상태는 식별자 값이 없을 수도 있지만 준영속 상태는 반드시 식별자 값을 가지고 있다.
    
- **지연 로딩을 할 수 없다**
    
    지연 로딩(LAZY LOADING)은 실제 객체 대신 프록시 객체를 로딩해두고 해당 객체를 실제 사용할 때 영속성 컨텍스트를 통해 데이터를 불러오는 방법
    
    준영속 상태는 영속성 컨텍스트가 더는 관리하지 않으므로 지연 로딩 문제가 발생함
    

### 5. 병합: merge()

*준영속 상태의 엔터티를 다시 영속 상태로 변경*

merge() 메소드는 준영속 상태의 엔터티를 받아서 그 정보로 새로운 영속 상태의 엔터티를 반환

**준영속 병합**

1. merge() 실행
2. 파라미터로 넘어온 준영속 엔터티의 식별자 값으로 1차 캐시에서 엔터티를 조회
(만약 1차 캐시에 엔터티가 없으면 데이터베이스에서 엔터티를 조회하고 1차 캐시에 저장)
3. 조회한 영속 엔터티에 member 엔터티 값을 채워 넣는다.
4. mergeMember를 반환

**비영속 병합**

merge는 비영속 엔터티도 영속 상태로 만들 수 있다.

```java
Member member = new Member();
Member newMember = em.merge(member);
tx.commit();
```

병합은 파라미터로 넘어온 엔터티의 식별자 값으로 영속성 컨텍스트를 조회하고 찾는 엔터티가 없으면 데이터베이스에서 조회한다. 데이터베이스에서도 발견하지 못하면 새로운 엔터티를 생성해서 병합한다.
