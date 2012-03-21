# SeaJS 源代码中的正则表达式介绍

由于 JavaScript 无法在正则表达式中添加注释，故而代码是用 CoffeeScript 写的

再规范一下用词

*: 任意数量（零到无限）

+: 若干（至少多于一）

?：没有或一个

(.*): 在小括号里的一致称之为子表达式

## 一些疑问
str.indexOf(params) === -1 相对于 ~str.indexOf(params) 有任何优势吗？

这段不理解
``` JavaScript
      // For some cache cases in IE 6-9, the script executes IMMEDIATELY after
      // the end of the insertBefore execution, so use `currentlyAddingScript`
      // to hold current node, for deriving url in `define`.
      currentlyAddingScript = node;
      head.insertBefore(node, head.firstChild);
      currentlyAddingScript = null;
```
说是会立即执行，闭包就不会了？
而且获取的代码在另外一个函数里，似乎这赋值与消除之间也没有回调函数，怎么能够在 getCurrentScript 函数里获取到 currentlyAddingScript ？
还有就是为什么要 insertBefore ？

这段不理解
``` JavaScript
    var scripts = head.getElementsByTagName('script');
    for (var i = 0; i < scripts.length; i++) {
      var script = scripts[i];
      if (script.readyState === 'interactive') {
        interactiveScript = script;
        return script;
      }
    }
```
readyState 有四个状态 uninitialized、loading、interactive、complete 为什么 complete 不行？会不会遗漏？

## dirname
``` CoffeeScript
 # Extracts the directory portion of a path.
 # dirname('a/b/c.js') ==> 'a/b/'
 # dirname('d.js') ==> './'
 # @see http://jsperf.com/regex-vs-split/2 (传说这样是若干方法中效率最高的)
 s = path.match ///
     .*                                 # 任意内容
     (?=                                # 被接下来的子正则匹配的内容不会出现在匹配结果中
     \/.*$                              # 匹配出以“/”开头且到字符串结尾部分依旧没有再出现“/”的字符串
     )
     ///
 return (if s then s[0] else '.') + '/' # 如果 s 非空，那么就把匹配的第一个结果拿出来（其实打死也就只有一个结果罢了），加上一个 '/'
```

## realpath
``` CoffeeScript
 # 'file:///a//b/c' ==> 'file:///a/b/c'
 path = path.replace ///
     (            # 先匹配出以若干斜杠结尾的字符串，比如上面的注释，先匹配出 file: a b 三个字符串
     [^:\/]       # 过滤掉其中有 : 和 / 的匹配，所以这个时候就只剩下 a 和 b 了
     )
     \/+
     ///g, '$1\/' # 将第一个子表达式的匹配结果
```

## getHost
``` CoffeeScript
 getHost = (url) ->
     url.replace ///
     ^         # 从字符串头开始 
     (
     \w+:\/\/  # 若干ASCII单字字符 + ://
     [^\/]*    # 到 /? 之前的所有除了 / 以外的字符
     )
     \/?.*$    # 以上的匹配只能匹配 /? 到字符串尾以外的内容
     ///, '$1' # 替换为第一个子表达式
```

## removeComments
``` CoffeeScript
 removeComments = (code) ->
     # 先替换掉 /* */ 形式的注释，再替换掉 // 形式的
     code.replace(///
         (?:^|\n|\r)      # 匹配行头或者折行，但不记录匹配结果
         \s*              # 匹配任意数量的空格
         \/\*[\s\S]*?\*\/ # 匹配尽量少的匹配（非贪心模式） /* */ 及他们之间的除了换行符以外的任意字符
         \s*
         (?:\r|\n|$)      # 匹配行尾或者折行，但不记录匹配结果
         ///g, '\n')
         .replace(///
         (?:^|\n|\r)
         \s*              # 匹配任意数量的空格
         \/\/.*           # 匹配 // 及 // 后到下个子正则之间的任意字符
         (?:\r|\n|$)
         ///g, '\n')
```

## parseDependencies
``` CoffeeScript
 # 正则对象的 exec 函数会返回一个数组，其中下标为 0 的元素为该正则对象匹配的结果，接下
 # 来的元素下标为多少就是第几个子正则的匹配结果
 pattern = ///
     (?:^|[^.])   # 匹配行头或者 \n 但不记录匹配结果
     \brequire    # 匹配位于边界处（字符串的开头或者结尾）的 require
     \s*          # 匹配任意数量的空格
     \(
     \s*
     (["'])       # 匹配一个 " 或者 '
     ([^"'\s\)]+) # 匹配若干除了 " ' 空格 ) 以外的字符
     \1           # 与第一个子正则相同
     \s* 
     \)
     ////g;
 # ... some code and defined 'code'
 match = pattern.exec(code)
 while ((match = pattern.exec(code))) {
   if (match[2]) {
     ret.push(match[2]);
   }
 }
```
