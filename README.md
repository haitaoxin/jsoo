# JavaScript Object-Oriented Programming
# 面向对象的JavaScript编程：原理与实践
---

# 1. 简介
## 初衷
笔者早年编写代码使用过 Motorola 68000 和 x86汇编、C/C++、Pascal、Fortran、PowerBuilder、Visual Basic，后来又短暂接触过Java、Python和Swift。最近这些年来使用最多的是JavaScript（正式名称是ECMAScript，本书经常会缩写为JS）。众所周知，JS问世之初是“难登大雅之堂”的小玩意儿，还要靠冠名Java以壮声势。但是随着浏览器作为软件发布、运行的平台成熟起来，再加上NodeJS项目的兴旺，JS本身的进步和普及程度已经使它成为每一个前台软件工程师和全栈工程师必不可少的工具。

虽然这种语言已经强大到适用于大多数类型的后台服务器程序（更不用说它是前台开发的唯一正式语言），但是还是有人诟病其不够严谨，争论它是不是完全符合面向对象（OO）语言的特征。更多的初学者和程序员更是对JS的对象运作机制和使用多少有些含糊不清。这部分地是因为JS的对象跟以前常见的C++或者Java的对象的确有所不同，甚至可以说JS的对象从最初就不像C++或者Java那样经过深思熟虑、严谨定义的。但是幸运的是，发展到今天，JS对OO的支持足以实现绝大多数的编程需要里。

本书并不想参与任何无谓的或者纯理论的讨论，只想理清JS的对象到底是怎么回事，以及怎么使用JS最有效地面向对象编程。
## 适合的读者
本书不是写给初学者看的。读者需要有一定的JS基础以及“知其然且知其所以然”的态度。如果你懂一些C++那更好，因为有时候我会将两者比较。

所有代码都已使用 Node 6.x LTS 环境或者在最新版本的Chrome浏览器里验证过。
## 引用资料
本书的动力之一就是我读了 Nicolas Zakas 的 [《The Principles of Object-Oriented JavaScript》](https://www.nostarch.com/oojs)所受到的启发。有些章节、图表和代码可能会引用其书。本书[永久免费](https://github.com/haitaoxin/jsoo)，但是我也鼓励有英文阅读能力和财力的读者购买 Zakas 的书学习。

其它好的参考书籍和网站我会逐步列出。

# 2. 基础数据类型（Primitive）和引用数据类型（Reference）
和任何其它现代的编程语言一样，JS对普通的算数运算和循环都有很容易理解的使用方法。JS语言本身的与众不同之处很大程度上在于它对数据的表达和处理。根据其在内存里的存储和管理方式，JS支持的数据类型可以分为两大类：基础数据类型（Primitive）和引用数据类型（Reference）。二者的区别对于我们理解对象有很关键的作用。
## 基础数据类型
基础数据类型用于存储比较简单这几类数据：
* Boolean：布尔值，取值范围只有`true`和`false`
* Number：任何整数和小数（浮点数）。⚠️ JS里另有`Number`这种对象，与此处Number数据类型的关系后面会提及。
* String：字符串，包括一个字符（JS没有'char'类型），支持Unicode
* Null：“空”。此类型只有一个值就是`null`
* Undefined：“未定义”。此类型只有一个值就是`undefined`，最常见的地方是你的代码定义了一个变量但是还没有赋值时，其值为`undefined`

所有的基础数据类型都是一个具体的值存储于内存某处，而你定义的变量就直接指向这个值（而不是指向具体值的地址；这是和引用类型最大的区别）。而每一个变量也都指向自己的值，并不跟其它变量共享这个存储地址，即便其二者的值是一样的。比如，

    // strings
    var name = "Jack";
    var answer = "a";

    // numbers
    var count = 25;
    var result = -0;
    var totalCost = 34.56;

    // boolean
    let default = true;

    // null
    var obj = null;

    // undefined
    let k = undefined;
    let q;  // 自动被赋值为 undefined

    // 每个变量都有自己的存贮地址
    let val1 = 50;
    let val2 = val1;  // val2 现在也将‘50’这个值存于自己的存储空间内
    val1 = 25;
    console.log(val2);  // 50；val2的值并不随着val1而改变

### 识别基础数据类型
由于JS在语法上定义一个变量的时候，并不需要声明变量类型，其后赋值时也不进行类型检查，所以有时候程序本身需要动态地检查其所持有的值的类型。为此JS提供了`typeof`操作符:


# 3. 函数（Function）

# 4. 对象（Object）

# 5. 构建函数（Constructor）和原型（Prototype）

# 6. 继承（Inheritance）

# 7. 类（Class）

# 8. Proxy和Reflection API

# 9. 编程攻略
