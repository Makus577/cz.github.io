# javascript常用方法
1. 获取DOM的children
    > firstChild除了会获取到dom节点，还会获取到之间的空格（代码未压缩）
    > firstElementChild会获取到第一个dom子节点
    > 常规的方法是使用children

2. 获取到DOM的CSS样式
     > 直接使用dom.style.property，只适合行内样式
     > 使用getComputedStyle(element, null|''|:before|:after)
