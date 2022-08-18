# COMP2511
COMP2511 Notes for review only.
Resources from Lecture Slides and Zhihu(as reference)
# Code Smell

链接：https://zhuanlan.zhihu.com/p/373380297
来源：知乎

1. Duplicated Code: 
    - Same code structure in more than one place
    Same expression in two sibling classes
        
        重复代码就是**不同地点，有着相同的程序结构**。一般是因为需求迭代比较快，开发小伙伴担心影响已有功能，就复制粘贴造成的。重复代码**很难维护**的，如果你要修改其中一段的代码逻辑，就需要修改多次，很可能出现遗漏的情况
        
        ```java
        class A {
            public void [method1](https://www.zhihu.com/search?q=method1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22373380297%22%7D)() {
                doSomething1
                doSomething2
                doSomething3
            }
            public void method2() {
                doSomething1
                doSomething2
                doSomething4
            }
        }
        ```
        
    - **TODO:** Extract Method
        
        ```java
        class A {
            public void method1() {
                commonMethod();
                doSomething3
            }
            public void method2() {
                commonMethod();
                doSomething4
            }
            
            public void commonMethod(){
               doSomething1
               doSomething2
            }
        }
        
        ```
        
        如果是两个毫不相关的类出现重复代码，可以使用**Extract Class**
        将重复代码提炼到一个类中。这个新类可以是一个普通类，也可以是一个工具类，看具体业务怎么划分吧。
        
2. Long Method
    - Calling a query method and passing its results as the parameters of another method, while that method could call the query directly.
    - too many parameters to remember
    - Bad for readability, usability and maintenance.
        
        长函数是指一个函数方法几百行甚至上千行，可读性大大降低，不便于理解。
        
        ```java
        
        public class Test {
            private String name;
            private Vector<Order> orders = new Vector<Order>();
        
            public void printOwing() {
                //print banner
                System.out.println("****************");
                System.out.println("*****customer Owes *****");
                System.out.println("****************");
        
                //calculate totalAmount
                Enumeration env = orders.elements();
                double totalAmount = 0.0;
                while (env.hasMoreElements()) {
                    Order order = (Order) env.nextElement();
                    totalAmount += order.getAmout();
                }
        
                //print details
                System.out.println("name:" + name);
                System.out.println("amount:" + totalAmount);
                ......
            }
        }
        ```
        
    - Solution: Extract Method
    try placing a query call inside the method body via replace
        
        ```java
        作者：捡田螺的小男孩
        链接：https://zhuanlan.zhihu.com/p/373380297
        来源：知乎
        著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
        
        public class Test {
            private String name;
            private Vector<Order> orders = new Vector<Order>();
        
            public void printOwing() {
        
                //print banner
                printBanner();
                //calculate totalAmount
                double totalAmount = getTotalAmount();
                //print details
                printDetail(totalAmount);
            }
        
            void printBanner(){
                System.out.println("****************");
                System.out.println("*****customer Owes *****");
                System.out.println("****************");
            }
        
            double getTotalAmount(){
                Enumeration env = orders.elements();
                double totalAmount = 0.0;
                while (env.hasMoreElements()) {
                    Order order = (Order) env.nextElement();
                    totalAmount += order.getAmout();
                }
                return totalAmount;
            }
        
            void printDetail(double totalAmount){
                System.out.println("name:" + name);
                System.out.println("amount:" + totalAmount);
            }
            
        }
        ```
        
3. Large Class:
    - When a class is trying to do too much, it often shows up as too many instance variables
    Usually violates Single Responsibility Principle
        
        一个类做太多事情，维护了太多功能，[可读性](https://www.zhihu.com/search?q=%E5%8F%AF%E8%AF%BB%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22373380297%22%7D)变差，性能也会下降。举个例子，订单相关的功能你放到一个类A里面，商品库存相关的也放在类A里面，积分相关的还放在类A里面
        
    - **TODO:** Extract Class or Extract Sub-Class
        
        试想一下，乱七八糟的代码块都往一个类里面塞，还谈啥可读性。应该按单一职责，使用`Extract Class`把代码划分开，
        
        ```java
        Class Order{
          public void printOrder(){
           System.out.println("订单");
          }
        }
        
        Class Goods{
           public void printGoods(){
           System.out.println("商品");
          }
        }
         
        Class Points{   
          public void printPoints(){
           System.out.println("积分");
          }
         }
        }
        
        作者：捡田螺的小男孩
        链接：https://zhuanlan.zhihu.com/p/373380297
        来源：知乎
        著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
        ```
        
4. Long Parameter List
    - long parameter list
        
        方法参数数量过多的话，可读性很差。如果有多个重载方法，参数很多的话，有时候你都不知道调哪个呢。并且，如果参数很多，做新老接口兼容处理也比较麻烦
        
        ```java
        public void getUserInfo（String name,String age,String sex,String mobile){
          // do something ...
        }
        ```
        
    - Solution:
        
        将参数封装成结构或者类，方法
        
5. Divergent Change
    - When one class is commonly changed in different ways for different reasons.
    One class is changed in different ways for different reasons
        
        对程序进行维护时, **如果添加修改组件, 要同时修改一个类中的多个方法**, 那么这就是 Divergent Change。举个汽车的例子，某个汽车厂商生产三种品牌的汽车：BMW、Benz和LaoSiLaiSi，每种品牌又可以选择燃油、纯电和混合动力。 新增一种品牌[新能源](https://www.zhihu.com/search?q=%E6%96%B0%E8%83%BD%E6%BA%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22373380297%22%7D)
        电车，然后它的启动引擎是核动力呢，那么就需要修改Car类的`start` 和`getBrand` 方法啦
        
        ```java
        public class Car {
        
            private String name;
        
            void start(Engine engine) {
                if ("HybridEngine".equals(engine.getName())) {
                    System.out.println("Start Hybrid Engine...");
                } else if ("GasolineEngine".equals(engine.getName())) {
                    System.out.println("Start Gasoline Engine...");
                } else if ("ElectricEngine".equals(engine.getName())) {
                    System.out.println("Start Electric Engine");
                }
            }
        
            void drive(Engine engine,Car car) {
                this.start(engine);
                System.out.println("Drive " + getBrand(car) + " car...");
            }
        
            String getBrand(Car car) {
                if ("Baoma".equals(car.getName())) {
                    return "BMW";
                } else if ("BenChi".equals(car.getName())) {
                    return "Benz";
                } else if ("LaoSiLaiSi".equals(car.getName())) {
                    return "LaoSiLaiSi";
                }
                return null;
            }
         }
        ```
        
    - Solution: Any change to handle a variation should change a single class, and all the typing in the new class should express the variation
    - To clean this up you identify everything that changes for a particular cause and use `Extract Class` to put them all together
6. Shotgun Surgery
    - The opposite of divergent change, when you have to make a lot of little changes to a lot of different classes
        
        当你实现某个小功能时，你需要在很多不同的类做出小修改。这就是**Shotgun Surgery（散弹式修改**。它跟**发散式变化(Divergent Change)** 的区别就是，它指的是同时对多个类进行单一的修改，发散式变化指在一个类中修改多处。
        
        多个类使用了`db.mysql.url` 这个变量，如果将来需要切换`mysql` 到别的数据库，如`Oracle` ，那就需要修改多个类的这个变量
        
        ```java
        public class DbAUtils {
            @Value("${db.mysql.url}")
            private String mysqlDbUrl;
            ...
        }
        
        public class DbBUtils {
            @Value("${db.mysql.url}")
            private String mysqlDbUrl;
            ...
        }
        ```
        
    - Move Method And Move Field
        
        可以使用 Move Method （搬移函数）和 Move Field （搬移字段）把所有需要修改的代码放进同一个类，如果没有合适的类，就去new一个
        
        ```java
        public class DbUtils {
            @Value("${db.mysql.url}")
            private String mysqlDbUrl;
            ...
        }
        ```
        
    - Use `Move Method` or `Move Field` to put all the changes into a single class
    - Often you can use `inline class` to bring a whole bunch of behaviour together.
7. Feature Envy
    - A method that is more interested in a class other than the one it actually is
    - invokes several methods on another object to calculate some value
    - Creates unnecessary coupling between the classes
        
        某个函数为了计算某个值，从另一个对象那里调用几乎半打的取值函数。通俗点讲，就是一个函数使用了大量其他类的成员，有人称之为**红杏出墙的函数**
        
        如何解决呢？在这种情况下，你可以考虑将这个方法移动到它使用的那个类中。例如，要将 `getFullPhoneNumber()` 从 `User`
         类移动到`Phone` 类中，因为它调用了`Phone` 类的很多方法
        
        ```java
        public class User{
         private Phone phone;
          public User(Phone phone){
                this.phone = phone;
            }
            public void getFullPhoneNumber(Phone phone){
                System.out.println("areaCode:" + phone.getAreaCode());
                System.out.println("prefix：" + phone.getPrefix());
                System.out.println("number：" + phone.getNumber());
            }
        }
        ```
        
    - Solution: A goal of OO design is to put the methods with is associated data
        - So the method must moved to the relevant class via `Move Method`
        - If only part of a method accesses the data of another object, use `Extract Method` followed by `Move Method` to move the part in question
        - If a method uses functions from several other classes, first determine which class contains most of the data use. Then place the method in this class along with the other data.
8. Data Clumps/ Classes
    - Different parts of the code contain identical groups of variables.
    Can lead to code smell `Long parameter List`
        
        数据项就像小孩子，喜欢成群结队地呆在一块。如果一些数据项总是一起出现的，并且一起出现更有意义的，就可以考虑，按数据的业务含义来封装成数据对象
        
        ```java
        public class User {
        
            private String firstName;
            private String lastName;
        
            private String province;
            private String city;
            private String area;
            private String street;
        }
        
        ```
        
        正例
        
        ```java
        public class User {
        
            private UserName username;
            private Adress adress;
        }
        
        class UserName{
            private String firstName;
            private String lastName;
        }
        class Address{
            private String province;
            private String city;
            private String area;
            private String street;
        }
        
        ```
        
    - Solution: Move the behaviour to the data class via `Move Method`
        - If repeating data comprises the fields of a class, use Extract Class to move the fields to their own class
        - If the same data clumps are passed in the parameters of methods, use `Introduce Parameter Object` to set them off as a class
        - If some of data is passed to other methods, think about passing the entire data object to the method instead of just individual fields `Preserver Whole Object` will help with this.
        
9. Primitive Obsession
    - 
        
        多数编程环境都有两种数据类型，**结构类型和基本类型**。这里的基本类型，如果指Java语言的话，不仅仅包括那八大基本类型哈，也包括String等。如果是经常一起出现的基本类型，可以考虑把它们封装成对象。我个人觉得它有点像**Data Clumps（数据泥团）**
        
10. Switch Statements
    - Switch statements are bad from an OO design point of view
        
        这里的Switch语句，不仅包括`Switch`相关的语句，也包括多层`[if...else](https://www.zhihu.com/search?q=if...else&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22373380297%22%7D)`的语句哈。很多时候，switch语句的问题就在于重复，如果你为它添加一个新的case语句，就必须找到所有的switch语句并且修改它们
        
        ```java
        String medalType = "guest";
            if ("guest".equals(medalType)) {
                System.out.println("嘉宾勋章");
             } else if ("vip".equals(medalType)) {
                System.out.println("会员勋章");
            } else if ("guard".equals(medalType)) {
                System.out.println("守护勋章");
            }
        
        ```
        
    - Solution: Replace switch statements with a polymorphic solution based on Strategy Pattern applying a series of refactoring techniques(`Extract Method`, `Move Method`, `Extract Interface`)
        
        ```java
        
        //勋章接口
        public interface IMedalService {
            void showMedal();
        }
        
        //守护勋章策略实现类
        public class GuardMedalServiceImpl implements IMedalService {
            @Override
            public void showMedal() {
                System.out.println("展示守护勋章");
            }
        }
        //嘉宾勋章策略实现类
        public class GuestMedalServiceImpl implements IMedalService {
            @Override
            public void showMedal() {
                System.out.println("嘉宾勋章");
            }
        }
        
        //勋章服务工厂类
        public class MedalServicesFactory {
        
            private static final Map<String, IMedalService> map = new HashMap<>();
            static {
                map.put("guard", new GuardMedalServiceImpl());
                map.put("vip", new VipMedalServiceImpl());
                map.put("guest", new GuestMedalServiceImpl());
            }
            public static IMedalService getMedalService(String medalType) {
                return map.get(medalType);
            }
        }
        ```
        
11. Parallel Inheritance Hierarchies
    - 
        
        平行继承体系 其实算是`Shotgun Surgery`的特殊情况啦。当你为A类的一个子类Ax，也必须为另一个类B相应的增加一个子类Bx
        
        ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled.png)
        
    - Solution:
        
        遇到这种情况，就要消除两个继承体系之间的引用，有一个类是可以去掉继承关系的。
        
    
12. Lazy Class
    - Classes that aren’t doing much to justify their existence(maintenance overhead)
    *Subclasses without any overridden methods or additional fields can be lazy classes*
        
        把这些不再重要的类里面的逻辑，合并到相关类，删掉旧的。一个比较常见的场景就是，假设系统已经有日期工具类`DateUtils`
        ，有些小伙伴在开发中，需要用到日期转化等，不管三七二十一，又自己实现一个新的日期工具类
        
    
    ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%201.png)
    
    - **Solution:**
        - Move the date from lazy class PostCode to the class Address
        - Delete the lazy class
13. Speculative Generality
    - 
        
        尽量避免过度设计的代码。例如：
        
        - 只有一个if else，那就不需要班门弄斧使用多态;
        - 如果某个抽象类没有什么太大的作用，就运用`Collapse Hierarchy`（折叠继承体系）```
        - 如果函数的某些参数没用上，就移除。
14. Temporary Field
    - 
        
        某个实例变量仅为某种特定情况而定而设，这样的代码就让人不易理解，我们称之为 `Temporary Field(令人迷惑的临时字段)`. 
        
        临时字段`excessMinutesCharge`多余
        
        ```java
        
        public class PhoneAccount {
        
            private double excessMinutesCharge;
            private static final double RATE = 8.0;
        
            public double computeBill(int minutesUsed, int includedMinutes) {
                excessMinutesCharge = 0.0;
                int excessMinutes = minutesUsed - includedMinutes;
                if (excessMinutes >= 1) {
                    excessMinutesCharge = excessMinutes * RATE;
                }
                return excessMinutesCharge;
            }
        	
            public double chargeForExcessMinutes(int minutesUsed, int includedMinutes) {
                computeBill(minutesUsed, includedMinutes);
                return excessMinutesCharge;
            }
        }
        ```
        
15. Message Chains
    - 
        
        当你看到用户向一个对象请求另一个对象，然后再向后者请求另一个对象，然后再请求另一个对象...这就是消息链。实际代码中，你看到的可能是一长串`getThis()`或一长串临时变量。反例如下：
        
        ```java
        A.getB().getC().getD().getTianLuoBoy().getData();
        ```
        
    - 
        
        A想要获取需要的数据时，必须要知道B，又必须知道C,又必须知道D...其实A需要知道得太多啦，回头想下**封装性**，嘻嘻。其实可以通过拆函数或者移动函数解决，比如由B作为代理，搞个函数直接返回A需要数据。
        
16. Middle Man
    - 
        
        对象的基本特征之一就是封装，即对外部世界隐藏其内部细节。封装往往伴随委托，过度运用委托就不好：某个类接口有一半的函数都委托给其他类。可以使用`Remove Middle Man`
        优化。反例如下：
        
        其实，A可以直接通过C去获取C，而不需要通过B去获取
        
        ```java
        A.B.getC(){
           return C.getC();
        }
        ```
        
17. Inappropriate Intimacy
    - 
        
        如果两个类过于亲密，过分狎昵，你中有我，我中有你，两个类彼此使用对方的私有的东西，就是一种坏代码味道。我们称之为`Inappropriate Intimacy`
        
        ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%202.png)
        
        建议尽量把有关联的方法或属性抽离出来，放到公共类，以减少关联
        
        ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%203.png)
        
18. Alternative Classes with Different Interfaces
    - 
        
        A类的接口a，和B类的接口b，做的的是相同一件事，或者类似的事情。我们就把A和B叫做异曲同工的类。
        
        ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%204.png)
        
19. Incomplete Library Class
    - 
        
        大多数对象只要够用就好，如果类库构造得不够好，我们不可能修改其中的类使它完成我们希望完成的工作。可以酱紫：**包一层函数或包成新的类**
        
20. Data Class
    - 
        
        它们拥有一些字段，以及用于访问(读写)这些字段的函数。这些类很简单，仅有公共成员变量，或简单操作的函数
        
    - 
        
        如果拥有public字段-> `Encapsulate Field`如果这些类内含容器类的字段，应该检查它们是不是得到了恰当地封装-> `Encapsulate Collection`封装起来对于不该被其他类修改的字段-> `Remove Setting Method`->找出取值/设置函数被其他类运用的地点-> `Move Method` 把这些调用行为搬移到`Data Class`来。如果无法搬移整个函数，就运用`Extract Method`产生一个可被搬移的函数->`Hide Method`把这些取值/设置函数隐藏起来
        
21. Refused Bequest
    - A subclass uses only some of the methods and properties inherited from its parents
    The unneeded methods may simply go unused or be redefined and give off exceptions
    Often caused by creating inheritance between classes only by the desire to reuse the code in a super-class
        
        子类应该继承父类的数据和函数。子类继承得到所有函数和数据，却只使用了几个，那就是**继承体系设计错误**，需要优化。
        
        ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%205.png)
        
    - **Solution**: If inheritance is appropriate, but super class contains field and methods not applicable to all classes, then consider the following options
    - Create a new subclass
    - Apply `Push Down Field` to move field relevant only to subclass from superclass
    - Apply `Push Down Method` to move behaviour from super class to sub class, as behaviour makes sense only to sub class
    - Often, you may apply an `Extract Sub-Class Class` to combine the above step.
        
        需要为这个子类新建一个兄弟类->`Push Down Method`和`Push Down Field`把所有用不到的函数下推给兄弟类，这样一来，超类就只持有所有子类共享的东西。所有超类都应该是抽象的。如果子类复用了超类的实现，又不愿意支持超类的接口，可以不以为然。但是不能胡乱修改继承体系->`Replace Inheritance with Delegation`(用委派替换继承).
        
    
    ![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%206.png)
    
22. Comments
    - 
        
        这个点不是说代码不建议写注释哦，而是，建议大家**避免用注释解释代码，避免过多的注释**。这些都是常见注释的坏味道：
        
        - 多余的解释
        - 日志式注释
        - 用注释解释变量等
        - ...
        

# Refactor Techniques

- Move Field/Method
- Extract Class/Inline Class
- Inline Method/Temporary Variable
- Replace Temp with Query
- Replace Method with Method Object
- Rename Method
- Substitute Algorithm
- Introduce Parameter Object
- Preserve Whole Object
- Extract Sub Class/Super Class/ Interface
- Extract Method
- Pull Up Field/Method/Constructor Body
- Form Template Method
- Replace Inheritance with Delegation
- Replace Conditional with Polymorphism
    
    

# Design Principles

## Single Responsibility Principle (SOLID)

> A class should only have a single responsibility
> 

```java
public class Phone {
	// call others
	// ! violates single responsibility principle
	public void dial(String phoneNumber);
	public void hangup();
	// msg others
	// ! violates single responsibility principle
	public void chat(Object o);
}
// --------------------- 拆分
public class Caller {
	public void dial(String phoneNumber);
	public void hangup();
}
public class Msger {
	public void chat(Object o);
}
```

## Open-closed principle (SOLID)

> Software entities should be open for extension, but close for modification
> 

```java
public class Greeter {
    public void greet() {
        System.out.println("Hello, World!");
    }
}
public class FrenchGreeter extends Greeter {
    @Override
    public void greet() {
        System.out.println("Bonjour!");
    }
}
// ----------- Abstraction and Composition
public interface GreeterService {
    void greet();
}
public class Greeter implements GreeterService{
    public void greet() {
        target.println("Hello, World!");
    }
}
public class FrenchGreeter implements GreeterService {
    @Override
    public void greet() {
        System.out.println("Bonjour!");
    }
}
```

## LSP(Liskov Substitution Principle) (SOLID)

> Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program

Subtypes must be substitutable for their base types
> 
> 
> 如果对每一个类型为S的对象o1，都有类型为T的对象o2，使得以T定义的所有程序P在所有的对象o1都代替o2时，程序P的行为没有发生变化，那么类型S是类型T的子类型。
> 
> 通俗点讲，就是只要父类能出现的地方，子类就可以出现，而且替换为子类也不会产生任何错误或异常。
> 

LSP is about well-designed inheritance

- Solution:
    - Delegation - delegate the functionality to another class
    - Composition- reuse behaviour using one or more classes with composition

***Design Principle**: Favour composition over inheritance*

*If your favour delegation, composition over inheritance, your software will be more flexible, easier to maintain, extend*

```java
class Dog {
  String getNoise() {
    return "WOOF WOOF!!!"
  }
}
class FrenchBulldog extends Dog {
  String getNoise() {
    return "mrfff!"
  }
}
// CANNOT
class Hotdog extends Dog implements Edible {
  String getNoise() {
    throw new NotImplementedException("I am actually a sausage");
  }
  String eat() {
    return "Delicious";
  }
}
```

****覆盖或实现父类的方法时，输入参数可以被放大****

****覆盖或实现父类的方法时，输出结果可以被缩小****

## Interface segregation principle(SOLID)

> Many client-specific interfaces are between than one general-purpose interface
The Interface Segregation Principle (ISP) states that **a client should not be exposed to methods it doesn’t need**. Declaring methods in an interface that the client doesn’t need pollutes the interface and leads to a “bulky” or “fat” interface.
> 

```java
interface OrderService {
    void orderBurger(int quantity);
    void orderFries(int fries);
    void orderCombo(int quantity, int fries);
}
class BurgerOrderService implements OrderService {
    @Override
    public void orderBurger(int quantity) {
        System.out.println("Received order of "+quantity+" burgers");
    }
    @Override
    public void orderFries(int fries) {
        throw new UnsupportedOperationException("No fries in burger only order");
    }
    @Override
    public void orderCombo(int quantity, int fries) {
        throw new UnsupportedOperationException("No combo in burger only order");
    }
}
// ---------------------------------
interface BurgerOrderService {
    void orderBurger(int quantity);
}
class OrderServiceObjectAdapter implements BurgerOrderService {
    private OrderService adaptee;
    public OrderServiceObjectAdapter(OrderService adaptee) {
        super();
        this.adaptee = adaptee;
    }

    @Override
    public void orderBurger(int quantity) {
        adaptee.orderBurger(quantity);
    }
}
// OR
interface OrderService {
    void submitOrder(Order order);
}
class OrderServiceObjectAdapter implements OrderService {
 
    @Override
    public void submitOrder(Order order) {

    }
}

```

## Dependency inversion principle(SOLID)

> One should “depend upon abstractions, [Not] concretions
> 
1. 模块间的依赖通过抽象发生，实现类之间不直接发生依赖关系，其依赖关系是通过接口或抽象类产生的；
2. 接口或抽象类不依赖于实现类；
3. 实现类依赖接口或抽象类

```java
public class Benz{ 
	public void run();
}
public class Driver {
	public void drive(Benz benz);
}
public class Client {
	public static void main(String[] args) {
		Driver driver = new Driver();
		driver.drive(new Benz());
	}
}
// --------------------
public interface Car {
	void run();
}
public interface Driver {
	void drive();
}
public class Benz implements Car{ 
	public void run();
}
public class HumanDriver implements Driver {
	public void drive(Car car);
}
public class Client {
	public static void main(String[] args) {
		Driver driver = new Driver();
		driver.drive(new Benz());
	}
}

```

## The principle of Least Knowledge or Law of Demeter

- Classes should know about and interact with as few classes as possible
- Reduce the interaction between objects to just a few close ‘friend’
- These friends are “immediate friends” or “local objects”
- Helps us to design “loosely coupled” systems so that changes to one part of the system does not cascade to other parts of the system
- The principle limits interaction through a set of rules

A method in a object should only invoke methods of 

- The object itself
- The object passed in as a parameter to the method
- Objects instantiated within the method
- Any component objects
- And not those of objects returned by a method

Don’t dig deep inside your friends for friends of friends of friends and get in deep conversations with them. 

> i.e. o.get(name).get(thing).remove(node)
> 

### Rules

1. A method **M** in an object **O** can call on any other method within **O** itself
2. A method **M** in an object **O** can call on any method of parameters passed to the method **M**
3. A method **M** can call a method **N** of another object, if that object is instantiated within the method **M**
4. Any method **M** in an object **O** can call on any methods of any type of object that is a direct component of **O**

# Testing

## Unit Tests: JUnit

1. Types of testing
    - Object Oriented Design document describes responsibilities of classes and method (APIs) → Unit Testing
    - System Design Document → Integration Testing
    - Requirements Analysis Document → System Testing
    - Client Expectation → Acceptance Testing
2. Unit Testing is also useful for refactoring tasks
3. Basic Unit Terminology
    - Test Case - class containing test methods
    - Test Method - a method that executes the test code
    - Assert - asserts or assert statements check an expected result versus the actual result
    - Test Suites - Collection of server Test Cases

## Test Design

### Test-Driven Development (TDD)

- Every iteration in the software development process must be preceded with a plan to properly verify(test) that the developed software meets the requirements
- A software developer must **not** create a software artifact and later think of how to test it
- Testing is an essential integral part of developing a software solution. It must not be considered as an afterthought
- Testing must be setup, based on the requirement specifications, **before** you start implementing your solution

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%207.png)

## Input Space Coverage

- Testing must not be conducted **haphazardly** by trial-and-error
- Testing must be conducted **systematically**, with a well though out `testing plan`.
- The aim should be consider a possible input space and cover it as mush as possible
- Consider **borderline** cases, often called `boundary testing`
- For multiple input values, consider possible input combinations, prioritise them and consider many as possible.

## Code coverage

- Code coverage is useful metric that can help you assess the quality of your test suite
- The common metric in most coverage reports include:
    - Function coverage: how many of the functions defined have been called
    - Statement coverage: how many of the statements in the program have been executed
    - Branches coverage: how many of the branches of the control structures(if statement for instance) have been executed
    - Condition coverage: how many of the boolean sub-expressions have been tested for a true and a false value
    - Line coverage: how many of lines of source code have been tested.

## Basic Test Template

1. Set up Precondition
2. Act
3. Verify Post condition

Normally, each test should run independently, order of execution should not be important

# Design Pattern Overall

作者：我吃印度飞饼

链接：https://zhuanlan.zhihu.com/p/86408779

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

| Patterns in programming | - Exceptions
- Generics Programming & Collections
- The Functional Paradigm
- Iterator Pattern |
| --- | --- |
| Behavioural Design Patterns | - Strategy Pattern
- State Pattern
- Observer Pattern
- Template Pattern
- Visitor Pattern
- Event Driven & Asynchronous Design
- Command & Facade Patterns (Bonus) |
| Structural Design Patterns | - Composite Pattern
- Decorator Pattern
- Adapter Pattern |
| Creational Design Patterns | - Factory Pattern
- Singleton Pattern |

## OO Basics

- Abstraction
- Encapsulation
- Inheritance
- Polymorphism

## OO Principles

- Principle of least knowledge
- Encapsulate what varies
- Favour composition over inheritance
- Program to an interface, not an implementation

# **Patterns in programming**

## Exceptions

- An `exception` is an event, which occurs during the execution of a program, that disrupts the normal flow of the program’s instructions.
- When error occurs, an exception object is created and given to the runtime system, this is called **throwing** an exception

### Exceptions in Java

kinds of Exceptions

- Checked exception(IOException, SQLException)
- Error(VirtualMachineError, OutOfMemoryError)
- Runtime exception(ArrayIndexOutOfBoundsExceptions)

### Checked vs. Unchecked Exceptions

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%208.png)

## Generic Programming & Collections

Generics enable types to be parameters when defining: classes, interface and methods.

### benefit

- removes casting and offers stringer type checks at compile time

```java
// without Generics
List list = new ArrayList()
list.add("Hello")
String s = (String) list.get(0)
```

```java
// with Generics
List<String> list = new ArrayList<>()
list.add("Hello")
String s = list.get(0)
```

### Usage example

1. A generic type is a generic class or interface that is parameterized over type.
    
    ```java
    Box<Intger> box = new Box<Integer>();
    // OR
    Box<Integer> box2 = new Box<>();
    ```
    
    ```java
    Pair<String, Integer> p1 = new OrderedPair<String, Interger>("string", 8);
    Pair<String, Box<String>> p2 = new OrderedPair<>("string", Box<>());
    ```
    
2. Generic methods are methods that introduce their own type parameters
    
    ```java
    public class Util {
    	public static <K, V> boolean compare(Pair<K, V> a, Pair<K, V> b) {
    			// do something
    	} 
    }
    Pair<Integer, Integer> a = new Pair<>(1,2)
    Pair<Integer, Integer> b = new Pair<>(2,3)
    Util.<Integer, Integer>compare(a, b);
    // OR
    Util.compare(a,b);
    
    ```
    
3. There may be times when you want to restrict the types that can be used as type arguments in a parameterized type

```java
Public <U extends Number> void inspect(U u) {
			System.out.println(u.getClass().getName());
}
// Benefits
// Look at this case
public static <T> int count(T[] array, T ele) {
		//....
		for (T e: array) {
				if (e > ele) { DO SOMETHING}
				// ERROR!
		}
}
// AND this case
public static <T extends Comparable<T>> int count(T[] array, T ele) {
		//....
		for (T e: array) {
				if (e.compareTo(ele)) { DO SOMETHING}
				// PASSED !
		}
}

```

1. the question mark(?) called the wildcard, represents an unknown type. The `Upper bounded wildcard`, <? extends Foo>, where Foo is any type, **matches Foo and any subtype of Foo.**

```java
public static void process(List<? extends Foo> list { 
		DO SOMETHING
{
// OR
public static void process(List<?> list { 
		Object ele = list.get(0)
		DO SOMETHING
{
```

1. `A lower bounded wildcard` is expressed using the wildcard character, following the super keyword. **matches Foo and any supertype of Foo**

```java
public static void process(List<? super Foo> list { 
		DO SOMETHING
{
```

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%209.png)

### Collection

![Nothing important to review.](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2010.png)

Nothing important to review.

[Slides](https://gitlab.cse.unsw.edu.au/COMP2511/22T2/content/-/blob/master/lecture-slides/week03/pdfs/GenericsCollectionsInJava.pdf)

## The Functional Paradigm

### Lambda Expressions

Lambda expressions allow us to

- easily define anonymous methods
- treat code as data
- and pass functionality as method argument

### Usage of Lambda Expressions

***Method References***

- A static method (ClassName::methName)
- An instance method of a particular object(instanceRef::methName)
- A class constructor reference(ClassName::new)

e.g. 

```java
strList.stream()
				.filter(String::isEmpty)
				.collect(Collectors.toList());
```

***Function shapes***

- Function T → R
- Consumer T → void
- Predicate t → boolean
- Supplier nilary function to R ///没见过， 我也不会
- BinaryOperator → T → T
- Optional  [LOOK_THIS_IF_NEEDED](https://www.baeldung.com/java-optional)

e.g.

```java
Function<String, Integer) func = x -> x.length();
Consumer<Integer> func2 = x -> x.valueOf(x.intValue() - 1);
Predicate<Shape> func3 = shape -> shape.isCircle();
BinaryOperator<Box, Box> func4 = (a, b) -> a.putIn(b);
```

### Stream

```java
final String SOMETHINGS = "SOMETHINGS"
double ave = list.stream()
		.filter(p -> p.equals(SOMETHINGS))
		.mapToInt(Integer::valueOf)    // -> IntStream
		.average()
		.getAsDouble();
```

[Slides](https://gitlab.cse.unsw.edu.au/COMP2511/22T2/content/-/blob/master/lecture-slides/week04/pdfs/JavaLambdaAggregateOps.pdf)

## Iterator Pattern

- An iterator is an object that enables a programmer to traverse a container
- Allows us to access the contents of a data structure
- Iterators vs Iterables
- An iterable is an object that can be iterated over
- All iterators are iterable , but not all iterables are iterators

```java
// Iterator Design
public class ArrayListIterator<T> implements Iterator<T> {
	ArrayList<T> items;
	int position = 0;
 
	public ArrayListIterator(ArrayList<T> items) {
		this.items = items;
	}
 
	public T next() {
		T object = items.get(position);
		position = position + 1;
		return object;
	}
 
	public boolean hasNext() {
		if (position >= items.size()) {
			return false;
		} else {
			return true;
		}
	}
}
```

```java
// Callers
public class PancakeHouseMenu implements Menu {
	ArrayList<MenuItem> menuItems;
 
	public PancakeHouseMenu() {
		menuItems = new ArrayList<MenuItem>();
    
	}
	public Iterator<MenuItem> createIterator() {
		return new PancakeHouseMenuIterator(menuItems);
	}

}

public class Waitress {
	PancakeHouseMenu pancakeHouseMenu;
	DinerMenu dinerMenu;
 
	public Waitress(PancakeHouseMenu pancakeHouseMenu, DinerMenu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}
 
	public void printMenu() {
		Iterator<MenuItem> pancakeIterator = pancakeHouseMenu.createIterator();
		
	}
 
}
```

- In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled
    
    迭代器模式就是分离了集合对象的遍历行为，抽象出一个迭代器类来负责，这样既可以做到不暴露集合的内部结构，又可让外部代码透明地访问集合内部的数据。
    

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2011.png)

***各元素作用***

Aggregate:聚集抽象类。

Iterator:迭代抽象类，用于定义 得到开始对象，得到下一个对象，判断是否到结尾，当前对象等抽象。方法，是一个统一接口。

ConcreteAggregate:具体聚集类 继承Aggregate。

ConcreteIterator:具体的迭代器类，继承Iterator，实现开始，下一个，是否结尾，当前对象等方法。

***优点***

1. 它支持以不同的方式遍历一个聚合对象。 可以供多种遍历方式，比如对于有序列表，可以正向遍历也可以倒序遍历，只要迭代器实现得好
2. 迭代器简化了聚合类。 
3. 在迭代器模式中，增加新的聚合类和迭代器类都很方便，无须修改原有代码。
4. 封装性好，用户只需要得到迭代器就可以遍历，而对于遍历算法则不用去关心

***缺点***

1. 由于迭代器模式将存储数据和遍历数据的职责分离，增加新的聚合类需要对应增加新的迭代器类，类的个数成对增加，这在一定程度上增加了系统的复杂性。
2. 对于比较简单的遍历（数组或者有序列表），使用迭代器方式遍历较为繁琐而且遍历效率不高，使用迭代器的方式比较适合那些底层以链表形式实现的集合

***使用场景***

1. 访问一个聚合对象的内容而无须暴露它的内部表示。
2. 需要为聚合对象提供多种遍历方式。
3. 为遍历不同的聚合结构提供一个统一的接口。
4. JAVA 中的 iterator

# **Behavioural Design Patterns**

## Strategy Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2012.png)

### Motivation

- Need a way too adapt the behaviour of an algorithm at runtime

### Intent

- Define a family of algorithms, encapsulate each one, and make them interchangeable.

### Applicability

- Many related classes differ in their behaviour
- A context class can benefit from different variants of an algorithm
- A class defines may behavious, and these appears as multiple conditional statements. Instead, move each conditional branch into their own concrete strategy class

### Benefits

- Use composition over inheritance which allows better decoupling between the behaviou and context class that uses the behaviour
- 易于扩展，增加一个新的策略只需要添加一个具体的策略类即可，基本不需要改变原有的代码，符合开放封闭原则
- 避免使用多重条件选择语句，充分体现面向对象设计思想
- 算法可以自由切换，即策略类之间可以自由切换，由于策略类都实现同一个接口，所以使它们之间可以自由切换
- 每个策略类使用一个策略类，符合单一职责原则
- 客户端与策略算法解耦，两者都依赖于抽象策略接口，符合依赖反转原则
- 客户端不需要知道都有哪些策略类，符合最小知识原则
- 扩展性良好。

### Drawbacks

- Increases the number of objects
- Client must be aware of different strategies
- 策略模式，当策略算法太多时，会造成很多的策略类
- 客户端不知道有哪些策略类，不能决定使用哪个策略类，这点可以通过本文中的方式解决，也可以考虑使用IOC容器和依赖注入的方式来解决
- 所有策略类都需要对外暴露。

### Notes

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2013.png)

***各元素作用***

环境(Context)角色：持有一个Strategy的引用。

抽象策略(Strategy)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。

具体策略(ConcreteStrategy)角色：包装了相关的具体算法或行为

***使用场景***

1. 算法族: 使用多种不同的处理方式，做同样的事情，仅仅是具体行为有差别。这些处理方式，组合构成算法策略族，它们的共性，体现在策略接口行为上。
    
    → 针对同一问题的多种处理方式，仅仅是具体行为有差别时
    
2. 算法封装: 将各个算法封装到不同的类中，这样有助于客户端来选择合适的算法。
    
    → 需要安全地封装多种同一类型的操作
    
3. 可互相替换: 客户端可以在运行时选择使用哪个算法，而且算法可以进行替换，所以客户端依赖于策略接口。
    
    → 同一抽象类有多个子类，而客户端需要使用 if-else 或者 switch-case 来选择具体子类时
    

### Usage

```java
// Body
public interface BrakeStrategy {
		public void apply();		
}
public class BrakeABS implements BrakeStrategy {
	@Override
	public void apply() {
		System.out.println("Braking style is ABS ");
		
	}	
}
```

```java
// Caller
public class Car {
	protected EngineType engine ; 
	protected BrakeStrategy brake; 
	public Car(EngineType engine, BrakeStrategy brake) {
		this.brake = brake;
		this.engine = engine;
	}
	public void brakeApply() {
		brake.apply();
	}
	public static void main(String[] args) {
		Car c1 = new Car(new EngineNormal(), new Brake123());
		c1.brakeApply();
	}

}

```

## State Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2014.png)

- A state is a description of the status of a system that is waiting to execute a transition
- A transition is a set of actions to be executed when a condition is fulfilled or when an event is received.
- Identical stimuli trigger different actions depending on the current state
- **use the State pattern when the behaviour of an object should be influenced by it's state, and when complex conditions tie object behaviour to it's state.**

> The state pattern is a behavioural software design pattern that allows an object to alter its behavior when its internal state changes. This pattern is close to the concept of finite-state machines. The state pattern can be interpreted as a strategy pattern, which is able to switch a strategy through invocations of methods defined in the pattern's interface.
> 

### **Advantages of State Design Pattern**

- With State pattern, the benefits of implementing polymorphic behavior are evident, and it is also easier to add states to support additional behavior.
- In the State design pattern, an object’s behavior is the result of the function of its state, and the behavior gets changed at runtime depending on the state. This removes the dependency on the if/else or switch/case conditional logic. For example, in the TV remote scenario, we could have also implemented the behavior by simply writing one class and method that will ask for a parameter and perform an action (switch the TV on/off) with an if/else block.
- The State design pattern also improves Cohesion since state-specific behaviors are aggregated into the ConcreteState classes, which are placed in one location in the code.

### **Disadvantages of State Design Pattern**

- The State design pattern can be used when we need to change state of object at runtime by inputting in it different subclasses of some State base class. This circumstance is advantage and disadvantage in the same time, because we have a clear separate State classes with some logic and from the other hand the number of classes grows up.

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2015.png)

- Context: 可以有多个内部States，每当在Context上调用request()方法时，消息就会委托给State去处理。
- State 接口为所有具体状态定义了一个通用接口，封装了与特定状态相关的所有行为。
- ConcreteState 为请求实现它自己的实现。当 Context 改变状态时，真正发生的是我们有一个不同的 ConcreteState 与之关联。

> This is all quite similar to the Strategy pattern, except the changes happen at runtime rather than the client deciding. State saves you from lots of conditional code in your Context: by changing the ConcreteState object used, you can change the behaviour of the context
> 

### Usage

```java
// Caller
public class GumballMachine{
	// All states
	// If you want, these states are not necessary, look at lab5 solution
	State soldOutState;
	State soldState;
	// curr state
	State state;
	public GumballMachine(int num) {
		soldOutState = new SoldOutState(this);
		soldState= new SoldState(this);
		// .....

	}
	public void insertQuarter() { state.insertQuarter(); }
	public void ejectQuarter() { state.ejectQuarter(); }
	// ....
	public void turnCrank() {
		state.turnCrank();
		state.dispense();
	}
	public void setState(State state) {
		this.state = state;
	}
}
```

```java
public interface State {
	public void insert();
	//....
	// real work
	public void dispense();
}

public class SoldState implements State {
		GumballMachine gumballMachine;
		// ...
		// here's where the real work begin
		public void dispense() {
				if (something) {
						// to another state, which stored in caller class
						gumballMachine.setState(gumballMachine.getQuarterState());
				} else {
						gumballMachine.setState(gumballMachine.getSoldOutState());
				}
			
		}
}
public class SoldOutState implements State {
		GumballMachine gumballMachine;
		// ...
		// here's where the real work begin
		public void dispense() {
				if (something) {
						// to another state, which stored in caller class
				} else {
						// to another state, which stored in caller class				}
			
		}
}
```

### Design Step

1. we are going to define a State interface that contains a method for every action
2. Then we are going to implement a abstract State Class for every state. There classes will be responsible for the behaviour when it is in the corresponding state
3. We are going to get rid of all of our conditional code and instead delegate to the State class to de work for us

## Observer Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2016.png)

The `Observer Pattern`is used to implement distributed **event handling** systems, in “event driven” programming. 

> The observer pattern defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically. The object which is being watched is called the subject. The objects which are watching the state changes are called observers or listeners.
> 

In the observer pattern

- An object, called subject, maintains a list of its dependents, called observers
- notifies the observers automatically of any state changes in the subject, usually by calling one of their.

The aim should be to

- define a one to many dependency between objects without making the objects tightly coupled.
- automatically notify/update an open-ended number of observers(dependent objects) when the subject changes state
- be able to dynamically add and remove observers

### Java Observer and Observable: Limitations

***DO NOT USE IN THIS COURSE***

- Observable is a class, not an interface
- Observable protects crucial methods, the `setChanged()` method is protected.
- We cannot call `setChange()` unless we subclass *Observable! Inheritance is must, bad design* 🙂
- We cannot add on the *Observable* behaviour to an existing class that already extends another superclass.
- There isn’t an Observable interface, for a proper custom implementation.

### Advantages:

- Avoids tight coupling between Subject and Observers
- This allows the Subject and its Observers to be at different level of abstractions in a system
- Loosely coupled objects are easier to maintain and reuse
- Allows dynamic registration and deregistration

### Disadvantage

1. 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间
2. 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃
3. 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化

### Be careful

- A change in the subject may result in a chain of updates to its observers and in turn their dependent objects - resulting in a complex update behaviour.
- Need to properly manage such dependencies.
- 避免循环引用

### Notes

***观察者模式的使用场景：***

1.  一个抽象模型有两个方面，其中一个方面依赖于另一个方面。将这些方面封装在独立的对象中使它们可以各自独立地改变和复用。
2. 一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。
3. 一个对象必须通知其他对象，而并不知道这些对象是谁。
4. 需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。
5. 拍卖的时候，拍卖师观察最高标价，然后通知给其他竞价者竞价

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2017.png)

***参与者***

- 被观察者(Subject)：知道它的通知对象，事件发生后会通知所有它知道的对象，提供添加删除观察者的接口。
- 观察者(Observer)：提供通知后的更新事件
- 具体被观察者(ConcreteSubject)：被观察者具体的实例，存储观察者感兴趣的状态。
- 具体观察者(ConcreteObserver)：观察者的具体实现

### Usage

```java
// Caller
public static void main(String[] args) {

		Thermometer thermo = new Thermometer();
		DisplayAustralia ausDisplay = new DisplayAustralia();
		thermo.attach(usaDisplay);
}
```

```java
// interface
public interface ObserverThermometer {
	public void update(SubjectThermometer obj);
}

public interface Subject {
	public void attach(Observer o);
	public void detach(Observer o);
	public void notifyObservers();
}
// classes
public class DisplayAustralia implements ObserverThermometer {
	Subject subject;
	double temperatureC = 0.0;
	
	
	@Override
	public void update(SubjectThermometer obj) {

			this.temperatureC =  obj.getTemperatureC();
			display();
	}

	public void display() {
		System.out.printf("From DisplayAus: Temperature is %.2f C\n", temperatureC);
	}
}
public class Thermometer implements SubjectThermometer {
	
	ArrayList<ObserverThermometer> listObservers = new ArrayList<ObserverThermometer>();
	double temperatureC = 0.0;
	
	@Override
	public void attach(ObserverThermometer o) {
		if(! listObservers.contains(o)) { listObservers.add(o); }
	}

	@Override
	public void detach(ObserverThermometer o) {
		listObservers.remove(o);
	}

	@Override
	public void notifyObservers() {
		for( ObserverThermometer obs : listObservers) {
			obs.update(this);
		}
	}

	public double getTemperatureC() {
		return temperatureC;
	}

	public void setTemperatureC(double temperatureC) {
		this.temperatureC = temperatureC;
		notifyObservers();
	}
}
```

## Template Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2018.png)

- Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure
- Template pattern implement the common parts of a behaviour once and leave it up to subclasses to implement the behaviour that can vary

### Structure

- Primitive operations: operations that have default implementations or must be implemented by sub classes.
- Final operations: concrete operations that cannot be overridden by sub classes
- Hook operation: concrete operations that do nothing by default and can be redefined by subclasses if necessary. This gives subclasses the ability to “hook into” the algorithm at various points, if they wish; a subclass is also free to ignore the hook

### Template Vs Strategy Pattern

- Template Method works at the class level, so it is static
- Strategy works on the object level, letting you switch behaviours at runtime
- Template Method is based on inheritance: it lets you alter parts of an algorithm by extending those parts in subclasses.
- Strategy is based on composition: you can alter parts of the objects behaviour by supplying it with different strategies that correspond to that behaviour at runtime.

### Benefit

- 封装不变部分，扩展可变部分。
- 提取公共代码，便于维护。 运用模板方法能够将原本可能出现重复的“算法流程”，从子类提升到父类中，减少重复的发生，并且也开放子类参与算法中各个步骤的执行或优化。
- 行为由父类控制，子类实现。提高代码复用性，将相同部分的代码放在抽象的父类中
- 提高了拓展性，将不同的代码放入不同的子类中，通过对子类的扩展增加新的行为实现了反向控制，通过一个父类调用其子类的操作，通过对子类的扩展增加新的行为
- 实现了反向控制 & 符合“开闭原则

### Disadvantage

1. 每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大。
2. 如果“算法流程”开放太多步骤，并要求子类必须全部重新实现的话，反而会造成实现的困难，也不容易维护。

### Be Careful

- 为防止恶意操作，一般模板方法都加上 final 关键词。

### Notes

***模板模式的使用场景***

- 在造房子的时候，地基、走线、水管都一样，只有在建筑的后期才有加壁橱加栅栏等差异。
- spring 中对 Hibernate 的支持，将一些已经定好的方法封装起来，比如开启事务、获取 Session、关闭 Session 等，程序员不重复写那些已经规范好的代码，直接丢一个实体就可以保存。
- 有多个子类共有的方法，且逻辑相同。
- 重要的、复杂的方法，可以考虑作为模板方法
- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现。
- 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2019.png)

### Usage

```java
// Caller
CSVReport rep1 = new CSVReport("src/example/data.csv", true);
rep1.genReport();
```

```java
// TEMPLATE
public abstract class MyReportTemplate {
	
	// Real work
	public void genReport() {
		InputStream f1 = openFile();
		SortedMap<String, ArrayList<String>> data = parseFile( f1 );
		generateReport(data);
		generateSummary(data);
		
	}

	public void generateSummary(SortedMap<String, ArrayList<String>> data) {
		// Do something
	}
	public void generateReport(SortedMap<String, ArrayList<String>> data) {
		// Do something
	}
	protected abstract SortedMap<String, ArrayList<String>> parseFile(InputStream f1);
	public InputStream openFile() {
	
		String filename = getFilename();
		InputStream f1 = null;
		// open file
		return f1;
	}
	public abstract String getFilename(); 
}
// CONCRETE CLASS
public class CSVReport extends MyReportTemplate{
	private String fname = "";
	private boolean reqSummary = false;
	public CSVReport(String filename, boolean requestSummary) {
		this.fname = filename;
		this.reqSummary = requestSummary;
	}

	@Override
	protected SortedMap<String, ArrayList<String>> parseFile(InputStream f1) {
		// CSV parsing code here
		TreeMap<String, ArrayList<String>> data = new TreeMap<String, ArrayList<String>>();
		// populate data object in this method  ...	
		return data;
	}

	@Override
	public String getFilename() {
		// ask user for a file name.. 
		return fname;
	}
}
```

## Visitor Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2020.png)

- The visitor design pattern is a way of separating an algorithm form an object structure on which it operates
- A pratical result of this separation is the ability to add new operations to existing object structures without modifying the structures
- it is one way to follow the `open-closed principle`
- A visitor class is created that implements all of the appropriate specializations of the virtual operation/method

### Applicability

- many unrelated operations on an object structure are required
- the classes that make up the object structure are known and not expected to change
- new operations need to be added frequently
- an algorithm involves several classes of the object structure, but it is desired to manage signle location
- an algorithm needs to work across several independent class hierarchies.

### Limitation

- extension to the class hierarchy more difficult,
as a new class typically require a new visit method to be added to each visitor.
- 具体元素对访问者公布细节，违反了迪米特原则。
- 违反了依赖倒置原则，依赖了具体类，没有依赖抽象 (Maybe)

### Notes

在访问者模式（Visitor Pattern）中，我们使用了一个访问者类，它改变了元素类的执行算法。通过这种方式，元素的执行算法可以随着访问者改变而改变。这种类型的设计模式属于行为型模式。根据模式，元素对象已接受访问者对象，这样访问者对象就可以处理元素对象上的操作

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2021.png)

- Visitor：接口或者抽象类，它定义了对每一个元素（Element）访问的行为，它的参数就是可以访问的元素，它的方法数理论上来讲与元素个数是一样的，因此，访问者模式要求元素的类族要稳定，如果经常添加、移除元素类，必然会导致频繁地修改Visitor接口，如果这样则不适合使用访问者模式。
- ConcreteVisitor1、ConcreteVisitor2：具体的访问类，它需要给出对每一个元素类访问时所产生的具体行为。
- Element：元素接口或者抽象类，它定义了一个接受访问者的方法（Accept），其意义是指每一个元素都要可以被访问者访问。
- ConcreteElementA、ConcreteElementB：具体的元素类，它提供接受访问方法的具体实现，而这个具体的实现，通常情况下是使用访问者提供的访问该元素类的方法。
- ObjectStructure：定义当中所说的对象结构，对象结构是一个抽象表述，它内部管理了元素集合，并且可以迭代这些元素供访问者访问

***访问者模式的使用场景***

1. 对象结构中对象对应的类很少改变，但经常需要在此对象结构上定义新的操作。
2. 需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，也不希望在增加新操作时修改这些类。
3. 访问者可以对功能进行统一，可以做报表、UI、拦截器与过滤器

### Usage

```java
// Caller
public static void main(String[] args) {
    	
        Dot dot = new Dot(1, 10, 55);
        Circle circle = new Circle(2, 23, 15, 10);
        Rectangle rectangle = new Rectangle(3, 10, 17, 20, 30);

        CompoundShape compoundShape = new CompoundShape(4);
        compoundShape.add(dot);
        compoundShape.add(circle);
        compoundShape.add(rectangle);

        CompoundShape c = new CompoundShape(5);
        c.add(dot);
        compoundShape.add(c);

        export(circle, compoundShape);
    }

private static void export(Shape... shapes) {
    XMLExportVisitor exportVisitor = new XMLExportVisitor();
    System.out.println(exportVisitor.export(shapes));
}
```

```java
public interface Visitor {
    String visitDot(Dot dot);
    String visitCircle(Circle circle);
    String visitRectangle(Rectangle rectangle);
    String visitCompoundGraphic(CompoundShape cg);
}
public interface Shape {
    void move(int x, int y);
    void draw();
    String accept(Visitor visitor);
}
public class XMLExportVisitor implements Visitor {
    public String export(Shape... args) {
        StringBuilder sb = new StringBuilder();
        sb.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>" + "\n");
        for (Shape shape : args) {
            sb.append(shape.accept(this)).append("\n");
        }
        return sb.toString();
    }

    public String visitDot(Dot d) {
        return "<dot>" + "\n" +
                "    <id>" + d.getId() + "</id>" + "\n" +
                "    <x>" + d.getX() + "</x>" + "\n" +
                "    <y>" + d.getY() + "</y>" + "\n" +
                "</dot>";
    }
   
}
public class Dot implements Shape {
    private int id;
    private int x;
    private int y;
    public Dot() {
    }
    public Dot(int id, int x, int y) {
        this.id = id;
        this.x = x;
        this.y = y;
    }
    @Override
    public void move(int x, int y) {
        // move shape
    }
    @Override
    public void draw() {
        // draw shape
    }
    public String accept(Visitor visitor) {
        return visitor.visitDot(this);
    }
    public int getX() {
        return x;
    }
    public int getY() {
        return y;
    }
    public int getId() {
        return id;
    }
}
```

## Event Driven & Asynchronous Design

[slides](https://slides.com/npatrikeos/comp2511-22t2-9-2-event-driven-async-ii)

## Command & Facade Patterns (Bonus)

[slides](https://gitlab.cse.unsw.edu.au/COMP2511/22T2/content/-/blob/master/lecture-slides/week09/pdfs/CommandFacadePattern.pdf)

# Structural Design Patterns

## Composite Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2022.png)

- A composite is an object designed as a composition of one or more similar object
- Aims is to be able to manipulate a single instance of object just we would manipulate a group of them. For example
    - Operation to resize a group of shapes should be same as resizing a single shape
    - calculating size of a file should be same as a directory
- **No discrimination** between a Single(leaf) Vs a Composite(group) object
- Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly
    
    组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。
    
    这种模式创建了一个包含自己对象组的类。该类提供了修改相同对象组的方式。
    

### Uniformity vs Type Safety

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2023.png)

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2024.png)

Design for Uniformity

- include all child-related operations in the component interface, this means the leaf class needs to implement these methods with “doing nothing” or “throw exception”
- a client can treat both leaf and composite object uniformly
- we loose type safety because leaf and composite types are not clearly separated
- useful for dynamic structures where children types change dynamically, and client needs to perform child-related operations regularly.

Design for Type Safety

- only define child-related operations in the composite class
- the type system enforces type constraints, so a client cannot perform child-related operations on a leaf object
- a client needs to treat leaf and composite object differently
- useful for static structures where a client doesn’t need to perform child-related operations on “unknown” object of type component.

### Notes

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2025.png)

组成元素：

1. 抽象构件角色(Composite)：是组合中对象声明接口，实现所有类共有接口的默认行为。
2. 树叶构件角色(Leaf)：上述提到的单个对象，叶节点没有子节点。
3. 树枝构件角色(Composite)：定义有子部件的组合部件行为，存储子部件，在Component接口中实现与子部件有关的操作。
4. 客户端(Client)：使用 Component 部件的对象。

### **优点：**

1. 高层模块(客户端)调用简单。组合模式使得客户端代码可以一致地处理单个对象和组合对象，无须关心自己处理的是单个对象，还是组合对象，这简化了客户端代码；
2. 节点自由增加,更容易在组合体内加入新的对象，客户端不会因为加入了新的对象而更改源代码，满足“开闭原则”；

### **缺点：**

1. 在使用组合模式时，其叶子和树枝的声明都是实现类，而不是接口，违反了依赖倒置原则(Dependence Inversion Principle)
2. 设计较复杂，客户端需要花更多时间理清类之间的层次关系；
3. 不容易限制容器中的构件；
4. 不容易用继承的方法来增加构件的新功能；

### 组合模式的使用场景：

1. 你想表示对象的部分-整体层次结构，如树形菜单，文件、文件夹的管理。文件系统由文件和目录组成，每个文件里装有内容，而每个目录的内容可以有文件和目录，目录就相当于是由单个对象或组合对象组合而成，如果你想要描述的是这样的数据结构，那么你就可以使用组合模式。
2. 算术表达式包括操作数、操作符和另一个操作数，其中，另一个操作符也可以是操作数、操作符和另一个操作数。
3. 你希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。
4. 在现实生活中，存在很多“部分-整体”的关系。汽车与轮胎、发动机的关系。医院与科室、医生的关系。学校与学院、学生、老师的关系

### Usage

```java
// Callers
Composite mainboard = new Composite("Mainboard", 100);
Leaf processor = new Leaf("Processor", 450);
Leaf memory    = new Leaf("Memory", 80);
mainboard.add(processor);
mainboard.add(memory);
Composite chasis = new Composite("Chasis", 75);
chasis.add(mainboard);
Component disk = new Leaf("Disk", 50);
chasis.add(disk);
```

```java
// Type Safe
public interface Component {
	
	public String nameString();
	public double calculateCost();
	
}
public class Composite implements Component {
	private String name; 
	private double cost;
	ArrayList<Component>  children = new ArrayList<Component>();
	public Composite(String name, double cost) {
		super();
		this.name = name;
		this.cost = cost;
	}
	@Override
	public double calculateCost() {
		double answer = this.getCost();
		for(Component c : children) {
			answer += c.calculateCost();
		}		
		return answer;
	}
	
	@Override
	public String nameString() {
		String answer = "[" + this.getName()  + " "; 
		for(Component c : children) {
			answer = answer + " " + c.nameString();
		}	
		answer = answer + "]";
		return answer;
	}
	public boolean add(Component child) {
		children.add(child);
		return true;
	}
	public boolean remove(Component child) {
		children.remove(child);
		return true;
	}
// MORE GETTERS AND SETTERS	
}

public class Leaf implements Component {
	private String name; 
	private double cost;
	public Leaf(String name, double cost) {
		super();
		this.name = name;
		this.cost = cost;
	}

	@Override
	public String nameString() {
		return this.name;
	}

	@Override
	public double calculateCost() {
		return this.cost;
	}
}
```

## Decorator Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2026.png)

### Intent

- attach additional responsibilities to an object dynamically.
- Decorator design pattern allow uo to selectively add functionality to an object  at runtime, based on the requirements
- Original class is not changed(Open-Closed Principle)
- Inheritance extends behaviours at compile time, additional functionality is bound to all the instances of that class for their life time
- The decorator design pattern prefers a composition over an inheritance. provides a wrapper to existing class.
- Object can be decorated multiple times, in different order, due to the recursion involved with this design pattern.
- Do not need to implement all possible functionality in a single class.

### Structure

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2027.png)

- Client: refer to the component interface
- Component: define a common interface for Component1 and Decorator objects, 声明了在具体构件中实现的业务方法，它的引入可以使客户端以一致的方式处理未被装饰的对象以及装饰之后的对象，实现客户端的透明操作。
- Cimponent1: define object that get decorated, 实现了在抽象构件中声明的方法，装饰器可以给它增加额外的职责（方法）
- Decorator: maintains a reference to a Component object, and forwards requests to this component object,用于给具体构件增加职责，但是具体职责在其子类中实现. 通过其子类扩展该方法，以达到装饰的目的。
- Decorators(1, 2): implement additional functionality to be performed before and/or after forwarding a request. 可以增加新的方法用以扩充对象的行为。

### Notes

**优点：**

- 装饰类和被装饰类可以独立发展，不会相互耦合，互相都不用知道对方的存在
- 装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能，**无论包装多少层，返回的对象都是is-a的关系**。
- 实现动态扩展，只要**继承了装饰器**就可以**动态**扩展想要的功能了。

### **缺点：**

多层装饰比较复杂，提高了系统的复杂度。不利于我们调试。

### 使用装饰器模式的场景：

1. 在不影响其它对象的情况下，以动态、透明的方式给单个对象添加职责
2. 处理那些可以撤销的职责
3. 当使用子类扩展的方式不切实际的时候，可考虑使用装饰器模式
4. 咖啡店里咖啡中可以加不同的配料，摩卡、牛奶、糖、奶泡。手抓饼可以有不同的搭配，可以增加香肠、培根、里脊肉等等，计算增加不同的配料需要多少钱？
5. 房子装修，地板可以用瓷砖、木板，冰箱、电视可以使用不同的品牌

### Usage

```java
// Caller
Beverage beverage = new Espresso();
beverage2 = new Mocha(beverage2);
System.out.println(beverage2.getDescription() 
				+ " $" + beverage2.cost());
```

```java
public abstract class Beverage {
	String description = "Unknown Beverage";
	public String getDescription() {
		return description;
	}
	public abstract double cost();
}
public abstract class CondimentDecorator extends Beverage {
	public abstract String getDescription();
}

public class Espresso extends Beverage {
	public Espresso() {
		description = "Espresso";
	}
	public double cost() {
		return 1.99;
	}
}

public class Mocha extends CondimentDecorator {
	Beverage beverage;
	public Mocha(Beverage beverage) {
		this.beverage = beverage;
	}
	public String getDescription() {
		return beverage.getDescription() + ", Mocha";
	}
	public double cost() {
		double beverage_cost = beverage.cost(); 
		System.out.println("Soy: beverage.cost() is: " + beverage_cost);		
		System.out.println("  - adding One Mocha cost of 0.15c ");
		System.out.println("  - new cost is: " + (0.15 + beverage_cost ) );
		return 0.15 + beverage.cost();
	}
}
```

## Adapter Pattern(Optional)

# Creational Design Patterns

## Factory Method Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2028.png)

provides an interface for creating objects in a superclass, but allows subclasses to alter the type of object that will be created

在基类中定义创建对象的一个接口，让子类决定实例化哪个类。工厂方法让一个类的实例化延迟到子类中进行。

****优点：** 
1. 一个调用者想创建一个对象，只要知道其名称就可以了。 
2. 扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。 
3. 屏蔽产品的具体实现，调用者只关心产品的接口。
**缺点：**
1. 每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。
**为什么要用工厂模式**
1. **解耦** ：把对象的创建和使用的过程分开
2. **降低代码重复:** 如果创建某个对象的过程都很复杂，需要一定的代码量，而且很多地方都要用到，那么就会有很多的重复代码。
3. **降低维护成本** ：由于创建过程都由工厂统一管理，所以发生业务逻辑变化，不需要找到所有需要创建某个对象的地方去逐个修正，只需要在工厂里修改即可，降低维护成本。
**工厂模式的使用场景：**
1. 一个类不知道它所需要的对象的类：在工厂方法模式中，客户端不需要知道具体产品类的类名，只需要知道所对应的工厂即可，具体的产品对象由具体工厂类创建；客户端需要知道创建具体产品的工厂类。
2. 一个类通过其子类来指定创建哪个对象：在工厂方法模式中，对于抽象工厂类只需要提供一个创建产品的接口，而由其子类来确定具体要创建的对象，利用面向对象的多态性和里氏替换原则
3. 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无需关心是哪一个工厂子类创建产品子类，需要时再动态指定，可将具体工厂类的类名存储在配置文件或数据库中。
4. 日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方。 
5. 数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 
6. 设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口。
7. 需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。

### Structure

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2029.png)

- Product: declares the interface, which is common to all object that can be produced by the creator and its subclasses
- Concrete Products : are different implementations of the products interface
- Creator: declares the factory method that returns new product objects
- Concrete Creators: override the bas factory method so it returns a different type of product

### Usage

```java
// Callers
Dialog dialog = new WindowsDialog();
dialog.renderWindow();
```

```java
// ------------------- product
public interface Button {
    void render();
    void onClick();
}
public class HtmlButton implements Button {

    public void render() {
        System.out.println("<button>Test Button</button>");
        onClick();
    }

    public void onClick() {
        System.out.println("Click! Button says - 'Hello World!'");
    }
}
public class WindowsButton implements Button {
    public void render() {
        // do something
    }
    public void onClick() {
        // do something
    }
}
// ----------------------- creator
public abstract class Dialog {
    public void renderWindow() {
        // ... other code ...

        Button okButton = createButton();
        okButton.render();
    }
    public abstract Button createButton();
}
public class HtmlDialog extends Dialog {
    @Override
    public Button createButton() {
        return new HtmlButton();
    }
}
public class WindowsDialog extends Dialog {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }
}
```

## Abstract Factory Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2030.png)

let users produce families of related objects without specifying their concreate classes

### Intent

Abstract Factory is a creational design pattern that lets you produce families of related object without specifying their concrete class

### Problem

- creating an object directly within the class that requires the object is inflexible
- it commits the class to a particular object
- make it impossible to change the instantiation independently from the class

### Possible Solution

- define a separate operation for creating an object
- Create an object by calling a factory method
- This enables writing of subclasses to change the way an object is created

### **优点：**

1. 当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。

### **缺点：**

1. 产品族扩展非常困难，要增加一个系列的某一产品，既要在抽象的 Creator 里加代码，又要在具体的里面加代码。

抽象工厂模式的使用场景：

1. QQ 换皮肤，一整套一起换。
2. 生成不同操作系统的程序

### Structure

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2031.png)

1. Abstract Products: declare interface for a set of distinct but related products which make up a product family.
2. Concrete Products: are various implementations of abstract products, grouped by variants. Each abstract product(chair/sofa) must be implemented in all given variants(Victorian/Modern)
3. Abstract Factory: interface declares a set of methods for creating each of the abstract products
4. Concrete Factories: implement creation methods of the abstract factory. Each concrete factory corresponds to a specific variant of products and creates only those product variants.
5. Client: can work with any concrete factory/product variant, as long as it communicates with their objects via abstract interfaces.

### Usage

```java
// Callers
Application app;
GUIFactory factory;
factory = new WindowsFactory();
app = new Application(factory);
app.paint();

public class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void paint() {
        button.paint();
        checkbox.paint();
    }
}

```

```java
// Abstract Products
public interface Button {
    void paint();
}
public interface Checkbox {
    void paint();
}
// Abstract factory
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
// Concrete Products
public class WindowsButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created WindowsButton.");
    }
}
public class WindowsCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created WindowsCheckbox.");
    }
}
// Concrete factory
// real work
public class WindowsFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}
```

## Singleton Pattern

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2032.png)

- In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one "single" instance. This is useful when exactly one object is needed to coordinate actions across the system. The term comes from the mathematical concept of a singleton
    
    这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象
    

### Intent

Let users ensure that a class has only one instance, while providing a global access point to this instance.

### Problem

- ensure that a class has just a single instance
- provide a global access point to that instance

### Solution

- Make the default constructor ***private***, to prevent other object form using the new operator with the Singleton class
- Create a static creation method that acts as a constructor. Under the hood this method calls the private constructor to create an object and save it in a static field. All following calls to this return the cached object.
- If your code has access to the Singleton class, then it is able to call the Singleton’s static method
- Whenever Singleton’s static method is called, the same object is always returned.

### Structure

![Untitled](COMP2511%2065aba9dcea1d46729905a3118a7f0285/Untitled%2033.png)

- Singleton: declares the static method `getInstance` that returns the same instance of its own class
- The Singleton’s constructor should be hidden from the client code.
- Calling the `getInstance` method should be the only way of getting the Singleton object

### Notes

**优点：**

1. 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。
2. 避免对资源的多重占用（比如写文件操作）。

**缺点：**

1. 没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

### **单例模式分类**

1. 懒汉单例模式：在第一次调用的时候实例化本身，在并发环境下，可能出现多个本身对象。所以线程是不安全的
2. 饿汉单例模式：在类初始化时，已经自行实例化一个静态对象，所以本身就是线程安全的
3. 登记单例模式：通过一个专门的类对各单例模式的单一实例进行管理和维护

### 单例模式的使用场景：

1. 要求生产唯一序列号。
2. WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。
3. 创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。
4. 线程池、缓存、日志对象、对话框、打印机、显卡的驱动程序对象常被设计成单例

### **为什么要用单例模式?**

1. 单例模式节省公共资源，对应到我们计算机里面，像日志管理、打印机、数据库连接池、应用配置。
2. 单例模式方便控制，就像日志管理，如果多个人同时来写日志，你一笔我一笔那整个日志文件都乱七八糟，如果想要控制日志的正确性，那么必须要对关键的代码进行上锁，只能一个一个按照顺序来写，而单例模式只有一个人来向日志里写入信息方便控制，避免了这种多人干扰的问题出现。

### Usage

```java
MySingleton s1 = MySingleton.getInstance();
System.out.println("s1.getTime() " + s1.getTime());
```

```java
public class MySingleton {

	private static MySingleton single_instance = null ;
	private LocalTime localTime;
	private MySingleton() {
		localTime = LocalTime.now();	
	}

	public static synchronized MySingleton getInstance() {
		if (single_instance == null) {
			single_instance = new MySingleton();
		}
		return single_instance;
	}

	public LocalTime getTime() {
		return localTime;
	}
}
```
