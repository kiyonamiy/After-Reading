# 第一部分 基本知识

## 第一章 面向对象的 JavaScript

### 1.1 动态类型语言和鸭子类型

**动态类型语言**对变量类型的宽容给实际编码带来了很大的灵活性。由于无需进行类型检测，我们可以尝试调用任何对象的任意方法，而无需去考虑它原本是否被设计为拥有该方法。

这一切都建立在**鸭子类型** （duck typing）的概念上，鸭子类型的通俗说法是：“如果它走起路来像鸭子，叫起来也是鸭子，那么它就是鸭子。 ”

### 1.2 多态

#### 1.2.1 一段“多态”的 JavaScript 代码

```js
// 虽然体现了“多态”，
// 但这样的“多态性”是无法令人满意的，如果后来又增加了一只动物，
// 比如狗，显然狗的叫声是“汪汪汪”，此时我们必须得改动makeSound 函数，才能让狗也发出叫声。
const Duck = function() {};
const Chicken = function() {};

const makeSound = animal => {
    if(animal instanceof Duck) {
        console.log('嘎嘎嘎');
    } else if(animal instanceof Chiken) {
        console.log('咯咯咯');
    }
}

makeSound(new Duck());      // 嘎嘎嘎
makeSound(new Chicken());   // 咯咯咯
```

#### 1.2.2 对象的多态性

```js
const makeSound = animal => {
    animal.sound();
}

const Duck = function(){};
Duck.prototype.sound = function() {
    console.log('嘎嘎嘎');
};

const ChiCken = function(){};
ChiCken.prototype.sound = function() {
    console.log('咯咯咯');
};

makeSound(new Duck());      // 嘎嘎嘎
makeSound(new Chicken());   // 咯咯咯


// 忽然来了一只狗
let Dog = function(){};
Dog.prototype.sound = function(){
    console.log( '汪汪汪' );
}

makeSound(new Dog()); 
```

#### 1.2.3 类型检查和多态

#### 1.2.4 使用继承得到多态效果

#### 1.2.5 JavaScript的多态

#### 1.2.6 多态在面向对象程序设计中的作用

换句话说，多态**最根本的作用**就是通过把过程化的条件分支语句转化为对象的多态性，从而*消除这些条件分支语句*。

#### 1.2.7 设计模式与多态

### 1.3 封装

#### 1.3.1 封装数据

#### 1.3.2　封装实现

#### 1.3.3　封装类型

#### 1.3.4　封装变化

23种设计模式分别：
- 创建型模式：封装创建对象的变化。
- 结构型模式：封装对象之间的组合关系。
- 行为型模式：封装对象的行为变化。

### 1.4　原型模式和基于原型继承的JavaScript对象系统

#### 1.4.1 使用克隆的原型模式

原型模式是通过克隆来创建对象（不关心类型）。

```js
// 编写一个飞机大战的网页游戏。某种飞机拥有分身技能
var Plane = function(){
    this.blood = 100;
    this.attackLevel = 1;
    this.defenseLevel = 1;
};

var plane = new Plane();
plane.blood = 500;
plane.attackLevel = 10;
plane.defenseLevel = 7;

// 原型模式的实现关键，是语言本身是否提供了clone 方法。
var clonePlane = Object.create( plane );
console.log( clonePlane );   // 输出：Object {blood: 500, attackLevel: 10, defenseLevel: 7}
```

#### 1.4.2　克隆是创建对象的手段

在用Java等静态类型语言编写程序的时候，类型之间的解耦非常重要。依赖倒置原则提醒我们创建对象的时候要**避免依赖具体类型**，而用new XXX 创建对象的方式显得**很僵硬**。

工厂方法模式和抽象工厂模式可以帮助我们解决这个问题，但这两个模式会带来许多跟产品类平行的工厂类层次，也会增加很多额外的代码。

#### 1.4.3　体验Io语言

#### 1.4.4　原型编程范型的一些规则

#### 1.4.5　JavaScript中的原型继承

1. 所有的数据都是对象

JavaScript在设计的时候，模仿Java引入了两套类型机制：基本类型和对象类型。（从现在看来，这并不是一个好的想法。）

2. 要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它

补充：
```js
// Array.prototype.slice.call() 能把类数组对象转化成数组

var a= { 
    length: 2, 
    0:'lai',
    1:'hua'
}; // 类数组, 有length属性，长度为2，第0个是lai，第1个是hua

console.log(Array.prototype.slice.call(a,0)); // ["lai", "hua"],调用数组的slice(0);
```

{!!!} 理解 new 运算的过程：
```js
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function() {
    return this.name;
}

const objectFactory = function() {
    //  从 Object.prototype 上克隆一个空的对象
    const obj = new Object();
    
    // 取得外部传入的构造器，此例是 Person
    const Constructor = Array.prototype.shift.call(arguments);

    // 指向正确的原型
    obj.__proto__ = Constructor.prototype;

    // 借用外部传入的构造器给obj设置属性
    const ret = Constructor.apply(obj, arguments);      // 第一个参数 构造函数 已经被 shift 掉，剩下的就只是参数

    // 确保构造器总是会返回一个对象
    return typeof ret === 'object' ? ret : obj;     
}

// 二者产生一样的结果
const myObj1 = objectFactory(Person, 'Kiyonami');
const myObj2 = new Person('Kiyonami');

```

3. 对象会记住它的原型

其实**并不能说**对象有原型，而只能说**对象的构造器**有原型。

对于“对象把请求委托给它自己的原型”这句话，更好的说法是对象把请求委托给它的构造器的原型。那么对象如何把请求顺利地转交给它的构造器的原型呢？

JavaScript给对象提供了一个名为`__proto__`的隐藏属性，某个对象的`__proto__`属性默认会指向它的构造器的原型对象，即`{Constructor}.prototype`。

4. 如果对象无法响应某个请求，它会把这个请求委托给它的构造器的原型

#### 1.4.6 原型继承的未来



