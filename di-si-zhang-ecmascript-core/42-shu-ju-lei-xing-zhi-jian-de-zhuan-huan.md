# 第2节 数据类型之间的转换

有3个函数可以把非数值转换为数值：Num-ber\(\)、parseInt\(\)和parseFloat\(\)。第一个函数，即转型函数Number\(\)可以用于任何数据类型，而另两个函数则专门用于把字符串转换成数值。这3个函数对于同样的输入会有返回不同的结果。

## 一、转换为数值

### 1.1 Number\(\)

Number\(\)函数的转换规则如下。

* 如果是Boolean值，true和false将分别被转换为1和0。
* 如果是数字值，只是简单的传入和返回。
* 如果是null值，返回0。
* 如果是undefined，返回NaN。
* 如果是字符串，遵循下列规则：
  * 如果字符串中只包含数字（包括前面带加号或头号的情况），则将其转换为十进制数值，即"1"会变成1，"123"会变成123，而"011"会变成11（注意：前导的零被忽略了）；
  * 如果字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
  * 如果字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的**十进制**整数值；
  * 如果字符串是空的（不包含任何字符），则将其转换为0；
  * 如果字符串中包含除上述格式之外的字符，则将其转换为NaN。
* 如果是对象，则调用对象的valueOf\(\)方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString\(\)方法，然后再次依照前面的规则转换返回的字符串值。

例如：

```js
//NaN
var num1 = Number("Hello world!");
//0
var num2 = Number("");
//11
var num3 = Number("000011");
//1
var num4 = Number(true);
```

一元加操作符的操作与Number\(\)函数相同。

### 1.2 parseInt\(\)

parseInt\(\)函数在转换字符串时，更多的是看其是否符合数值模式。

**转换规则**

* 它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt\(\)就会返回NaN；也就是说，用parseInt\(\)转换空字符串会返回NaN（Number\(\)对空字符返回0）。
* 如果第一个字符是数字字符，parseInt\(\)会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非数字字符。例如，"1234blue"会被转换为1234，因为"blue"会被完全忽略。类似地，"22.5"会被转换为22，因为小数点并不是有效的数字字符。
* 如果字符串以"0x"开头且后跟数字字符，就会将其当作一个十六进制整数；如果字符串以"0"开头且后跟数字字符，则会将其当作一个八进制数来解析。

为了更好地理解parseInt\(\)函数的转换规则，下面给出一些例子：

```js
// 1234
var num1 = parseInt("1234blue");
// NaN
var num2 = parseInt("");
// 10（十六进制数）
var num3 = parseInt("0xA");
// 22
var num4 = parseInt(22.5);
// 56（八进制数）
var num5 = parseInt("070");
// 70（十进制数）
var num6 = parseInt("70");
// 15（十六进制数）
var num7 = parseInt("0xf");
```

**转换基数**

在使用parseInt\(\)解析像八进制字面量的字符串时，ECMAScript 3和5存在分歧。

在ECMAScript 3 JavaScript引擎中，"070"被当成八进制字面量，因此转换后的值是十进制的56。而在ECMAScript 5 JavaScript引擎中，par-seInt\(\)已经不具有解析八进制值的能力，因此前导的零会被认为无效，从而将这个值当成"0"，结果就得到十进制的0。在ECMAScript 5中，即使是在严格模式下也会如此。

为了消除在使用parseInt\(\)函数时可能导致的上述困惑，可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）。

如果知道要解析的值是十六进制格式的字符串，那么指定基数16作为第二个参数，可以保证得到正确的结果

```js
//175
var num = parseInt("0xAF", 16);
```

如果指定了16作为第二个参数，字符串可以不带前面的"0x"。

指定基数会影响到转换的输出结果。例如：

```js
//2 （按二进制解析）
var num1 = parseInt("10", 2);
//8 （按八进制解析）
var num2 = parseInt("10", 8);
//10（按十进制解析）
var num3 = parseInt("10", 10);
//16（按十六进制解析）
var num4 = parseInt("10", 16);
```

多数情况下，我们要解析的都是十进制数值，**因此始终将10作为第二个参数是非常必要的**。

### 1.3 parseFloat\(\)

parseFloat\(\)也是从第一个字符（位置0）开始解析每个字符。而且也是一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。

转换规则：

* 字符串中的第一个小数点是有效的，而第二个小数点就是无效的了，因此它后面的字符串将被忽略。比如：，"22.34.5"将会被转换为22.34。
* parseFloat\(\)与parseInt\(\)的第二个区别在于它始终都会忽略前导的零。
* 如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后都是零），parseFloat\(\)会返回整数。

以下是使用parseFloat\(\)转换数值的几个典型示例。

```js
//1234 （整数）
var num1 = parseFloat("1234blue");
//0
var num2 = parseFloat("0xA");
//22.5
var num3 = parseFloat("22.5");
//22.34
var num4 = parseFloat("22.34.5");
//908.5
var num5 = parseFloat("0908.5");
//31250000
var num6 = parseFloat("3.125e7");
```

## 二、转换为字符串

要把一个值转换为一个字符串有两种方式。

### 2.1 toString\(\)

几乎每个值都有的toString\(\)方法，这个方法唯一要做的就是返回相应值的字符串表现。

```js
var age = 11;
// 字符串"11"
var ageAsString = age.toString();
var found = true;
// 字符串"true"
var foundAsString = found.toString();
```

但null和undefined值没有toString方法。

在调用数值的toString\(\)方法时，可以传递一个参数：输出数值的基数。默认情况下，toString\(\)方法以十进制格式返回数值的字符串表示。而通过传递基数，toString\(\)可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。

```js
var num = 10;
// "10"
alert(num.toString());
// "1010"
alert(num.toString(2));
// "12"
alert(num.toString(8));
// "10"
alert(num.toString(10));
// "a"
alert(num.toString(16));
```

### 2.2 toLocaleString\(\)

该方法返回符合本地区规范的字符串表现形式，这个方法的一个典型的应用就是把数字转换成货币表现形式

```js
var num = 9800768800
num.toLocaleString() // "9,800,768,800"
```

### 2.3 String\(\)

在不知道要转换的值是不是null或unde-fined的情况下，还可以使用转型函数String\(\)，这个函数能够将任何类型的值转换为字符串。

转换规则如下：

* 如果值有toString\(\)方法，则调用该方法（没有参数）并返回相应的结果；
* 如果值是null，则返回"null"；
* 如果值是undefined，则返回"undefined"。

**toString\(\)与String\(\)的区别就在于String\(\)还能转换null和undefined值**，可以说是toString\(\)的增强版。在开发中直接使用String\(\)似乎更好，这样能避免潜在的转换风险。

## 三、转换为Boolean

Boolean对象除了可以作为构造函数，还可以单独使用，将任意值转为布尔值。这时Boolean就是一个单纯的工具方法。

```js
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean('') // false
Boolean(NaN) // false

Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function () {}) // true
Boolean(/foo/) // true
```

上面代码中几种得到true的情况，都值得认真记住。

使用双重的否运算符（!）也可以将任意值转为对应的布尔值。

```js
!!undefined // false
!!null // false
!!0 // false
!!'' // false
!!NaN // false
!!1 // true
!!'false' // true
!![] // true
!!{} // true
!!function(){} // true
!!/foo/ // true
```

最后，对于一些特殊值，Boolean对象前面加不加new，会得到完全相反的结果，必须小心。

```js
if (Boolean(false)) {
  console.log('true');
} // 无输出

if (new Boolean(false)) {
  console.log('true');
} // true

if (Boolean(null)) {
  console.log('true');
} // 无输出

if (new Boolean(null)) {
  console.log('true');
} // true
```

---

**参考资料**

* JavaScript高级程序设计，【美】Nicholas C. Zakas
* [JavaScript标准参考教程 - 包装对象](http://javascript.ruanyifeng.com/stdlib/wrapper.html#toc1)



