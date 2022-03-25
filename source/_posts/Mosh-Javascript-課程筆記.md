---
title: JavaScript Tutorial for Beginners 課程筆記
date: 2021-06-09 16:07:37
---

# Javascript mastery series-1

> Mosh 的 Javascript 系列課程分別為 javascript mastery series-1 以及 javascript mastery series-2，這一篇為 javascript mastery series-1 的筆記。

## 內容
這部課程的內容基本上從最基礎的變數開始介紹，然後延伸介紹各種變數的特性以及使用方式。而 series-1 與 series-2 兩部課程都沒有介紹到常用的 DOM 操作或是 Javascript 的實務應用，而是注重在講解 Javascript 的基礎觀念。
- [x] Primitive-type 與 Reference-type
- [x] OOP
- [x] Object, Array, Function
- [x] Factory Function 與 Constructor Function
- [x] Call-by-value 與 Call-by-reference
- [x] Hoisting
- [x] let 與 var 的差別
- [x] this 的意思

**Javascript mastery series-2** : [Javascript OOP 觀念](https://github.com/aleetsaiya/Blog/issues/18)

## ES6
ECMAScript (簡稱 ES6)，Javascript 語言新一代標準，一般是指 ES2015 標準。

參考：[JavaScript ES6 介紹](https://www.fooish.com/javascript/ES6/)

## 變數型態
`typeof` variable
### primitives types
+ number
+ string
+ boolean
+ undefined
+ null
### reference types
+ object
```javascript
let person = {
    name: 'ALEE',
    age: 30
};
// dot notation
person.name = 'Alex';

//bracket notation
person['name'] = 'Yo';
```
+ array
> array 也是一個object
```javascript
//create array
let selectedColors = ['red', 'blue'];

// add
selectedColors[2] = 'green';

// different type
selectedColors[3] = 5
```
+ function
> function也是一個object ...
```javascript
function square(number){
    return number * number;
}

let number = square(2);
```

## Operators
+ `+`, `-`, `*`, `/`, `**`
+ `==`,  `===`, `!==`
```javascript
/* strict equality (type + value) */
console.log(1 === 1); //true
console.log('1' === 1); //false

/* lose equality (value) */
console.log(1 == 1); //true
console.log('1' == 1); //true
console.log(true == 1); //true

```
+ `&&`, `||`
```javascript
//when a operand is truthy it returnd
console.log(false || true); //true
console.log(false || 'Mosh'); //"Mosh"
console.log(false || 1); //1

/*  Falsy   */
undefined, null, 0, false, '', NaN

// Truthy >> anything that is not Falsy

let userColor = undefined;
let defaultColor = 'blue';
let currentColor = userColor || defaultColor;

console.log(currentColor); //blue
```
## Control Flow
+ `if... else if ... else`
+ `switch`
```javascript
let role;
switch(role){
    case 'geust':
        console.log('guest user');
        break;
    case 'moderator':
        console.log('moderator user');
        break;
    default:
        console.log('Unknown User');
}
```
+ `while`, `do ... while`
> *do while loop至少會執行一次, while loop不一定會執行*

+ `for...in`, `for...of`
```javascript
const person = {
    name: 'Alee',
    age: 30
}

for(let key in person){
    console.log(key, person[key]);
}

const colors = ['red', 'green', 'blue'];

for(let index in colors){ //for...in 指向property
    console.log(colors[index]);
}

for(let color of colors){ //for...of 指向items
    console.log(color);
}
```

## Object-oriented Programming (OOP)
> 將物件作為程式的基本單位，如果一個function在object裡面的話，要把它稱之為 `method`。

```javascript
//Object-oriented Programming (OOP)
const circle = {
    radius: 1,
    location:{
        x: 1,
        y: 1
    },
    isVisible: true,
    draw: function(){
        console.log('draw');
    }
};

circle.draw(); //Method

```


### 變數命名方式
+ *Camel Notation* : 第一個字小寫，後面的都大寫  `oneTwoThreeFour`
+ *Pascal Notation* : 第一個就大寫 `OneTwoThreeFour`


## 建立Object方式
> 為了讓程式師在debug的時候不用一次改太多程式碼(很麻煩)，通常會使用底下兩種方式來建立`客製化的Object`



### Factory Function
呼叫function，回傳一個object。使用Pascal Notation命名
```javascript
// Factory Function
function createCircle(radius){
    return { //Object
        radius, //等同於 radius:radius
        draw(){
            console.log('draw');
        }
    }
}

const circle1 = createCircle(1);
console.log(circle1);

const circle2 = createCircle(2);
console.log(circle2);
```


### Constructor Function
利用 `new`，建立一個空的物件，並在function中建立物件完整資訊(*this*)。使用Camel Notation命名

```javascript
//Constructor Function (Pascal Notation)
function Circle(radius){
    this.radius = radius;
    this.draw = function(){
        console.log('draw');
    }
}

const circle = new Circle(1);
```
**使用 *new* 時我們做了什麼?**
1. 建立一個empty-object `{}`
2. 根據 `new` 後面的 constructor function，function中的 `this` 會指向empty-object `{}`，使獲得 `properties`

## Dynamic Object
在javascript中，object中的properties是可以隨時增加或是刪除的。

### 刪除property
```javascript
const circle = {
    radius: 1
};

delete circle.radius;
```

### 檢查property是否存在
```javascript
const circle = {
    radius: 1,
    draw: function(){
        console.log('draw');
    }
};

if ('radius' in circle)
    console.log('yes')
```

### 複製object
```javascript
const circle = {
    radius: 1,
    draw: function(){
        console.log('draw');
    }
};

const hello = {
    hello: 'yo'
}

// 複製object
// const another = {};
// for (let key in circle)
//     another[key] = circle[key];

const another = {...circle, ...hello};
console.log(another);
```


## Constructor
每一個object都有一個 `constructor` 屬性，用來指向創立那個object的function。(前面提到的 *constructor function*)。
通常我們在寫 *let x = {}* 的時候，就等同於 *let x = new Object()*
```javascript
let x = {};
//let x = new Object();

//Constructor Function
function Circle(radius){
    this.radius = radius;
    this.draw = function(){
        console.log('draw');
    }
}

console.log(x.constructor); // return "Object" function
console.log(new Circle(1).constructor); //return "Circle" function

let s = ''
// let s = new String();
console.log(s.constructor); // return "String" function

let n = 10
console.log(n.constructor); //return "Number" function
```

## call by value 跟 call by reference
前面有提到，變數可以分成 *primitives type* 以及 *reference type*，這兩種最大的差別就是前者是 *call by value* ，而後者是 *call by reference*。

+ ***call by value***

兩個變數的記憶體位置，分別存放他們各自擁有的值。

```javascript
let x = 10;
let y = x;
x = 20;
console.log(y); //10
```

+ ***call by reference***

而call by reference的話，兩個變數的記憶體位置裡面存放的不是值，是***指向其它記憶體的位置***。
```javascript
let x = {value:10};
let y = x;
x.value = 20
console.log(y); //20
```
## ES6 templates literals

相較於原本的 string 使用 `""` 包起來，這裡使用 ` `` `把字串包起來，可以解決平常使用字串會需要特殊處理的文字，像是要顯示 
`"` 這個符號的時候，使用 templates literals就不用特殊處理。
```javascript
const another = `This is my
first' message`;
```

使用 *${ }*，將變數放在字串中:
```javascript
const name = 'Alee';
const message = 
`Hi ${name},

Thank you for your reply.

by Alex.
`
console.log(message);
/*
Hi Alee,

Thank you for your reply.

by Alex.
*/
```

## Array

### 增加元素
```javascript
const numbers = [3, 4];

// End
numbers.push(5, 6);

// Begining
numbers.unshift(1, 2);

// Middle
// 增加或是刪除都用這個api
numbers.splice(2, 0, 9, 9, 9) //(start, deleteCount, [items wanna add])

console.log(numbers); // [1, 2, 9, 9, 9, 3, 4, 5, 6]
```

### 找尋元素
*primitive type* 與 *reference type*的找尋方式不一樣

+ ***primitive type***
```javascript
const numbers = [1, 2, 1, 3, 4];

// return -1 if not in array
console.log(numbers.indexOf(1));
console.log(numbers.lastIndexOf(1));

// true or false
console.log(numbers.includes(1));
```

+ ***reference type***
```javascript
const courses = [
    {id: 1, name: 'b'},
    {id: 2, name: 'c'},
    {id: 3, name: 'd'},
];

// 根據自己定義的function來找元素
let found = courses.find(function(course){
    return course.name === 'c';
});

let index = courses.findIndex(function(course){
    return course.name === 'd';
});

console.log(found); //{ id: 2, name: 'c' }
console.log(index); //2
```

### 刪除元素
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

// End
numbers.pop()

// Begining
numbers.shift(numbers);

// Middle
// 增加或是刪除都用這個api
numbers.splice(2, 1);
```

### 切割 / 合併陣列
```javascript
const first = [1, 2, 3];
const second = [4, 5, 6];

const combine = first.concat(second); //[ 1, 2, 3, 4, 5, 6 ]

const combine = [...first, 'a', ...second, 'b'];
/*[
  1, 2, 3, 'a',
  4, 5, 6, 'b'
]*/

const slice = combine.slice(2); //[3, 4, 5, 6]
```

### 排序陣列
```javascript
const numbers = [2, 3, 1];

numbers.sort();
console.log(numbers); // [ 1, 2, 3 ]

numbers.reverse();
console.log(numbers); //[ 3, 2, 1 ]
```

### 其它常用的 Array API
```javascript
//every
const numbers = [1, 2, 3];
const allPostive = numbers.every(n => n >= 0);
console.log(allPostive); //true

//some 
const numbers2 = [-5, 0, -20];
const atLeastOnePostive = numbers2.some(n => n >= 0);
console.log(atLeastOnePostive); //true

//filter
const filtered = numbers.filter(n => n >= 0);
console.log(filtered); //[1, 2, 3]

//map
const items = filtered.map(n => '<li>' + n + '</li>');
console.log(items); //[ '<li>1</li>', '<li>2</li>', '<li>3</li>' ]
```


## Function

### 宣告方式

+ ***Function Declaration***
```javascript
function walk(){
    console.log('walk');
}
```

+ ***Function Expression***
```javascript
// Anonynous Function Expression
const run = function(){
    console.log('run');
};
```

### Hoisting
當我們使用 *Function Declaration* 的方式去宣告一個function時，javascript 會自動把這些 function 移到程式碼的最上面，而  ***Hoisting*** 的意思就是 javascript 把 function 移到最上面的過程。
[Javascript Hoisting](https://github.com/aleetsaiya/Blog/issues/16)

***程式碼的樣子***
```javascript
walk();

function walk(){
    console.log('walk');
}
```

***javascript執行***
> 自動移到最上面
```javascript
function walk(){
    console.log('walk');
}

walk();
```

### Arguments
每個 function 中都有 `arguments` 用來表示接收到的所有參數。
```javascript
function sum() {
    let total = 0;
    for(let value of arguments)
        total += value;
    return total;
}

console.log(sum(1, 2, 3, 4));
```

### Rest parameter
除了用上面介紹的 `arguments` 來獲得所有參數外，也可以用 Rest parameters ( `...args` ) 獲得所有參數至一個陣列裡面。
```javascript
function sum(...args) {
    console.log(args); //[ 1, 2, 3, 4 ]
}

sum(1, 2, 3, 4);
```

### default value of parameters
在 ES6 中，可以透過底下語法來設定一個 Function 的預設值。
```javascript
// default value of parameter
function interest(principal, rate=3.5, years=5) {
    return principal * rate / 100 * years;
}

console.log(interest(1000));
```

### Getters and Setters
當我們想獲得一個 function 的值，可是不想以呼叫 function 的方式，而是使用 properties 來獲得值得話，可以透過 `get` 來讓我們在 Object 外部直接呼叫 properties 來獲得值。
```javascript
const person = {
    firstName: 'Alee',
    lastName: 'Tsai',
    get fullName() {
        return `${this.firstName} ${this.lastName}`
    }
};
// 以property的方式
console.log(person.fullName); 
```

而 setters 能讓我們透過 property 的方式來去呼叫一個 function

```javascript
const person = {
    firstName: 'Alee',
    lastName: 'Tsai',
    get fullName() {
        return `${this.firstName} ${this.lastName}`
    },
    set fullName(value) {
        const parts = value.split(' ');
        this.firstName = parts[0];
        this.lastName = parts[1];
    }
};
//設定 Object 中的 properties 值
person.fullName = 'A BaBa'
console.log(person.fullName);  //A BaBa
```

### Error Handling
用 `try...catch` 以及 `throw new Error( )`。
```javascript
let check = function(value) {
    if(typeof value !== 'string')
        throw new Error('Not a string!');
}

try{
    check(1);
}
catch (e) {
    console.log(e); //Error: Not a string!
}
```

## let 與 var 的差別
在 ES6 之前，我們只能透過 `var` 來宣告一個變數；而在 ES6 之後，我們可以透過 `let` 以及 `const` 來宣告一個變數，而 `var` 與其它兩個宣告方式的差別為：
> var 為 function-scoped => 以一個 function 來做為區隔單位
> let, const 為 block-scoped => 以一個 block `{ }` 來做為區隔單位

```javascript
function test_let(){
    for(let i = 0; i < 5; i++){
        console.log(i);
    }
    console.log(i); //i is not defined
}

function test_var(){
    for(var i = 0; i < 5; i++){
        console.log(i);
    }
    console.log(i); //5
}
```

## this 的意思
`this` 在不同情況會指向不同的東西，有可能是 `object` ，有可能是 `window`，分別他們的方法：
+ ***method in object -> object***
+ ***general function -> global(window, global)***

舉例:
```javascript
const video = {
    title: 'a',
    f(){
        console.log(this);
    }
}

function movie() {
    console.log(this);
}
video.f() // this指向video obj
movie() // this指向global window
```

***使用 arrow function 的話，`this` 會繼承 ( inherit ) 父親的 `this`***
```javascript
const video = {
    tags: ['a', 'b', 'c'],
    showTags(){
        this.tags.forEach(function(value){
            console.log(this, value);
        })
    }
}
const movie = {
    tags: ['a', 'b', 'c'],
    showTags(){
        this.tags.forEach(value=>{
            console.log(this, value);
        })
    }
}
video.showTags() // this 指向 global window
movie.showTags() // this 指向 object
```


### 改變 this 指向
利用 `bind` 改變 this 指向的物件
```javascript
function movie() {
    console.log(this);
}
movie(); //global window
const m = movie.bind({title:'Nice movie'});
m(); // {title: 'Nice movie'}
```

## 結語
看完之後說真的幫助蠻大的，比起之前自己東找西找，我覺得有系統性的學習對我來說幫助比較大，而且它的課程也不會很貴 ( 我買一個月看到飽方案換算台幣 826元 ) ，課程裡面對基礎的東西以及觀念也介紹得很仔細，所以總結下來如果你也在找 Javascript 課程的話，我個人是蠻推薦光頭的課程。接下來 javascript mastery series-2 的筆記會放在另一份 issue。

<img src="https://user-images.githubusercontent.com/67775387/116808493-ac918380-ab6b-11eb-8acd-4b4ebc260cbb.jpg" width="500"/>