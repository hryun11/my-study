# Singleton 패턴

<br>

싱글톤 패턴은 클래스가 오직 하나의 인스턴스만을 가지고 그 인스턴스에 접근하는 글로벌 포인트를 보장한다.<br>
이는 애플리케이션의 생명주기 전반에 걸쳐 클래스의 공유 인스턴스를 하나만 갖고 싶을 때 유용하다.
<br>


```java
public class Singleton {
    
    private static Singleton instance;

    // 다른 클래스의 인스턴스 생성을 막는 private 생성자
    private Singleton() {
        
    }

    // 싱클톤 인스턴스를 얻는 메소드
    public static Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("싱글톤 인스턴스입니다.");
    }
}
```