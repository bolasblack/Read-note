# 深入浅出 CoffeeScript
由于使用 CoffeeScript 已经颇有些时日，而且作为一种工具性质的语言，真正的语言细节依旧在于 JavaScript ，所以阅读的目的在于查漏补缺，不会有太多的笔记。

## 第三章 集合与迭代
### 3.2 数组

```coffeescript
[][3...0] is []
[][1..] is [][1...] is [].slice 1
arr[0...-1] is arr.slice 0, arr.length - 1

['a', 'c'][1...2] = ['b'] is ['a', 'b']
['a', 'c'][1...1] = ['b'] is ['a', 'b', 'c']
['a', 'c'][1...] = ['b'] is ['a', 'b']
['a', 'c'][1...] = ['b', 'd'] is ['a', 'b', 'd']
['a', 'c'][1...-1] = ['b'] is ['a', 'b', 'c']
['a', 'c'][1...-2] = ['b'] is ['a', 'b', 'c']
```

### 3.3 集合的迭代

```coffeescript
for own k, v of obj
  # ...

# is

for k, v of obj when obj.hasOwnProperty k
  # ...
```

> 当写 `for x of obj` 或者 `for x in arr` 时，你其实时在给一个当前作用域内名为 x 的变量赋值。循环结束后还可以继续利用这些变量。

```coffeescript
for name, occupation of murder
  break if occupation is 'butler'
console.log "#{name} did it!"

countdown = [10..0]
for num in countdown 
  break if abortLaunch()
if num is 0
  console.log 'We have liftoff!'
else
  console.log "Launch aborted with #{num} seconds left"
```

> `for ... in` 还支持 `for ... of` 不支持的补充修饰符 `by` 。

```coffeescript
decimate = (army) ->
  execute(soldier) for soldier in army by 10
  
animate = (startTime, endTime, framesPerSecond) ->
  for pos in [startTime..endTime] by 1 / framesPerSecond
    addFrame pos

countdonw = (max) ->
  console.log x for x in [max..0] by -1
```

> 但是要注意，数组并不支持负步值。当你写 `for ... in[start..end]` 时，`start` 是第一个迭代值（而 `end` 是最后一个迭代值），只要 `start>end` 负步值就没有问题。但是每当你写 `for ... in arr` 时，第一个迭代索引值总是 0 ，最后一个迭代索引是 `arr.length - 1` 。因此如果 `arr.length` 大于零，则负步值就会产生无限循环——永远不可能达到最后一个迭代索引！

### 条件迭代

```coffeescript
makeHay() while not sunShines() is makeHay() until sunShines()
```

> 注意，在这两种句法中，如果条件起初就不满足的话 makeHay() 一次都不会执行。

```coffeescript
loop
  console.log "loop"
  
# is

while true
  console.log "loop"
  
a = 0
loop break if ++a > 999
console.log a # => 1000

break if ++a >999 loop # Error
```

### 列表解析

```coffeescript
negativeNumbers = (-num for num in [1, 2, 3, 4])
keysPressed = (char while char = handleKeyPress())
code = ["U", "U", "D", "D", "L", "R", "L", "R", "B", "A"]
codeKeyValues = for key in code
  switch key
    when "L" then 37
    when "U" then 38
    when "R" then 39
    when "D" then 40
    when "A" then 65
    when "B" then 66
evens = (x for x in [2..10] by 2)
```

> 列表解析式是 CoffeeScript 核心设计哲学的产物： CoffeeScript 中所有的东西都是表达式，并且每个表达式有一个值。

## 第四章 模块与类
### 4.2 原型的威力
http://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototype

## 第六章 Node.js 服务器端程序
### 6.3 异步思想

> 回忆一下我们在 2.2 节学到的：*只有函数会产生作用域*。在处理异步的回调时，不要期望循环能产生作用域，它会毁了你的，即使在其他方面做的再好也没用。事实上，这通常是在异步代码中出错最多的部分。

```coffeescript
sum = 0
while sum < limit
  sum += x = nextNum()
  getEncryptionKey (key) ->
    saveEncryptiond key, x, sum # FAIL!
```

> 在调用 `getEncryptionKey` 的回调函数时， x 和 sum 已经前移了——事实上，整个循环都已结束。因此随着循环对每一个 x 的迭代，最后保存下来的是循环运行结束后的 x 和 sum 的值。

```coffeescript
sum = 0
while sum < limit
  sum += x = nextNum()
  do (x, sum) ->
    getEncryptionKey (key) ->
      saveEncryptiond key, x, sum

# is

sum = 0
while sum < limit
  sum += x = nextNum()
  ((x, sum) ->
    getEncryptionKey (key) ->
      saveEncryptiond key, x, sum
  ) x, sum
```

## 附录 B 运行 CoffeeScript 的几种方法

### B.3 Rails 中的 CoffeeScript

> Rails 3.1 通过 [Sprockers2](http://github.com/sstephenson/sprockets) 提供了对 CoffeeScript 的支持。

> 旧版 Rails 借助于 [Barista](http://github.com/Sutto/barista) 也能直接集成 CoffeeScript 。

Barista 更进一步，允许将 CoffeeScript 代码打包为 gems 或者从 gems 中获取 CoffeeScript 代码。这是一个将大项目拆成可重用的精简版组件的极佳方式。还能兼容 Heroku ，只需要扩展 [therubyracer-heroku](http://github.com/aler/therubyracer-heroku) 即可。therubyracer-heroku 是一个能运行于所有 Ruby 环境中的 JavaScript 解释器。（这些同样适用于 Rails 3.1 程序。Barista 和 Rails 3.1 都采用同样的 gem 来包装 CoffeeScript 解释器[1]。）

Barista 和多数为 CoffeeScript 整合的 Web 框架的一个缺点是如果代码中有语法错误（编译未通过），则需要等到刷新页面或遇到不标准的 JavaScript 才能发现。

> 我为解决这个问题写了一个 Growl 插件来扩展 Barista ，在 CoffeeScript 编译失败时，它能给出醒目的提示。

### B.4 CoffeeScript 中间件

中间件是一种在 Web 框架和服务器中间进行适配的软件，能让 CoffeeScript 的编译透明化，让程序以为自己使用的就是原来的 JavaScript 。

[Rack-coffee](http://github.com/mattly/rack-coffee) 兼容 Rails , Sinatra 及其他所有主流的 Ruby Web 框架。

Barista 也能运行在所有基于 Rack 的框架中。

[CoffeeCup](http://github.com/dec/coffeecup) 为 Django , Pylons , CherryPy 以及其他无数的基于 WSGI 的框架提供了第一流的 CoffeeScript 支持。

### B.5 Node.js 上的 CoffeeScript

在 Node.js 中可是使用 coffee 命令直接运行 .coffee 问题，真正困难的是如何为前端提供编译好的 JavaScript 代码。

Connect （Node.js Web 框架核心标准）提供了自动完成该工作的中间件。

```coffeescript
compiler = connect.compiler src: coffeeDir, enable: ['coffeescript']
static = connect.static coffeeDir
connect.createServer compiler, static
```

## 附录 C JavaScript 开发者备忘录

```coffeescript
x and= y # => x = x && y
x or= y # => x = x || y
x ?= y # => if (typeof x === 'undefined' || x == null) {x=y}
```

CoffeeScript 的 `in` 操作符其实是借用了 Array::indexOf ，如果 Array::indexOf 尚未定义（比如 IE8 ），那就会用等价的函数替代。

[1]: https://github.com/josh/ruby-coffee-script
