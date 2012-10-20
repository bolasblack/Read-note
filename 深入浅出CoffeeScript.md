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

> 列表解析式时 CoffeeScript 核心设计哲学的产物： CoffeeScript 中所有的东西都是表达式，并且每个表达式有一个值。
