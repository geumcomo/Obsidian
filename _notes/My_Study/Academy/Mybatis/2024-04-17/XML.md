```xml 
<?xml version="1.0" encoding="UTF-8"?>

<!-- 주석

- XML (eXtensible Markup language)

: Extensible - 확장 가능하다는 의미

결국 확장 가능한 markup 언어(markup:조판지시)

<대제목>자바의 특징 </대제목> 글씨 크기 할필요없다 꺽새로 표현

내용

<그림1>doc/images/img1.png

xml은 세계 표준이다. (w3.org) html 정해진 태그만 사용 하지만 xml은 확장 태그 사용가능

xml은 html을 대체하기 위해 나온 언어가 아니다.

현재 웹 표준 언어는 html

xml의 구조에서 중요한 것은 첫 태그(root)는 오직 1개만 가능!

-태그(element) 이름은 숫자로 시작할 수 없고, 공백이 포함되어서는 안된다.

태그가 열렸으면 반드시 닫아야 한다.<test> 내용 O</test>

또는 내용 X<test id = "a"/> 테이블

-->

<main>

<product>제품1</product>

<product>제품2</product>

<product>제품3</product>

</main>

<?xml version="1.0" encoding="UTF-8"?>

<total>

<person>

<name>홍길동</name>

<email>hong@korea.com</email>

<age>22</age>

</person>

<person>

<name>마루치</name>

<email>maru@korea.com</email>

<age>24</age>

</person>

</total>

<?xml version="1.0" encoding="UTF-8"?>

<!-- 교육생 정보를 저장하려 한다. (이름,연락처,거주지) -->

<info>

<student>

<name>마루치</name>

<phone>010-0232-2323</phone>

<address>서울시</address>

</student>

<student>

<name>홍길동</name>

<phone>010-0231-2323</phone>

<address>경기도</address>

</student>

</info>

<!-- 어떤 작업자는 <info>가 아니라 <total>로 작업을 할 수 도 있다 그럼 이런 오류를 없애기 우해 문서에 대한 규칙을 정하면 된다

그것이 바로 DTD(document type definition)-->

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE info [

<!ELEMENT info (student+)> <!--(+)생략불가 중복 가능 -->

<!ELEMENT student (name,phone+,address?)> <!--(?)생략가능 -->

<!ELEMENT name (#PCDATA)>

<!ELEMENT phone (#PCDATA)>

<!ELEMENT address (#PCDATA)>

]>

<info>

<student>

<name>마루치</name>

<phone>010-0232-2323</phone>

<phone>010-1588-2323</phone>

</student>

<student>

<name>홍길동</name>

<phone>010-0231-2323</phone>

<address>경기도</address>

</student>

</info>

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE 회원목록 [

<!ELEMENT 회원목록 (회원+, 이달의회원, 신규회원)>

<!ELEMENT 회원 (이름, 전화번호)>

<!ELEMENT 이름 (#PCDATA)>

<!ELEMENT 전화번호 (#PCDATA)>

<!ELEMENT 이달의회원 EMPTY>

<!ELEMENT 신규회원 EMPTY>

<!-- 회원태그에 등급이라는 속성을 정의한다. 이때 등급의 값으로는 준회원 또는 정회원 또는 우수회원중 하나만 허용한다. 만약! 생략시에는 준회원으로

자동 정의한다. -->

<!ATTLIST 회원 등급 (준회원|정회원|우수회원) "준회원">

<!-- 회원태그에 회원번호라는 속성을 정의한다. 이때 회원번호의 값은 ID와 같이 중복되어서는 안되며 반드시 정의해야한다. -->

<!ATTLIST 회원 번호 ID #REQUIRED>

<!-- 이달의 회원 태그에 회원번호라는 속성을 정의한다. 이때 회원번호의 값은 ID를 참조하는 값이어야 반드시 정의해야한다. -->

<!ATTLIST 이달의회원 회원번호 IDREF #REQUIRED>

<!-- 신규회원 태그에 회원번호라는 속성을 정의한다, 이때 회원번호의 갑은 ID들을 참조하는 값이어야하며 반드시 정의해야한다.-->

<!ATTLIST 신규회원 회원번호 IDREFS #REQUIRED>

]>

<회원목록>

<회원 회원번호= "A123">

<이름>일지매</이름>

<전화번호>010</전화번호>

</회원>

<회원 회원번호= "B133">

<이름>마루치</이름>

<전화번호>010</전화번호>

</회원>

<이달의회원 회원번호 = "A123"/>

<신규회원 회원번호 = "B133 A123"/>

</회원목록>

<?xml version="1.0" encoding="UTF-8"?>

<!-- 외부 dtd참조 -->

<!DOCTYPE list SYSTEM "ex6.dtd"> <!-- 공동 system 표준 public -->

<List>

<personal number="129">

<name>마루치</name>

<c_phone>010</c_phone>

<email>maru@naver.com</email>

</personal>

</List>

```dtd
<?xml version="1.0" encoding="UTF-8"?>

<!-- 외부 dtd파일은 <!doctype 를 선언하지 않는다. -->

<!ELEMENT List (personal)+>

<!ELEMENT personal (name, c_phone, email?)>

<!ELEMENT name (#PCDATA)>

<!ELEMENT c_phone (#PCDATA)>

<!ELEMENT email (#PCDATA)>

<!ATTLIST personal number CDATA #REQUIRED>

<!ENTITY sist "쌍용교육센터">

```

