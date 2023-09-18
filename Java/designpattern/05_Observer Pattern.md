# Observer Pattern

- 옵저버 패턴은 객체 간의 일대다 관계를 정의하는 자바의 행동 패턴이다.
- 한 객체(대상)의 상태가 변화할 때 그에 의존하는 객체(옵저버)를 대상 객체에 알릴 수 있다. 그래서 이들은 자동으로 스스로를 업데이트 한다.
- 이 패턴은 객체 간의 결합을 더 느슨하게 하여 코드를 유지보수하고 확장하는 것을 용이하게 한다.
- 자바에서 옵저버 패턴은 종종 객체가 다른 여러 객체들에게 상태에서의 변화를 알리는 것이 필요한 상황에 사용된다.
- 이 패턴은 GUI 프레임 워크, 이벤트 핸들링 시스템 등에서 널리 사용된다.


### Example Code

```java
package Java.designpattern;

import java.util.ArrayList;
import java.util.List;

interface Subject {
    void addObserver(Observer observer);

    void removeObserver(Observer observer);

    void notifyObservers();
}

// 구현체
class WeatherStation implements Subject {
    private double temperature;
    private List<Observer> observers = new ArrayList<>();

    public void setTemperature(double temperature) {
        this.temperature = temperature;
        notifyObservers();
    }

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature);
        }
    }
}

interface Observer {
    void update(double temperature);
}

// Observer 구현체
class WeatherDisplay implements Observer {
    private double temperature;

    @Override
    public void update(double temperature) {
        this.temperature = temperature;
        display();
    }

    public void display() {
        System.out.println("Current Temperature " + temperature +" degrees Celsius");
    }
}

public class ObserverPattern {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation();
        WeatherDisplay display1 = new WeatherDisplay();
        WeatherDisplay display2 = new WeatherDisplay();

        // weatherstation에 옵저버를 추가한다
        weatherStation.addObserver(display1);
        weatherStation.addObserver(display2);

        // 온도 변화
        weatherStation.setTemperature(25.5);
    }
}

```

- 'WeatherStation' : 날씨를 모니터하고 온도가 변화할 때 옵저버에게 알리는 주체
- 'WeatherDisplay' : 현재 기온을 표시해주는 옵저버 구현체

<br>

- WeatherStation의 `setTemperature` 메서드가 호출되면, 온도를 수정하고 모든 등록된 옵저버에게 알린다.
- 옵저버(WeatherDisplay 인스턴스)는 자동으로 새 온도로 업데이트되며 온도를 표시한다.

<br>

이것이 자바 옵저버 패턴의 기본 구현이다. 주체(WeatherStation)가 상태가 변할 때 어떻게 여러 옵저버들에게(WeatherDisplay) 알릴 수 있는지 보여준다.