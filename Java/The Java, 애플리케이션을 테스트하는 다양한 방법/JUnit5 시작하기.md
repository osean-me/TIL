# JUnit5

Spring Boot 의 기본 의존성으로 제공하는 자바 진영의 대표적인 단위 테스트 프레임워크이다.

이러한 JUnit5은 `Java 8` 이상부터 지원하며, `JUnit Platform + JUnit Jupiter + JUnit Vintage` 로 구성되어 있는데, 각각의 역할은 다음과 같다.

- JUnit Platform
    - 테스트 개발을 위해 TestEngine API 를 정의하며, JVM 에서 테스트 프레임워크를 수행할 수 있도록 한다.
- JUnit Jupiter
    - JUnit5 의 쓰기 테스트와 확장을 위한 새로운 프로그래밍 모델, 확장 모델의 조합이다. `Jupiter 하위 프로젝트는 Mock 기반의 테스트를 실행`할 수 있는 테스트 엔진을 제공한다.
- JUnit Vintage
    - Vintage 는 JUnit 3 혹은 4 기반의 테스트를 실행하기 위한 테스트 엔진을 제공한다. 클래스 경로나 모듈 경로에 JUnit 4.2 이상 버전이 존재해야 한다.

## @Test

테스트를 진행하기 위해서는 테스트를 진행하고자 하는 메소드에 `@Test` 어노테이션을 붙여줘야 한다.

JUnit4 에서의 `@Test` 와는 달리 Jupiter 의 `@Test` 는 별도의 속성을 가지지 않으며, 해당 어노테이션이 선언된 메소드는 재정의되지 않는 한 상속된다.

```java
class StudyTest {

    /**
     * 단위 테스트를 진행하기 위해서 테스트 메소드에 @Test 을 선언해줘야 한다.
     */
    @Test
    void create_1() {
        Study study = new Study();
        assertNotNull(study);
        System.out.println("create_1");
    }
}
```

## @BeforeXXX

`@BeforeEach`, `@BeforeAll` 같은 @BeforeXXX 어노테이션들은 본 테스트가 수행되기 전에 자신이 속한 메소드가 선행될 수 있도록 하는 어노테이션으로, 우선순위는 `@BeforeAll` 다음에 `@BeforeEach` 가 수행된다.

```java
/**
 * 전체 테스트를 진행할 때, 모든 테스트 메소드 실행 전에 실행되는 메소드이다.
 * 모든 우선순위 중 가장 먼저 수행되며, static 제어자는 필수 요소이다.
 */
@BeforeAll
static void beforeAll() {
    System.out.println("@BeforeAll");
}

/**
 * 단일 테스트 메소드 혹은 전체 테스트 메소드의 테스트를 진행하기 전에 실행되는 메소드이다.
 * static 제어자는 필수 요소가 아니며, @BeforeAll 다음에 수행된다.
 */
@BeforeEach
void beforeEach() {
    System.out.println("@BeforeEach");
}
```

## @AfterXXX

`@AfterEach`, `@AfterAll` 같은 @AfterXXX 어노테이션들은 본 테스트가 수행된 후에 자신이 속한 메소드가 수행될 수 있도록 하는 어노테이션으로, 우선순위는 `@AfterEach` 다음에 `@AfterAll` 이 수행된다.

```java
/**
 * 단일 테스트 메소드 혹은 전체 테스트 메소드의 테스트를 진행한 뒤에 실행되는 메소드이다.
 * static 제어자는 필수 요소가 아니며, @Test 다음에 수행된다.
 */
@AfterEach
void afterEach() {
    System.out.println("@AfterEach");
}

/**
 * 전체 테스트를 진행할 때, 모든 테스트 메소드 실행 후에 실행되는 메소드이다.
 * 모든 우선순위 중 가장 마지막에 수행되며, static 제어자는 필수 요소이다.
 */
@AfterAll
static void afterAll() {
	System.out.println("@AfterAll");
}
```

## @Disabled

테스트 클래스 전체를 테스트하고자 할 때 특정 테스트 메소드를 제외하기 위해서 사용된다.

```java
/**
 * 전체 테스트 중 특정 테스트 메소드를 제외하고 싶다면 @Disabled 를 선언한다.
 */
@Test
@Disabled
void create_2() {
	System.out.println("create_2");
}
```

## @DisplayNameGenration

테스트 메소드명에 대한 전략을 설정하는 어노테이션이다. 설정할 수 있는 전략은 다음과 같다.

- Standard
    - 기본 전략이다.
- Simple
    - 매개 변수가 없는 메서드의 후행 괄호를 제거한다.
- ReplaceUnderscoures
    - 밑줄(_)을 공백으로 바꾼다.
- IndicativeSentences
    - 문장으로 바꾼다.

```java
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
class StudyTest {
	...
}
```

## @DisplayName

테스트 메소드의 표시명을 설정하는 어노테이션으로 원하는 이름으로 테스트 메소드를 설정할 수 있다.

```java
@Test
@DisplayName("@DisplayName 이용해서 메소드명 바꾸기")
void create_3() {
	System.out.println("create_3");
}
```