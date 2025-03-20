DB config

```java
@Configuration
@MapperScan(basePackages = "com.sist.app.ex0729_secure.mapper") // 패키지 경로 확인
public class DbConfig {
    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource ds) throws Exception {
        SqlSessionFactoryBean sb = new SqlSessionFactoryBean();
        sb.setDataSource(ds);

        PathMatchingResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
        sb.setMapperLocations(resolver.getResources("classpath:mapper/**/*.xml"));
        return sb.getObject();
    }

    @Bean
    public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory factory) {
        return new SqlSessionTemplate(factory);
    }
}
```

## [@MapperScan이란?](https://honinbo-world.tistory.com/91)

@MapperScan annotation을 명시해 준 class는 basePackages로 지정한 곳에 존재하는 @Mapper로 명시된 interface를 스캔한다.

```java
@Mapper
public interface BbsMapper {
    int count(String searchType, String searchValue , String bname);
    // mybatis 인자를 많이 받을수 없어 자동으로 map으로 묶어준다
    List<BbsVO> bbsList(String searchType, String searchValue , String bname, int begin, int end);
}
```

###  **스프링 빈(Bean) 이란?**

> 빈(Bean)은 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트이다. 

즉, 스프링 컨테이너가 관리하는 자바 객체를 뜻하며, 하나 이상의 빈(Bean)을 관리한다.

빈은 인스턴스화된 객체를 의미하며, 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다. 

쉽게 이해하자면 new 키워드 대신 사용한다고 보면된다.

 **스프링 빈(Bean) 사용이유 ?**

가장 큰 이유는 스프링 간 객체가 의존관계를 관리하도록 하는 것에 가장 큰 목적이 있다.  객체가 의존관계를 등록할 때 스프링 컨테이너에서 해당하는 빈을 찾고, 그 빈과 의존성을 만든다.

###  **스프링 빈(Bean) 등록 방법**

**Spring Bean을 등록하는 방법은 대표적으로 3가지 정도가 있다.**

- xml에 직접 등록
- @Bean 어노테이션을 이용
- @Component, @Controller, @Service, @Repository 어노테이션을 이용