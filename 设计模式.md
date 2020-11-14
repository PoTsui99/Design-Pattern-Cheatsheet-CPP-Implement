**XMLUtil.getbean():根据config.xml内容生成类**

```java
import javax.xml.parsers.*;
import org.w3c.dom.*;
import org.xml.sax.SAXException;

import java.io.*;

public class XMLUtil {
    //该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
    public static Object getBean() {
        try {
            //创建文档对象
            DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = dFactory.newDocumentBuilder();
            Document doc;
            doc = builder.parse(new File("config.xml"));

            //获取包含类名的文本节点
            NodeList nl = doc.getElementsByTagName("className");
            Node classNode = nl.item(0).getFirstChild();
            String cName = classNode.getNodeValue();

            //通过类名生成实例对象并将其返回
            Class c = Class.forName(cName);
            Object obj = c.newInstance();
            return obj;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```



**Abstract Factory**

```java

```



**Builder**

```java
// in Product.java
public class Product {
    private int partA;
    private int partB;
    public Product() {
    }
}

// in Builder.java
interface Builder{
    Product p = null;
    public void buildPartA();
    public void buildPartB();
    public Product getResult();
}

// in ConcreteBuilder.java
public class ConcreteBuilder implements Builder{
    @Override
    public void buildPartA() {
        // do something here
    }

    @Override
    public void buildPartB() {
        // do something here
    }

    @Override
    public Product getResult() {
        return p;
    }
}

// in Director.java
public class Director {
    private Builder b;
    public Director() {
    }
    public setBuilder(Builder b) {
        this.b = b;
    }
    public Product construct(){
        b.buildPartA();
        b.buildPartB();
        return b.getResult();
    }
}

```



**Factory**

```java
// 就是有几个Factory,Client想要哪个Product就要先有哪个ConcreteFactory,就这...
```



**Prototype**

```java
// 就是实现个clone()方法,有啥好写的
```



**Singleton**

```java
// in Singleton.java
public class Singleton {
    private static Singleton instance = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
}
```



**Adapter**

```java
// in Adapter.java
public class Adapter extends Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void request() {
        adaptee.specificRequest();
    }
}

// Adaptee是现成的题目给的,其中提供了所需要的specificRequest()方法
// Client中用Adapter的父类Target实例来调用request()方法
```



**Bridge**

```java
// in Abstraction.java
public abstract class Abstraction {
    protected Implementor impl;

    public void setImpl(Implementor impl) {
        this.impl = impl;
    }

    public abstract void operation();
}

// in RefinedAbstraction.java
public class RefinedAbstraction extends Abstraction {
    public void operation() {
        // do something here
        
        // 下面是实现部分的代码
        impl.operationImpl();
    }
}

// in Implementor.java
public interface Implementor {
    public void operationImpl();
}

// in ConcreteImplementor.java
public class ConcreteImplementor implements Implementor {
    public void operationImpl() {
        // 具体的实现内容
    }
}

// 实现代码需要调用setImpl,将具体实现类注入到Abstraction中
```



**Composite**

```java
// in Component.java
public abstract class Component {

    public abstract void add(Component c);

    public abstract void remove(Component c);

    public abstract Component getChild(int i);

    public abstract void operation();

}

// in Composite.java
import java.util.ArrayList;

public class Composite extends Component {
    private ArrayList<Component> list = new ArrayList<Component>();

    public void add(Component c) {
        list.add(c);
    }

    public void remove(Component c) {
        list.remove(c);
    }

    public Component getChild(int i) {
        return list.get(i);
    }

    public void operation() {
        for (var obj : list) {
            ((Component) obj).operation();
        }
    }
}

// in Leaf.java
public class Leaf extends Component {
    public void add(Component c) {
    }

    public void remove(Component c) {
    }

    public Component getChild(int i) {
        return null;
    }

    public void operation() {
        //实现代码
    }
}
```



**Decorator(*)**

```java
// in Component.java 把原来的类的接口拿了出来,"高层依赖抽象"
public class Component {
    public Component() {
    }

    public void operation() {
    }
}

// in ConcreteComponent.java
public class ConcreteComponent extends Component{
    public ConcreteComponent() {
        super();
    }

    @Override
    public void operation() {
        super.operation();
        System.out.println("This is the operation of ConcreteComponent.");
    }
}

// in Decorator.java 所有装饰器的父类
public class Decorator extends Component {
    private Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    public void operation() {
        component.operation();
    }
}

// in ConcreteDirector.java 增加了operation中的行为
public class ConcreteDecorator extends Decorator {
    public ConcreteDecorator(Component component) {
        super(component);
    }

    public void operation() {
        super.operation();
        addedBehavior();
    }

    public void addedBehavior() {
        System.out.println("Here is additional behavior.");
    }
}
```



**Facade**

```java
// 就是给你个方法可以调用,然后这个方法操作了一堆Subsystem,显然你啥也不知道里头具体发生了什么...
```



**Flyweight**

```java
// 就是我瞅瞅你要的对象我有没有,我要是有就给你,没有就生成一个,然后你有我有全都有了...
```



**Proxy**

```java
// in Subject.java
public interface Subject {
    public void request();
}

// in RealSubject.java 被代理的对象
public class RealSubject implements Subject {
    @Override
    public void request() {
        // 被代理之前的函数体
    }
}

// in Proxy.java 拥有RealSubject的实例,对其request()方法进行了扩充
public class Proxy implements Subject {
    private RealSubject realSubject = new RealSubject();

    public void preRequest() {
        // do something before request()
    }

    public void postRequest(){
        // do something after request()
    }

    @Override
    public void request() {
        preRequest();
        realSubject.request();
        postRequest();
    }
}
```



**Chain of Responsibility**

```java
// 相当于链表,一个个传递处理,需要注意的是不一定是链,还可以是树,看你怎么玩...
```



**Command**

```java
// 模式核心: 发出者Invoker拥有Command队列,挨个调用其中的execute()方法
// 同时注意到Command又拥有Receiver对象,所以execute()方法中会调用Receiver的action()方法
// 将Invoker和Receiver进行了解耦
// in Command.java
public abstract class Command {
    public abstract void execute();
}

// in ConcreteCommand.java
public class ConcreteCommand extends Command {
    private Receiver receiver;
    public void execute() {
        receiver.action();
    }
}

// in Receiver.java
public class Receiver {
    public void action() {
        //具体操作
    }
}

// in Invoker.java
public class Invoker {
    private Command command; // 也可以是放Command的一个容器,可迭代对象
    // 一种初始化方法
    public Invoker(Command command) {
        this.command = command;
    }
    // 又一种初始化方法
    public void setCommand(Command command) {
        this.command = command;
    }
    public void call() { // 调用Command的execute()方法,间接调用Receiver的方法
        command.execute();
    }
}
```



**Mediator**

```java
// Mediator和任意一个Colleague都是我中有你你中有我,相互通信,而Colleague之间没有直接关联
// 这个模式可以看作是一个关闭成员间私聊的微信群
```



Memento

```java
// Originator负责生成快照,Memento负责记录当前状态和进行快照的恢复,Caretaker就是个容器用于保存快照
```



**Observer**

```java
// in Subject
interface Subject {
    // 绑定观察者
    public void attach(Observer obs);
    // 解绑观察者
    public void detach(Observer obs);
    // 通知观察者
    public void notifyObserver();
}

// in Observer
interface Observer {
    public void update(Subject sub);
    public void setState();
}

// in ConcreteSubject
public class ConcreteSubject implements Subject{
    Observer obs = null;
    @Override
    public void attach(Observer obs) {
        this.obs = obs;
    }

    @Override
    public void detach(Observer obs) {
        if(this.obs.equals(obs))
            this.obs = null;
    }

    @Override
    public void notifyObserver() {
        if(obs != null)
            obs.setState();
    }
}

// in ConcrreteObserver
public class ConcreteObserver implements Observer{
    Subject sub = null;
    private int observerState = 0; // 反映了观察者自身的状态，是否屏蔽消息等
    @Override
    public void update(Subject sub) {
        // do something here to update Subject
    }

    @Override
    public void setState() {
        this.observerState = 1; // 模拟接收到消息后观察者的状态变化
    }
}
```



**State**

```java
// 当状态机理解,用的不多,用时再看就行:)
```



**Strategy**

```java
// in Stategy.java
interface Strategy {
    public void method();
}

// in ConcreteStrategy.java
class ConcreteStrategy implements Strategy {
    public void method() {
    }
}

// in Context.java
class Context {
    private Strategy s;

    public Context(Strategy s) {
        this.s = s;
    }

    public void method() {
        s.travelMethod();
    }
}

// in Client.java
public class Client {
    public static void main(String args[]) {
        Context c = new Context((Strategy) XMLUtil.getBean());
        c.method();
    }
}
```



