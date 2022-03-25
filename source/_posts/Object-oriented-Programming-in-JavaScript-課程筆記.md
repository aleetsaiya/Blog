---
title: Object-oriented Programming in JavaScript 課程筆記
date: 2021-06-10 01:16:34
---

# Javascript mastery series-2

## 簡介
Mosh 的 Javascript 系列課程分別為 javascript mastery series-1 以及 javascript mastery series-2，這一篇為 javascript mastery series-2 的筆記。

## 內容
這份課程比起前一部 ( series-1 ) 的內容，更注重在介紹 Javascript Objected Orient Type ( OOP ) 的特性，而 series-1 與 series-2 兩部課程都沒有介紹到常用的 DOM 操作或是 Javascript 的實務應用，而是注重在講解 Javascript 的基礎觀念。
- [x] Objects
- [x] Prototypes
- [x] Prototypical Inheritance
- [x] ES6 Classes
- [x] Modules

## OOP 四個特性
1. 封裝（Encapsulation）
2. 抽象（Abstraction）
3. 繼承（Inheritance）
4. 多型（Polymorphism）

## Object
這部分的影片有一半以上跟 series-1 的教學影片一樣，所以這邊就打一些之前上過的或是比較沒印象的。
### Private Properties
如果不想要將Properties顯示在物件上進行操作的話，只要將變數用 `let` 宣告就可以了。
```javascript
function Circle(color){
    this.color = color;
    let radius = 10;
    this.square = function(){
        return Math.PI * radius * radius;
    }
}

const circle = new Circle('blue');

for(let property in circle){
    console.log(property); //color, square
}
```
### private property 讀取
使用 Object.defineProperty 並使用 getter 讓新屬性只能讀取
```javascript
const sw = new Stopwatch();

function Stopwatch() {
    let duration = 1.5;
    Object.defineProperty(this, 'duration', {
        get: function() {
            return duration;
        }
    });
}

console.log(sw.duration);
```

### Object 操作練習
寫一個 Stopwatch Object，start 能開始計算時間，stop 能停止計算時間，resrt 能重新設定時間，duration 為持續時間。
```javascript
const sw = new Stopwatch();

function Stopwatch() {
    let srartTime, endTime, running, duration = 0;
    
    this.start = function() {
        if (running)
            throw new Error('Stopwatch has already started.');
        
        running = true;
        startTime = new Date();
    };
    this.stop = function() {
        if(!running)
            throw new Error('Stopwatch is not started.');

        running = false;
        endTime = new Date();
        const seconds = (endTime.getTime() - startTime.getTime()) / 1000;
        duration += seconds;
    };
    this.reset = function() {
        startTime = null;
        endTime = null;
        running = false;
        duration = 0;
    };
    Object.defineProperty(this, 'duration', {
        get: function() {
            return duration;
        }
    });
}
```

## Prototypes
+ Prototype 就是一個 Object 的 Parent，而連接的方式為 `__proto__`。
+ 同一個 Constructor Function 擁有同樣的 prototype。

範例: 

<img src="https://user-images.githubusercontent.com/67775387/118945718-4b014f80-b988-11eb-961d-dc8210258e9d.png" alt="prototype" width="450"/>

```javascript
function Circle(radius) {
    this.radius = radius;

    this.draw = function() {
        console.log('draw');
    };
}

const circle = new Circle(10);
```

prototype 的相關文章 :
+ [該來理解 JavaScript 的原型鍊了](https://blog.techbridge.cc/2017/04/22/javascript-prototype/)
+ [你懂 JavaScript 嗎？#19 原型（Prototype）](https://cythilya.github.io/2018/10/26/prototype/)

## Prototypical Inheritance
### 如何實作 Prototype-based 的繼承 ? 
```javascript
function extend(Child, Parent) { //傳入兩個Constructor function
    Child.prototype = Object.create(Parent.prototype); //建立一個繼承 Parent 的空物件，放入Child prototype
    Child.prototype.constructor = Child; // //因為原本的Child.prototype.constructor 被覆蓋掉了，所以重新設定 constructor function
}
```
ex. 讓 Circle Obejct 繼承 Shape Object
```javascript
//make Cicle inheritance Shape

function Shape(color){ //Shape 的 Constructor function
    this.color = color;
}

function Circle(radius, color) { //Circle 的 Constructor function
    Shape.call(this, color); //代替super.color
    this.radius = radius;
}

Circle.prototype = Object.create(Shape.prototype); //回傳一個繼承 Shape.prototype 的空物件，並放入Circle prototype
Circle.prototype.constructor = Circle; //因為原本的Circle.prototype.constructor 被覆蓋掉了，所以重新設定 constructor function

Circle.prototype.say = function() { //add function in Circle prototype
    console.log('say');
}

Shape.prototype.hi = function() { // add function in Shape prototype
    console.log('HI');
}

circle = new Circle.prototype.constructor(10, 'red'); //等同於 circle = new Circle(10);
console.log(circle);

// circle >> Cicle prototype >> Shape prototype >> Object prototype
// radi,color >>  say: func  >>    hi: func     >>    toString... 

```
### Mixin
將不同 Object 的 Property 融合到一個 Prototype 中。
```javascript
function mixin(target, ...sources){
    Object.assign(target, ...sources);
}
```

範例:
```javascript
function mixin(target, ...sources){
    Object.assign(target, ...sources);
}

const canEat = {
    eat: function() {
        this.hunger--;
        console.log("eating");
    }
};

const canWalk = {
    walk: function() {
        console.log('walking');
    }
};

const canSwim = {
    swim: function() {
        console.log('swim');
    }
}

function Person() {
    this.height = 165;
    this.weight = 55;
}

function Fish() {
    this.length = 30;
}


mixin(Person.prototype, canEat, canSwim, canWalk);
mixin(Fish.prototype, canEat, canSwim);


let person = new Person();
for (let property in person)
    console.log(property);  // height, eat, swim, walk

console.log('----------');

let fish = new Fish();
for (let property in fish)
    console.log(property); // length, eat, swim
```

## ES6 Classes
ES6 的 `class` 只是原本 constructor function 的 syntax sugar，而要注意的地方是**class 不會 Hoisting**。

```javascript
class Circle {
    constructor(radius) {
        this.radius = radius;
    }
    //prototype 
    draw() {
        console.log('draw');
    }
}
```

### Static  method
不用創立一個 instance 就可以直接使用的 method。
```javascript
class Circle {
    constructor(radius) {
        this.radius = radius;
    }
    //prototype 
    static draw() {
        return 'draw';
    }
}

console.log(Circle.draw()); //draw
```

### 繼承直接 extends 就好
要注意的地方是，如果 parents 中包含 constructor function，則需在 constructor function 中呼叫 `super` ，否則會出錯。
```javascript
class Shape {
    constructor(color) {
        this.color = color;
    }
    move() {
        console.log('move');
    }
}

class Circle extends Shape{
    constructor(color, radius) {
        super(color);
        this.radius = radius;
    }
    draw() {
        console.log('draw');
    }
}

const c = new Circle('red', 10);
```

### Module
在 ES6 中，輸出一個 `class` 。
```javascript
const _radius = new WeakMap();

// Public Interface
export class Circle {
    constructor(radius) {
        _radius.set(this, radius);
    }

    draw() {
        console.log('Circle with radius ' + _radius.get(this));
    }
}
```
ES6 中 import 一個 class
```javascript
import {Circle} from './circle';

const c = new Circle(10);
c.draw();
```

## 後記
上完光頭的 javascript 兩份課程後，對javascript的理解程度真的比以前還要深很多，只是因為裡面都沒包含那種實作小工具的練習，所以現在打算多做一些 javascript 的實作增加自己的熟練度，而觀念的話看完這兩部教學基本上就有基礎的觀念了。相較於前一部，*The Ultimate JavaScript Mastery Series - Part 2* 講的深蠻多的。
