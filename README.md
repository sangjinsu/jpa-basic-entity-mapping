# jpa-basic-entity-mapping
엔티티 매핑

## JPA 엔티티 매핑

### 객체와 테이블 매핑 @Entity @Table

#### @Entity

- JPA가 관리하는 엔티티
- 매핑할 클래스틑 필수적
- 주의
  - 기본 생성자 필수
  - final X enum interface inner X

#### @Table

- name : 매핑할 테이블 이름

### 데이터베이스 스키마 자동 생성

- DDL 을 애플리케이션 실행 시점에 자동 생성
- 테이블 중심 -> 객체 중심
- 데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL 생성
- DDL 은 개발 장비에서만 사용
- 생성된 DDL 은 운영서버에서는 사용하지 않거나 적절히 다듬어서 사용

- 주의

  - create, create-drop, update 사용 X
  - 개발 초기에는 create 또는 update
  - 테스트 서버는 update 또는 validate
  - 스테이징과 운영 서버는 validate 또는 none

- 제약 조건 추가: 회원 이름 필수, 10자 초과 X
  - @Column(nullable=false, length=10)

### 필드와 컬럼 매핑 @Column

- @Column 컬럼 매핑
  - name : 필드와 매핑할 테이블 컬럼 이름
  - insertable, updatable : 등록, 병경 가능 여부
  - nullable : null 값의 허용 여부를 설정한다
  - unique : 유니크 제약 조건걸 때 사용 (잘 사용하지 않음) 클래스에서 제약조건을 사용하는 방향이다
  - length : 문자 길이 제약조건 String 타입에만 사용한다
- @Temporal 날짜 타입 매핑
- @Enumerated enum 타입 매핑
  - 주의 ORDINAL 사용 X (enum 에 값 추가시 데이터베이스에 오류 발생 가능성 매우 높음)
- @Lob BLOB CLOB 매핑
- @Transient 특정 필드를 컬럼

### 기본 키 매핑 @Id

- @GeneratedValue

  - strategy = GenerationType.IDENTITY
    - 기본 키 생성을 데이터베이스에 위임
    - MySQL, PostgreSQL, DB2 에서 사용
    - JPA는 보통 트랜잭션 커밋 시점에 Insert SQL 실행 
    - AUTO_INCREMENT 는 데이터베이스에 INSERT SQL 실행한 이후에 ID 값을 알 수 있음
    - IDENTITY 전략은 em.persist() 시점에서 즉시 INSERT SQL 실행하고 DB 에서 식별자를 조회

  - :star2: strategy = GenerationType.SEQUENCE
    - @SequenceGenerator()
      - name: 식별자 생성기 이름
      - sequenceName: 데이터베이스에 등록되어 있는 시퀀스 이름
      - initialValue: DDL 생성 시에만 사용됨, 시퀀시 DDL을 생성할 때 처음 1 시작하는 수를 지정한다 
      - <u>**allocationSize: 시퀀스 한 번 호출에 증가하는 수, 성능 최적화에 사용되며 데이터베이스 시퀀스 값이 하나씩 증가하도록 설정되어 있으면 이 값을 반드시 1로 설정해야 한다 (기본값 50)**</u>
      - catalog, schema: 데이터베이스 catalog, schema 이름 
      - @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "sequence_name")

  - strategy = GenerationType.TABLE
    - 키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 흉내내는 전략
    - 장점: 모든 데이터베이스에 적용 가능
    - 단점: 성능 

#### 식별자 전략 권장

- 기본키 제약 조건 null 아님, 유일, 변하면 안된다 

- 미래까지 변하지 않는다는 조건을 만족하는 자연키를 찾기 힘들다. 대리키를 사용하자 

- 주민등록번호도 기본 키로 적절하지 않다 

- 권장 사항 : Long 형 + 대체키 + 키 생성 전략 사용 

  

### 연관관계 매핑 @ManyToOne @JoinColumn

```java
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;
```

#### 외래키가 있는 곳을 주인으로 정해야 한다 

#### 양방향 매핑시 연관관계를 둘 다 설정해 주어야 한다 

- 순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자 
- 연관관계 편의 메소드를 생성하자 
- 양방향 매핑시에 무한 루프를 조심하자 

#### 단방향 매핑만으로도 이미 연관관계 매핑 완료

- 양방향 매핑은 반대 방향으로 조회 기능이 추가된 것 뿐 

  



