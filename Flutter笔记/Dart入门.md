# Dart 入门 —— 变量常量与内置类型

> [Dart](http://dart.goodev.org/) 可用于开发 flutter， 初期目标成为下一代 web 开发语言，目前已经用于全平台开发。而且它也是面向对象的编程语言，和 Java/Typescript/Javascript 很像，从语法上看是两者的结合。从事前端开发的童鞋肯定很开心了。

## 应用场景

1. web 开发
2. 跨平台移动应用开发（flutter）
3. 脚本或服务端开发

## 变量与常量

### 1.变量

#### 使用 var 声明变量

```dart
var name = 'apple';
var name;
```

前端开发肯定很熟悉这个了。使用 var 可以给变量设置不同的值。或者不设置值的话，默认为 null。

但是跟 JavaScript 不一样的是，第一次赋值的时候，如果确定是字符串类型后面就不能改为别的类型了。JavaScript 是可以重新赋值的。

因为 Dart 是强类型语言，Js 是弱类型语言。

> 如果真的想改变 Dart 的类型，可以使用 dynamic 关键字。

### 2. 常量

#### 使用 const/final 进行修饰

> 在 es6 里只有 const 声明常量，但是在 dart 里有常用两个可以修饰常量的关键字：const/final。两者有什么区别？

##### Final

Flutter 官方教程中，有这么一行代码： `final wordPair = WordPair.random();`

Final 表明这个变量不能再发生更改，但是这个初始化的值在编译时是不确定的， 只有在运行时，才能确定其值。一旦初始化，则不允许再次发生更改。

例如：

- HTTP 接口的返回
- 三方库的随机数据

**如果 final 定义的是个 collection，其子元素不需要是 final 的。**

##### const

const 定义时，需要是个明确的值，不能像 final 那样，运行时才知道是什么值。

例如，`const city = '广州'`

**如果 const 定义的是个 collection，其子元素也需要是 const 的。**

### 3. 内置类型

与 Javascript 不一样，Dart 内置了七种数据类型：**Number/String/Boolean/List/Map/Runes/Symbols**

#### Number（数值型）

数值型分为：整型（int）和浮点型（double），可以用 int/double/num 声明变量。

变量声明用 num 声明，首次赋值的是 int 型，后面可以改为 double 型。

如果用 int 声明，后面则不能改为 double 型。

如果用 double 声明 int 类型的值，int 会自动打印成 double 型。

```dart
num a = 1
a = 2.3     √

int b = 2
b = 2.1     ×


double c = 2
b           // 2.0
```

关于数值型的操作，常用的有三中类型，跟 JavaScript 使用很相似：

1. 方法： floor()/ceil()/round()/abs()/toDouble()/toInt()等。
2. 属性：isNaN/isEven/isOdd
3. 运算符：+ - \* / %

```dart
num a = 10
a.isOdd

var b = 1.23
b.floor()
b.toInt()
```

#### String（字符串）

创建字符串方式有：

- 字符串：单引号（''） / 双引号（""）
- 多行字符串：三个引号（'''）
- 原始 raw 字符串： r 创建

```dart
var str1 = 'name'

String str2 = "name"

var str2 = '''前端
        好给力'''

str1.length
str2.contains(str1)
```

在 ES6 我们我们使用`${str1}`模板字符串,而 dart，也有类似的插值表达式`$expression`。

```dart
var str1 = 'name'
print('姓名的英文是$str1')

```

#### Boolean（布尔值）

- 表示布尔值`bool`

  ```dart
  bool a = false
  ```

- 在 debug 模式下，通过 assert 断言函数判断布尔值。为 false 则终止运行，开始时有用。

```dart
var a = ''
assert(a.isEmpty)
```

#### List（数组）

表示一个数组，可以用 List。List 表示集合。

```dart
var list = [1,2]

// 构造方式创建list
var list2 = new List()

// 不可变的list
var list3 = const[1,2,3]

// 通过索引的方式获取数组里的内容
list[0]  // 1

// List3是不可变，会报错。
list3[0] = 3
```

List 的方法跟 Javascript 有点不一样，比 Javascript 方便了一些，不需要自己写。

```dart
var a = [1,2,3]
list.shuffle()      // 随机打乱list元素的顺序
list.add(4)         // 新增一个元素
list.remove(2)      //移除一个元素
list.insert(2, 5)   // 在指定位置插入元素
list.indexOf(2)     // 获取元素的位置
```

#### Map（键值对）

这个和 ES6 的用法差不多，在 Dart 中，Map 以 key-value 存储

- 声明方式有三种：

```dart
// 创建一个Map
Map person = {'name': 'jason', 'company': 'wbiao'}

// 创建不可变的Map
Map person = const{'name': 'jason', 'company': 'wbiao'}

// 构造方式声明

Map person = new Map()
person['name'] = 'jason'
person['company'] = 'wbiao'

```

- 常用的方法：

```dart
game['name'] = 'boy'    //修改元素的内容
game.remove('name')     //移除元素
game.clear()            //清空Map
```

#### dynamic

在 Dart 里，**一切皆为对象**，以上所有内置类型的父类都是 Object。

我们除了使用`String a = 123`这么定义的话，还可以通过 dynamic/Object 来定义。

```dart
Object a = 123
dynamic a = 123
```

但是**官方不推荐我们这么做**。我们最好确定好一个类型，加快编译和安全，而且还可以做类型检测。这和 typescript 的 any 的道理是一样的。

#### 类型检测

运行时进行类型检测，可以用 as 和 is 关键字。

- as: 判断属于某种类型
- is: 如果对象有指定的类型，则 true

```dart
 var val = "jasonho is working in wbiao";
  if (val is String) {
    print("good");
  }

  var isString = val as String
```
