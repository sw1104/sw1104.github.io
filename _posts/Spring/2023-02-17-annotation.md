---
title: "[SPRING] Annotation(어노테이션) 정리 - 계속 추가"
excerpt: "자주 사용되는 annotation 정리"

categories:
  - spring
tags:
  - [spring, Annotation]

permalink: /spring/annotation

toc: true
toc_sticky: true

date: 2023-02-18
last_modified_at: 2023-02-18
---

## 순서는 a-z 순으로 정렬

|이름|설명|
|---|---|
|@Autowired|필요한 의존성을 자동으로 주입|
|@Component| 스프링에서 관리하는 빈(Bean)을 선언할 때 사용|
|@ComponentScan|컴포넌트 클래스를 자동으로 검색하고 Spring Bean으로 등록하는 데 사용|
|@Configuration|스프링 Bean 구성 클래스를 표시한다. 이 클래스는 애플리케이션의 Bean을 정의하는 데 사용|
|@Controller| 웹 애플리케이션에서 HTTP 요청을 처리하는 컨트롤러를 선언할 때 사용|
|@DeleteMapping| HTTP DELETE 요청을 처리하는 메서드를 매핑하는 데 사용|
|@EnableAutoConfiguration|클래스 경로에 있는 jar 파일, 설정 파일 및 기타 속성을 기반으로 스프링 Bean을 자동으로 구성하는 데 사용|
|@GetMapping| HTTP GET 요청을 처리하는 메서드를 매핑하는 데 사용|
|@PostMapping| HTTP POST 요청을 처리하는 메서드를 매핑하는 데 사용|
|@PutMapping| HTTP PUT 요청을 처리하는 메서드를 매핑하는 데 사용|
|@PathVariable| URL 경로 변수를 메서드의 인자로 전달하는 데 사용|
|@Repository| 데이터베이스와 관련된 작업을 처리하는 레파지토리 클래스에 사용|
|@RequestBody| HTTP 요청 본문을 메서드의 인자로 전달하는 데 사용|
|@RequestMapping| 요청 URL을 어떤 메서드가 처리할지 매핑하는 데 사용|
|@RequestParam| HTTP 요청 파라미터를 메서드의 인자로 전달하는 데 사용|
|@ResponseBody| 메서드가 반환하는 객체를 HTTP 응답 본문으로 전송하는 데 사용|
|@ResponseStatus| 컨트롤러에서 예외가 발생했을 때 HTTP 응답 코드를 설정하는 데 사용|
|@RestController|@Controller와 @ResponseBody를 합쳐 놓은 것으로, RESTful 웹 서비스를 제공하는 컨트롤러를 작성할 때 사용합니다.|
|@Service| 비즈니스 로직을 수행하는 서비스 클래스에 사용|
|@SpringBootApplication| 스프링 부트 애플리케이션의 메인 클래스를 표시하는 데 사용한다. @Configuration, @EnableAutoConfiguration 및 @ComponentScan의 기능이 결합되어 있다.|
|@Value|설정 파일에서 값을 읽어와 빈에 주입할 때 사용|
