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

Spring Data Jpa 적용
dependency 에 
org.springframework.boot:spring-boot-starter-data-jpa
- 스프링 부트용 Spring Data Jpa 추상화 라이브러리이다
- 스프링 부트 버전에 맞춰 자동으로 JPA관련 라이브러리들의 버전을 관리해 준다
com.h2database:h2
- 인메모리 관계형 데이터베이스이다
- 별도 설치 필요 없이 프로젝트 의존성만으로 관리할 수 있다.
- 메모리에서 실행되기 때문에 애플리케이션을 재시작할 때마다 초기화된다는 점을 이용하여 테스트 용도로 많이 사용된다.
- 이 책에서는 JPA의 테스트, 로컬 환경에서의 구동에서 사용할 예정이다.


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

JPA
MyBatis와 같은 SQL 매퍼를 사용해서 개발을 하다보면 실제 개발하는 시간보다 SQL을 다루는 시간이 더 많다.
관계형 데이터베이스를 이용하는 프로젝트에서 객체지향 프로그래밍을 하는 방법은 JPA라는 자바 표준 ORM을 사용하는 것이다.
현재 자사 서비스를 개발하는 곳에서는 SpringBoot & JPA를 전사 표준으로 사용한다. 
관계형 데이터베이스는 어떻게 데이터를 저장할지에 초점이 맞춰진 기술이고, 객체지향 프로그래밍 언어는 메시지를 기반으로
기능과 속성을 한 곳에서 관리하는 기술이다. 이 둘이 패러다임이 서로 다른데, 객체를 데이터베이스에 저장하려고 하니
여러 문제가 발생한다. 이를 패러다임 불일치라고 한다. 이를 중간에서 패러다임 일치를 시켜주기 위한 기술이 JPA이다.
개발자는 객체지향적으로 프로그래밍을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성해서 실행한다.
개발자는 항상 객체지향적으로 코드를 표현할 수 있으니 더는 SQL에 종속적인 개발을 하지 않아도 된다.

Spring Data JPA
JPA는 인터페이스로서 자바 표준명세서이다. 인터페이스인 JPA를 사용하기 위해서는 구현체가 필요하다.
대표적으로 Hibernate, Eclipse Link 등이 있다. 하지만 Spring에서 JPA를 사용할 때는 이 구현체들을 직접 다루진 않는다.
구현체들을 좀 더 쉽게 사용하고자 추상화시킨 Spring Data JPA라는 모듈을 이용해 JPA 기술을 다룬다. 관계를 살펴보면
JPA <- Hibernate <- Spring Data JPA
Hibernate를 쓰는 것과 Spring Data JPA를 쓰는 것 사이에는 큰 차이가 없다. 그럼에도 스프링 진영에서는
Spring Data JPA를 개발했고, 이를 권장하고 있다. Spring Data JPA가 등장한 이유는 크게 두 가지이다.
1. 구현체 교체의 용이성
2. 저장소 교체의 용이성
구현체 교체의 용이성이란 Hibernate외에 다른 구현체로 쉽게 교체하기 위함이다. Hibernate가 언젠가 수명을 다해서
새로운 JPA 구현체가 대세로 떠오를 때, Spring Data JPA를 쓰는 중이라면 아주 쉽게 교체할 수 있다.
Spring Data JPA 내부에서 구현체 매핑을 지원해주기 때문. 실제로 자바의 Redis 클라이언트가 Jedis에서 Lettuce로
대세가 넘어갈 때 Spring Data Redis를 쓰신 분들은 아주 쉽게 교체를 했습니다.
저장소 교체의 용이성이란 관계형 데이터베이스 외에 다른 저장소로 쉽게 교체하기 위함이다.
서비스 초기에는 관계형 데이터베이스로 모든 기능을 처리했지만, 점점 트래픽이 많아져 관계형 데이터베이스로는 도저히 감당이
안 될 때가 올 수 있습니다. 이때 MongoDB로 교체가 필요하다면 개발자는 Spring Data JPA에서
Spring Data MongoDB로 의존성만 교체하면 됩니다. 이는 Spring Data의 하위 프로젝트들은 기본적인 CRUD 인터페이스가
같기 때문이다. 즉 Spring Data JPA, Spring Data Redis, Spring Data MongoDB 등등 Spring Data의 하위 프로젝트들은
save(), findAll(), findOne() 등을 인터페이스로 갖고 있다. 그러다 보니 저장소가 교체되어도 기본적인 기능은
변경할 것이 없다. 이런 장점들로 인해 Hibernate를 직접 쓰기보다 Spring팀에서 계속해서 Spring Data 프로젝트를 
권장하고 있다. 
실무에서 JPA를 사용하지 못하는 가장 큰 이유로 높은 러닝 커브를 이야기한다. 이점은 필자도 동의한다.
JPA를 잘 쓰려면 객체지향 프로그래밍과 관계형 데이터베이스를 둘 다 이해해야 한다. JPA를 사용하면 CRUD 쿼리를
직접 작성할 필요가 없고, 속도에 관해서는 JPA를 잘 활용하면 네이티브 쿼리만큼의 퍼포먼스를 낼 수 있다.

등록/수정/조회 API 만들기
API를 만들기 위해 총 3개의 클래스가 필요하다.
1. Request 데이터를 받을 Dto
2. API 요청을 받을 Controller
3. 트랜잭션, 도메인 기능 간의 순서를 보장하는 Service
여기서 많은 사람들이 오해하는 것이, Service에서 비즈니스 로직을 처리해야 한다는 것이다. 하지만 전혀 그렇지 않다.
Service는 트랜잭션, 도메인 간 순서 보장의 역할만 한다.

Web Layer
- 흔히 사용하는 컨트롤러(@Controller)와 JSP/Freemarker등의 뷰 템플릿 영역이다.
- 이외에도 필터(@Filter), 인터셉터, 컨트롤러 어드바이스(@ControllerAdvice) 등 외부 요청과 응답에 대한 
전반적인 영역을 이야기합니다.

Service Layer
- @Service에 사용되는 서비스 영역이다.
- 일반적으로 Controller와 Dao의 중간 영역에서 사용된다
- @Transactional이 사용되어야 하는 영역이기도 합니다.

Repository Layer
- Database와 같이 데이터 저장소에 접근하는 영역이다.
- 기존에 개발하셨던 분들이라면 Dao(data access object) 영역으로 이해하면 쉽다.

Dtos
- Dto(data transfer object)는 계층 간에 데이터 교환을 위한 객체를 이야기하며 Dtos는 이들의 영역을 이야기한다.
- 예를 들어 뷰 템플릿 엔진에서 사용될 객체나 Repository Layer에서 결과로 넘겨준 객체 등이 이들을 이야기한다.

Domain Model
- 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라고 한다.
- 이를테면 택시 앱이라고 하면 배차, 탑승, 요금 등이 모두 도메인이 될 수 있다.
- @Entity를 사용해본 사람은 @Entity가 사용된 영역 역시 도메인 모델이라고 이해하면 된다.
- 다만, 무조건 데이터베이스의 테이블과 관계가 있어야만 하는 것은 아니다.
- VO처럼 값 객체들도 이 영역에 해당하기 때문이다.

이 5가지 레이어에서 비즈니스 처리를 담당하는 곳이 바로 Domain 레이어이다.
(참고) 기존에 서비스로 처리하던 방식을 트랜잭션 스크립트라고 함.

JPA Auditing으로 생성시간/수정시간 자동화하기
보통 엔티티에는 해당 데이터의 생성시간과 수정시간을 포함한다. 언제 만들어졌는지, 언제 수정되었는지 등은 차후 유지보수에 있어 굉장히 중요한 정보다.
그렇다 보니 매번 DB에 삽입 하기 전, 갱신 하기 전에 날짜 데이터를 등록/수정하는 코드가 여기저기 들어가게 된다. 이것은 매우 반복적이고
귀찮은 문제이기 때문에 이 문제를 해결하고자 JPA Auditing을 사용하겠다.

LocalDate 사용
여기서부터는 날짜 타입을 사용한다. Java8부터 LocalDate와 LocalDateTime이 등장했다. 그간 Java의 기본 날짜 타입인 Date의 문제점을 제대로
고친 타입이라 Java8일 경우 무조건 써야 한다고 생각하면 된다. LocalDate, LocalDateTime은 스프링 부트 1.x버전을 쓴다면 별도로
Hibernate 5.2.10 버전 이상을 사용하도록 설정이 필요하지만, 스프링 부트 2.x버전을 사용하면 기본적으로 해당 버전을 사용 중이라 별다른
설정 없이 바로 적용하면 된다.



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
- @RequestParam -> 외부에서 API로 넘긴 파라미터를 가져오는 어노테이션입니다. 여기서는 외부에서 
name(@RequestParam("name")) 이란 이름으로 넘긴 파라미터를 메서드 파라미터 name(String name)에 저장하게 된다.

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
- param -> API 테스트할 때 사용될 요청 파라미터를 설정한다. 단 값은 String만 허용한다(그래서 String.valueOf사용). 
그래서 숫자/날짜 등의 데이터도 등록할 때는 문자열로 변경해야 가능하다.
- jsonPath -> JSON 응답값을 필드별로 검증할 수 있는 메서드이다. $를 기준으로 필드명을 명시한다.
여기서는 name과 amount를 검증하니 $.name, $.amount로 검증한다.

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

domain 패키지
도메인을 담을 패키지이다. 여기서 도메인이란 게시글, 댓글, 정산, 회원, 결제 등 소프트웨어에 대한 요구사항 혹은 문제 영역이라고
생각하면 된다. 기존 Mybatis와 같은 쿼리 매퍼를 사용했다면 dao 패키지를 떠올리겠지만 dao 패키지와는 조금 결이 다르다.
그간 xml에 쿼리를 담고, 클래스는 오로지 쿼리의 결과만 담던 일들이 모두 도메인 패키지에서 해결된다.

Posts 클래스
필자는 어노테이션 순서를 배치할 때 주요 어노테이션을 클래스에 가까이 둔다. entity는 JPA의 어노테이션이며, getter와
noargsconstructor는 롬복의 어노테이션이다. 롬복은 코드를 단순화시켜 주지만 필수 어노테이션은 아니다. 이렇게 하면
이후에 코틀린 등의 새 언어 전환으로 롬복이 더 이상 필요 없을 경우 쉽게 삭제할 수 있다.
여기서 Posts 클래스는 실제 DB의 테이블과 매칭될 클래스이며 보통 entity 클래스라고 한다. JPA를 사용하면 DB 데이터에
작업할 경우 실제 쿼리를 날리기보다는 이 entity 클래스의 수정을 통해 작업한다.
- @Entity -> 테이블과 링크될 클래스임을 나타냄. 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍으로 테이블 이름을
매칭한다 ex) SalesManager.java -> sales_manager table
- @Id -> 해당 테이블의 PK 필드를 나타낸다.
- @GeneratedValue -> PK의 생성 규칙을 나타낸다. 스프링 부트 2.0에서는 GenerationType.IDENTITY 옵션을 추가해야만
auto_increment가 된다.
- @Column -> 테이블의 칼럼을 나타내며 굳이 선언하지 않아도 해당 클래스의 필드는 모두 칼럼이 된다.
사용하는 이유는 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용한다. 문자열의 경우 VARCHAR(255)가 기본값인데,
사이즈를 500으로 늘리고 싶거나(ex.title), 타입을 TEXT로 변경하고 싶거나(ex.content) 등의 경우에 사용한다.
(참고)
- 웬만하면 Entity의 PK는 Long타입의 Auto_increment를 추천한다(MySQL 기준으로 이렇게 하면 bigint 타입이 된다).
주민등록번호와 같이 비즈니스상 유니크 키나, 여러 키를 조합한 복합키로 PK를 잡을 경우 난감한 상황이 종종 발생한다.
1. FK를 맺을 때 다른 테이블에서 복합키 전부를 갖고 있거나, 중간 테이블을 하나 더 둬야 하는 상황이 발생한다.
2. 인덱스에 좋은 영향을 끼치지 못한다.
3. 유니크한 조건이 변경될 경우 PK 전체를 수정해야 하는 일이 발생한다. 주민등록번호, 복합키 등은 유니크 키로 별도로
추가하는 걸 추천한다.
- @NoArgsConstructor -> 기본 생성자 자동 추가. public Posts() {} 와 같은 효과
- @Getter -> 클래스 내 모든 필드의 Getter 메서드를 자동생성
- @Builder -> 해당 클래스의 빌더 패턴 클래스를 생성. 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함.
(Id 를 빌더에 넣지 않기 위함)
서비스 초기 구축 단계에선 테이블 설계(여기서는 Entity 설계)가 빈번하게 변경되는데, 이때 롬복의 어노테이션들은
코드 변경량을 최소화시켜 주기 때문에 적극적으로 사용한다.
Posts 클래스에는 한 가지 특이점이 있다. 바로 Setter 메서드가 없다는 점이다. 자바빈 규약을 생각하면서
getter/setter 를 무작정 생성하는 경우가 있다. 이렇게 되면 해당 클래스 인스턴스 값들이 언제 어디서 변해야
하는지 코드상으로 명확하게 구분할 수가 없어, 차후 기능 변경 시 정말 복잡해진다.
그래서 Entity 클래스에서는 절대 Setter 메서드를 만들지 않는다. 대신 해당 필드 값 변경이 필요하면 명확히 그 목적과
의도를 나타낼 수 있는 메서드를 추가해야만 한다.
그렇다면 Setter가 없는 상황에서 어떻게 값을 채워 DB에 삽입할까? 기본적인 구조는 생성자를 통해 최종값을 채운 후 DB에
삽입하는 것이며, 값 변경이 필요한 경우 해당 이벤트에 맞는 public 메서드를 호출하여 변경하는 것을 전제로 한다.
여기서는 생성자 대신 @Builder를 통해 제공되는 빌더 클래스를 사용한다. 생성자나 빌더나 생성 시점에 값을 채워주는 역할은
똑같다. 다만 생성자의 경우 지금 채워야 할 필드가 무엇인지 명확히 지정할 수가 없다. 빌더 패턴을 잘 익혀놓으면 좋다.

PostsRepository 인터페이스
보통 MyBatis 에서 Dao라고 불리는 DB Layer 접근자이다. Jpa에선 Repository라고 부르며 인터페이스로 생성한다.
단순히 인터페이스를 생성 후, JpaRepository<Entity 클래스, PK 타입>를 상속하면 기본적인 CRUD 메서드가 자동으로
생성된다. 나중에 프로젝트 규모가 커져 도메인별로 프로젝트를 분리해야 한다면 이때 Entity 클래스와 기본 Repository는
함께 움직여야 하므로 도메인 패키지에서 함께 관리합니다.

PostsRepositoryTest 클래스
- @After -> JUnit에서 단위 테스트가 끝날 때마다 수행되는 메서드를 지정
보통은 배포 전 젠체 테스트를 수행할 때 테스트간 데이터 침범을 막기 위해 사용한다.
여러 테스트가 동시에 수행되면 테스트용 데이터베이스인 H2에 데이터가 그대로 남아 있어 다음 테스트 실행 시 테스트가 실패할
수 있다.
- postsRepository.save -> 테이블 posts에 insert/update 쿼리를 실행한다. id값이 없다면 insert, 있다면
update 쿼리가 실행된다.
- postsRepository.findAll -> 테이블 posts에 있는 모든 데이터를 조회해오는 메서드입니다.

별다른 설정 없이 @SpringBootTest를 사용할 경우 H2 데이터베이스를 자동으로 실행해 준다.

resource에 application.properties파일을 만들고 spring.jpa.show_sql=true 를 추가하고 다시 테스트를 돌리면
sql 쿼리를 확인할 수 있다. 직접 확인해보면 create table 쿼리에서 id bigint generated by default as identity
라는 옵션으로 생성된다. 이는 H2의 쿼리 문법이 적용되었기 때문이다. 

MySQL 쿼리를 수행해도 정상적으로 작동하기 때문에 이후 디버깅을 위해 출력되는 쿼릴 로그를 MySQL 버전으로 변경해 보겠다.
applicaiton.properties에 spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect

PostsApiController 클래스
- @RequestBody란 클라이언트가 전송하는 Http 요청의 Body 내용을 java object로 변환시켜주는 역할을 한다.
그래서 Body가 존재하지 않는 Get 방식의 메서드에 @RequestBody를 활용하는 것은 적합하지 않으므로
에러가 발생한다. 즉 Post방식으로 Json의 형태로 넘어온 데이터를 객체로 바인딩하기 위해 사용할 수 있다.

PostsService 클래스
- @Transactional -> 데이터 삽입 프로세스를 진행하던 중 에러가 발생하면 기존에 삽입이 이미 이루어진 데이터들도 다시 롤백이 되어야 한다.
이러한 점을 감안하여 Spring은 @Transactional 이란 어노테이션을 제공한다. 메서드 앞에 해당 어노테이션을 넣으면
작업 중에 Exception이 발생하면 해당 작업을 모두 롤백 시키며 Exception을 던져준다.
또한 해당 Exception마다 롤백 여부 또한 정해줄 수 있다.
- update 기능에서 데이터베이스에 쿼리를 날리는 부분이 없다. 이게 가능한 이유는 JPA 영속성 때문이다. 영속성 컨텍스트란, 엔티티를 영구 
저장하는 환경이다. 일종의 논리적 개념이고, JPA의 핵심 내용은 엔티티가 영속성 컨텍스트에 포함되어 있냐 아니냐로 갈린다. 


스프링에서는 Bean을 주입받는 방식이 @Autowired, setter, 생성자 세 가지이다. 이 중 가장 권장하는 방식이 생성자로 주입받는 방식이다.
@Autowired는 권장되지 않는다. 생성자를 이용하면 동일한 효과를 볼 수 있다. 바로 @RequiredArgsConstructor에서 해결해 준다.
final이 선언된 모든 필드를 인자값으로 하는 생성자를 롬복의 @RequiredArgsConstructor가 대신 생성해 준다. 생성자를 직접 안 쓰고
롬복 어노테이션을 사용한 이유는 해당 클래스의 의존성 관계가 변경될 때마다 생성자 코드를 계속해서 수정하는 번거로움을 해결하기 위함이다.

PostsSaveRequestDto 클래스
여기서 Entity 클래스와 거의 유사한 형태임에도 Dto클래스를 추가로 생성했다. 하지만, 절대로 Entity 클래스를 Request/Response 클래스로
사용하면 안된다. Entity 클래스는 데이터베이스와 맞닿은 핵심 클래스이다. Entity 클래스를 기준으로 테이블이 생성되고, 스키마가 변경된다.
화면 변경은 사소한 기능 변경인데, 이를 위해 테이블과 연결된 Entity 클래스를 변경하는 건 너무 큰 변경이다. 수많은 서비스 클래스나 비즈니스
로직들이 Entity 클래스를 기준으로 동작한다. Entity클래스가 변경되면 여러 클래스에 영향을 끼치지만 Request와 Response용 Dto는 View를
위한 클래스라 정말 자주 변경이 필요하다. 
View Layer, DB Layer의 역할 분리를 철저히 하는 것이 좋다. 실제로 Controller에서 결과값으로 여러 테이블을 조인해서 줘야 할 경우가
빈번하므로 Entity클래스만으로 표현하기 어려운 경우가 많다. 꼭 Entity 클래스와 Controller에서 쓸 Dto는 분리해서 사용해야 한다.

PostsApiControllerTest 클래스
Api Controller를 테스트하는데 HelloController와 달리 @WebMvcTest를 사용하지 않았다. @WebMvcTest의 경우 JPA 기능이 작동하지 않기
때문인데, Controller와 ControllerAdvice 등 외부 연동과 관련된 부분만 활성화되니 지금같이 JPA 기능까지 한번에 테스트할 때는
@SpringBootTest 와 TestRestTemplate를 사용하면 된다.

PostsResponseDto 클래스
PostsResponseDto는 Entity의 필드 중 일부만 사용하므로 생성자로 Entity를 받아 필드에 값을 넣는다. 굳이 모든 필드를 가진 생성자가 필요하진
않으므로 Dto는 Entit를 받아 처리한다.

BaseTimeEntity 클래스
이 클래스는 모든 Entity의 상위 클래스가 되어 Entity들의 createdDate, modifiedDate를 자동으로 관리하는 역할이다.
- @MappedSuperclass -> JPA Entity 클래스들이 BaseTimeEntity를 상속할 경우 필드들(createdDate, modifiedDate)도 칼럼으로 인식하도록 한다.
- @EntityListeners(AuditingEntityListener.class) -> BaseTimeEntity 클래스에 Auditing 기능을 포함시킵니다.
- @CreatedDate -> Entity가 생성되어 저장될 때 시간이 자동 저장된다.
- @LastModifiedDate -> 조회한 Entity의 값을 변경할 때 시간이 자동 저장된다.
이 클래스 작성 후, Posts 클래스가 BaseTimeEntity를 상속받도록 변경한다. 그 후 Application 클래스에 활성화 어노테이션을 추가하여
JPA Auditing 어노테이션을 모두 활성화할 수 있도록 한다.

                    


참조 
- [https://www.redhat.com/ko/topics/api/what-is-a-rest-api]
- [스프링 부트와 AWS로 혼자 구현하는 웹 서비스 저자:이동욱]
- [https://inseok9068.github.io/springboot/springboot-transaction/]

