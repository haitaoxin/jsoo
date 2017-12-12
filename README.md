# JavaScript Object-Oriented Programming
# 面向对象的JavaScript编程：原理与实践
---

# 1. 简介
## 初衷
笔者早年编写代码使用过 Motorola 68000 和 x86汇编、C/C++、Pascal、Fortran、PowerBuilder、Visual Basic，后来又短暂接触过Java、Python和Swift。最近这些年来使用最多的是JavaScript（正式名称是ECMAScript，本书经常会缩写为JS）。众所周知，JS问世之初是“难登大雅之堂”的小玩意儿，还要靠冠名Java以壮声势。但是随着浏览器作为软件发布、运行的平台成熟起来，再加上NodeJS项目的兴旺，JS本身的进步和普及程度已经使它成为每一个前台软件工程师和全栈工程师必不可少的工具。

虽然这种语言已经强大到适用于大多数类型的后台服务器程序（更不用说它是前台开发的唯一正式语言），但是还是有人诟病其不够严谨，争论它是不是完全符合面向对象（Object-Oriented）语言的特征。更多的初学者和程序员更是对JS的对象运作机制和使用多少有些含糊不清。这部分地是因为JS的对象跟以前常见的C++或者Java的对象的确有所不同，甚至可以说JS的对象从最初就不像C++或者Java那样经过深思熟虑、严谨定义的。但是幸运的是，发展到今天，JS对OO的支持足以实现绝大多数的编程需要里。

本书并不想参与任何无谓的或者纯理论的讨论，只想理清JS的对象到底是怎么回事，以及怎么使用JS最有效地面向对象编程。
## 适合的读者
本书不是写给初学者看的。读者需要有一定的JS基础以及“知其然且知其所以然”的态度。如果你学习或者使用了一段时间JavaScript，但是对有些概念还是理解得似是而非；或者你以前用C++和Java，对JavaScript的对象、函数总感觉有些别扭，那这本书就恰好是为你写的。

所有代码都已使用 Node 6.x LTS 环境或者在最新版本的Chrome浏览器里验证过。另外需要抱歉的是有些英文专用单词也许翻译得不是最常见的用法。任何技术上或者翻译上的错误，或者讲述不够清楚的地方，还望读者不吝赐教。
## 引用资料
本书的动力之一就是我读了 Nicolas Zakas 的 [《The Principles of Object-Oriented JavaScript》](https://www.nostarch.com/oojs)所受到的启发。有些章节、图表和代码可能会引用其书。本书[永久免费](https://github.com/haitaoxin/jsoo)，但是我也鼓励有英文阅读能力和财力的读者购买 Zakas 的书学习。

其它好的参考书籍和网站我会逐步列出。

# 2. 基础数据类型（Primitive）和引用数据类型（Reference）
和任何其它现代的编程语言一样，JS对普通的算数运算和循环都有很容易理解的使用方法。JS语言本身的与众不同之处很大程度上在于它对数据的表达和处理。根据其在内存里的存储和管理方式，JS支持的数据类型可以分为两大类：基础数据类型（Primitive）和引用数据类型（Reference）。二者的区别对于我们理解对象有很关键的作用。
## 基础数据类型
基础数据类型用于存储比较简单这几类数据：
* boolean：布尔值，取值范围只有`true`和`false`
* number：任何整数和小数（浮点数）。⚠️ JS里另有`Number`这种对象，与此处number数据类型的关系后面会提及。
* string：字符串，包括一个字符（JS没有'char'类型），支持Unicode
* null：“空”。此类型只有一个值就是`null`
* undefined：“未定义”。此类型只有一个值就是`undefined`，最常见的场景是你的代码定义了一个变量但是还没有赋值时，其值为`undefined`

所有的基础数据类型都是一个具体的值存储于内存某处，而你定义的变量就直接指向这个值（而不是指向具体值的地址；这是和引用类型最大的区别）。而每一个变量也都指向自己的值，并不跟其它变量共享这个存储地址，即便其二者的值是一样的。比如，

```
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
```

### 识别基础数据类型
由于JS在语法上定义一个变量的时候，并不需要声明变量类型，其后赋值时也不进行类型检查，所以有时候程序本身需要动态地检查其所持有的值的类型。为此JS提供了`typeof`操作符:

```
    // 继续上面的代码
    console.log(typeof name);       // 'string'
    console.log(typeof count);      // 'number'
    console.log(typeof totalCost);  // 'number'
    console.log(typeof default);    // 'boolean'
    console.log(typeof k);          // 'undefined'
    console.log(typeof q);          // 'undefined'
    console.log(typeof obj);        // 'object' 这个结果有问题
```

 以上这段代码通俗易懂，但是有两点需要说明：
 1. 严格来说，`typeof`是作用于其后变量当时所被赋予的值，而不是变量本身。因为JS对变量本身并没有定义数据类型。
 2. 最后一句里，变量`obj`的值是`null`，但是`typeof`返回的值却是`object`。这已经被确认为一个JS语言定义里的bug。但是因为这个bug已经存在很久了，如果改正反倒可能让一些以前运行正常的程序出错。所以这个bug就被一直保留了下来。

在我们的代码里正确判断`null`也很简单：

```
    // 如何判断一个值为 null
    console.log(obj === null);    // true
    console.log(k === null);      // false; k is undefined
```

### 带强制类型转换的比较
当你用双等号`==`比较两个不同基础数据类型的值时，JavaScript引擎会试图进行强制数据转换之后再进行比较，而三等号`===`不进行强制类型转换，JavaScript引擎会既比较数据类型也比较数据值。比如

```
    console.log("5" == 5);      // true
    console.log("5" === 5);     // false

    console.log(undefined == null);   // true
    console.log(undefined === null);  // false

```
如果双等号的效果恰好是你需要的，当然这样的代码会比你自己转换数据类型后再比较更简洁。反之这会是不那么容易找到的bug。最好的办法是养成习惯：每次比较都问自己：该使用双等号还是三等号？除非你确定双等号真的是你需要的，都使用三等号。

### 基础数据类型的变量也可以调用一些方法（methods）
在传统的面向对象语言里，方法是只有对象才有的特性，是让对象比数据更强大的主要因素。但是在JS里，赋值为string、number、boolean这三种基础数据类型的变量也有一些自带的方法可以调用，比如

```
    var street = "East First Street";
    var lowercaseStreet = street.toLowerCase();   // 'east first street'
    var firstChar = street.charAt(0);    // 'E'
    var middleStreet = street.substring(5, 10);   // 'First'

    var price = 10;
    var fixedPrice = price.toFixed(2);    // 10.00
    var hexCount = price.toString(16);    // 'a'；十六进制表达的10

    var isDone = true;
    var isDoneFlag = isDone.toString();   // 'true'

    var p = "East First Street".toLowerCase();    // type error
    var q = 10.toFixed(2);    // syntax error
```

由以上代码的最后两行可以看出来，基础数据类型的值是没有方法可以调用的；但是代表这些值的变量却可以。其原因是因为JS对应这三种基础数据类型有三种已经内建好的标准对象类型：String，Number，和Boolean，并且给这三种对象类型提供了一些最常用的方法以简化程序员的工作。为了把这样的好处也带给相应的基础数据类型的变量，JS引擎遇到`price.toFixed(2)`这样的调用方法时，其实执行了类似下面的代码：

```
    let __price__ = new Number(price);      //  构建一个 Number 对象
    return __price__.toFixed(2);
```

所以一个基础数据类型变量可以使用的方法，其实就是它对应的内建对象的方法，这些方法都可以在JS的文档中查到。关于这些标准内建对象（standard built-in objects，也有一个更容易混淆的名字叫全局对象 global objects），我们后面还会讲到。
## 引用数据类型：初步认识对象
引用数据类型可以描述为用一个地址索引指向的对象。注意这里我用了“对象”（object）这个词，是因为在JavaScript里**引用数据类型和对象其实就是一回事**。换一种说法，所有不是基础类型的数据结构都是对象。像函数、数组这样特定类型的对象除了名字特殊外，当然也有一些普通对象没有的特点。但是引用数据类型作为一个对象的本质它们都有。在本书里我们会经常用“对象”来表达“引用数据类型”的概念。

引用数据类型的变量很像C语言的指针或者reference，你可以将其理解为里面存储的是指向对象的地址。但是不同于C的指针，你是无法读取这个地址的绝对值的（实际上你也不需要）。每次你读取一个变量，如果变量里存的是基础数据类型，JS引擎就把这个数值返回给你；如果变量里存的是对象的地址，JS引擎就自动按地址取得对象送给你。以下的代码有助于你理解这些概念：

```
    let a = { 'count:': 5 };    // a 里存了指向一个对象的地址
    let b = a;                  // b 被赋值为 a，现在 b 里存储的是和 a 一样的地址，所以 b 也指向了同一个对象
    a.count = 9;                // 通过 a 改变对象里的一个值
    console.log(b.count);       // 9
```

对比于

```
    let a = 5;
    let b = a;
    a = 9;
    console.log(b);             // 5
```

就可以看出来JS对于不同数据类型存取的不同方法。

对象内部对数值的存储可以粗略地看作是一张哈希表，这张表的键必须是字符串，而对应的值可以是任何基础数据或者另外一个对象（包括函数、数组等等）。

在JavaScript里，“对象（object）”一词其实有两层含义。狭义的内涵是指我们用花括弧标识的若干对 key-value 值的这种数据类型，在英文里第一个字母大些（Object）。而当我们说“所有的引用数据类型都是对象”的时候，这里的“对象”是取其广义，是指狭义的对象数据类型加上其它各种内建的对象类型，即便这些内建类型不一定是被括弧包括的 k-v 对，比如函数、函数、Map、Set等等。
### 构建对象（Object）
构建一个狭义的对象，我们通常直接赋值就可以：

```
    let laowang = {
        'name': '老王',
        'age': 35,
        'relationship': '邻居'
    }

    // 或者，如果你事先不知道对象所需的键
    let person = {};                    // 先创建一个空对象
    ...
    person.name = "老张";               // 以后再填充内容
    person.age = 24;
    person.relation = 'undefined';
```

比较少见的另一个形式是使用对象的构建函数：

```
    let obj1 = new Object();
    obj1.'id' = 10029;
```

 二者的效果完全一样。第二种方式在我们讲到构建函数的时候还会有深入了解。

### 常见的标准内建对象
除了Object这种对象外，JavaScript还定义了很多标准的对象类型。最常用的有以下几个
* Array：数组，一组有序、可以索引的值
* Set: 集合，无索引、无序的一组值
* Map: 无序的 k-v 值
* Date: 日期和时间
* Function: 函数，将会在下一章详细描述
* RegExp: 正则表达式
* Error: 通用运行错误。JavaScript还定义了几个专用的错误类型，比如数据类型错误（[TypeError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError)）
* WeakMap 和 WeakSet: 特殊的Map和Set，在某些情况下对管理内存有重要作用

另外，还有三个我们已经见过的基础数据类型对应的对象，也有人把他们称为基础数据类型的 wrappers
* Number
* String
* Boolean

JavaScript对每个标准对象都提供了相应的自带方法，可以简化很多相关的操作。当然，这些标准对象对Object对象提供的方法，比如`toString()`，也是支持的。
### 对象成员的使用
不论是读写对象的数值成员，还是调用对象的成员函数，都有两种方法。第一种是在对象和成员的键之间用`.`隔开，比如

```
    let person = {
        name: "Obama",
        job: "retired"
    }

    let name = person.name;
```

第二种方法是用方括号括住键值，比如

```
    let name = person["name"];
    var array = [];
    array["push"](100);     // now array is [100]
```

这里需要注意的是，如果用第二种方法，键值必须用单引号或者双引号标识为字符串，而第一种不用。另外，虽然第二种方法看起来有些奇怪（因为很多其它语言不支持这样的语法），但是它有一个重要的作用：方括号里可以是一个变量，按运行时的情况赋值。比如

```
    let wantName = true;
    let key = wantName ? "name" : "job";
    console.log(person[key]);
```

### 对象成员的增减
通过之前的例子，我们已经看到如何容易地增加一个对象的成员。减少一个成员可以使用`delete`运算符

```
    let person2 = {
        name: "Josh",
        dept: "Math",
        advisor: "Prof. Obama"
    }
    delete person2.advisor;
```
 
如果多余的成员变量并没有占用很大内存空间，通常并没用必要经常去除它们。一旦去除只后再试图读写这个成员变量，反倒会导致运行错误。

在很多情况下你定义好了对象的成员，并不希望其他使用者去增减它们。如何做到这一点，我们会在第四章讲述。

## 释放对象使用的内存
JavaScript是自带垃圾回收的，所以通常你并不需要考虑内存占用问题。但是如果你使用的对象使用了很大内存，还是会导致程序运行速度降低。JavaScript没有命令去释放一个对象占据的存储空间。但是当一个对象没有变量指向它时，它就会及时被JavaScript的引擎释放。你需要做的仅仅是把变量指向`null`

```
    let obj = new Object{};
    obj.data = "something really big";
    // 处理 obj 相关的逻辑
    
    obj = null;     // 之后对象占用的空间会被释放
```

## 识别变量的数据类型
众所周知JavaScript在声明一个变量的时候并不定义其类型，其类型取决于它在运行时所指向数值的类型。所以在运行状态下有时候我们需要识别一个变量的数据类型。一个非常明显的应用场景是实现面向对象编程的多态性：JavaScript可以容易地用同一个函数，根据输入参数的类型和数量，应用不同的逻辑进行处理。但是应该说JavaScript识别变量类型的方法是比较混乱的，有以下几种情况：

首先，是我们已经见过的`typeof`运算符: 可以正确识别除了`null`之外的各种基础数据类型和function，但是所有其它引用数据类型全部返回`object`。虽然这也不能算错，但是没什么用，毕竟一个 Array 和一个 Error 差很远。比如

```
    let num = 99;
    console.log(typeof num);        // 'number'
    let str = "99";
    console.log(typeof str);        // 'string'
    let bl = true;
    console.log(typeof bl);         // 'boolean'
    let foo = function() {return "happy";};
    console.log(typeof f);          // 'function'
    let obj = {};
    console.log(typeof obj);        // 'object'
    let arr = [9, 8, 7];
    console.log(typeof arr);        // 'object'
    let err = new Error("Something Wrong!");
    console.log(typeof err);        // 'object'
```

其次，针对数组，Array类型提供了一个方法 .isArray()

```
    // 继续上面的代码
    console.log(Array.isArray(arr));    // 'true'
    console.log(Array.isArray(obj));    // 'false'
    console.log(Array.isArray(err));    // 'false'
```

第三，`instanceof`运算符可以识别对象类型，但是注意它对一个对象应用于其父类也返回`true`

```
    // 继续上面的代码
    console.log(arr instanceof Array);      // 'true'
    console.log(err instanceof Error);      // 'true'
    console.log(foo instanceof Function);   // 'true'
    console.log(arr instanceof Object);     // 'true'
```

最后，可以查询对象的constructor，比如

```
    // 继续上面的代码
    console.log(arr.constructor === Array);     // 'true'
    console.log(arr.constructor === Object);    // 'false'
```

以上最后两种方法不仅适用于标准内建对象，也适用于使用自定义的构建函数生成的对象。我们会在第五章专门讲述构建函数。

# 3. 函数（Function）
正如我们前面提到的，在JavaScript里函数也是一种对象。这跟其它很多语言的语法都不一样，但是我倒是觉得JavaScript的这种处理方法并不难理解。

比如在C++语言里，一个函数也是放在一块内存里的一段程序，而你的代码里的函数名就是指向这块内存的地址。所以本质上来说，你使用函数也是在读取一个引用数据类型。只不过跟指向一个对象的地址不同的是，使用函数的名字能做的事情很有限，通常就是调用那个函数。

而在JavaScript里，除了调用这个函数，你还可以把它当对象来使用（所以函数在JavaScript里也被称作“一等公民”，可以跟其它数据类型并列）。把它当作一个对象类型的参数输入另一个函数（比如作为回调函数），或者把它当作另一个函数的结果输出，在JavaScript代码里都很常见。在本章里我们会看到这样的例子。

既然函数不是普通的对象，那必定有它特别之处。表象之下，关键是它比其它对象类型多了一个内部特征值（internal property）叫做`[[Call]]`。内部特征值被JavaScript语言定义于标准对象类型上，它们无法被我们的代码读写，但是JavaScript引擎会使用它们来正确处理不同种类的对象。内部特征值通常用双方括号`[[ ]]`标识，今后我们还会见到。而`[[Call]]`这个特征值只有函数这种对象才有，比如JavaScript引擎的`typeof`运算符就是靠检查这个特征值来正确识别`function`这个类型的。当然有个这个特征值，JavaScript引擎也就可以调用这个函数了。
## 函数的四种定义方式
使用函数之前当然首先要定义它。定义函数有三种方式：函数声明（function declaration）、函数表达式（function expression）、使用Function构建函数和箭头函数（arrow function）。下面依次说明。
### 函数声明
函数声明是我们最常见到的函数定义形式。
```
    function add(n1, n2) {
      return n1 + n2;
    }
```
它的特点是***语句以关键字`function`开头***，后面加函数名和参数。函数的代码放在花括弧里。
### 函数表达式
函数表达式的形式和其它赋值语句一样，都是以等号为关键字，等号左边是变量名（也就是函数名），右边是你要定义的函数。
```
    let sub = function (n1, n2) {
      return n1 - n2;
    }
```
显然函数表达式要多输入几个字符，所以函数声明稍微更常见一些。
### Function 构建函数
在JavaScript里，`Function`也是一个标准内建对象，也可以使用`new`（第五章会介绍）来构建一个函数。严格来说这是函数表达式的一个特例。比如
```
    var sayHi = new Function("console.log(\"Hi!\");");
    sayHi();    // 'Hi!'
```
这种方式是把全部函数内容的语句当作一整个字符串，作为输入参数传给`Function()`这个构建函数。显然这种办法既难写又难读，更重要的是难以debug。所以读者知道一下就好，一般不会用到。

只有在一种情况下这个定义方式才显示出来其强大的、很多其它语言没有的功能：因为函数的内容就是个普通的字符串，你可以在运行时根据当时的条件、按照JavaScript语法组合一个你需要的字符串，然后用这个方法动态地生成一个新的函数。
### 函数表达式和函数声明的异同
以上两种定义方式定义的函数，在使用上基本是一样的。二者最大的区别是 ***函数声明会被置顶（hoisted），而函数表达式不会*** 。比如
```
    var sum = function(3, 5);
    console.log(sum);           // 5
    var result = sub(9, 4);     // type error

    // 函数的定义在调用之后
    function add(n1, n2) {
      return n1 + n2;
    }

    var sub = function (n1, n2) {
      return n1 - n2;
    }
```
在上面的代码中，变量`add`的声明和内容（也就是它指向的函数）都被JavaScript置顶，所以你在任何时候都可以调用它。而变量`sub`的声明虽然也会被置顶（因为它由`var`定义，如果由`let`定义则不会），但是它的赋值语句的位置并没有提前。所以在第三行调用它的时候，它的值还是`undefined`。回忆我们前面讲过的，JavaScript引擎靠检查对象是否有`[[Call]]`这个内部特征值决定其是否是函数。`undefined`显然没有`[[Call]]`，所以会产生`TypeError`。

这两种方式的有个相同点常常被忽视：不论怎样定义，你定义的函数名其实就是个普通的变量。你还可以给它赋其它值；当然如果它得到的新值不是函数，你就不可以调用它了。比如以下的语句：
```
    // 接上面的代码
    add = 100;      // 没问题
    sub = 0;        // 没问题
    add(1, 2);      // type error
```
另外，执行以上语句之后，原先定义的函数就再没有变量指向它们了。别忘了函数也是对象，所以这些函数就有可能被 garbage collector 从内存里抹去了。
### 箭头函数（Arrow Function)
箭头函数是ECMAScript 2015（ES6）引入的一个新的函数定义方式。它的格式在视觉效果上很直观：
* 用等号加大于号（`=>`）分割左边的输入参数和右边的函数内容，看上去很像是从左边的输入得到右边的输出
* 输入参数用圆括号括住，跟一般的函数定义一样。但是在有且只有一个的时候，可以直接写变量名而省去圆括号和关键字`return`。
* 函数内容这边也是用花括弧括住。但是在有且只有一句代码的情况下，可以省去花括弧。
* 箭头函数因为被大量用作回调函数（callback），所以通常都没有名字，当然你也可以给它用函数表达式命名。
下面咱们看几个例子
```
    setCallback1((err, result) => {       // popular pattern in NodeJS
      if (err) {
        console.error(err.message);
      } else {
        console.log(result)
      }
    });

    setCallback2( result => result * 2);
    // same as setCallback2((result) => { return result * 2;});

    setCallback3(() => { console.log('done!'); });   // 无输入参数时需要括号！

    setCallback4(result => {});   // 函数无内容时也需要花括弧！

    let sum = (number1, number 2) => number1 + number2; // 给函数命名并且省略花括弧和‘return’
```

我个人很喜欢在回调函数上使用箭头函数--除了写法更简洁，还有下面会讲到的好处。但是我并不非常鼓励省略圆括号或者花括弧这种简写。仅仅少敲了两个字符是好的，但是会降低代码可读性。尤其对新人来说，看到

```
    fs.access('./secret', err => fileReady = err ? false : true);
```
这种语句还要花更多一点儿功夫来断句。
### 箭头函数的特点
箭头函数除了长得样子不同于一般函数之外，还有几个重要的特点。

首先，最重要的一条就是在箭头函数范围内的`this`是由此函数定义在语句中的位置决定，而不是被调用时的场景决定（对`this`的使用不是很清楚的读者，我建议你一定要再去弄清楚）。比如当此箭头函数是一个对象里某句函数调用的回调函数时，箭头函数里的`this`总是指向这个对象。这样你写代码的时候就不会因为忘了`.bind(this)`而出错。比如
```
    // 使用箭头函数
    function CheckMyFile(filename) {
      let this.fileReady;
      fs.access(filename, (err) => {
        this.fileReady = err ？false : true;      // 不论何处被调用，这个this永远指向CheckMyFile
        }
      });
    }

    // 相比较使用传统回调函数
    function CheckMyFile(filename) {
      let this.fileReady;
      fs.access(filename, function (err) {
        this.fileReady = err ? false :true;
        }
      }.bind(this))；      // 很多人常常忘了 .bind(this)而在运行时出错--更可怕的是运行时不报错！
    };
```

其次，箭头函数里不能使用我们下面马上就要讲到的`arguments`这个标准对象。

第三，箭头函数不能做构建函数，也就是不能用关键字`new`来调用。

第四，跟以上三条密切相关的，箭头函数不能另外绑定`this`、`arguments`、`super`和`new.target`。另外，因为不能改变`this`，箭头函数不能（也不需要）使用`bind()`方法；使用`call()`和`apply()`的时候，第一个输入参数的`this`也会被忽略掉。

第五，箭头函数没有原型（prototype）。反正它不能用来做构建函数，所以这条没什么关系。

最后，显而易见地，你不能给两个或更多的输入参数赋予重复的变量名。

如果上面提到的`new`、构建函数、原型等概念你还不是很清楚，请耐心读下去。

箭头函数这些特点除了可以减少开发者的错误之外，还可以帮助JavaScript引擎进一步优化执行效率，不过这就不在本书的讨论范围之内了。总之，如果你对箭头函数不熟悉，那你应该在回调函数里试着用起来。比如对数组进行倒排序：
```
    let arr = [5, 8, 2, 7];
    let sorted = arr.sort((a, b) => b - a);
```
### generator
ES6 还引入了一种新的特殊函数，generator，用 `function*`来定义。我认为它更适合放在类似于“ES6新功能”或者“JavaScript Async 编程”这样的书里跟Promise一起介绍比较好。希望我以后有机会再写吧。
## 输入参数
### 输入参数的随意性
不论是定义还是调用函数，函数名之后立刻就是输入参数了。跟很多语言不同，JavaScript引擎并不严格检查参数的类型、个数，但是参数的次序还是重要的。比如
```
    function multiply(first, second) {
      if (typeof second ===  'number') {   // 严格来说，此处还应该检查是正整数
        switch (typeof first) {
          case 'number':
            return first * second;    //  如果两个输入参数都是数字则做乘法

          case 'string':
            return first.repeat(second);  // 如果第一个参数是字符串则复制

          default:
            return null;
        }
      } else {
        throw TypeError('Second parameter must be positive integer');
      }
    }

    console.log(multiply(5, 6));         // 30
    console.log(multiply('ok', 3));      // 'okokok'
    console.log(multiply('ok', 3, 5));   // 'okokok'，最后一个5被无视
    console.log(multiply(true, 3));      // 'null'
    console.log(multiply(100));          // TypeError
```
从以上的代码可以看出，你调用一个函数的时候，可以给少于或者多于函数定义里的参数个数，也可以给任何类型的参数，JavaScript引擎并不会因为这样的调用本身而报错。当然函数里面的代码在使用这些参数的时候，还是可能因为类型不对而出错。比如
```
    function doubleAndCallback(value, callback) {
      console.log('Input value is', value);
      callback(value * 2);
    }

    doubleAndCallback(8, 5);      // TypeError; 5 不能作为函数被调用
```
这种输入参数的随意性有好有坏。
* 最明显的好处是你不需要再定义很多个名字相同、参数不同的函数来实现多态性，一个函数就全部搞定了。
* 今后再增加参数以扩展函数功能的时候，已经调用这个函数的代码也不需要修改，节省了开发成本。
* 因为输入参数可以少于函数定义里的参数，你定义函数的时候可以（也应该）把必须的参数放在前边，把可选的参数放在后边。这样调用者可以更方便的根据自己的需要来决定。

坏处是
* 因为没有编译器帮你做数据类型检查，你的函数定义内部往往要检查一下这些参数是否存在及其类型，尤其是在你不确定谁会怎样调用你的函数的情况下。否则很容易出运行错误。
* 在支持多种输入数据类型的情况下，你要更加小心。比如仅仅对输入参数是数字的时候，你希望调整一下函数的行为。但是因为输入参数是字符串的时候也是调用同一个函数，你要避免被影响到。在C++这种使用 function overloading 的语言，就没有这样的顾虑。

其实你可能已经注意到了，JavaScript自带的运算符和函数里已经大量使用了这种随意性，比如
```
    let a = 8 + 9;      // 17
    a = 8 + '9';        // '89'
    console.log(10);    // '10'
    console.log('10');  // '10'
```
所以不论你喜欢不喜欢这种随意性，它都是JavaScript不可避免的一部分，你还是要熟练地掌握它。

## arguments 对象
除了箭头函数之外，其它“普通”的函数域范围内，除了调用的时候输入的参数之外，还有一个隐含的输入参数：`arguments`。你可以这样理解`arguments`：JavaScript引擎把所有调用一个函数时输入的参数（不管函数定义里有没有、有几个参数）按顺序排好，放到这个对象里。它对每个输入参数的排序很像是标准数组对象里每个成员的排序，而JavaScript也给它加上一些数组对象同名的方法让你使用这些参数。但是`arguments`不是标准数组对象，当然我们可以轻易地把它变成数组：

```
    function something() {
      console.log(typeof arguments);    // 'object'
      console.log(Array.isArray(arguments););   // 'false'
      let arr = [...arguments];         // arr is an Array
    }
```

JavaScript提供这样一个对象的目的很简单：如果你的函数需要处理输入对象个数不确定的情况，可以用这个对象查询输入参数的个数、遍历每个输入参数；如果你的函数没有这种需要，那你使用函数定义里的输入参数变量名就够了。

`arguments`对象提供的最重要的两个用法是`.length`和方括号索引。比如我们要计算任意个数值的和：
```
    function sumOfAll() {
      var result = 0,
          i = 0,
          len = arguments.length;

      while(i < len) {
        result += arguments[i];
        i++;
      }

      return result;
    }

    console.log(sumOfAll(5, 6, 7, 8));  // 26
    console.log(sumOfAll());            // 0
```
如果此函数定义了输入参数名，那这些输入参数其实就是按次序排列的`arguments`的元素。比如
```
  function twoParam(first, second) {
    console.log((first === arguments[0]) + ' and ' + (second === arguments[1]));
  }

  twoParam('hello', 9);   // 'true and true'
  twoParam(100);   // 'true and true'; second 和 arguments[1] 都是 undefined
```
### 函数的 length
顺便提一下，函数作为一个对象，它也有`length`这个特征值，等于函数定义里的参数个数(对比于`arguments.length`是函数被调用时实际传入的参数个数)。比如
```
    // 接上面两段代码
    console.log(sumOfAll.length);   // 0
    console.log(twoParam.length);   // 2
```
不过函数的`length`好像没什么用，至少我目前还没有机会真的在产品代码里使用。
## 函数重载（overloading）
前面已经提到过JavaScript的函数重载，这里再多啰嗦两句。看看以下的代码
```
    function sayMsg(message) {
      console.log(message);
    }

    function sayMsg() {
      console.log('Have a nice day!')
    }

    sayMsg();           // Have a nice day!
    sayMsg('Hello');    // Have a nice day!
```
这段代码运行不会出错，但是最后一句并没有像其它面向对象语言的函数重载那样去调用第一个函数，而是也运行了第二个函数。这是因为第二个函数定义的时候，因为与第一个重名（即便参数列表不同），已经把第一个函数覆盖了----第一个函数再也无法被调用了。而第二个并不在乎是否有输入参数。

所以，在JavaScript里需要判断输入参数的个数、类型等来实现函数重载的效果：
```
    function sayMsg(message) {
      if(!message) {                  // 或者 if(arguments.length === 0) {
        message = 'Have a nice day!'
      }
      console.log(message);
    }

    sayMsg();           // Have a nice day!
    sayMsg('Hello');    // Hello
```
## 作为对象方法（method）的函数
我们已经知道一个对象可以有任意个特征值（properties），而每一个特征值都可以是基础数据类型或者另一个对象。而函数这种对象在JavaScript里是“一等公民”，当然也可以做对象的特征值。作为另一个对象特征值的函数被叫做这个对象的方法（method）。这种叫法跟其它面向对象语言一致，很容易理解。但是反之，函数作为对象也有它自己的特征值、有它自己的方法，这与很多语言不一样，我们下面也会遇到。
### 定义方法
定义一个对象的方法跟定义其它类似的对象特征没什么不同，唯一区别就是分号“:”后面跟着的是函数定义：
```
    var person = {
      name: "老王",
      sayName: function) {
        console.log(person.name);
      }
    }

    person.sayName();   // 老王

    person.sayAgain = function() {        // 也可以动态增减对象的方法
      console.log(person.name.repeat(2));
    }

    person.sayAgain();  // 老王老王
```
以上的代码虽然工作正常，但是有个很大的问题：每当方法需要用到对象的其它特征值的时候，必须前缀对象的名字`person.`。如果以后对象的名字改了、被复制了或者用其它方法生成新的对象，这些方法就都不工作了。⚠️ 如果不加这个前缀，这些方法就更不能工作了，因为它们找不到一个叫“name”的变量，而JavaScript并不会因为这是某个对象的方法就自动把这个对象的同名特征值拿过来用。

这个时候就要用到`this`这个关键字了。完全讲解`this`的概念大概需要另外一本书，本节只能讲解在方法里怎么使用它。
### `this`对象
首先，`this`也是一个对象（所以你才可以使用`this.name`）。在一个函数内部，它就是那个调用此函数的对象，是在被调用的时候动态决定的（箭头函数除外----如果你已经忘了，请翻回去复习）。简略地讲，常见的`this`有这么几种情况：
* 如果一个非方法的全局函数被调用，它的`this`就是“全局对象”。问题在于这个全局对象并不一定是什么；比如在一个浏览器里它通常是`window`对象，显然在NodeJS里就不是。所以除非你很清楚知道为什么要在全局函数里使用`this`，就不要用。
* 作为对象的方法，通常是这样被调用的：`object.function(parameters)`。这时`function`里的`this`就是前面的`object`，所以可以用`this.`来获取这个对象的任何特征值。这是方法里很常见的使用。
* 在回调函数（callback）里，既然是回调函数，调用这个函数的对象就往往不是你能控制甚至你能知道的。在这种情况下，回调函数里的`this`几乎是不能用的。但是作为方法里的回调函数，你又往往要读写此对象的特征值（比如更新一个对象内部的状态）。这时候你可以使用箭头函数（在箭头函数的章节有例子），也可以使用下面的几个工具。
* 使用`call()`、`apply()`或者`bind()`设定`this`。这是我们下节要讲的内容。

### 设定`this`
如前所述，有时候你需要设定一个函数的`this`对象。虽然你不能直接写`function.this = ...`，但是JavaScript提供了很方便的几个方法（正好温习一下，函数也是对象，也有自己的方法）。
#### `call()`方法
函数本来就是被用来调用的，居然它还有一个`call`方法是有点儿奇怪的。我们可以这样理解：如果是简单的调用，你不需要用这个方法；如果你特意用了`call`，那就是要更“高级”地使用这个函数了----这个高级之处，就是设定函数的`this`。为了区分这个函数和它所拥有的`call`方法（也是个函数），我们称这个函数为父函数。

`call`的使用不复杂：因为它是父函数的方法，它要被用`.`加在父函数名后面；因为它自己也是函数，它名子后面要加括号和参数。它的第一个参数永远是父函数所需要的`this`所指的对象，其它参数会被完整地送给父函数。比如
```
    function sayName(label) {
      console.log(`In ${label} my name is ${this.name}`);
    }

    var p1 = { name: 'Jack Ma' };
    var p2 = { name: 'YT Jia' };

    let name = 'Obama';

    sayName.call(p1, 'person1');    // In person1 my name is Jack Ma
    sayName.call(p2, 'person2');    // In person2 my name is YT Jia

    sayName('global');              // In global my name is result (Chrome)
                                    // In global my name is undefined (NodeJS)
```
以上代码执行到`sayName.call(p1, 'person1');`这句话时，你可以想象成JavaScript引擎先把`sayName`函数里所有的`this`用`call`的第一个参数（也就是p1）代替，然后把第二个参数（‘person1’）传给这个新的`sayName`，让它执行。

另外，最后一句话是在最新的Chrome浏览器和NodeJS 6.x LTS里执行的结果。你可以看到‘Obama’并没有被`this.name`找到。这是因为把`this`指向全局太危险----设想你在离这句话很远的地方有个变量叫‘name’，你可能无意间就把那个变量的值改变了，也没有任何报错。所以新的JavaScript引擎已经不给你设定全局为`this`，你代码的错误更容易在开发阶段就被发现。

# 4. 对象：深入了解



# 5. 构建函数（Constructor）和原型（Prototype）

# 6. 继承（Inheritance）

# 7. 类（Class）

# 8. Proxy和Reflection API

# 9. 编程攻略
