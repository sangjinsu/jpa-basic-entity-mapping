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

### 연관관계 매핑 @ManyToOne @JoinColumn
