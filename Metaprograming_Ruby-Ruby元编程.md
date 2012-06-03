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

跟目录与文件一样，常量也可以通过路径方式来唯一标识。如果深入探究常量的树形结构，可以在常量前加上一组双冒号来表示根路径，从而得到一个绝对路径。

Module 类提供了两个叫做 constants 的方法，一个是实例方法，一个是类方法。实例方法返回当前范围内的常量，像文件系统的 `ls` 命令。类方法返回当前程序程序中所有顶级常量，包括类名。

如果想获得当前常量的路径，可以使用 Module.nesting 方法。

使用命名空间（也就是在模块中定义常量），可以避免类名的冲突问题。比如加载文件用到的 load 方法，如果第二个可选参数为 true ，则其常量常量就仅在自身范围内有效。用这种方式加载的文件，Ruby 会创建一个匿名模块，使用它作为命名空间来容纳文件中定义的所有常量，加载完成后，该模块被销毁。

require 方法和 load 颇为相似，但目的不同。require 用来导入类库，这些类库中的类名通常是导入这些库是希望得到的，因此没有理由在加载后销毁它们。C

### 1.5 调用一个方法时发生了什么
当调用一个方法时，Ruby 会做两件事：

1. 找到这个方法。这个过程称为*方法查找*。
2. 执行这个方法。为了做到这一点，Ruby 需要一个叫做 self 的东西。

#### 方法查找

* 接收者：就是调用方法所在的对象。比如 `my_string.reverse()` my_string 就是接收者。
* 祖先链：每个类都有一个祖先链，可以通过调用类的 ancestors() 方法来获得。这个链从自己所属的类开始，向上直到 BaseObject 为止。

为了查找一个方法，Ruby 会首先在接收者的类中查找，然后一层层地在祖先链中查找，直到找到这个方法。

当你在一个类（甚至在另外一个模块）中包含（include）一个模块时，Ruby 创建了一个封装该模块的匿名类，并把这个匿名类插入到祖先链当中，其在链中的为止正好在包含它的类上方。

封装类有时也叫代理类，superclass 方法会当它根本不存在，正常的代码也无法访问这些类。

Ruby 中有些像 print() 的方法，好像所有对象都有 print() 方法一样。这样的方法其实是 Kernel 模块的私有方法。

``` Ruby
Kernel.private_instance_methods.grep /^pr/ # => [:printf, :print, :proc]
```

Object 类包含了 Kernel 模块，因此 Kernel 就进入了每个对象的祖先链。这样在某个对象中可以随意调用 Kernel 模块的方法。使得 print 好像一个语言的关键字。其实它不过是一个方法。

如果给 Kernel 模块增加一个方法，这个*内核方法(Kernel Method)*就对所有对象可用。

#### 执行方法
当调用一个方法时，Ruby 需要持有一个接收者的引用，正是这个引用的存在，它可以记得哪个对象是接收者，再用它来执行这个方法。

每一行代码都会在一个对象中被执行——这个对象就是“当前对象”，用 self 表示，因为可以用 self 关键字来访问它。

在给定时刻，只有一个对象能够充当当前对象，但没有哪个对象能够长期充当这个角色。

*当调用一个方法时，接收者就称为了 self 。从这一刻起，所有的实例变量都是 self 的实例变量，所有没有明确指明接收者的方法都在 self 上调用*。

如果没有调用任何方法时，self 由 Ruby 解释器在开始运行 Ruby 程序是就创建的名为 main 的对象担任。这个类有时被称为*顶级上下文(top level context)*。

在类或者模块定义中，self 的角色由这个类或模块担任：

``` Ruby
class MyClass
  self # => MyClass
end
```

#### 私有变量
私有变量由两条规则一起控制：

1. 如果调用方法的接收者不是自己，则必须明确指明一个接收者。
2. 私有方法只能被隐含接收者(self)调用。

这两条规则耦合后的规则被称为“私有规则”：

不能明确指定一个接收者来调用一个私有方法。换言之，每次调用一个私有方法时，只能调用与隐含的的接收者——self上。

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
