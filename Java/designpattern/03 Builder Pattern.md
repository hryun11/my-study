# Builder Pattern

- 단계적인 접근방식으로 복잡한 객체를 생성하는데 사용되는 생성 디자인 패턴
- 특히 객체가 많은 생성자 매개 변수나 구성 옵션이 많아 생성 프로세스를 복잡하게 만들거나 오류가 발생하기 쉬운 경우 유용하다.
- 빌더 패턴은 객체의 구성을 객체의 표현으로부터 분리하며, 그 타입과 내용을 특정하여 객체를 생성할 수 있게 한다.

<br>

### Example Code

```java
class Computer {
    private String CPU;
    private String RAM;
    private String storage;

    private Computer() {}

    public String getCPU() {
        return CPU;
    }

    public String getRAM() {
        return RAM;
    }

    public String getStorage() {
        return storage;
    }

    public static class ComputerBuilder {
        private String CPU;
        private String RAM;
        private String storage;

        public ComputerBuilder setCPU(String CPU) {
            this.CPU = CPU;
            return this;
        }

        public ComputerBuilder setRAM(String RAM) {
            this.RAM = RAM;
            return this;
        }

        public ComputerBuilder setStorage(String storage) {
            this.storage = storage;
            return this;
        }

        public Computer build() {
            Computer computer = new Computer();
            computer.CPU = this.CPU;
            computer.RAM = this.RAM;
            computer.storage = this.storage;
            return computer;
        }
    }

}
public class BuilderPattern {
    public static void main(String[] args) {
        Computer computer = new Computer.ComputerBuilder()
                .setCPU("Intel Core i7")
                .setRAM("16GB")
                .setStorage("512GB SSD")
                .build();

        System.out.println("computer.getCPU() = " + computer.getCPU());;
        System.out.println("computer.getRAM() = " + computer.getRAM());
        System.out.println("computer.getStorage() = " + computer.getStorage());
    }
}

```
>Output
> 
> computer.getCPU() = Intel Core i7
computer.getRAM() = 16GB
computer.getStorage() = 512GB SSD


<br>

#### 1. 빌더 클래스를 정의한다

- 먼저 복합 객체를 구성하는 분리된 빌더 클래스를 생성한다.
- 이 클래스는 생성하려는 객체의 다양한 속성들이나 구성 옵션을 설정하는 메서드를 가진다.
- 이 메서드들은 보통 빌더 자신을 리턴하며 메서드 체이닝을 가능하게 한다. ex) setCPU().setRAM().setRAM().build();

#### 2. 생산 클래스를 정의한다

- 생성하려는 복합 객체를 보여주는 생산 클래스 또한 가진다.
- 이 클래스는 private 생산자를 가져야하며 객체의 속성에 접근하기 위한 메서드를 제공한다.

#### 3. Director(선택)

- 몇 가지 경우에는 빌더를 사용하여 구성 프로세스를 지휘하는 'Director' 클래스를 만들 수 있다.
- 그러나 간단한 경우에는 클라이언트 코드가 객체를 생성하기 위한 빌더를 직접적으로 사용할 수 있다.

<br>

> - 이 예시에서 'Computer' 클래스는 생산 클래스이고 'ComputerBuilder'는 Computer 객체를 단계적으로 구성하는데 사용되며 객체의 속성을 깨끗하고 알아보기 쉬운 방식으로 특정할 수 있게 한다.

<br>

### 장점

- 빌더 패턴은 특히 많은 선택적 매개 변수들이 있는 복잡한 객체 생성을 보다 유연하게 한다.
- 클라이언트 코드에서 구성 로직을 분리함으로써 코드 가독성과 유지보수를 증대시킨다.