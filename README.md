
​享元（Flyweight、Cache）模式属于**结构型**模式的一种。
享元模式通过将对象的**内部状态**和**外部状态**分开，尽量共享内部状态来减少对象的创建。内部状态是对象可以共享的部分，而外部状态是对象特有的、依赖于环境的部分。


享元模式旨在有效**共享**对象，避免重复创建相同内容的对象，减少内存开销，让你能在有限的内存中载入更多对象。


享元类的状态只能由构造函数的参数进行**一次性初始化**，它不能对其他对象公开其设置器或公有成员变量。


享元模式只是一种**优化**。 


享元模式在Java标准库中有很多使用，比如**Byte、Integer**。


享元模式是通过**工厂方法**创建对象，在工厂方法内部，很可能返回缓存的实例，而不是新创建实例，从而实现**不可变**实例的复用。


在实际应用中，享元模式主要应用于**缓存**，直接返回内存中缓存的数据。


当需要创建大量**相似**的对象时，且这些对象的内部状态有很多相同部分时，可以使用享元模式。


享元模式通常有以下组成部分：


* Flyweight（享元类）：抽象享元类，负责存储共享的内部状态，定义接口以允许客户端访问内部状态。
* ConcreteFlyweight（具体享元类）：具体的享元类，负责实现共享对象的操作，存储内部状态。
* FlyweightFactory（享元工厂）：享元工厂类，用来管理和维护享元对象池。它确保享元对象的共享，避免重复创建相同对象。
* Client（客户端）：使用享元对象的代码，负责传递外部状态，并将共享对象传递给其他对象进行操作。


假如开发一个文字编辑器，我们使用享元模式将相同字符的显示对象共享。


1、享元类




```
public interface Flyweight {
    void display(String extrinsicState); // 外部状态，依赖客户端的具体状态
}

```

![](https://img2024.cnblogs.com/blog/1171560/202412/1171560-20241204231949918-734844350.gif "点击并拖拽以移动")
2、具体享元类




```
public class ConcreteFlyweight implements Flyweight {
    private String intrinsicState; // 内部状态
    
    public ConcreteFlyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    @Override
    public void display(String extrinsicState) {
        System.out.println("Displaying " + intrinsicState + " with external state " + extrinsicState);
    }
}

```

![](https://img2024.cnblogs.com/blog/1171560/202412/1171560-20241204231949918-734844350.gif "点击并拖拽以移动")
3、享元工厂




```
import java.util.HashMap;
import java.util.Map;

public class FlyweightFactory {
    private Map flyweights = new HashMap<>();

    public Flyweight getFlyweight(String intrinsicState) {
        // 享元工厂决定是否复用已有享元或者创建一个新的对象
        if (!flyweights.containsKey(intrinsicState)) {
            flyweights.put(intrinsicState, new ConcreteFlyweight(intrinsicState));
        }
        return flyweights.get(intrinsicState);
    }
}

```

![](https://img2024.cnblogs.com/blog/1171560/202412/1171560-20241204231949918-734844350.gif "点击并拖拽以移动")
4、客户端




```
public class Client {
    public static void main(String[] args) {
        FlyweightFactory factory = new FlyweightFactory();
        
        Flyweight f1 = factory.getFlyweight("A");
        f1.display("First Instance");

        Flyweight f2 = factory.getFlyweight("A");
        f2.display("Second Instance");

        Flyweight f3 = factory.getFlyweight("B");
        f3.display("Third Instance");
    }
}

```

![](https://img2024.cnblogs.com/blog/1171560/202412/1171560-20241204231949918-734844350.gif "点击并拖拽以移动")
享元模式的**优缺点**。


优点：


* **节省内存**：通过共享相同的对象，减少内存占用。
* **提高性能**：减少对象的创建，减少内存垃圾回收的频率，从而提高性能。
* **降低资源消耗**：共享对象减少了系统的内存消耗，特别是在系统需要创建大量相似对象时。


缺点：


* **增加复杂度**：享元模式的实现较为复杂，需要设计共享对象池。


享元模式的设计思想是尽量复用已创建的对象，常用于工厂方法内部的优化。


可能很多同学觉得享元模式与单例模式很像。的确，但是他们也有区别。单例模式是**不允许创建**新实例。享元模式是**要求实例不变**，所以把 "应该创建一个新实例" 的操作优化为 "直接返回一个缓存的实例"。


单例对象可以是**可变**的。享元对象是**不可变**的。


纸上得来终觉浅，绝知此事要躬行。\-\- 烟沙九洲


​

 本博客参考[milou云加速器官网](https://jiechuangmoxing.com)。转载请注明出处！
