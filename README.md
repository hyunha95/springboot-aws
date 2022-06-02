# springboot-aws

@RestController   
- 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 준다.
- 예전에는 @ResponseBody를 각 메소드마다 선언했던 것을 한번에 사용할 수 있게 해준다고 생각하면 된다.   
   
@GetMapping
- HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어 준다.
- 예전에는 @RequestMapping(method = RequestMethod.GET)으로 사용되었다.   
   
@RunWith(SpringRunner.class)   
- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킨다.
- 여기서는 SpringRunner라는 스프링 실행자를 사용한다.
- 즉, 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 한다.
   
@WebMvcTest   
- 여러 스프링 테스트 어노테이션 중, Web(Spring MVC)에 집중할 수 있는 어노테이션이다.
- 선언할 경우 @Controller, @ControllerAdvice 등을 사용할 수 있다.
- 단, @Service, @Component, @Repository 등은 사용할 수 없다.
- 여기서는 컨트롤러만 사용하기 땜누에 선언한다.
   
@Autowired
- 스프링이 관리하는 빈(Bean)을 주입 받는다.
   
private MockMvc mvc   
- 웹 API를 테스트할 때 사용한다.
- 스프링 MVC 테스트의 시작점이다.
- 이 클래스를 통해 HTTP GET, POST 등에 대한 API 테스트를 할 수 있다.   
   
mvc.perform(get("/hello"))   
- MockMvc를 통해 /hello 주소로 HTTP GET 요청을 한다.
- 체이닝이 지원되어 아래와 같이 여러 검증 기능을 이어서 선언할 수 있다.
   
.andExpect(status().isOK())
- mvc.perform의 결과를 검증한다.
- HTTP Header의 Status를 검증한다.
- 우리가 흔히 알고 있는 200, 404, 500 등의 상태를 검증한다.
- 여기선 OK 즉, 200인지 아닌지를 검증한다.
   
.andExpect(content().string(hello))   
- mvc.perform의 결과를 검증한다.
- 응답 본문의 내용을 검증한다.
- Controller에서 "hello"를 리턴하기 때문에 이 값이 맞는지 검증한다.
   
@RequiredArgsContructor
- 선언된 모든 final 필드가 포함된 생성자를 생성해 준다.
- final이 없는 필드는 생성자에 포함되지 않는다.
   
@assertThat    
- assertj라는 테스트 검증 라이브러리의 검증 메소드이다.
- 검증하고 싶은 대상을 메소드 인자로 받는다.
- 메소드 체이닝이 지원되어 isEqualTo와 같이 메소드를 이어서 사용할 수 있다.

@isEqualsTo   
- assertj의 동등 비교 메소드이다.
- assertThat에 있는 값과 isEqualTo값을 비교해서 같을 때만 성공이다.   
   
param
- API 테스트할 때 사용될 요청 파라미터를 설정한다.
- 단, 값은 String만 허용된다.
- 그래서 숫자/날짜 등의 데이터도 등록할 때는 문자열로 변경해야만 가능하다.
   
jsonPath   
- JSON 응답값을 필드별로 검증할 수 있는 메소드이다.
- $를 기준으로 필드명을 명시한다.
- 여기서는 name과 amount를 검증하니 $.name, $.amount로 검증한다.   
   
spring-boot-starter-data-jpa   
- 스프링 부트용 Spring Data Jpa 추상화 라이브러리이다.
- 스프링 부트 버전에 맞춰 자동으로 JPA관련 라이브러리들이 버전을 관리해 준다.   
   
h2   
- 인메모리 관계형 데이터베이스이다.
- 별도의 설치가 필요 없이 프로젝트 의존성만으로 관리할 수 있다.
- 메모리에서 실행되기 때문에 애플리케이션을 재시작할 때마다 초기화된다는 점을 이용하여 테스트 용도로 많이 사용된다.
- JPA의 테스트, 로컬 환경에서의 구동에서 사용할 예정이다.   
   
@Entity   
- 테이블과 링크될 클래스임을 나타낸다.
- 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍(\_)으로 테이블 이름을 매칭한다.
- ex)SalesManager.java -> sales_manager table
   
@Id   
- 해당 테이블의 PK필드를 나타낸다.
   
@GeneratedValue   
- PK의 생성 규칙을 나타낸다.
- 스프링 부트 2.0에서는 GenerationType.INDENTITY옵션을 추가해야만 auto_increment가 된다.
   
@Column   
- 테이블의 칼럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 칼럼이 된다.
- 사용하응 이유는, 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용한다.
- 문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶거나(ex:title), 타입을 TEXT로 변경하고 싶거나(ex:content) 등의 경우에 사용된다.   
   
@Builder   
- 해당 클래스의 빌더 패턴 클래스를 생성
- 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함   
   
Entity클래스에서는 절대 setter메소드를 만들지 않는다. 대신, 해당 필드의 값 변경이 필요하면 명확히 그 목적과 의도를 나타낼 수 있는 메소드를 추가해야만 한다.
