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
   
JpaRepository   
MyBatis 등에서 Dao라고 불리느 DB Layer 접근자이다. JPA에선 Repository라고 부르며 인터페이스로 생성한다. 단순히 인터페이스를 생성 후, JpaRepository<Entity클래스, PK타입>를 상속하면 기본적인 CRUD 메소드가 자동으로 생성된다.   
@Repository를 추가할 필요도 없다. 여기서 주의할 점은 Entity클래스와 기본 Entity Repository는 함께 위치해야 한다는 점이다. 둘은 아주 밀접한 관계이고, Entity 클래스는 기본 Repository없이는 제대로 역할을 할 수가 없다.   
   
@After   
- Junit에서 단위 테스트가 끝날 때마다 수행되는 메소드를 지정
- 보통은 배포 전 전체 테스트를 수행할 때 테스트간 데이터 침범을 막기 위해 사용한다.
- 여러 테스트가 동시에 수행되면 테스트용 데이터베이스인 H2에 데이터가 그대로 남아 있어 다음 테스트 실행 시 테스트가 실패할 수 있다.   
   
postsRepository.save   
- 테이블 posts에 insert/update 쿼리를 실행한다.
- id 값이 있다면 update가, 없다면 insert쿼리가 실행된다.   
   
postRepository.findAll   
- 테이블 posts에 있는 모든 데이터를 조회해오는 메소드이다.
   
https://github.com/hyunha95/springboot-aws/blob/b14c49fa5541fc0132889c1ff2f20ac10e2538de/src/main/java/com/jojoldu/book/springboot/service/posts/PostsService.java#L25   
여기서 신기한 것이 있다. update 기능에서 데이터베이스에 쿼리를 날리는 부분이 없다. 이게 가능한 이유는 JPA의 영속성 컨텍스트 때문이다.   
영속성 컨텍스트란, 엔티티를 영구 저장한는 환경이다. 일종의 논리적 개념이라고 보면 되며, JPA의 핵심 내용은 엔티티가 영속성 컨텍스트에 포함되어 있냐 아니냐로 갈린다. JPA의 엔티티 매니저(Entity Manager)가 활성화된 상태로(Spring Data Jpa를 쓴다면 기본 옵션) 트랜잭션 안에서 데이터베이스에서 데이터를 가져오면 이 데이터는 영속성 컨텍스트가 유지된 상태이다.   
이 상태에서 해당 데이터의 값을 변경하면 트랜잭션이 끝나느 시점에 해당 테이블에 변경분을 반영한다. 즉, Entity 객체의 값ㅁ나 변경하면 별도로 Update쿼리를 날릴 필요가 없다는 것이다. 이 개념을 더티 체킹(dirty checking)이라고 한다.   
   
Java8이 나오기 전까지 사용되었던 Date와 Calendar 클래스는 다음과 같은 문제점들이 있었다.   
1. 불변(변경이 불가능한)객체가 아니다.   
   - 멀티스레드 환경에서 언제든 문제가 발생할 수 있다.   
2. Calendar는 월(Month)값 설계가 잘못되어있다.   
   - 10월을 나타내는 Calendar.OCTOBER의 숫자 값은 '9'이다.
   - 당연히 '10'으로 생각했던 개발자들에게는 큰 혼란이 왔다.   
   
JPA Auditing으로 생성시간/수정시간 자동화하기
---
BaseTimeEntity클래스는 모든 Entity의 상위 클래스가 되어 Entity들의 createdDate, modifiedDate를 자동으로 관리하는 역할을 한다.   
@MappedSuperclass
- JPA Entity 클래스들이 BaseTimeEntity을 상속할 경우 필드들(createdDate, modifiedDate)도 컬럼으로 인식하도록 한다.
   
@EntityListeners(AuditingEntityListener.class)   
- BaseTimeEntity클래스에 Auditing 기능을 포함시킨다.   
   
@CreatedDate   
- Entity가 생성되어 저장될 때 시간이 자동 저장된다.   
   
@LastModifiedDate   
- 조회한 Entity의 값을 변경할 때 시간이 자동 저장된다.








