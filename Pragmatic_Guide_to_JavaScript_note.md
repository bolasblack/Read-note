JavaScript 修炼之道笔记
===

方括号操作符
---
利用 JavaScript 的方括号操作符我们可以做到动态的调用方法及熟悉，比如

    window['history'] = ...

由于传入的是一个字符串，所以几乎所有结果能够获得字符串的代码都能在方括号里使用，比如：

    window[isHistory?'history':'other']
    window[(enable?'add':'delete')+'action']
    window[{firstName:'foo',lastName:'bar'}[needFirst?'firstName':'lastName']]

>值得注意的是，如果你在向方法传递的参数上大量使用这个技巧，将使代码变得难以阅读，此时使用常规的 if/else 结构更加明智


