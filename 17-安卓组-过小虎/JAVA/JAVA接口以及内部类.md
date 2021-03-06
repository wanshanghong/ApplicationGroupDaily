## 接口
### 接口是什么
接口是一组抽象方法、常量和内嵌类型的集合。

一个接口可被多个类实现，变现多态性
### 接口申明
使用关键词**interface**

- 接口中的成员变量都是常量，申明时要赋值，默认修饰符为public static final;不能声明实例。
- 接口中的成员方法都是抽象的。
- 接口中不能包含构造方法。
- 接口的访问控制权限是public或者缺省。
- 接口没有任何具体实现，也就是不能创建实例。
### 申明实现接口的类
使用关键词**implements**声明一个类实现指定接口

- 一个类可以实现多个接口，用逗号隔开。
- 接口是多继承的。用逗号隔开
- 接口是引用数据类型
### 接口与抽象类的区别
- 抽象类为子类约定方法声明，抽象类可以给出部分实现，包括构造方法；抽象方法在多个子类中表现出多态性，类是**单继承**。
- 接口为多个互不相关的类约定某一特性的方法声明，在类型层次中表达对象拥有的属性。接口没有实现部分。接口是多继承的。一个类实现多个接口，就具有多种特性，也就是**多继承**。

### 接口的多态性
- 当子接口和父接口的抽象方法同名时，两个方法的关系根据参数列表决定，如果参数列表和返回类型相同，则覆盖。参数列表不同则重载。
- 当子接口继承了多个父接口的时候，如果存在重复方法，如果参数列表和返回值都不同，则子接口都继承下来，属于重载关系。如果完全相同，则继承一个。
- 一个类有多个接口，这些接口如果有相同的方法的话，这个类只需要实现一个就好了。
- 接口的多继承不存在二义性，而类的多继承会存在二义性。
## 内部类和内部接口
包含内嵌类或者接口的类叫做**外层类型**。

内嵌类型的作用

类型嵌套和对象嵌套。静态内嵌类型用于定义类型的嵌套结构，实例内嵌类型用于定义对象的嵌套结构。

内嵌类型既有类型的特性，也有类中成员的特性。

#### 内部类作为类型的特性
- 内嵌类型不可以和外层类型同名。
- 内部类中可以声明成员变量和成员方法，内部类成员可以与外部类成员同名；内部接口中可以声明成员常量和抽象成员方法。
- 内部类可以继承父类或实现接口。
- 可以声明内部类为抽象类，但是这样它必须被其他内部类继承；内部接口必须被其他内部类实现
#### 内部类作为成员的特征
- 使用点运算符可以应用内嵌类型。
    外层类型.内部类型
- 创建内部类要先创建外部类在创建内部类
```
Point x = new Test().new Point(1,2);
```
- 内嵌类型具有类中成员的4种访问控制权。当内部类可被访问时，才能考虑内部类中成员的访问控制权限。
- 作为成员，内部类型与其他外层类型彼此信任，能访问对方的所有成员。
- **内部接口总是静态的**。内部类可以声明是静态的或实例的，静态内部类能够声明静态成员，但不能应用外部类的实例成员；实例内部类不能声明静态成员。

### 内部类和外部类变量重名时以及如何通过外部类调用内部类
```
public class Test8 {
	public static void main(String[] args) {	
		Person x = new Person();
		x.new Heart().show();
	}
}
class Person {
	int age = 2;		
	class Heart {
		int age = 4;
		void show() {
			System.out.println(age);
			System.out.println(Person.this.age);
		}		
	}	
}
```
### 匿名内部类

```
new 父类构造器（参数列表）|实现接口（）  
    {  
     //匿名内部类的类体部分  
    }
```
在这里我们看到使用匿名内部类我们**必须要继承一个父类或者实现一个接口**，当然也仅能只继承一个父类或者实现一个接口。同时它也是**没有class关键字**，这是因为匿名内部类是直接使用new来生成一个对象的引用。当然这个引用是隐式的。
-  对于匿名内部类的使用它是存在一个缺陷的，就是它仅能被使用一次，创建匿名内部类时它会立即创建一个该类的实例，该类的定义会立即消失，所以匿名内部类是不能够被重复使用。

例子：
```
public abstract class Bird {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    
    public abstract int fly();
}

public class Test {
    
    public void test(Bird bird){
        System.out.println(bird.getName() + "能够飞 " + bird.fly() + "米");
    }
    
    public static void main(String[] args) {
        Test test = new Test();
        test.test(new Bird() {
            
            public int fly() {
                return 10000;
            }
            
            public String getName() {
                return "大雁";
            }
        });
    }
}
```
其中匿名内部类等价于
```
public class WildGoose extends Bird{
    public int fly() {
        return 10000;
    }
    
    public String getName() {
        return "大雁";
    }
}

WildGoose wildGoose = new WildGoose();
test.test(wildGoose);
```
这样的操作。

注意！
- 使用匿名内部类时，我们必须是继承一个类或者实现一个接口，但是两者不可兼得，同时也只能继承一个类或者实现一个接口。
- 匿名内部类中是不能定义构造函数的。
- 匿名内部类中不能存在任何的静态成员变量和静态方法。
- 匿名内部类为局部内部类，所以局部内部类的所有限制同样对匿名内部类生效。
- 匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法。

**我们给匿名内部类传递参数的时候，若该形参在内部类中需要被使用，那么该形参必须要为final。也就是说：当所在的方法的形参需要被内部类里面使用时，该形参必须为final。**
#### 匿名内部类的构造采用构造代码块实现。