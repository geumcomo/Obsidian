
 
 
## H2 데이터베이스 설치

<[H2 Database Engine](https://www.h2database.com/html/main.html)>

1. 스프링 버전에 맞게 설치
2. 데이터베이스 파일 생성 방법 ` jdbc:h2:~/test` (최초 한번) 
3. ` ~/test.mv.db` 파일 생성 확인 이후부터는 ` jdbc:h2:tcp://localhost/~/test` 이렇게 접속

## 순수 Jdbc

### 환경 설정

```java 
implementation 'org.springframework.boot:spring-boot-starter-jdbc' 
runtimeOnly 'com.h2database:h2'
```

build.gradle 파일에 jdbc, h2 데이터베이스 관련 라이브러리 추가 


### 스프링 부트 데이터베이스 연결 설정 추가

`resources/application.properties`

```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test 
spring.datasource.driver-class-name=org.h2.Driver 
spring.datasource.username=sa
```


> [!참고]
> 인텔리J 커뮤니티(무료) 버전의 경우 공백 주의, 공백은 모두 제거해야 한다. ` application.properties ` ` 파일의 왼쪽이 다음 그림고 같 이 회색으로 나온다. 엔터프라이즈(유료) 버전에서 제공하는 스프링의 소스 코드를 연결해주는 편의 기능이 빠진 것인데, 실제 동작하는데는 아무런 문제가 없다.


## jdbc 리포지토리 구현 (예전방식)
```java
package hello.hellospring.repository;
 import hello.hellospring.domain.Member;
 import org.springframework.jdbc.datasource.DataSourceUtils;
 import javax.sql.DataSource;
 import java.sql.*;
 import java.util.ArrayList;
 import java.util.List;
 import java.util.Optional;
public class JdbcMemberRepository implements MemberRepository {
 private final DataSource dataSource;
 public JdbcMemberRepository(DataSource dataSource) {
 this.dataSource = dataSource;
    }
    @Override
 public Member save(Member member) {
 String sql = "insert into member(name) values(?)";
 Connection conn = null;
 PreparedStatement pstmt = null;
 ResultSet rs = null;
 try {
            conn 
= getConnection();
            pstmt 
= conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
            pstmt.setString(1, member.getName());
            pstmt.executeUpdate();
            rs 
= pstmt.getGeneratedKeys();
 if (rs.next()) {
                member.setId(rs.getLong(1));
            } 
else {
 throw new SQLException("id 조회 실패");
            }
 return member;
        } 
        } 
        }
    }
 catch (Exception e) {
 throw new IllegalStateException(e);
 finally {
 close(conn, pstmt, rs);
    @Override
 public Optional<Member> findById(Long id) {
 String sql = "select * from member where id = ?";
 Connection conn = null;
PreparedStatement pstmt = null;
 ResultSet rs = null;
 try {
            conn 
= getConnection();
            pstmt 
= conn.prepareStatement(sql);
            pstmt.setLong(1, id);
            rs 
= pstmt.executeQuery();
 if(rs.next()) {
 Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
 return Optional.of(member);
            } 
else {
 return Optional.empty();
            }
        } 
        } 
        }
    }
 catch (Exception e) {
 throw new IllegalStateException(e);
 finally {
 close(conn, pstmt, rs);
    @Override
 public List<Member> findAll() {
 String sql = "select * from member";
 Connection conn = null;
 PreparedStatement pstmt = null;
 ResultSet rs = null;
 try {
            conn 
= getConnection();
            pstmt 
= conn.prepareStatement(sql);
            rs 
= pstmt.executeQuery();
 List<Member> members = new ArrayList<>();
 while(rs.next()) {
Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                members.add(member);
            }
        } 
        } 
        }
    }
 return members;
 catch (Exception e) {
 throw new IllegalStateException(e);
 finally {
 close(conn, pstmt, rs);
    @Override
 public Optional<Member> findByName(String name) {
 String sql = "select * from member where name = ?";
 Connection conn = null;
 PreparedStatement pstmt = null;
 ResultSet rs = null;
 try {
            conn 
= getConnection();
            pstmt 
= conn.prepareStatement(sql);
            pstmt.setString(1, name);
            rs 
= pstmt.executeQuery();
 if(rs.next()) {
 Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
 return Optional.of(member);
            }
 return Optional.empty();
        } 
        } 
        }
 catch (Exception e) {
 throw new IllegalStateException(e);
 finally {
 close(conn, pstmt, rs);
    }
private Connection getConnection() {
 return DataSourceUtils.getConnection(dataSource);
    }
 private void close(Connection conn, PreparedStatement pstmt, ResultSet rs) {
 try {
 if (rs != null) {
                rs.close();
            }
        } 
catch (SQLException e) {
            e.printStackTrace();
        }
 try {
 if (pstmt != null) {
                pstmt.close();
            }
        } 
catch (SQLException e) {
            e.printStackTrace();
        }
 try {
 if (conn != null) {
 close(conn);
            }
        } 
catch (SQLException e) {
            e.printStackTrace();
        }
    }
 private void close(Connection conn) throws SQLException {
 DataSourceUtils.releaseConnection(conn, dataSource);
    }
 }
```

### 스프링 설정 변경

```java
package hello.hellospring;
 import hello.hellospring.repository.JdbcMemberRepository;
 import hello.hellospring.repository.JdbcTemplateMemberRepository;
 import hello.hellospring.repository.MemberRepository;
 import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
 import org.springframework.context.annotation.Bean;
 import org.springframework.context.annotation.Configuration;
 import javax.sql.DataSource;
 @Configuration
 public class SpringConfig {
 private final DataSource dataSource;
 public SpringConfig(DataSource dataSource) {
 this.dataSource = dataSource;
    }
    @Bean
 public MemberService memberService() {
 return new MemberService(memberRepository());
    }
    @Bean
 public MemberRepository memberRepository() {
 //return new MemoryMemberRepository(); 
 return new JdbcMemberRepository(dataSource); }
 
 }
```

DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정 보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어둔다. 그래서 DI를 받을 수 있다.

> [!개방-폐쇄 원칙(OCP, Open-Closed Principle)]
> 스프링의 DI (Dependencies Injection)을 사용하면 래스를 변경 할 수 있다. 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경 할 수 있다.


## 스프링 통합 테스트

```java
package hello.hellospring.service;  
  
import hello.hellospring.domain.Member;  
import hello.hellospring.repository.MemberRepository;  
import org.assertj.core.api.Assertions;  
import org.junit.jupiter.api.Test;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.boot.test.context.SpringBootTest;  
import org.springframework.transaction.annotation.Transactional;  
  
import java.util.List;  
  
import static org.assertj.core.api.AssertionsForClassTypes.assertThat;  
import static org.junit.jupiter.api.Assertions.assertThrows;  
  
@SpringBootTest  
@Transactional  
class MemberServiceIntegrationTest {  
    @Autowired  
    MemberService memberService;  
    @Autowired  
    MemberRepository memberRepository;  
  
    @Test  
    void 회원가입() throws Exception {  
        //given  
        Member member1 = new Member();  
        member1.setName("geummo");  
  
        //when  
        long join = memberService.join(member1);  
        //then  
        Assertions.assertThat(member1.getId()).isEqualTo(join);  
    }  
  
    @Test  
    void 중복회원가입() throws Exception {  
        //given  
        Member member1 = new Member();  
        member1.setName("geummo");  
        Member member2 = new Member();  
        member2.setName("geummo");  
  
        //when  
        memberService.join(member1);  
        //then  
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));  
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");  
    }  
  
    @Test  
    void 회원조회() throws Exception {  
        //given  
        Member member1 = new Member();  
        member1.setName("geummo");  
        Member member2 = new Member();  
        member2.setName("kummo");  
        memberService.join(member1);  
        memberService.join(member2);  
        //when  
        List<Member> members = memberService.findMembers();  
        //then  
        Assertions.assertThat(members).hasSize(2);  
    }  
  
    @Test  
    void 아이디찾기() {  
        //given  
        Member member1 = new Member();  
        member1.setName("geummo");  
        memberService.join(member1);  
        //when  
        Member member2 = memberService.findOne(member1.getId()).get();  
        //then  
        Assertions.assertThat(member1).isEqualTo(member2);  
    }  
}
```

`@SpringBootTest` : 스프링 컨테이너와 테스트를 함께 실행한다.

`@Transactional`: 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다. (@AfterEach, @BeforeEach)

> [!객체 비교]
> 테스트에서 객체가 같은지 비교하기 위해서 같은 참조를 하기 위한 @AutoWire를 설정하고,
> 객체 클래스한에 `equals()` ,`hashcode()`를  꼭 선언해야 한다.


## JdbcTemplate

`JdbcMemberRepository`를 보면 같은 변수가 여러 개 선언되어있다. JdbcTemplate을 사용하면 최대한 줄일 수 있다.

- 순수 Jdbc와 동일한 환경설정을 하면 된다.
- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야 한다.

```java
package hello.hellospring.repository;  
  
import hello.hellospring.domain.Member;  
import org.springframework.jdbc.core.JdbcTemplate;  
import org.springframework.jdbc.core.RowMapper;  
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;  
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;  
  
import javax.sql.DataSource;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.util.HashMap;  
import java.util.List;  
import java.util.Map;  
import java.util.Optional;  
  
public class JdbcMemberTemplate implements MemberRepository {  
  
    private final JdbcTemplate jdbcTemplate;  
  
    public JdbcMemberTemplate(DataSource dataSource) {  
        this.jdbcTemplate = new JdbcTemplate(dataSource);  
    }  
  
    @Override  
    public Member save(Member member) {  
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);  
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");  
  
        Map<String, Object> parameters = new HashMap<>();  
        parameters.put("name", member.getName());  
  
        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));  
        member.setId(key.longValue());  
        return member;  
    }  
  
    @Override  
    public Optional<Member> findById(Long id) {  
        List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);  
        return result.stream().findAny();  
    }  
  
    @Override  
    public Optional<Member> findByName(String name) {  
        List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);  
        return result.stream().findAny();  
    }  
  
    @Override  
    public List<Member> findAll() {  
        return jdbcTemplate.query("select * from member", memberRowMapper());  
    }  
  
//    private RowMapper<Member> memberRowMapper() {  
//        return (rs, rowNum) -> {  
//            Member member = new Member();  
//            member.setId(rs.getLong("id"));  
//            member.setName(rs.getString("name"));  
//            return member;  
//        };  
//    }  
    private RowMapper<Member> memberRowMapper(){  
        return new RowMapper<Member>() {  
            @Override  
            public Member mapRow(ResultSet rs, int rowNum) throws SQLException {  
                Member member = new Member();  
                member.setId(rs.getLong("id"));  
                member.setName(rs.getString("name"));  
                return member;  
            }  
        };  
    }  
}
```

- 생성자가 하나라면 `@AutoWire` 생략 가능
- `RowMapper` : JDBC의 결과(ResultSet)를 객체로 변환하는 역할(`쿼리 결과를 매핑하는 기능`)

## JPA

### 라이브러리 추가 
```build.gradle
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

### 설정 추가
```application.properties
spring.jpa.show-sql=true  
spring.jpa.hibernate.ddl-auto=none
```
- `show-sql` : JPA가 생성하는 SQL을 출력한다. 
- `ddl-auto` : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다. 
	- create 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.

## `Member`
```java
package hello.hellospring.domain;  
  
import jakarta.persistence.*;  
  
import java.util.Objects;  
  
@Entity  
public class Member {  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private long id;  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public long getId() {  
        return id;  
    }  
  
    public void setId(long id) {  
        this.id = id;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (o == null || getClass() != o.getClass()) return false;  
        Member member = (Member) o;  
        return id == member.id && Objects.equals(name, member.name);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(id, name);  
    }  
}
```
- `@Entity` : JPA가 관리하는 코드
- `@Id`: 기본키
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`: 자동 증가 (아이덴티티 전략)


```java
package hello.hellospring.repository;  
  
import hello.hellospring.domain.Member;  
import jakarta.persistence.EntityManager;  
  
import java.util.List;  
import java.util.Optional;  
  
public class JpaMemberRepository implements MemberRepository {  
    private final EntityManager em;  
  
    public JpaMemberRepository(EntityManager em) {  
        this.em = em;  
    }  
  
    @Override  
    public Member save(Member member) {  
        em.persist(member);  
        return member;  
    }  
  
    @Override  
    public Optional<Member> findById(Long id) {  
        Member member = em.find(Member.class, id);  
        return Optional.ofNullable(member);  
    }  
  
    @Override  
    public Optional<Member> findByName(String name) {  
        List<Member> result = em.createQuery("select m from Member m where m.name= :name", Member.class).setParameter("name", name).getResultList();  
        return result.stream().findAny();  
    }  
  
    @Override  
    public List<Member> findAll() {  
        return em.createQuery("select m from Member m", Member.class).getResultList();  
    }  
}
```
- `EntityManager`: 데이터를 저장 및 관리
- `persist()` : JPA 저장 함수
- `find` : JPA 찾는 함수
- `createQuery`: JPQL 사용 (Entity객체를 가져옴)

## 서비스 계층에 트랜잭션 추가

```java
import hello.hellospring.domain.Member;  
import hello.hellospring.repository.MemberRepository;  
import org.springframework.transaction.annotation.Transactional;  
  
import java.util.List;  
import java.util.Optional;  
  
@Transactional  
public class MemberService {
```

- `org.springframework.transaction.annotation.Transactional`
- ==JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.==

## 스프링 설정 변경

```java
package hello.hellospring;  
  
import hello.hellospring.repository.JdbcMemberRepository;  
import hello.hellospring.repository.JdbcMemberTemplate;  
import hello.hellospring.repository.JpaMemberRepository;  
import hello.hellospring.repository.MemberRepository;  
import hello.hellospring.service.MemberService;  
import jakarta.persistence.EntityManager;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class SpringConfig {  
    private EntityManager em;  
  
    public SpringConfig(EntityManager em) {  
        this.em = em;  
    }  
  
    @Bean  
    public MemberService memberService() {  
        return new MemberService(memberRepository());  
    }  
  
    @Bean  
    public MemberRepository memberRepository() {  
        return new JpaMemberRepository(em);  
    }  
}
```

## 통합 테스트

```java
@SpringBootTest  
@Transactional  
class MemberServiceIntegrationTest {  
    @Autowired  
    MemberService memberService;  
    @Autowired  
    MemberRepository memberRepository;  
  
    @Test  
    @Commit    void 회원가입() throws Exception {  
        //given  
        Member member1 = new Member();  
        member1.setName("geummo");  
  
        //when  
        long join = memberService.join(member1);  
        //then  
        Assertions.assertThat(member1.getId()).isEqualTo(join);  
    }
```

`@Commit` : 데이터 반영 

## 스프링 데이터 JPA

`Interface`만으로 개발을 완료 할 수 있다. `CRUD`기능 모두 제공
`JPA`는 선택이 아닌 필수!  `JPA`를 먼저 배우고` Spring Data JPA`를 배워야 한다. -김영한-

## 스프링 데이터 리포지토리
```java
package hello.hellospring.repository;  
  
import hello.hellospring.domain.Member;  
import org.springframework.data.jpa.repository.JpaRepository;  
  
import java.util.Optional;  
  
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {  
    @Override  
    Optional<Member> findByName(String name);  
}
```

## 스프링 설정
```java
package hello.hellospring;  
  
import hello.hellospring.repository.MemberRepository;  
import hello.hellospring.service.MemberService;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class SpringConfig {  
    private final MemberRepository memberRepository;  
  
    public SpringConfig(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }  
  
    @Bean  
    public MemberService memberService() {  
        return new MemberService(memberRepository);  
    }  
  
}
```

- 스프링 데이터 JPA가 `SpringDataJpaMemberRepository` 를 스프링 빈으로 자동 등록해준다.

### 스프링 데이터 JPA 제공 기능
- 인터페이스를 통한 기본적인 `CRUD` 
- `findByName()` , `findByEmail()` 처럼 메서드 이름 만으로 조회 기능 제공 
- 페이징 기능 자동 제공