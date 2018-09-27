---
title: typescript
date: 2018-09-20 09:02:52
tags: typescript javascript
---

typescrpt(以下简称为ts)是js的超集。ts扩展了JavaScript的语法，
   ## 一、数据类型
          1. number

```
    const num:number = 123;
```

          2. string

```
    const str:string = 'this is a numebr'
```
          3. boolean

```
    const flag:boolean = true;
```

          4. undefined
 
```
    const undef:undefined = undefined;
```

          5. null
  
```
    const vo:null = null
```
  >  以上跟之前的es5(6)使用一样

          6. array
  > 声明方式有两种+一种

 ```javasctipt
  const arr:Array<number> = [1,2,3,4]
  const arr2:string[] = ['123','2','3']
  const arr3:any[] = ['123',22,true]
```
  
          7. 元组
 >  听名字就可以联想到数组，它属于数组
 >  

```
    const tuple:[number, string] = [123, 'this is ts'];
    
    const arr:Array<[number, string]> = [[123, 'this is ts']]

```

          8. 枚举
```
/*
enum 枚举名{ 
    标识符[=整型常数], 
    标识符[=整型常数], 
    ... 
    标识符[=整型常数], 
};
*/
enum Flag { success = 1, error = 2 }
let flag:Flag = Flag.success // 1
// 可以应用于状态码
// 如果标识符没有赋值，那么它的值就是下标
// 标识符可以带字符串或者不带，eg: success, 'success'
```

          9. void
 > typescript中的void表示没有任何类型，一般用于定义方法的时候方法没有返回值
```
function run():void { // 这里不能写undefined
    console.log('run')
}
```
          10. never
> 是其他类型(包括null和undefined)的子类型，代表从不会出现的值

          11. any
 > 任意类型，不容限制就跟es5中的变量一样了


## 二、函数

1. 函数的定义
> es5中定义函数有两种，函数声明式和函数表达式
```
   // 参数要写类型，返回值也要写类型
   function getInfo(name:string, age:number):string {
       return `{$name} --- ${age}`;
   }
   const getInfo2 = (name:string, age:number):string {
       return `{$name} --- ${age}`;
   }
   
```
2. 可选参数
> es5里面实参和形参可以不一样，但是ts必须一样，如果不一样就需要配置可选参数(必须配置到参数的最后面)
```
function getIfno(name:string, age?:number):string {
    return age 
        ? `${name} === ${age}`
        : `${name} === 年龄保密`
}
alert(getInfo('zhangsan'));
alert(getInfo('zhangsan', 123));
```
3. 默认参数
> es5中没有，es6和ts中有
```
function getInfo(name:string, age:number = 20):string {
    return age 
        ? `${name} === ${age}`
        : `${name} === 年龄保密`
}
alert( getInfo('张三'));
alert( getInfo('张三',30));
```
4. 剩余参数（扩展运算符）
```
    function sum(a:number, b:number, c:number, d:number):number {
        return a + b + c + d
    }
    // 以下是简写
    function sum2(...res:number[]):number {
        return res.reduce((a, b) => a + b, 0)
    }
```
5. 函数重载
> java中方法的重载：是指两个或两个以上的同名函数，但它们参数不一样，这时会出现函数重载的情况。
> ts中的重载：通过为同一个函数提供多个函数类型定义来试下多种功能的目的。
```
 // es5中出现同名函数会覆盖
 function getInfo(name:string):string;
 function getInfo(age: number):string;
 function getInfo(str:any):any
 {
     if (typeof str === 'string') {
         return `我叫：${str}`
     } else if (typeof str === 'number') {
         return `我的年龄:${str}`
     }
 } 
 
 //
 function getInfo(name:string):string;
 function getInfo(name:string, age:number):string;
 function getInfo(name:any, age?:any):any {
     /*
     一系列操作
     */
 }
```
6. 箭头函数
> 同es6

## 三、类
1. ES5的类
```
// 1. 简单类
function Person() {
    this.name = '张三';
    this.age = 22;
}
const p = new Person()
// 2. 构造函数和原型链里面增加方法
function Person() {
    this.name = '张三';
    this.age = 20;
    this.run = function() {
        alert(`${this.name}在运动`)
    }
}
// 3. 原型链上面的属性会被多个实例共享 构造函数不会
Person.prototype.sex = '男'
Person.prototype.work = function() {
    alert(`${this.name}在工作`)
}
// 4. 静态方法
Person.getInfo = function() {
    // 静态方法
}
// 5. 继承 对象冒充实现继承
function Web() {
    Person.call(this)
}
const w = new Web()
w.run() // 可以继承构造函数里面的方法
w.work() // 没法继承原型链的方法
// 6. 原型链继承
function Web() {
}
Web.prototype = {
    constructor: Person,
}
// 或写成
// Web.prototype = new Person()
// 这种无法传参
// 7. 原型链+对象冒充组合继承模式
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.run = function() {
        // 
    }
}
Person.prototype.sex = '男'
Person.prototype.work = function() {
    //
}
function Web(name, age) {
    Person.call(this, name, age)
}
Web.protoType = new Person();


、、、

```
2. Typescript中的类
```
// 1. 定义
class Person {
    name:string; // 属性，前面省略了public关键词
    constructor(name:string) {
        this.name = name;
    }
    run():void {
        alert(this.name);
    }
    getName():string {
        return this.name;
    }
    setName(name:string):void {
        this.name = name
    }
}
const p = new Person('zhangsan')
p.run()

// 2. 继承 
class Student extends Person {
    constructor(name:string){
        super(name)
    }
    run():string() {
        return `${this.name}在运动-子类`
    }
    work():void {
        alert(`${this.name}在工作`)
    }
}
// 3. 修饰符
/*
public: 公有(默认)     在当前类里面、子类、类外面都可以访问
protected: 保护  在当前类里面、子类里面可以访问，在类外部没法访问
private: 私有    在当前里面可以访问，子类、类外部都没法访问
*/
// 4. 静态属性 静态方法
class Per {
    public name:string;
    public age:number = 20;
    
    static sex = '男' // 静态属性
    constructor(name:string) {
        this.name = name
    }
    run() {
        alert(`${this.name}在运动`)
    }
    work() {
        alert(`${this.name}在工作`)
    }
    static print() { // 静态方法，无法调用类里面的属性
        alert(`print方法${Per.sex}`)
    }
}
// 5. 多态
// 父类定义一个方法不去实现，让继承它的子类去实现，每个子类有不同的表现
class Animal {
    name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat() {}
}
class Dog extends Animal {
    constructor(name:string) {
        super(name)
    }
    eat() {
        return this.name + '吃粮食'
    }
}
class Cat extends Animal {
    constructor(name:string) {
        super(name)
    }
    eat() {
        return this.name + '吃老鼠'
    }
}
// 抽象类
/*
提供其他类继承的基类，不能直接被实例化。
用abstract关键词定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。
abstract抽象方法只能放在抽象类中，
抽象类和抽象方法用来定义标准。
*/
abstract class Animal {
    public name:string;
    constructor(name:string) {
        this.name = name
    }
    abstract eat():any {}
    run():void {
        /*...*/
    }
}
```
## 四、接口
> 作用：在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范，在程序设计里面，接口起到一种限制和规范的作用。接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。接口类似于java，同时还增加了更灵活的接口类型，包括属性、函数、可索引和类等。

1. 属性类接口
```
function printLabel():void {
}
printLabel()

function printLabel(label:string):void {
}
printLabel('this is ts')
// ts中自定义方法传入参数，对json进行约束
function printLabel(labelInfo:{ label:string}):void {
}
//printLable('hhah') // 错误
//printLable({name: '123'}) // 错误
printLable({lable: '123'}) // 正确

interface FullName {
    firstName: string;
    secondName?: string;
}
function printName(name:FullName):void {
    const { firstName, secondName } = name;
    console.log(`${firstName} -- ${secondName}`);
}

```
2. 函数类型接口
> 对方法传入的参数 以及返回值进行约束

```
interface encrypt {
    (key:string, value:string):string;
}
var md5:encrypt = function(key:string, value:string):string {
    return key + value
}
```
3. 可索引接口
> 数组、对象的约束（不常用）

```
interface UserArr {
    [index:number]:string
}
var arr:UserArr = ['aaa', 'bbb'];

interface UserObj {
    [indx:string]:string
}
var arr:UserObj = { name: '张三' }
```
4. 类类型接口
> 对类的约束和抽象类抽象相似

```
interface Animal {
    name:string;
    eat(str:string):void
}
class Dog implements Animal {
    name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat(food:string) {
        console.log(`${this.name}吃${food}`)
    }
}
```
5. 接口扩展
> 接口可以继承接口

```
interface Animal {
    eat():void;
}
interface Person extends Animal {
    work():void;
}
class Web implements Person {
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat() {
        console.log(`${this.name}...`)
    }
    work() {
        console.log(`${this.name}...`)
    }
}
```
## 五、泛型
1. 定义
> 软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。
> 组件不仅能够支持当前的数据类型，同时也要支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。
> 在像C#和Java面向语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。这样用户就可以以自己的数据类型来使用组件。
> 通俗理解：泛型就是解决类、接口、方法的复用性、以及对不特定数据类型的支持（类型校验）
2. 函数
```
function getData(value:string):string {
    return value
}
// 同时返回string类型和number类型，就需要写两个函数
//  any可以解决这个问题，any放弃了类型检查，传入什么，返回什么。如果设置返回值的类型，就可以解决。

// 泛型:可以支持不特定的数据类型，要求传入的参数和返回的参数一致
// T表示泛型，具体什么类型是调用这个方法的时候决定的
function getData<T>(value: T):T {
    return value
}
getData<string>('类型一致 string');
```
3. 类
```
// 比如有个最小堆算法，需要同时支持返回数字和字符串 a-z两种类型
class MinClass<T> {
    public list:T[] = [];
    add(value:T):void {
        this.list.push(value)
    }
    min():T {
        const minNum = this.list[0];
        for(let i = 0; i < this.list.length; i++) {
            minNum > this.list[i] && minNum = this.list[i];
            return minNum;
        }
    }
}
var m1 = new MinClass<number>();
```
4. 接口
```
interface ConfigFn {
    <T>(value: T):T;
}
const getdata:ConfigFn = function<T>(value:T):T {
return value
}
```

