# Adaptor Pattern

- 기존 클래스의 인터페이스를 다른 인터페이스로 사용할 수 있도록 Java에서 사용되는 구조 패턴(Structural Design)
- 호환되지 않는 인터페이스의 두 클래스를 함께 작동하고 싶을 때 유용하다.
- 타겟 인터페이스를 구현하고 호출을 어댑터에 위임하는 wrapper 클래스를 생성하여 호출을 어댑터가 이해할 수 있는 포맷으로 변환한다.

### Example Code
```java
class OldSystem {
    public void oldRequest() {
        System.out.println("Old system is processing the request");
    }
}

interface NewInterface {
    void newRequest();
}

// adaptor class : OldSystem의 메서드를 NewInterface의 newRequest 메서드에 넣는다.
class Adapter implements NewInterface {
    private OldSystem oldSystem;

    public Adapter(OldSystem oldSystem) {
        this.oldSystem = oldSystem;
    }

    @Override
    public void newRequest() {
        // 호출을 old system 메서드로 위임
        oldSystem.oldRequest();
    }
}

public class AdapterPattern {
    public static void main(String[] args) {
        OldSystem oldSystem = new OldSystem();
        NewInterface adapter = new Adapter(oldSystem);

        adapter.newRequest();
    }
}
```
Console : 
> Old system is processing the request


<br>

- Adapter 클래스는 OldSystem과 NewInterface 사이의 가교 역할을 하며 이들을 함께 작동시킨다.
- **Adapter 클래스는 호출을 newRequest()에서 oldRequest()로 전달한다. 그래서 old system은 new system의 맥락에서 사용할 수 있게 된다.** 이것이 Adapter 패턴의 핵심이다.