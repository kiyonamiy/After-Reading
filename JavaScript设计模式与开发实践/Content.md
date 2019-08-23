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