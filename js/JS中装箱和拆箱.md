# JS 中装箱和拆箱

## 概览

我们可以在基本类型值（比如字符串 "Hello"）上访问属性 & 调用方法，但基本类型值并不支持属性和方法设置。能这样做的原因，是因为**引擎内部做了装箱与拆箱操作**。

![2020-09-24-11-45-34](http://qn.cawsct.com/2020-09-24-11-45-34.png)

> JavaScript 中值的类型有两种：基本类性值（primitives）和复杂类性值（即对象，objects）。

两种类性值本质上不同的地方：

1. 基本类型: 存储的是一个简单值。
2. 对象：则使用引用访问，我们通过引用里记录的地址，找到对象数据在内存中实际存储的位置，并进行操作。

**调用方法和返回属性是对象上所特有的访问数据的方式。**

```javascript
let num = new Number(1234.236);

// 访问属性
typeof num.toLocaleString; // "function"
// 调用方法
num.toLocaleString(); // "1,234.236"
```

但会发现，直接在 1234.236 上进行同样是 OK 的。

![2020-09-24-11-48-10](http://qn.cawsct.com/2020-09-24-11-48-10.png)

原始类性值居然也有属性和方法？其实 JS 引擎背地里做了操作：**装箱和拆箱**。

### 装箱和拆箱

装箱就是把原始类型值转为对应的包装对象，比如：

```javascript
let num = 1234.236;
// 数值包装成 Number 对象
new Number(num);

let str = "Hello";
// 字符串包装成 String 对象
new String(str);
```

num 转为 new Number(num)，str 转为 new String(str) 等。当然，这个过程对我们来说是不可见的。也正是因为 JS 内部做了如此的转换，我们才得以能够“看起来可以直接在原始类型值访问属性和调用方法”。

装箱使用完毕后，还有一个**拆箱**的过程。所谓拆箱，就是把包装对象转为对应的原始类型值表现形式。

```javascript
// 将 new Number 拆箱成 1234.236
new Number(num).valueOf(); // 1234.236

// 将 new String 拆箱成 Hello
new String(num).valueOf();
```

也就是说下面代码的执行过程：

![2020-09-24-11-52-39](http://qn.cawsct.com/2020-09-24-11-52-39.png)

做了类似于如下的处理：

```javascript
// 装箱：转换成对应的包装对象
num = new Number(1234.236);

// 使用
typeof num.toLocaleString; // "function"
num.toLocaleString(); // "1,234.236"

// 拆箱
num = num.valueOf();
```

### 好处

之所以这么做，肯定是因为这种处理方式利大于弊，可以简单总结为：

- 方便。操作基本类性值的场景还是比较多的。如果每次为了使用属性或者调用方法之前都要包装一次，未免太过麻烦。
- 省内存。大家知道存储同一个数据（比如 new Number(1) 和 1 ），对象对内存的开销比基本类型值要大。有了拆装箱的操作，我们只在使用的时候暂时包装成对象访问，其余时间还是以基本类型值的形式存在，能够节省不少的内存。
- 另外，我们其实不用担心这种方式的开销问题。因为 JS 自打出生就支持这个拆装箱特性了，内部也对这类操作做了优化，所以性能问题几乎可以忽略。

### null/undefined 没有对应的包装函数

根据最新的语言标准，这 7 种语言类型是：1. Undefined；2. Null；3. Boolean；4. String；5. Number；6. Symbol；7. Object。

除了 null 和 undefined 之外，其他基本类型都要对应的包装函数。因此在 null 和 undefined 上访问属性是会报错的。

**undefined 表示变量未初始化，null 表示变量为空。两者都没有什么实际的数据意义，因此没有对应的包装函数。**

```javascript
alert(null.test); // error
```
