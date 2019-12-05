## 简介

TypeScript是由微软开发并开源的编程语言，是JavaScript的超集。它拓展了JavaScript的语法，添加了可选的静态类型和基于类的面向对象编程。

### 优点

相较于JavaScript，它有如下优点：

* 类型限制，让代码易于维护和调试
* 对模块，命名空间和面向对象的支持，更容易组织代码开发大型复杂程序

- 可以在编译阶段捕获运行之前的错误

### 设计目标

* 在编译阶段排查一些类型错误
* 与现存的JavaScript代码互相兼容
* 对于生成阶段的代码，没有运行时开发
* 遵循未来的ES规范

* 成为跨平台的开发语言

### 搭建开发环境

```bash
npm i -g typescript
```

这样就可以通过全局`tsc`命令来编译`.ts`文件了。

有的时候，我更需要监听`.ts`文件发生变化，自动进行编译。

```bash
tsc index.ts --watch
```

当然你可以配合webpack来使用，此时就需要使用`ts-loader`来处理`.ts`文件。

### 配置文件

通过`tsc --init`可以生成TypeScript配置文件`tsconfig.json`。该文件的具体使用规则如下：

* 如果直接运行`tsc`，此时编译器会从目录开始搜索该配置文件
* 如果直接运行`tsc -p folder`，folder目录下必须存在该配置文件
* 如果直接运行`tsc index.ts`，编译器将忽略该配置文件

注意：局部安装的`typescript`，可以使用`npx tsc --init`来生成配置文件。

### 命令行工具

```bash
tsc -v				# 查看版本
tsc *.ts			# 编译所有ts文件
tsc a.ts b.ts		# 编译多个ts文件
tsc main.ts --watch # 监听编译
tsc --init 			# 生成tsconfig.json
```

更多命令如下：

* `--watch`或`-w`：监听编译
* `--version`或`-v`：查看编译器的版本
* `--target=""`：指定编译的ES版本，默认是ES3
* `--strict`：启用所有严格类型检查
* `--sourceMap`：生成对应的`.map`文件
* `--outFile`：输出到单个文件
* `--outDir`：输出到目录
* `--module`或`-m`：指定使用哪种模块标准来输出代码
* `--lib`：要包含在编译中的库文件列表，比如Promise
* `--jsx`：在`.tsx`中支持JSX

### 代码提示

编译的时候加上`-d`选项，你会发现多生成一个`.d.ts`文件。该文件中记录了原来的`.ts`文件文中所有的声明，而TypeScript正是通过该文件进行代码提示的。

注意：自己的写的TS文件可以通过`-d`选项来生成，而对于第三方库，需要去安装对应的声明文件包。如果没有，你就需要自己写了。

## 基础类型

### 定义变量

使用let来定义变量，使用const来定义常量。

```typescript
let d = new Demo()

const PI = 123
```

当然你也可以使用var关键字来定义变量，但是不建议。

### 声明类型

通过`:`来对声明的变量进行类型描述。

```typescript
let a: string = '123'
```

变量类型可以是：

* number

  ```typescript
  let a:number = 1
  let b:number = 2.1	
  ```

* string

* `type[]`或`Array<type>`

  ```typescript
  const a: number[] = new Array(10)
  const b: Array<number> = [1, 2]				// 数组泛型定义方式
  const c: boolean[] = new Array(true, false, false)
  
  interface NumberArray {
      [index: number]: number
  }
  const f: NumberArray = [1, 2, 3]			// 用接口表示数组
  /*
  	类数组都有自己的接口定义，如IArguments,NodeList,HTMLCollectio等
  */
  ```

* boolean

* tuple（元组类型）

  ```typescript
  let x: [string, number] = ['hi', 1]		// 类型要对应一致
  let y: [string, number]					// 单独赋值
  y[0] = 'hi'
  y[1] = 12
  x.push(true)					// 报错，添加越界元素时，类型会被限制为元组中每个类型的联合类型
  ```

* any（任意类型）

  ```typescript
  let a: any = 1
  a = '1'						// 不会报错
  
  let a: string | number;		// 联合类型
  a = '1'
  a = 1
  console.log(a)				// 1
  /*
  	1. 如果数据类型不止一种，不推荐使用any，而是使用联合类型
  	2. any没有语法提示
  	3. 联合类型的变量只能访问它们共有的属性或方法
  */
  ```

* void（空类型）

  ```typescript
  let a: void = null	// 声明一个void的类型没有意义，因为你只能赋值成undefind或null
  ```

* undefined

  ```typescript
  let a:number;
  console.log(a)			// undefined
  ```

* null

  ```typescript
  let a:number;
  a = null			// 正确
  /*
  	1. 默认null和undefined是其他类型的子类型，可以赋值给其他类型
  	2. 如果指定了`--strictNullChecks`标记，那么只能赋值给void或自身
  */
  ```

* enum（枚举类型）

  ```typescript
  enum SEASON {
    SPR,
    SUM,
    AUT,
    WIN
  };
  
  console.log(SEASON.SPR);					// 1
  
  enum SEASON {
    SPR=2,
    SUM,
    AUT,
    WIN
  }
  
  console.log(SEASON.SPR, SEASON.AUT);		// 2 4
  
  enum CHOOSE { WIFE = 2, MOTHER = 3 }
  
  function question(choose: CHOOSE): void {
    console.log(choose)
  }
  
  question(CHOOSE.WIFE)						// 2
  
  /*
  	1. 枚举类型的变量一般是大写
  	2. 结果一般也用大写，用逗号隔开
  	3. 可以自定义赋值，默认是0
  */
  ```

* never

  ```typescript
  function err(msg: string): never {
      throw new Error(msg)
  }
  /*
  	1. never类型表示那些永不存在的值的类型，例如总是会抛出异常
  	或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型
  	2. never类型是任何类型的子类型，可以赋值给任何类型。但是没有
  	类型是never的子类型或可以赋值给never类型（除了自身），即使是any类型
  */
  ```

如果省略类型声明，TypeScript会从代码中自动推断出来。

### 类型转换

```typescript
let a: any = 1;
let b: string = <string>a;

console.log(b)

// 第二种写法
let msg: any = '123';
let len: number = (msg as string).length;
console.log(len)
```

转换的前提是待转换的变量是any类型。 

前面提到，当使用联合类型的时候，只能访问它们共有的属性或方法。

```typescript
function demo(v: number | string): number {
  return v.length						// 报错
}

// 将变量断言成string后类型后
function demo(v: number | string): number {
  if(<string>v.length) {
      return v.length
  }
  return v.toString().length
}
```

注意：当你在TypeScript里使用JSX时，只有as语法断言才被允许。

### 变量解构

```typescript
let [a,b] = [1,2]
console.log(a, b)			// 1 2

let [, , b] = [1,2,3]		
console.log(b)				// 3

let [a, ...b] = [1, 2, 3]
console.log(a, b)			// 1 [2, 3]

let {a, ...b} = {a: '1', b: '2', c: '3'}
console.log(a, b)			// '1' {b: '2', c: '3'}

let {a: x, ...b} = {a: '1', b: '2', c: '3'}
console.log(x, b)			// '1' {b: '2', c: '3'}
```

你还可以放到函数中去，当做函数的默认值：

```typescript
function fn({a: x, ...b} = {a: '1', b: '2', c: '3'}) {
	console.log(x)
    console.log(b)
}
```

或者调用的时候再传参：

```typescript
function fn({a: x, ...b}) {
	console.log(x)
    console.log(b)
}

fn({a: '1', b: '2', c: '3'})
```

### 类型别名

类型别名用来给一个类型起个新名字。

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

### 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  	// ok
handleEvent(document.getElementById('world'), 'dbclick'); 	// 报错
```

### 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型：

```typescript
// 函数合并
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}

// 接口合并
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

注意：合并的属性的类型必须是唯一的，重复并不会报错。

## 函数定义

返回值可有可无，若没有，返回类型为void。

```typescript
function add(a: number, b: number): number {
    return a+b
}

add(1, 2)			// 参数个数和类型必须一致，否则报错

function add(a: number, b?:number): number {
  if(b) {
      return a+b
  }
    return a
}

add(1)				// 可选参数

function add(a: number, b:number = 0): number {
  if(b) {
      return a+b
  }
    return a
}

add(1)				// 默认参数


function add(...foo: number[]): number {
  let sum: number = 0;
  
  foo.forEach((ele: number) => sum += ele)

  return sum
}

add(1, 2, 3, 4)		// 剩余参数


const add = function(a: number, b: number): number {
    return a+b
}

add(1, 2)			// 函数表达式定义

const add = (a: number, b: number): number => a+b

add(1, 2)			// 箭头函数，多用于回调函数中

const add: (n1: number, n2: number) = function(n1: number, n2: number): number {
    return n1 + n2
}

add()				// 匿名函数赋值给add变量，并且添加类型说明符

const add: (n1: number, n2: number) = (n1: number, n2: number): number => n1 + n2

add()				// 箭头函数赋值给add变量，并且添加类型说明符
```

## 面向对象

### 类

类是对属性和方法的封装，在类中有一个特殊的方法：构造函数。它是对类中分装的属性进行赋值的。

```typescript
class Person {
  name: string
  age: number

  constructor(name: string) {
    this.name = name;
  }

  sayName(): void {
    console.log(this.name)
  }
}

const a: Person = new Person('ugu')
a.sayName()
```

### 抽象类

抽象类做为其它派生类的基类使用，它们一般不能直接被实例化，不同于接口，抽象类可以包含成员的实现细节，`abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现，抽象方法的语法与接口方法相似，两者都是定义方法签名但不包含方法体，然而，抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符。

### 访问修饰符

如果希望对类中的属性和方法对外进行访问限制，此时需要借助访问修饰符。

* public：公有修饰符，可以在类内或者类外使用public修饰的属性或者行为，默认修饰符
* protected：受保护的修饰符，可以本类和子类中使用protected修饰的属性和行为
* private：私有修饰符，只可以在类内使用private修饰的属性和行为

```typescript
class Person {
  private name: string
  protected age: number
  public isMan: boolean

  constructor(name: string, age: number, isMan: boolean) {
    this.name = name;
    this.age = age
    this.isMan = isMan
  }

  sayName(): void {
    console.log(this.name)
  }
}

const a: Person = new Person('ugu', 20, true)
console.log(a.name)					// 报错
console.log(a.age)					// 报错
console.log(a.isMan)				// 正确

// 当使用private时，可以使用参数属性来简化初始化操作
class Person {
  constructor(private name: string) {}
  /*
  	等价于
  	private name: string
  	constructor(name: string) {
  		this.name = name
  	}
  */

  sayName(): void {
    console.log(this.name)
  }
}

const p: Person = new Person('ugu')
p.sayName()

// getter和setter
class Person {
  private _name: string

  constructor(name: string) {
    this._name = name;
  }

  public get name(): string {
    return this._name
  }

  public set name(newName: string) {
    this._name = newName;
  }
}

const a: Person = new Person('ugu')

console.log(a.name)				// 'ugu'
a.name = 'hello'					
console.log(a.name)				// 'hello'
/*
	编译的时候可能会报错，可以指定编译到较高版本
	tsc demo.ts -t es5
*/

// 只读属性修饰符
class Person {
  readonly name: string = 'ugu'

  constructor(name: string) {
    this.name = name;
  }
}

const a: Person = new Person('hi');
console.log(a.name)					// 'hi'
a.name = 'world'					// 报错

interface Person {
    readonly name: string;
}
/*
	1. 修饰的属性只能在声明的时候赋值，或者在构造函数中赋值
	2. 只读修饰符还可以修饰接口
*/
```

### static修饰符

当程序运行时，系统会在内存中开辟一块空间分配给这个程序，然后将这块空间划分成常量区，全局静态区，栈区，堆区。

- 常量区，存储常量和程序转变成的二进制数据
- 全局静态区，存储全局变量和静态变量
- 栈区，存储局部变量
- 堆区，存储对象

这些区在分配和回收上不同：

- 常量区，有系统分配，程序结束之后回收
- 全局静态区，当创建一个全局变量或者 static 变量，系统会在全局静态区为其开辟一块空间，该空间直到程序结束之后回收
- 栈区，当调用函数时，会在栈区为函数中的局部变量开辟一块空间，当函数调用结束之后，该空间被销毁
- 堆区，程序员通过 new 关键字创建对象，然后在堆区为该对象开辟空间，最后由程序员手动释放

因此，static修饰的变量存储在全局静态区，一旦创建就一直存在知道程序调用结束。

通过static修饰的属性和方法，称为静态属性和静态方法（或者类属性或类方法）。静态属性和静态方法可以直接通过类名访问。

```typescript
class Person {
  static msg: string = 'hello world'
}

console.log(Person.msg);
```

static 修饰符在开发中主要用于：

- 工具类的封装
- 项目数据的封装
- 单例设计模式

### 继承

继承就是用来扩展已有的类，它允许创建一个类的子类，从已有的类上继承所有的属性和方法，并且子类可以包含父类中没有的属性和方法。

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  sayName(): void {
    console.log(this.name);
  }
}

class Student extends Person {
  email: string;

  constructor(name: string, age: number, email: string) {
    super(name, age);
    this.email = email;
  }

  sayEmail(): void {
    console.log(this.email);
  }
}

const s: Student = new Student('ugu', 20, 'yinyun957@gmail.com');
s.sayName();			// 'ugu'
s.sayEmail();			// 'yinyun957@gmail.com'


// case2
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  sayName(): void {
    console.log(this.name);
  }
}

class Student extends Person {
  email: string = 'yinyun957@gmail.com';
  // 默认已经实现了constructor
  sayEmail(): void {
    console.log(this.email);
  }
}

const s: Student = new Student('ugu', 20);
s.sayName();
s.sayEmail();
```

TypeScript不支持多重继承。

### 重写

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  sayName(): void {
    console.log(this.name);
  }
}

class Student extends Person {
  email: string;

  constructor(name: string, age: number, email: string) {
    super(name, age);
    this.email = email;
  }
  // 重写sayName方法
  sayName(): void {								
    super.sayName();							// 调用父类的方法，可以不调用 
    console.log(this.name, 'class Student');
  }
}

const s: Student = new Student('ugu', 20, 'yinyun957@gmail.com');
s.sayName();
```

### 接口

接口的作用就是去描述结构的形态，用关键字interface定义。

```typescript
interface Point {
  x: number;
  y?: number;				// 可选
}

let pt: Point = {
  x: 1,
  y: 1
}

console.log(pt)					// {x: 1, y: 1}

function fn({ x = 1, y = 1 }: Point) {
  console.log(x, y)
}

fn({x: 2, y: 2})				// 2 2
/*
	1. 类和类之间共有的特性用接口来定义
	2. 一个类可以实现多个接口
*/

// 继承
interface Point3d extends Point {
  z: number;
}

const p: Point3d = {
  x: 1,
  z: 2
}
console.log(p.x, p.z);			// 1 2
```

如果新增了未定义的属性就会报错，除非做了一些设置。

```typescript
interface Point {
  x: number;
  y: number;
}

let p: Point = {			// 报错
  x: 1,
  y: 1,
  z: 1
}

interface Point {
  x: number;
  y: number;
  [propName: string]: number;		// 任意属性
}

let p: Point = {			// 正确
  x: 1,
  y: 1,
  z: 1
}
```

注意：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它类型的子集。

接口也可以用于描述函数的类型，规定函数的参数和返回值。

```typescript
interface Add {
  (a: number, b: number): number
}

// 类型必须一致，也就是说只约束类型不约束变量名
let add: Add = function (m: number, n: number): number {
  return m + n;
}

console.log(add(1, 2))
```

接口也可以用来描述类的类型。

```typescript
interface Person {
  name: string;
  sayAge(): void;
}
// 实现类接口的关键字是implements
class Student implements Person {
  name: string;
  age: number;

  constructor(age: number) {
    this.age = age;
  }

  sayAge(): void {
    console.log(this.age);
  }
}

const s: Student = new Student(12)
s.sayAge();				// 12
```

### 命名空间

命名空间，又称内部模块，被用于组织一些具有内在联系的特性和对象。命名空间能够使代码更加清晰，可以使用 namespace 和 export 关键字。

```typescript
namespace myStudent {
  interface Person {
    name: string;
    sayAge(): void;
  }

  export class Student implements Person {
    name: string;
    age: number;

    constructor(age: number) {
      this.age = age;
    }

    sayAge(): void {
      console.log(this.age);
    }
  }
}

const s: myStudent.Student = new myStudent.Student(12)
s.sayAge()				// 12 
```

命名空间主要用于组织代码。如果在写一个大型应用，在代码量增加的同时引入第三方库为了避免命名冲突的问题，可以使用命名空间解决。

当声明一个命名空间的时候，所有实体部分默认是私有的，可以使用 export 关键字导出公共部分。在一个命名空间中还可以嵌套另一个命名空间。

### 模块

> 在 TypeScript 中，“内部模块”现在称作“命名空间”，“外部模块”现在则简称为“模块”

模块在其自身的作用域里执行，而不是在全局作用域里。这意味着定义在一个模块里的变量、函数、类等等在模块外部是不可见的，除非明确地使用 `export` 形式之一导出它们。 相反，如果想使用其它模块导出的变量、函数、类、接口等的时候，必须要导入它们，可以使用 `import` 形式之一。

```typescript
// demo1.ts
export const a: number = 2;

// demo2.ts
import { a } from './demo1'

console.log(a);
```

## 异步

### async/await

```typescript
const p: Promise<number> = new Promise<number>(resolve => resolve(1));

async function fn() {
  const a = await p;
  console.log(a);
}

fn()
```

## 泛型

泛型是允许同一个函数接受各种不同类型的参数的模板，使用泛型创建可重用组件比使用具体的数据类型要好，因为泛型保留了输入、输出它们的变量的类型。

```typescript
function demo<T>(a: T): T {
  return a
}

console.log(demo(123));					// 123

// 泛型数组
function demo<T>(a: T[]): T {			// 或者 a: Array<T>
  return a[0]
}

console.log(demo([1, 2, 3]));			// 1

// 接口泛型
interface someI<T>{
    (a : T) : T
}

let b : someI<number>
/*
	1. b是一个(a: number) => number 的匿名函数
*/
```

泛型的作用就是在调用的时候，再限定某些值的类型

## 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

例如：当使用jQuery的时候，TS编译器并不知道`$`或者`jQuery`是什么，此时需要使用`declare var`来定义它的类型。

```typescript
declare const jQuery: (select: string) => any;
```

> 注意：声明语句并没有真的定义一个变量，只是定义了全局变量 `jQuery` 的类型，仅仅会用于编译时的检查，在编译结果中会被删除。

通常会把声明语句放到一个单独的文件（`*.d.ts`）中，这个文件就是声明文件，必须以`.d.ts`结尾。

目前更推荐的是使用`@types`统一管理第三方库的声明文件。

```bash
npm i @types/jquery -D
```

如果第三方库没有提供声明文件时，就需要自己写声明文件了。具体点击[这里](<https://ts.xcatliu.com/basics/declaration-files>)。

## 内置对象

对于ES、BOM和DOM的内置对象，TS已经包含，但是Node不是内置对象的一部分，所以如果你用TS写Node.js，需要下载第三方声明文件。

```bash
npm i @types/node -D
```