# Factory Method Pattern

- Java와 다른 객체 지향 프로그래밍 언어의 **생성 디자인 패턴**
- 생성될 객체의 정확한 클래스를 특정할 것 없이 객체를 생성하는 데 사용된다.
- 객체를 생성하기 위해 직접적으로 생성자를 호출하는 대신, Factory Method Pattern은 객체 생성 메서드가 있는 인터페이스나 추상 클래스를 제공한다. <br>그런 다음, 하위 클래스 또는 구현 클래스가 이 메서드를 오버라이딩하여 해당 클래스와 관련된 객체를 생성한다.
- 객체를 생성하는 인터페이스를 정의하지만 하위 클래스에서 생성할 객체의 유형을 변경하게 만들 수 있다.
- 인스턴스화의 세부 정보를 노출하지 않고 객체를 생성하는 유연한 방법을 제공할 때 유용하다.

<br>

### Example Code

```java
// 1: Product 인터페이스를 정의한다.
interface Product {
    void display();
}

// 2: ConcreteProduct 클래스들을 생성한다.
class ConcreteProductA implements Product {
    @Override
    public void display() {
        System.out.println("Product A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void display() {
        System.out.println("Product B");
    }
}

// 3: Creator 인터페이스(or 추상 클래스)를 정의한다.
abstract class Creator {
    public abstract Product factoryMethod();
}

// 4 : ConcreteCreator 클래스들을 생성한다.
class ConcreteCreatorA extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}

// 5 : 팩토리 메서드 패턴을 사용하는 클라이언트 코드
public class FactoryMethodPattern {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.factoryMethod();
        productA.display();

        Creator creatorB = new ConcreteCreatorB();
        Product productB = creatorB.factoryMethod();
        productB.display();
    }
}
```

<br>

- Creator : 객체를 생성하는 Factory Method를 정의하는 인터페이스 혹은 추상클래스이다.<br>여기에는 생성된 객체에서 작동하는 다른 메서드가 가끔 포함된다.
- ConcreteCreator : Creator 인터페이스를 구현하거나 Creator 추상 클래스를 확장하는 구체적인 클래스. <br>각 ConcreteCreator는 특정 타입의 객체를 생성하기 위해 자체적인 구현을 제공한다.
- Product : 팩토리 메서드에 의해 생성된 객체에 대한 공통 인터페이스를 정의하는 인터페이스 혹은 추상 클래스.<br>실제 생성되는 객체인 Concrete 클래스는 이 인터페이스를 구현하거나 추상 클래스를 확장한다.
- ConcreteProduct : Product 인터페이스를 구현하거나 추상 클래스를 확장하는 구체적인 클래스.<br>각 ConcreteProduct는 ConcreteCreator가 생성할 수 있는 특정 유형의 객체를 나타낸다.

<br>

팩토리 메서드 패턴은 정확한 클래스를 몰라도 다른 유형의 Product(ConcreteProductA와 concreteProductB)를 생성할 수 있다.<br>
ConcreteCreator(ConcreteCreatorA and ConcreteProductB)는 특정 Product를 생성하기 위해 팩토리 메서드를 구현한다.<br>
이 패턴은 유연성을 높이고 클라이언트 코드를 사용하는 실체화된 클래스로부터 클라이언트 코드를 분리시킨다.