#  第6章 面向对象的程序设计([返回首页](https://github.com/qianjilou/javascript3))

**本章内容** 

- [ ] 理解对象属性
- [ ] 理解并创建对象
- [ ] 理解继承  

向对象（Object-Oriented, OO)的语言有一个标志，那就是它们都有类的概念，而通过类可
以创建任意多个具有相同属性和方法的对象。前面提到过，ECMAScript中没有类的概念，因
此它的对象也与基于类的语言中的对象有所不同。
ECMA-262把对象定义为:“无序属性的集合，其属性可以包含基本值、对象或者闲数。”严格来讲,
这就相岀丁•说对象是一组没有特定顺序的值。对象的每个属件或方法都有一个名字，而每个名字都映射
到一个值。正因为这样（以及其他将要讨论的原因），我们可以把ECMAScript的对象想象成散列表:无
非就是一组名值对，其中值可以是数据或函数。
每个对象都是基于一个引用类塑创建的，这个引用类铟可以是第5章讨论的原生类型，也可以是开
发人员定义的类型。  

##  6.1 理解对象
上一章曾经介绍过，创建自定义对象的最简单方式就是创建一个Object的实例，然后再为它添加 属性和方法，如下所示。
```javascript
    var person = new Object();
    person.name = "Nicholas";
    person.age = 29;
    person.job = "Software Engineer";
    
    person.sayName = function(){
        alert(this.name);
    };
```
上面的例子创建了--个名为person的对象，并为它添加jT三个属性（name、age和job )和一个 方法（sayName U〉。其中，sayName U方法用于显示l:hi s. name (将被解析为person • name )的值。 早期的JavaScript开发人员经常使用这个模式创建新对象。几年后，对象字面ft成为创建这种对象的首选 模式。前面的例子用对象字卤量语法可以写成这样:
```javascript
var person = {
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",

    sayName: function(){ 
        alert(this.name);
    }
};
```
这个例子中的person对象与前面例子中的person对象是一样的，都有相同的属性和方法。这些 属性在创建时都带有一些特征值（characteristic), •JavaScript通过这些特征值来定义它们的行为。 

###  6.1.1 属性类型
ECMA-262第5版在定义只有内部才用的特性（attribute )时，描述了属性（property )的各种特征。 ECMA-262定义这邱特性是为了实现JavaScript引擎用的，因此在JavaScript屮不能直接访问它们。为丫 表示特性是内部值，该规范把它们放在了两对儿方括号中，例如[[Enumerable]】。尽管ECMA-262 第3版的定义有些不同，伹本书只参考第5版的描述。
ECMAScript屮有两种成性:数据属性和访问器属性。  

**1.数据属性**  

数据属性包含-•个数据值的位置。在这个位置吋以读取和写入值。数据庙性有4个描述其行为的 特件。
- [ ] [[Configurable]]:表示能否通过delete删除属性从而屯新定义属性，能否修改属性的特 性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的 这个特性默认值为true。
- [ ] [[Enumerable]]:表不能否通过for-in循环返冋属性。像前面例子中那样直接在对象上定 义的属性，它们的这个特性默认值为true。
- [ ] [[writable]]:表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的 这个特性默认值为true。
- [ ] [[Value]]:包含这个属性的数据值。读取属性值的时候，从这个位贵读;写人属性值的时候， 把新值保存在这个位置。这个特性的默认值为uiKief ined。  

对于像前面例T中那样.ft接在对象上定义的属性，它们的[[Configurable] ]、[ [Enumerable]] 和[[Writable]]特性都被设置为true,而[[Value】]特性被设置为指定的值。例如:
```
var person = {
    name: "Nicholas"
};
```
这里创建了一个名为name的《性，为它指定的值是"Nicholas•。也就是说，[[Value]]特性将 被设置为"Nicholas”，而对这个值的任何修改都将反映在这个位置。
要修改属性默认的特性，必须使用ECMASclipt5的0bject.definePropert:y(}方法D这个方法 接收=•个参数:厲性所在的对象、属性的名字和一个描述符对象。其中，描述符（descriptor)对象的属 性必须是:configurable、enumerable、writable和value。设置其中的一或多个值，可以修改 对应的特性值。例如:
```javascript
var person = {};
Object.defineProperty(person, "name", {
    writable: false,
    value: "Nicholas"
});

alert(person.name);
person.name = "Michael";//"Nicholas"
alert(person.name);//"Nicholas"
```
这个例子创建了一个名为name的属性，它的值"Nicholas"是只读的。这个属性的值是不可修改 的，如果尝试为它指定新值，则在非严格模式下，赋值操作将被忽略;在严格模式下，赋值操作将会导致抛出错误。
类似的规则也适用于不可配置的属性。例如:
```javascript
var person = {};
Object.defineProperty(person, "name", {
    configurable: false,
    value: "Nicholas"
});

alert(person.name);//"Nicholas"
delete person.name;
alert(person.name);//"Nicholas"
```
把configurable设置为false，表示不能从对象中删除属性。如果对这个属性调用delete,则 在非严格模式下什么也不会发生，而在严格模式下会导致错误。而凡，一旦把属性定义为不可配置的， 就不能再把它变回可配置此时，再调用Object.defineProperty()方法修改除writable之外 的特性，都会导致错误:
```javascript
var person = {};
Object.defineProperty(person, "name", {
    configurable: false,
    value: "Nicholas"
});

//抛出错误
Object.defineProperty(person, "name", {
    configurable: true,
    value: "Nicholas"
});
```
也就是说，可以多次调用Object. defineProperty ()方法修改同一个属性,但在把configurable 特性设置为false之后就会有限制/。
在调用 Object.defineProperty()方法时，如果不指定，configurable、enumerable 和 writable特性的默认值都是false。多数情况下，可能都没有必要利用object.defineProperty 方法提供的这些高级功能。不过，理解这些概念对理解JavaScript对象却非常有用。

---

(MIE8是第一个实现Object.defineProperty()方法的浏览器版本。然而，这个 版本的实现存在诸多限制:只能在DOM对象上使用这个方法，而且只能创建访问器 属性。由于实现不彻底，建议读者不要在IE8中使用Object.defineProperty() 方法。

---

**2.访问器属性**  

访问器属性不包含数据值;它们包含一对儿getter和setter函数（不过，这两个阐数都不是必需的)。 在读取访问器属性时，会调用getter函数，这个闲数负责返冋有效的值;在写人访问器屈性时，会调用 setter函数并传人新值，这个函数负责决定如何处理数据。访问器属性有如下4个特性。
- [ ] [[Configurable]]:表示能否通过delete删除属性从而重新定义属性 } 能否修改属性的特 性，或者能杏把W性修改为数据属件。对TK接在对象上定义的屑性，这个特性的默认值为 true.
- [ ] [[Enumerable]]:表示能许通过for-in循环返问属性。对于直接在对象上定义的属性，这 个特性的默认值为true。
- [ ] [[Get]]:在读取属性时调用的闲数。默认值为undefined。
- [ ] [[Set]]:在写人M性时调用的減数。默认值为undefined。  

访问器属性不能直接定义，必须使用Object.defineProperty{)来定义。请看下面的例子。
```javascript
var book = {
    _year: 2004,
        edition: 1
    };
      
Object.defineProperty(book, "year", {
    get: function(){
        return this._year;
    },
    set: function(newValue){
    
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        
        }
    }
});
    
book.year = 2005;
alert(book.edition);   //2
```
以上代码创建一个 book对象，并给仑定义两个默认的属性:_year和edition。_year前面 的下划线兹-种常用的记号，用丁•表示只能通过对象方法访问的属性。而访问器厲性year则包含一个 getter闲数和--个setter函数。getter 1#数SW_year的值，setter函数通过计算来确定正确的版本。因此， 把year属性修改为2005会#致」^肛变成2005,而edition变为2。这是使用访问器属性的常见方 式，即设宵一个属性的位会导致其他属性发生变化。
不一定非要同时指定getter和setter。只指定getter意味着属性足•不能写，尝试写人属性会被忽略。 存:严格模式下，尝试写人只指定丫 getter函数的属性会抛出错误。类似地，没有指定settei•函数的属性 也不能读，杏则在非严格模式下会返回undefined,而在严格模式下会抛出错误。
支持ECMAScript5的这个方法的浏览器有IE9+(IE8只是部分实现）、Firefox4+、Safari5+、Opera 12+和Chrome。在这个方法之前，要创建访问器屈性，一般都使用两个非标准的方法: _def ineGetter_(} 和_def ineSetter_<)。这两个方法最初是由 Firefox 引人的，后来 Safari 3、 Chrome 1和Opera9.5也给出了相同的实现。使用这两个遗留的方法，可以像下面这样重写前面的例子。
```javascript
var book = {
    _year: 2004,
    edition: 1
};
  
//定义访问器的旧有方法
book.__defineGetter__("year", function(){
    return this._year;    
});

book.__defineSetter__("year", function(newValue){
    if (newValue > 2004) {
        this._year = newValue;
        this.edition += newValue - 2004;
    }    
});

book.year = 2005;
alert(book.edition);   //2
```
在不支持Object.definePropertyU方法的浏览器中不能修改[[Configurable]]和
[[Enumerable]]。  

###  6.1.2 定义多个属性
由于为对象定义多个属性的能性很大，ECMAScript 5又定义了一个Object.definePro- pertiesO方法。利用这个方法可以通过描述符一次定义多个属性。这个方法接收两个对象参数:第一 个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对 应。例如:
```javascript
var book = {};

Object.defineProperties(book, {
    _year: {
        value: 2004
    },
    
    edition: {
        value: 1
    },
    
    year: {            
        get: function(){
            return this._year;
        },
        
        set: function(newValue){
            if (newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }                  
        }            
    }        
});
   
book.year = 2005;
alert(book.edition);   //2
```
以I:代码在book对外！:定义了两个数据属性（jear和edition)和一个访问器属性（year)。 最终的对象与上一节中定义的对象相同。唯一的K别是这里的属性都足在同一时间创建的。
支持 Object.defineProperties()方法的浏览器有 IE9+、Firefox4+、Safari 5+、Opera 12+和 Chrome。  

###  6.1.3 读取属性的特性
使用ECMAScript 5的Objects getOwnPropertyDescriptor (}方法，町以取得给定涵性的描述 符。这个方法接收两个参数:属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果 是访问器腐性，这个对象的属性有configurable、enumerable、get和set;如果是数据属性，这 个对象的属性有 configurable、enumerable、writable 和 value。例如:
```javascript
var book = {};

Object.defineProperties(book, {
    _year: {
        value: 2004
    },
    
    edition: {
        value: 1
    },
    
    year: {            
        get: function(){
            return this._year;
        },
        
        set: function(newValue){
            if (newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }                  
        }            
    }        
});
   
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value);          //2004
alert(descriptor.configurable);   //false
alert(typeof descriptor.get);     //"undefined"

var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value);          //undefined
alert(descriptor.enumerable);     //false
alert(typeof descriptor.get);     //"function"
```
对于数据属性_/ear，value等于最初的值，configurable是false，而get等于undefined。 对于访问器属性 year, value 等于 undefined, enumerable 是 false, [fl丨'get:是一个指向 getter 函数的指针。
在JavaScript中，可以针对任何对象   包括DOM和BOM对象，使用Object .getOwnProperty-
Descriptor<)方法。支持这个方法的浏览器有 IE9+、Firefox4+、Safari 5+、Opera 12+和 Chrome。  

##  6.2 创建对象
虽然Object构造函数或对象字曲量都可以用来创建单个对象，但这®方式有个明显的缺点:同 一个接口创建很多对象，会产生大M的重复代码。为解决这个问题，人们开始使用工厂模式的一种变体。  

###  6.2.1 工厂模式
工厂模式是软件T:程领域一种广为人知的设计模式，这种模式抽象了创建具体对象的过程（本书后面还将讨论其他设计模式及其在JavaScript中的实现)。考虑到在ECMAScript中无法创建类，开发人员 就发明了一种函数，用函数来封装以特定接U创建对象的细节，如下面的例子所示。
```javascript
function createPerson(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
函数createPerson (}能够根据接受的参数来构建一个包含所苻必要信息的Person对象。可以无 数次地调用这个函数，而毎次它都会返回一个包含三个屈性一个方法的对象。丨:厂模式虽然解决了创建 多个相似对象的问题，但却没有解决对象识別的问题（即怎样知道一个对象的类型)。随着JavaScript 的发展，乂一个新模式出现了。  

###  6.2.2构造函数模式
前几章介绍过，ECMAScript中的构造函数nj•用来创建特定类型的对象。像0bject "Array这样
的原生构造函数，在运行时会A动出现在执行环境屮。此外，也可以创建自定义的构造函数，从而定义 自定义对象类型的属性和方法。例如，可以使用构造函数模式将前面的例子重写如下。
```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    };    
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```
在这个例子中，Person()闲数取代createPerson(}函数。我们注意到，Person()中的代码 除了与createPerson ()中相同的部分外，还存在以下不同之处:
- [ ] 没省显式地创建对象;
- [ ] It接将属性和方法赋给了 this对象;
- [ ] 没有return语句。  

此外，还应该注意到函数名Person使川的是大写字母P。按照惯例，构造函数始终都应该以一个 大写字母开头，而非构造函数则应该以一个小写字母开头。这个做法借鉴自其他00语言，主要是为了 区别于ECMAScript中的其他函数;因为构造函数本身也是函数，只不过可以用来创建对象而已。
要创建Person的新实例，必须使用new操作符。以这种方式调用构造函数实际上会经历以下4 个步骤:
(1)创建一个新对象;
(2)将构造函数的作用域賦给新对象（因此this就指向了这个新对象);
(3}执行构造函数中的代码（为这个新对象添加属性);
(4}返回新对象。
在前面例子的最personl和person2分别保存着Person的一个不N的实例。这两个对象都 有-个constructor (构造函数）屈性，该M性指向Person,如下所示。
```javascript
alert(person1.constructor == Person);  //true
alert(person2.constructor == Person);  //true
```
对象的constructor属性最初是用来标识对象类型的。但是，提到检测对象类型，还是instan- ceof操作符要更可靠一共。我们在这个例子中创建的所冇对象既是Object的实例，同时也是Person 的实例，这一点通过instanceof操作符可以得到验证。
```javascript
alert(person1 instanceof Object);  //true
alert(person1 instanceof Person);  //true
alert(person2 instanceof Object);  //true
alert(person2 instanceof Person);  //true
```
创建A定义的构造函数意味着将来可以将它的实例标识为一种特定的类型;而这正是构造函数模式 胜过工厂模式的地方。在这个例子中，personl和person2之所以同时是Object的实例，是因为所 冇对象均继承自Object (详细内界稍后讨论)。

---

(以这种方式定义的构造函数是定义在Global对象(在浏览器中是window对象）
一中的。第8章将详细讨论浏览器对象模型（BOM)。

---

**1.将构造函数当作函数**  

构造函数与其他函数的唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是函数，不 存在定义构造函数的特殊语法。任何函数，只要通过new操作符来调用，那它就可以作为构造函数;而 任何函数，如果不通过new操作符来调用，那它跟普通函数也不会有什么两样。例如，前面例子中定义 的Person <)函数可以通过下列任何一种方式来调用。
```javascript
//当作构造函数使用
var person = new Person("Nicholas", 29, "Software Engineer");
person.sayName();   //"Nicholas"

//作为普通函数调用
Person("Greg", 27, "Doctor");  //adds to window
window.sayName();   //"Greg"

//在另外一个对象的作用域中调用
var o = new Object();
Person.call(o, "Kristen", 25, "Nurse");
o.sayName();    //"Kristen"
```
这个例子中的前两行代码展示构造函数的典型用法，即使用new操作符来创建一个新对象。接下 来的两行代码展示了不使用new操作符调用Person ()会出现什么结果:属性和方法都被添加给window 对象了。有读者可能还记得，当在全局作用域中调用一个函数时，this对象总是栺向Global对象（在 浏览器中就是window对象)。因此，在调用完函数之后，可以通过window对象来调用sayName ()方 法，并且还返回了 "Greg•。最后，也可以使用call()(或者applyO )在某个特殊对象的作用域中 调用Person ()函数。这里是在对象〇的作用域中调用的，因此调用后〇就拥有了所有属性和sayName () 方法。  

**2.构造函数的问题**  

构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是每个方法都要在每个 实例上重新创建一遍。在前面的例子中，personl和person2都有一个名为sayName ()的方法，但那 两个方法不是同一个Function的实例。不要:忘了——ECMAScript中的函数是对象，因此每定义一个 函数，也就是实例化了一个对象。从逻辑角度讲，此时的构造函数也可以这样定义。
```
function Person(name, age, job){
    this.name = name;
 this.age = age;
this.job = job;
this.aayNaiae = new Function("alert (this.name)"); // 与声明函数在逻辑上是等价的
}
```
从这个角度上来看构造函数，更容易明甶每个Person实例都包含一个不同的Function实例（以 显示name属性）的本质。说明甶些，以这种方式创建函数，会导致不同的作用域链和标识符解析，佴 创建Function新实例的机制仍然是相同的。因此，不同实例上的问名函数是不相等的，以下代码可以 证明这一点。
```javascript
alert(person1.sayName == person2.sayName); //false
```
然而，创建两个完成同样任务的Function实例的确没有必要;况且有this对象在，根本不用在 执行代码前就把函数綁定到特定对象上面。因此，大可像下面这样，通过把函数定义转移到构造函数外 部来解决这个问题。
```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
}

function sayName(){
    alert(this.name);
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```
在这个例子中，我们把sayName ()函数的定义转移到了构造函数外部。而在构造函数内部，我们 将sayName属性设置成等于全局的sayName函数。这样~來，由于sayName包含的是一个指向闲数® 的指针，因此personl和person2 XJ■象就共享f在全局作用域中定义的同一个sayName<)困数。这 样做确实解决了两个函数做同一件事的问题，可是新问题又来了:在全周作用域中定义的函数实际上只 能被某个对象调用，这让全局作用域有点名不副实。而更让人无法接受的是:如果对象需要定义很多方 法，那么就要定义很多个全局函数，于是我们这个自定义的引用类型就丝毫没有封装性可言了。好在， 这些问题可以通过使用原型模式来解决。  

###  6.2.3 原型模式
我们创建的每个函数都有一个prototype (原型）属性，这个属性是一个指针，指向一个对象， 而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。如果按照字面意思来理解，那 么prototype就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以 让所冇对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是 可以将这些信息直接添加到原型对象中，如下面的例子所示。
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
person1.sayName();   //"Nicholas"

var person2 = new Person();
person2.sayName();   //"Nicholas"

alert(person1.sayName == person2.sayName);  //true
```
在此，我们将sayNameU方法和所办属件立接添加到f Person的prototype属性巾，构造函数 变成了空函数。即使如此，也仍然可以通过调用构造函数来创建新对象，而且新对象还会具釘相同的属 性和方法。似与构造闲数模式不同的是，新对象的这些厲性和方法是由所有实例共亨的。换句话说， personl和person2访问的都是同一组属性和同一个sayName n函数。要理解原型模式的T.作原理， 必须先理解ECMAScript中原型对象的性质。  

**1.理解原型对象**  

无论什么时候，只要创建了-个新函数，就会根据一组特定的规则为该函数创建-个prototype 属性，这个属性指向涵数的原型对象。在默认情况下，所有原型对象都会自动获得-个constructor (构造函数）属性，这个属性包含一个指向prototype属性所在函数的指针。就韋前面的例子来说， Person.prototype. constructor指向Person。而通过这个构造涵数，我们还可继续为原型对象 添加其他屈性和方法。
创建了 f!定义的构造函数之后，其原型对象默认只会取得constructor属性;至于其他方法，则 都是从Object继承而来的。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部 属性），指向构造闲数的原坦对象。ECMA-262第5版中管这个指针叫[[Prototype]]。虽然在脚本中 没有标准的方式访问[[Prototype]],但Fircfox、Safari和Chrome在每个对象h都支持-个属性 _pr〇t〇_;而在其他实现中，这个属性对脚本则楚完全不可见的。不过，要明确的真正重要的一点就 是，这个连接存在丁•实例与构造函数的原型对象之间，而不是存在于实例与构造确数之间。
以前曲'使用Person构造函数和Person.prototype创建实例的代码为例，图6-1展示了各个对 象之间的关系。
阁6-丨展本/ Person构造涵数、Person的原3!属性以及Person现有的两个实例之间的关系。 在此，Person • prototype 指向 了原趣对象，而 Person .prototype • constructor 又指回了 Person。 原型对象中除了包含constructor属性之外，还包括后来添加的其他属性。Person的每个实例—— personl和person2都包含一个内部属性，该厲性仅仅指向了 Person.prototype;换句话说，它们 与构造函数没有直接的关系。此外，要格外注意的是，里然这两个实例都不包含属性和方法，但我们却
可以调用personl. sayName (}。这是通过査找对象属性的过程来实现的。
M然在所有实现中都无法访问到[[Prototype]],似可以通过isProtoCypeOfO方法来确定对象之 间是否存在这种关系。从本质上讲，如果[[Prototype]]指向调用isPrototypeOfO方法的对象 (Person.prototype),那么这个方法就返[ai true,如下所不:
```javascript
alert(Person.prototype.isPrototypeOf(person1));  //true
alert(Person.prototype.isPrototypeOf(person2));  //true
```
这里，我们川股型对象的isPrototypeOf(}方法测试了 personl和person2。W为它们内部都 有一个指向Person.prototype的指针，因此都返回了 t:rue〇
ECMAScript5增加了一个新方法，叫Object.getPrototypeOf{)，在所有支持的实现中，这个 方法返回[[Prototype]]的值。例如:
```javascript
alert{Object.getPrototypeOf(personl) == Person.prototype); //true  
alert(Object.getPrototypeOf(personl).name);//"Nicholas"
```
这里的第一行代码只是确定Object .getPrototypeOf ()返回的对象实际就是这个对象的原型〇 第二行代码取得了原粗对象中name属性的值，也就是"虹〇11〇133"13使用Object.getPrototypeOf () 可以方便地取得一个对象的原型，而这在利用原型实现继承（本章稍后会讨论）的情况下是非常重要的。
支持这个方法的浏览器有 IE9+、Firef〇jc3.5+、Safari 5+、Opera 12+和 Chrome〇
每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先 从对象实例本身开始。如果在实例中找到了具有给定名字的属性，则返冋该属性的值;如果没有找到， 则继续搜索指针指向的原型对象，在原型对象中査找具打给定名字的属性。如果在原型对象中找到了这 个属性，则返回该属性的值。也就是说，在我们调用personl. sayNameO的时候，会先后执行两次搜 索。首先，解析器会问:“实例personl有sayName属性吗？ ”答:“没有。”然后，它继续搜索，再 问:“personl的原®有sayName属性吗？ ”答:“有。”于是，它就读取那个保存在原型对象中的函 数。.当我们调用persoW.sayNameOB、丨，将会重现相同的搜索过程，得到相同的结果。而这正是多个 对象实例共享原型所保存的属性和方法的基本原理。
前面提到过，原型最初只包含constructor属性，而该属性也是共享的，因此 可以通过对象实例访问。
虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们 在实例中添加了 -个属性，而该属性与实例原型中的-个属性同名，那我们就在实例中创建该属性，该 属性将会屏蔽原型中的那个属性。来看下面的例子。
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name);   //"Greg"-来自实例
alert(person2.name);   //"Nicholas"--来自原型
```
在这个例子中，personl的name被一个新值给屏蔽了。但无论访问personl. name还是访问 person2.name都能够正常地返网值，即分別是"Greg■(来fl对象实例）和"Nicholas"(来自原型)〇 当在alert {)中访问personl.name时|需要读取它的值，因此就会在这个实例上搜索一个名为name 的属性。这个属性确实存在，于是就返回它的值而不必再搜索原型了。当以同样的方式访问perS〇n2. name时，并没有在实例I:发现该属性，因此就会继续搜索原型，结果在那里找到了 name"属性。
当为对象实例添加-个厲性时，这个厲性就会屏藪原S对象中保存的同名属性;换句话说，添加这 个属性只会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个厲性设置为null,也 只会在实例中设置这个厲性，而不会恢复其指向原型的连接。不过，使用delete操作符则可以完全删 除实例属性，从而让我们能够重新访问原型中的属性，如下所示。
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name);   //"Greg" - 来自实例
alert(person2.name);   //"Nicholas" - 来自实例

delete person1.name;
alert(person1.name);   //"Nicholas" - 来自原型
```
在这个修改后的例子中，我们使用delete操作符删除了 personl.name,之前它保存的_Greg" 值屏蔽了同名的原型属性。把它删除以后，就恢复了对原型中name属性的连接。因此，接下来再调用 personl.name时，返间的就是原型中name属性的值
使用hasOwnProperty ()方法可以检测一个属性是存在于实例中，还是存在于原型中。这个方法（不 要忘了它是从Object继承来的）只在给定属性存在于对象实例中时，才会返回true。来看下面这个例子。
```javascript
function Person(){
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer"; 
Person.prototype.sayName = function(){
alert(this.name);
);
var person1 = new Person(); 
var person2 = new Person();
alert (person1.hasOwnProperty( "name"));    / /false

personl.name = "Greg";
alert (person1.name);   /"Greg""    来自实例
alert (person1.hasOwnProperty( "name")); //true

alert (person2.name) ;  //"Nicholas"    来自原型
alert (person2.hasOwnProperty( "name")); //falae

delete person1.name;
alert (person1.name) ;  //"Nicholas"    来自原型
alert(person1.hasOwnProperty("name")); //false
```
通过使用hasOwnPropertyU方法，什么时候访问的是实例属性，什么时候访问的是原型属性就 一清二楚广。调用personl .hasOwnProperty( "name”）时，只有.当personl重写name属性后才会 返回true，因为只有这时候name才是一个实例属性，而非原型属性。图6-2展示了上面例子在不同情 况下的实现与原型的关系（为了简单起见，阁中宵略了 与Person构造函数的关系)。



图卜2

---

ECMAScript5 的 Object.getOwnPropertyDescripcorO 方法只能用于实例属
性，要取得原型属性的描述符，必须直接在原型对象上调用Ob j ect. get Own Proper ty -
Descriptor ()方法。

---

**2.原型与in操作符**  

冇两种方式使用in操作符:单独使用和在for-in循环中使用。在单独使用时，in操作符会在通 过对象能够访问给定属性时返回true,无论该属性存在丁-实例中还是原塑中。看一看下面的例子。
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

alert(person1.hasOwnProperty("name"));  //false
alert("name" in person1);  //true

person1.name = "Greg";
alert(person1.name);   //"Greg" - 来自实例
alert(person1.hasOwnProperty("name"));  //true
alert("name" in person1);  //true

alert(person2.name);   //"Nicholas" - 来自原型
alert(person2.hasOwnProperty("name"));  //false
alert("name" in person2);  //true

delete person1.name;
alert(person1.name);   //"Nicholas" - 来自原型
alert(person1.hasOwnProperty("name"));  //false
alert("name" in person1);  //true
```
在以上代码执行的整个过程中，name属性要么是直接在对象上访问到的，要么是通过原型访问到 的。因此，调用"name" in personl始终都返回true,无论该属性存在于实例中还是存在于原型中。 同时使用hasOwnProperty 〇方法和in操作符，就可以确定该厲性到底是存在于对象中，还是存在于 原型中，如下所示。
```javascript
function hasPrototypeProperty(object, name){
return !object.hasOwnProperty(name) && (name in object);
}
```
由于in操作符只要通过对象能够访问到属性就返冋true, hasOwnProperty U只在属性存在于 实例中时才返回true，因此只要in操作符返回true而hasOwnProperty U返回false,就可以确 定属性是原型中的属性。下面来看一看上面定义的闲数hasPrototypeProperty ()的用法。
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person = new Person();        
alert(hasPrototypeProperty(person, "name"));  //true

person.name = "Greg";
alert(hasPrototypeProperty(person, "name"));  //false
```
在这里，name属性先是存在于原型中，因此hasPrototypeProperty()返回true。在实例中
重写name属性后，该属性就存在于实例中/,因此hasPrototypeProperty()返间false。即使原
型中仍然有name属性，仍由于现在实例中也有了这个属性，W此原1!中的name属性就用不到了。
在使用for-in循环时，返回的是所有能够通过对象访问的、可枚举的（enumerated)属性，其中
既包括存在f实例中的屈性，也包括存存:于原型中的属性。屏蔽f原型中不可枚举属性（即将
[[Enumerable]]标记的属件）的实例属性也会在for-in循环中返冋，因为根据规定，所有开发人员
定义的属性都是可枚举的^—只有在IE8及更早版本中例外。
IE早期版本的实现中存在一个bug,即屏蔽不可枚举属性的实例®性不会出现在for-in循环中。
例如:
```javascript
var o = {
    toString : function(){
        return "My Object";
    }
}

for (var prop in o){
    if (prop == "toString"){
        alert("Found toString");
    }
}
```
当以上代码运行时，应该会显示一个瞥告框，表明找到了 toStringO方法。这里的对象〇定义了 一个名为t〇String()的方法，该方法屏蔽丫原型中（不可枚举）的toString()方法。在IE中，由 于Jt实现认为原型的toString ()方法被打上了 [ [Enumerable]]标记就应该跳过该属性，结果我们就 不会看到警击框。该bug会影响默认不可枚举的所有属性和方法，包括:hasOwnPropertyU、 propertylsEnumerableU、toLocaleString()、toString()和 valueOf()。ECMAScript5 也将 constructor和prototype属性的[[Enumerable】]特性设置为false,值并不是所有浏览器都照此 实现。

要取得对象上所有可枚举的实例属性，可以使用ECMAScript5的Object. keys ()方法。这个方法 接收•个对象作为参数，返冋一个包含所有可枚举属性的字符串数组。例如:
```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var keys = Object.keys(Person.prototype);
alert(keys);   //"name,age,job,sayName"

var p1 = new Person();
p1.name = "Rob";
p1.age = "31";
var p1keys = Object.keys(p1);
alert(p1keys);//"name.age"
```
这里，变度keys中将保存一个数组，数组中是字符串"nameK、"age"、" job"和"sayName"。这 个顺序也是它们在for-in循环中出现的顺序。如果是通过Person的实例调用，则Object. keys {) 返冋的数组只也含"name"和w age "•这两个实例属性。
如果你想要得到所有实例属性，无论它是否可枚举，都"]"以使用Object .getOwnPropertyNaznes (J 方法。
```javascript
var keys = Object.getOwnPropertyNames(Person.prototype);
alert(keys);   //"constructor,name,age,job,sayName"
```
注意结果中包含了不可枚举的 constructor•属性。Object.keysU和 Object.get_OwnPr〇perty- Names ()方法都可以用来替代for-in循环。支持这两个方法的浏览器冇IE9+、Firefox4+、Saferi5十、Opera 12+和 Chrome。  

**3.更简单的原型语法**  

读者大概注意到了，前面例子中每添加一个属性和方法就要敲-遍Person,prototype。为减少 不必要的输人，也为了从视觉卜.更好地封装原塑的功能，更常见的做法是用一个包含所有属性和方法的 对象字面最来重写整个原型对象，如下面的例子所示。
```javascript
function Person(){
}

Person.prototype = {
    name : "Nicholas",
    age : 29,
    job: "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};
```
在上面的代码中，我们将Person.prototype设置为等于一个以对象字面量形式创建的新对象。 最终结果相同，但有一个例外:constructor厲性不再指向Person 丫。前面曾经介绍过，每创建一 个函数，就会同时创建它的prototype对象，这个对象也会自动获得constructor属性。而我们在 这里使用的语法，本质上完全重写了默认的prototype对象，因此constructor属性也就变成了新 对象的constructor屈性（指向Object构造函数），不再指向Person函数。此时，尽管instanceof 操作符还能返回正确的结果，但通过constructor G经无法确定对象的类型了，如下所示。
```javascript
var friend = new Person();

alert(friend instanceof Object);  //true
alert(friend instanceof Person);  //true
alert(friend.constructor == Person);  //false
alert(friend.constructor == Object);  //true
```
在此，用instanceof操作符测试Object和Person仍然返冋true,但constructor属性则 等于Object而不等于Person 了。如果constructor的值真的很重要，可以像下面这样特意将它设 置回适当的值。
```javascript
function Person(){
}

Person.prototype = {
    name : "Nicholas",
    age : 29,
    job: "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};
```
以上代码特意包含了一个constructor属性，并将它的值设置为Person,从而确保了通过该属 性能够访问到适当的值。
注意，以这种方式重设constructor属性会导致它的[[Enumerable〕]特性被设置为true。默认 情况下，原生的consumctor属性是不可枚举的，因此如果你使用兼容ECMAScript 5的JavaScript引 擎，可以试一试 Object .defineProperty ()。
```javascript
function Person(){
}

Person.prototype = {
    name : "Nicholas",
    age : 29,
    job: "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};
// 构造函数，只是用于ECMAScript 5 兼容的浏览器
Object.defineProperty(Person.prototype, "constructor% { 
enumerable: false, 
value: Person
```  

**4.原型的动态性**  

由于在原型中査找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上 反映出来——即使是先创建了实例后修改原铟也照样如此。请看下面的例子。
```javascript
var friend = new Person();

Person.prototype.sayHi = function(){
    alert("hi");
};

friend.sayHi();   //"hi" (没有问题!)
```
PmtotypePattemExampie09.htm
以上代码先创建了 Person的一个实例，并将其保存在person中。然后，下一条语句在Person, prototype中添加广-个方法sayHi ()。即使person实例是在添加新方法之前创建的，但它仍然可 以访问这个新方法。其原因可以W结为实例与原期之间的松散连接关系。当我们调用person. sayHi (} 时，酋先会在实例中搜索名为sayHi的屈性，在没找到的情况下，会继续搜索原型。因为实例与原型 之间的连接只不过是一个指针，而非一个副本，因此就可以在原型中找到新的sayHi属性并返回保存 在那里的函数。
尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重 写整个原塑对象，那么情况就不一样了。我们知道，调用构造函数时会为实例添加一个指向最初原型的 [[Prototype]]指针，而把原型修改为另外一个对象就等丁•切断了构造函数与最初原塑之间的联系。 谛记住:实例中的指针仅指向原型，而不指向构造函数。宥下面的例子。
```javascript
function Person(){
}

var friend = new Person();
        
Person.prototype = {
    constructor: Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};

friend.sayName();   //error
```
在这个例子中，我们先创建了 Person的一个实例，然后乂東写丫其原瑠对象。然后在调用 friend. sayName()时发生了错设，W为friend指向的原型中不包含以该名字命名的属性。图6-3肢
示了这个过程的内幕。
重写原型对象之前



重写原型对象之后
```
Person  ► | New Person Prototype
'—►I    Person Prototype   
```

图《
从图6-3可以肴出，重写原型对象切断f现有原型与任何之前已经存在的对象实例之间的联系;它 们引用的仍然是最初的原型。  

**5.原生对象的原型**  

原甩模式的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型，都是采用这种模式 创建的。所冇原生引用类型（Object、Array、String,等等）都在其构造函数的原型上定义了方法0 例如，在Array.prot:otype中可以找到sort(}方法，而在String.prototype中可以找到 substring ()方法，如下所亦。
```javascript
alert(typeof Array.prototype.sort); //_function"
alert(typeof String.prototype.substring);   //"function"
```
通过原生对象的原塑，不仅可以取得所有默认方法的引用，而且也可以定义新方法D可以像修改自 定义对象的原型一样修改原生对象的原型，因此可以随时添加方法。下面的代码就给基本包装类型 String添加了一个名为startsWith ()的方法。
```javascript
String.prototype.startsWith = function {text) {
return this.indexOf(text) == 0;
);
var msg = 'Hello world!";
alert(msg.startsWith("Hello"}); //true
```
这里新定义的startswith(}方法会在传人的文本位于一个字符串开始时返回true。既然方法被 添加给了 String.prototype,那么当前环境中的所有字符串就都可以调用它u由于msg是字符串， 而且后ft会调用String基本包装函数创建这个字符串，因此通过mgs就可以调用startsWith ()方法。

---

    尽管可以这样做,但我们不推荐在产品化的程序中修改原生对象的原型。如果因
某个实现中缺少某个方法，就在原生对象的原型中添加这个方法，那么当在另一个支 持该方法的实现中运行代码时，就可能会导致命名冲突。而且，这样做也可能会意外 地重写原生方法。

---

**6.原型对象的问题**  

原型模式也不是没有缺点。首先，它省略/为构造函数传递初始化参数这一环节，结果所有实例在 默认情况下都将取得相同的属性值。虽然这会在某种程度上带来-些不方便，但还不是原型的最火问題。 原型模式的S大问题是由其共享的本性所导致的。
原型中所有厲性是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性倒 也说得过去，毕竞（如前面的例子所示），通过在实例上添加一个同名属性，可以隐藏原型中的对应属 性。然而，对于包含引用类型值的属性来说，M题就比较突出了。来看下面的例子。
```javascript
function Person(){
}

Person.prototype = {
    constructor: Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    friends : ["Shelby", "Court"],
    sayName : function () {
        alert(this.name);
    }
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push("Van");

alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court,Van"
alert(person1.friends === person2.friends);  //true

```
在此，Person.prototype对象有一个名为friends的属性，该属性包含一个字符串数组。然后， 创建了 Person的两个实例。接着，修改了 personl .friends 丨用的数组，向数组巾添加了--个字符 率。由于friends数组存在于Person.prototype而非personl中，所以刚刚提到的修改也会通过 person2, friends (与personl .friends指向同一个数组）反映出来。假如我们的初衷就是像这样 在所有实例中共享-个数组，那么对这个结果我没有话可说。可娃，实例一般都是要有属于自己的仝部 城性的。而这个问题正是我们很少看到有人单独使用原型模式的原W所在。  

###  6.2.4 组合使用构造函数模式和原型模式  

创建自定义类塑的最常见方式，就是组合使用构造闲数模式与原型模式。构造函数模式用于定义实 例属件，而原型模式用于定义方法和共莩的属性。结果，每个实例都会有自己的一份实例属性的副本， 但同时又共享着对方法的引用，M大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参 数;可谓是集两种模式之长。下面的代码重写了前面的例7。
```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}

Person.prototype = {
    constructor: Person,
    sayName : function () {
        alert(this.name);
    }
};

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");

alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court"
alert(person1.friends === person2.friends);  //false
alert(person1.sayName === person2.sayName);  //true
```
在这个例子屮，实例属性都是在构造函数中定义的，而由所有实例共享的属性constructor和方 法sayName (}则是在原型中定义的。而修改了 personl. friends (向其中添加一个新字符串），并不 会影响到perS〇n2 . f riends,因为它们分別引用了不N的数组。
这种构造函数与原甩混成的模式，是目前在ECMAScript中使用最广泛、认同度最高的一种创建自 定义类翌的方法。可以说，这是用来定义引用类型的一种默认模式。  

###  6.2.5 动态原型模式  

有35他00语言经验的开发人员在肴到独立的构造函数和原型时，很可能会感到非常困惑。动态原 型模式正是致力于解决这个问题的一个方案，它把所有信息都封装在了构造函数中，而通过在构造函数 中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。换句话说，可以通过 检杳某个应该存在的方法是否有效，来决定是否需要初始化原型。来看--个例子。 
```javascript
function Person(name, age, job){

    //properties
    this.name = name;
    this.age = age;
    this.job = job;
    
    //methods
    if (typeof this.sayName != "function"){
    
        Person.prototype.sayName = function(){
            alert(this.name);
        };
        
    }
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```
注意构造阐数代码中加粗的部分。这里只在sayNameO方法不存在的情况下，才会将它添加到原 型中。这段代码只会在初次调用构造函数时才会执行。此;5,原型已经完成初始化，不®要再做什么修 改了。不过要记住，这里对原型所做的修改，能够立即在所有实例中得到反映。因此，这种方法确实可 以说非常完美。其中，if语句检查的可以是初始化之后应该存在的任何属性或方法一不必用一大堆 if语句检奄每个属性和每个方法;只要检査其中一个即可。对于采用这种模式创建的对象，还可以使 用instanceof操作符确定它的类划。
使用动态原型模式时，不能使用对象字面量重写原型。前面已经解释过了，如果 在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。  

###  6.2.6 寄生构造函数模式  

通常，在前述的几种模式都不适用的情况下，畔以使用寄生（parasitic)构造函数模式。这种模式 的基本思想是创建-个函数，该函数的作用仅仅足封装创建对象的代码，然后再返回新创建的对象;但 从表面上看，这个函数乂很像是典细的构造函数。下面是一个例子
```
function Person(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();  //"Nicholas"
```
在这个例子屮，Person函数创建了一个新对象，并以相应的M性和方法初始化该对象，然后乂返 M了这个对象。除了使用new操作符并把使用的包装函数叫做构造闲数之外，这个模式跟工厂模式其实 足•一模一样的。构造函数在不返冋值的情况下，默认会返回新对象实例。而通过在构造函数的末尾添加
-个return语句，可以重写调用构造函数时返冋的值。
这个模式可以在特殊的情况下用来为对象创建构造函数。假设我们想创建一个具冇额外方法的特殊 数组。由丁‘不能直接修改Array构造函数，丙此可以使用这个模式。
```
function SpecialArray(){       

    //create the array
    var values = new Array();
    
    //add the values
    values.push.apply(values, arguments);
    
    //assign the method
    values.toPipedString = function(){
        return this.join("|");
    };
    
    //return it
    return values;        
}

var colors = new SpecialArray("red", "blue", "green");
alert(colors.toPipedString()); //"red|blue|green"
```
在这个例子中，我们创建了一个名叫SpecialArray的构造函数。在这个函数内部，首先创建了 -个数组，然后push ()方法（用构造函数接收到的所有参数）初始化了数组的值。随后，又给数组实 例添加了一个toPipedStringf)方法，该方法返N以竖线分割的数组值。最后，将数组以函数值的形 式返回。接着，我们调用了 SpecialArray构造函数，向其中传人了用于初始化数组的值，此后又调 用 rt-〇?ipedString()方法。
关于寄生构造函数模式，有一点需要说明:首先，返回的对象与构造函数或者与构造函数的原型属 性之间没有关系;也就是说，构造函数返回的对象与在构造函数外部创建的对象没有什么不同。为此， 不能依赖instanceof操作符来确定对象类型。由于存在上述问题，我们建议在可以使用其他模式的情 况下，不要使用这种模式。  

###  6.2.7 稳妥构造函数模式  

道格拉斯"克罗克福德（Douglas Crockford)发明了 JavaScript中的稳妥对象（durable objects)这 个概念。所谓稳妥对象，指的是没有公共属性，而且艽方法也不引用this的对象。稳妥对象最适合在 -些安全的环境中（这些环境中会禁止使用this和new),或者在防止数据被其他应用程序（如Mashup 程序）改动时使用。稳妥构造函数遵循与寄生构造函数类似的模式，但冇两点不同:一是新创建对象的 实例方法不引用this;二是不使用new操作符调用构造函数。按照稳妥构造函数的要求，可以将前面 的Person构造函数重写如下。
```
function Person(name, age, job){
//创建要返回的对象 var o = new Object()?

//可以在这里定义私有变董和函数
//添加方法
o.sayName - function。{ alert{name)?
};
//返©对象 return o?
}
```
注意，在以这种模式创建的对象中，除丫使用sayNameU方法之外，没有其他办法访问name的值。 可以像下面使用稳妥的Person构造函数。
var friend = Person(_Nicholas", 29, -Software Engineer"); friend.sayNane()? //"Nicholas"
这样，变量person中保存的是一个稳妥对象，而除了调用sayNameO方法外，没有别的方式可 以访问其数据成员。即使有其他代码会给这个对象添加方法或数据成员，但也不可能有别的办法访问传 人到构造函数中的原始数据。稳妥构造函数模式提供的这种安全性，使得它非常适合在某些安全执行环
境   例如，ADsafe ( 

www.adsafe.org )和 Caja ( http:〃code.google.com/p/google-caja/ )提供的环境    
下使用。

---

(^\ 与寄生构造函数糢式类似，使用稳妥构造函数模式创建的对象与构造函数之间也
没有什么关系，因此instanceof操作符对这种对象也没有意义。

---

##  6.3 继承  

继承是00语言中的一个最为人津津乐道的概念。许多〇〇语言都支持两种继承方式:接口继承和 实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。如前所述，由于函数没有签名， 在ECMAScript中无法实现接口继承。ECMAScript只支持实现继承，而且其实现继承主要是依靠原型链 来实现的。  

###  6.3.1 原型链  

ECMAScript中描述了厣型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原 型让一个弓I用类型继承另一个引用类型的属性和方法。简单回顾一下构造函数、原型和实例的关系:每 个构造函数都有一个原型对象，原型对象都包含-个指向构造函数的指针，而实例都包含个指向原塑 对象的内部指针。那么，假如我们让原M对象等于另一个类型的实例，结果会怎么样呢？显然，此时的 原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另-个构造函数 的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实 例与原型的链条。这就是所谓原型链的基本概念。
实现原型链有一种基本模式，其代码大致如下。
```javascript
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty = false;
}

//inherit from SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function (){
    return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue());   //true
```
以上代码定义了两个类型:SuperType和SuMype。每个类型分別有一个属性和一个方法。它们 的主耍W别是Subiype继承了 SuperType,而继承是通过创建SuperType的实例，并将该实例赋给 SubType.prototype实现的。实现的本质是"写原型对象，代之以一个新类型的实例。换句«说，原 来存在于SuperType的实例中的所有属性和方法，现在也存在于SubType.prototype中了。在确立了 继承关系之后，我们给SubType.prototype添加,一个方法’这样就在继承了 SuperType的属性和方 法的基础上乂添加r一个新方法。这个例了-中的实例以及构造函数和原型之间的关系如图m所示。



SuperType
prototype
SubType
prototype |
图6-4
在上面的代码中，我们没有使用SubType默认提供的原趙，而是给它换了一个新原53;这个新原型 就是SuperType的实例。于是，新鹿塑不仅具有作为一个SuperType的实例所拥杳的仝部M性和方法， 而且其内部还有•个指针，指向了 SuperType的原型。始终结果就是这样的:instance指向SubType 的原姻，Subiype的原型又指向Super^iype的原型。getSuperValue (}方法仍然还在 SuperType.prototype 中，但 property 则位于 SubType.prototype 中。这是因为 property 是一 个实例属性，而getSuperValue 〇则是一个腺羽方法。既然SubType .prototype现在是SuperType

的实例，那么property当然就位于该实例中+J"。此外，要注意instance.constructor现在指向的 是SuperType,这是因为原来SubType.prototype中的constructor被重写了的缘故®。
通过实现原S!链，本质上扩展了本章前面介绍的原型搜索机制。读者大概还C得，当以读取模式访 问一个实例属性时，首先会在实例中搜索该属性。如果没有找到该属性，则会继续搜索实例的原沏。在 通过原型链实现继承的情况下，搜索过程就得以沿着原型链继续向上。就章上面的例子来说，调用 instance.getSuperValueO会经历三个搜索步骤:1)搜索实例;2)搜索 SubType.prototype; 3)搜索SuperType.prototype,最后一步才会找到该方法。在找不到属性或方法的情况下，搜索过 程总是要一环一环地前行到原型链末端才会停F来。

**1.别忘记默认的原型**  

亊实上，前面例子中展示的原型链还少一环。我们知道，所有引用类型默认都继承了 Object,而 这个继承也足通过原型链实现的。大家要记住，所有函数的默认原型都是Object的实例，因此默认原 塯都会包含一个内部指针，指向Object.prototype。这也正是所有自定义类型都会继承toStringl)、 valueOfU等默认方法的根本原因。所以，我们说上面例子展示的原型链中还应该包括另外一个继承层 次。图6-5为我们展示了该例子中完整的原型链。



m 6-5
一句话,SubType继承了 SuperType,而SuperType 了继承Object。当调用 instance.toString() 时，实际上调用的是保存在Object .prototype中的那个方法。
①实际1:，不是SubType的原堪的constructor属性被重写广，而是SubType的原塑指向了另一个对象   
SuperType的原型，而这个原麼对象的constructor属性指向的楚SuperType。

**2.确定原型和实例的关系**  

可以通过两种方式来确定原刮和实例之间的关系。第一种方式是使用instanceof操作符，只要用 这个操作符来测试实例与原纸链中出现过的构造函数，结果就会返冋true。以下几行代码就说明了这 —点。
```javascript
alert(instance instanceof Object);      //true
alert(instance instanceof SuperType);   //true
alert(instance instanceof SubType);     //true
```
由于掠型链的关系，我们可以说instance是Object、SuperType或SubType中仟何一个类型 的实例。W此，测试这三个构造函数的结果都返间了 crue。
第二种方式是使用isPrototypeOfO方法。同样，只要是原塑链中出现过的原型，都可以说是该 原型链所派生的实例的原型，因此isPrototypeOfO方法也会返Ntrue,如下所示。
```javascript
alert(Object.prototype.isPrototypeOf(instance));    //true
alert(SuperType.prototype.isPrototypeOf(instance)); //true
alert(SubType.prototype.isPrototypeOf(instance));   //true
```
**3.谨憤地定义方法**  

了-类型有时候需要重写超类型中的某个方法，或者需要添加超类型中不存在的某个方法。但不管怎 样，给原型添加方法的代码一定要放在替换原型的语句之后。来看下面的例子。
```javascript
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty = false;
}

//inherit from SuperType
SubType.prototype = new SuperType();

//new method
SubType.prototype.getSubValue = function (){
    return this.subproperty;
};

//override existing method
SubType.prototype.getSuperValue = function (){
    return false;
};

var instance = new SubType();
alert(instance.getSuperValue());   //false
```
在以上代码中，加粗的部分是两个方法的定义。第一个方法getSubValue()被添加到了 SubType 中。第二个方法getSuperValueU是原型链中已经存在的-个方法，但重写这个方法将会屏蔽原来的 那个方法。换句话说，肖通过SubType的实例调用getSuperValueO时，调用的就是这个重新定义 的方法;但通过SuperType的实例调用getSupervalueU时，还会继续调用原来的那个方法。这里 要格外注意的是，必须在用SuperType的实例替换原型之后，再定义这两个方法。
还有一点耑要提醒读者，即在通过原型链实现继承时，不能使用对象字面量创建原型方法。因为这 样做就会重写原型链，如下面的例子所示。
```javascript
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty = false;
}

//inherit from SuperType
SubType.prototype = new SuperType();

//try to add new methods ?this nullifies the previous line
SubType.prototype = {
    getSubValue : function (){
        return this.subproperty;
    },

    someOtherMethod : function (){
        return false;
    }
};

var instance = new SubType();
alert(instance.getSuperValue());   //error!
```
以上代W展示了刚刚把SuperType的实例斌值给原型，紧接着又将原型替换成一个对象字面量而 导致的问题。丨丨丨+T现在的原型包含的是一个Object的实例，而非SuperType的实例，因此我们设想 中的原型链已经被切断——SubType和SuperType之间已经没有关系了。  

**4.原型链的问题**  

原型链虽然很强大，可以用它来实现继承，仴它也存在一些问题。其中，最主要的问题来自包含引 用类型值的原型。想必大家还记得，我们前面介绍过包含引用类型值的原型属性会被所有实例共亨;而 这也正是为什么要在构造函数中，而不是在原铟对象中定义属性的原因。在通过原型来实现继承时，原 型实际上会变成另一个类型的实例。于是，原先的实例属性也就顺理成章地变成了现在的原型属性了。 下列代码可以用来说明这个问题。
```javascript
function SuperType(){
    this.colors = ["red", "blue", "green"];
}

function SubType(){            
}

//inherit from SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);    //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors);    //"red,blue,green,black"
```
这个例子中的Superiype构造函数定义了一个colors M性，该属性包含一个数组（引用类型值)。 SuperType的每个实例都会有各自包含自Li数组的colors域性。肖SubType通过原型链继承f SuperType之后，SubType.prototype就变成了 SuperType的一个实例，因此它也拥有/ -个它fi
己的colors岡性  就跟专门创建T -个SubType.prototype.colors属性一样。何结果是什么
呢？结果是SubType的所存实例都会共亨这一个colors属性。丨fif我们对instancel .colors的修改 能够通过instance2.colors反映出来，就已经充分证实f这一点。
原型链的第二个问题是:在创建了-类型的实例时，不能向超类塑的构造函数中传递参数。实际 应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。有鉴于此，再加上 前面刚刚讨论过的由丁•原项中包含引用类塑值所带来的问题，实践中很少会单独使用原铟链。  

###  6.3.2 借用构造函数  

在解决原型屮包含引用类沏值所带来问题的过程中，开发人员开始使用一种叫做借用构造函数 (constructorsteaiing〉的技术（有时候也叫做伪造对象或经典继承)。这种技术的基本思想相当简单，即 在子类铟构造函数的内部调用超类型构造函数。别忘了，函数只不过是在特定环境中执行代码的对象， 因此通过使用apply ()和call ()方法也可以在（将来）新创建的对象上执行构造函数，如下所示:
```javascript
function SuperType(){
    this.colors = ["red", "blue", "green"];
}

function SubType(){  
    //inherit from SuperType
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);    //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors);    //"red,blue,green"
```
代码中加#景的那一行代码“借调” /超类型的构造函数。通过使用calif)方法（或apply ()方 法也可以），我们实际上适在（未来将耍）新创建的SuMype实例的环境F调用了 SuperType构造函 数。这样一来，就会在新SuMYpe对象上执行Super'iype ()函数中定义的所有对象初始化代码。结果， SubType的紐个实例就都会具在自己的colors属性的副本了。  

**1.传递参数**  

相对于原甩链iWrt,借用构造函数有一个很大的优势，即可以在子类型构造函数中向超类型构造函 数传递参数。肴下面这个例了-。
```javascript
function SuperType(name){
    this.name = name;
}

function SubType(){  
    //inherit from SuperType passing in an argument
    SuperType.call(this, "Nicholas");
    
    //instance property
    this.age = 29;
}

var instance = new SubType();
alert(instance.name);    //"Nicholas";
alert(instance.age);     //29
```
以上代码中的SuperType只接受-个参数name,该参数会直接陚给一个属性„在SubType构造 函数内部调用SuperType构造函数时，实际上是为SubType的实例设置了 name属性。为丫确保 SuperType构造函数不会重写子类铟的属性，可以在调用超类型构造函数Ff，再添加应该在子类型中 定义的属性。
借用构造函数的问题
如果仅仅是借用构造函数，那么也将无法避免构造函数模式存在的问题一•方法都在构造函数中定 义，因此函数复用就无从谈起了。而且，在超类型的原逛中定义的方法，对子类型而言也是不可见的，结 果所有类沏都只能使用构造函数模式。考虑到这些问题，借用构造闲数的技术也是很少单独使用的。  

###  6.3.3 组合继承  

组合继承（combinationinheritance),有时候也叫做伪经典继承，指的是将原型链和借用构造函数的 技术组合到一块，从而发挥二者之长的一种继承模式。其背后的思路是使用原M链实现对原型属性和方 法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数 复用，又能够保证每个实例都右它ftd的M性。下面来宥一个例子。
```javascript
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age){  
    SuperType.call(this, name);
    
    this.age = age;
}

SubType.prototype = new SuperType();

SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);  //"red,blue,green,black"
instance1.sayName();      //"Nicholas";
instance1.sayAge();       //29


var instance2 = new SubType("Greg", 27);
alert(instance2.colors);  //"red,blue,green"
instance2.sayName();      //"Greg";
instance2.sayAge();       //27
```
在这个例子中，SuperType构造函数定义了两个属性:name和colors。SuperType的原型定义 了一个方法sayName ()。SubType构造闲数在阔用SuperType构造®数时传入丫 name参数，紧接着 又定义了它自己的属性age。然后，将SuperType的实例賦值给SubType的原型，然后乂在该新原型 上定义了方法sayAge ()»这样一来，就可以让两个不同的SubType实例既分别拥有自己属性一包 括colors属性，又可以使用相同的方法T。
组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为JavaScript中最常用的继 承模式。而且，instanceof和isPrototypeOf()也能够用于识別基于组合继承创建的对象。  

###  6.3.4 原型式继承  

道格拉斯•克罗克福德在2006年写，一篇文章，题为Prototypal Inheritance in JavaScript (■JavaScript 中的原型式继承)。在这篇文章中，他介绍f 一种实现继承的方法，这种方法并没有使用严格意义上的 构造函数。他的想法是借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。为 了达到这个fi的，他给出了如下函数。
```javascript
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
```
在object ()函数内部，先创建了一个临时性的构造函数，然后将传人的对象作为这个构造函数的 原型，域后返冋了这个临时类型的一个新实例。从本质上讲，objects)对传人其中的对象执行了一次

浅复制。来奍下面的例子。
```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"
```
克罗克福德主张的这种原型式继承，要求你必须有一个对象可以作为另•个对象的基础。如果有这么 --个对象的话，町以把它传递给object ()函数，然后冉根据具体谣求对得到的对象加以修改即"I。在这 个例子中，可以作为另一个对象基础的楚person对象，fJS我们把它传人到object ()函数中，然后该 函数就会返回-个新对象。这个新对象将person作为原型，所以它的原型中就包含一个基本类型值属性 和一个引用类型值属性。这意味# person, friends不仅属T person所有，而且也会被anotherPerson 以及yetAnotherPerson共享。实际匕这就相当十乂创建了 personX^t象的两个副本。
ECMAScdpt 5通过新增Obj ect. create ()方法规范化了原型式继承。这个方法接收两个参数:一 个用作新对象原型的对象和（可选的）一个为新对象定义额外W性的对象。在传人•-•个参数的情况下， Object. create (}与object ()方法的行为相同。
```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = Object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"
```
Object. create (}方法的第二个参数与Object .defineproperties ()方法的第二个参数格式相 同:每个m性都是通过r】d的描述符定义的。以这种方式指定的任何属件都会覆盖原迆对象上的同名埔 性。例如:
```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
                   
var anotherPerson = Object.create(person, {
    name: {
        value: "Greg"
    }
});

alert(anotherPerson.name);  //"Greg"
```
支持0的6£:1;.£^63七6()方法的浏览器有1£9+、？丨比£'(«4+、33{^5+、〇?6«12+和〇11"01116。
在没有必要兴师动众地创建构造函数，rfj只想让一个对象与另-个对象保持类似的悄况下，原型式 继承是完全可以胜任的。不过别忘了，包含引用类型值的属性始终都会共享相应的值，就像使用原型模
式一样。 

###   6.3.5 寄生式继承  

寄生式（parasitic)继承是tj原型式继承紧密相关的一种思路，并且同样也是由克罗克福德推而广 之的。寄生式继承的思路与寄生构造函数和丁.厂模式类似，即创建一个仅用于封装继承过程的函数，该 函数在内部以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象。以下代码示范/寄
牛式继承模式^
```
function createAnother(original){
var clone = object (original);  //通过调用函数创建一个新对象
clone. sayHi = function(){  //以某种方式来增强这个4象
alert("hi");
};
return clone;   //返©这个对象
)
```
在这个例了•中，createAnotherO函数接收了一个参数，也就是将要作为新对象基础的对象。然 把这个对象（original)传递给ot}ject()函数，将返回的结果斌值给clone。再为clone对象 添加一个新方法sayHi ()，最后返回clone对象。可以像下面这样来使用createAnother ()困数:
```
var person = {
name: "Nicholas",
friends: [ "Shelby•, "Court" ( "Van"].
};
var anotherPerson = createAnother(person); 
anotherPerson.sayHi(); //"hi"
```
这个例子中的代码基于person返回了 ■个新对象   anotherPerson。新对象不仅具有person
的所有属性和方法，lfi〖FL还有自d的sayHi ()方法。
在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。前面示 范继承模式时使用的object ()函数不是必需的;任何能够返回新对象的函数都适用于此模式。

---

使用寄生式继承来为对象添加"数，会由于不能做到函数复用而降低效率;这一 点与构造函数模式类似。

---

###   6.3.6 寄生组合式继承  

前面说过，组合继承是JavaScript域常用的继承模式;不过，它也有〇己的不足。组合继承最大的 问题就是无论什么情况下，都会调用两次超类型构造函数:一次是在创建子类型原铟的时候，另一次是 在子类型构造函数内部。没错，子类MS终会包含超类型_对象的全部实例属性，但我们不得不在调用子 类型构造闲数时2写这些属性。冉来看一看下面组合继承的例子。
```javascript
function SuperType(name){
            this.name = name;
            this.colors = ["red", "blue", "green"];
        }
        
        SuperType.prototype.sayName = function(){
            alert(this.name);
        };

        function SubType(name, age){  
            SuperType.call(this, name);
            
            this.age = age;
        }

        inheritPrototype(SubType, SuperType);
        
        SubType.prototype.sayAge = function(){
            alert(this.age);
        };
```
加粗字体的行中是调用SuperType构造阑数的代码。在第一次调用SuperType构造函数时， SubType.prototype会得到两个M性:name和colors;它们都是SuperType的实例属性，只不过 现在位于SubType的原型中。当调用SubType构造闲数时，乂会调川一次SuperType构造函数，这 —次乂在新对象上创建丫实例属性name和colors。是，这两个属性就屏蔽了原沏中的两个同名属 性。图6-6展尕了上述过程。
如阁6-6所示，有两组name和colors属性:一组在实例上，一组在SubType原期中。.这就是调 用两次SuperType构造函数的结果。好在我们已经找到了解决这个问题方法一寄生组合式继承。
所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背 后的基本思路是:不必为f指定了•类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型 原型的一个副本时G。本质上，就足使用寄生式继承来继承超类塯的®型，然后再将结果指定给子类型 的原型。寄生组合式继承的基本模式如下所示。
```javascript
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype);   //create object
    prototype.constructor = subType;               //augment object
    subType.prototype = prototype;                 //assign object
}
```
这个小-例中的inheritPrototype<)闲数实现广寄生组合式继承的最简单形式。这个函数接收两 个参数:子类型构造函数和超类型构造函数。在函数内部，第一步是创建超类型原项的-个副本。第二 步是为创建的副本添加constructor属性，从而弥补W重写原铟而失去的默认的constructor属性。 最后一步，将新创建的对象（即副本）赋值给了•类型的原型。这样，我们就吋以用调用irxherit- PrototypeO函数的语句，去替换前面例子中为子类铟原型赋值的语句了，例如:
```javascript
function SuperType(name){
            this.name = name;
            this.colors = ["red", "blue", "green"];
        }
        
        SuperType.prototype.sayName = function(){
            alert(this.name);
        };

        function SubType(name, age){  
            SuperType.call(this, name);
            
            this.age = age;
        }

        inheritPrototype(SubType, SuperType);
        
        SubType.prototype.sayAge = function(){
            alert(this.age);
        };
```

图6-6

这个例子的高效率体现在它只调用T一次Superoype构造螭数，并迁W此避免了在SiibType. prototype上flf创建不必要的、多余的域性。与此同时，原型链还能保持不变;W此，还能够TH常使用 instanceof和isPrototypeOf ()。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。

---

YUI的YAHOO_ lang.extend()方法采用了寄生组合继承，从而让这种模式首次 出现在了一个应用非常广泛的JavaScript库中。要了解有关YUI的更多信息，请访问 
http://developer. yahoo.com/yui/。

---

##  6.4 小结
ECMAScript支持面向对象（00)编程，但/K使用类或者接口。对象可以在代码执行过程中创建和 坩强，因此具釘动态性而非严格定义的实体。在没有类的情况下，吋以采用下列模式创建对象。
- [ ]  工厂模式，使用简单的函数创建对象，为对象添加属性和方法，然后返冋对象。这个模式后来 被构造涵数模式所取代。
- [ ] 构造函数模式，可以创建G定义引用类型，可以像创建内置对象实例一样使用new操作符。不 过，构造函数模式也有缺点，即它的每个成员都无法得到复用，包括函数。由于函数可以不局 限于任何对象（即与对象具有松散耦合的特点），因此没有理由不在多个对象间共享函数。
- [ ] 原型模式，使用构造函数的prototype «性来指定那些应该共享的M性和方法。组合使用构造 函数模式和原型模式时，使用构造函数定义实例属性，而使用原玴定义共亨的属性和方法。
JavaScript主要通过原型链实现继承。原型链的构建是通过将一个类塑的实例陚值给另一个构造函 数的原型实现的。这样，子类型就能够访问超类型的所冇厲性和方法，这一点与基丁类的继承很相似。 原型链的问题是对象实例共享所冇继承的屈性和方法，闪此不适宜单独使用。解决这个问题的技术是借 用构造函数，即在子类型构造函数的内部调用超类型构造函数。这样就可以做到每个实例都具有己的 W性，间时还能保证只使W构造函数模式来定义类型。使用最多的继承模式是组合继承，这种模式使用 原型链继承共享的属性和方法，而通过借用构造函数继承实例属性。
此外，还存在下列可供选择的继承模式。
- [ ] 原铟式继承，可以在不必预先定义构造函数的情况下实现继承，其本质是执行对给定对象的浅 复制。而笈制得到的副本还可以得到进一步改造。
- [ ] 寄生式继承，与原型式继承非常相似.也是基f某个对象或某《信息创建一个对象，然后增强 对象，最后返W对象。为了解决组合继承模式由于多次调用超类羝构造涵数而导致的低效率问 题，可以将这个模式与组合继承一起使用。
- [ ] 寄生组合式继承，集寄生式继承和组合继承的优点与一身，是实现基于类型继承的最有效方式。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter5.md)  [下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter7.md)
