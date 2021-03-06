#第三章 基本概念

##3.1 语法

###3.1.1 区分大小写

###3.1.2 标识符

- 标识符就是指变量、函数、属性的名字或者函数的参数
- 第一个字符必须是一个字母、下划线、美元符号或数字
- 其他字符可以是字符、下划线、美元符号或数字
- 标识符中的字母也可以包含扩展的ASCII或Unicode字母字符，但不推荐使用
- 驼峰写法
- 关键字、保留字、true、false和null不能用作标识符

###3.1.3 注释

``` js
//单行注释
/*
 * 这是一个多行
 * （块级）注释
 */
```

###3.1.4 严格模式

- 如果要在整个脚本中启用严格模式，可以在顶部添加：“use strict";
- 也可以对指定函数使用严格模式

``` js
function doSomething(){
  "use strict";
  //函数体
}
```

###3.1.5 语句

- 推荐给语句结尾都加上分号
- 可以使用C风格的语法把多条语句组合到一个代码块中，用{}括起来
- 条件控制语句（如if），即使只有一条语句，也推荐同花括号括起来

##3.2 关键字和保留字

ECMA-262 描述了一组具有特定用途的关键字

**关键字**

> break do instanceof typeof case else new var catch finally return void continue for switch while debugger* function this with default if throw delete in try 


ECMA-262 还描述了另外一组不能用作标识符的保留字

**保留字**

> abstract enum int short boolean export interface static byte extends long super char final native synchronized class float package throws const goto private transient debugger implements protected volatile double import public


第 5 版把在非严格模式下运行时的保留字缩减为下列这些:

> class enum extends super const export import 


在严格模式下，第 5 版还对以下保留字施加了限制：

> implements package public interface private static let protected yield 

ECMA-262 第 5 版对 eval 和 arguments 还施加了限制。在严格模式下，这两个名字也不能作为标识符或属性名，否则会抛出错误。

##3.3 变量

``` js
var message1; //未经初始化的变量，会保存一个特殊的值——undefined
var message2 = 'hi'; //直接初始化变量
message2 = 100; //有效，但不推荐

function test1(){
    var message3 = 'hi'; //局部变量，用var操作符定义的变量成为定义该变量作用域中的局部变量
}
test1(); //函数调用时，会创建该变量并赋值；之后变量会被立即销毁
alert(message3); //错误！

function test2(){
    message4 = 'hi'; //全局变量，没有var
}
test2();
alert(message4); //'hi'
```

``` js
//一条语句定义多个变量，推荐缩进格式
var message = 'hi',
    found = false,
    age = 29;
```

##3.4 数据类型

ECMAScript中有五种简单数据类型：Undefined、Null、Boolean、Number、String；一种复杂数据类型：Object。Object本质上是由一组无序的键值对组成的。

###3.4.1 typeof操作符

typeof操作符返回的永远都是字符串：

- "undefined" —— 如果这个值未定义
- "boolean" ——如果这个值是布尔值
- "string" ——如果这个值是字符串
- ”number" ——如果这个值是数值
- "object" ——如果这个值是对象或null
- "function" ——如果这个值是函数

``` js
var message = "some string"; 
alert(typeof message);     // "string"  // 注意 typeof 返回的都是字符串类型
alert(typeof(message));    // "string"  // typeof 操作变量
alert(typeof 95);          // "number"  // typeof 操作数值字面量
// typeof 是一个操作符而不是一个函数，因此例子中的圆括号尽管可以使用，但不是必需的。

var a = function(){alert(1)}
alert(typeof a); //"function" //虽然函数也是一种对象，但是函数尤其特殊性，因此用typeof将function从object中区分出来是有必要的。
```

> typeof null 会返回 "object"，因为 null 被认为是一个空的对象引用。
> typeof 正则表达式 在大多数情况下返回 "object"，但是在Safari 5及之前版本、Chrome 7及之前版本会返回 "function"

###3.4.2 Undefined 类型

Undefined类型只有一个值：undefined。在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined。

``` js
var message;
alert(message == undefined); //true
```

上述例子等价于下面的例子：

``` js
var message = undefined; //单单写 var message 就相当于将undefined赋给了这个变量
alert(message == undefined); //true
```

注意，undefined有两个条件：1、用var声明了；2、未对其初始化

``` js
var message; //变量声明后默认取得undefined值

//var age //该变量并没有被声明，因为在注释里
alert(message); //"undefined"
alert(age); //产生错误
```

对于尚未声明过的变量，只能执行一项操作，就是使用typeof操作符检测其数据类型。如果变量未声明，用typeof来操作，也会返回"undefined"。对未声明的变量执行typeof操作符也同样返回"undefined"

``` js
var message; // 声明但未初始化，默认获得 undefined 值
//var age //未声明的变量

alert(typeof message); //"undefined"
alert(typeof age); //"undefined"
```

> 即使未初始化的变量会自动被赋予undefined值，但显式地初始化变量依然是明智的选择，如果能做到这一点，那么当typeof操作符返回"undefined"值时，我们就知道被检测的变量还没有被声明，而不是尚未初始化。

###3.4.3 Null类型

Null类型也只有一个值null。null值表示一个空对象指针，因此 typeof null返回的是"object"。

``` js
var car = null;
alert(typeof null); //"object"
```

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null 而不是其他值。这样，只要直接检查 null 值就可以知道相应的变量是否已经保存了一个对象的引用，如：

``` js
if (car != null) {
    //对 car 对象执行某些操作
}
```

> 实际上，undefined 值是派生自 null 值的，因此ECMA-262规定对它们的相等性测试要返回true，如：

``` js
alert(null == undefined); //true
```

无论什么情况下，都没有必要把一个变量的值显示设置为 undefined，但是只要意在保存对象的变量还没有真正保存对象，就应该明确让该变量保存 null 值。这样不仅可以体现 null 作为空对象指针的惯例，也有助于进一步区分 null 和 undefined 。

###3.4.4 Boolean类型

该类型有两个字面值：true 和 false。注意这两个字面值是区分大小写的。

ECMAScript中所有类型的值都有与这两个Boolean值等价的值，可以通过调用转型函数Boolean()来实现：

``` js
var message = "Hello world!";
var messageAsBoolean = Boolean(message);
```

数据类型 | 转换为 true 的值 | 转换为 false 的值
:--|:--|:--
Boolean | true | false
String | 任何非空字符串 | "" （空字符串）
Number | 任何非零数字值（包括无穷大） | 0和NaN
Object | 任何对象 | null
Undefined | n/a | undefined

这些转换规则对于理解流控制语句（如 if 语句）自动执行相应的 Boolean 转换非常重要，如：

``` js
var message = "Hello world!";
if(message){
    alert("Value is true.");
}
//运行上述代码会弹出警告框，因为字符串message被自动转换成了对应的Boolean值(true)。
```

###3.4.5 Number类型

这种类型使用IEEE754格式来表示整数和浮点数值。

- 十进制整数 `var intNum = 55; //整数`
- 八进制 `var octalNum1 = 070; //八进制的56`
    - 八进制字面量的第一位必须是零
    - 八进制字面量中的数字是 0~7，超出该范围无效
        - `var octalNum2 = 079; //无效八进制——解析为79`
        - `var octalNum3 = 08; //无效的八进制——解析为8`
    - 八进制字面量在严格模式下无效，会导致js引擎抛出错误
- 十六进制 `var hexNum1 = 0xA; //十六进制的10` 
    - `var hexNum2 = 0x1f; //十六进制的31`
    - 十六进制字面值的前两位必须是0x
    - 后面跟任何十六进制数字0~9、A~F，其中字母大小写皆可
- 在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。

####1. 浮点数值

数值中必须包含一个小数点，且小数点后面必须至少有一位数字。

``` js
var floatNum1 = 1.1;
var floatNum2 = 0.1;
var float Num3 = .1; //有效，但不推荐
```

由于浮点数保存需要的内存空间是保存整数值的两倍，因此ECMAScript会不失时机地将浮点数值转换成整数值：

``` js
var floatNum1 = 1.; //小数点后面没有数字——解析为1
var floatNum2 = 10.0 //整数——解析为10
```

对于极大或极小的数值，用e表示法（科学计数法）：

``` js
var floatNum1 = 3.125e7; //就相当于是 3.123 × 10 ^7 即 31250000
var floatNum2 = 3e-7; //就是0.0000003
```

浮点数值的最高精度是17位小数，进行算术计算其精确度远不如整数，如：0.1加0.2的结果不是0.3，而是0.30000000000000004。

``` js
if (a + b == 0.3) { //不要做这样的测试！
    alert("You got 0.3."); 
}
//如果a、b两个数是0.05和0.25或者0.15和0.15都不会有问题，但是如果两个数是0.1和0.2，那么测试将无法通过。
```

####2. 数值范围

ECMAScript能够表示的最小数值保存在 Number.MIN_VALUE中，大多数浏览器中这个值为 5e-324，小于这个值就会被自动转换成 -Infinity（负无穷）；最大的值保存在 Number.MAX_VALUE中，大多数浏览器中这个值为 1.7976931348623157e+308，大于它会被转成 Infinity（正无穷）。

如果某次计算反回了正或负的 Infinity 值，那么该值将无法继续参与下一次的计算。可以用 isFinite() 函数确定一个数值是否是有穷的：

``` js
var result = Number.MAX_VALUE + Number.MAX_VALUE;
alert(isFinite(result)); //false
```

> 访问Number.NEGATIVE_INFINITY和Number.POSITIVE_INFINITY也可以得到负和正Infinity的值。可以想见，这两个属性中分别保存着 -Infinity 和 Infinity 。

####3. NaN

NaN，即非数值（Not a Number）是一个特殊的数值，用于表示一个本来要返回数值的操作数未返回数值的情况。例如，在ECMAScript中，0除以0会返回NaN，不会抛出错误，影响其他代码的执行。（整数除以0得到Infinity；负数除以0得到 -Infinity）

NaN的两个特点：

1. 任何涉及NaN的操作（例如NaN/10）都会返回NaN。
2. NaN与任何值都不相等，包括NaN本身。例如：`alert(NaN == NaN); //false`

ECMAScript定义了isNaN()函数，来确定参数是否“不是数值”。任何不能被转换为数值的值都会导致这个函数返回true，例如：

``` js
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false (10是一个数值)
alert(isNaN("10")); // false (可以被转换成数值10)
alert(isNaN("blue")); //true (不能转换成数值)
alert(isNaN(true)); //false (可以被转换成数值1)
```

> isNaN()也适用于对象，它会首先调用对象的valueOf()方法，然后确定该方法返回的值是否可以转换为数值。如果不能，则给予这个返回值再调用toString()方法，再测试返回值。

####数值转换

- Number() 函数，可用于任何数据类型
- parseInt() 函数，专门用于把字符串转换成数值
- parseFloat() 函数，专门用于把字符串转换成数值

**转型函数Number() 的转换规则**

- 如果是Boolean值，true和false将分别转换为1和0
- 如果是数字值，知识简单的传入和返回
- 如果是null值，返回0
- 如果是undefined，返回NaN。
- 如果是字符串，则
    - 如果字符串只包含数字（包含前面带正、负号的情况），则将其转换为十进制数值，即"1"会变成1；"123"会变成123，而"011"会变成11（注意，前导的零被忽略）
    - 如果字符串中包含有效的浮点格式，如"1.1"，则将会转换为对应的浮点数值（前导零也会被忽略）
    - 如果字符串中包含有效的十六进制格式，例如"0xf"，则转换为相同大小的十进制整数值
    - 如果字符串为空（不包含任何字符），则转换为0
    - 如果字符串中包含上述格式之外的字符，则转换为NaN
- 如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再次依照前面的规则转换返回的字符串值。

``` js
var num1 = Number("Hello world!"); //NaN
var num2 = number(""); //0
var num3 = Number("000011"); //11
var num4 = Number(true); //1
```

**parseInt() 函数**

- 它会忽略字符串前面的空格，直至找到第一个非空格字符串
- 如果第一个字符不是数字字符或者负号，则返回NaN，例如`alert(parseInt("hi123")); //NaN`
- 转换空字符串，会返回NaN：`alert(parseInt("")); //NaN`
- 如果第一个字符是数字字符，则会继续解析第二个字符，直至解析完所有后续字符或者遇到了一个非数字字符。例如：`alert(parseInt("1234blue")); //1234`。又如：`alert(parseInt("22.5")); //22 因为小数点不是有效的数字字符`
- 也能够识别出各种整数格式，如果字符串以"0x"开头且后跟数字字符，就会当作一个十六进制整数；如果字符串以"0"开头且后跟数字字符，则会当作八进制数来解析。

``` js
var num1 = parseInt("1234blue"); //1234
var num2 = parseInt(""); //NaN
var num3 = parseInt("0xA"); //10 （十六进制数）
var num4 = parseInt(22.5); //22
var num5 = parseInt("070"); //56 （八进制数）
var num6 = parseInt("70"); //70 （十进制数）
var num7 = parseInt("0xf"); //15 （十六进制数）
```

> ECMAScript 3 JS引擎与5的JS引擎有区别。`var num = parseInt("070");` 在3中被认为是56（八进制）；在5中认为是70（十进制）

parseInt()函数的第二个参数：转换时使用的基数（即多少进制）：

``` js
var num = parseInt("0xAF", 16); //175
var num1 = parseInt("AF", 16); //175 指定了进制之后，字符串前面可以不带"0x"
var num2 = parseInt("AF"); //NaN
```

指定基数会影响到转换的输出结果，如：

``` js
var num1 = parseInt("10", 2); //2 （按二进制解析）
var num2 = parseInt("10", 8); //8 （按八进制解析）
var num3 = parseInt("10", 10); //10 （按十进制解析）
var num4 = parseInt("10", 16); //16 （按十六进制解析）
```

**parseFloat()函数**

- 从第一个字符（位置0）开始解析每个字符，一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。（字符串中第一个小数点是有效的，第二个小数点是无效的），如：`parseFloat("22.34.5"); //22.34`
- parseFloat()之中会忽略前导的零，它只解析十进制值，它没有第二个参数指定进制。十六进制的字符串始终解析为0，如`parseFloat("0xA"); //0`
- 如果字符串包含的是一个课解析为整数的数（没有小数点，或者小数点后都是零），那么parseFloat()会返回整数

``` js
var num1 = parseFloat("1234blue"); //1234 （整数）
var num2 = parseFloat("0xA"); //0
var num3 = parseFloat("22.5"); //22.5
var num4 = parseFloat("22.34.5"); //22.34
var num5 = parseFloat("0908.5"); //908.5
var num6 = parseFloat("3.125e7"); //31250000
```

###3.4.6 String类型

String类型用于表示由零或多个16位Unicode字符组成的字符序列，即字符串。可由双引号（"）或单引号（'）表示。用双引号或单引号表示的字符串完全相同。不过以双引号开头的字符串必须以双引号结尾；以单引号开头的字符串必须以单引号结尾，即左右引号必须匹配。

####1. 字符字面量

字面量 | 含义
:--|:--
\n | 换行
\t | 制表
\b | 空格
\r | 回车
\f | 进纸
\\ | 斜杠
\' | 单引号（'），在用单引号表示的字符串中使用，例如：`'He said, \'hey.\''`
\" | 双引号（"），在用双引号表示的字符串中使用，例如：`"He said, \"hey.\""`
\xnn | 以十六进制代码nn表示的一个字符（其中n为0~F）。例如，\x41表示"A"
\unnnn | 以十六进制代码nnnn表示一个Unicode字符（其中n为0~F）。例如，\u03a3表示希腊字符∑

以上这些字符字面量可以出现在字符串中的任意位置，而且也将被作为一个字符来解析。如：

``` js
var text = "This is the letter sigma: \u03a3.";
alert(text.length); //输出28
```

####2. 字符串的特点

ECMAScript中的字符串是不可变的，即字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首相要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。例如：

``` js
var lang = "Java";
lang = lang + "Script";
```

以上示例的实现过程如下：先创建一个能容纳10个字符的新字符串，然后在这个字符串中填空“Java”和“Script”，最后一步是销毁原来的字符串“Java”和字符串“Script”。

####3. 转换为字符串

**方法一：toString()方法**

- 数值、布尔值、对象和字符串值（每个字符串也都有一个toString()方法，该方法返回字符串的一个副本）都有toString()方法
- null和undefined值没有这个方法，会报错
- 在调用数值的toString()方法时，可以传递一个参数，输出数值的基数，默认情况下，toString()方法以十进制格式返回数值的字符串表示。

``` js
var age = 11;
var ageAsString = age.toString(); //字符串"11"
var found = true;
var foundAsString = found.toString(); //字符串"true"

var num = 10;
alert(num.toString()); //"10"
alert(num.toString(2)); //"1010"
alert(num.toString(8)); //"12"
alert(num.toString(10)); //10
alert(num.toString(16)); //"a"
```

**方法二：转型函数String()**

在不知道要转换的值是不是null或undefined的情况下，还可以使用String()，它可以将任何类型的值转换为字符串。规则如下：

- 如果值有toString()方法，则调用该方法（没有参数），并返回相应结果
- 如果值是null，则返回"null"
- 如果值是undefined，则返回"undefined"

``` js
var value1 = 10;
var value2 = true;
var value3 = null;
var value4;

alert(String(value1)); //"10"
alert(String(value2)); //true;
alert(String(value3)); "null"
alert(String(value4)); "undefined"
```

**方法三：要把某个值转换为字符串，可以使用加号操作符把它与一个字符串("")加在一起**

###3.4.7 Object类型

ECMAScript中的对象其实就是一组数据和功能的集合。对象可以通过执行new操作符后跟要创建的对象类型的名称来创建。而创建Object类型的实例并为其添加属性和（或）方法，就可以创建自定义对象，如：`var o = new Object();`。

如果不给构造函数传递参数，则可以省略后面的那一对圆括号，但是并不推荐这样做。如：`var o = new Object; //有效，但不推荐省略圆括号`。

在ECMAScript中，Object类型是所有它的实例的基础。换句话说，Object类型所具有的任何属性和方法也同样存在于更具体的对象中。

Object的每个实例都具有下列属性和方法：

- constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）就是Object()。
- hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定，例如：`o.hasOwnProperty("name")`。
- isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型
- propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用for-in语句来枚举。作为参数的属性名必须以字符串形式指定。
- toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。
- toString()：返回对象的字符串表示。
- valueOf()：返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同。

>从技术角度讲，ECMA-262中对象的行为不一定适用于JavaScript中的其他对象。浏览器环境中的对象，比如BOM和DOM中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262不负责定义宿主对象，因此宿主对象可能会也可能不会继承Object。

##3.5 操作符

###3.5.1 一元操作符

只能操作一个值的操作符叫做一元操作符。

####1. 递增和递减操作符

- 前置型 `var age = 29; ++age;`、`var age = 29; --age;`
    - 执行前置的递增和递减操作时，变量的值都是在语句被求值以前改变的。

``` js
var age = 29;
var anotherAge = --age +2;

alert(age); //输出29
alert(anotherAge); //输出30
```

由于前置递增和递减操作与执行语句的优先级相等，因此整个语句会从左至右被求值，又如：

``` js
var num1 = 2;
var num2 = 20;
var num3 = --num1 + num2; //等于21
var num4 = num1 + nu2; //等于21
```

- 后置型 ` var age = 29; age++;`
    - 后置型递增和递减操作是在包含它们的语句被求值之后才执行的

``` js
var num1 = 2;
var num2 = 20;
var num3 = num1-- + num2; //等于22
var num4 = num1 + num2; //等于21
```

前、后置型递增、递减操作符对于任何值都适用，也就是说它们不仅适用于整数，还可以用于字符串、布尔值、浮点数值和对象。在应用不同的值时，递增和递减操作符遵循下列规则。

- 在应用于一个包含有效数字字符的字符串时，先将其转换为数字值，再执行加减1的操作。字符串变量变成数值变量。
- 在应用于一个不包含有效数字字符的字符串时，将变量的值设置为NaN。字符串变量变成数值变量。
- 在应用于布尔值false时，先将其转换成0再执行加减1的操作。布尔值变量变成数值变量。
- 在应用于布尔值true时，先将其转换为1再执行加减1的操作。布尔值变量变成数值变量。
- 在应用于浮点数值时，执行加减1的操作。
- 在应用于对象时，先调用对象的valueOf()方法以取得一个可供操作的值，然后对该值应用前述规则。如果是NaN，则在调用toString()方法后再应用前述规则。对象变量变成数值变量。

``` js
var s1 = "2";
var s2 = "z";
var b = false;
var f = 1.1;
var o = {
    valueOf: function(){
        return -1;
    }
};

s1++; //值变成数值3
s2++; //值变成NaN
b++; //值变成数值1
f--; //值变成0.10000000000000009（由于浮点舍入错误所致）
o--; //值变成数值-2
```

####2. 一元加和减操作符

一元加操作符以一个加号（+）表示，放在数值前面，对数值不会产生任何影响。如：

``` js
var num = 25;
num = +num; //仍然是25
```

不过，对于非数值应用一元加操作符时，该操作符会像Number()转型函数一样对这个值执行转换。即：布尔值false和true将被转换为0和1，字符串值会被按照一组特殊的规则进行解析，而对象是先调用它们的valueOf()和（或）toString()方法，再转换得到的值。例如：

``` js
var s1 = "01";
var s2 = "1.1";
var s3 = "z";
var b = false;
var f = 1.1;
var o = {
    valueOf: function(){
        return -1;
    }
};

s1 = +s1; //值变成数值1
s2 = +s2; //值变成数值1.1
s3 = +s3; //值变成NaN
b = +b; //值变成数值0
f = +f; //值未变，仍然是1.1
o = +o; //值变成数值-1
```

一元减操作符主要用于表示负数，例如将1转换成-1。

``` js
var num = 25;
num = -num; //变成了-25
``` 

当一元减操作符应用于非数值时，遵循与一元加操作符相同的规则，最后再将得到的数值转换为负数，例如：

``` js
var s1 = "01";
var s2 = "1.1";
var s3 = "z";
var b = false;
var f = 1.1;
var o = {
    valueOf: function(){
        return -1;
    }
};

s1 = -s1; //值变成数值-1
s2 = -s2; //值变成数值-1.1
s3 = -s3; //值变成NaN
b = -b; //值变成数值0
f = -f; //值未变，仍然是-1.1
o = -o; //值变成数值1
```

###3.5.2 位操作符

位操作符用于在最基本的层次上，即按内存中表示数值的位来操作数值。ECMAScript中的所有数值都以IEEE-754 64位格式存储，但是位操作符并不直接操作64位的值，而是先将64位的值转换成32位的整数，然后执行操作。

对于有符号的整数，32位中的前31位用于表示整数的值。第32位用于表示数值的符号：0表示整数，1表示负数。这个表示符号的位叫做符号位。符号位的值决定了其他位数值的格式。其中，正数以纯二进制格式存储，31位中的每一位都表示2的幂。第一位（叫做位0）表示2<sup>0</sup>，第二位表示2<sup>1</sup>以此类推。没有用到的位以0填充，即忽略不计。例如，数值18的二进制表示是00000000000000000000000000010010，或者更简洁的10010。（2<sup>4</sup> * 1 + 2<sup>3</sup> * 0 + 2<sup>2</sup> * 0 + 2<sup>1</sup> * 1 + 2<sup>0</sup> * 0 = 18）

负数同样以二进制码存储，但使用的格式是**二进制补码**。计算一个数值的二进制补码，需要经过下列3个步骤：

1. 求这个数值绝对值的二进制码（例如，要求-18的二进制补码，先求18的二进制码）
2. 求二进制反码，即将0替换为1，将1替换为0
3. 得到的二进制反码加1

例如要求-18的二进制码：

1. 18的二进制码是：0000 0000 0000 0000 0000 0000 0001 0010
2. 求二进制反码，0、1互换：1111 1111 1111 1111 1111 1111 1110 1101
3. 二进制反码加1：1111 1111 1111 1111 1111 1111 1110 1110

> 注意：在处理有符号整数时，是不能访问位31的。

ECMAScript在以二进制字符串形式输出一个负数时，我们只是看到了这个负数绝对值的二进制码前面加上了一个负号：

``` js
var num = -18;
alert(num.toString(2)); //"-10010"
```

> 默认情况下，ECMAScript中的所有整数都是有符号整数。不过，当然也存在无符号整数。对于无符号整数来说，第32位不再表示符号，因为无符号整数只能是正数。而且，无符号整数的值可以更大，因为多出的一位不再表示符号，可以用来表示数值。

在对特殊的NaN和Infinity值应用位操作时，这两个值都会被当成0来处理。

如果对非数值应用位操作符，会先使用Number()函数将该值转换为一个数值（自动完成），然后再应用位操作符。得到的结果将是一个数值。

####1. 按位非（NOT）

按位非操作符由一个波浪线（~）表示，执行按位非的结果就是返回数值的反码。按位非的操作本质：操作数的负值减1.

``` js
var num1 = 25; //二进制 00000000000000000000000000011001
var num2 = ~num1; //二进制 11111111111111111111111111100110
alert(num2); //-26
```

上述代码与下面的代码效果相同，但是上面的代码更快：

``` js
var num1 = 25;
var num2 = -num1 - 1;
alert(num2); //"-26"
```

####2. 按位与（AND）

按位与操作符由一个和号字符（&）表示，它有两个操作符数。从本质上讲，按位与操作就是将两个数值的每一位对齐，然后根据下表中的规则，对相同位置上的两个数执行AND操作：

第一个数值的位 | 第二个数值的位 | 结果
:-- | :-- | :--
1 | 1 | 1
1 | 0 | 0
0 | 1 | 0
0 | 0 | 0

简而言之：按位与操作只有在两个数值的对应位都是1时才返回1，任何一位是0，结果都是0.

``` js
var result = 25 & 3;
//25: 0000 0000 0000 0000 0000 0000 0001 1001
// 3:  0000 0000 0000 0000 0000 0000 0000 0011
alert(result); //1: 0000 0000 0000 0000 0000 0000 0000 0001
```

####3. 按位或（OR）

按位或操作符由一个竖线符号（|）表示，同样也有两个操作符，按位或操作遵循下面这个真值表：

第一个数值的位 | 第二个数值的位 | 结果
:-- | :-- | :--
1 | 1 | 1
1 | 0 | 1
0 | 1 | 1
0 | 0 | 0

简而言之，按位或操作在有一个位是1的情况下就返回1，而只有在两个位都是0的情况下才返回0.

``` js
var result = 25 | 3;
//25: 0000 0000 0000 0000 0000 0000 0001 1001
// 3:  0000 0000 0000 0000 0000 0000 0000 0011
alert(result); //27: 0000 0000 0000 0000 0000 0000 0001 1011
```

####4. 按位异或（XOR）

按位异或操作符由一个插入符号（^）表示，也有两个操作数，一下是按位异或的真值表。

第一个数值的位 | 第二个数值的位 | 结果
:-- | :-- | :--
1 | 1 | 0
1 | 0 | 1
0 | 1 | 1
0 | 0 | 0

按位异或与按位或的不同之处在于，这个操作在两个数值对应位上只有一个1时才返回1，如果对应的两位都是1或都是0，则返回0.

``` js
var result = 25 ^ 3;
//25: 0000 0000 0000 0000 0000 0000 0001 1001
// 3:  0000 0000 0000 0000 0000 0000 0000 0011
alert(result); //26: 0000 0000 0000 0000 0000 0000 0001 1010
```

####5. 左移

左移操作符由两个小于号（<<）表示，这个操作符会将数值的所有位向左移动指定的位数。例如，如果将数值2（二进制码为10）向左移动5位，结果就是64（二进制码为1000000）。

``` js
var oldValue = 2; //等于二进制的10
var newValue = oldValue << 5; //等于二进制的1000000，十进制的64
```

> 向左移位后，原数值右侧多出来的空位用0来填充，以便得到的结果是一个完整的32位二进制数。
> 左移不会影响操作数的符号位。换句话说，如果将-2向左移动5位，结果将是-64。

####6. 有符号的右移

有符号的右移操作符由两个大于号（>>）表示，这个操作符会将数值向右移动，但保留符号位（即正负号标记）。

``` js
var oldValue = 64; //等于二进制的1000000
var newValue = oldValue >> 5; //等于二进制的10，即十进制的2
```

>由于右移而出现在符号位右侧、原数值左侧的空位，用符号位的值来填充。

####7. 无符号右移

无符号右移操作符由3个大于号（>>>）表示，这个操作符会将数值的所有32位都向右移动。对正数来说，无符号右移的结果与有符号右移相同。

``` js
var oldValue = 64; //等于二进制的1000000
var newValue = oldValue >>> 5; //等于二进制的10，即十进制的2
```

对于负数来说，情况不同。无符号右移是以0来填充空位，而不是像有符号右移那样以符号位的值来填充空位。无符号右移操作符会把负数的二进制码当成正数的二进制码。而且，由于负数以其绝对值的二进制补码形式表示，因此就会导致无符号右移后的结果非常之大，如：

``` js
var oldValue = -64; //等于二进制的11111111111111111111111111000000
var newValue = oldValue >>> 5; //等于十进制的134217726: 00000111111111111111111111111110
```

###3.5.3 布尔操作符

####1. 逻辑非

逻辑非操作符由一个叹号（！）表示，可以应用于ECMAScript中的任何值。无论这个值是什么数据类型，这个操作符都会返回一个布尔值。逻辑非操作符首先会将它的操作数转换为一个布尔值，然后再对其求反。

- 如果操作数是一个对象，返回false
- 如果操作数是一个空字符串，返回true
- 如果操作数是一个非空字符串，返回false
- 如果操作数是数值0，返回true
- 如果操作数是任何非0数值（包括Infinity），返回false
- 如果操作数是null，返回true
- 如果操作数是NaN，返回true
- 如果操作数是undefined，返回true

``` js
alert(!false); //true
alert(!"blue"); //false
alert(!0); //true
alert(!NaN); //true
alert(!""); //true
alert(!12345); //false
```

逻辑非操作符也可以用于将一个值转换为与其对应的布尔值。而同时使用两个逻辑非操作符，实际上就会模拟Boolean()转型函数的行为。

``` js
alert(!!"blue"); //true 
alert(!!0); //false 
alert(!!NaN);  //false 
alert(!!"");  //false 
alert(!!12345); //true
```

####2. 逻辑与

逻辑与操作符由两个和号（&&）表示，有两个操作数。逻辑与的真值表如下：

第一个操作数 | 第二个操作数 | 结果
:-- | :-- | :--
true | true | true
true | false | false
false | true | false
false | false | false

逻辑与操作可以应用于任何类型的操作数，而不仅仅是布尔值。在有一个操作数不是布尔值的情况下，逻辑与操作就不一定返回布尔值；此时，它遵循下列规则：

- 如果第一个操作数是对象，则返回第二个操作数
- 如果第二个操作数是对象，则只有在第一个操作数的求值结果为true的情况下才返回该对象。
- 如果两个操作数都是对象，则返回第二个操作数
- 如果有一个操作数是null，则返回null
- 如果有一个操作数是NaN，则返回NaN
- 如果有一个操作数是undefined，则返回undefined

逻辑与操作属于短路操作，即如果第一个操作数能够决定结果，那么就不会再对第二个操作数求值。对于逻辑与操作而言，如果第一个操作数是false，则无论第二个操作数是什么值，结构都不再可能是true了。下面的例子充分说明逻辑与属于短路操作：

``` js
var found = true;
var result = (found && someUndefinedVariable); //这里会发生错误
alert(result); //这一行不会执行
```

但是，

``` js
var found = false;
var result = (found && someUndefinedVariable); //不会发生错误，因为就根本没有求someUndefinedVariable的值
alert(result); //会执行("false")
```

####3. 逻辑或

逻辑或操作符由两个竖线符号（||）表示，有两个操作符。逻辑或的真值表如下：

第一个操作数 | 第二个操作数 | 结果
:-- | :-- | :--
true | true | true
true | false | true
false | true | true
false | false | false

与逻辑与操作相似，如果有一个操作数不是布尔值，逻辑或也不一定返回布尔值。此时，它遵循下列规则：

- 如果第一个操作数是对象，则返回第一个操作数
- 如果第一个操作数的求值结果为false，则返回第二个操作数
- 如果两个操作数都是对象，则返回第一个操作数
- 如果两个操作数都是null，则返回null
- 如果两个操作数都是NaN，则返回NaN
- 如果两个操作数都是undefined，则返回undefined

逻辑或操作符也是短路操作符，如果第一个操作数的求值结果为true，就不会对第二个操作数求值了。

``` js
var found = true;
var result = (found || someUndefinedVariable); //不会发生错误
alert(result); //会执行 ("true")
```

但是，

``` js
var found = false;
var result = (found || someUndefinedVariable); //这里会发生错误
alert(result); //这一行不会执行
```

我们可以利用逻辑或这一行为来避免为变量赋null或undefined值，例如：`var myObject = preferredObject || backupObject;`。

###3.5.4 乘性操作符

乘性操作符包括：乘法、除法和求模。如果参与乘性计算的某个操作数不是数值，后台会先使用Number()转型函数将其转换为数值，即空字符串被当作0，布尔值true被当作1。

####1. 乘法

乘法操作符由一个星号（*）表示，用于计算两个数值的乘积。在处理特殊值的情况下，乘法操作符遵循下列特殊的规则：

- 如果操作数都是数值，执行常规的乘法计算。如果乘积超过了ECMAScript数值的表示范围，则返回Infinity或-Infinity。
- 如果有一个操作数是NaN，则结果为NaN
- 如果是Infinity与0相乘，则结果是NaN
- 如果是Infinity与非0数值相乘，则结果是Infinity或-Infinity，取决于有符号操作数的符号
- 如果是Infinity与Infinity相乘，则结果是Infinity
- 如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则。

####2. 除法

除法操作符由一个斜线符号（/）表示，执行第二个操作数除第一个操作数的计算。规则如下：

- 如果操作数都是数值，执行常规的除法计算。如果商超过了ECMAScript数值的表示范围，则返回Infinity或-Infinity
- 如果有一个操作数是NaN，则结果是NaN
- 如果是Infinity被Infinity除，则结果是NaN
- 如果是零被零除，则结果是NaN
- 如果是非零的有限数被零除，则结果是Infinity或-Infinity，取决于有符号操作数的符号
- 如果是Infinity被任何非零数值除，则结果是Infinity或-Infinity，取决于有符号操作数的符号
- 如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则。

####3. 求模

求模（余数）操作符由一个百分号（%）表示，遵循如下规则：

- 如果操作数都是数值，执行常规的除法计算，返回除得的余数
- 如果被除数是无穷大值而除数是有限大的数值，则结果是 NaN
- 如果被除数是有限大的数值而除数是零，则结果是 NaN
- 如果是 Infinity 被 Infinity 除，则结果是 NaN
- 如果被除数是有限大的数值而除数是无穷大的数值，则结果是被除数
- 如果被除数是零，则结果是零
- 如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则

###3.5.5 加性操作符

加性操作符也会在后台转换不同的数据类型，且转换规则还比较复杂。

####1. 加法

加法操作符（+）。如果两个操作符都是数值，执行常规的加法计算，然后根据下列规则返回结果：

- 如果两个操作符都是数值，则执行常规的算术加法操作并返回结果
- 如果有一个操作数是NaN，则结果是NaN
- 如果是Infinity加Infinity，则结果是Infinity
- 如果是-Infinity加-Infinity，则结果是-Infinity
- 如果是Infinity加-Infinity，则结果是NaN
- 如果是+0加+0，则结果是+0
- 如果是-0加-0，则结果是-0
- 如果是+0加-0，则结果是+0
- 如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接起来
- 如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来
- 如果有一个操作数是对象、数值或布尔值，则调用它们的toString()方法取得相应的字符串值，然后再应用前面关于字符串的规则
- 对于undefined和null，分别调用String()函数并取得字符串"undefined"和"null"

``` js
var result1 = 5 + 5; //两个数值相加
alert(result1); //10
var result2 = 5 + "5"; //一个数值和一个字符串相加
alert(result2); //"55"
```

常见错误：

``` js
var num1 = 5;
var num2 = 10;
var message = "The sum of 5 and 10 is " + num1 + num2;
alert(message); //"The sum of 5 and 10 is 510"
```

应更改为：

``` js
var num1 = 5;
var num2 = 10;
var message = "The sum of 5 and 10 is " + (num1 + num2); //注意这里圆括号的使用
alert(message); //"The sum of 5 and 10 is 15"
```

####2. 减法

减法操作符（-），在处理各种数据类型转换时，同样需要遵循一些特殊规则：

- 如果两个操作符都是数值，则执行常规的算术减法操作并返回结果
- 如果有一个操作数是NaN，则结果是NaN
- 如果是Infinity减Infinity，则结果是NaN
- 如果是-Infinity减-Infinity，则结果是NaN
- 如果是Infinity减-Infinity，则结果是Infinity
- 如果是-Infinity减Infinity，则结果是-Infinity
- 如果是+0减+0，则结果是+0
- 如果是+0减-0，则结果是-0
- 如果是-0减-0，则结果是+0
- 如果有一个操作数是字符串、布尔值、null或undefined，则先在后台调用Number()函数将其转换为数值，然后再根据前面的规则执行减法计算。如果转换的结果是NaN，则减法的结果就是NaN
- 如果有一个操作数是对象，则调用对象的valueOf()方法以取得表示该对象的数值。如果得到的值是NaN，则减法的结果就是NaN。如果对象没有valueOf()方法，则调用其toString()方法并将得到的字符串转换为数值。

``` js
var result1 = 5 - true; //4，因为true被转换成了1
var result2 = NaN - 1; //NaN
var result3 = 5 - 3; //2
var result4 = 5 - ""; //5，因为""被转换成了0
var result5 = 5 - "2"; //3， 因为"2"被转换成了2
var result6 = 5 - null; //5，因为null被转换成了0
```

###3.5.6 关系操作符

关系操作符：小于（<）、大于（>）、小于等于（<=）、大于等于（>=）用于对两个值进行比较，这几个操作符返回的都是布尔值。当关系操作符的操作数使用了非数值时，也要进行数据转换或完成某些奇怪的操作，规则如下：

- 如果两个操作数都是数值，则执行数值比较
- 如果两个操作数都是字符串，则比较两个字符串对应的字符编码值
- 如果一个操作数是数值，则将另一个操作数转换为一个数值，然后执行数值比较
- 如果一个操作数是对象，则调用这个对象的valueOf()方法，用得到的结果按照前面的规则执行比较。如果对象没有valueOf()方法，则调用toString()方法，并用得到的结果根据前面的规则执行比较
- 如果一个操作数是布尔值，则先将其转换为数值，然后再执行比较

比较字符串时，按照字符串中对应位置的每个字符的字符编码值，经过一番比较后，返回一个布尔值。由于大写字母的字符编码全部小于小写字母的字符编码，因此会出现：`var result = "Brick" < "alphabet"; //true`。如果把大写字母转换为小写的话，就会得到：`var result = "Brick".toLowerCase() < "alphabet".toLowerCase(); //false`。

另外在比较两个数字字符串时，会发生这种情况：`var result = "23" < "3"; //true`。如果像下面这样写，结果又不一样：`var result = "23" < 3; //false`

如果比较的字符串不能被转换成一个合理数值，则：`var result = "a" < 3; //false，因为"a"被转换成了NaN`。根据规则，任何操作数与NaN进行关系比较，结果都是false，于是：`var result1 = NaN < 3; //false`；`var result2 = NaN >= 3; //false`

###3.5.7 相等操作符

####1. 相等和不相等（先转换再比较）

- 相等：==
- 不相等：!=

这两个操作符都会先转换操作数（通常称为强制转型），然后再比较他们的相等性。在转换不同的数据类型时，相等和不相等操作符遵循下列基本规则：

- 如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值——false转换为0，而true转换为1
- 如果一个操作数是字符串，另一个操作数是数值，在比较相等性之前先将字符串转换为数值
- 如果一个操作数是对象，另一个操作数不是，则调用对象的valueOf()方法，用得到的基本类型值按照前面的规则进行比较

这两个操作符在进行比较时则要遵循下列规则：

- null和undefined是相等的
- 要比较相等性之前，不能将null和undefined转换成其他任何值
- 如果有一个操作数是NaN，则相等操作符返回false，而不相等操作符返回true。重要提示：即使两个操作数都是NaN，相等操作符也返回false；因为按照规则，NaN不等于NaN
- 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回true；否则，返回false。

下表列出了一些特殊情况：

表达式 | 值
:-- | :--
null == undefined | true
"NaN" == NaN | false
5 == NaN | false
NaN == NaN | false
NaN != NaN | true
false == 0 | true
true == 1 | true
true == 2 | false
undefined == 0 | false
null == 0 | false
"5" == 5 | true

####2. 全等和不全等（仅比较不转换）

- 全等：===
- 不全等：!==

> null == undefined 会返回true，但是null === undefined会返回false，因为它们是不同类型的值。

###3.5.8 条件操作符

``` js
variable = boolean_expression ? true_value : false_value;
```

例如：

``` js
var max = (num1 > num2) ? num1 : num2;
```

###3.5.9 赋值操作符

简单的赋值操作符由等于号（=）表示，其作用就是把右侧的值赋给左侧的变量。

等于号前面添加乘性操作符、加性操作符或位操作符，可以完成复合赋值操作。如：

``` js
var num = 10;
num = num + 10;
```

相当于：

``` js
var num = 10;
num += 10;
```

- 乘/赋值（*=）
- 除/赋值（/=）
- 模/赋值（%=）
- 加/赋值（+=）
- 减/赋值（-=）
- 左移/赋值（<<=）
- 有符号右移/赋值（>>=）
- 无符号右移/赋值（>>>=）

复合赋值操作符主要目的是简化赋值操作，不会带来任何性质的提升

###3.5.10 逗号操作符

使用逗号操作符可以再一条语句中执行多个操作，如：`var num1 = 1, num2 = 2, num3 = 3;`。逗号操作符多用于声明多个变量，但除此之外，它还可用于赋值。在用于赋值时，逗号操作符总会返回表达式中的最后一项，如：`var num = (5, 1, 4, 8, 0); //num的值为0`

##3.6 语句

###3.6.1 if语句

语法：`if (condition) statement1 else statement2`。其中的condition（条件）可以是任意表达式，而且对这个表达式求值的结果不一定是布尔值。ECMAScript会自动调用Boolean()转换函数将这个表达式的结果转换为一个布尔值。执行语句可以是一行代码，也可以是代码块（以一对花括号括起来的多行代码），如：

``` js
if (i > 25)
    alert("Greater than 25."); //单行语句
else {
    alert("Less than or equal to 25."); //代码块中的语句
}
```

###3.6.2 do-while语句

do-while语句是一种后测试循环语句，即只有在循环体中的代码执行之后，才会测试出口条件。也就是说，在对条件表达式求值之前，循环体内的代码至少会被执行一次。语法如下：

``` js
do {
    statement
} while (expression);
```

例如：

``` js
var i = 0;
do {
    i += 2;
} while (i < 10);
alert(i);
```

###3.6.3 while语句

while语句属于前测试循环语句，即在循环体内的代码被执行之前，就会对出口条件求值。因此，循环体内的代码有可能永远不会被执行，语法如下：

``` js
while (expression) statement
```

例如：

``` js
var i = 0;
while (i < 10) {
    i += 2;
}
```

###3.6.4 for语句

for语句也是一种前测试循环语句，但它具有执行循环之前初始化变量和定义循环后要执行的代码的能力，语法如下：

``` js
for (initialization; expression; post-loop-expression) statement
```

例如：

``` js
var count = 10;
for (var i = 0; i < count; i++){
    alert(i);
}
```

上述for循环语句与下面的while语句的功能相同：

``` js
var count = 10;
var i = 0;
while(i < count) {
    alert(i);
    i++;
}
```

for循环的变量初始化表达式中，也可以不适用var关键字，该变量的初始化可以再外部执行，如：

``` js
var count = 10;
var i;
for(i = 0; i < count; i++){
    alert(i);
}
```

以上代码与在循环初始化表达式中声明变量的效果是一样的。由于ECMAScript中不存在块级作用域，因此在循环内部定义的变量也可以在外部访问到。例如：

``` js
var count = 10;
for (var i = 0; i < count; i++){
    alert(i);
}
alert(i); //10
```

此外，for语句中的初始化表达式、控制表达式和循环后表达式都是可选的。将这两个表达式全部省略，就会创建一个无限循环，例如：

``` js
for (;;) { //无限循环
    doSomething();
}
```

如果只给出控制表达式，实际上就把for循环转换成了while循环，例如：

``` js
var count = 10;
var i = 0;
for(; i < count; ){
    alert(i);
    i++;
}
```

###3.6.4 for-in语句

for-in语句是一种精准的迭代语句，可以用来枚举对象的属性，语法如下：

``` js
for (property in expression) statement
```

例如：

``` js
for (var propName in window){ //这里的var不是必须的，但是为了保证使用局部变量，推荐这种用var的做法
    document.write(propName);
}
```

ECMAScript对象的属性没有顺序，因此，通过for-in循环输出的属性名的顺序是不可预测的。具体来讲，所有属性都会被返回一次，但返回的先后次序可能会因浏览器而异。

ECMAScript碰到要迭代的对象的变量值为null或undefined时，不再抛出错误，而只是不执行循环体。为了保证最大限度的兼容性，建议在使用for-in循环之前，先检测确认该对象的值不是null或undefined。

###3.6.6 label语句

使用lable语句可以再代码中添加标签，以便将来使用。语法如下：

``` js
label: statement
```

例如：

``` js
start: for(var i = 0; i < count; i++){
    alert(i);
}
```

上述例子中定义的start标签可以在将来由break或continue语句引用。加标签的语句一般都要与for语句等循环语句配合使用。

###3.6.7 break和continue语句

break语句会立即退出循环，强制继续执行循环后面的语句。continue语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行，例如：

``` js
var num = 0;
for(var i=1; i<10; i++){
    if(i%5 == 0){
        break;
    }
    num++;
}
alert(num); //4
```

``` js
var num = 0;
for(var i=1; i<10; i++){
    if(i%5 == 0){
        continue;
    }
    num++;
}
alert(num); //8
```

与label语句联合使用：

``` js
var num = 0;

outermost:
for(var i=0; i<10; i++){
    for(var j=0; j<10; j++){
        if(i==5 && j==5){
            break outermost;
        }
        num++;
    }
}
alert(num); //55
```

``` js
var num = 0;

outermost:
for(var i=0; i<10; i++){
    for(var j=0; j<10; j++){
        if(i==5 && j==5){
            continue outermost;
        }
        num++;
    }
}
alert(num); //95
```

###3.6.8 with语句

with语句的作用是将代码的作用域设置到一个特定的对象中，语法如下：

``` js
with (expression) statement;
```

定义with语句的目的主要是为了简化多次编写同一个对象的工作，例如：

``` js
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;
```

以上几行代码都包含location对象。如果使用with语句，就可以改写如下：

``` js
with(location){
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}
```

在这个重写后的例子中，使用with语句关联了location对象。这意味着在with语句的代码块内部，每个变量首先被认为是一个局部变量。而如果在局部环境中找不到该变量的定义，就会查询location对象中是否有同名的属性。如果发现了同名属性，则以location对象属性的值作为变量的值。

严格模式下不允许使用with语句，将被视为语法错误。

> 由于大量使用with语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程不建议使用with语句。

###3.6.9 switch语句

语法：

``` js
switch (expression) {
    case value: statement
        break;
    case value: statement
        break;
    case value: statement
        break;
    case value: statement
        break;
    default: statement
}
```

switch语句中的每一种情形（case）的含义是：“如果表达式等于这个值（value），则执行后面的语句（statement）”。而break关键字会导致代码执行流跳出switch语句。如果省略break关键字，就会导致执行完当前case后，继续执行下一个case。最后的default关键字则用于在表达式不匹配前面任何一种情形的时候，执行机动代码（因此，也相当于一个else语句）。

从根本上讲，switch语句就是为了让开发人员免于编写像下面这样的代码：

``` js
if (i == 25) {
    alert("25);
} else if (i == 35) {
    alert("35");
} else if (i == 45) {
    alert("45);
} else {
    alert("Other);
}
```

而与此等价的switch语句如下所示：

``` js
switch (i) {
    case 25:
        alert("25");
        break;
    case 35:
        alert("35");
        break;
    case 45:
        alert("45");
        break;
    default:
        alert("Other");
}
```

通过为每个case后面都添加一个break语句，就可以避免同时执行多个case代码的情况。假如确实需要混合几种情形，不要忘了在代码中添加注释，说明你是有意省略了break关键字，如下所示：

``` js
switch (i) {
    case 25:
         /* 合并两种情形 */
    case 35:
        alert("25 or 35");
        break;
    case 45:
        alert("45");
        break;
    default;
        alert("Other");
}
```

- 可以再switch语句中使用任何数据类型，无论是字符串，还是对象都没有问题
- 每个case的值不一定是常量，可以是变量，甚至是表达式，如：

``` js
switch ("hello world") {
    case "hello" + "world":
        alert("Greeting was found.");
        break;
    case "goodbye":
        alert("Closing was found.");
        break;
    default:
        alert("unexpected message was found.");
}
```

在上述例子中，switch语句使用的就是字符串。其中，第一种情形实际上是一个对字符串拼接操作求值的表达式。由于这个字符串拼接表达式的结果与switch的参数相等，因此结果就会显示“Greeting was found.”。而且，使用表达式作为case值还可以实现下列操作：

``` js
var num = 25;
switch (true) {
    case num < 0:
        alert("Less than 0.");
        break;
    case num >= 0 && num <= 10:
        alert("Between 0 and 10.");
        break;
    case num >10 && num <= 20:
        alert("Between 10 and 20.");
        break;
    default:
        alert("More than 20.");
}
```

##3.7 函数

通过函数可以封装任意多条语句，而且可以再任何地方、任何时候调用执行。语法如下：

``` js
function functionName(arg0, arg1, ..., argN){
    statements
}
```

以下是一个函数示例：

``` js
function sayHi(name, message){
    alert("Hello " + name + "," + message);
}
```

这个函数可以通过其函数名来调用，后面还要加上一对圆括号和参数（圆括号中的参数如果有多个，可以用逗号隔开）。调用sayHi函数的代码如下所示：

``` js
sayHi("Nicholas", "how are you today?");
```

ECMAScript中的函数在定义时不必指定是否返回值。实际上，任何函数在任何时候都可以通过return语句后跟要返回的值来实现返回值。请看下面的例子：

``` js
function sum(num1, num2){
    return num1 + num2;
}
```

这个sum()函数的作用是把两个值加起来返回一个结果，我们注意到，除了return语句之外，没有任何声明表示该函数会返回一个值。调用这个函数的示例代码如下：

``` js
var result = sum(5, 10);
```

这个函数会执行完return语句之后停止并立即退出。因此，位于return语句之后的任何代码都永远不会执行。例如：

``` js
function sum(num1, num2){
    return num1 + num2;
    alert("Hello world"); //永远不会执行
}
```

一个函数中也可以包含多个return语句，如：

``` js
function diff(num1, num2) {
    if (num1 < num2) {
        return num2 - num1;
    } else {
        return num1 - num2;
    }
}
```

另外，return语句也可以不带有任何返回值。在这种情况下，函数在停止执行后将返回undefined值。这种用法一般用在需要提前停止函数执行而又不需要返回值的情况下。如下例中，就不会显示警告框：

``` js
function sayHi(name, message){
    return;
    alert("Hello " + name + "," + message); //永远不会调用
}
```

> 推荐的做法是要么让函数始终都返回一个值，要么永远都不要返回值。否则，如果函数有时候返回值，有时候又不返回值，会给调试代码带来不便。

严格模式对函数有一些限制：

- 不能把函数命名为eval或arguments
- 不能把参数命名为eval或argument
- 不能出现两个命名参数同名的情况

如果发生以上情况，就会导致语法错误，代码无法执行。

###3.7.1 理解参数

ECMAScript函数的参数与大多数其他语言中函数的参数有所不同。ECMAScript函数不介意传递进来多少个参数，也不在乎传进来参数是什么数据类型。也就是说，即便你定义的函数只接收两个参数，在调用这个函数时也未必一定要传递两个参数。可以传递一个、三个甚至不传递参数，而解析器永远不会有什么怨言。之所以会这样，原因是ECMAScript中的参数在内部是用一个数组来表示的。函数接收到的始终都是这个数组，而不关心数组中包含哪些参数（如果有参数的话）。如果这个数组中不包含任何元素，无所谓；如果包含多个元素，也没有问题。实际上，在函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的每一个参数。

其实，arguments对象只是与数组类似（它并不是Array的实例），因为可以使用方括号语法访问它的每一个元素（即第一个元素是arguments[0]，第二个元素是arguments[1]，以此类推），使用length属性来确定传递进来多少个参数。在前面的例子中，sayHi()函数的第一个参数的名字叫name，而该参数的值也可以通过访问argument[0]来获取。因此，那个函数也可以像下面这样重写，即不显式地使用命名参数：

``` js
function sayHi(){
    alert("Hello " + arguments[0] + "," + arguments[1]);
}
```

上述示例说明了ECMAscript函数的一个重要特点：命名的参数只提供便利，但不是必需的。另外，在命名参数方面，其他语言可能需要事先创建一个函数签名，而将来的调用必须与该签名一致。但在ECMAScript中，没有这些条条框框，解析器不会验证命名参数。

通过访问arguments对象的length属性可以获知有多少个参数传递给了函数。下面这个函数在每次调用时，输出传入其中的参数个数：

``` js
function howManyArgs(){
    alert(arguments.length);
}
howManyArgs("string", 45); //2
howManyArgs(); //0
howManyArgs(12); //1
```

由此可见，开发人员可以利用这一点让函数能够接收任意个参数并分别实现适当的功能，见下例：

``` js
function doAdd(){
    if(arguments.length == 1) {
        alert(arguments[0] + 10);
    } else if (arguments.length == 2){
        alert(arguments[0] + arguments[1]);
    }
}

doAdd(10); //20
doAdd(30, 20); //50
```

另一个与参数相关的重要方面，就是aruments对象可以与命名参数一起使用，如：

``` js
function doAdd(num1, num2){
    if(arguments.length == 1){
        alert(num1 + 10);
    } else if(arguments.length == 2){
        alert(arguments[0] + num2);
    }
}
```

关于arguments的行为，还有一点比较有意思，那就是它的值永远与对应命名参数的值保持同步，如：

``` js
function doAdd(num1, num2) {
    arguments[1] = 10;
    alert(arguments[0] + num2);
}
```

每次执行这个doAdd()函数都会重写第二个参数，将第二个参数的值修改为10。因为arguments对象中的值会自动反映到对应的命名参数，所以修改arguments[1]，也就修改了num2，结果它们的值都会变成10。不过，这并不是说读取这两个值会访问相同的内存空间；它们的内存空间是独立的，但它们的值会同步。但这种更影响是单向的：修改命名参数不会改变arguments中对应的值。另外还要记住，如果只传入了一个参数，那么为arguments[1]设置的值不会反应到命名参数中。这是因为arguments对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。

关于参数还要记住最后一点：没有传递值的命名参数将自动被赋予undefined值。这就跟定义了变量但又没有初始化一样。例如，如果只给doAdd()函数传递了一个参数，则num2中就会保存undefined值。

严格模式对如何使用arguments对象做出了一些限制。首先，像前面例子中那样的赋值会变得无效。也就是说，即使把arguments[1]设置为10，num2的值仍然还是undefined。其次，重写arguments的值会导致语法错误（代码将不会执行）。

> ECMAScript中的所有参数传递的都是值，不可能通过引用传递参数。

###3.7.2 没有重载

ECMAScript函数不能像传统意义上的那样实现重载。而在其他语言（如Java）中，可以为一个函数编写两个定义，只要这两个定义的签名（接受的参数的类型和数量）不同即可。如前所述，ECMAScript函数没有签名，因为其参数是由包含零或多个值的数组来表示的。而没有函数签名，真正的重载是不可能做到的。

如果在ECMAScript中定义了两个名字相同的函数，则该名字只属于后定义的函数，如：

``` js
function addSomeNumber(num){
    return num + 100;
}

function addSomeNumber(num){
    return num + 200;
}

var result = addSumNumber(100); //300
```

后面定义的函数覆盖了先定义的函数，因此当在最后一行代码中调用这个函数时，返回的结果就是300。

如前所述，通过检查传入函数中的参数的类型和数量并作出不同的反应，可以模仿方法的重载。

##3.8 小结

- ECMAScript中的基本数据类型包括Undefined、Null、Boolean、Number和String
- 与其他语言不同，ECMAScript没有为整数和浮点数值分别定义不同的数据类型，Number类型可用于表示所有数值
- ECMAScript中也有一种复杂的数据类型，即Object类型，该类型是这门语言中所有对象的基础类型
- 严格模式为这门语言中容易出错的地方施加了限制
- ECMAScript提供了很多与C及其他类C语言中相同的基本操作符，包括算数操作符、布尔操作符、关系操作符、相等操作符及赋值操作符等
- ECMAScript从其他语言中借鉴了很多流控制语句，例如：if语句、for语句和switch语句等。ECMAScript中的函数与其他语言中的函数有诸多不同之处
- 无须指定函数的返回值，因为任何ECMAScript函数都可以在任何时候返回任何值
- 实际上，未指定返回值的函数返回的是一种特殊的undefined值
- ECMAScript中也没有函数签名的概念，因为其函数参数是以一个包含零或多个值的数组的形式传递的
- 可以向ECMAScript函数传递任意数量的参数，并且可以通过arguments对象来访问这些参数
- 由于不存在函数签名的特性，ECMAScript函数不能重载。




