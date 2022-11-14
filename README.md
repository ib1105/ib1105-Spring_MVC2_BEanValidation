# ib1105-Spring_MVC2_BeanValidation

<br>
스프링 MVC 2편 - 백엔드 웹 개발 활용 기술
섹션1. 타임리프 - 기본기능<br>
섹션2. 타임리프 - 스프링 통합과 폼<br>
섹션3. 메시지, 국제화<br>
섹션4. 검증1 - Validation<br>
섹션5. 검증2 - Bean Validation👨<br>
섹션6. 로그인 처리1 - 쿠키, 세션<br>
섹션7. 로그인 처리2 - 필터, 인터셉터<br>
섹션8. 예외 처리와 오류 페이지<br>
섹션9. API 예외 처리<br>
섹션10. 스프링 타입 컨버터<br>
섹션11. 파일 업로드<br>

기본셋팅

Annotation Processors > Enable annotation processing 체크

Gradle > Build and run using, Run tests using = IntelliJ IDEA

File Encodings > Global Encoding, Project Encoding, Default encodeing for properties files : UTF-8

**Bean Validation - 시작**

의존관계 추가
Bean Validation을 사용하려면 다음 의존관계를 추가해야 한다.

build.gradle
implementation 'org.springframework.boot:spring-boot-starter-validation'

spring-boot-starter-validation 의존관계를 추가하면 라이브러리가 추가 된다.

@NotBlank
private String itemName;
@NotNull
@Range(min = 1000, max = 1000000)
private Integer price;
@NotNull
@Max(9999)
private Integer quantity;

검증 애노테이션
@NotBlank : 빈값 + 공백만 있는 경우를 허용하지 않는다.
@NotNull : null 을 허용하지 않는다.
@Range(min = 1000, max = 1000000) : 범위 안의 값이어야 한다.
@Max(9999) : 최대 9999까지만 허용한다.

스프링 MVC는 어떻게 Bean Validator를 사용?
스프링 부트가 spring-boot-starter-validation 라이브러리를 넣으면 자동으로 Bean Validator를 인지하고 스프링에 통합한다.

**Bean Validation - 에러 코드**

Bean Validation을 적용하고 bindingResult 에 등록된 검증 오류 코드를 보자.
오류 코드가 애노테이션 이름으로 등록된다. 마치 typeMismatch 와 유사하다.
NotBlank 라는 오류 코드를 기반으로 MessageCodesResolver 를 통해 다양한 메시지 코드가 순서대로 생성된다.

**Bean Validation - 한계**

데이터를 등록할 때와 수정할 때는 요구사항이 다를 수 있다.

->Item을 직접 사용하지 않고, ItemSaveForm, ItemUpdateForm 같은 폼 전송을 위한 별도의 모델 객체를 만들어서 사용한다.

수정의 경우 등록과 수정은 완전히 다른 데이터가 넘어온다. 생각해보면 회원 가입시 다루는 데이터와 수정시 다루는 데이터는 범위에 차이가 있다. 예를 들면 등록시에는 로그인id, 주민번호 등등을 받을 수 있지만, 수정시에는 이런 부분이 빠진다. 그리고 검증 로직도 많이 달라진다. 그래서 ItemUpdateForm 이라는 별도의 객체로 데이터를 전달받는 것이 좋다.

@Data
public class ItemSaveForm {
@NotBlank
private String itemName;
@NotNull
@Range(min = 1000, max = 1000000)
private Integer price;
@NotNull
@Max(value = 9999)
private Integer quantity;
}

@Data
public class ItemUpdateForm {
@NotNull
private Long id;
@NotBlank
private String itemName;
@NotNull
@Range(min = 1000, max = 1000000)
private Integer price;
//수정에서는 수량은 자유롭게 변경할 수 있다.
private Integer quantity;
}
