# Dependency Injection (DI)

- 스프링은 객체 의존성을 관리하고 소프트웨어 애플리케이션에서 컴포넌트들을 분리시켜주는 디자인 패턴인 의존성 주입을 통해 결합을 느슨하게 한다.
- 스프링에서 DI는 객체 생성과 관리의 [제어를 역전](02_IoC.md)(IoC)하여 DI를 이룬다.
- 객체가 자신들의 의존성을 생성하는 대신. 스프링 컨테이너(ApplicationContext)가 런타임에 객체에 요구되는 의존성을 주입한다.
- 이는 코드를 더 모듈화하고, 테스트에 용이하게 하며 유지보수 가능하게 한다.

### Process
#### 1. Bean을 정의한다.

- 스프링에서 일반적으로 어노테이션이나 XML 설정을 이용하여 빈을 정의할 수 있다.
```java

@Component
class UserService {
    private UserRepository userRepository;

    @Autowired // 필드 인젝션을 통한 의존성 주입
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

- `UserService` 클래스에서 `UserRepository`에 대한 의손성을 선언했다. 생성자에 `@Autowired` 어노테이션을 달아 `UserService` 빈이 생성될 때 스프링이 `UserRepository`의 인스턴스를 주입해야한다고 지시한다.


#### 2. 또 다른 빈을 정의한다.

- UserService에 의해 의존성으로 사용되는 UserRepository이다.
```java

@Repository
class UserRepository {
    
}
```
- @Repository로 스프링이 관리하는 리포지토리로 UserRepository에 표시한다.


#### 3. Configuration 클래스 생성
- 스프링을 설정하고 컴포넌트 스캔을 가능하게 만들기 위해 설정 클래스를 생성한다.
```java
@Configuration
@ComponentScan(basePackages = "com.example.app")
class AppConfig{
    
}
```

- @Configuration 어노테이션은 설정 클래스로 이 클래스를 표시하고 @ComponentScan은 스프링 컴포넌트(@Component, @Repository 등)들을 스캔하려는 베이스 패키지를 특정한다.

#### 4. 스프링 컨테이너를 초기화한다.
- 애플리캐이션의 엔트리포인트에서 Spring ApplicationContext를 초기화한다.

```java
class MainApp {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        // 컨테이너에서 빈을 검색해온다.
        UserService userService = context.getBean(UserService.class);
    }
}
```
- `AnnotationConfigApplicationContext`를 생성하고 `AppConfig`클래스를 특정할 때 스프링은 컴포넌트를 위해 특정된 패키지를 스캔하고 빈의 인스턴스를 생성하고, 그들 간의 의존성을 해제한다. 그리고 그들을 ApplicationContext에 담는다.
- 스프링은 초기화할 동안에 `UserService` 빈에 `UserRepository` 의존성 주입을 책임진다.
- `UserService`는 `UserRepository`가 어떻게 생성되는지 알 필요가 없기 때문에 이는 결합을 느슨하게 한다. 
- 오직 인터페이스에 의해 정의된 추상화에만 의존한다.
- 이 단계를 따라 스프링 DI를 만들어 결합을 느슨하게 할 수 있고 코드가 유지보수에 용이하게 되며, 테스트 중 의존성을 mock이나 stub으로 대체하여 쉽게 테스트 할 수 있다.