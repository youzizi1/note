## ES版本

* ES5：09年发布
* ES6（ES2015）：15年发布，也称为ECMA2015
* ES7（ES2016）：16年发布，也称为ECMA2016

## 块级绑定

多数语言中，变量总是在它声明的地方创建。但是JavaScript除外，变量实际创建的位置取决于如何声明它，因此ES6提供新特性以便更好的控制变量的作用域。

### 作用域

ES6之前，JavaScript只有全局作用域和函数作用域。

```js
for(var i = 1; i<=3; i++) {
    setTimeout(() => {
       console.log(i) 
    },0)
}

/*
	输出三次4,
	本质上是没有块级作用域造成。
*/
```

#### var

var声明的变量，会有变量提升的问题（hoisting）。

```js
function fn(){
  console.log(a)
  var a = 1
}
fn()		// undefined
```

### 块级作用域

块级作用域，又称词法作用域。在块级作用域中声明的变量是无法在作用域之外访问，会在如下情况创建：

* 函数内部
* `{}`组成的代码块

> 块级作用域是很多类C语言的工作机制。

在块级作用域声明的变量有如下优点：

* 不会污染全局变量
* 重复定义会出错
* 不会出现声明提升

使用let来声明变量，const声明常量，赋值后在更改会报错。但是如果const声明的是对象，那么是不会阻止对齐变量成员的修改。

```js
const obj = {
    name: 'ugu'
}

obj.name = 'guyou'			// ok

const arr = [1, 2]
arr[0] = 3					// ok

arr = 1						// error
```

let，const和var不能混合使用。

```js
let a = 1
var a = 2	// 报错
```

另外，在let或const声明之前使用变量会报错，即使是使用安全的typeof操作符。

```js
console.log(typeof a)				// undefined
const a = 1							// error
```

这是因为JavaScript引擎在处理var时会将声明提升到作用域顶部，而处理let和const时，会将声明放入到暂时性死区（TDZ），任何在暂时性死区内访问变量都会导致运行时错误。只有执行到声明位置，变量才会从TDZ中移除，才能正常使用。

const在不同循环内表现状况不同。

```js
// for循环
for(const i = 0; i < 5; i++){
  console.log(i)			// error
}

// for...in或for...of循环
var obj = {
  a:1,
  b:2,
  c:3
}
for(const key in obj){
  console.log(key)
}

/*
	ok，效果和let声明一样
*/
```

在全局作用域下，let和const不同于var。var声明的变量会成为全局变量，并成为全局对象的属性。而let和const并不会有这种行为。

## 解构赋值

### 对象解构

```js
const obj = {name: 'ugu', age: 12, sex: 'man'}
const {name, age} = obj
console.log(name, age)
/*
	ugu 12
	等同于
	const name = obj.name
	const age = obj.age
*/
```

对象解构时还可以设置别名。

```js
const obj = {name: 'ugu', age: 12, sex: 'man'}
let {name: myName, age: myAge} = obj
console.log(myName, myAge)
/*
	ugu 12
	如果打印name和age会直接报错
*/
```

设置默认值，这样就不用写`||`运算符了。

```js
const {name = 'guyou', age} = obj


// 函数中
function foo({name = 'guyou', age}) {
    console.log(name, age)
}

foo(obj)
```

### 数组解构

```javascript
const [a, ,b] = [1, 2, 3]
console.log(a, b)
/*
	1 3
*/
```

### 嵌套解构

```js
let [x, [y], z] = [1, [2, 3]]
console.log(x, y, z)
/*
	1 2 undefined
*/
```

## 字符串

### 模版字符串

ES5字符串待解决的问题：

* 多行字符串
* 基本的字符串格式化
* HTML转义

在没有模板字符串之前，通过如下方式可以输出多行字符串：

```js
['hello', 'world'].join('\n')
```

同样也可以通过`\`对其转义。

```js
let str = `he\`llo`
console.log(str)			// 'he`llo'
```

最重要的功能是替换位。

```js
const name = 'ugu'
const des = `my name is ${name}`
/*
	如果变量未定义则报错
*/
```

当然，你可以嵌入计算和函数调用等JavaScript表达式。

```js
// 计算
let count = 1
let countStr = `count is ${count + 1} now`


// 函数调用
function getCount() {
  return 1
}

const str = `count is ${getCount()} now`
console.log(str)
```

模版字符串本身也是JavaScript表达式，所以你也可以将其嵌入。

```js
function getCount() {
  return 1
}

const countStr = `${getCount()}`
const str = `count is ${countStr}`

console.log(str)
```

### tag

还可以在模版字符串前加一个标签，用来更方便的处理数据。

```js
const name = 'ugu'
const age = 12

function desc(strings, ...values) {
  console.log(strings, values)					
  let str = ''
  for (let i = 0; i < values.length; i++) {
    str += (strings[i] + values[i])				
  }
  str += strings[values.length]
  return str
}

let str = desc`${name}今年${age}`
console.log(str)
/*
	[ '', '今年', '' ] [ 'ugu', 12 ]
	
	ugu今年12
*/
```

### 方法

* includes
* startsWith
* endsWith
* repeat
* padStart
* padEnd

```js
const str = 'hello world'
console.log(str.includes('wo'))
```

这三个方法都有第二个参数表示开始搜索的位置。

另外，还新增了repeat方法用于重复指定次数的字符串。

```js
console.log("a".repeat(2))				// 'aa'
console.log(" ".repeat(4))				// '    ' 
```

通过padStart和padEnd来格式化字符串。

```js
let str = 'ugu'

console.log('('+str.padStart(5)+')')
console.log('('+str.padEnd(5)+')')
/*
	(  ugu)
	(ugu  )
*/
```

### 原理

```js
function replace(str) {
  const reg = /\$\{(\w)+\}/

  return str.replace(reg, (matched, key) => {
    console.log(key)			// 'name', 'age'
    return eval(key)
  })
}

const name = 'ugu',
  age = 12

const str = '${name}今年${age}岁了'

const newStr = replace(str)

console.log(newStr)
```

### 最佳实践

```js
const arr = [{name:'ugu',age:12},{name: 'leesin',age:20}]

const newArr = arr.map((item,index)=>{
    return `<li>${item.name},${item.age}</li>`
}).join('')

const ulStr = `<ul>${newArr}</ul>`
```

## 对象

### 对象字面量

在ES6中的对象字面量中，同名的属性和function关键字可以省略。

```javascript
let name = "youzi"
let age = 12
let obj = {
  name,
  age,
  getName(){
    console.log(this.name)
  }
}
```

### Object

#### Object.is

判断两个值是否相等，该方法会先将数据转换为其字符串形式后再进行比较。

```js
const obj1 = {
  name: 'ugu'
}

const obj2 = {
  name: 'ugu'
}

console.log(Object.is(obj1, obj2))			// false
```

#### Object.assign

```js
const obj1 = {
  name: 'ugu'
}

const obj = Object.assign({}, obj1)
console.log(obj)				// 浅拷贝
```

#### Object.setPrototypeOf

将一个指定的对象的原型设置为另一个对象或者null。

```js
const obj1 = {
  name: 'ugu'
}

const obj = {}

Object.setPrototypeOf(obj, obj1)
console.log(obj.name)			// 'ugu'
```

#### proto

在ES6中，可以直接在对象表达式中通过`__proto__`设置原型。

```js
const obj1 = {
  name: 'ugu'
}

const obj = {
  __proto__: obj1
}

console.log(obj.name)		// 'ugu'
```

#### super

可以通过super去调用原型上的属性或方法。

```js
const obj1 = {
  name: 'ugu'
}

const obj = {
  __proto__: obj1,
  getName() {
    console.log(super.name)
  }
}

obj.getName()				// 'ugu'
```

### proxy

除非特意使用defineProperty或冻结对象的属性来保护数据外，一般来说，外部可以轻松的访问对象内的属性。为了保护数据安全，ES6引入了对象代理（proxy）的概念。

```js
const obj = {
  name: 'ugu'
}

const o = new Proxy(obj, {
  get(target, key) {
    return target[key]
  },
  set(target, key, value) {
    target[key] = value
  }
})

o.name = 'youzi'
console.log(o.name, obj.name)		// 'youzi' 'youzi'
```

## 函数

### 默认参数

```js
function foo(age, name = 'ugu') {
  console.log(name, age)
}

foo(12)
/*
	12 'ugu'
*/
```

### 展开操作符

```js
function max() {
  return Math.max(...arguments)		// 类数组转数组
}

console.log(max([1, 2, 3]))		// 3
```

可以用来取代apply。

```js
const max = Math.max.apply(null, [1, 2, 3])

const max = Math.max(...[1, 2, 3])
```

当然还有其它一些使用场景。

```js
// 替代concat
const arr = [...arr1, ...arr2]

// 合并对象，ES7特性
const obj = {...obj1, ...obj2}		// 浅拷贝
```

### 剩余操作符

```js
function foo(a, ...rest) {
  console.log(rest)
}

foo(0, 1, 2, 3)
/*
	[1, 2, 3]
*/
```

### 解构参数

```js
function foo({name, age}) {
    console.log(name, age)
}

foo({name: 'ugu', age: 12})
```

### 箭头函数

* 若没有形参，小括号不能省略
* 若只有一个形参，小括号可以省略
* 若有两个或以上形参，小括号不能省略
* 若函数体只有一条语句或一条表达式时，包裹函数体的大括号可以省略
* 若函数体只有一条表达式时，会自动将表达式的结果作为函数返回值返回
* 若函数体只有一条语句是，不会自动将语句的结果作为返回值返回

```js
const foo = () => console.log(1)

foo()
```

若函数返回的是一个对象，使用箭头函数时要使用小括号括起来，否则会当做语句块来解析。

```js
const foo = () => (return {a:1})
```

箭头函数的this是词法作用域，由上下文决定。因此用call和apply调用箭头函数时，无法对this进行绑定，即传入的第一个参数会被忽略。简单来说，箭头函数中的this不是向ES5中函数在调用时决定，而是在定义时决定。

另外，箭头函数中无法使用arguments，因为箭头函数有作用域（词法作用域），词法作用域简单来讲就是，一切变量（包括this）都根据作用域链来查找。所以会把arguments当做普通的变量进行作用域查找，而不是实参对象。

因此，要获取箭头函数中实参的个数，需要通过`...`运算符。

```js
const add = (...values) => console.log(values.length)
```

## 数组

### Array.from

将数组或类数组转换为数组。

```js
function foo() {
  console.log(Array.from(arguments))
}

foo(1, 2, 3)
/*
	[1, 2, 3]
*/

const set = new Set([1,2,3,1])
const arr= Array.from(set)			// [1,2,3]
const arr = [...set]				// [1,2,3]

const map = new Map([[1,2],[3,4]])
const arr= Array.from(map)			// [[1,2],[3,4]]

const arr = Array.from('ugu')		// [ 'u', 'g', 'u' ]

// 使用map函数转换数据元素
Array.from([1,2,3], x=>x+1)			// [2,3,4]

// 生成一个数字序列
Array.from({length:5}, (v,k)=>K)	// [0,1,2,3,4]
```

### Array.of

将一组数值转换为数组。

```js
const arr = Array.of(3, 4, 5)
console.log(arr)
/*
	[3, 4, 5]
*/
```

### find和findIndex

查找对应的元素和索引。

```js
const arr = [1, 2, 3, 4, 5]
let find = arr.find(item=> item === 3)
let findIndex = arr.findIndex(item => item === 3)

console.log(find, findIndex);
/*
	3 2
*/
```

### fill

填充数组，接收如下三个参数：

* value：填充的值
* start：开始位置
* end：结束位置

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.fill('a', 1, 3)
console.log(arr)
/*
	[ 1, 'a', 'a', 4, 5, 6 ]
*/
```

### includes

ES7引入了includes方法，用来判断数组中是否存在某个值。

```js
let arr = [1, 2, 3]
console.log(arr.includes(1))		// true
```

### 原理

#### reduce

```js
const arr = [1, 2, 3, 4];

Array.prototype.myReduce = function(fn, initValue) {
  const { length } = this;

  for (let i = 0; i < length; i++) {
    initValue = fn(initValue, this[i]);
  }

  return initValue;
};

const result = arr.myReduce((prev, next) => {
  return prev + next;
}, 0);

console.log(result);
```

#### filter的原理

```js
const arr = [1,2,3,4]

Array.prototype.filter1 = function(fn){
  let newArr = []

  this.forEach(item=>{
    const flag = fn(item)
    flag && newArr.push(item)
  })
  return newArr
}

const arr1 = arr.filter1(function(item){
  return item > 2
})
```

#### map的原理

```js
Array.prototype.myMap = function(fn) {
  const arr = [];

  this.forEach(item => arr.push(fn(item)))

  return arr
};
```

#### some的原理

```js
Array.prototype.mySome = function(fn) {
  if (typeof fn !== "function") {
    throw new TypeError("arguments is not a function");
  }

  for (let i = 0, length = this.length; i < length; i++) {
    if(fn(this[i])) {
      return true
    }
  }

  return false
};
```

#### every的原理

```js
Array.prototype.myEvery = function(fn) {
  if (typeof fn !== "function") {
    throw new TypeError("arguments is not a function");
  }

  for (let i = 0, length = this.length; i < length; i++) {
    if(!fn(this[i])) {
      return false
    }
  }

  return true
};
```

## Symbol属性

symbol是原始数据类型的一种，有如下特点：

* Symbol属性对应的值是唯一的，可以解决对象中命名冲突问题
* Symbol值不能与其它数据进行计算，包括字符串拼接
* `for...in`和`for...of`不能遍历Symbol属性

通过`Symbol()`便可以创建Symbol类型的值。

```js
const s = Symbol()

console.log(s)			// Symbol()
```

既然是基本数据类型，那么久可以作为对象的属性。

```js
const s = Symbol()
const obj = {}

obj[s] = 'ugu'

console.log(obj[s])			// 'ugu'
```

注意：Symbol类型的值只能通过`[]`操作符来设置和获取值。

另外，任意两个Symbol类型的值是不相等的。

```js
let s1 = Symbol()
let s2 = Symbol()
console.log(s1 === s2)	// false
```

> 除了定义自己使用的Symbol值之外，ES6还提供了11个内置的Symbol值，指向语言内部使用的方法。其中最常用的是Symbol.iterator：对象的Symbol.iterator属性指向该对象的默认遍历器方法。

## 生成器和迭代器

多数编程语言都将迭代数据的方式从for循环转换到使用迭代器对象，JavaScript也不例外。

### 迭代器（Iterator）

迭代器是被专门设计用于迭代的对象，它使得操作数据更加高效简单。所有的迭代器对象都拥有next()方法，调用时会返回一个结果对象。这个结果对象有两个属性：value和done。当done为true时，表示迭代结束。

下面使用ES5代码模拟迭代器机制。

```js
function createIterator(items) {
  let i = 0

  return {
    next() {
      const done = i >= items.length
      const value = done ? undefined : items[i++]
      return {
        done,
        value
      }
    }
  }
}

const it = createIterator([1, 2, 3])

console.log(it.next())
console.log(it.next())
console.log(it.next())
console.log(it.next())
/*
	{ value: 1, done: false }
    { value: 2, done: false }
    { value: 3, done: false }
    { value: undefined, done: true }
*/
```

目前，字符串、数组、arguments、map和set都部署了迭代器接口，便于使用`for...of`去循环遍历。

```js
let arr = ["a","b","c"]

for(let i of arr){
  console.log(i)
}
//	"a" "b" "c"
```

如果使用`for...of`循环去遍历没有内置迭代器接口的数据结构，需要先寻找是否存在Symbol.iterator属性是否实现了迭代器接口逻辑，如果有，则可以循环遍历，否则就报错。

```js
var obj = {
    name:"youzi",
    age:12,
    [Symbol.iterator]:function(){
        //实现逻辑
    }
}
```

另外，解构赋值和`...`运算符会默认调用iterator接口。

```js
const arr = [2, 3]
const newArr = [1, ...arr]
```

### 生成器（generator）

有了生成器就不用手动模拟迭代器，直接使用生成器生成迭代器即可。

```js
function* fn() {
  console.log('first')
  yield 1
  console.log('second')
  yield 2
  console.log('third')
  yield 3
}

const it = fn()						// 不会执行内部代码

console.log(it.next())				// value属性取决于yield后面表达式的值
console.log(it.next())
console.log(it.next())
console.log(it.next())				// 最后一次value属性取决于函数是否有返回值，没有则是undefined
/*
	first
	{ value: 1, done: false }
	
	second
    { value: 2, done: false }
    
    third
    { value: 3, done: false }
    
    { value: undefined, done: true }
*/
```

并且可以轻松遍历未内置迭代器的数据结构。

```js
let obj = {
  name: 'ugu',
  age: 12,
  [Symbol.iterator]: function* gen() {
    yield obj.name
    yield obj.age
  }
}

for(let item of obj) {
  console.log(item)
}
/*
	'ugu'
	12
*/
```

## 类

### class

通过class关键字来定义类。

```javascript
class Person{
    
  constructor(name, age){		// 实例化时，会调用constructor函数
    this.age = age
    this.name = name
  }
  
  getName(){
    console.log(this.name)
  }
}
let p = new Person('ugu',12)
p1.getName()														// 'ugu'
console.log(Object.getPrototypeOf(p).hasOwnProperty('getName'))		// true
/*
	通过class定义的方法是部署在原型上
*/
```

### static方法

```js
class Person {
  static sayMsg() {
    console.log('hi, ugu')
  }
}

Person.sayMsg()			// 'hi, ugu'
```

### 继承

```js
class Person {
   constructor(name){
     this.name = name
   }
}
class Student extends Person{
    constructor(name,age){
        super(name)
        this.age = age
    }
}

const s = new Student('ugu', 12)
```

#### super

通过super可以拿到原型上的属性和方法。

```js
const obj1 = {
  name: 'fu'
}

const obj = {
  name: 'ugu',
  __proto__: obj1,
  getName(){
    console.log(super.name)
  }
}

obj.getName()			// 'fu'
```

## 集合

### Map

在ES5中，对象的属性必须是字符串，但是实际上数字或其他数据类型作为键名也是非常合理的。因此ES6中引入了Map数据类型。它是一组键值对的结构，具有极快的查找速度。

```js
const map = new Map([[1, { name: 'ugu' }], [2, 'hi,ugu']])

map.set(3, [1, 2, 3])

console.log(map.get(1))		// { name: 'ugu' }
console.log(map.get(2))		// 'hi,ugu'
console.log(map.get(3))		// [1, 2, 3]
```

Map实例对象有如下方法：

* set(key,value)
* get(key)
* delete(key)
* has(key)
* clear()
* size

注意：Map存储的数据，键名不能重复，所以后定义的会覆盖前面定义的。

### Set

和Map类似，Set是一组key的集合，但不存储value。Set实例有如下属性和方法：

* add(value)
* delete(value)
* has(value)
* clear()
* size

```js
const set = new Set([1, 2, 3])
set.add(4)

console.log(set)			// Set { 1, 2, 3, 4 }
```

由于Set对象会自动删除重复的值，所以通过这个特性可以实现数组去重。

```js
function unique(arr) {
  const set = new Set(arr)
  
  const newArr = []

  for (const item of set) {
    newArr.push(item)
  }

  return newArr
}

const arr = unique([1, 2, 3, 2])
console.log(arr)			// [1, 2, 3]
```

## for...of循环

遍历数组可以采用下标循环，而遍历Map和Set则无法使用下标。因此，为了统一集合类型，数组、字符串、Set、Map和伪数组都部署了Iterator接口，这些接口都可以通过`for...of`循环遍历。

```javascript
let arr = [1,2,3]
for(let i of arr){
  console.log(i)
}
/*
	输出： 1 2 3
*/
```

ES5中，还引入了`for...in`循环。

```js
let arr = [1,2,3]
arr.name = "youzi"
for(let i in arr){
  console.log(i)
}
/*
	"0" "1" "2" "name"
*/
```

相比较于`for...in`会带来意想不到的结果，`for...of`循环只循环集合本身的元素。

## 运算符

### 幂运算符

ES7引入了幂运算符。

```js
console.log(3**3)					// 27
```

## 模块

```js
// a.js
export const a = 'hi, ugu'

// b.js
import { a } from './a'

console.log(a)			// 'hi, ugu'

// c.js
import * as A from './a'

console.log(A.a)		// 'hi, ugu'
```

### 重命名

```js
// 导出时重命名
const a = 1

export a as moduleA

// 导入时重命名
import { a as A } from './a'
```

### 默认导出

```js
// a.js
export default const a = 'hi, ugu'

// b.js
import a from './a'

console.log(a)			// 'hi, ugu'
```

## 深度克隆

* 浅拷贝：修改拷贝后的数据会影响原数据
* 深拷贝：修改拷贝后的数据不会英雄原数据

对象拷贝一般有如下几种：

* 直接赋值
* Object.assign
* Array.prototype.concat
* Array.prototype.slice
* JSON.parse(JSON.stringify())

前四种都是浅拷贝，最后一种是深拷贝。但是最后一种有一个缺点：不支持函数，会被自动删除。

下面是自定义实现深度克隆。

```javascript
function checkType(data){
  return Object.prototype.toString.call(data).slice(8,-1)
}
function deepClone(obj){
  let result,
      type = checkType(obj)
  
  if(type === "String"){
    result = {}
  }else if(type === "Array"){
    result = []
  }else {
    return obj
  }
  
  for(let i in obj){
    let value = obj[i]
    if(checkType(value) === "Object" || checkType(value) === "Array"){
      	result[i] = deepClone(value)
    }else{
      	result[i] = value
    }
  }
  return result
}
```