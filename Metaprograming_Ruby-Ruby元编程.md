# Ruby 
Ruby 是们很有意思的语言，它的对象模型与 JavaScript 的原型模型非常相似，同时 Ruby 和 JavaScript 又都是动态语言，所以作笔记的时候，我会力图将书中的代码以 JavaScript 实现一次，代码会附在最后，同时标明章节。所有代码在 Firebug 下进行测试。

## 引言
元编程通常用来处理以下工作：

* 编写一个外部系统的包装器，由于其动态特性，使得外部系统增加新接口时在不修改 Ruby 代码的情况下自动对其产生支持
* 定义 DSL （特定领域语言）
* 降低代码的重复性
* 改造语言本身，使其满足工作需要
* 等等……

### 元编程的定义
元编程是编写在运行时候操纵语言构件的代码。（Page xvii）

### 静态语言和动态语言的区别：内省
在静态语言中，语言构件的行为像是幽灵，你可以在源代码中看到它们，然而在运行时它们就消失了。一旦编译器完成了它的工作，像变量和方法这样的东西就看不见摸不着了：它们只是内存的位置而已。

在动态语言中，在运行时绝大部分语言构件依然存在，你甚至可以询问关于它自己的问题。这就是内省 (introspection) 。

``` Ruby
class Greeting
  def initialize(text)
    @text = text
  end
  
  def welcome
    @text
  end
end
  
my_object = Greeting.new "hello"
  
my_object.class # => Greeting
my_object.class.instance_methods false # => [:welcome]
my_object.instance_variables # => [:@text]
```

### 在程序运行时添加实例方法
书中提到了 ActiveRecord 类库，我就直接在最后写个 JavaScript 版的简短代码吧。

### 元编程与 Ruby
Ruby 无疑是现今流行语言中对元编程最友好的一种语言。元编程如此的深入 Ruby 语言，甚至无法和“普通”的编程明确区分开来。你无法看着一段 Ruby 代码说，“这部分是元编程，其他不是”。从某种程度上看，元编程不过是每个 Ruby 程序员的例行工作。

## 第一章 对象模型
Ruby 世界中对象只是其中一个公民，除此之外还有 类（class）、模块（module）以及实例变量（instance variable）等。元编程操控的就是这些语言构件。

这些语言构件存在于其中的系统称之为对象模型（object model）。对象模型是 Ruby 的灵魂，认真钻研它不仅能学到一些功能强大的技术，还能避免陷入某些陷阱。

### 1.2 打开类

``` Ruby
class String
  def to_alphanumeric
    gsub /[^\w\s]/, ''
  end
end
```

在 Ruby 中，定义类的语句和其他语句没有本质的区别，你可以在类定义中放置任何语句。

从某种意义上来说，Ruby 的 class 关键字更像是一个作用域操作符而不是类型声明语句。它的确可以创建一个还不存在的类，不过也可以把这开成是一种副作用。*对于 class 关键字，其核心任务是把你带到类的上下文中，让你可以在其中定义方法*。你总是可以重新打开以及存在的类并对它进行动态修改。

#### 隐患：猴子补丁（Monkeypatch）
如果你粗心地为某个类添加了某些功能，可能会无意地覆盖原有的功能，如果程序的其他部分依赖这个功能，程序就会出错。

可以通过 instance.methods 内省得到已有的方法名，避免与其产生冲突。

### 1.3 类的真相
#### 什么是对象
* 一组实例变量：可以通过 Object#instance_variables 方法获得一个实例变量的列表。Ruby 中对象和实例变量没有关系，当给实例变量赋值时，它们就生成了，如果没有调用赋值的方法，那么它们就不存在。因此，对同一个类，你可以创建具有不同实例变量的对象。
* 一个指向其类的引用：可以通过 Object#methods 方法获得一个对象的方法列表。一个对象的实例变量存在于对象本身，而对象的方法存在与对象的类。一个对象仅仅包含它的实例变量以及一个对自身类的引用。这就是为什么同一个类的对象共享同样的方法，但不共享实例变量的原因。*对象调用的称之为方法，类中定义的称之为实例方法*，也就是说，需要定义一个类的实例才能调用这个方法。

#### 什么是类
**类自身也是对象**。

类和其他任何对象一样，也有自己的类，它的名字叫做 Class :

``` Ruby
"hello".class # => String
String.class # => Class
```

和任何对象一样，类也有方法 (new) ，类的方法就是 Class 的实例方法。所以的类都最终继承于 Object ，Object 本身继承于 BaseObject ，BaseObject 是 Ruby 对象体系中的根节点。

``` Ruby
Class.superclass # => Module
Module.superclass # => Object
```

因此，一个类不过是一个增强的 Module 。增加了三个方法—— new(), allocate(), superclass() 而已。这几个方法可以让你创建对象并把它纳入到类体系中。除此之外，类和模块基本上是一样的。绝大多数适用于类的内容也适用于模块，反之亦然。

类无非是一个对象（Class类的一个实例）外加一组实例方法和一个对其超类的引用。Class类是Module的子类，因此一个类也是一个模块。

#### 常量
常量（任何大写字母开头的引用）作用域不同于变量，常量的结构和树形结构类似，模块（还有类）像目录，常量则像文件，只要不在同一个目录下，不同文件的文件名可以重复，甚至可以通过路径方式引用一个常量，如 MyModule::MyClass::MyConstant。

使用命名空间（也就是在模块中定义常量），可以避免类名的冲突问题。

### 1.5 调用一个方法时发生了什么
当调用一个方法时，Ruby 会做两件事：

1. 找到这个方法。这个过程称为*方法查找*。
2. 执行这个方法。为了做到这一点，Ruby 需要一个叫做 self 的东西。

#### 方法查找

## 附录：JavaScript 的代码实现：
### 附录
#### 内省

``` JavaScript
function Greeting(text) {
  this.text = text;
  
  this.welcome = function() {
    return this.text;
  };
}
  
var my_object = new Greeting("hello");
  
my_object.constructor; // => Greeting(text)
  
// JavaScript 没有实例变量的概念，对它而言，方法也好，变量也罢，无非属性。
for (var attr in my_object)
  console.log(attr); // => text welcome
```

#### 在程序运行时添加实例方法
``` JavaScript
function test(nameList) {
  var i, name;
  if (Object.prototype.toString.call(nameList) !== '[object Array]') return;
  for (var i=0; i < nameList.length; i++) {
    name = nameList[i];
    this[name] = function() {
      console.log('this is ' + name);
    }
  }
}
  
var a = new test(["hello", "world"]);
```

### 第一章
#### 1.2 打开类
``` JavaScript
String.prototype.to_alphanumeric = function() {
  return this.replace(/[^\w\s]/g, "");
};
```
