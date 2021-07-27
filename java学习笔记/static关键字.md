## static的用途

用最简单的一句话来概括，就是方便在没有创建对象的情况下来进行调用。

被static修饰的方法或变量，不需要依赖于对象来进行访问，只要类被加载了，就可以通过类名去访问。

static可以用来修饰类的成员方法，类的成员变量，还有static代码块

1. static方法

static方法一般称作静态方法，由于**静态方法不依赖于任何对象就可以进行访问**，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。

并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。

**虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。**

举例：

```java
class MyObject{
	public static String str1="staticProperty";
	private String str2="property";

	public MyObject(){
	}
	
	public void print1(){
		System.out.println(str1);
		System.out.println(str2);
		print2()
	}
	
	public static void print2(){
		System.out.println(str1);
		System.out.println(str2); *
		print1(); *
	}
}
```

上面代码中，标注了\*号的两行无法运行，因为**静态方法中无法访问非静态成员方法和非静态成员变量**

2. static变量

static变量也称作静态变量，静态变量和非静态变量的区别是：
- 静态变量
	- 被所有的对象所共享，
	- 在内存中只有一个副本，
	- 它当且仅当在类初次加载时会被初始化。
- 而非静态变量
	- 是对象所拥有的，
	- 在创建对象的时候被初始化，
	- 存在多个副本，
	- 各个对象拥有的副本互不影响。

3. static代码块

静态代码块可以优化程序性能

static块可以置于类中的任何地方，类中可以有多个static块。

在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

static块可以用来优化程序性能，原因在于它的特性:
- **只会在类加载的时候执行一次。**

---
## static关键字的误区

1. static关键字**不会改变类中成员的访问权限**

2. 能否通过this来访问静态成员变量？

```java
public class Main{
	static int value=33;
	
	public static void main(String[] args) throws Exception{
		new Main().printValue();
	}
	
	private void printValue(){
		int value=3;
		System.out.println(this.value);
	}
}
```

此处最终打印出来的应该是33，因为this代表的是当前对象，

那么通过new Main()来调用printValue的话，当前对象就是通过new Main()生成的对象

而static变量是被对象所享有的，因此在printValue中的this.value的值毫无疑问是33。

在printValue方法内部的value是局部变量，根本不可能与this关联

**需要记住一点**：
静态变量虽然独立于对象，不代表不可以通过对象去访问，所有的静态变量和静态方法都可以通过对象访问（前提是权限足够）

3. static能否作用于局部变量？

**不可以**，Java规定，static不可以用来修饰局部变量

---
## 测试

1. 下面这段代码的输出结果是什么？

```java
public class Test extends Base{
 
    static{
        System.out.println("test static");
    }
     
    public Test(){
        System.out.println("test constructor");
    }
     
    public static void main(String[] args) {
        new Test();
    }
}
 
class Base{
     
    static{
        System.out.println("base static");
    }
     
    public Base(){
        System.out.println("base constructor");
    }
}
```

答案：
base static 
test static 
base constructor
test constructor

解：

先来想一下这段代码具体的执行过程，在执行开始，先要寻找到main方法，因为main方法是程序的入口，

但是在执行main方法之前，必须先加载Test类，而在加载Test类的时候发现Test类继承自Base类，因此会转去先加载Base类，在加载Base类的时候，发现有static块，便执行了static块。

在Base类加载完成之后，便继续加载Test类，然后发现Test类中也有static块，便执行static块。在加载完所需的类之后，便开始执行main方法。在main方法中执行new Test()的时候会先调用父类的构造器，然后再调用自身的构造器。因此，便出现了上面的输出结果。

2. 这段代码的输出结果是什么？

```java
public class Test {
    Person person = new Person("Test");
    static{
        System.out.println("test static");
    }
     
    public Test() {
        System.out.println("test constructor");
    }
     
    public static void main(String[] args) {
        new MyClass();
    }
}
 
class Person{
    static{
        System.out.println("person static");
    }
    public Person(String str) {
        System.out.println("person "+str);
    }
}
 
 
class MyClass extends Test {
    Person person = new Person("MyClass");
    static{
        System.out.println("myclass static");
    }
     
    public MyClass() {
        System.out.println("myclass constructor");
    }
}
```

答案：
test static 
myclass static 
person static 
person Test
test constructor
person MyClass
myclass constructor

首先加载Test类，因此会执行Test类中的static块。

接着执行new MyClass()，而MyClass类还没有被加载，因此需要加载MyClass类。

在加载MyClass类的时候，发现MyClass类继承自Test类，但是由于Test类已经被加载了，所以只需要加载MyClass类，那么就会执行MyClass类的中的static块。
	
在加载完之后，就通过构造器来生成对象。而在生成对象的时候，必须先初始化父类的成员变量，因此会执行Test中的Person person = new Person()，而Person类还没有被加载过，因此会先加载Person类并执行Person类中的static块，接着执行父类的构造器，完成了父类的初始化，然后就来初始化自身了，因此会接着执行MyClass中的Person person = new Person()，最后执行MyClass的构造器。

3. 这段代码的输出结果是什么？

```java
public class Test {
     
    static{
        System.out.println("test static 1");
    }
    public static void main(String[] args) {
         
    }
     
    static{
        System.out.println("test static 2");
    }
}
```

答案：
test static 1 
test static 2