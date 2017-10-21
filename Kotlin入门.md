Kotlin入门

[TOC]

# 入门
## 基本句法
### 定义包名
在源文件的顶部定义包名

```java
package my.demo
import java.util.* 
// ...
```

Kotlin不要求匹配目录和包名：源文件可以放在文件系统任意的位置。

### 定义函数
下述函数定义了两个Int参数和一个Int的返回类型：

```java
fun sum(a: Int, b: Int): Int { 
    return a + b 
}
```

kotlin函数可以根据表达式类容推断出返回类型：

```java
fun sum(a: Int, b: Int) = a + b
```

函数返回Unit类型（相当于java中的void）：

```java
fun printSum(a: Int, b: Int) : Unit {
    println("sum of $a and $b is ${a + b}") 
}
```

Unit 返回类型可以省略：

```java
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}") 
}
```

### 定义本地变量
一次赋值（只读）本地变量：

```java
val a: Int = 1 //立即分配
val b = 2 //Int型是被推断出
val c: Int
c = 3          //推迟赋值
```

可变变量

```java
var x = 5 
x += 1
```

### 注释

同Java和JavaScript一样，Kotlin支持行注释和块注释。

```java
// 这是单行注释

/* 这是多行的块注释 /* 嵌套注释 */ */
```

和Java不一样的地方在于，Kotlin中的块注释可以嵌套。

### 使用字符串模板

```java
var a = 1 

// 模板中使用简单的名称
val s1 = "a is $a"

a = 2 

// 模板中使用随意的表达式
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

### 使用条件表达式

```java
fun maxOf(a: Int, b: Int): Int {
     if (a > b) {
        return a 
     } else {
        return b 
     } 
}
```

将if当做一个表达式

```java
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

### 使用nullable和校验null
当一个引用可能是null值时，必须明确标记nullable

下述函数，当str没有保存一个整数时将返回null

```java
fun parseInt(str: String): Int? { 
    // ...
}
```

使用一个函数，返回nullable值

```java
fun printProduct(arg1: String, arg2: String) { 

    val x = parseInt(arg1) 
    val y = parseInt(arg2)

    // 使用`x*y`会产生错误，因为他们可能是空值。
    if (x != null && y != null) {
    // 做了空校验之后，x 和 y会自动转成非空值。 
        println(x * y) 
    } else {
        println("either '$arg1' or '$arg2' is not a number") 
    }
}
```

### 使用类型检查和类型自动转化
操作符 is 校验表达式是否是某个类型的实例，如果本地常量或属性经过明确的类型校验，他将没必要做明确的类型转换。

```java
fun getStringLength(obj: Any): Int? {

    if (obj is String) { 
        // 这个分支中，obj已经自动转换成String类型
        return obj.length 
    }

    // 在类型检查分支外，obj还是Any类型
    return null
}
```
或者：

```java
fun getStringLength(obj: Any): Int? { 
    if (obj !is String) return null

    // 这个分支中，obj已经自动转换成String类型
    return obj.length

}
```
更甚至：

```java
fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&` 
    //在执行&&右边运算时，obj 已经自动转换成String类型
    if (obj is String && obj.length > 0) { 
        return obj.length 
    }
    return null
}
```

### 使用for循环

```java
val items = listOf<String>("apple","orange","kiwi")
    
for(item in items)
    println(item)

for(index in items.indices)
    println("item at $index is ${items[index]}")
```

### 使用while循环

```java
val items = listOf<String>("apple","orange","kiwi")

var index = 0
while(index<items.size){
    println("item at $index is ${items[index]}")
    index++
}
```

### 使用when表达式

```java
fun describe(obj: Any): String =
    when (obj) {
        1 -> "one"
        is Long -> "Long"
        "Hello" -> "Greeting"
        !is String -> "not String"
        else -> "Unknown"
    }
```

### 使用序列
用in操作符检查数字是否在序列中

```java
val x = 9
val y = 10
if(x in 1..y+1)
    print("fits in range")
```

检查数字是否在序列外

```java
val list = listOf<String>("a","b","c")
    
if(-1 !in 0..list.lastIndex)
    println("-1 is out of range")
    
if(list.size !in list.indices)
    println("list size is out of valid list indices range too")
```

迭代序列

```java
for(index in 1..5)
    println("index = $index")
```

跳跃迭代

```java
for(index in 0..10 step 2)
    println("index = $index")
    
for(index in 9 downTo 1 step 3)
    println("index = $index")
```
### 使用集合

迭代集合

```java
val items = listOf<String>("apple", "orange", "kiwi")

for (item in items)
    println(item)
```

用in操作符检查集合是否包含指定对象

```java
when { 
    "orange" in items -> println("juicy") 
    "apple" in items -> println("apple is fine too") 
}
```

使用lambda表达式过滤和转换集合

```java
val items = listOf<String>("apple", "orange", "kiwi")
var it :String
items
    .filter { it.startsWith("a") } 
    .sortedBy { it } 
    .map { it.toUpperCase() } 
    .forEach { println(it) }
```

## 风格
### 创建DTOs（POJOs/POCOs）

```java
data class Customer(val name: String, val email: String)
```
定义的`Customer`类有如下功能：

* getters(var 变量还有setters)方法
* equals()
* hashCode()
* toString()
* copy()
* component1() , component2()...(对应每个属性)



### 函数参数默认值

```java
fun foo(a: Int = 0, b: String = "") { ... }
```

### 筛选list

```java
val list = 1..5
val position = list.filter { x->x>3 }//position = [4,5]
```
或者简写：

```java
val position = list.filter { x>3 }//position = [4,5]
```

### 插入字符串

```java
println("Name $name")
```

### 检查实例

```java
when (x) { 
    is Foo -> ... 
    is Bar -> ... 
    else -> ... 
}
```

### 遍历map/list

```java
for ((k, v) in map) { 
    println("$k -> $v") 
}
```

>k、v可以是任意类型

### 使用序列

```java
for (i in 1..100) { ... } // 封闭式，包含100
for (i in 1 until 100) { ... } // 半封闭式，不包含100
for (x in 2..10 step 2) { ... } 
for (x in 10 downTo 1) { ... } 
if (x in 1..10) { ... }
```

### 只读list

```java
val list = listOf("a", "b", "c")
```

### 只读map

```java
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

### 访问map

```java
println(map["key"]) 
map["key"] = value //需要实现set方法
```

### 懒属性(lazy property)

```java
val p: String by lazy { 
    // compute the string 
}
```

### 函数表达式

```java
fun String.spaceToCamelCase() { ... }

"Convert this to camelcase".spaceToCamelCase()
```

### 创建单例

```java
object Resource { 
    val name = "Name" 
}
```

### if not null 简单表达

```java
val files = File("Test").listFiles()

println(files?.size)
```
### if not null and else 简单表达

```java
val files = File("Test").listFiles()

println(files?.size ?: "empty")
```
### 如果空，执行声明

```java
val data = ...

val email = data["email"] ?: throw IllegalStateException("Email is missing!")
```
### 如果非空执行
```java
val data = ...

data?.let { ... // execute this block if not null 
}
```
### 返回when声明
```java
fun transform(color: String): Int {
    return when (color) { 
        "Red" -> 0 
        "Green" -> 1 
        "Blue" -> 2 
        else -> throw IllegalArgumentException("Invalid color param value") 
    }
}
```

### try/catch 表达式
```java
fun test() {

    val result = try { 
        count() 
    } catch (e: ArithmeticException) {
        throw IllegalStateException(e) 
    }
// .....

}
```

###  if 表达式

```java
fun foo(param: Int) {
    val result = if (param == 1) { 
        "one" 
    } else if (param == 2) {
        "two" 
    } else {
        "three" 
    }
}
```
### 对返回Unit方法使用构建风格

```java
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```
### 单表达式函数

```java
fun theAnswer() = 42
```

等价于

```java
fun theAnswer(): Int {
    return 42
}
```

还可以结合其他风格，形成更简短的语句。when 表达式为例：

```java
fun transform(color: String): Int = when (color) { 
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}
```

### 使用with，执行对象实例的多个方法

```java
class Turtle {
    fun penDown(){}
    fun penUp(){}
    fun turn(degrees: Double){}
    fun forward(pixels: Double){}
}

val myTurtle = Turtle()

with (myTurtle) {
    penDown()
    for(i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```

### 使用java7 try 读取资源

```java
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}
```

###  要求泛型参数的泛型函数的简单格式

```java
//  public final class Gson {
//  ...
//  public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
//  ...

inline fun <reified T : Any> Gson.fromJson(json): T = this.fromJson(json, T::class.java)
```

### 使用可能为空的Boolean
```java
val b: Boolean? = ... 
if (b == true) {
    ...
} else {
    // `b` is false or null
}
```

## 编码规范
**这页包含了Kotlin语言当前的编码风格**

### 命名风格

如果存疑虑，默认使用java的编码规范，如下：

* 对变量名使用驼峰法，避免使用下划线
* 类型名称用大写字母开头
* 方法名和属性名用小写字母开头
* 四个空格缩进
* 公共方法需要有注释记录，这样就会出现在Kotlin文档中

### 冒号
当用冒号分割类型和父类型时，冒号前后都需要空格；当用冒号分割实体和类型时，冒号后需要空格。

```java
interface Foo<out T : Any> : Bar {
    fun foo(a: Int): T
}
```

### Lambda
在Lambda表达式中，空格用在花括号前后，也用在分割函数体中参数的箭头前后。任何时候，若有可能，lambda表达式都在圆括号的外面。

```java
list.filter { it > 10 }.map { element -> element * 2 }
```

lambda应当简短避免嵌套，推荐使用`it`方便的替代明确声明的参数。在嵌套的lambda中使用参数时，参数应当明确申明。

### 类开头格式
只有少数几个参数的类，可以写在一行：

```java
class Person(id: Int, name: String)
```

>类开头太长时需要格式化，每个主构造函数函数要单独一行且缩进。而且，结束的圆括号也需要单独一行。如果使用继承，调用或列举实现接口的超类构造函数需要在结束圆括号的同一行。

```java
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) {
    // ...
}
```

使用多个接口时，超类构造函数放在前面，每个接口放在单独一行。

```java
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker {
    // ...
}
```

>构造函数参数可以使用单次或多次缩进


### Unit

如果函数返回Unit，返回类型可以省略。

```java
fun foo() { 
    // ": Unit"在此处省略
}
```

### 函数和属性

在某些场景，使用无参函数和只读属性可交换。尽管在语义上相似，但在文法约定上却有一些偏好。

在下述条件中，比属性比函数更适用：
* 不抛出异常
* 具有O(1)复杂度
* 在初次运行时，更方便计算和缓存
* 每次返回相同值


# 基础

## 基本类型

Kotlin中，任何感官上的事务都是对象，我们可以调用任何变量的成员函数和成员属性。某些类型在实现时经过优化，它们在构建时生成，但是对程序员而言，就像使用普通类一样。在这一节将描述这些类型：Numbers、Characters、Booleans和Arrays。

### Numbers

Kotlin处理数字的方式同Java很接近，但不雷同。举个例子：Kotlin不存在数字内在变宽转换（java中，int可以转成long），而且某些场景中存在字面上的差别。

Kotlin提供如下內建类型表示数字（这同Java相似）

| Type | Bit width | Byte width |
| --- | :---: | :---: |
| Double | 64 | 8 |
| Float | 32 | 4 |
| Long | 64 | 8 |
| Int | 32 | 4 |
| Short | 16 | 2 |
| Byte | 8 | 1 |

**注：Kotlin中，字符不是数字**

#### 数字常量

有如下类型的整数常量：

* 十进制：123
    * Long类型用L标记：123L
* 16进制：0x0F
* 二进制：0b00001011

**注：Kotlin中，不支持8进制表示**

也支持浮点标记的数字：

* 默认双精度型：123.5，123.5e10
* 单精度型用F(f)标记：123.5f

#### 下划线分割的数字（1.1开始）
可以使用下划线使数字更易读

```java
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```
#### 存储表示
在Java平台，数字当做原始类型保存在物理存储空间，除非我们需要一个可以为空的引用（如：Int？）或者涉及泛型。后者情况下，数字是封装的。

**注：封装的数字没必要保持它的特征**

```java
val a: Int = 10000
print(a === a) // 输出 'true' 
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA === anotherBoxedA) // !!!输出'false'!!!
```

另一方面，他们保持相等

```java
val a: Int = 10000
print(a == a) // 输出 'true' 
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA == anotherBoxedA) // !!!输出'true'!!!
```

#### 显示转换
由于不同的存储表示，小的类型并不是大些类型的子类型。如果是这样，我们将会遇到下述麻烦。

```java
//假设的代码，不是实际编译的
val a: Int? = 1 //Int类型（java.lang.Integer）
val b: Long? = a //隐式的转换成Long类型（java.lang.Long）
println(a==b) //输出false，用Long.equal()方法校验另一部分的Long
```

不仅仅是特征，还有相等的属性都在替换中悄无声息的丢失了。

简而言之，小的类型不能隐式转换成大的类型，也就是说，我们不能不使用显示转换就将Byte类型的值转换成Int类型的值。

```java
val b: Byte = 1 // 可以，字面意思会静态校验
val i: Int = b // 错误
```

我们可以使用显示转换成更大的类型。

```java
val i: Int = b.toInt() // 可以，显示变宽（位宽）
```

每个数字类型都支持下述类型转换：

* toByte(): Byte
* toShort(): Short
* toInt(): Int
* toLong(): Long
* toFloat(): Float
* toDouble(): Double
* toChar(): Char

缺少隐式转换是很难被发现出来的，因为类型可以通过上下文推理出来、也可以通过算术运算转化成合适的类型。如：

```java
val l = 1L + 3 // Long + Int => Long
```
#### 运算
Kotlin支持数字的标准算术运算，当做合适类的成员声明（但是，编译器会在调用相应的指令进行优化）。

作为位运算，没有为他们定义特殊的字符，只有叫做修整形式命名的函数。如：

```java
val x = (1 shl 2) and 0x000FF000
```

下面完整的列举出所有的位运算（只支持Int和Long）

* shl(bits) ——有符号左移(Java中<<)
* shr(bits) ——有符号右移(Java中>>)
* ushr(bits) ——无符号右移(Java中>>>)
* and(bits) ——位运算与
* or(bits) ——位运算或
* xor(bits) ——位运算异或
* inv() ——位运算取反

### Characters
Character通常用Char表示，不能直接当做数字对待

```java
fun check(c: Char) {
    if (c == 1) { // 错误: 类型不匹配 
    // ...
    } 
}
```
Character用单引号包括:`'1'`，特殊字符可以用反斜线转义。支持转义的特殊字符有：`\t`,`\b`,`\n`,`\r`,`\'`,`\"`,`\\`和`\$`。为了编码更多别的字符，使用Unicode转义序列语法：`'\uFF00'`。

我们可以显示的将字符转换成Int：

```java
fun decimalDigitValue(c: Char): Int { 
    if (c !in '0'..'9') 
        throw IllegalArgumentException("Out of range") 
    return c.toInt() - '0'.toInt() // 显示转换成Int
}
```

同数字一样，当字符需要一个可空的引用时，会被封装起来，字符的特征就不会被保持。

### Booleans
Boolean用类型`Boolean`表示，通常有两个值：**true**和**false**。

如果可以为空引用是必须的，Boolean 也会封装起来。
Boolean类型的內建操作符包括：

* || —— 或
* && —— 与
* ！——非

### Arrays
Kotlin中，Array类表示数组，有`get`和`set`两个函数(通过运算符重载用`[]`表示)和size属性，和一些其他的有用的成员函数。

```java
class Array<T> private constructor() {

    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit
    operator fun iterator(): Iterator<T>

    // ...

}
```

使用库函数`arrayOf()`传入item的值创建数组。`arrayOf(1, 2, 3)`创建`[1, 2, 3]`数组。或者使用`arrayOfNulls()`库函数创建一个指定大小的空数组，每个item值都是null。

还可以使用工厂方法用数组大小和根据索引返回每个item初始值的函数生成数组。

```java
//创建一个Array<String>，值["0", "1", "4", "9", "16"]
val asc = Array(5, { i -> (i * i).toString() })
```

之前已经提过，`[]`表示调用`get()`和`set()`成员函数。

**注：不同于Java，Kotlin中Array是不变的，我们不能给`Array<Any>`分配`Array<String>`，防止可能的运行失败。(但是可以使用`Array<out Any>`)**

Kotlin有一些专门的类来表示没有封装的原始类型数组。`ByteArray`、`ShortArray`、`IntArray`……这些类没有继承实现`Array`，但是他们有相同的方法和属性。他们中的任意一个都有对应的工厂函数。

```java
val x: IntArray = intArrayOf(1, 2, 3) 
x[0] = x[1] + x[2]
```
### Strings
使用`String`表示字符串，String是不可变的，字符串的每个字符元素可以使用索引运算符`[]`访问。可以用for循环迭代访问一个字符串的元素。

```java
for (c in str) { 
    println(c) 
}
```

#### 字符串表示
Kotlin有两种字符串表示形式。转义字符串可能包含转义字符以及由换行和任意文本组成的字符串。转义字符串和Java中的字符串很相似。

```java
val s = "Hello, world!\n"
```

实现转义是由一些传统的方式，比如反斜线。见上述提到的支持的转义序列列表。

原始字符串有三个引号分割，没有转义，可以有换行和其他的字符。

```java
val text = """ 
    for (c in "foo") 
    print(c) 
"""
```

可以使用`trimMargin()`移除开头的空格。

```java
val text = """
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
    """
```

`|`默认用来做空白前缀，你也可以选择其他字符作为参数传递，如`trimMargin(">")`

#### 字符串模板

字符串可以包含模板表达式，即将一段代码推算出来并将结果拼接在字符串中，一个模板表达式由一个dollar符开始($)和一些简单的名称组成。

```java
val i = 10 
val s = "i = $i" // 推算出 "i = 10"
```

或者在花括号中包含任意的表达式

```java
val s = "abc" 
val str = "$s.length is ${s.length}" // 推算出 "abc.length is 3"
```

模板既支持原始字符串，也支持内部转义字符串。如果你需要在原始字符串中表示出'$'(不支持下划线的转义),可以使用下面的语句。

```java
val price = """ 
    ${'$'}9.99 
    """
```


## 包
源文件可能开始于包的声明

```kotlin
package foo.bar 
fun baz() {} 
class Goo {} 
// ...
```

源码中所有的内容（类和函数）都被包含在包的声明中。所以，上述例子中函数`baz`的全称是`foo.bar.baz`,类`Goo`的全称是`foo.bar.Goo`。

>如果包没有具体说明，那么该文件中所有内容都属于没有名称的默认包。

### 默认导入
每个Kotlin源文件都会默认导入数个包：

* kotlin.* 
* kotlin.annotation.*
* kotlin.collections.*
* kotlin.comparisons.* (since 1.1)
* kotlin.io.*
* kotlin.ranges.*
* kotlin.sequences.*
* kotlin.text.*

根据不同的目标平台，再导入一些附加的包：

* JVM:
    * java.lang.*
    * kotlin.jvm.*
* JS:
    * kotlin.js.*

### 自定义导入
除了默认导入的包，每个文件可能包含他自己输入的指令。
我们既可以导入单个名称

```java
import foo.Bar // 现在可以使用Bar而不需要其他的先决条件
```
也可以导入范围内所有可以访问的内容

```java
import foo.* // foo 中所有的内容都可以访问
```

如果存在命名冲突，我们可以使用关键词`as`在本地重命名有冲突的实体来消除歧义。

```java
import foo.Bar // Bar可以访问
import bar.Bar as bBar // bBar 表示'bar.Bar'
```

关键字`import`并不局限于导入类，还可以用来导入别的声明

* 顶层函数和属性
* 对象声明中的函数和属性
* 枚举常量

>不同于Java,Kotlin不支持单独`import static`句法，所有的这些声明都是用常用的import关键字。

### 顶层声明的权限
如果顶层声明的被标记成`private`，那么它只属于文件内部私有。

## 控制流
### if表达式
在Kotlin中，if是一个表达式，即，if有返回值。因此没有三元运算符（条件?yes:no）,因为if已经很好的扮演了这个角色。

```java
// 传统使用方式
var max = a 
if (a < b) 
    max = b

// 使用else时
var max: Int 
if (a > b) {
    max = a 
} else {
    max = b 
}

// 当做表达式
val max = if (a > b) a else b
```
if分支可以使用代码块，块中的最后一个表达式就是改代码块的值。

```java
val max = if (a > b) { 
    print("Choose a") 
    a 
} else { 
    print("Choose b") 
    b 
}
```

如果使用if作为表达式而不是语句（如：返回它的值或者赋值给一个变量），那么表达式就需要else分支。

### when表达式

`when`替换C类编程语言的`switch`操作，它具有简单的格式，如下：

```java
when (x) { 
    1 -> print("x == 1") 
    2 -> print("x == 2") 
    else -> { // Note the block 
        print("x is neither 1 nor 2") 
    } 
}
```

`when`用它的参数按顺序匹配所有的分支，直到满足某个分支条件。`when`也可以当做表达式或语句使用。如果当做表达式使用，那满足条件的分支就是整个表达式的值。如果当做语句使用，那每个分支的值将会被忽略。**同if一样，每个分支可以是一个代码块，每个代码块的值就是该代码块内最后一个表达式的值。**

如果多个案例都用相同的处理方式，那么分支条件可以用逗号绑定。

```java
when (x) { 
    0, 1 -> print("x == 0 or x == 1") 
    else -> print("otherwise") 
}
```

还可以用任意的表达式当做分支条件，不仅仅是常量。

```java
when (x) { 
    parseInt(s) -> print("s encodes x") 
    else -> print("s does not encode x") 
}
```

也可以用`in`、`!in`检验序列或集合中是否存在某个值

```java
when (x) { 
    in 1..10 -> print("x is in the range") 
    in validNumbers -> print("x is valid") 
    !in 10..20 -> print("x is outside the range") 
    else -> print("none of the above") 
}
```

另一方面，我们可以使用`is`、`!is`检查值是不是一些特殊的类型。
需要注意的是，由于智能转换的作用，你可以直接访问类型的属性和方法而不需要直接转换。

```java
fun hasPrefix(x: Any) = when(x) { 
    is String -> x.startsWith("prefix") 
    else -> false 
}
```

`when`还可以替换`if-else``if`使用，如果没有提供参数，分支条件便是一个简单的boolean表达式，当表达式值是true时，便会执行分支的代码。

```java
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}
```

### for循环
for循环可以迭代任何提供了迭代器的对象。句法如下：

```java
for (item in collection) print(item)
```

循环体可以是代码块

```java
for (item: Int in ints) { 
    // ...
}
```

照之前所描述，for循环可以迭代任何提供了迭代器的对象，即：该对象有个成员（或扩展）函数iterator()，函数的返回类型有个成员（或扩展）函数next()还有个返回Boolean类型的成员（或扩展）函数hasNext()。、

**所有的这三个方法都需要用`operator`标记**

for寻短迭代数组是编译成基于索引的循环，因此它没有创建迭代器对象。

如果想要根据索引迭代数组或序列，可以按照下述方式：

```java
for (i in array.indices) { 
    print(array[i]) 
}
```

>迭代一个序列是在编译时做优化处理，没有创建额外对象。

或者，使用库函数`withIndex()`

```java
for ((index, value) in array.withIndex()) { 
    println("the element at $index is $value") 
}
```

### while循环
`while`和`do...while`循环的工作原理同平常一样。

```java
while (x > 0) { 
    x--
}

do {
    val y = retrieveData() 
} while (y != null)
```
### 循环内的break和continue
在循环内部，kotlin支持传统的`break`和`continue`操作。

## 返回和跳转
kotlin有三种跳转结构表达式：

* return 默认情况下，从最近的封闭函数（或匿名函数）返回
* break 中断最近的封闭循环
* continue 处理最近循环中的下一步

所有的这些表达式都可以当做某个更大的表达式的一部分：

```java
val s = x.name ?: return //name==null时，执行return
```

### break和continue标签
Kotlin中任何表达式都可以标记为标签。标签通常有可识别的格式用后缀@标记。如：abc@，fooBar@都是合法的标签。给表达式贴上标签只需在其前面添加一个标签即可。

```java
loop@ for (i in 1..100) { 
    // ...
}
```

现在我们可以使用标签执行`break`和`continue`

```java
loop@ for (i in 1..100) { 
    for (j in 1..100) { 
        if (...) break@loop 
    } 
}
```

一个有资格使用标签的break跳转到被标签标记的循环右面的地方开始执行。continue处理循环的下一次迭代。

### return到标签
Kotlin中与函数式、本地函数和对象表达式对比，函数可以嵌套。有资格`return`的地方允许我们返回到外层函数。最重要的使用案例是从lambda表达式中返回。当我们这样写时会调用。

```java
fun foo() { 
    ints.forEach { 
        if (it == 0) return 
        print(it) 
    } 
}
```

`return`表达式从最近的函数返回，即 foo。**注意，这种非本地返回只支持传递给内联函数的lambda表达式。**如果我们需要从lambda表达式返回，我们需要先标记才可以。

```java
fun foo() { 
    ints.forEach lit@ { 
        if (it == 0) return@lit 
        print(it) 
    } 
}
```

现在，它只从lambda表达式返回。通常情况下，使用隐式标签非常方便。这样的标签与lambda传递的函数的名称相同。

```java
fun foo() { 
    ints.forEach { 
        if (it == 0) return@forEach 
        print(it) 
    } 
}
```

或者，我们把lambda表达式替换成一个匿名函数。一个匿名函数内部的返回语句会从他本身返回。

```java
fun foo() { 
    ints.forEach(fun(value: Int) { 
        if (value == 0) return  
        print(value) 
    })
 }
```

当我们返回一个值时，解析器会优先选择有资格返回，即：

```java
return@a 1
```

表示给标签@a返回1，而不是返回一个标签表达式`@a 1`

# 类和对象
## 类和继承
### 类
Kotlin中使用关键词`class`声明类。

```java
class Invoice{
}
```

类的声明有类名、类头（尤其是类型参数、主构造函数等等）、类体组成，用花括号圈起。类头和类体都是可选的，如果类体为空，那么花裤OAO可以省略。

```java
class Empty
```
#### 构造器
每个类，有一个主构造函数和一个或多个次构造函数。主构造函数是类头的一部分，它在类名称（如果有类型参数）的后面。

```java
class Person constructor(firstName: String) { 
}
```

如果主构造函数没有任何注解或访问修饰符，那么关键词`constructor`可以省略。

```java
class Person(firstName: String) { 
}
```

主构造函数没有任何代码，初始化代码可以发放在用关键词`init`做前缀的代码块里。

```java
class Customer(name: String) { 
    init { 
        logger.info("Customer initialized with value ${name}") 
    } 
}
```

>主构造函数的参数可以在初始化代码块中使用，它们也可以用在类体中，声明的属性初始化赋值时。

```java
class Customer(name: String) { 
    val customerKey = name.toUpperCase() 
}
```

实际上，针对在主构造函数中声明类属性并初始化他们，Kotlin提供了更简洁的句法：

```java
class Person(val firstName: String, val lastName: String, var age: Int) { 
    // ...
}
```

与普通属性大致相同的是，声明在主构造函数中的属性，可以是变量(`var`)也可以是常量(`val`)

如果构造器有注解或者修饰符，关键词`constructor`要求放在修饰符前面。

```java
class Customer public @Inject constructor(name: String) { ... }
```

声明次构造函数，也需要关键词`constructor`做前缀,且不可省略。

```java
class Person { 
    constructor(parent: Person) { 
        parent.children.add(this) 
    } 
}
```

如果类有一个主构造函数，那么每个次构造函数都需要委托给主构造函数，不管是直接委托还是通过其他次构造函数简介委托。同一个类中，委托给其他构造器，需要关键词`this`。

```java
class Person(val name: String) { 
    constructor(name: String, parent: Person) : this(name) { 
        parent.children.add(this) 
    } 
}
```

如果非抽象类没有声明任何构造函数（主构造函数或次构造函数），它将生成一个没有参数的主构造函数。构造函数的可见性将是公共的。如果不希望你的类有公共的构造函数，你可以声明一个非默认可见性的空主构造函数。

```java
class DontCreateMe private constructor () { //主构造函数不可见
}
```

>注：JVM中，如果主构造函数的所有参数都有默认的值，那么编译器将会附加生成一个无参的构造函数使用这些默认值。这样使Kotlin使用Jackson、JPA这类的库通过无参构造函数创建实例变得很容易。

```java 
class Customer(val customerName: String = "")
```
#### 创建类的实例

像使用普通函数一样，调用构造函数创建实例。

```java
val invoice = Invoice()

val customer = Customer("Joe Smith")
```

**注：Kotlin中没有关键词`new`。**

#### 类成员

类包含：

* 构造函数和初始化代码块
* 函数
* 属性
* 嵌套和内部类
* 对象声明

### 继承
Kotlin中所有的类都有一个公共父类`Any`，这是没有声明父类的类的默认父类。

```java
class Example // 隐式集成Any类
```

`Any`不是`java.lang.Object`,特别是他没有除`equals()`、`hashCode()`和`toString()`以外的成员。

在类头部的冒号后，显示声明一个父类型：

```java
//定义父类
open class Base(p: Int)
//继承父类
class Derived(p: Int) : Base(p)
```

如果该类有主构造函数，父类型必须使用主构造函数的参数在这里初始化。

如果该类没有主构造函数，那么每个次构造函数都需要使用关键词`super`初始化父类型，或者委托给其他次构造函数。注意的是，不同的次构造函数可以调用不同的父类型的构造函数。

```java
class MyView : View { 
    constructor(ctx: Context) : super(ctx)
    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

`open`注解的作用和Java中的`final`相反：它允许其他类继承这个类，Kotlin中所有类默认是final。

#### 重写方法

正如我们前面提到的，在kotlin中坚持做明确的事情。不同于Java，Kotlin要求显示注解可重写成员（我们称之为`open`）然后在重写它。

```java
open class Base { 
    open fun v() {} //fun可以被重写
    fun nv() {} 
} 

class Derived() : Base() {
    override fun v() {} //
}
```

必须对`Derived.v()`使用`override`注解，若注解丢失，编译器就会报错。如果没有对函数使用`open`注解，像`Base.nv()`，那么在子类型中不管有没有使用`override`声明相同名称的方法是非法的。在final类中（即，该类没有`open`注解）,禁止开放成员。

标记被重写的成员，其本身是开放的。即，它可以在子类中被重写。如果你想禁止它被再次重写，使用关键词`final`。

```java
open class AnotherDerived() : Base() { 
    final override fun v() {} 
}
```

#### 重写属性
属性重写的工作原理同方法重写的工作原理一致；在父类型中声明的属性在派生类型中声明时需要在其前面添加`override`，而且他们必须有一致的类型。每个声明的属性都可以被有初始值的属性或有getter方法赋值的属性重写。

```java
open class Foo { 
    open val x: Int get { ... } 
}

class Bar1 : Foo() { 
    override val x: Int = ... 
}
```

通常可以把`val`属性重写成`var`属性，反之则不可以。之所以这样是因为，`val`本质上生成了getter方法，而重写成`var`属性则在子类中附加声明了setter方法。

```java
interface Foo { 
    val count: Int 
}

class Bar1(override val count: Int) : Foo

class Bar2 : Foo { 
    override var count: Int = 0 
}
```


#### 重写规则

Kotlin中，实现继承，需要遵循如下规则：如果一个类继承超类相同成员的很多实现，必须重写改成员并提供自己的实现（也许使用继承的某一个）。为了表示使用继承实现中的哪个父类型，使用`super`限定尖括号中的父类型名称。即：`super<Base>`：

```java
open class A { 
    open fun f() { 
        print("A") 
    } 
    fun a() { 
        print("a") 
    } 
}

interface B { 
    fun f() { print("B") } // 接口的成员默认使用open 
    fun b() { print("b") } }

class C() : A(), B { // 编译器要求f()被重写: 
    override fun f() { 
        super<A>.f() // 调用 A.f() 
        super<B>.f() // 调用 B.f() 
    } 
}
``` 

>这样继承`A`和`B`时候没问题的。由于`C`只继承了每个函数的一个实现，所以我们使用`a()`和`b()`是没有问题的。但对`f()`我们有`C`继承的两个实现，因此我们不得不在`C`中重写`f()`和提供自己的实现，消除歧义。

### 抽象类

一个类或它的某些成员可以被声明成`abstract`。一个抽象成员在他的类中不能有具体的实现。
>我们没必要给抽象类使用`open`注解——不言而喻。

**可以对非抽象的open函数重写成抽象函数。**

```java
open class Base { 
    open fun f() {} 
}

abstract class Derived : Base() { 
    override abstract fun f() 
}
```

### 伴随对象

Kotlin中，不同于Java或C#，类没有静态方法。在更多场合，要求使用包层级的函数取代。

如果你需要写一个函数调用时不需要类实例但是需要访问到类内部（如：工厂方法），你可以将它作为该类中对象声明的成员编写。

更具体地说，如果你在你的类中声明一个伴生对象，你可以只使用类名作为修饰语调用它的成员，与Java或C#调用静态方法具有相同的语法。


## 属性和字段
###  声明属性
Kotlin中的类可以有属性，它们可以被声明成可变的`var`可以被声明成只读`val`的。

```java
class Address { 
    var name: String = ... 
    var street: String = ... 
    var city: String = ... 
    var state: String? = ... 
    var zip: String = ...
}
```

要使用属性，我们只需按名称引用它，就好像它是Java中的一个字段。

```java
fun copyAddress(address: Address): Address { 
    val result = Address() // Kotlin中没有new关键词
    result.name = address.name // 访问被调用 
    result.street = address.street 
    // ...
    return result 
}
```

### getter和setter

声明一个属性，完整的句法是这样的：

```java
var <propertyName>[: <PropertyType>] [= <property_initializer>] 
[<getter>] 
[<setter>] //方括号内是可选的
```

初始值、setter、getter都是可选的。如果属性类型能通过初始值或getter返回类型推断，那么也是可选的。像下面这样：

```java
var allByDefault: Int? // 错误: 要求显示初始值, 隐藏setter getter
var initialized = 1 //Int类型是被推算出来,默认有getter setter方法
```

只读属性声明的完整句法与可变属性的声明存在两点不同：`val`开头替代了`var`而且不允许有setter方法。

```java
val simple: Int? //Int类型，默认有getter方法，必须在构造函数中初始化
val inferredType = 1 //Int类型，默认有getter方法
```

我们可以编写自定义的访问器,就像写普通函数一样，在声明属性的右边。下面举个例子：自定义getter方法：

```java
val isEmpty: Boolean 
    get() = this.size == 0
```

自定义setter方法：

```java
var stringRepresentation: String
    get() = this.toString() 
    set(value) { 
        setDataFromString(value) 
    }
```

>为了方便，setter参数名称默认使用`value`，只要你喜欢，也可以使用不同的名称。

Kotlin 版本1.1 起，如果属性类型可以从getter推断出，那么你可以省略属性类型。

```java
val isEmpty get() = this.size == 0//Boolean 类型
```

如果你需要改变一个访问器的权限或添加注解，不需要改变他默认的实现。定义访问器即可，不需要定义方法体。

```java
var setterVisibility: String = "abc" 
    private set // setter 是 private 而且有默认的实现

var setterWithAnnotation: Any? = null 
    @Inject set // setter添加Inject注解
```

#### 幕后字段

Kotlin的类中不能有字段。然而当使用自定义访问器时，有时候拥有个幕后字段是很有必要的。
为此，Kotlin提供一个自动幕后字段，它可以通过标示符`field`访问。

```java
var counter = 0 // 初始值直接写给幕后字段
    set(value) {
        if (value >= 0) field = value
    }
```
> 标识符`field`只能用在属性的访问器中。

如果一个属性至少有一个默认实现的访问器或者使用标识符`field`自定义访问器时，才会生成一个幕后字段。

举个例子，像下述情况就没有幕后字段。

```java
// 只有一个访问器且没有在自定义访问器中使用field。
val isEmpty: Boolean
    get() = this.size == 0
```

> 译者注：好多人分不清属性和字段的区别。在Java和其他大多数面向对象编程语言中，字段指类中申明的变量，属性指类中对变量的读写行为（setter和getter）。而Kotlin中已经将两者合成在一起，因此若单独定义字段，将会发生冲突。

#### 幕后属性

如果你想做的事情不符合这个“隐式的幕后字段”设计，你总可以后退一步使用幕后属性。

```java
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if (_table == null) {
            _table = HashMap() // Type parameters are inferred
        }
        return _table ?: throw AssertionError("Set to null by another thread")
    }
```

从各方面看，这恰好和Java一样，由于访问私有属性的默认setter和getter被优化过，所以没有引入函数调用开销。

### 编译时常量

在编译时已知值的属性可以用`const`修饰符标记为编译常量。

这些属性需要满足以下要求：
- 在顶层或者`object`的成员。
- 用String或者别的原生类型初始化。
- 没有自定义的getter。

这些属性可以用在注解中：

```java
const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"
@Deprecated(SUBSYSTEM_DEPRECATED) fun foo() { ... }
```

### 延迟初始化属性

通常，被声明为非空类型的属性必须在构造函数中初始化。然而，这往往不是很方面。例如：属性可以通过依赖注入来初始化或者在单元测试的setup方法中初始化。在这种情况下，你不能在构造函数中提供非空初始值，但是你仍然想在类体中引用该属性时避免空检查。

为处理这种情况，你可以使用`lateinit`修饰符标记属性。

```java
public class MyTest {
    lateinit var subject: TestSubject
    @SetUp fun setup() {
        subject = TestSubject()
    } @
    Test fun test() {
        subject.method() // 直接引用
    }
}
```

修饰符只能被用在类体中（不是在主构造函数中）声明的变量属性，而且属性没有自定义的setter和getter方法。属性的类型必须是非空，而且不能使原始类型。

访问没有被初始化的lateinit属性会抛出异常，该异常会详细默描述属性被访问而且事实上没有被初始化。

### 重写属性

参见重写属性。

### 委托属性

多数常见的属性都是简单的从幕后属性读取（也有可能写入）值。另一方面，使用自定义的setter和getter可以实现属性的任一行为。介于两者之间必然存在属性如何工作的常见模式。一些例子：懒值、通过key从map获取、访问数据库、访问时通知监听器，等等。

这些常见的行为可以使用委托属性实现为库。

## 接口

### 实现接口
### 接口中的属性
### 解决重写冲突

```
## 访问修饰符
### 包
### 类和接口
### 模块
## 表达式

##数据类
##密封类
##泛型
##嵌套类
##枚举类
##对象表达式和声明
##代理
##代理属性
#函数和lambda
##函数
##高阶函数和Lambda
##内联函数
##协程
#其他
##解构声明
##集合
##序列
##类型校验和转换
##this表达式
##等价
##操作符重载
##null 保护
##异常
##注解
##反射
##类型安全构造器
#java交互
##kotlin中调用java
##java中调用kotlin
#JavaScript
##动态类型
##kotlin中调用JavaScript
##JavaScript中调用Kotlin
##JavaScript模块
##JavaScript反射
```







