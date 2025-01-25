# TypeScript

TS是JS的超集，将IS动态类型转为静态类型。

## 1 TS开发环境搭建

安装nodejs，npm命令是node的包管理器命令，ts解释器就是其中的一个包。

```
npm i -g typescript
```

ts文件浏览器还不支持，需要转换为js文件。在文件夹目录中输入cmd，进入cmd窗口，通过`tsc 文件名`进行编译。

## 2 TS类型声明

ts 类型隐式声明会自动辨别。

```typescript
// 变量声明
let uage: number
uage = 20
console.log(`I'm ${uage} years old.`)

// 变量声明同时赋值
let uname: string = 'tom'
uname = 'Alice'
console.log(`My name is ${uname}.`);

// 函数参数和返回值声明
function increment (a: number, b: number): number {
  return a + b
}
console.log(increment(1, 2)) // 3
```

**基本类型**

`number | string | boolean |null | undefined`。

`bigint`  表示任意精度的大整数 (ES2020)  `let large: bigint = 123n;`

`symbol`  唯一标识符 (ES6)  `let sym: symbol = Symbol();`

```typescript
// 字面量声明类型
// | 表示或
let gender: 'male' | 'female'
gender = 'male'
gender = 'female'
// gender = 'other' // ×
```

**特殊类型**

| 类型      | 描述                                    | 示例                         |
| --------- | --------------------------------------- | ---------------------------- |
| `any`     | 任意类型，关闭类型检查（不推荐）        | `let value: any = "hello";`  |
| `unknown` | 表示未知类型，安全版本的 `any`          | `let value: unknown;`        |
| `void`    | 表示无返回值类型 (常用于函数)           | `function log(): void {}`    |
| `never`   | 表示永远不会发生的类型 (抛错或无限循环) | `function error(): never {}` |

any

```typescript
// any类型
let a: any
a = 123
a = 'hello'

// 隐式any类型
let any_v
any_v = 123
any_v = 'hello'
```

unknown

断言

```typescript
变量 as 类型  a as string
<类型>变量  <string>a
```

```typescript
// unknown类型
let un_v: unknown
un_v = 18
un_v = '18岁'

let str: string
str = 'hello'
// 将 any 类型赋值给其他类型不会报错
str = any_v
// str = un_v  // 将 unknown 类型赋值给其他类型会报错
// 可以用类型断言解决
if (typeof un_v === 'string') {
  str = un_v
}
str = un_v as string
str = <string>un_v
```

void | never

```typescript
// void 常用于函数，无返回值类型
function saynothing () : void {
  return
}

// never 常用于函数报错情况
function err () :never {
  throw new Error('error')
}
```

 **复合类型**

| 类型     | 描述                                      | 示例                                       |
| -------- | ----------------------------------------- | ------------------------------------------ |
| `array`  | 数组两种声明方式`类型[]`，`Array<类型>`。 | `let nums: number[] = [1, 2, 3];`          |
| `tuple`  | 元组，定长数组。`[类型,类型]`。           | `let tuple: [string, number] = ["a", 1];`  |
| `object` | 定义动态属性`[props: string]: any`。      | `let obj: { name: string; age: number; };` |
| `enum`   | 枚举类型，定义一组命名常量。              | `enum Role { Admin, User };`               |

object，动态属性表示：`[propname: string] : any`。

```typescript
// object 类型 结构声明
// ? 表示可有可无
let people: {
  name: String,
  age: Number,
  phone?: String 
}

people = {
  name: 'Tome',
  age: 33,
  phone: 'ipnone'
}
people = {
  name: 'Tome',
  age: 33,
}
// [propname: string] : any 如果只确定一定属性，其他属性不确定
let house: {
  price: string,
  [propName: string]: any // 表示其他为 any 的任意属性
}
house = {
  price: '100万',
  address: '北京市朝阳区',
}
```

function，函数结构，箭头函数的形式。

```
// function 类型
let add : (a: number, b: number, c?: number) => number
```

数组和元组

数组两种声明方式`类型[]`，`Array<类型>`。

```typescript
// array 类型
// 2种声明方式
let arr: number[]  // 声明了为数字类型的数组
let arr1: Array<string> // 声明了为字符串类型的数组
arr = [1, 2, 3]
arr1 = ['a', 'b', 'c']

// tuple 类型 元组类型，固定长度的数组
let tuple: [string, number, boolean]
tuple = ['hello', 123, true]
```

enum

```typescript
// enum 类型 枚举类型，可以定义一组命名常量
enum Color {
  red = 0,
  green = 1,
  blue = 2
}
enum Size {
  default = 0,
  small = 1,
  medium = 2,
  large = 3
}

let goods: {
  name: string,
  price: string,
  color: Color,
  size: Size,
}
goods = {
  name: '小碎花法式衬裙',
  price: '199元',
  color: Color.red,
  size: Size.medium
}
```

自定义类型`type defaultFruit = 'apple' | 'banana' | 'orange'`。

```typescript
// 自定义类型
type defaultFruit = 'apple' | 'banana' | 'orange'
let fruit: defaultFruit = 'banana'
```

## 3 TS编译

创建tsconfig.json文件在根目录中

```
tsc --init
```

基本配置

```json
"include": [], // 编译哪些文件夹下的文件
"exclude": [], // 不编译哪些文件夹下的文件
```

完整示例。

```json
{
  "compilerOptions": {},
  // ** 表示文件夹
  // * 表示文件
  "include": ["src/**/*.ts"], // 编译范围
  "exclude": ["node_modules", "bower_components", "jspm_packages"], // 不编译范围
  "files": ["01-hello-world.ts", "02-variable.ts"], // 指定编译的文件
  "extends": ["./tsconfig.base.json"], // 继承配置
}
```

complierOptions配置。

```json
{
  "compilerOptions": {
    // 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'es2023', 'es2024', 'esnext'.
    "target": "ES2015", // 编译成JS的版本
    // 'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'es2022', 'esnext', 'node16', 'nodenext', 'preserve'.
        "module": "System", // 模块化
    // 'es5', 'es6', 'es2015', 'es7', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'es2023', 'es2024', 'esnext', 'dom', 'dom.iterable', 'dom.asynciterable', 'webworker', 'webworker.importscripts', 'webworker.iterable', 'webworker.asynciterable', 'scripthost', 'es2015.core', 'es2015.collection', 'es2015.generator', 'es2015.iterable', 'es2015.promise', 'es2015.proxy', 'es2015.reflect', 'es2015.symbol', 'es2015.symbol.wellknown', 'es2016.array.include', 'es2016.intl', 'es2017.arraybuffer', 'es2017.date', 'es2017.object', 'es2017.sharedmemory', 'es2017.string', 'es2017.intl', 'es2017.typedarrays', 'es2018.asyncgenerator', 'es2018.asynciterable', 'es2018.intl', 'es2018.promise', 'es2018.regexp', 'es2019.array', 'es2019.object', 'es2019.string', 'es2019.symbol', 'es2019.intl', 'es2020.bigint', 'es2020.date', 'es2020.promise', 'es2020.sharedmemory', 'es2020.string', 'es2020.symbol.wellknown', 'es2020.intl', 'es2020.number', 'es2021.promise', 'es2021.string', 'es2021.weakref', 'es2021.intl', 'es2022.array', 'es2022.error', 'es2022.intl', 'es2022.object', 'es2022.string', 'es2022.regexp', 'es2023.array', 'es2023.collection', 'es2023.intl', 'es2024.arraybuffer', 'es2024.collection', 'es2024.object', 'es2024.promise', 'es2024.regexp', 'es2024.sharedmemory', 'es2024.string', 'esnext.array', 'esnext.collection', 'esnext.symbol', 'esnext.asynciterable', 'esnext.intl', 'esnext.disposable', 'esnext.bigint', 'esnext.string', 'esnext.promise', 'esnext.weakref', 'esnext.decorators', 'esnext.object', 'esnext.regexp', 'esnext.iterator', 'decorators', 'decorators.legacy'.
    "lib": ["ES2015", "DOM", "DOM.Iterable", "ESNext.AsyncIterable"], // 库文件
    "outDir": "./dist", // 编译输出目录
    "outFile": "./dist/app.js", // 编译输出文件，配合打包工具。
    "allowJs": false, // 允许编译JS文件
    "checkJs": false, // 类型检查JS文件
    "jsx": "react-jsx", // 编译JSX文件
    "removeComments": false, // 移除注释
    "noEmit": false, // 不生成编译文件
    "noEmitOnError": false, // 编译出错时不生成编译文件
    "noImplicitAny": false, // 隐式any检查
    "noImplicitThis": false, // 不明确this是否禁用
    "strict": true, // 严格模式
    "alwaysStrict": true, // 严格模式
    "strictNullChecks": true, // 严格空检查
    "strictFunctionTypes": true, // 严格函数类型检查
  },
}
```

## 4 webpack打包TS文件

在项目根目录生成package.json文件。

```
npm init -y
```

下载webpack打包工具

```
npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader html-webpack-plugin clean-webpack-plugin
```

- webpack：构建工具webpack
- webpack-cli：webpack的命令行工具
- webpack-dev-server：webpack的开发服务器
- typescript：ts编译器
- ts-loader：ts加载器，用于在webpack中编译ts文件
- html-webpack-plugin：webpack中html插件，用来自动创建html文件
- clean-webpack-plugin：webpack中的清除插件，每次构建都会先清除目录

创建webpack.config.js，配置。

```js
const path = require('path');
const HTMLWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  // 程序入口文件
  entry: './src/index.ts',
  // 文件打包所在目录
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  // 指定webpack打包时所需要的模块
  module: {
    rules: [
      {
        // 规则生效的文件
        test: /\.ts$/,
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: [
                [
                  "@babel/preset-env",
                  {
                    "targets": { "chrome": "85" },
                    "corejs": "3",
                    "useBuiltIns": "usage",
                  },
                ],
              ],
            },
          },
          {
            loader: 'ts-loader',
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
  // 插件
  plugins: [
    new HTMLWebpackPlugin({
      template: './src/index.html', // 修正路径
    }),
    new CleanWebpackPlugin(),
  ],
  // 模块解析
  resolve: {
    extensions: ['.ts', '.js'], // 添加文件后缀的解析
  },
};
```

在package.json中配置`"build": "webpack",`。`npm run build`执行webpack。

安装完`webpack-dev-server`，配置`"start": "webpack serve --open chrome.exe"`，`npm start`启动该服务器。

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack serve --open chrome.exe"
  },
```

安装babel

```
npm i -D @babel/core @babel/preset-env babel-loader core-js
```

在webpack.config.js中配置。

```js
  // 指定webpack打包时所需要的模块
  module: {
    rules: [
      {
        // 规则生效的文件
        test: /\.ts$/,
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: [
                [
                  "@babel/preset-env",
                  {
                    "targets": { "chrome": "85" },
                    "corejs": "3",
                    "useBuiltIns": "usage",
                  },
                ],
              ],
            },
          },
          {
            loader: 'ts-loader',
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
```

## 5 面向对象

### 5.1 class

实例属性，静态属性，只读，方法

```typescript
enum Gender {
  'male' = 0,
  'female' = 1,
}
class Person {
  // 实例方法，需要创建实例才能访问
  name: string = '哪吒';
  age: string = '18岁';
  // 静态方法，不需要创建实例，直接通过类访问
  static gender: Gender = Gender.male
  sayHello () {
    console.log('你好');
  }
  readonly skill: string[] = ['风火轮', '金箍棒']
}
// 实例属性可读，可改
const per = new Person
per.name = '东海龙王'
console.log(per.name);
console.log(per.skill);
console.log(per.sayHello);


// 静态属性
console.log(Person.gender);
```

### 5.2 constructor

构造函数`constructor() {}`，注意`this`指向，静态属性中的`this`指向不同。

```typescript
// 构造函数
class Dog {
  name: string;
  age: number;
  constructor (name: string, age: number) {
    // this 指向实例对象
    this.name = name,
    this.age = age
  };
  bark () {
    // this 指向调用方法的对象
    console.log(`${this.name} says: wow!Wow!`);
    
  }
}

const blackDog = new Dog('小黑',5)
const whitDog = new Dog('小白', 2)
console.log(blackDog.bark);
console.log(whitDog.bark);
```

### 5.3 extends & super & abstract

继承`extends`，`super()`继承父类的属性和方法。

构造函数中继承`super(props1, props2...)`。

方法继承`super.methodsName`。

```typescript
/** 
 * 继承 extends
 * super 继承父类的实例属性和方法
 * 子类可以修改父类的方法，称为重写
*/ 

class Animal {
species: string;
foods: string[];
constructor (species: string, foods: string[]) {
this.species = species;
this.foods = foods;
}
sayHello () {
console.log('xxxx');
}
}

class Dog extends Animal {
name: string;
age: number;
constructor(species: string, foods: string[], name: string, age: number) {
super(species, foods)
this.name = name;
this.age = age
}
sayHello() {
console.log('汪汪汪');
}
}
class Cat extends Animal {
name: string;
age: number;
constructor (species: string, foods: string[] ,name: string, age: number) {
super(species, foods)
this.name = name;
this.age = age
}
sayHello() {
console.log('喵喵喵');
}
}
const dog = new Dog('狗', ['骨头', '肉'], '小黑', 5)
const cat = new Cat('猫', ['鱼', '肉'], '小白', 2)
console.log(dog.species);
console.log(dog.foods);
console.log(dog.name);
console.log(dog.sayHello());
console.log(cat.species);
console.log(cat.foods);
console.log(cat.name);
console.log(cat.sayHello());
```

抽象类`abstract`，只能被继承，不能创造实例。

抽象方法只能在抽象类中，且被继承时，需要重写。

```typescript
abstract class Animal {
  species: string;
  foods: string[];
  constructor (species: string, foods: string[]) {
    this.species = species;
    this.foods = foods;
  }
  // 抽象方法只能在抽象类中，能定义方法结构，但不能定义具体内容，需要继承体重写
  abstract sayHello (): void;
}

class Dog extends Animal {
  name: string;
  age: number;
  constructor(species: string, foods: string[], name: string, age: number) {
    super(species, foods)
    this.name = name;
    this.age = age
  }
  sayHello() {
    console.log('汪汪汪');
  }
}
```

### 5.4 interface

Interface defines the types, properties, and methods that an object should have.

```
interface InterfaceName {
  property1: type;
  property2: type;
  method1(): returnType;
  // ... more properties and methods
}
```

```typescript
// 接口用来定义类结构
  interface MyPerson {
    name: string;
    age: string;
  }
  interface MyPerson {
    sayHi(): void;
  }
  class person implements MyPerson {
    name: string;
    age: string;
    constructor(name: string, age: string) {
      this.name = name;
      this.age = age;
    }
    sayHi(): void {
      console.log(`你好，我是${this.name}，今年${this.age}岁`);
    }
  }
```

### 5.5 属性的封装

`public` | `private`(只能在本类中访问) | `protected`(只能在类中访问，不能在实例对象中访问)

修改`private`属性通过`get()`和`set()`方法。

```typescript
// 属性的封装
class Person {
  public name: string;
  private age: number;
  protected money: string;
  constructor(name: string, age: number, money: string) {
    this.name = name;
    // 私有属性的修改通过get和set方法实现
    this.age = age; // 私有属性只能在本类中访问
    this.money = money;
  }
  sayHi () {
    console.log('Hi~');
  }
  getAge() {
    return this.age;
  }
  setAge(age: number) {
    if (age >= 0 && age <= 120)
      this.age = age;
  }
}
const p1 = new Person('小明', 18, '10000');
console.log(p1.getAge());
console.log(p1.setAge(20));
console.log(p1.name);
// console.log(p1.age); // 报错，只能在本类中访问
// console.log(p1.money); // 报错，只能在本类中访问
class Woman extends Person {
  constructor( public name: string) {
    super(name, 20, '50000');
  }
}
```

`public`加在构造函数的传入参数中可简写。

```typescript
class Dog {
    constructor(public name: string, public age: number) {}
  }
```

## 6 泛型

泛型（Generic）是一种在定义函数、类或接口时不预先指定具体类型，而在使用时再指定具体类型的工具。

```typescript
function identity<T>(value: T): T {
  return value;
}

// 使用泛型函数
identity<number>(42); // 指定类型为 number
identity<string>("Hello, Generic!"); // 指定类型为 string
identity(true); // 类型推断为 boolean
```

## 7 练习贪吃蛇项目

### 7.1 项目搭建

根据复制的package.json的`devDependencies`安装包。

```
npm i
```

运行项目。

```
npm run build
```

继续安装本项目需要的包。

```
npm i -D less less-loader css-loader style-loader postcss postcss-loader postcss-preset-env
```

webpck.config.js中配置。

```js
// 设置less文件的处理
      {
        test: /\.less$/,
        use: [
          // 执行顺序，从下往上
          "style-loader",
          "css-loader",
          // 引入postcss-loader，用于处理css兼容性问题
          {
            loader: "postcss-loader",
            options: {
              postcssOptions: {
                plugins: [
                  require("postcss-preset-env")({
                    browsers: "last 2 versions",
                  }),
                ],
              },
            },
          },
          "less-loader",
        ],
      },
```