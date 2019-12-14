## 编程语言

按照强弱类型可以将编程语言分为：

* 强类型：不允许改变变量的数据类型，除非进行强制类型转换

* 弱类型：变量可以被赋予不同的数据类型

按照静态类型可以将编程语言分为：

* 静态语言：在编译阶段确定所有变量的类型
* 动态语言：在执行阶段确定所有变量的类型

所以`JavaScript`属于动态弱类型语言，而`TypeScript`属于静态弱类型语言。

相较于前者，`TypeScript`有如下优点：

* 类型约束，让代码易于维护和调试
* 模块，命名空间和面向对象的全面支持，更容易组织代码开发复杂程序
* 在编译阶段检查类型错误

虽然它是一门新的语言，但是它可以：

* 与现存`JavaScript`代码兼容
* 生成`JavaScript`代码，没有运行时开销
* 遵循未来的`ES`规范

## 安装

首先通过`npm i typescript -g`全局安装，然后便可以使用`tsc`编译器来编译`.ts`文件。

它接收如下参数：

* `--watch`或`-w`：监听编译
* `--version`或`-v`：查看编译器的版本
* `--target=""`：指定编译的`ES`版本，默认是`ES3`
* `--strict`：启用所有严格类型检查
* `--sourceMap`：生成对应的`.map`文件
* `--outFile`：输出到单个文件
* `--outDir`：输出到目录
* `--module`或`-m`：指定使用哪种模块标准来输出代码
* `--lib`：要包含在编译中的库文件列表，比如`Promise`
* `--jsx`：在`.tsx`中支持`JSX`
* `tsc *.ts`：编译所有`ts`文件
* `tsc a.ts b.ts`：同时编译多个`ts`文件

注意：通过`tsc`命令直接编译`ts`文件会默认生成`ES3`代码，而通过配置文件编译`ts`文件会默认生成`ES5`代码。

## 数据类型

### 布尔类型

```typescript
const flag: boolean = true
```

### 数字类型

```typescript
const num: number = 1
```

### 字符串

```typescript
const s: string = 'hello world'
```

### 空值

空值表示没有任何类型，当一个函数没有返回值时通常是`void`类型。

```typescript
function sayName():void {
    console.log('my name is ugu')
}
```

实际上，只有`null`和`undefined`可以赋值给`void`类型。

### Null和Undefined

```typescript
const u: undefined = undefined
const n: null = null
```

默认情况下，`null`和`undefined`是所有类型的子类型，可以赋值给任意类型。但是如果你开启了`--strictNullChecks`检测，那么就只能赋值给自身和`void`类型，这样可以规避许多问题。

### Symbol

```typescript
const sym1 = Symbol('key1');
const sym2 = Symbol('key2');
```

`Symbol`的值是唯一不变的。

```typescript
Symbol('1') === Symbol('1') 			// false
```

### BigInt

`BigInt`用于安全的存储和操作大整数，即使这个数已经超出了`JavaScript`能够表示的安全整数范围。

```js
const max = Number.MAX_SAFE_INTEGER;

const max1 = max + 1
const max2 = max + 2

max1 === max2 			// true
```

上面代码是因为超出精度导致的，这显然是不合理的，而使用`TypeScript`就没有这个问题。

```typescript
const max = BigInt(Number.MAX_SAFE_INTEGER);

const max1 = max + 1n
const max2 = max + 2n

max1 === max2 			// false
```

### any

在开发阶段对于不确定类型的变量可以指定为`any`类型。

```typescript
let m: any = 1
m = 'hello world'
```

### unknown

`unknown`是`any`对应的安全类型，它相较于`any`更加严格。简单来说就是，在对`unknown`类型的值执行大多数操作之前,我们必须进行某种形式的检查,而在对 `any` 类型的值执行操作之前,我们不必进行任何检查。 

```typescript
let value: any;

value.foo.bar;  // OK
value();        // OK
new value();    // OK
value[0][1];    // OK

let value: unknown;

value.foo.bar;  // ERROR
value();        // ERROR
new value();    // ERROR
value[0][1];    // ERROR
```

### never

`never`类型表示哪些永不存在的值的类型，`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 

> 即使是`any`也不能赋值给`never`。

```typescript
// 抛出异常的函数永远不会有返回值
function error(message: string): never {
    throw new Error(message);
}

// 空数组，而且永远是空的
const empty: never[] = []
```

### 数组

数组有两种定义方式。

```typescript
const list: Array<number> = [1, 2, 3]
const list: number[] = [1,2,3]
```

### 元组

元组表示一个已知元素数量和类型的数组，各元素的类型不必相同。

```typescript
let x: [string, number];
x = ['hello', 1]			// OK
x = ['hello', 10, false] 	// Error
x = ['hello'] 				// Error
x = [1, 'hello']			// Error
```

和数组相比，元组元素不仅个数和类型相同，定义顺序也要相同。

### Object

```typescript
const value: object = [1, 2]
const value: object = {
    name: 'ugu'
}
```

普通对象，枚举，数组和元组都可以看作是`object`类型。

### 枚举

枚举类型的值默认是数字类型，从0开始累加。

```typescript
enum Color {
    reb,
    blue,
    yellow
}

console.log(Color.reb)			// 0
console.log(Color.blue)			// 1
```

当然你也可以手动赋值。

```typescript
enum Color {
    reb=10,
    blue,
    yellow
}

console.log(Color.reb)		// 10
console.log(Color.blue)		// 11
```

枚举类型的值还可以是字符串。

```typescript
enum Operation {
    Up ='up',
    Down ='down',
    Right = 'right',
    Left ='left'
}

console.log(Operation.Up)			// 'up'
console.log(Operation.Left)			// 'left'
```

#### 异构枚举

枚举类型的值还可以混合使用，构成异构枚举。

```typescript
enum Info {
    Name = 'ugu',
    Age = 10
}

console.log(Info.Name)			// 'ugu'
console.log(Info.Age)			// 10
```

> 异构枚举很少使用。

#### 常量枚举

还可以通过`const`声明将枚举变为常量枚举。

```typescript
const enum OtherInfo {
    Name = 'ugu',
    Age = 10
}

console.log(OtherInfo.Name)
console.log(OtherInfo.Age)
```

相较于之前的枚举类型，常量枚举在编译阶段会省略声明，可以提升新跟那个。当然你也可以通过` --preserveConstEnums `保留编译后的声明。

#### 枚举成员类型

当所有枚举成员都拥有字面量枚举值时，它就带有了一种特殊的语义，即枚举成员成为了类型。 

```typescript
type c = 0

declare let b: c

b = 1 						// 不能将类型“1”分配给类型“0”
b = Direction.Up 			// ok
```

#### 枚举合并

分开声明的相同枚举，会自动合并。

```typescript
enum Direction {
    Up = 'Up',
    Down = 'Down',
    Left = 'Left',
    Right = 'Right'
}

enum Direction {
    Center = 1
}
```

所以上述代码并不冲突。

### 接口

`TypeScript`的核心原则之一是对值所具有的结构进行类型检查,它有时被称做“鸭式辨型法”。而接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

```typescript
const getName = info => console.log(info.name)
```

此时你会发现代码会报错，因为编译器并不知道`info`有`name`属性，除非你设置为`any`类型，或者通过接口描述这个类型。

```typescript
interface Info { 
    name: string
}

const getName = (info: Info) => console.log(info.name)
```

这样，上述代码才能编译通过。注意：接口不会去检查属性的顺序，只要相应的属性存在且类型正确即可。

#### 可选属性

```typescript
interface Info { 
    name: string,
    age?: number
}
```

#### 只读属性

当属性设置为只读属性后，一旦设置后就无法更改。

```typescript
interface Info { 
    name: string,
    age?: number,
    readonly sex: boolean
}

const getName = (info: Info) => console.log(info.sex)

const info: Info = {
    name: 'ugu',
    sex: true
}

getName(info)			// true
```

#### 函数类型

如果接口里面还有函数如何声明？

```typescript
interface Info { 
    name: string,
    age?: number,
    readonly sex: boolean,
    sayName: () => void
}

const info: Info = {
    name: 'ugu',
    sex: true,
    sayName() { 
        console.log(this.name)
    }
}

info.sayName()			// 'ugu'
```

或者先用接口描述出来，然后再进行赋值。

```typescript
interface SayName { 
    (): void
}

interface Info { 
    name: string,
    age?: number,
    readonly sex: boolean,
    sayName: SayName
}

const info: Info = {
    name: 'ugu',
    sex: true,
    sayName() { 
        console.log(this.name)
    }
}

info.sayName()			// 'ugu'
```

#### 属性检查

```typescript
interface Config {
  width?: number;
}

function  CalculateAreas(config: Config): { area: number} {
  let square = 100;
  if (config.width) {
      square = config.width * config.width;
  }
  return {area: square};
}

let mySquare = CalculateAreas({ widdth: 5 });			// 报错
console.log(mySquare)
```

上面代码中因为无法通过接口结构检查而报错，解决这个问题有如下两种方式：

**类型断言**

```typescript
interface Config {
    width?: number;
}

function  CalculateAreas(config: Config): { area: number} {
  let square = 100;
  if (config.width) {
      square = config.width * config.width;
  }
  return {area: square};
}
// 类型断言
let mySquare = CalculateAreas({ widdth: 5 } as Config);
console.log(mySquare)
```

**添加字符串索引签名**

```typescript
interface Config {
    width?: number;
    // 添加签名
    [prop: string]: any
}

function  CalculateAreas(config: Config): { area: number} {
  let square = 100;
  if (config.width) {
      square = config.width * config.width;
  }
  return {area: square};
}

let mySquare = CalculateAreas({ widdth: 5 });
console.log(mySquare)
```

#### 可索引类型

有这样一个场景，某个用户信息下有一个`phone`属性拥有该用户的所有邮箱，但是有多少个无法确定，此时就可以使用可索引类型。

```typescript
interface Phone { 
    [prop: string]: string
}

interface User { 
    name: string,
    phone: Phone
}

const ugu: User = {
    name: 'ugu',
    phone: {
        'qq': '123@qq.com'
    }
 }
```

#### 继承接口

一个接口允许继承另一个接口。

```typescript
interface User { 
    name: string
}

interface VipUser extends User { 
    vip: boolean
}

const ugu: VipUser = {
    name: 'ugu',
    vip: true
}
```

### Class

#### 抽象类

抽象类常常用于基类使用，一般不直接实例化。

```typescript
abstract class Animal { 
    abstract run(): void
}

class Dog extends Animal { 
    run() { 
        console.log('dog is running')
    }
}

const d = new Dog()
d.run()

const a = new Animal()			// 报错
```

#### 访问限定符

**public**

类中定义的成员默认是`public`，表示该成员可以被外部访问。

```typescript
class Car {
    public run() {
        console.log('启动...')
    }
}

const car = new Car()

car.run() 			// 启动...
```

**private**

私有成员只能在`Class`的内部被访问。

```typescript
class Car {
    private run() {
        console.log('running ...')
    }
}

const c = new Car()
c.run()				// Error
```

**protected**

被保护的成员只能在`Class`的内部和字类中被访问。

```typescript
class Car {
    protected run() {
        console.log('启动...')
    }
}

class GTR extends Car {
    init() {
        this.run()
    }
}

const car = new Car()
const gtr = new GTR()

car.run() 			// Error
gtr.init() 			// 启动...
gtr.run() 			// Error
```

#### 作为接口

```typescript
class Props { 
    public type: string = 'circle'
    public x: number = 0
    public y: number = 0
}

const p = new Props()

console.log(p.x)			// 0
```

这样不仅能起到接口的作用，还可以设置初始值。

### 函数

`TypeScript`中的函数不需要刻意去定义，会自动进行类型推断。

```typescript
const add  = (a: number, b: number) => a + b
```

上述函数定义，会自动将`add`变量推断成`(a: number, b: number) => number`类型。

#### 可选参数

```typescript
const add  = (a: number, b?: number) => a + (b?b:0)
```

#### 默认参数

```typescript
const add = (a: number, b = 10) => a + b
```

#### 剩余参数

```typescript
const add  = (a: number, b?: number) => a + (b?b:0)
```

#### 函数重载

```typescript
// 重载
interface Direction {
    top: number
    right: number
    bottom: number
    left: number
}

function assigned(all: number): Direction
function assigned(topAndBottom: number, leftAndRight: number): Direction
function assigned(top: number, right: number, bottom: number, left: number): Direction

// 代码实现函数不可被调用
function assigned (a: number, b?: number, c?: number, d?: any) {
    if (b === undefined && c === undefined && d === undefined) {
      b = c = d = a
    } else if (c === undefined && d === undefined) {
      c = a
      d = b
    }
    return {
      top: a,
      right: b,
      bottom: c,
      left: d
    }
}
```

函数重载在多人协作项目或者开源库中经常使用，因为你的函数可能在不同业务场景下被调用。

## 泛型

泛型可以给予开发者创造灵活，可重用代码的能力。例如：一个函数要求你接收某个类型的参数，返回值同样是同样的类型，你总不能将每种类型都重新定义一遍吧，这时泛型就派上用场了。

```typescript
function demo<T>(val: T): T {
    return val
}
```

你还可以定义多个泛型：

```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); 		// ['seven', 7]
```

### 泛型变量

有下面场景，函数接收一个数组参数，函数体打印数组的长度并返回该数组本身。

```typescript
function demo<T>(arr: T): T {
    console.log(arr.length)			// Error
    return arr
}
```

编译器会报错，`arr`参数没有`length`属性。为了保证编译通过我们需要手动类型断言：

```typescript
function demo<T>(arr: T): T {
    console.log((arr as Array<any>).length)		// OK
    return arr
}
```

或者直接使用泛型数组不更好：

```typescript
function demo<T>(arr: Array<T>): T {
    console.log(arr.length)		// OK
    return arr
}
```

### 泛型接口

接口也可以泛型化。

```typescript
interface ReturnItemFn<T> {
    (para: T): T
}
```

### 泛型类

类中也可以使用泛型，既可以作用于类本身，也可以作用于类成员。

```typescript
class Stack<T> {
    private arr: T[] = []

    public push(item: T) {
        this.arr.push(item)
    }

    public pop() {
        this.arr.pop()
    }
}
```

### 泛型约束

泛型可以是任意类型，有的时候开发者清楚传入的泛型属于哪种类型，这时就需要用到泛型约束。

```typescript
type Params = number | string

class Stack<T extends Params> {
    private arr: T[] = []

    public push(item: T) {
        this.arr.push(item)
    }

    public pop() {
        this.arr.pop()
    }
}

const stack = new Stack<booleam>()		// 报错
const s = new Stack<number>()			// OK
```

### 泛型约束和索引类型

```typescript
function getVal(info: Object, key: string) { 
    return info[key]			// Error
}
```

上面代码编译失败。解决这个问题无非是上面两个方法——通过接口定义`info`结构或者通过属性索引来实现，下面以属性索引为例：

```typescript
interface Info { 
    [prop: string]: string
}

function getVal(info: Object, key: string) { 
    return info[key]		// OK
}
```

还有一种解决办法就是通过泛型约束：

```typescript
function getVal<T extends Object, U extends keyof T>(info: T, key: U) { 
    return info[key]
}
```

其中`keyof T`是将`T`中的属性类型组合成联合类型，`key`参数的类型被约束在联合类型之中。

```typescript
const info = {
    name: 'ugu',
    age: 1
}

getVal(info, age)		// OK
getVal(info, name)		// OK
getVal(info, sex)		// Error
```

### 多重泛型约束

如何进行多重泛型约束呢？

```typescript
interface FirstInterface {
  doSomething(): number
}

interface SecondInterface {
  doSomethingElse(): string
}

// 错误
class Demo<T extends FirstInterface, T extends SecondInterface> {
  private genericProperty: T

  useT() {
    this.genericProperty.doSomething()
    this.genericProperty.doSomethingElse() 
  }
}

// 错误
class Demo<T extends FirstInterface, SecondInterface> {
  private genericProperty: T

  useT() {
    this.genericProperty.doSomething()
    this.genericProperty.doSomethingElse() 
  }
}
```

上面两种方式都是错误的，正确的如下：

```typescript
interface ChildInterface extends FirstInterface, SecondInterface {}

class Demo<T extends ChildInterface> {
  private genericProperty: T

  useT() {
    this.genericProperty.doSomething()
    this.genericProperty.doSomethingElse()
  }
}
```

### 泛型与new

假如声明一个函数返回指定构造函数的实例。

```typescript
function getInstance<T>(type: T): T {
    return new type()
}
```

上面代码实际上会报错，因为编译器并不知道`T`是构造函数，你应该这样：

```typescript
function getInstance<T>(type: {new(): T}): T {
  return new type() 
}
```

其中`type`的类型`{new(): T}`表示此泛型`T`可以被实例化，实例化的类型是泛型`T`。

## 类型断言和类型守卫

### 类型断言

有些情况下，`TS`不能准确的推断类型，这时就会编译不通过。

```typescript
const info = { name: 'ugu' }
console.log(info.name)		// error
```

由于类型推断，`info`被推断成`{}`，无法得知其中的属性，但是开发者是知道有这个属性的，这时你就可以使用类型断言。

```typescript
interface Info {
    name: string
}
const info = { name: 'ugu' } as Info

console.log(info.name)
```

类型断言不要滥用，可能会造成`TypeScript`失去代码提示的能力。正确的做法应该如下：

```typescript
interface Info {
    name: string
}
const info: Person = {
    name: 'ugu'
} 

console.log(info.name)
```

这样你就可以准确的得到代码提示。

在极端场景下，你还可以使用双重断言。

```typescript
interface Person {
	name: string;
	age: number;
}

const person = 'xiaomuzhu' as Person; // Error
```

上述代码夏然是错误的，因为你无法将字符串断言成`Person`接口，这时可以使用双重断言。

```typescript
interface Person {
	name: string;
	age: number;
}

const person = 'xiaomuzhu' as any as Person; // ok
```

双重断言不建议使用。

### 类型守卫

类型守卫就是缩小类型的范围。

```typescript
// instanceof
class Person {
	name = 'xiaomuzhu';
	age = 20;
}

class Animal {
	name = 'petty';
	color = 'pink';
}

function getSometing(arg: Person | Animal) {
	if (arg instanceof Animal) {
		console.log(arg.color); // ok
		console.log(arg.age); // Error
	}
	if (arg instanceof Person) {
		console.log(arg.age); // Error
		console.log(arg.color); // ok
	}
}

// in
class Person {
	name = 'xiaomuzhu';
	age = 20;
}

class Animal {
	name = 'petty';
	color = 'pink';
}

function getSometing(arg: Person | Animal) {
	if ('age' in arg) {
		console.log(arg.color); // ok
		console.log(arg.age); // Error
	}
	if ('color' in arg) {
		console.log(arg.age); // Error
		console.log(arg.color); // ok
	}
}

// 字面量类型守卫
type Foo = {
  kind: 'foo'; // 字面量类型
  foo: number;
};

type Bar = {
  kind: 'bar'; // 字面量类型
  bar: number;
};

function doStuff(arg: Foo | Bar) {
  if (arg.kind === 'foo') {
    console.log(arg.foo); // ok
    console.log(arg.bar); // Error
  } else {
    console.log(arg.foo); // Error
    console.log(arg.bar); // ok
  }
}
```

## 类型兼容性

类型兼容性用于确定一个类型是否能赋值给其它类型。

### 结构类型

`TypeScript`里的类型兼容性是基于「结构类型」的，结构类型是一种只使用其成员来描述类型的方式，其基本规则是，如果`x`要兼容`y`，那么`y`至少具有与`x`相同的属性。

``` typescript
interface Info { 
    name: string
}

class Person { 
    constructor(public name: string, public age: number) { }
}

let t: Info

t = new Person('ugu', 12)			// OK

console.log(t.name)
```

上述代码能够编译成功，但是反过来`Info`没有兼容`Person`类。

### 函数的类型兼容性

判断函数的类型兼容性，首先需要判断参数列表，如果`x`的每个参数能在`y`里找到对应类型的参数，注意只要类型相同，参数名称无所谓。

```typescript
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; 		// OK
x = y; 		// Error
```

那么当函数的参数存在可选参数或者`rest`参数会怎样？

```typescript
let foo = (x: number, y: number) => {};
let bar = (x?: number, y?: number) => {};
let bas = (...args: number[]) => {};

foo = bar = bas;
bas = bar = foo;
```

上面代码如果开启了`strictNullChecks`检查，那么就不兼容，这是因为可选参数的类型可能是`undefined`。

### 枚举的类型兼容性

枚举和数字类型相互兼容。

```typescript
enum Status {
  Ready,
  Waiting
}

let status = Status.Ready;
let num = 0;

status = num;
num = status;
```

### 类的类型兼容性

仅仅只有实例成员和方法会相比较，构造函数和静态成员不会被检查:

```typescript
class Animal {
  feet: number;
  constructor(name: string, numFeet: number) {}
}

class Size {
  feet: number;
  constructor(meters: number) {}
}

let a: Animal;
let s: Size;

a = s; // OK
s = a; // OK
```

私有的和受保护的成员必须来自于相同的类:

```typescript
class Animal {
  protected feet: number;
}
class Cat extends Animal {}

let animal: Animal;
let cat: Cat;

animal = cat; // ok
cat = animal; // ok

class Size {
  protected feet: number;
}

let size: Size;

animal = size; // ERROR
size = animal; // ERROR
```

### 泛型的类型兼容性

泛型本身就是不确定的类型，它的表现根据是否被成员使用而不同。

```typescript
interface Person<T> {

}

let x : Person<string>
let y : Person<number>

x = y // ok
y = x // ok
```

上面代码由于没有成员使用泛型，那么就没有问题。

```typescript
interface Person<T> {
    name: T
}

let x : Person<string>
let y : Person<number>

x = y 	// Error
y = x 	// Error
```

## 高级类型

### 交叉类型

交叉类型是将多个类型合并为一个类型，这个合并类型包含所需的所有类型的特性。

```typescript
function mixin<T, U>(first: T, second: U): T & U {
    const result = <T & U>{};
    for (let id in first) {
        (<T>result)[id] = first[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<U>result)[id] = second[id];
        }
    }

    return result;
}

const x = mixin({ a: 'hello' }, { b: 42 });

// 现在 x 拥有了 a 属性与 b 属性
const a = x.a;
const b = x.b;
```

上述代码中`mixin`方法可以创建一个新对象，拥有两个参数对象的所有属性。

### 联合类型

联合类型表示一个值可以是几种类型的其中一个。

```typescript
let a: number | string

a = 1					// OK
a = 'hello world'		// OK
```

### 类型别名

类型别名会给一个类型起个新名字，类型别名有时和接口很像，但是可以作用于原始值、联合类型、元组以及其它任何你需要手写的类型。

```typescript
type some = boolean | string

let a: some = true		// OK
a = 'hello world'
```

类型别名还可以是泛型。

```typescript
type Container<T> = { value: T };
```

也可以使用类型别名来在属性里引用自己：

```typescript
type Tree<T> = {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
}
```

类型别名虽然和接口相似，但是还是有区别的。接口通常用于定义对象属性，而`type`的声明方式除了对象之外还可以定义交叉类型，联合类型等等其它高级类型，所以后者应用更广泛。但是前者有如下类型别名没有特性：

* 可以`extends`和`implements`
* 接口可以合并

## 可辨识联合类型

### 字面量类型

字面量类型的要和实际的值的字面量一一对应,如果不一致就会报错。

```typescript
const a: 11 = 11

const b: 22 = 11		// Error
```

字面量类型配合联合类型使用，可以模拟枚举类型：

```typescript
type Color = 'red' | 'blue' | 'green'

const c: Color = 'yellow'		// Error
```

### 类型字面量

类型字面量和`Javacript`中的对象字面量的语法很相似。

```typescript
type Info = {
    name: 'ugu',
    age: 12
}

const i: Info = {
    name: 'ugu',
    age: 12			// 只能是12字面量类型
}
```

和接口很相似，一定程度上可以替代接口。

### 可辨识的联合类型

有这样一个场景，你需要定义一个接口来实现创建用户和删除用户。

```typescript
interface Info {
    username: string
}

interface UserAction {
    id?: number
    action: 'create' | 'delete'
    info: Info
}
```

上面代码有一个问题——创建用户的时候是不需要`id`的，但是根据上面定义，下面代码是合法的。

```typescript
const action: UserAction = {
    action: 'create',
    id: 1,
    info: {
        username: 'ugu'
    }
}
```

此时你可能会想到上面的对象字面量来解决这个问题：

```typescript
type UserAction = | {
    id: number
    action: 'delete'
    info: Info
} |
{
    action: 'create'
    info: Info
}
```

但是这样会导致你无法使用`id`属性。

```typescript
function getId(ua: UserAction) { 
    console.log(ua.id)				// Error
}
```

为了解决这个问题，你需要利用可辨识的标签，上例中`action`属性就是辨识的关键：

```typescript
type UserAction = | {
    id: number
    action: 'delete'
    info: Info
} |
{
    action: 'create'
    info: Info
}

interface Info { 
    username: string
}

function getId(ua: UserAction) { 
    switch (ua.action) { 
        case 'delete':
            console.log(ua.id)			// OK
            break
        default:
            break
    }
}
```

总结来说，要想使用可辨识的联合类型，你需要三个条件：

* 唯一的辨识
* 类型别名必须是联合类型
* 类型守卫的特性

> 可辨识的联合类型适用于`Redux`的`reducer`代码。

## 装饰器

装饰器最早由`Python`引入，主要作用是给一个已有的方法或类扩展一些新的行为，而不是直接去修改其本身。

> 在`JavaScript`需要` babel-plugin-transform-decorators-legacy `来支持`decorator`，而在`TS`中你只需要开启`experimentDecorators`即可。

实际上，装饰器本质上是一个函数，他会在运行时被调用，被装饰的声明信息作为参数传入，`@expression`仅仅是语法糖。

### 类装饰器

```typescript
function addAge(constructor: Function) {
  constructor.prototype.age = 18;
}

@addAge
class Person{
  name: string;
  age: number;
  constructor() {
    this.name = 'xiaomuzhu';
  }
}

let person = new Person();

console.log(person.age); 		// 18
```

上述代码通过装饰器给`Person`类添加`age`属性。

### 属性和方法装饰器

```typescript
// 声明装饰器修饰方法/属性
function method(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
   console.log(target);
   console.log("prop " + propertyKey);
   console.log("desc " + JSON.stringify(descriptor) + "\n\n");
   descriptor.writable = true;
};

class Person{
  name: string;
  constructor() {
    this.name = 'xiaomuzhu';
  }

  @method
  say(){
    return 'instance method';
  }

  @method
  static run(){
    return 'static method';
  }
}

const xmz = new Person();

// 修改实例方法say
xmz.say = function() {
  return 'edit'
}

// 打印结果,检查是否成功修改实例方法
console.log(xmz.say());
```

### 参数装饰器

参数装饰器，顾名思义用于修饰参数的装饰器。

```typescript
function logParameter(target: Object, propertyKey: string, index: number) {
    console.log(target, propertyKey, index);
}

class Person {
    greet(@logParameter message: string,@logParameter name: string): string {
        return `${message} ${name}`;
    }
}
const p = new Person();
p.greet('hello', 'xiaomuzhu');

// Person { greet: [Function] } greet 1
// Person { greet: [Function] } greet 0
```

参数装饰器的三个参数分别表示：

* `target`：当前对象的原型
* `propertyKey`：参数的名称
* `index`：参数数组中的位置

参数装饰器可以用来做什么？简单来说，参数装饰器可以提供信息，给比如给类原型添加了一个新的属性，属性中包含一系列信息，这些信息就被成为「元数据」，然后我们就可以使用另外一个装饰器来读取「元数据」。是不是优点像`Java`中的注解。

当然 那种直接修改类原型属性的方法并不优雅，还有一种更通用更优雅的方式——元数据反射。 

### 装饰器工厂

装饰器工厂就是一个简单的函数，它返回一种类型的装饰器。例如：你可能需要几个装饰器来分别打印类中的部分属性，方法以及参数名称等等信息，此时就可以使用装饰器工厂，而不是写好几个装饰器。

```typescript
function log(...args : any[]) {
  switch(args.length) {
    case 1:
      return logClass.apply(this, args);
    case 2:
      return logProperty.apply(this, args);
    case 3:
      if(typeof args[2] === "number") {
        return logParameter.apply(this, args);
      }
      return logMethod.apply(this, args);
    default:
      throw new Error("Decorators are not valid here!");
  }
}
```

### 装饰器顺序

在 TypeScript 里，当多个装饰器应用在一个声明上时会进行如下步骤的操作：

1. 由上至下依次对装饰器表达式求值。
2. 求值的结果会被当作函数，由下至上依次调用。

如果我们使用装饰器工厂的话，可以通过下面的例子来观察它们求值的顺序：

```typescript
function f() {
    console.log("f(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("f(): called");
    }
}

function g() {
    console.log("g(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
}

class C {
    @f()
    @g()
    method() {}
}
```

在控制台里会打印出如下结果：

```typescript
f(): evaluated
g(): evaluated
g(): called
f(): called
```

类中不同声明上的装饰器将按以下规定的顺序应用：

- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个静态成员
- 参数装饰器应用到构造函数
- 类装饰器应用到类

## Reflect Metadata

使用元数据反射需要先安装`reflect-metadata`模块，然后在开启`emitDecoratorMetadata`。

```typescript
@Reflect.metadata('name', 'A')
class A {
  @Reflect.metadata('hello', 'world')
  public hello(): string {
    return 'hello world'
  }
}

Reflect.getMetadata('name', A) // 'A'
Reflect.getMetadata('hello', new A()) // 'world'
```

总结来说：`Reflect Metadata`可以通过装饰器来给类添加一些自定义的信息，然后通过反射将这些信息提取出来，也可以通过反射来添加这些信息。

> `Reflect`反射允许运行中的程序对自身进行检查，并能直接操作程序的内部属性和方法。反射在`C#`和`Java`中早有定义。

### 基本概念

下面是`Reflect Metadata`的基础`API`：

```typescript
// define metadata on an object or property
Reflect.defineMetadata(metadataKey, metadataValue, target);
Reflect.defineMetadata(metadataKey, metadataValue, target, propertyKey);

// check for presence of a metadata key on the prototype chain of an object or property
let result = Reflect.hasMetadata(metadataKey, target);
let result = Reflect.hasMetadata(metadataKey, target, propertyKey);

// check for presence of an own metadata key of an object or property
let result = Reflect.hasOwnMetadata(metadataKey, target);
let result = Reflect.hasOwnMetadata(metadataKey, target, propertyKey);

// get metadata value of a metadata key on the prototype chain of an object or property
let result = Reflect.getMetadata(metadataKey, target);
let result = Reflect.getMetadata(metadataKey, target, propertyKey);

// get metadata value of an own metadata key of an object or property
let result = Reflect.getOwnMetadata(metadataKey, target);
let result = Reflect.getOwnMetadata(metadataKey, target, propertyKey);

// get all metadata keys on the prototype chain of an object or property
let result = Reflect.getMetadataKeys(target);
let result = Reflect.getMetadataKeys(target, propertyKey);

// get all own metadata keys of an object or property
let result = Reflect.getOwnMetadataKeys(target);
let result = Reflect.getOwnMetadataKeys(target, propertyKey);

// delete metadata from an object or property
let result = Reflect.deleteMetadata(metadataKey, target);
let result = Reflect.deleteMetadata(metadataKey, target, propertyKey);

// apply metadata via a decorator to a constructor
@Reflect.metadata(metadataKey, metadataValue)
class C {
  // apply metadata via a decorator to a method (property)
  @Reflect.metadata(metadataKey, metadataValue)
  method() {
  }
}
```

你会发现这些`API`接收的参数就四种：

* `Metadata Key`: 元数据的Key，本质上内部实现是一个Map对象，以键值对的形式储存元数据
* `Metadata Value`: 元数据的Value
* `Target`: 一个对象，表示元数据被添加在的对象上
* `Property`: 对象的属性，元数据不仅仅可以被添加在对象上，也可以作用于属性，这跟装饰器类似

### 设置和获取元数据

你可以通过原生`API`来添加元数据。

```typescript
Reflect.defineMetadata(metadataKey, metadataValue, target);
Reflect.defineMetadata(metadataKey, metadataValue, target, propertyKey);
```

或者使用装饰器简单地使用它。

```typescript
import 'reflect-metadata'

@Reflect.metadata('name', 'xiaomuzhu')
class Person {

    @Reflect.metadata('time', '2019/10/10')
    public say(): string {
        return 'hello'
    }
}


console.log(Reflect.getMetadata('name', Person)) 			// xiaomuzhu
console.log(Reflect.getMetadata('time', new Person, 'say')) // 2019/10/10
```

元数据如果被添加在了实例方法上，那么必须实例化之后才能取出，如果不想实例化，可以加在静态方法上。

### 内置元数据

上面的例子中，我们的元数据是开发者自己设置的，其实我们也可以获取一些 TypeScript 本身内置的一些元数据。

比如，我们通过 `design:type` 作为 key 可以获取目标的类型，比如在上例中，我们获取 `say` 方法的类型:

```typescript
...
// 获取方法的类型
const type = Reflect.getMetadata("design:type", new Person, 'say')

[Function: Function]
```

通过 `design:paramtypes` 作为 key 可以获取目标参数的类型，比如在上例中，我们获取 `say` 方法参数的类型:

```typescript
// 获取参数的类型,返回数组
const typeParam = Reflect.getMetadata("design:paramtypes", new Person, 'say')

// [Function: String]
```

使用 `design:returntype` 元数据键获取有关方法返回类型的信息:

```typescript
const typeReturn = Reflect.getMetadata("design:returntype", new Person, 'say')
// [Function: String]
```

### 实践

`Angular`或者`Nest.js`这样的框架就是通过`decorator`+`Reflect.metadata`来模拟注解功能。

## 赋值断言

`TypeScript 2.7`引入了一个新的控制严格性的标记`--strictPropertyInitialization`，用于保证变量声明和实例属性都会有初始值。

```typescript
class StrictClass {
    foo: number;
    bar = 'hello';
    baz: boolean; 		// Error
    constructor() {
        this.foo = 42;
    }
}
```

这个功能用来帮助开发者写出更严格的代码，但是会有如下不可避免的情况：

* 该属性本身就是`undefined`，这个时候需要再手动类型注解`undefined`
* 属性被间接初始化了，例如：构造函数中调用一个方法，更改了属性的值

此时，我们需要提醒编译器这里不需要一个初始值，这就是明确赋值断言。它允许将`!`放置在实例属性和变量声明之后，来表明此属性已经确定它已经被赋值了。

```typescript
let x: number;
initialize();
console.log(x + x); 			// Error
function initialize() {
    x = 10;
}
```

上面代码明明间接赋值了，但是还是报错，此时就可以使用明确类型断言。

```typescript
let x: number;
initialize();
console.log(x! + x!); //ok

function initialize() {
    x = 10;
}
```

## is关键字

```typescript
function isString(s: any): s is string {
    return typeof s === 'string'
}

function example(foo: number | string) { 
  if (isString(foo)) { 
    console.log(foo.length)			// OK
  }
}
```

上面代码的作用无非就是判断传入的参数是否是字符串类型，那为什么不直接返回`boolean`类型呢？

```typescript
function isString(s: any): boolean {
    return typeof s === 'string'
}

function example(foo: number | string) { 
  if (isString(foo)) { 
    console.log(foo.length)			// Error
  }
}
```

你会发现改为`boolean`就会报错，这时因为 `is` 为关键字的「类型谓语」把参数的类型范围缩小了，明确知道了其中参数是`string`。

## 可调用类型注解

前面已经学习过如何注解常见的变量类型，那么如何注解一个接口可调用呢？

```typescript
interface ToString {
  (): string
}

declare const sometingToString: ToString;

sometingToString() // ok
```

那么，当我们想实例化它呢？

```typescript
interface ToString {
  new (): string
}

declare const sometingToString: ToString;

new sometingToString() // ok
```

## 类型推导

编译器会自动进行类型推导，在前面的函数章节你已经见识过了。

```typescript
let arr1 = [1, 'one', 1n]		// 推导成 string | number | bigint 联合类型

const bar = [1, 2];
let [a, b] = bar;				// a,b 被推导成number类型
```

但是一些情况下，类型推导可能不符合开发者的想法。

```typescript
const action = {
  type: 'update',
  payload: {
    id: 10
  }
}
```

上面代码，你会发现`type`被推导成`string`类型，而你可能想要的字面量类型`update`。这时你就需要使用类型断言或者声明一个接口，而不是使用类型推导：

```typescript
interface Action {
  type: 'update',
  payload: {
    id: number
  }
}

const action: Action = {
  type: 'update',
  payload: {
    id: 10
  }
}
```

## 高级类型

### 索引类型

```typescript
interface Obj { 
  [key: string]: any
}

function pick(obj: Obj, key: string) { 
  return obj[key]
}

const obj = {
  name: 'ugu',
  age: 10
}

console.log(pick(obj, 'name'))			// 'ugu'
```

上述代码中`pick`函数可以实现从一个对象中取某个属性的值。但是在类型注解上面似乎有些不完美：

* `names`应该是对象的属性，而不是宽泛的字符串类型
* 函数返回值类型不应该是`any`类型，而应该是属性值的联合类型

这时就需要使用索引类型查询操作符和索引访问操作符了。其中索引类型查询操作符可以通过`keyof`定义，可以将其作用于泛型`T`上来获取泛型`T`上的所有`public`属性名构成联合类型；而索引访问操作符可以获取属性名对应的属性值类型，通过`[]`来访问。

```typescript
function pick<T, K extends keyof T>(obj: T, key: K): T[K] { 
  return obj[key]
}

const obj = {
  name: 'ugu',
  age: 10
}

console.log(pick(obj, 'name'))		// 'ugu'
```

### 映射类型

有如下场景，我们需要把一个已知接口中成员全部变为可选，难道我们应该在所有成员上都手动加`?`，这显然是不优雅的。此时，映射类型就派上用场了，语法`[K in Keys]`：

* `K`：类型变量，依次绑定到每个属性上，对应每个属性名的类型
* `Keys`：字符串字面量构成的联合类型，表示一组属性名

```typescript
type partial<T> = { [K in keyof T]?: T[K] }

interface User { 
  name: string,
  age: number
}

type partialUser = partial<User>
```

### 条件类型

条件类型能够表示非统一的类型，以一个表达式进行类型关系检测，从而在两种类型中选择其一。

```typescript
T extends U ? X : Y
```

上述代码中，若`T`能够赋值给`U`，那么类型为`X`，否则为`Y`。

```typescript
declare function f<T extends boolean>(x: T): T extends true ? string : number;

const x = f(Math.random() < 0.5)
const y = f(false)
const z = f(true)
```

上述代码中`f`函数接收一个布尔类型参数，若参数为`true`，则函数返回值为`string`，否则函数返回值类型为`number`。

#### 条件类型和联合类型

条件类型有一个特性，就是「分布式有条件类型」，但是分布式有条件类型是有前提的，条件类型里待检查的类型必须是`naked type parameter`。

`naked type parameter`指的是**裸类型参数**，这个裸是指类型参数没有被包装在其他类型里，比如没有被数组，元组，函数和Promise等等包裹。例如：

```typescript
// 裸类型参数,没有被任何其他类型包裹即T
type NakedUsage<T> = T extends boolean ? "YES" : "NO"
// 类型参数被包裹的在元组内即[T]
type WrappedUsage<T> = [T] extends [boolean] ? "YES" : "NO";
```

而分布式有条件类型在实例化时会自动分发成联合类型，例如：

```typescript
type Distributed = NakedUsage<number | boolean> //  = NakedUsage<number> | NakedUsage<boolean> =  "NO" | "YES"
type NotDistributed = WrappedUsage<number | boolean > // "NO"
```

当我们给类型`NakedUsage`加入联合类型`number | boolean`时,它的结果返回`"NO" | "YES"`,相当于联合类型中的`number`和`boolean`分别赋予了`NakedUsage`然后再返回出一个联合类型,这个操作大家可以类比JavaScript中的`Array.map()` 。

我们看`NotDistributed`的结果,他接受的同样是联合类型`number | boolean`,但是返回一个特定的类型`"NO"`,而非一个联合类型,就是因为他的类型参数是被包裹的即`[]`,不会产生分布式有条件类型的特性。

#### 条件类型和映射类型

条件类型还可以和映射类型联合起来使用。

### infer关键字

看官方网站。

### 常见工具类型

官方内置了`ReturnType`，`Partial`，`ConstructorParameters`，`Pick`的工具类型，当然除了这些内置的工具类型，我们还可以手动设计工具类型。

看官方网站。

## 实用技巧

### 注释

通过`/** */`可以来注释`TS`的类型，这样在多人协作的时候就会有注释提示。

### 巧用类型推断

```typescript
function add(a: number, b: number) {
    return a+b
}
```

上述代码中，`add`函数返回值会被自动推导成`number`类型。

如果想获取`add`函数的类型，你可以通过：

```typescript
type AddFn = typeof add					// (a: number, b: number) => number
```

当然上述情况算是比较简单的情况，有时候我们的返回值类型其实比较复杂，这个时候借助类型推导和 `ReturnType` 就可以很轻松地获取返回值类型。

```typescript
type returnType = ReturnType<typeof add> // number
```

上述技巧在对`redux`进行编码的时候非常适用，这样可以省略我们大量的重复代码。

### 巧用元组

有时候我们可能需要批量的来获取参数，并且每一个参数的类型还不一样，我们可以声明一个元组如：

```typescript
function query(...args:[string, number, boolean]){
  const d: string = args[0];
  const n: number = args[1];
  const b: boolean = args[2];
}
```

### 巧用Omit

有时候我们需要复用一个类型，但是又不需要此类型内的全部属性，因此需要剔除某些属性，这个时候 `Omit` 就派上用场了。

```typescript
interface User {
    username: string
    id: number
    token: string
    avatar: string
    role: string
}
type UserWithoutToken = Omit<User, 'token'>
```

这个方法在`React`中经常用到，当父组件通过`props`向下传递数据的时候，通常需要复用父组件的`props`类型，但是又需要剔除一些无用的类型。

### 运用Record

`Record`是`TS`的一个高级类型，允许从`Union`类型中创建新类型，`Union`类型中的值用作新类型的属性。

```typescript
type Car = 'Audi' | 'BMW' | 'MercedesBenz'
type CarList = Record<Car, {age: number}>

const cars: CarList = {
    Audi: { age: 119 },
    BMW: { age: 113 },
    MercedesBenz: { age: 133 },
}
```

### 巧用类型约束

在 .tsx 文件里，泛型可能会被当做 jsx 标签

```tsx
const toArray = <T>(element: T) => [element]; 			// Error in .tsx file.
```

加 extends 可破

```tsx
const toArray = <T extends {}>(element: T) => [element]; // No errors.
```

## 模块和命名空间

我们新建两个`ts`文件，分别输入`const a = 1`，你会发现代码报错。这是因为尽管`a`遍历在两个不同的文件中声明，但是还是共用一个全局环境。为了避免命名冲突的问题，你需要使用模块化改进，将声明写成`export const a = 1`，这样代码就不会报错了。

注意：`TS`兼容`ES6`的模块化规范。

另外命名空间也是用来解决命名冲突问题：

```typescript
namespace XiaoMing { 
  export const name = 'ugu'
}

namespace XiaoNing { 
  export const name = 'ugu'
}

console.log(XiaoMing.name)
console.log(XiaoNing.name)
```

> 命名空间本质上是一个自执行函数。

命名空间在实际开发中很少使用，这是因为`ES6`的模块化规范已经能够很好的组织代码了，但是命名空间并非一无是处，通常在一些非`TS`的原生代码的`.d.ts`文件中使用。

## 编写声明文件

### 声明文件

由于很多第三方模块都是使用`JavaScript`编写的，尽管一些知名模块已经被`@types`定义好声明文件了，但是还有一些包我们需要手动编写`.d.ts`声明文件。

关键字 `declare` 表示声明的意思,我们可以用它来做出各种声明:

- `declare var` 声明全局变量
- `declare function` 声明全局方法
- `declare class` 声明全局类
- `declare enum` 声明全局枚举类型
- `declare namespace` 声明（含有子属性的）全局对象
- `interface` 和 `type` 声明全局类型

```typescript
// 声明变量
declare const jQuery: (selector: string) => any

// 声明类
declare class Person {
    name: string
    constructor(name: string);
    say(): string;
}

// 声明枚举
declare enum Directions {
    Red,
    Blue
}

// 声明命名空间
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
    
// 声明interface和type
interface AjaxSettings {
    method?: 'GET' | 'POST'
    data?: any;
}
declare namespace jQuery {
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

### 声明合并

另外还可以组合多个声明语句，进行声明合并。例如：`jQuery`既可以直接被当作函数调用，又有`jQuery.ajax`等子属性。

```typescript
declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

### 自动生成声明文件

当然，如果你的代码是通过`ts`编写的，那么在编译`js`的时候可以加上`-d`参数自动生成声明文件，或者在配置文件中开启`declartion`配置。

### 发布声明文件

你可以为某个开源库编写声明文件，然后进行发布，发布途径如下：

* 向源代码库提交`PR`
* 向` DefinitelyTyped `库进行发布

具体如何发布看官网。

## 编译原理

`Babel`暴露接口给开发者编写插件，通过插件可以定制自己的`JavaScript`，而`TypeScript`在`2.3`版本也暴露了相关的接口给开发者，这样你就可以编写自己的`TypeScript`插件来定制生成`JavaScript`代码。

`TypeScript`的编译器由如下部分组成：

* Scanner 扫描器
* Parser 解析器
* Binder 绑定器
* Emitter 发射器
* Checker 检查器

具体过程如下：

1. 扫描器通过扫描源代码生成`token`流
2. 解析器将`token`流解析为`AST`
3. 绑定器将`AST`中的声明节点与相同实体的其他声明相连形成符号，符号是语义系统的主要构造块 
4. 检查其通过符号和`AST`来验证源代码语义
5. 最后通过发射器来生成`JavaScript`代码

上面过程可以粗略的总结为：

* 解析代码生成`AST`
* 转换旧`AST`成新`AST`
* 将新的`AST`生成新的代码

其中遍历转换的工作，可以通过第三方插件或自身编译器来实现：

* 自身`tsc`编译器

* `ts-loader`代替`tsc`
* `ttypescript`代替`tsc`
* 自身实现一个编译器

## 工程化

## 配置文件

通过`tsc --init`可以生成配置文件，具体含义如下：

```json
{
  "compilerOptions": {
    "target": "es5",
      /* 
      	target用于指定编译之后的版本目标: 
      	'ES3' (default), 'ES5', 'ES2015', 
      	'ES2016', 'ES2017', 'ES2018', 
      	'ES2019' or 'ESNEXT'. 
      	*/
    "module": "commonjs", 
      /* 
      	用来指定要使用的模块标准: 'none', 
      	'commonjs', 'amd', 'system', 
      	'umd', 'es2015', or 'ESNext'. 
      	*/
    "lib": ["es6", "dom"] 		/* lib用于指定要包含在编译中的库文件 */,
    "allowJs": true,                       
     /* allowJs设置的值为true或false，用来指定是否允许编译js文件，默认是false，即不编译js文件 */
    "checkJs": true,                       
      /* checkJs的值为true或false，用来指定是否检查和报告js文件中的错误，默认是false */
    "jsx": "preserve",                     
      /* 指定jsx代码用于的开发环境: 'preserve', 'react-native', or 'react'. */
    "declaration": true,                   
      /* 
      	declaration的值为true或false，
      	用来指定是否在编译的时候生成相应的".d.ts"声明文件。
      	如果设为true，编译每个ts文件之后会生成一个
      	js文件和一个声明文件。但是declaration和allowJs不能同时设为true 
      */
    "declarationMap": true,                
      /* 值为true或false，指定是否为声明文件.d.ts生成map文件 */
    "sourceMap": true,                     
      /* sourceMap的值为true或false，用来指定编译时是否生成.map文件 */
    "outFile": "./dist/main.js",                       
      /* 
      	outFile用于指定将输出文件合并为一个文件，
      	它的值为一个文件路径名。比如设置为"./dist/main.js"，
      	则输出的文件为一个main.js文件。但是要注意，只有设置
      	module的值为amd和system模块时才支持这个配置 
      */
    "outDir": "./dist",                        
      /* outDir用来指定输出文件夹，值为一个文件夹路径字符串，输出的文件都将放置在这个文件夹 */
    "rootDir": "./",                       
      /* 
      	用来指定编译文件的根目录，编译器会在根目录查找入口文件，
      	如果编译器发现以rootDir的值作为根目录查找入口文件并不会
      	把所有文件加载进去的话会报错，但是不会停止编译 
      */
    "composite": true,                     /* 是否编译构建引用项目  */
    "removeComments": true,                
      /* 
      	removeComments的值为true或false，
      	用于指定是否将编译后的文件中的注释删掉，设为true的话即删掉注释，默认为false 
      */
    "noEmit": true,                        /* 不生成编译文件，这个一般比较少用 */
    "importHelpers": true,                 
      /* importHelpers的值为true或false，指定是否引入tslib里的辅助工具函数，默认为false */
    "downlevelIteration": true,            
      /* 
      	当target为'ES5' or 'ES3'时，为'for-of', 
      	spread, and destructuring'中的迭代器提供完全支持 
      */
    "isolatedModules": true,               
      /* 
      	isolatedModules的值为true或false，指定是否将每个
      	文件作为单独的模块，默认为true，它不可以和declaration同时设定 
      */

    /* Strict Type-Checking Options */
    "strict": true 
      /* 
      	strict的值为true或false，用于指定是否启动所有类型检查，
      	如果设为true则会同时开启下面这几个严格类型检查，默认为false 
      */,
    "noImplicitAny": true,                 
      /* 
      	noImplicitAny的值为true或false，如果我们没有为一些
      	值设置明确的类型，编译器会默认认为这个值为any，
      	如果noImplicitAny的值为true的话。则没有
      	明确的类型会报错。默认值为false 
      */
    "strictNullChecks": true,              
      /* 
      	strictNullChecks为true时，null和undefined值不
      	能赋给非这两种类型的值，别的类型也不能赋给他们，除了
      	any类型。还有个例外就是undefined可以赋值给void类型 
      */
    "strictFunctionTypes": true,           
      /* strictFunctionTypes的值为true或false，用于指定是否使用函数参数双向协变检查 */
    "strictBindCallApply": true,           
      /* 设为true后会对bind、call和apply绑定的方法的参数的检测是严格检测的 */
    "strictPropertyInitialization": true,  
      /* 
      	设为true后会检查类的非undefined属性是否已经在构造函数里
      	初始化，如果要开启这项，需要同时开启strictNullChecks，默认为false 
      */
   "noImplicitThis": true,                
      /* 当this表达式的值为any类型的时候，生成一个错误 */
    "alwaysStrict": true,                  
      /* 
      	alwaysStrict的值为true或false，指定始终以严格模式检查每个模块，
      	并且在编译之后的js文件中加入"use strict"字符串，用来告诉浏览器该js为严格模式 
      */

    "noUnusedLocals": true,                
      /* 
      	用于检查是否有定义了但是没有使用的变量，对于这一点的检测，
      	使用eslint可以在你书写代码的时候做提示，你可以配合使用。
      	它的默认值为false 
      */
    "noUnusedParameters": true,            
      /* 用于检查是否有在函数体中没有使用的参数，这个也可以配合eslint来做检查，默认为false */
    "noImplicitReturns": true,             
      /* 用于检查函数是否有返回值，设为true后，如果函数没有返回值则会提示，默认为false */
    "noFallthroughCasesInSwitch": true,    
      /* 用于检查switch中是否有case没有使用break跳出switch，默认为false */

    /* Module Resolution Options */
    "moduleResolution": "node",            
      /* 用于选择模块解析策略，有'node'和'classic'两种类型' */
    "baseUrl": "./",                       
      /* baseUrl用于设置解析非相对模块名称的基本目录，相对模块不会受baseUrl的影响 */
    "paths": {},                           /* 用于设置模块名称到基于baseUrl的路径映射 */
    "rootDirs": [],                        
      /* rootDirs可以指定一个路径列表，在构建时编译器会将这个路径列表中的路径的内容都放到一个文件夹中 */
    "typeRoots": [],                       
      /* 
      	typeRoots用来指定声明文件或文件夹的路径列表，
      	如果指定了此项，则只有在这里列出的声明文件才会被加载 
      */
    "types": [],                           
      /* types用来指定需要包含的模块，只有在这里列出的模块的声明文件才会被加载进来 */
    "allowSyntheticDefaultImports": true,  
      /* 用来指定允许从没有默认导出的模块中默认导入 */
    "esModuleInterop": true 
      /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性 */,
    "preserveSymlinks": true,              
      /* 不把符号链接解析为其真实路径，具体可以了解下webpack和nodejs的symlink相关知识 */

    /* Source Map Options */
    "sourceRoot": "",                      
      /* sourceRoot用于指定调试器应该找到TypeScript文件而不是源文件位置，这个值会被写进.map文件里 */
    "mapRoot": "",                         
      /* 
      	mapRoot用于指定调试器找到映射文件而非生成文件的位置，
      	指定map文件的根路径，该选项会影响.map文件中的sources属性 
      */
    "inlineSourceMap": true,               
      /* 
      	指定是否将map文件的内容和js文件编译在同一个js文件中，
      	如果设为true，则map的内容会以//# sourceMappingURL=
      	然后拼接base64字符串的形式插入在js文件底部 
      */
    "inlineSources": true,                 
      /* 用于指定是否进一步将.ts文件的内容也包含到输入文件中 */

    /* Experimental Options */
    "experimentalDecorators": true 			/* 用于指定是否启用实验性的装饰器特性 */
    "emitDecoratorMetadata": true,         
      /* 
      	用于指定是否为装饰器提供元数据支持，关于元数据，也是ES6的新标准，
      	可以通过Reflect提供的静态方法获取元数据，如果需要使用Reflect的一些方法，
      	需要引入ES2015.Reflect这个库 
      */
  }
  "files": [], 
// files可以配置一个数组列表，里面包含指定文件的相对或绝对路径，编译器在编译的时候只会编译包含在files中列出的文件，如果不指定，则取决于有没有设置include选项，如果没有include选项，则默认会编译根目录以及所有子目录中的文件。这里列出的路径必须是指定文件，而不是某个文件夹，而且不能使用* ? **/ 等通配符
  "include": [],  
// include也可以指定要编译的路径列表，但是和files的区别在于，这里的路径可以是文件夹，也可以是文件，可以使用相对和绝对路径，而且可以使用通配符，比如"./src"即表示要编译src文件夹下的所有文件以及子文件夹的文件
  "exclude": [],  
// exclude表示要排除的、不编译的文件，它也可以指定一个列表，规则和include一样，可以是文件或文件夹，可以是相对路径或绝对路径，可以使用通配符
  "extends": "",   
// extends可以通过指定一个其他的tsconfig.json文件路径，来继承这个配置文件里的配置，继承来的文件的配置会覆盖当前文件定义的配置。TS在3.2版本开始，支持继承一个来自Node.js包的tsconfig.json配置文件
  "compileOnSave": true,  
// compileOnSave的值是true或false，如果设为true，在我们编辑了项目中的文件保存的时候，编辑器会根据tsconfig.json中的配置重新生成文件，不过这个要编辑器支持
  "references": [],  
// 一个对象数组，指定要引用的项目
}
```

该文件的使用规则如下：

* 如果直接运行`tsc`，此时编译器会从目录开始搜索该配置文件
* 如果直接运行`tsc -p folder`，folder目录下必须存在该配置文件
* 如果直接运行`tsc index.ts`，编译器将忽略该配置文件

### 代码检测

`TypeScript`主要用于检查类型和语法错误，而代码检测工具主要用于检查代码风格，其作用是统一团队的代码风格，便于项目维护迭代。所以，相较于`TypeScript`，代码检测工具更关注代码风格，如：

* 缩进是几个空格
* 是否禁用`var`
* ...

一开始，对于`ts`文件会使用`tslint`来进行风格校验，由于`TSLint`直接寄生于`TypeScript`之下，他们的`parser`是相同的，产生的`AST`也是相同的，所以`tslint`对`typescript`支持更友好。但是由于性能原因，现在已经被废弃，转向`eslint`。

但是`eslint`的`parser`是基于`estree`，对`typescript`支持并不友好，要做很多兼容性工作，并且一直没有做好。但是目前`TypeScript`支持`ESLint`，共同发布了`typescript-eslint`插件来解决兼容性问题。也就是说`eslint`已经完美支持`typescript`。

另外， 相较于`tslint`，`eslint`的生态也更加完善，并且可配置的规则很多。

你可以通过`npx eslint --init`生成配置文件，这里我们分别选择：

* How would you like to use ESLint? **To check syntax, find problems, and enforce code style**
* What type of modules does your project use? **JavaScript modules (import/export)**
* Which framework does your project use? **None of these**
* Does your project use TypeScript? **Yes**
* Where does your code run? **Browser, Node**
* How would you like to define a style for your project? **Use a popular style guide**
* Which style guide do you want to follow? **Airbnb**
* What format do you want your config file to be in? **JavaScript**
* Would you like to install them now with npm? y

配置文件的具体配置规则如下：

* env: 预定义那些环境需要用到的全局变量，可用的参数是：es6、broswer、node等
* extends: 指定扩展的配置，配置支持递归扩展，支持规则的覆盖和聚合
* plugins: 配置那些我们想要Linting规则的插件。
* parser: 默认ESlint使用Espree作为解析器，我们也可以用其他解析器在此配置
* parserOptions: 解析器的配置项
* rules: 自定义规则，可以覆盖掉extends的配置
* globals： 脚本在执行期间访问的额外的全局变量

### 单元测试

具体看官网。

