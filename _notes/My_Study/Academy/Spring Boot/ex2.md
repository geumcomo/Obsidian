위에 보이는 파일은 가장 기본이 되는 **application.properties** 파일로 스프링부트가 어플리케이션을 구동할 때 자동으로 로딩하는 파일입니다.
그렇다면 개발환경에 따라 properties를 다르게 사용하는 이유와 사용하는 방법은 어떻게 될까요?
실제 배포를 위한 프로젝트를 작업하며 **application.properties, application-local.properties, application-dev.properties, application-prod.properties** 등 여러게의 프로퍼티를 사용하였습니다.
개발을 하며 로컬 서버에서 구동할 때와 개발서버에서 구동할 때, 실 서버에서 구동할 경우 설정 변수 값이 다르게 적용되는 경우가 많은데, 이럴 때 개발환경에 따라 각각의 프로퍼티를 사용하면 다르게 사용되는 값들을 하나하나 다 바꿔주는 번거로운 일을 하지 않을 수 있습니다.
* 주의할 점으로는 새로 생성하는 properties는 **application-{이름}.properties** 라는 규칙을 따라야 한다는 점입니다.
그렇다면 로컬, 개발, 실서버 각각의 환경에서 환경에 맞는 properties를 불러와서 사용하는 방법을 보겠습니다.

dev 또는 prod 프로퍼티를 사용하고 싶다면

**java -jar -Dspring.profiles.active=prod (어플리케이션 파일명).jar** 명령어를 통해 실행할 수 있습니다.

## @Configuration

@Configuration이라고 하면 설정파일을 만들기 위한 애노테이션 or Bean을 등록하기 위한 애노테이션이다.