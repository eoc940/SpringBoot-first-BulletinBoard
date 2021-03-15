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
spring-boot-starter-oauth2-client -> 소셜 로그인 등 클라이언트 입장에서 소셜 기능 구현시 필요한 의존성이다.
spring-security-oauth2-client와 spring-security-oauth2-jose를 기본으로 관리해준다.


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

텝플릿 엔진
웹 개발에 있어 템플릿 엔진이란, 지정된 템플릿 양식과 데이터가 합쳐져 html 문서를 출력하는 소프트웨어를 이야기한다.
예전에 스프링이나 서블릿을 사용해 본 독자들은 아마도 jsp, freemarker등을 떠올리실 테고, 요즘 개발을 시작한 독자들은
리액트, 뷰의 view 파일들을 떠올릴 것이다. 둘 모두 결과적으로 지정된 템플릿과 데이터를 이용하여 html을 생성하는 템플릿 엔진이다.
하지만 차이가 있는데 전자는 서버 템플릿 엔진이라 불리며, 후자는 클라이언트 템플릿 엔진이라 불린다. 개발을 시작하는 많은 개발자들이
이 둘 간에 많은 오해를 한다. 
(책 126페이지 코드 참조)
서버 템플릿 엔진을 이용한 화면 생성은 서버에서 java코드로 문자열을 만든 뒤 이 문자열을 html로 변환하여 브라우저로 전달한다.
앞선 코드는 html을 만드는 과정에서 System.out.println("test"); 를 실행할 뿐이며, 이때의 자바스크립트 코드는 단순한 문자열일 뿐입니다.
반면에 자바스크립트는 브라우저 위에서 작동한다. 앞에서 작성된 자바스크립트 코드가 실행되는 장소는 서버가 아닌 브라우저다. 
즉, 브라우저에서 작동될 때는 서버 템플릿 엔진의 손을 벗어나 제어할 수가 없다. 흔히 이야기하는 vue.js나, react.js를 이용한 
SPA(single page application)는 브라우저에서 화면을 생성한다. 그래서 다음과 같이 서버에서는 json혹은 xml형식의 데이터만 전달하고
클라이언트에서 조립한다. 물론 최근엔 리액트나 뷰와 같은 자바스크립트 프레임워크에서 서버사이드 렌더링을 지원하는 모습을 볼 수 있지만,
여기까지 이야기하기에는 책의 범위를 뛰어넘는다. 대신 간단히 설명하면, 자바스크립트 프레임워크의 화면 생성 방식을 서버에서 실행하는
것을 이야기한다. 이는 V8엔진 라이브러리들이 지원하기 때문이며, 스프링 부트에서 사용할 수 있는 대표적인 기술로는 Nashorn, J2V8이 있다.

머스테치
머스테치는 수많은 언어를 지원하는 가장 심플한 템플릿 엔진이다. 루비, 자바스크립트, 파이썬, php,자바, 펄, go, asp 등 현존하는 대부분의
언어를 지원하고 있다. 그러다 보니 자바에서 사용될 때는 서버 템플릿 엔진으로, 자바스크립트에서 사용될 때는 클라이언트 템플릿 엔진으로
모두 사용할 수 있다. 자바 진영에서는 jsp, velocity, freemarker, thymeleaf등 다양한 서버 템플릿 엔진이 존재한다.
필자가 생각하는 템플릿 엔진들의 단점은 다음과 같다.
jsp, velocity - 스프링 부트에서는 권장하지 않는 템플릿 엔진이다.
freemarker - 템플릿 엔진으로는 너무 과하게 많은 기능을 지원한다. 높은 자유도로 인해 숙련도가 낮을수록 freemarker 안에 비즈니스 로직이
추가될 확률이 높다
thymeleaf - 스프링 진영에서 적극적으로 밀고 있지만 문법이 어렵다. html태그에 속성으로 템플릿 기능을 사용하는 방식이 기존 개발자에게는
높은 허들로 느껴지는 경우가 많다. 실제로 사용해 본 분들은 자바스크립트 프레임워크를 배우는 기분이라고 후기를 이야기하기도 한다. 물론
vue.js를 사용해 본 경험이 있어 태그 속성 방식이 익숙하다면 thymeleaf를 선택해도 된다.
머스테치의 장점 - 문법이 다른 템플릿 엔진보다 심플하다, 로직 코드를 사용할 수 없어 view의 역할과 서버의 역할이 명확하게 분리된다.
mustache.js와 mustache.java 2가지가 다 있어, 하나의 문법으로 클라이언트/서버 템플릿을 모두 사용 가능하다.

개인적으로 템플릿 엔진은 화면 역할에만 충실해야 한다고 생각한다. 너무 많은 기능을 제공하면 api와 템플릿 엔진, 자바스크립트가 서로 로직을 나눠 
갖게 되어 유지보수하기 상당히 어렵다.

머스테치의 장점이 하나 더 있는데, 인텔리제이 커뮤니티 버전을 사용해도 플러그인을 사용할 수 있다는 것이다. 이 플러그인을 사용하면
머스테치의 문법 체크, html문법 지원, 자동완성 등이 지원되니 개발할 때 큰 도움을 받을 수 있다.

게시글 등록 화면 만들기
이번에는 게시글 등록 화면을 구현할 것이다. 앞서 3장에서 PostsApiController로 api를 구현하였으니 여기선 바로 화면을 개발한다.
아무래도 그냥 html만 사용하기엔 멋이 없다. 그래서 오픈소스인 부트스트랩을 이용하여 화면을 만들 것이다.
부트스트랩, 제이쿼리 등 프론트엔드 라이브러리를 사용할 수 있는 방법은 크게 2가지가 있다. 하나는 외부 cdn을 사용하는 것이고
다른 하나는 직접 라이브러리를 받아서 사용하는 방법이다. 여기서는 전자인 외부 cdn을 사용한다. 본인의 프로젝트에서 직접 내려받아 
사용할 필요도 없고, 사용 방법도 html/jsp/mustache에 코드 한 줄 추가하면 되니 굉장히 간단하다. 그러나 실제 서비스에서는
이 방법을 잘 사용하지 않는다. 결국은 외부 서비스에 우리 서비스가 의존하게 되버려서, cdn을 서비스하는 곳에 문제가 생기면
덩달아 같이 문제가 생기기 때문이다. 2개의 라이브러리 부트스트랩과 제이쿼리를 index.mustache에 추가해야 한다. 하지만 여기서는
바로 추가하지 않고 레이아웃 방식으로 추가할 것이다. 레이아웃 방식이란 공통 영역을 별도의 파일로 분리하여 필요한 곳에서 가져다 쓰는 
방식을 이야기한다. 부트스트랩과 제이쿼리는 머스테치 화면 어디서나 필요하다. 매번 해당 라이브러리를 머스테치 파일에 추가하는 것은
귀찮은 일이니, 레이아웃 파일들을 만들어 추가합니다.

브라우저의 스코프
만약 index.mustache에서 a.js가 추가되어 a.js도 a.js만의 init과 save function이 있다면 어떻게 될까요?
브라우저의 스코프는 공용 공간으로 쓰이기 때문에 나중에 로딩된 js의 init, save가 먼저 로딩된 js의 function을 덮어쓰게 된다.
여러 사람이 참여하는 프로젝트에서 중복된 함수 이름은 자주 발생할 수 있다. 모든 function이름을 확인하면서 만들 수는 없다.
그러다 보니 이런 문제를 피하려고 index.js만의 유효범위를 만들어 사용한다. 방법은 var index이란 객체를 만들어 해당 객체에서
필요한 모든 function을 선언하는 것이다. 이렇게 하면 index 객체 안에서만 function이 유효하기 때문에 다른 js와 겹칠 위험이 사라진다.
이런 식의 프론트엔드 의존성 관리, 스코프 관리 등의 문제들로 최근에는 자바스크립트 개발 환경이 급변했습니다. ES6를 비롯한 최신
자바스크립트 버전이나 앵귤러, 리액트, 뷰 등은 이미 이런 기능을 프레임워크 레벨에서 지원한다.

스프링 시큐리티
스프링 시큐리티는 막강한 인증과 인가 기능을 가진 프레임워크이다. 사실상 스프링 기반의 애플리케이션에서는 보안을 위한 표준이라고 보면 된다.
인터셉터, 필터 기반의 보안 기능 구현보다 스프링 시큐리티를 통해 구현하는 것을 적극 권장하고 있다. 
스프링의 대부분 프로젝트들(Mvc, Data, Batch 등등)처럼 확장성을 고려한 프레임워크다 보니 다양한 요구사항을 손쉽게 추가하고 변경할 수 있다.
OAuth 로그인 구현 시 로그인 시 보안, 회원가입 시 이메일 혹은 전화번호 인증, 비밀번호 찾기, 비밀번호 변경, 회원정보 변경과 같은 것들을 
모두 구글, 페이스북, 네이버 등에 맡기면 되니 서비스 개발에 집중할 수 있다.
이 책에서는 기존에 쓰던 1.5설정이 아닌 스프링 부트 2 방식인 Spring Security Oauth2 Client 라이브러리를 사용해서 진행한다.
그 이유는 1. 스프링 팀에서 기존 1.5에서 사용되던 spring-security-oauth 프로젝트는 유지 상태로 결정했으며 더 이상 신규 기능은 
추가하지 않고 버그 수정 정도의 기능만 추가될 예정, 신규 기능은 새 oauth2 라이브러리에서만 지원하겠다고 선언.
2. 스프링 부트용 라이브러리(starter) 출시  3. 기존에 사용되던 방식은 확장 포인트가 적절하게 오픈되어 있지 않아 직접 상속하거나
오버라이딩 해야 하고 신규 라이브러리의 경우 확장 포인트를 고려해서 설계된 상태.
그리고 이 책 이외에 스프링 부트 2 방식의 자료를 찾고 싶은 경우 인터넷 자료들 사이에서 두 가지를 확인하면 된다. 
먼저 spring-security-oauth2-autoconfigure 라이브러리를 썼는지를 확인하고 application.properties혹은 application.yml 정보를 확인해야 한다.
스프링 부트 1.5방식에서는 url주소를 모두 명시해야 하지만, 2.0방식에서는 client인증 정보만 입력하면 된다. 1.5버전에서 직접 입력했던
값들은 2.0버전으로 오면서 모두 enum으로 대체되었다. CommonOAuth2Provider라는 enum이 새롭게 추가되어 구글, 깃허브, 페이스북, 옥타의
기본 설정값은 모두 여기서 제공한다.
resource에 application-oauth.properties파일을 만들고 코드를 등록한다. 여기서 scope=profile,email 로 등록하였다.
많은 예제에서는 이 scope를 별도로 등록하고 있지 않다. 기본값이 openid, profile, email이기 때문이다.
강제로 profile,email을 등록한 이유는 openid라는 scope가 있으면 open id provider로 인식하기 때문이다. 
이렇게 되면 openid provider인 서비스(구글)와 그렇지 않은 서비스(네이버/카카오 등)로 나눠서 각각 OAuth2Service를 만들어야 한다.
하나의 OAuth2Service로 사용하기 위해 일부러 openid scope를 빼고 등록한다.

세션 저장소로 데이터베이스 사용하기
지금 만든 서비스는 애플리케이션을 재실행하면 로그인이 풀린다. 이는 세션이 내장 톰캣의 메모리에 저장되기 때문이다. 기본적으로 세션은
실행되는 WAS의 메모리에서 저장되고 호출된다. 메모리에 저장되다 보니 내장 톰캣처럼 애플리케이션 실행 시 실행되는 구조엔선 항상 초기화된다.
즉 배포할 때마다 톰캣이 재시작된다. 한 가지 문제가 더 있는데, 2대 이상의 서버에서 서비스하고 있다면 톰캣마다 세션 동기화 설정을 해야한다.
그래서 현업에서는 세션 저장소에 대해 다음의 3가지 중 한가지를 선택한다
1. 톰캣 세션을 사용한다
- 일반적으로 별다른 설정을 하지 않을 때 기본적으로 선택되는 방식이다. 이렇게 될 경우 톰캣(WAS)에 세션이 저장되기 때문에 2대 이상의 WAS가
구동되는 환경에서는 톰캣들 간의 세션 공유를 위한 추가 설정이 필요하다.
2. MySQL과 같은 데이터베이스를 세션 저장소로 사용한다.
- 여러 WAS간의 공용 세션을 사용할 수 있는 가장 쉬운 방법이다. 많은 설정이 필요 없지만, 결국 로그인 요청마다 DB IO가 발생하여 성능상 이슈가
발생할 수 있다. 보통 로그인 요청이 많이 없는 백오피스, 사내 시스템 용도에서 사용한다.
3. Redis, Memcached와 같은 메모리 DB를 세션 저장소로 사용한다. 
- B2C 서비스에서 가장 많이 사용하는 방식이다. 실제 서비스로 사용하기 위해서는 Embedded Redis와 같은 방식이 아닌 외부 메모리 서버가 필요하다.

여기서는 두 번째 방식인 데이터베이스를 세션 저장소로 사용하는 방식을 선택하여 진행하겠다. 이유는 설정이 간단하고 사용자가 많은 서비스가 아니며
비용 절감을 하기 위해서다. 이후 AWS에서 이 서비스를 배포하고 운영할 때를 생각하면 레디스와 같은 메모리DB를 사용하기는 부담스럽다. 왜냐하면
레디스와 같은 서비스(엘라스틱 캐시)에 별도로 사용료를 지불해야 하기 때문이다. 먼저 build.gradle에 의존성(spring-session-jdbc)을 등록한다.
(202페이지). 이렇게 세션 저장소를 데이터베이스로 교체했다. 물론 지금은 기존과 동일하게 스프링을 재시작하면 세션이 풀린다. 이유는 H2 기반으로
스프링이 재실행될 때 H2도 재시작되기 때문이다. 이후 AWS로 배포하게 되면 AWS의 데이터베이스 서비스인 RDS(Relational Database Service)를
사용하게 되니 이때부터는 세션이 풀리지 않는다.  


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
SpringDataJpa에서 제공하지 않는 메서드는 위처럼 쿼리로 작성해도 되는 것을 보여드리고자 @Query를 사용했습니다.
실제로 앞의 코드는 SpringDataJpa에서 제공하는 기본 메서드만으로 해결할 수 있지만, @Query가 훨씬 가독성이 좋으니
선택해서 사용하면 된다.(규모가 있는 프로젝트에서는 Querydsl을 쓰자)

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

IndexController 클래스
머스테치 스타터 덕분에 컨트롤러에서 문자열을 반환할 대 앞의 경로와 뒤의 파일 확장자는 자동으로 지정된다. 앞의 경로는 src/main/resources/templates로,
뒤의 파일 확장자는 .mustache가 붙는 것입니다. 즉 여기선 "index"를 반환하므로, src/main/resources/templates/index.mustache로 전환되어
view resolver가 처리하게 됩니다. 
- Model -> 서버 템플릿 엔진에서 사용할 수 있는 객체를 저장할 수 있다. 여기서는 postsService.findAllDesc()로 가져온 결과를 
posts로 index.mustache에 전달한다.
- (SessionUser) httpSession.getAttribute("user") -> 앞서 작성된 CustomOAuth2UserService에서 로그인 성공 시 세션에
SessionUser를 저장하도록 구성했다. 즉 로그인 성공 시 httpSession.getAttribute("user")에서 값을 가져올 수 있다.
- if(user != null) -> 세션에 저장된 값이 있을 때만 model에 userName으로 등록한다. 세션에 저장된 값이 없으면 model엔 
아무런 값이 없는 상태이니 로그인 버튼이 보이게 된다.
- @LoginUser SessionUser user -> 기존에 (User) httpSession.getAttribute("user")로 가져오던 세션 정보 값이 개선되었다.
이제는 어느 컨트롤러든지 @LoginUser만 사용하면 세션 정보를 가져올 수 있게 되었다.

header.mustache, footer.mustache 
코드를 보면 css와 js의 위치가 서로 다르다. 페이지 로딩속도를 높이기 위해 css는 header에, js는 footer에 두었다. html은 위에서부터
코드가 실행되기 때문에 head가 다 실행되고서야 body가 실행된다. 즉 head가 다 불러지지 않으면 사용자 쪽에선 백지 화면만 노출된다.
특히 js의 용량이 크면 클수록 body부분의 실행이 늦어지기 때문에 js는 body 하단에 두어 화면이 다 그려진 뒤에 호출하는 게 좋다.
반면 css는 화면을 그리는 역할이므로 head에서 불러오는 것이 좋다. 그렇지 않으면 css가 적용되지 않은 깨진 화면을 사용자가 볼 수 있기 때문이다.
추가로 bootstrap.js의 경우 제이쿼리가 꼭 있어야만 하기 때문에 부트스트랩보다 먼저 호출되도록 코드를 작성했다. 보통 앞선 상황을
bootstrap.js가 제이쿼리에 의존한다고 한다. 라이브러리를 비롯해 기타 html 태그들이 모두 레이아웃에 추가되니 이제 index.mustache에는
필요한 코드만 남게 됩니다. index.mustache의 코드는 다음과 같이 변경된다.
{{>layout/header}} -------> {{> }}는 현재 머스테치 파일(index.mustache)을 기준으로 다른 파일을 가져온다.
<h1>스프링 부트로 시작하는 웹 서비스</h1>
{{>layout/footer}}

index.mustache
- {{#userName}} -> 머스테치는 다른 언어와 같은 if문(if userName != null)을 제공하지 않는다. true/false 여부만 판단한다.
그래서 머스테치에는 항상 최종값을 넘겨줘야 한다. 여기서도 역시 userName이 있다면 userName을 노출시키도록 구성했다. 
- a href="/logout" -> 스프링 시큐리티에서 기본적으로 제공하는 로그아웃 URL이다. 즉 개발자가 별도로 저 URL에 해당하는 컨트롤러를
만들 필요가 없다. SecurityConfig클래스에서 URL을 변경할 순 있지만 기본 URL을 사용해도 충분하니 여기서는 그대로 사용한다.
- {{^userName}} -> 머스테치에서 해당 값이 존재하지 않는 경우에는 ^를 사용한다. 여기서는 userName이 없다면 로그인 버튼을 
노출시키도록 구성했다. 
- a href="/oauth2/authorization/google" -> 스프링 시큐리티에서 기본적으로 제공하는 로그인 URL이다. 로그아웃 URL과 마찬가지로
개발자가 별도의 컨트롤러를 생성할 필요가 없다.
                    
index.js
- window.location.href = '/' -> 글 등록이 성공하면 메인페이지(/)로 이동한다.
생성된 index.js를 머스테치 파일이 쓸 수 있게 footer.mustache에 추가하겠다.
src="/js/app/index.js" 로 설정하였다. footer.mustache에서 index.js 호출 코드를 보면
절대경로(/)로 바로 시작한다. 스프링 부트는 기본적으로 src/main/resource/static에 위치한 자바스크립트, css, 이미지 등
정적 파일들은 url에서 /로 설정된다. 그래서 다음과 같이 파일이 위치하면 위치에 맞게 호출이 가능하다.
- {{#posts}} -> posts라는 리스트를 순회한다, Java의 for문과 동일하게 생각하면 된다
- {{id}} 등의 {{변수명}} -> list에서 뽑아낸 객체의 필드를 사용한다.
- $('#btn-update').on('click') -> btn-update란 id를 가진 html엘리먼트에 click 이벤트가 발생할 때 update function을
  실행하도록 이벤트를 등록한다
- update : function() -> 신규로 추가될 update function입니다.
- type: 'PUT' -> 여러 http method중 put메서드를 선택한다. PostsApiController에 있는 API에서 이미 @PutMapping으로 
  선언했기 때문에 PUT을 사용해야 합니다. 참고로 이는 REST 규약에 맞게 설정된 것입니다. 참고로 이는 REST 규약에 맞게 설정된 것이다.
  REST에서 CRUD는 다음과 같이 HTTP Method에 매핑된다. 생성(Create) - POST, 읽기(Read) - GET, 수정(Update) - PUT, 
  삭제(Delete) - DELETE
- url: '/api/v1/posts/'+id -> 어느 게시글을 수정할지 URL Path로 구분하기 위해 Path에 id를 추가한다.
posts-update.mustache
- {{post.id}} -> 머스테치는 객체의 필드 접근 시 점으로 구분합니다. 즉 Post 클래스의 id에 대한 접근은 post.id로 사용할 수 있다.
- readonly -> input태그에 읽기 가능만 허용하는 속성이다. id와 author는 수정할 수 없도록 읽기만 허용하도록 추가한다.

Role
- @Enumerated(EnumType.STRING) -> JPA로 데이터베이스로 저장할 때 Enum 값을 어떤 형태로 저장할지를 결정한다.
기본적으로 int로 된 숫자가 저장된다. 숫자로 저장되면 데이터베이스로 확인할 때 그 값이 무슨 코드를 의미하는지 알 수 없다.
그래서 문자열 (EnumType.STRING)로 저장될 수 있도록 선언한다.

UserRepository
- findByEmail -> 소셜 로그인으로 반환되는 값 중 email을 통해 이미 생성된 사용자인지 처음 가입하는 사용자인지 판단하기 위한 메서드이다.

SecurityConfig
- @EnableWebSecurity -> Spring Security 설정들을 활성화시켜 준다
- csrf().disable().headers().frameOptions().disable() -> h2-console 화면을 사용하기 위해 해당 옵션들은 disable한다
- authorizeRequests -> URL별 권한 관리를 설정하는 옵션의 시작점이다. ,authorizeRequests가 선언되어야만 antMatchers 옵션을
사용할 수 있다.
- antMatchers -> 권한 관리 대상을 지정하는 옵션이다. URL, HTTP 메서드별로 관리가 가능하다. "/"등 지정된 URL들은 permitAll() 옵션을
통해 전체 열람 권한을 주었다. "/api/v1/**"주소를 가진 API는 USER권한을 가진 사람만 가능하도록 했다.
- anyRequest -> 설정된 값들 이외 나머지 URL들을 나타낸다. 여기서는 authenticated()을 추가하여 나머지 URL들은 모두 인증된 
사용자에게만 허용하게 한다. 인증된 사용자 즉, 로그인한 사용자들을 이야기한다.
- logout().logoutSuccessUrl("/") -> 로그아웃 기능에 대한 여러 설정의 진입점이다. 로그아웃 성공 시 / 주소로 이동한다.
- oauth2Login -> oauth2Login -> OAuth2 로그인 기능에 대한 여러 설정의 진입점이다.
- userInfoEndpoint -> OAuth2 로그인 성공 이후 사용자 정보를 가져올 때의 설정들을 담당한다.
- userService -> 소셜 로그인 성공 시 후속 조치를 진행할 UserService 인터페이스의 구현체를 등록한다. 리소스 서버(즉, 소셜 서비스들)
에서 사용자 정보를 가져온 상태에서 추가로 진행하고자 하는 기능을 명시할 수 있다.

CustomOAuth2UserService
- registrationId -> 현재 로그인 진행 중인 서비스를 구분하는 코드이다. 지금은 구글만 사용하는 불필요한 값이지만, 이후 네이버 로그인
연동 시에 네이버 로그인인지, 구글 로그인인지 구분하기 위해 사용한다.
- userNameAttributeName -> OAuth2 로그인 진행 시 키가 되는 필드값을 이야기한다. Primary Key와 같은 의미이다. 구글의 경우
기본적으로 코드를 지원하지만, 네이버 카카오 등은 기본 지원하지 않습니다. 구글의 기본 코드는 "sub"입니다. 이후 네이버 로그인과
구글 로그인을 동시 지원할 때 사용된다.
- OAuthAttributes -> OAuth2UserService를 통해 가져온 OAuth2User의 attribute를 담을 클래스이다. 이후 네이버 등 다른 소셜
로그인도 이 클래스를 사용한다.
- SessionUser -> 세션에 사용자 정보를 저장하기 위한 Dto 클래스이다. 왜 User 클래스를 쓰지 않고 새로 만들어서 쓰는지 뒤이어서
상세히 설명하겠다.

OAuthAttributes
- of() -> OAuth2User에서 반환하는 사용자 정보는 Map이기 때문에 값 하나하나를 변환해야만 한다.
- toEntity() -> User 엔티티를 생성한다. OAuthAttributes에서 엔티티를 생성하는 시점은 처음 가입할 때이다.
가입할 때의 기본 권한을 GUEST로 주기 위해서 role 빌더값에는 Role.GUEST를 사용한다. OAuthAttributes 클래스 생성이 끝났으면
같은 패키지에 SessionUser 클래스를 생성한다.

SessionUser
SessionUser에는 인증된 사용자 정보만 필요하다. 그 외에 필요한 정보들은 없으니, name, email, picture만 필드로 선언한다.

LoginUSer 어노테이션
- @Target(ElementType.PARAMETER) -> 이 어노테이션이 생성될 수 있는 위치를 지정한다. PARAMETER로 지정했으니 메서드의 파라미터로
선언된 객체에서만 사용할 수 있다. 이 외에도 클래스 선언문에 쓸 수 있는 TYPE 등이 있다.
- @Retention(RetentionPolicy.RUNTIME) -> Retention어노테이션은 어노테이션 타입을 어디까지 보유할지를 설정하는 것
1. SOURCE : 어노테이션을 사실상 주석처럼 사용하는 것. 컴파일러가 컴파일할때 해당 어노테이션의 메모리를 버린다.
2. CLASS : 컴파일러가 컴파일에서는 어노테이션의 메모리를 가져가지만 실질적으로 런타임시에는 사라지게 된다. 런타임시에 사라진다는
것은 리플렉션으로 선언된 어노테이션 데이터를 가져올 수 없게 된다. 디폴트값이다.
3. RUNTIME : 어노테이션을 런타임시에까지 사용할 수 있다. JVM이 자바 바이트코드가 담긴 class 파일에서 런타임환경을 구성하고
런타임을 종료할 때까지 메모리는 살아있다.
- @interface -> 이 파일을 어노테이션 클래스로 지정한다. LoginUser라는 이름을 가진 어노테이션이 생성되었다고 보면 된다.

LoginUserArgumentResolver
LoginUserArgumentResolver는 HandlerMethodArgumentResolver 인터페이스를 구현한 클래스이다. 이 인터페이스는 한가지 기능을 지원한다.
바로 조건에 맞는 경우 메서드가 있다면 HandlerMethodArgumentResolver의 구현체가 지정한 값으로 해당 메서드의 파라미터로 넘길 수 있다.
- supportsParameter() -> 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단한다. 여기서는 파라미터에 @LoginUser 어노테이션이 붙어 있고,
파라미터 클래스 타입이 SessionUser.class인 경우 true를 반환한다.
- resolveAugument() -> 파라미터에 전달할 객체를 생성한다. 여기서는 세션에서 객체를 가져온다

참조 
- [https://www.redhat.com/ko/topics/api/what-is-a-rest-api]
- [스프링 부트와 AWS로 혼자 구현하는 웹 서비스 저자:이동욱]
- [https://inseok9068.github.io/springboot/springboot-transaction/]

