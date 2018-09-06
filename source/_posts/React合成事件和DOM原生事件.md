---
title: React合成事件和DOM原生事件
date: 2018-09-06 08:56:03
tags: react 
---
首先我们在jsx绑定的事件，都是委托在document上，当事件发生并冒泡到document时，React将事件内容**封装**并交由真正的处理函数运行。
> 封装：合成事件的Event并不是原生Event，而是封装的一层。
> 合成事件层（中间层）：SyntheticEvent，避免DOM绑定过多的事件处理函数，整个页面响应以及内存占用受到影响，而且还兼容不同浏览器之间的事件系统差异。
> Event对象，合成事件的Event是复用的，所以在事件处理函数执行完之后，属性都会被清空，所以event的属性无法被异步访问。

1. 冒泡响应顺序
    原生事件先响应，合成事件后响应。
2. 阻止冒泡
    1. evt.stopPropagtion()
        如果在合成事件内调用，那么只能阻止合成事件之前的冒泡，不能阻止原生事件。
        如果在原生事件调用，那么会阻止吊所有的合成事件执行。
    2. evt.nativeEvent.stopImmediatePropagation()
        evt.nativeEvent是原生Event对象，但是作用不大，这个函数只能阻止document上的事件监听器。其中还要注意事件绑定顺序问题，如果在ReactDOM.render之前绑定的document事件，那么是无法阻止的（这种情况一般不会发生）。
3. 事件捕获
  合成事件的代理在document只注册冒泡阶段的事件监听器。代理内部会有一个path，path的顺序是从child到parent，注册所有的事件监听函数，captured和bubbled都会进行标记，等到执行的时候，如果是captured的话，那么执行顺序就是从后到前，如果是bubbled的话，那么执行顺序是从前到后