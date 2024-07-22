---
title: "[OOP] Design Pattern"
published: 2024-07-22
description: ''
image: ''
tags: [CS, 'Design Pattern' , GoF]
category: 'Object-Oriented Programming'
draft: false 
---
# 개요

Design Pattern이 필요한 이유는 무엇일까?

우리가 문서를 작성한다고 할때, 띄어쓰기, 문단 줄바꿈, 자간 그리고 주석달기와 형식에 맞춰서 작성을 한다.

이유는 왜그럴까? 문서를 쉽고 빠르게 읽기 위해서 그런 것이다. 즉 가독성 향상을 위한 방법이다.
그리고, 누구든 문서를 알아볼 수 있도록한다. 또한, 다음 버전에 문서를 쓸때 레퍼런스를 삼을 수 있고, 인용구로 활용할 수 있다.

코드를 작성하는 것도 우리가 문서를 작성하거나, 문서를 독해하는 것과 똑같다.
소스코드도 문서처럼 여러 사람들과 함께 협업을 해야하고, 코드를 기능 별로 나누어서 이 기능들을 재사용성을 올려한다. 즉 코드 자체의 재사용성이 올라가는 것이다.

그렇다면, 자간, 띄어쓰기 처럼 소스코드에서는 GoF의 디자인패턴을 통하여 소스코드의 템플릿화를 한 것이라고 볼 수 있다.

또한, 우리가 OOP와도 매우 좋은 시너지를 갖고 있다.

그리고 생성 패턴 5가지, 구조 패턴 7가지, 행동 패턴 11가지로 나타난다. 하나씩 알아보자.

## 생성 패턴

객체 생성과 관련된 패턴으로, 객체 생성의 과정을 캡슐화하고 제어하여 시스템의 유연성과 재사용성을 높일 수 있다. `객체 생성` 로직을 클래스에서 분리, 생성과정 단순화하여 코드의 유지보수성을 높일 수 있다.

### 싱글톤(Singleton)

- `특정 클래스의 인스턴스가 하나만 존재하도록 보장`하며, 이 인스턴스에 대한 `전역적인 접근점을 제공`
- 여러 프로세스가 동시에 참조할 수 없음
- 불필요한 메모리 낭비를 최소화한다.
- 주로 애플리케이션의 설정 정보, 로깅, 스레드 풀 등에서 사용

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 팩토리 메서드(Factory Method)

- 객체를 생성하기 위한 `인터페이스를 정의하고 서브 클래스에서 어떤 클래스의 인스턴스를 생성할지 결정`하는 패턴
- 객체 생성을 위한 인터페이스를 정의하고, 실제 `객체 생성은 서브클래스에서 처리`
- 클라이언트 코드에서 `구체적인 클래스에 의존하지 않고 객체를 생성`할 수 있게 합니다.
- 제품군 생성, 다양한 형태의 객체 생성시 사용

```java
abstract class Product {
    abstract void use();
}

class ConcreteProductA extends Product {
    void use() { System.out.println("Using Product A"); }
}

class ConcreteProductB extends Product {
    void use() { System.out.println("Using Product B"); }
}

abstract class Creator {
    abstract Product factoryMethod();
}

class ConcreteCreatorA extends Creator {
    Product factoryMethod() { return new ConcreteProductA(); }
}

class ConcreteCreatorB extends Creator {
    Product factoryMethod() { return new ConcreteProductB(); }
}
```

### 추상 팩토리(Abstract Factory)

- 관련된 `객체의 집합을 생성하는 인터페이스를 제공`하며, `구체적인 팩토리 클래스를 통해 객체 생성을 추상화`하는 패턴
- 관련된 객체들의 제품군을 생성할 수 있는 인터페이스를 제공하며, 구체적인 클래스는 서브클래스에서 정의
- 구체적인 클래스에 의존하지 않고, `인터페이스를 통해 서로 연관,의존하는 객체들의 그룹`으로 생성하여 추상적으로 표현
- 서로 관련된 객체들을 일관된 방식으로 생성시 사용
- 연관된 서브클래스를 묶어 한 번에 교체하는 것이 가능

```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}
```

### 빌더(Builder)

- `복잡한 객체를 단계별로 생성`할 수 있게 하며, `객체의 생성 과정을 분리하여 동일한 생성 절차에서 다양한 표현을 제공`합니다.
- 객체 생성의 과정과 표현을 분리하여 유연성가져 `동일한 객체 생성에서도 서로 다른 결과`를 만들어 낼 수 있음

```java
class Product {
    private String partA;
    private String partB;

    public void setPartA(String partA) { this.partA = partA; }
    public void setPartB(String partB) { this.partB = partB; }
}

class Builder {
    private Product product = new Product();

    public Builder buildPartA(String partA) {
        product.setPartA(partA);
        return this;
    }

    public Builder buildPartB(String partB) {
        product.setPartB(partB);
        return this;
    }

    public Product getResult() { return product; }
}
```

> 🥕 Spring Lombok
>
> 스프링 Lombok에는 `@Builder 애너테이션`이 있다. 이를 통해서 쉽게 빌더 패턴을 구현할 수있다.

### 프로토타입(Prototype)

- `기존 객체를 복제하여 새로운 객체를 생성하는 패턴`입니다.
- 객체 생성 비용이 큰 경우에 유용하며, `객체를 복제함으로써 새로운 인스턴스를 빠르게 생성`할 수 있습니다.

```java
class Prototype implements Cloneable {
    private String field;

    public Prototype(String field) {
        this.field = field;
    }

    public Prototype clone() {
        try {
            return (Prototype) super.clone();
        } catch (CloneNotSupportedException e) {
            return new Prototype(this.field);
        }
    }
}
```

> 🥕 JS의 Prototype
>
> 자바스크립트에서는 프로토타입 패턴을 활용하여 객체의 청사진을 만들수 있다.

## 구조 패턴

객체나 클래스를 조합하여 더 큰 구조를 형성하는 방법을 다루는 디자인 패턴입니다. `인터페이스를 정의하고 객체 간의 관계를 설정`함으로써 시스템의 구조를 효율적이고 유연하게 설계할 수 있다.

### 어뎁터(Adaptor)

- 인터페이스 호환성을 제공하지 않는 `클래스를 사용하기 위한 Wrapper를 제공`하는 패턴
- 서로 다른 인터페이스를 가진 클래스를 함께 사용할 수 있도록 변환
- 클라이언트가 기대하는 인터페이스를 제공하여, 기존 코드를 수정하지 않고도 새로운 기능을 통합

```java
interface Target {
    void request();
}

class Adaptee {
    void specificRequest() {
        System.out.println("Specific request");
    }
}

class Adapter implements Target {
    private Adaptee adaptee;

    Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}
```

### 브릿지(Bridge)

- `추상화와 구현을 분리하여 두 가지를 독립적으로 확장`할 수 있는 패턴
- 구현부와 추상부를 분리하여 독립적으로 변형
- 인터페이스와 구현을 분리하여, 서로 독립적으로 확장

```java
interface Implementor {
    void operationImpl();
}

class ConcreteImplementorA implements Implementor {
    public void operationImpl() {
        System.out.println("Implementation A");
    }
}

class ConcreteImplementorB implements Implementor {
    public void operationImpl() {
        System.out.println("Implementation B");
    }
}

abstract class Abstraction {
    protected Implementor implementor;

    Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    abstract void operation();
}

class RefinedAbstraction extends Abstraction {
    RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    void operation() {
        implementor.operationImpl();
    }
}
```

### 컴포지트(Composite)

- 객체들을 트리 구조로 구성하여 부분-전체 계층을 표현
- 개별 객체와 복합 객체를 동일하게 처리
- 객체들을 트리구조로 구성하여 디렉터리 안에 디렉터리가 있듯이 복합 객체 안에 복합 객체가 포함되는 구조를 구현할 수있음

```java
interface Component {
    void operation();
}

class Leaf implements Component {
    public void operation() {
        System.out.println("Leaf");
    }
}

class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void operation() {
        for (Component child : children) {
            child.operation();
        }
    }
}
```

### 데코레이터(Decorator)

- 객체간의 결합을 통해 능동적으로 기능들을 확장 할 수 있는 패턴
- 객체에 `추가 요소를 동적으로 더하여 기능`을 확장
- 서브클래싱 없이 기능을 확장

```java
interface Component {
    void operation();
}

class ConcreteComponent implements Component {
    public void operation() {
        System.out.println("ConcreteComponent");
    }
}

class Decorator implements Component {
    protected Component component;

    Decorator(Component component) {
        this.component = component;
    }

    public void operation() {
        component.operation();
    }
}

class ConcreteDecoratorA extends Decorator {
    ConcreteDecoratorA(Component component) {
        super(component);
    }

    public void operation() {
        super.operation();
        System.out.println("Decorator A");
    }
}

class ConcreteDecoratorB extends Decorator {
    ConcreteDecoratorB(Component component) {
        super(component);
    }

    public void operation() {
        super.operation();
        System.out.println("Decorator B");
    }
}
```

### 파사드(Facade)

- `서브시스템을 더 쉽게 사용할 수 있도록 단순한 인터페이스를 제공`하는 패턴
- 복잡한 서브시스템에 대한 간단한 인터페이스를 제공
- 서브시스템을 쉽게 사용할 수 있게 하고, 코드의 복잡성을 감소

```java
class SubsystemA {
    void operationA() {
        System.out.println("Operation A");
    }
}

class SubsystemB {
    void operationB() {
        System.out.println("Operation B");
    }
}

class Facade {
    private SubsystemA subsystemA = new SubsystemA();
    private SubsystemB subsystemB = new SubsystemB();

    void operation() {
        subsystemA.operationA();
        subsystemB.operationB();
    }
}
```

> 🥕 Facade란?
>
> CS를 학습하다보면 건축용어들이 꽤나 많이 등장을 한다. 그 중 하나도 파사드이다. 프랑스어로 파사드의 사전적 의미는 건축물의 전면,정면을 의미한다.
> 우리가 건물의 정면 입구를 통해 들어가는 건물의 정면의 외벽을 파사드라고 한다.
>
> 파사드 패턴도 정면과 같이 서브시스템의 인터페이스를 형성해주는 것이다.

### 플라이웨이트(Flyweight)

- `공유 가능한 객체를 통해 메모리 사용을 최적화`하는 패턴
- 많은 수의 작은 객체를 효율적으로 공유하여 메모리 사용
- 동일한 객체를 공유하여 메모리 낭비를 방지 ➡️ 경량화

```java
class Flyweight {
    private String intrinsicState;

    Flyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    void operation(String extrinsicState) {
        System.out.println("Intrinsic: " + intrinsicState + ", Extrinsic: " + extrinsicState);
    }
}

class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();

    Flyweight getFlyweight(String key) {
        if (!flyweights.containsKey(key)) {
            flyweights.put(key, new Flyweight(key));
        }
        return flyweights.get(key);
    }
}
```

### 프록시(Proxy)

- `실제 객체에 대한 대리자를 제공`하여 접근을 제어
- 접근 제어, 로깅, 지연 초기화 등 다양한 목적으로 사용

```java
interface Subject {
    void request();
}

class RealSubject implements Subject {
    public void request() {
        System.out.println("RealSubject");
    }
}

class Proxy implements Subject {
    private RealSubject realSubject;

    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.request();
    }
}
```

## 행동(Behavior) 패턴

객체나 클래스 간의 `책임 분배와 통신 방식`을 정의하는 패턴입니다. 객체 간의 `상호작용을 통해 기능을 수행`할 수 있도록 하며, 시스템의 유연성과 재사용성을 높이는 데 중점을 둡니다.

### 옵저버(Observer)

- `객체 간의 1: m 종속 관계를 정의하여 한 객체의 상태 변경이 다른 객체들에게 알려`지도록 설계된 패턴
- 객체의 상태 변화에 따라 의존성 객체들이 자동으로 업데이트
- 일대다(one-to-many) 의존성을 정의하여, 주체 객체의 상태 변화가 여러 객체에 전파
- 주로 분산된 시스템 간에 이벤트를 생성, 발생(Publish), 구독(Subscribe)해야할 때 이용함

```java
import java.util.ArrayList;
import java.util.List;

// Observer 인터페이스
interface Observer {
    void update(String message);
}

// ConcreteObserver 클래스
class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received: " + message);
    }
}

// Subject 클래스
class Subject {
    private List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        Subject subject = new Subject();
        Observer observer1 = new ConcreteObserver("Observer 1");
        Observer observer2 = new ConcreteObserver("Observer 2");

        subject.addObserver(observer1);
        subject.addObserver(observer2);

        subject.notifyObservers("Hello, Observers!");
    }
}
```

### 전략(Strategy)

- `알고리즘을 정의하고, 실행 중에 선택`할 수 있도록 설계된 패턴
- 알고리즘군을 정의하고, 각각을 캡슐화하여 상호 교환 가능
- 클라이언트는 알고리즘을 직접 사용하지 않고, 런타임에 알고리즘을 선택

```java
// Strategy 인터페이스
interface Strategy {
    void execute();
}

// ConcreteStrategyA 클래스
class ConcreteStrategyA implements Strategy {
    public void execute() {
        System.out.println("Executing strategy A");
    }
}

// ConcreteStrategyB 클래스
class ConcreteStrategyB implements Strategy {
    public void execute() {
        System.out.println("Executing strategy B");
    }
}

// Context 클래스
class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.execute();
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        Context context = new Context();

        context.setStrategy(new ConcreteStrategyA());
        context.executeStrategy();

        context.setStrategy(new ConcreteStrategyB());
        context.executeStrategy();
    }
}
```

### 커맨트(Command)

- `요청을 객체로 캡슐화하여 요청을 매개변수화 하고, 요청을 큐에 저장하거나 로깅하고 실행을 지연`시키도록 설계된 패턴
- 요청을 객체의 형태로 캡슐화하여 다양한 요청, 큐잉, 로깅, 실행 취소 작업을 매개변수화
- 요청을 큐에 저장하거나 로그에 기록할 수 있으며, 작업을 취소하거나 다시 실행

```java
interface Command {
    void execute();
}

class Light {
    public void on() {
        System.out.println("Light is on");
    }

    public void off() {
        System.out.println("Light is off");
    }
}

class LightOnCommand implements Command {
    private Light light;

    LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }
}

class LightOffCommand implements Command {
    private Light light;

    LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.off();
    }
}

class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}
```

### 상태(State)

- `객체의 상태를 캡슐화하고, 캡슐화된 내부 객체가 해당 객체의 행동을 변경`할 수 있도록하는 패턴
- 객체의 상태에 따라 행동이 달라지도록 하고, 상태를 객체로 캡슐화하여 상태 전환을 관리.
- 상태에 따른 행위 변화를 캡슐화하여 코드의 복잡성 감소

```java
// State 인터페이스
interface State {
    void handle(Context context);
}

// ConcreteStateA 클래스
class ConcreteStateA implements State {
    public void handle(Context context) {
        System.out.println("State A is handling the request.");
        context.setState(new ConcreteStateB());
    }
}

// ConcreteStateB 클래스
class ConcreteStateB implements State {
    public void handle(Context context) {
        System.out.println("State B is handling the request.");
        context.setState(new ConcreteStateA());
    }
}

// Context 클래스
class Context {
    private State state;

    public Context(State state) {
        this.state = state;
    }

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handle(this);
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        Context context = new Context(new ConcreteStateA());
        context.request();
        context.request();
        context.request();
    }
}
```

### 책임연쇄(Chain of Responsibility)

- `요청을 보내는 객체와 이를 핸들러 객체를 분리하여, 요청을 처리할지 그 다음 핸들러로 요청`을 넘길지를 결정하는 패턴
- `요청을 처리할 수 있는 객체의 연쇄`를 만들고, 각 객체가 자신이 처리할 수 있는 요청을 처리하거나 다음 객체로 넘김
- 클라이언트는 요청의 수신자와 수신자의 순서를 알 필요가 없음

```java
abstract class Handler {
    protected Handler next;

    public void setNext(Handler next) {
        this.next = next;
    }

    public abstract void handleRequest(String request);
}

class ConcreteHandlerA extends Handler {
    public void handleRequest(String request) {
        if (request.equals("A")) {
            System.out.println("Handler A processed the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

class ConcreteHandlerB extends Handler {
    public void handleRequest(String request) {
        if (request.equals("B")) {
            System.out.println("Handler B processed the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

```

### 방문자(Visitor)

- `알고리즘들을 그들이  작동하는 객체들로부터 분리`할 수 있도록 하는 행동 패턴
- 객체 구조를 변경하지 않고도 새로운 연산을 추가 가능
- 연산을 객체 구조로부터 분리하여, 새로운 연산을 쉽게 추가

```java
// Visitor 인터페이스
interface Visitor {
    void visit(ConcreteElementA elementA);
    void visit(ConcreteElementB elementB);
}

// ConcreteVisitor 클래스
class ConcreteVisitor implements Visitor {
    public void visit(ConcreteElementA elementA) {
        System.out.println("Visiting ConcreteElementA");
    }

    public void visit(ConcreteElementB elementB) {
        System.out.println("Visiting ConcreteElementB");
    }
}

// Element 인터페이스
interface Element {
    void accept(Visitor visitor);
}

// ConcreteElementA 클래스
class ConcreteElementA implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// ConcreteElementB 클래스
class ConcreteElementB implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        Element[] elements = { new ConcreteElementA(), new ConcreteElementB() };
        Visitor visitor = new ConcreteVisitor();

        for (Element element : elements) {
            element.accept(visitor);
        }
    }
}
```

### 인터프리터(Intepreter)

- `주어진 언어나 문법을 해석하고 실행하는 역할을 담당`하는 패턴
- 언어의 문법을 나타내는 클래스를 사용하여 언어의 해석 과정을 정의
- 주로 SQL이나 통신 프로토콜, 표현식 계산 등에 사용

```java
interface Expression {
    int interpret();
}

class Number implements Expression {
    private int number;

    Number(int number) {
        this.number = number;
    }

    public int interpret() {
        return number;
    }
}

class Add implements Expression {
    private Expression left;
    private Expression right;

    Add(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    public int interpret() {
        return left.interpret() + right.interpret();
    }
}

class Subtract implements Expression {
    private Expression left;
    private Expression right;

    Subtract(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    public int interpret() {
        return left.interpret() - right.interpret();
    }
}

```

### 메멘토(Memento)

- `객체를 이전상태로 되돌릴 수 있도록 설계`된 패턴
- 객체의 상태를 저장하고 복원할 수 있게 하여, 상태를 이전으로 되돌리기 가능
- 객체의 내부 상태를 캡슐화하여 외부에 노출 안함

```java
// Memento 클래스
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// Originator 클래스
class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento saveStateToMemento() {
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento) {
        state = memento.getState();
    }
}

// Caretaker 클래스
class Caretaker {
    private List<Memento> mementoList = new ArrayList<>();

    public void add(Memento state) {
        mementoList.add(state);
    }

    public Memento get(int index) {
        return mementoList.get(index);
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.setState("State #1");
        originator.setState("State #2");
        caretaker.add(originator.saveStateToMemento());

        originator.setState("State #3");
        caretaker.add(originator.saveStateToMemento());

        originator.setState("State #4");

        System.out.println("Current State: " + originator.getState());
        originator.getStateFromMemento(caretaker.get(0));
        System.out.println("First saved State: " + originator.getState());
        originator.getStateFromMemento(caretaker.get(1));
        System.out.println("Second saved State: " + originator.getState());
    }
}
```

### 중재자(Mediator)

- `객체간의 복잡한 통신을 조정하고 의존 관계들을 줄일 수 있도록 설계`된 패턴
- 객체들이 서로 통신하는 방법을 캡슐화하여 객체 간의 직접적인 통신을 피하고, 객체 간의 상호작용을 중앙 집중화
- 객체 간의 결합도를 줄이고 재사용성을 상승

```java
interface Mediator {
    void sendMessage(String message, Colleague colleague);
}

abstract class Colleague {
    protected Mediator mediator;

    Colleague(Mediator mediator) {
        this.mediator = mediator;
    }

    public abstract void receive(String message);
}

class ConcreteColleagueA extends Colleague {
    ConcreteColleagueA(Mediator mediator) {
        super(mediator);
    }

    public void send(String message) {
        mediator.sendMessage(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague A received: " + message);
    }
}

class ConcreteColleagueB extends Colleague {
    ConcreteColleagueB(Mediator mediator) {
        super(mediator);
    }

    public void send(String message) {
        mediator.sendMessage(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague B received: " + message);
    }
}

class ConcreteMediator implements Mediator {
    private ConcreteColleagueA colleagueA;
    private ConcreteColleagueB colleagueB;

    public void setColleagueA(
```

### 탬플릿 매서드(Template Method)

- `알고리즘의 골격을 정의`하고, 일부 단계를 `서브클래스에서 구현`
- 알고리즘의 구조는 변경하지 않고, 특정 단계의 구현을 다양화

```java
// AbstractClass 클래스
abstract class AbstractClass {
    // 템플릿 메서드
    public final void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
        concreteOperation();
    }

    // 추상 메서드
    protected abstract void primitiveOperation1();
    protected abstract void primitiveOperation2();

    // 구체 메서드
    private void concreteOperation() {
        System.out.println("Common operation");
    }
}

// ConcreteClass 클래스
class ConcreteClass extends AbstractClass {
    protected void primitiveOperation1() {
        System.out.println("ConcreteClass: primitiveOperation1");
    }

    protected void primitiveOperation2() {
        System.out.println("ConcreteClass: primitiveOperation2");
    }
}

// Main 클래스
public class Main {
    public static void main(String[] args) {
        AbstractClass abstractClass = new ConcreteClass();
        abstractClass.templateMethod();
    }
}
```

### 이터레이터(Iterator)

- `컬렉션의 요소들의 기본 표현을 노출하지 않고 그들을 하나씩 순회`할 수 있도록 하는 패턴
- 컬렉션 객체의 요소들에 접근하는 방법을 제공
- 내부 표현을 노출시키지 않고도 컬렉션의 요소들을 순차적으로 접근
- 다양한 컬렉션 구조에 대해 일관된 접근 방법을 제공

```java
interface Iterator {
    boolean hasNext();
    Object next();
}

class ConcreteIterator implements Iterator {
    private List<Object> items;
    private int position = 0;

    ConcreteIterator(List<Object> items) {
        this.items = items;
    }

    public boolean hasNext() {
        return position < items.size();
    }

    public Object next() {
        return items.get(position++);
    }
}

interface Aggregate {
    Iterator createIterator();
}

class ConcreteAggregate implements Aggregate {
    private List<Object> items = new ArrayList<>();

    public void addItem(Object item) {
        items.add(item);
    }

    public Iterator createIterator() {
        return new ConcreteIterator(items);
    }
}
```
# FINAL

이론적으로 디자인패턴에 아는 것보다 어떻게 실무에 적용하여 유지보수성을 올릴 수 있는지 고민해야한다. 작가가 여러명인 책은 어떻게 작성될까?
문체, 띄어쓰기, 인물간의 복선이 지켜질 수 있을까?  지키기는 매우 힘들것이다.  

이처럼 개발자는 프로젝트에 여러 개발자들과 함께 협업을 해야한다. 그렇기 때문에 디자인패턴, 코드 컨벤션들 통해 소스코드의 독해력을 향상시키는 것을 지향해야한다.

