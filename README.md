# SpringBoot-Projects

SpringBoot 프로젝트 만들기 순서
1. springboot-projects 폴더 안에 프로젝트 이름으로 폴더 생성
2. intellij에서 해당 폴더로 프로젝트 생성
3. springboot-projects 폴더에서 git init 
4. README.md 또한 springboot-projects 폴더에 추가
5. intellij에서 springboot-projects 열어서 initial commit, push
6. 이후로는 springboot-projects나 내부 프로젝트 폴더 둘다 commit, push 가능

프로젝트 생성 이후 순서
1. 그레이들 프로젝트를 스프링 부트 프로젝트로 변경하기
- 스프링 이니셜라이저 사용하지 않겠음. 사용하게 되면 build.gradle의 코드가 무슨
역할을 하는지, 이니셜라이저 외에 의존성이 추가로 필요하면 어떻게 해야 할지 등을 
모르는 상태로 개발하는 경우가 있기 때문

2. build.gradle 작성
- buildscript와 내부 코드에서 ext는 build.gradle에서 사용하는 전역변수를 설정하겠다는 의미,
여기서는 springBootVersion 전역변수를 생성하고 그 값을 '2.1.7.RELEASE'로 하겠다는 의미.
즉 spring-boot-gradle-plugin 라는 스프링 부트 그레이들 플러그인의 2.1.7.RELEASE를 의존성으로
받겠다는 의미.
- apply plugin 은 앞서 선언한 플러그인 의존성들을 적용할 것인지를 결정하는 코드.
특히 io.spring.dependency-management 플러그인은 스프링 부트의 의존성을 관리해주는 
플러그인이라 꼭 추가해야 한다. 앞 4개 플러그인은 자바, 스프링 부트를 사용하기 위한 필수 
플러그인이니 항상 추가하면 됨.
- repository는 각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지를 정합니다.
기본적으로 mavenCentral을 많이 사용하지만, 최근에는 라이브러리 업로드 난이도 때문에 
jcenter도 많이 사용.
mavencentral은 본인이 만든 라이브러리를 업로드하기 위해서는 많은 과정과 설정이 필요.
최근에 나온 jcenter는 이런 문제점을 개서나여 라이브러리 업로드를 간단하게 했음.
더욱이 jcenter에 라이브러리를 업로드하면 mavencentral에도 업로드될 수 있도록 자동화
할 수 있음. 여기서는 둘 다 등록해서 사용하겠음.
- dependencies는 프로젝트 개발에 필요한 의존성들을 선언하는 곳임. 여기서는
org.springframework.boot:spring-boot-starter-web와 
org.springframework.boot:spring-boot-starter-test를 받도록 선언되어 있습니다.
인텔리제이는 메이븐 저장소의 데이터를 인덱싱해서 관리하기 때문에 커뮤니티 버전을 사용해도
의존성 자동완성이 가능합니다. 
dependency 의존성 코드의 경우 특정 버전을 명시하면 안됨. 버전을 명시하지 않아야만 맨 위에 작성한
org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}의 버전을 따라감.
- 이렇게 관리할 경우 라이브러리 버전 관리가 한 곳에서 집중되고, 버전 충돌도 해결되어 편하게 개발할 수 있음.

깃 연동 방법
1. ctrl shift a 하면 repository와 연결시킬 수 있지만 그냥 위의 방식으로 repository에 연결
2. ctrl k 하면 커밋할 수 있다.
3. ctrl shift k 하면 푸시할 수 있다.

테스트 코드
TDD와 단위 테스트(unit test)는 다른 이야기다. TDD는 테스트가 주도하는 개발을 이야기한다.
테스트 코드를 먼저 작성하는 것부터 시작한다.
TDD는 레드-그린 사이클이라 하여 레드는 항상 실패하는 테스트를 작성, 그린은 테스트가 통과하는
프로덕션 코드를 작성, 테스트를 통과하면 프로덕션 코드를 리팩토링한다.
단위 테스트는 TDD의 첫 번째 단계인 기능 단위의 테스트 코드를 작성하는 것을 이야기함.
TDD와 달리 테스트 코드를 꼭 먼저 작성해야 하는 것도 아니고, 리팩토링도 포함되지 않는다.
순수하게 테스트 코드를 작성하는 것을 이야기한다. 이 책에서는 TDD가 아닌 단위 테스트 코드를 배운다.
우리는 자바용 테스트코드 프레임워크인 JUnit을 사용할 것임. 많은 회사에서 JUnit4를 사용하고 있기에
여기서도 4버전으로 진행.

테스트 코드의 이점
- 단위 테스트는 개발 단계 초기에 문제를 발견하게 도와줌
- 단위 테스트는 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이
올바르게 작동하는지 확인할 수 있다.
- 단위 테스트는 기능에 대한 불확실성을 감소시킬 수 있다.
- 단위 테스트는 시스템에 대한 실제 문서를 제공한다. 즉 단위 테스트 자체가 문서로 사용될 수 있음.

롬복
- 자바 개발자들의 필수 라이브러리이며 Getter, Setter, 기본생성자, toString 등을 어노테이션으로 자동생성해줌
- 1. build.gradle에 compile('org.projectlombok:lombok') 을 추가한다.
2. ctrl shift a 에서 plugins로 들어가 lombok 버튼을 클릭하여 설치한다.
3. 인텔리제이를 재시작하고 나서 setting > build > compiler > annotation processors 들어간 후
enable annotation processing을 체크한다.
4. (참고) 롬복은 프로젝트마다 설정해야 한다. 플러그인 설치는 한 번만 하면 되지만, build.gradle에
라이브러리를 추가하는 것과 Enable annotation processing를 체크하는 것은 프로젝트마다 진행해야 한다. 


클래스

Application 클래스
- @SpringBootApplication -> 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성을 모두 자동으로 설정
특히나 @SpringBootApplication이 있는 위치부터 설정을 읽어가기 때문에 이 클래스는 항상 프로젝트 최상단에 위치해야 함
- main 메서드에서 실행하는 SpringApplication.run으로 인해 내장 WAS를 실행한다. 내장 WAS가 있으면 외부 톰캣을 설치할 필요가 없고,
스프링 부트로 만들어진 Jar파일(실행 가능한 java 패키징 파일)로 실행하면 된다. 스프링 부트에서는 언제 어디서나 같은 환경에서 
스프링 부트를 배포하기 위하여 내장 WAS를 사용하는 것을 권장함.

HelloController 클래스
- @RestController -> 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 준다. 예전에는 @ResponseBody를 각 메서드마다 선언했던 것을
한번에 사용할 수 있게 해준다고 생각하면 됨.
- @GetMapping -> HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어 준다. 예전에는 @RequestMapping(method = RequestMethod.GET)
으로 사용되었습니다. 이제 이 프로젝트는 /hello로 요청이 오면 문자열 hello를 반환하는 기능을 가짐.

HelloControllerTest 클래스
- @RunWith(SpringRunner.class) -> 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킨다.
여기서는 SpringRunner라는 스프링 실행자를 사용한다. 즉 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 한다.
- @WebMvcTest -> 여러 스프링 어노테이션 중, Web(Spring MVC)에 집중할 수 있는 어노테이션이다.
선언할 경우 @Controller, @ControllerAdvice 등을 사용할 수 있지만, @Service, @Component, @Repository 등은 사용불가.
여기서는 컨트롤러만 사용하기 때문에 선언함.
- @Autowired -> 스프링이 관리하는 빈(Bean)을 주입 받습니다.
- private MockMvc mvc -> 웹 API를 테스트할 때 사용한다. 스프링 MVC 테스트의 시작점이다. 
이 클래스를 통해 HTTP GET, POST 등에 대한 API 테스트를 할 수 있습니다.
- mvc.perform(get("/hello")) -> MockMvc를 통해 /hello 주소로 HTTP GET 요청을 한다.
체이닝이 지원되어 아래와 같이 여러 검증 기능을 이어서 선언할 수 있습니다.
- andExpect(status().isOk()) -> mvc.perform 의 결과를 검증한다. HTTP Header의 Status를 검증한다.
우리가 흔히 알고 있는 200, 404, 500 등의 상태를 검증합니다. 여기서는 Ok 즉, 200인지 아닌지를 검증합니다.
- andExpect(content().string(hello)) -> mvc.perform의 결과를 검증한다. 응답 본문의 내용을 검증한다.
Controller에서 "hello"를 리턴하기 때문에 이 값이 맞는지 검증합니다.

HelloResponseDto 클래스
- @Getter -> 선언된 모든 필드의 get메서드를 생성해 준다
- @RequiredArgsConstructor -> 선언된 모든 final 필드가 포함된 생성자를 생성한다
final이 없는 필드는 생성자에 포함되지 않는다.

HelloResponseDtoTest 클래스
- assertThat -> assertj라는 테스트 검증 라이브러리의 검증 메서드이다. 검증하고 싶은 대상을 메서드 인자로 받는다.
메서드 체이닝이 지원되어 isEqualTo와 같이 메서드를 이어서 사용할 수 있다.
-isEqualTo -> assertj의 동등 비교 메서드이다. assertThat에 있는 값과 isEqualTo의 값을 비교해서 같을 때만 성공.
여기서 JUnit의 기본 assertThat이 아닌 assertj의 assertThat을 사용했다. assertj 역시 JUnit에서 자동으로
라이브러리 등록을 해준다. JUnit과 비교하여 assertj의 장점은 다음과 같다.
1. CoreMatchers와 달리 추가적으로 라이브러리가 필요하지 않다. -> JUnit의 assertThat을 쓰게 되면 is()와 같이
CoreMatchers 라이브러리가 필요하다.
2. 자동완성이 좀 더 확실하게 지원된다 -> IDE 에서는 CoreMatchers와 같은 Matcher 라이브러리의 자동완성 지원이 약하다.
- 만약 테스트가 실패한다면 그레이들 버전확인이 필요하다. 위 프로젝트는 현업에서 아직 많이 사용중인 그레이들 4로 진행되었다.
테스트 실패 원인과 해결 방법은 (http://bit.ly/382Q7d7) 에서 확인 가능하며 터미널에 
gradlew wrapper --gradle-version 4.10.2 을 입력하여 버전을 내려도 된다.





정보

REST API
REST는 Representational State Transfer의 줄임말. REST API란 REST 아키텍처의 제약 조건을 준수하는 애플리케이션
프로그래밍 인터페이스를 뜻함. 
API란 애플리케이션 소프트웨어를 구축하고 통합하는 정의 및 프로토콜 세트. API는 정보 제공자와 정보 사용자 간의 계약으로 지칭되며 소비자에게 필요한 콘텐츠(호출)와 생산자에게 필요한 콘텐츠(응답)를 구성한다. 예를 들어 날씨 서비스용 API에서는 사용자는 우편번호 제공하고 생산자는 두 부분(첫 번째는 최고 기온, 두 번째는 최저 기온)으로 구성된 응답으로 답하도록 지정할 수 있다.
컴퓨터나 시스템과 상호 작용해 정보 검색하거나 기능 수행하고자 할 때 API는 사용자가 원하는 것을 시스템에 전달할 수 있게 지원하여 시스템이 이 요청을 이해하고 이행하도록 할 수 있다.
API의 또 다른 이점은 리소스 검색 방법 또는 리소스 출처에 대한 지식 없이도 사용이 가능하다는 점

REST(RESTful)란?
REST는 프로토콜이나 표준이 아닌 아키텍처 원칙 세트이다. API 개발자는 REST를 다양한 방식으로 구현할 수 있다
RESTful API를 통해 요청이 수행될 때 RESTful API는 리소스 상태에 대한 표현을 요청자에게 전송합니다.
이 정보 또는 표현은 HTTP:JSON(Javascript Object Notation), HTML XLT 또는 일반 텍스트를 통해 몇 가지 형식으로 전송됨. JSON은 그 이름에도 불구하고 사용 언어와 상관이 없고 인간과 머신이 모두 읽을 수 있기에 널리 사용됨.

API가 RESTful로 간주되려면 다음 기준을 따라야 함
- 클라이언트, 서버 및 리소스로 구성되었으며 요청이 HTTP를 통해 관리되는 클라이언트-서버 아키텍처
- 스테이스리스(stateless) 클라이언트-서버 커뮤니케이션 : 요청 간에 클라이언트 정보가 저장되지 않으며, 
  각 요청이 분리되어 있고 서로 연결되어 있지 않음
- 클라이언트-서버 상호 작용을 간소화하는 캐시 가능 데이터
- 요청된 정보를 검색하는 데 관련된 서버(보안, 로드 밸런싱 등을 담당)의 각 유형을 클라이언트가 볼 수 없는
  계층 구조로 체계화하는 계층화된 시스템.
- 코드 온디맨드(선택 사항) - 요청을 받으면 서버에서 클라이언트로 실행 가능한 코드를 전송하여 클라이언트 기능 
  을 확장할 수 있는 기능.
  
이처럼 REST API는 따라야 할 기준이 있지만 속도를 저하시키고 더 무겁게 만드는 XML메시징, 빌트인 보안 및 트랜잭션 컴플라이언스처럼 특정 요구 사항이 있는 SOAP(simple object access protocol)등의 규정된 프로토콜보다 사용하기 쉬운 것으로 간주됩니다.

이와 대조적으로 REST는 필요에 따라 구현할 수 있는 일련의 지침으로, 이를 통해 REST API는 더 빨라지고 경량화되며 사물인터넷(Iot) 및 모바일 앱 개발에 가장 적합한 API가 된다

참조 
- [https://www.redhat.com/ko/topics/api/what-is-a-rest-api]
- [스프링 부트와 AWS로 혼자 구현하는 웹 서비스 저자:이동욱]

