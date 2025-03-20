### association 과 collection 사용 역할 구분하기!

매핑결과를 깔끔하게 해주는 association 과 collection 은 결과데이터 간의 n:n 관계에 따라 사용을 달리 해준다.

#### **1. 네이버 영어사전.. 으로 이해해보기**

association은 제휴, 연계, 유대(combination) collection은 (물건·사람들의) 무리, 더미

제휴는 어쨌든 1:1로 관계를 맺는 거니까, **association**은 1:**1** 관계에서 사용한다고 생각하기  
무리나 더미는 여러가지를 한꺼번에 모아놓는 거니까, **collection**은 1:**N** 관계에서 사용한다고 생각하기! 

결론은 

association은 객체와 객체간의 관계가 **1:1 매핑**이 될 때 사용한다.   
collection은 객체와 객체간의 관계가 **1:N 매핑**이 될 때 사용한다.

#### **2. association, collection의 속성값 설명**

예)

```
<association property="additionalInfoVO" column="{memberSeq=MBR_SEQ}" javaType="MemberAdditionalInfoVO" select="getMemberAdditionalInfo"/>
```

- **property** = 쿼리결과를 세팅할 변수명

- **column** = (association으로 가져올) 데이터 색인에 사용할 필드, 여러개 사용 가능, 콤마(,)로 구분  
                   예)column=”{prop1=col1,prop2=col2}” 

- **select** = 사용할 select 쿼리 id. 같은 xml 파일 내에 있는 쿼리가 아닐경우, 매퍼의 namespace + 쿼리 id

- **resultMap** = 결과데이터를 객체에 매핑시키는 것에 대한 정의

- **columnPrefix** = 지정된 Prefix로 시작하는 컬럼에 대해서만 매핑한다. (예를들어 같은 이름의 컬럼을 가져 올 때)  
  
  예)  
, MEMBER_ID   
, MEMBER_ID AS ANOTHER_MEMBER_ID  
에서 Map 데이터의 속성값에 columnPrefix = "ANOTHER_" 로 지정 시 ANOTHER_ 접두어가 붙은 컬럼을 매핑할 수 있음.

즉, prefix도 상속됨!!!!

  
- **javaType** = association의으로 가져올 결과를 매핑할 변수의 타입

- **ofType** = collection 으로 가져올 결과를 매핑할 변수의 타입.. association의 javaType이랑 같은 역할 인 듯!  
즉, association 에서는 javaType으로 결과객체의 타입을 명시해주고, collection 에서는 ofType으로 명시해준다.

출처: [https://turing0809.tistory.com/96](https://turing0809.tistory.com/96) [정리라도 하자:티스토리]
