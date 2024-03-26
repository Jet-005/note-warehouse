## ts基础语法

## **安装**

- **全局安装**typescript，把ts编译成js运行

```text
npm install -g typescript
tsc hello.ts
```

- **全局安装**ts-node，直接运行ts文件

```text
npm install -g ts-node
ts-node hello.ts
```

## **ts基础类型**

### **布尔值**

```ts
let isDone: boolean = false;
```

### **数字**

```ts
let decLiteral: number = 6;
```

### **字符串**

```text
let name: string = "bob";
//也支持模板字符串
let sentence: string = `Hello, my name is ${ name }`
```

### **数组**

```text
let list: number[] = [1, 2, 3];
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```text
let list: Array<number> = [1, 2, 3];
```

### **元组 Tuple**

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string`和`number`类型的元组。

```text
let x: [string, number];
x = ['hello', 10];      // OK
x = [10, 'hello'];     // Error
```

### **枚举**

`enum`类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

### 数字枚举

```text
enum Color {Red, Green, Blue}
let c: Color = Color.Green;     //1
```

默认情况下，从`0`开始为元素编号。 你也可以手动的指定成员的数值。

```text
enum Color {Red = 10, Green, Blue}
let c: Color = Color.Green;   //11
```

数字枚举除了支持 **从成员名称到成员值** 的普通映射之外，它还支持 **从成员值到成员名称** 的反向映射

```text
console.log(Color[10]);  //Red
```

### 异构枚举

异构枚举的成员值是数字和字符串的混合：

```text
enum Enum {
    A,
    B,
    C = "C",
    D = "D",
    E = 8,
    F
}
console.log(Enum['F'])    //9
```

以上代码对应的 ES5 代码如下：

```text
"use strict";
var Enum;
(function (Enum) {
    Enum[Enum["A"] = 0] = "A";
    Enum[Enum["B"] = 1] = "B";
    Enum["C"] = "C";
    Enum["D"] = "D";
    Enum[Enum["E"] = 8] = "E";
    Enum[Enum["F"] = 9] = "F";
})(Enum || (Enum = {}));
```

通过观察上述生成的 ES5 代码，我们可以发现数字枚举相对字符串枚举多了 “反向映射”

### 常量枚举

除了数字枚举和字符串枚举之外，还有一种特殊的枚举 —— 常量枚举。它是使用 `const` 关键字修饰的枚举，常量枚举会使用内联语法，不会为枚举类型编译生成任何 JavaScript

```text
const enum Direction {
  NORTH,
  SOUTH,
  EAST,
  WEST,
}
let dir: Direction = Direction.NORTH;

以上代码对应的 ES5 代码如下：
"use strict";
var dir = 0 /* NORTH */;
```

### **any**

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量：

```text
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; 
```

### **unknown**

就像所有类型都可以赋值给 `any`，所有类型也都可以赋值给 `unknown`。这使得 `unknown` 成为 TypeScript 类型系统的另一种顶级类型（另一种是 `any`）。

```text
let value: unknown;
value = true; // OK
value = 42; // OK
value = "Hello World"; // OK
value = []; // OK
value = {}; // OK
value = Math.random; // OK
value = null; // OK
value = undefined; // OK
value = new TypeError(); // OK
value = Symbol("type"); // OK
```

`unknown` 类型只能被赋值给 `any` 类型和 `unknown` 类型本身。直观地说，这是有道理的：只有能够保存任意类型值的容器才能保存 `unknown` 类型的值。毕竟我们不知道变量 `value` 中存储了什么类型的值。

```text
let value: unknown;
let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error
let value4: number = value; // Error
```

### **any 和 unknown**

any`表示任意类型，这个类型会逃离`Typescript`的类型检查，和在`Javascript`中一样，`any类型的变量可以执行任意操作，编译时不会报错。`unknown` 也可以表示任意类型，但它同时也告诉 `Typescript` 开发者对其也是一无所知，做任何操作时需要慎重。这个类型仅可以执行有限的操作（`==、=== 、||、&&、?、!、typeof、instanceof` 等等），其他操作需要向 `Typescript` 证明这个值是什么类型，否则会提示异常。

```text
//any和unknown
let foo: any;
let bar: unknown;

let num: number;
num = foo;
num = bar;

foo.NoFun();
bar.NoFun(); 
any` 会增加运行时出错的风险，不到万不得已不要使用。表示【不知道什么类型】的场景下使用 `unknown
```

### **void**

某种程度上来说，void 类型像是与 any 类型相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到其返回值类型是 void：

```text
// 声明函数返回值为void
function warnUser(): void {
  console.log("This is my warning message");
}
```

需要注意的是，声明一个 void 类型的变量没有什么作用，因为在严格模式下，它的值只能为 `undefined`：

```text
let unusable: void = undefined;
```

### **null 和 undefined**

TypeScript 里，`undefined` 和 `null` 两者有各自的类型分别为 `undefined` 和 `null`。

```text
let u: undefined = undefined;
let n: null = null;
```

### **object 和{} 和 Object**

`object` 表示的是常规的 `Javascript` 对象类型，非基础数据类型。

```text
declare function create(o: object): void;

create({ prop: 0 }); // OK
create(null); // Error
create(undefined); // Error
create(42); // Error
create("string"); // Error
create(false); // Error
create({
  toString() {
    return 3;
  },
}); // OK
```

`{}` 表示的非 null，非 undefined 的任意类型。

```text
declare function create(o: {}): void;

create({ prop: 0 }); // OK
create(null); // Error
create(undefined); // Error
create(42); // OK
create("string"); // OK
create(false); // OK
create({
  toString() {
    return 3;
  },
}); // OK
```

`Object` 和 `{}` 几乎一致，区别是 `Object` 类型会对 `Object` 原型内置的方法（`toString/hasOwnPreperty`）进行校验。

```text
declare function create(o: Object): void;

create({ prop: 0 }); // OK
create(null); // Error
create(undefined); // Error
create(42); // OK
create("string"); // OK
create(false); // OK
create({
  toString() {
    return 3;
  },
}); // Error
```

如果需要一个对象类型，但对对象的属性没有要求，使用 `object`。`{}` 和 `Object` 表示的范围太泛尽量不要使用。

### **never 类型**

`never` 类型表示的是那些永不存在的值的类型。 例如，`never` 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。

```text
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

## **类型断言**

有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

### 1.“尖括号” 语法

```text
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

### 2.as 语法

```text
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;

//类型断言
const assertionGetLength = (something: string | number): number => {
    // if ((something as string).length) {  //as 语法
    //     //告诉TS something 就是字符串类型
    //     return (something as string).length;
    // }
    if ((<string>something).length) {
        //“尖括号” 语法
        //告诉TS something 就是字符串类型
        return (<string>something).length;
    } else {
        console.log("jjjjjj");
        return something.toString().length;
    }
};
console.log(assertionGetLength(234));
```

## **类型守卫**

**类型保护是可执行运行时检查的一种表达式，用于确保该类型在一定的范围内。** 换句话说，类型保护可以保证一个字符串是一个字符串，尽管它的值也可以是一个数值。类型保护与特性检测并不是完全不同，其主要思想是尝试检测属性、方法或原型，以确定如何处理值。目前主要有四种的方式来实现类型保护：

- 类型判断： `typeof`
- 实例判断： `instanceof`
- 属性判断： `in`
- 字面量相等判断：`==`，`===`，`!=`， `!==`

### 类型判断（typeof）

`typeof` 类型保护只支持两种形式：`typeof v === "typename"` 和 `typeof v !== typename`，`"typename"` 必须是 `"number"`， `"string"`， `"boolean"` 或 `"symbol"`。 但是 TypeScript 并不会阻止你与其它字符串比较，语言不会把那些表达式识别为类型保护

```text
function test(own: string | boolean | number) {
  if (typeof own == 'string') {
    // 这里 own 的类型限制为 string
  } else if (typeof own == 'number') {
    // 这里 own 的类型限制为 number
  } else {
      // 这里 own 的类型限制为 boolean
  }
}
```

### 属性判断(in)

```text
interface one {
  name: string;
  speak:string;
}

interface two {
  age: number;
  see:string;
}

function test(own:one | two){
  console.log("Name: " + own.name);
  if ("name" in own) { 
    //这里限制为own 对象为one
    console.log(own.speak);
  }
  if ("see" in own) {
    //这里限制为own限制的对象为two
    console.log(own.see);
  }
}
```

### 实例判断（instanceof）

```text
interface  Padder {
  getPaddingString():string
}
class Space implements Padder {
  constructor(private numSpaces: number) {} 
  getPaddingString() {
    return Array(this.numSpaces + 1).join(' ');
  }
}
class StringPadder implements Padder {
  constructor(private value: string) {}
  getPaddingString() {
    return this.value;
  }
}
function getrandom() {
  return Math.random() < 0.5 ? new Space(4) : new StringPadder('');
}

let padder: Padder = getrandom();

//判断padder是否是Space的实例对象,如果中间有其他值覆盖了，会出现问题
if (padder instanceof Space) {
  //判断后，确保这个值是它的实例对象 padder类型收缩在'SpaceRepeatingPadder'
}
```

### **自定义类型守卫**

返回布尔值条件函数

```text
function isString (own: any): own is string {
  return typeof own === 'string';
}
function test (xdom:any) {
 if (isString(xdom)) {
        //xdom 限制为 'string'
    } else {
     //其他类型
    }
}
```

## **类型别名和接口**

### **类型别名**

类型别名用来给一个类型起个新名字,用**type**定义

```text
type Message = string | string[];
let init: Message;
init = ['www', 'wwww'];
```

### **接口**

TypeScript 中的接口是一个非常灵活的概念，除了可用于[对类的一部分行为进行抽象](https://link.zhihu.com/?target=https%3A//ts.xcatliu.com/advanced/class-and-interfaces.html%23%E7%B1%BB%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)以外，也常用于对「对象的形状（Shape）」进行描述。

### 对象的形状

```text
interface Person {
    name: string;
    age: number;
}
let semlinker: Person = {
    name: "semlinker",
    age: 33,
};
```

### 可选 | 只读属性

```text
interface Person {
  readonly name: string;
  age?: number;
}
```

只读属性用于限制只能在对象刚刚创建的时候修改其值。此外 TypeScript 还提供了 `ReadonlyArray<T>` 类型，它与 `Array<T>` 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。

```text
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

### 任意属性

有时候我们希望一个接口中除了包含必选和可选属性之外，还允许有其他的任意属性，这时我们可以使用 **索引签名** 的形式来满足上述要求。

```text
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

const p1 = { name: "semlinker" };
const p2 = { name: "lolo", age: 5 };
const p3 = { name: "kakuqo", sex: 1 }
```

### **接口与类型别名的区别**

### 1.Objects/Functions

接口和类型别名都可以用来描述对象的形状或函数签名：

**接口**

```text
//对象的形状
interface Point {
  x: number;
  y: number;
}
//函数签名
interface SetPoint {
  (x: number, y: number): void;
}
```

**类型别名**

```text
type Point = {
  x: number;
  y: number;
};

type SetPoint = (x: number, y: number) => void;
```

### 2.Other Types

与接口类型不一样，类型别名可以用于一些其他类型，比如原始类型、联合类型和元组：

```text
// primitive
type Name = string;

// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];
```

## **联合类型和交叉类型**

### **联合类型**

**取值可以为多种类型中的一种**,通常与 `null` 或 `undefined` 一起使用

```text
let stringAndNumber: string | number;
```

### **交叉类型**

在 TypeScript 中交叉类型是将多个类型合并为一个类型。通过 `&` 运算符可以将现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。

```text
type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };

let point: Point = {
  x: 1,
  y: 1
}
```

## **TypeScript 类**

### **类的属性与方法**

在 TypeScript 中，我们可以通过 `Class` 关键字来定义一个类：

```text
class Greeter {
  // 静态属性
  static cname: string = "Greeter";
  // 成员属性
  greeting: string;

  // 构造函数 - 执行初始化操作
  constructor(message: string) {
    this.greeting = message;
  }

  // 静态方法
  static getClassName() {
    return "Class name is Greeter";
  }

  // 成员方法
  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```

那么成员属性与静态属性，成员方法与静态方法有什么区别呢？这里无需过多解释，我们直接看一下编译生成的 ES5 代码：

```text
"use strict";
var Greeter = /** @class */ (function () {
    // 构造函数 - 执行初始化操作
    function Greeter(message) {
      this.greeting = message;
    }
    // 静态方法
    Greeter.getClassName = function () {
      return "Class name is Greeter";
    };
    // 成员方法
    Greeter.prototype.greet = function () {
      return "Hello, " + this.greeting;
    };
    // 静态属性
    Greeter.cname = "Greeter";
    return Greeter;
}());
var greeter = new Greeter("world");
```

### **访问器**

在 TypeScript 中，我们可以通过 `getter` 和 `setter` 方法来实现数据的封装和有效性校验，防止出现异常数据。

```text
let passcode = "Hello TypeScript";

class Employee {
  private _fullName: string;

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (passcode && passcode == "Hello TypeScript") {
      this._fullName = newName;
    } else {
      console.log("Error: Unauthorized update of employee!");
    }
  }
}

let employee = new Employee();
employee.fullName = "Semlinker";
if (employee.fullName) {
  console.log(employee.fullName);
}
```

### **类的继承**

继承（Inheritance）是一种联结类与类的层次模型。指的是一个类（称为子类、子接口）继承另外的一个类（称为父类、父接口）的功能，并可以增加它自己的新功能的能力，继承是类与类或者接口与接口之间最常见的关系。

```text
class Animal {
  name: string;
  
  constructor(theName: string) {
    this.name = theName;
  }
  
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends Animal {
  constructor(name: string) {
    super(name); // 调用父类的构造函数
  }
  
  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

let sam = new Snake("Sammy the Python");
sam.move();
```

### **抽象类**

使用 `abstract` 关键字声明的类，我们称之为抽象类。抽象类不能被实例化，因为它里面包含一个或多个抽象方法。所谓的抽象方法，是指不包含具体实现的方法：

```text
abstract class Person {
  constructor(public name: string){}
  abstract say(words: string) :void;
}
const lolo = new Person();     // Error
```

抽象类不能被直接实例化，我们只能实例化实现了所有抽象方法的子类。

```text
abstract class Person {
  constructor(public name: string){}

  // 抽象方法
  abstract say(words: string) :void;
}

class Developer extends Person {
  constructor(name: string) {
    super(name);
  }
  
  say(words: string): void {
    console.log(`${this.name} says ${words}`);
  }
}

const lolo = new Developer("lolo");
lolo.say("I love ts!");     // lolo says I love ts!
```

### **类方法重载**

对于类的方法来说，它也支持重载。比如，在以下示例中我们重载了 `ProductService` 类的 `getProducts` 成员方法：

```text
class ProductService {
    getProducts(): void;
    getProducts(id: number): void;
    getProducts(id?: number) {
      if(typeof id === 'number') {
          console.log(`获取id为 ${id} 的产品信息`);
      } else {
          console.log(`获取所有的产品信息`);
      }  
    }
}

const productService = new ProductService();
productService.getProducts(666); // 获取id为 666 的产品信息
productService.getProducts(); // 获取所有的产品信息 
```

## **泛型**

泛型（Generics）是允许同一个函数接受不同类型参数的一种模板。相比于使用 any 类型，使用泛型来创建可复用的组件要更好，因为泛型会保留参数类型。

### **泛型语法**

对于刚接触 TypeScript 泛型的读者来说，首次看到 `<T>` 语法会感到陌生。其实它没有什么特别，就像传递参数一样，我们传递了我们想要用于特定函数调用的类型。



![img](https://pic2.zhimg.com/80/v2-5901c758600b26dbf8b755086b4f2b35_720w.jpg)



参考上面的图片，当我们调用 `identity<Number>(1)` ，`Number` 类型就像参数 `1` 一样，它将在出现 `T` 的任何位置填充该类型。图中 `<T>` 内部的 `T` 被称为类型变量，它是我们希望传递给 identity 函数的类型占位符，同时它被分配给 `value` 参数用来代替它的类型：此时 `T` 充当的是类型，而不是特定的 Number 类型。

其中 `T` 代表 **Type**，在定义泛型时通常用作第一个类型变量名称。但实际上 `T` 可以用任何有效名称代替。除了 `T` 之外，以下是常见泛型变量代表的意思：

- K（Key）：表示对象中的键类型；
- V（Value）：表示对象中的值类型；
- E（Element）：表示元素类型。

### **泛型接口**

```text
interface GenericIdentityFn<T> {
  (arg: T): T;
}
```

### **泛型类**

```text
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```