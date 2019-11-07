# __스프링부트 세팅 방법__

* maven 혹은 gradle 프로젝트 생성
* [spring initializr](https://start.spring.io/) 이용
* CLI로 생성하는 방법도 있으나 강좌에서는 다루지않음

_`@SEE also`_ [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/)

# __스프링부트 구조__

```sh
├─ main
│   ├─ java
│   │   └─ default package
│   │          main Application
│   │          *.class
│   └─ resources
│
└─ test
    ├─ java
    │      *Test.class
    └─ resources
```

# __스프링부트의 의존성__

* spring-boot-stater에 의존성 관리에 대한 것들이 이미 되어 있음.
* 굳이 라이브러리 버전을 명시하지않아도 됨.
* 커스터마이징
  * dependency:Management 엘리먼트를 이용하면 됨. (단, parent의 설정을 사용 할 수 없음)
  * properties에 해당 변경하려면 버전 명시

# __스프링부트의 자동설정__

## __빈등록 단계__

* _`@ComponentScan`_
* _`@EnableAutoConfiguration`_

## __@ComponentScan__

* _`@Configuration`_
* _`@Component`_
* _`@Controller`_
* _`@RestController`_
* _`@Service`_
* _`@Repository`_

## __@EnableAutoConfiguration__

* ___spring.factories___ ; EnableAutoConfiguration이 빈 등록을 하는데 필요한 class들 정의
* _`@Configuration`_
* _`@ConditionalOn*`_

