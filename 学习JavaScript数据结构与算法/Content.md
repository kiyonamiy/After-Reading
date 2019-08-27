## 第1章 JavaScript 简介

### 1.3 JavaScript 基础

#### 1.3.4　相等运算符（==和===）

类型（x）| 类型（y）| 结果
--- | --- | ---
null | undefined | true
undefined | null | true
数 | 字符串 | x == toNumber(y)
字符串 | 数 | toNumber(x) == y
布尔值 | 任何类型 | toNumber(x) == y
任何类型 | 布尔值 | x == toNumber(y)
字符串或数 | 对象 | x == toPrimitive(y)
对象 | 字符串或数 | toPrimitive(x) == y

`toNumber`和`toPrimitive`方法是内部的，并根据以下表格对其进行估值:
`toNumber`方法对不同类型返回的结果如下：
值类型 | 结果
--- | ---
undefined | NaN
null | +0
布尔值 | 如果是true，返回1；如果是false，返回+0
数 | 数对应的值

`toPrimitive`方法对不同类型返回的结果如下：
值类型 | 结果
--- | ---
对象 | 如果对象的valueOf方法的结果是原始值，返回原始值；如果对象的toString方法返回原始值，就返回这个值；其他情况都返回一个错误。

```js
console.log('packt' ? true : false);        // true（字符串长度大于1）。

console.log('packt' == true);               // false    // true => 1 'packt' => NaN     // 1 == NaN
```

## 第2章　ECMAScript和TypeScript概述

### 2.3 介绍 TypeScript

#### 2.3.1 类型推断

```ts
// 啰嗦版
let age: number = 20;
let existsFlag: boolean = true;
let language: string = 'JavaScript';

// 自动推断版
let age = 20; // 数
let existsFlag = true; // 布尔值
let language = 'JavaScript'; // 字符串

language = 10;      // 报错，但还是会继续编译成 ES5 。 Type '10' is not assignable to type 'string'. 
```

#### 2.3.2 接口

```js
/*
第一种TypeScript接口的概念 是把接口看作一个实际的东西。
它是对一个对象必须包含的属性和方法的描述。
*/
interface Person {
  name: string;
  age: number;
}

function printName(person: Person) {
  console.log(person.name);
}

// 在本例中，变量mary的行为和Person接口定义的一样，那么它就是一个Person。（鸭子模型）
const john = { name: 'John', age: 21 };
const mary = { name: 'Mary', age: 21, phone: '123-45678' };
printName(john);
printName(mary);
```

```js
/*
第二种TypeScript接口的概念和面向对象编程相关。
*/
interface Comparable {
  compareTo(b): number;
}

// Comparable接口告诉MyObject类，它需要实现一个叫作compareTo的方法，并且该方法接收一个参数。
class MyObject implements Comparable {
    age: number;
    compareTo(b): number {
        if (this.age === b.age) {
            return 0;
        }
        return this.age > b.age ? 1 : -1;
    }
}
```

```js
/*
泛型
*/
interface comparable<T> {
    compareTo(b: T): number;
}

class MyObject implements comparable<MyObject> {
    age: number;

    compareTo(b: MyObject): number {
        if(this.age == b.age) {
            return 0;
        }
        return this.age > b.age ? 1: -1;
    }
}
```

#### 2.3.4 TypeScript中对JavaScript文件的编译时检查

使用时，只需要在JavaScript文件的第一行添加一句`// @ts-check`，即可在JavaScript中使用一些类型和错误检测功能。