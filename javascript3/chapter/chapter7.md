#  第7章 函数表达式([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 函数表达式的特征
- [ ] 使用函数实现递归
- [ ] 使用闭包定义私有变贵  

数表达式是JavaScript中的•个既强大乂容易令人困惑的特性。第5章曾介绍过，定义函数的
方式有两种：一种是函数声明，另一种就是函数表达式。函数卢明的语法是这样的。
function functionName{argO, argl, arg2) {
//函数体
}
首先是function关键字，然后是函数的名字，这就是指定函数名的方式。Firefox、Safari、Chrome 和Opera都给函数定义了一个非标准的name属性，通过这个属性可以访问到给函数指定的名字。这个 属性的值永远等于跟在function关键字后面的标识符。
//只在 Firefox、Safari' Chrome 和 Opera 有效 alert(functionName.name); iI*funct ionName"
FunctionNameExampleOl. htm
关于函数声明，它的一个重要特征就是函数声明提升（fonction declaration hoisting)，意思是在执行 代码之前会先读取函数声明。这就意味着可以把函数声明放在调用它的语句后面。
```
sayHi{);
function sayHi(){ alert{"Hi!");
}
```
FunctionDeclaratiortHoistingOl. htm
这个例子不会抛出错误，w为在代码执行之前会先读取函数声明。
第二种创建函数的方式是使用函数表达式。函数表达式有几种不同的语法形式。下面是最常见的一 种形式。
var functionName = functiontargO, argl, arg2>{ //函數体
};
这种形式看起来好像是常规的变t陚值语句，即创建一个函数并将它賦值给变量functionName。 这种情况下创建的函数叫做匿名函数（anonymous fiinction),因为function关键宁后面没有标识符。 (匿名函数有时候也叫拉姆达函数。）匿名函数的name属性是空字符串。
函数表达式与其他表达式一样，在使用前必须先斌值。以下代码会导致错误。
sayHi ();   〃铸谈：函教还不存在
var sayHi = function(){ alert("Hi!");
};
理解病数提升的关键，就是理解函数声明与函数表达式之间的区别。例如，执行以下代码的结果可 能会让人意想不到。
//不要这样做！ if(condition){
function sayHi(){ alert(-Hi!"};
)
} else {
function sayHi(}{ alert(nYo!-);
}
FunctionDechrationsErrorExample01.htm
表面上看，以上代码表示在condition为true时，使用一个sayHi ()的定义;否则，就使用另 一个定义。实际上，这在ECMAScript中属f无效语法，JavaScript引擎会尝试修正错误，将其转换为合 理的状态。但问题是浏览器尝试修正错误的做法并不一致。大多数浏览器会返回第二个声明，忽略 condition; Firefox会在condition为true时返回第一个声明。因此这种使用方式很危险，不应该 出现在你的代码中。不过，如果是使用函数表达式，那就没有什么问题了。
//可以这样做 var sayHi;
if(condition){
sayHi = function U{ alert("Hi!*);
};
> else {
sayHi = function (H alert(-Yo!");
);
}
这个例子不会有什么意外，不同的函数会根据condition被赋值给sayHi。
能够创建函数再賦值给变量，也就能够把函数作为其他函数的值返冋。还记得第5章中的那个 createComparisonFunct ion () S数吗：
function createComparisonFunction(propertyName) {
return function(objectl, object2){
var valuel = objectl[propertyName); var value2 = object2[propertyNameJ;
if (valuel < value2){ return -1;
> else if (valuel > value2){ return 1;
)else {
return 0;
}
};
i
createCoroparisonFunction()就返回r—个匿名函数，返回的闲数可能会被斌值给-•个变董， 或者以其他方式被调用;不过，在createComparisonFunction()函数内部，它是匿名的。在把函数 当成值来使用的情况下，都可以使用匿名函数。不过，这并不是匿名函数唯一的用途。
##  7.1 递归
递归函数是在一个函数通过名字调用fl身的情况下构成的，如下所示。
function factorial (nm) { if (num <= 1}{ return 1?
} else {
return num * factorial(num-1);
RecursionExampleOI. htm
这是一个经典的递归阶乘函数。虽然这个函数表面看来没什么问题，但下面的代码却可能导致它出错。
var anotherFactorial = factorial; factorial = null?
alert (anotherFactorial (4)) ; // 出播！
RecursionExampleOI. htm
以上代码先把factorial 〇函数保存在变量anotherFactorial中，然A?将factorial变董设 置为null,结果指向原始函数的引用只剩卞一个。但在接下来调用anotherFactorial。时，由于必 须执行factorial^)，而factorial已经不再是函数，所以就会导致错误。在这种情况下，使用arguments, callee 可以解决这个问题。
我们知道，arguments.callee娃一个指向iK在执行的函数的指针，因此可以用它来实现对函数 的递归调用，例如：
function factorial(num){ if {num <= 1){ return 1;
} else (
return niun * arguments .callee(niin-l);
}
RecursionExample02.htm

178 第7章函数表达式
加粗的代码显示，通过使用arguments.callee代替函数名，可以确保无论怎样调用函数都不会 出问题。因此，在编写递归函数时，使用arguments.callee总比使用函数名更保险0
但在严格模式下，不能通过脚本访问arguments.callee,访问这个M性会导致错误。不过，可 以使用命名函数表达式来达成相同的结果。例如：
var factorial = (function f(num) { if (num <= 1){ return 1;
} else {
return n\un * £ (num-1);
}
});
以上代码创建了一个名为f〇的命名函数表达式，然后将它赋值给变量factorial。即便把函数 賦值给了另一个变fi,函数的名字f仍然有效，所以递归调用照样能正确完成。这种方式在严格模式和 非严格模式下都行得通。
##  7.2 闭包
有不少开发人员总是搞不清匿名函数和闭包这两个概念，因此经常混用。闭包是指有权访问另一个 函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数，仍以前面的
createComparisonFunction (}函数为例，注意加粗的代码。
```javascript
function createComparisonFunction(propertyName){
	return function(objectl, object2){
		var valuel = objectl[propertyName];(加粗)
		var value2 = object2[propertyName];
		if (valuel < value2){
			return -1;
		else if (valuel > value2){
			return 1;
		else {
			return 0;
		)
	};
}
```
在这个例子中，突出的那两行代码是内部函数（一个匿名函数）中的代码，这两行代码访问了外部 函数中的变量propertyNamee即使这个内部函数被返回了，而且是在其他地方被调用了，但它仍然口J 以访问变M propertyName。之所以还能够访问这个变童，是因为内部函数的作用域链中包含 createComparisonFunctiion ()的作用域。要彻底搞清楚其中的细节，必须从理解函数第一次被调用 的时候都会发生什么人手。
第4草介绍了作用域链的概念。而有关如何创建作用域链以及作用域链有什么作用的细节，对彻底 理解闭包至关重要。当某个闲数第一次被调用时，会创建--个执行环境（executioncontext)及相应的作 用域链，并把作用域链赋值给一个特殊的内部属性（B卩[[Scope]丨）。然后，使用this、arguments 和其他命名参数的值来初始化涵数的活动对象（activationobject)。但在作用域链中，外部函数的活动对
象始终处于第二位，外部函数的外部函数的活动对象处于第三位，……直至作为作用域链终点的全局执 行环境。
在函数执行过程中，为读取和写人变量的值，就需要在作用域链中査找变量。来看下面的例子。
function compare{valael, valued){ if (valuel < value2){ return -1;
} else if (vaiuel > value2){ return 1;
} else {
return 0;
var result = compare(5# 10)?
以上代码先定义了 compare (>兩数，然后又在全局作用域中调用了它。当第一次调用compare 〇 时，会创建一个包含this、arguments、valuel和value2的活动对象。全局执行环境的变量对象 (包含this、result和compare )在compare ()执行环境的作用域链中则处于第二位。图7-丨展7K了 包含上述关系的compare ()函数执行时的作用域链。



图7-1
后台的每个执行环境都有一个表示变量的对象——变tt对象。全局环境的变量对象始终存在，而像 compare (>涵数这样的局部环境的变量对象，则只在函数执行的过程中存在。在创建compare ()函数 时，会创建一个预先包含全局变量对象的作用域链，这个作用域链被保存在内部的[[Scope]]属性中。 当调用compareU函数时，会为函数创建一个执行环境，然后通过&制函数的[[Scope]]属性中的对 象构建起执行环境的作用域链。此后，乂有一个活动对象（在此作为变S对象使用）被创建并被推人执 行环境作用域链的前端。对于这个例子中compare (>函数的执行环境而言，其作用域链中包含两个变 贵对象：本地活动对象和全局变ft对象。显然，作用域链本质上是一个指向变景对象的指针列表，它只 引用但不实际包含变量对象。
无论什么时候在函数中访问一个变馕时，就会从作用域链中搜索具有相应名字的变ft。一般来讲， 当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象)。 但是，闭包的情况又有所不同。

180 第7章函數表达式
在另一个函数内部定义的函数会将包含函数（即外部函数）的活动对象添加到它的作用域链中。因
此，在createComparisonFunction()涵数内部定义的匿名函数的作用域链中，实际上将会包含外部 闲数createComparisonFunction<>的活动对象。阁7-2展示了肖下列代码执行时，包含函数与内部 E名函数的作用域链。
var compare = createComparisonFunction("name");
var result = compare ({ name: "Nicholas'* }, < nane： "Greg" >);
在匿名函数从createComparisonFunction()中被返问后，它的作用域链被初始化为包含 createComparisonFunction(>函数的活动对象和全局变量对象。这样，廣名函数就可以访问在 createComparisonFxmctionU 中定义的所有变M。更为遺要的是，createComparisonFunction() 阐数在执行完毕后，其活动对象也不会被销毁，因为匿名函数的作川域链仍然在引川这个活动对象。换 句话说，当createComparisonFunction()函数返回后，其执行环境的作川域链会被销毁，但它的活 动对象仍然会留在内存中;育到匿名函数被销毁后，createComparisonFunctionO的活动对象才会 被销毁，例如：
//创建函数
var compareNames = creaceComparisonFunct.ion{"name");
//调用函数
var result = compareNames({ name: "Nicholas" }# { name： "Greg" });
//解除对匿名函数的引用（以便释放内存）
compareNames ^ null;
首先，创建的比较函数被保存在变:tt compareNames中。而通过将compareNames设置为等于null 解除该函数的引用，就等丁-通知垃圾N收例程将其清除。随着匿名函数的作用域链被销毁，典他作用域 (除了全局作用域）也都可以安全地销毁了。图7-2展示了调用compareNames (>的过程中产生的作用 域链之间的关系。



阁7-2
由于闭包会携带包含它的函数的作用城，因此会比其他函数占用更多的内存。过 度使用闭包可能会导致内存占用过多，我们建议读者只在绝对必要时再考虑使用闭 包。虽然像V8等优化后的JavaScript引擎会尝试回收被闭包占用的内存，但请大家 还是要慎重使用闭包。
###  7.2.1 闭包与变量
作用域链的这种配置机制引出了一个值得注意的副作用，即闭包只能取得含函数中任何变量的最 后一个值。别忘了闭包所保存的是整个变量对象，而不是某个特殊的变ii。下面这个例子可以清晰地说 明这个问题。
function createFunctions(){ var result = new Array();
for (var i=0; i < 10; i++}{ result fi) = function(){ return i;
};
}
return result;
ClosureExample01.htm
这个函数会返冋一个函数数组。表面上看，似乎每个函数都应该返自己的索引值，即位置〇的函数 返回0,位置丨的函数返回1，以此类推。但实际上，每个函数都返回10。因为每个函数的作用域链中 都保存着createFunctionsU函数的活动对象，所以它们引用的都是同一个变量i。当 createFunctions 〇函数返凹后，变虽i的值是10,此时每个函数都引用着保存变最i的同一个变最 对象，所以在每个函数内部i的值都是10。但是，我们可以通过创建另一个匿名函数强制让闭包的行为 符合预期，如下所示。
```
function createFunctions(){ var result - new Array();
for (var i=0; i < 10; i++){ result【1】《 functlon(num)\ return function(){ return num;
);
>(i);
}
return result;
```
CbsureExampie02.htm
在重写了前面的createFunctionsU函数后，每个函数就会返回各6不同的索引值了。在这个版 本中，我们没宥直接把闭包陚值给数组，而是矩义了一个匿名函数，并将立即执行该匿名函数的结果賦 给数组。这里的匿名函数有一个参数num,也就是最终的函数要返回的值。在调用每个匿名函数时，我 们传人了变fii。由于函数参数是按值传递的，所以就会将变Mi的当前值复制给参数num。而在这个 匿名函数内部，又创建并返回了一个访问nur.的闭包。这样一来，result数组巾的每个函数都有自己 num变世的••个副本，因此就可以返冋各自不N的数值了。
###  7.2.2 关于this对象
在闭包中使州this对象也W能会导致一些问题。我们知道，this对象是在运行时基于函数的执 行环境绑定的：在全局函数中，this等于window,而当函数被作为某个对象的方法调用时，this等 于那个对象。不过，匿名函数的执行环境具冇全局性，因此其this对象通常指向window®。但冇时候 由于编写闭包的方式不同，这一点可能不会那么明显。下面来看一个例子。
var nair.e = "The Window* ?
var object = {
name: "My Object",
getNameFunc : function(){
return function(){
return this.name;
};
}
);
alert (object .getNameFunc() ()); //"The Window"(在非严格模式下）
TkisObjectExample01.htm
以上代码先创建了一个全局变量name, 乂创建了一个包含name属性的对象。这个对象还包含一
个方法 getNameFunc。，它返回一个匿名函数,而匿名函数又返冋this.name。由于getNameFunc()
返回一个函数，因此调用object.getNameFunc()(>就会立即调用它返回的闲数，结果就是返回一-个 字符串。然而，这个例子返冋的字符串是"The Window",即全局name变量的值。为什么匿名函数没 有取得其包含作用域（或外部作用域）的this对象呢？
前面曾经提到过，每个函数在被调用时，其活动对象都会自动取得两个特殊变董：this和 arguments。内部函数在搜索这两个变最时，只会搜索到其活动对象为止，因此永远不可能直接访问外 部函数中的这两个变量（这一点通过图7-2可以吞得更清楚)。不过，把外部作用域中的this对象保存 在一个闭包能够访问到的变tt里，就可以让闭包访问该对象了，如下所示。
var name = "The Window*;
var object = {
name : *My Object"#
getNameFunc: function{){
①当然，在通过call()s£applyU改变函数执行环境的情况下，this就会指向其他对象„

7.2 闭包 183
var that = this; return function。{
return that.name;
};
alert(object.getNameFunc()());  //"My Object
ThisObjectExampleOl. him
代码中突出的行展示丫这个例子与前一个例子之间的不同之处。在定义匿名函数之前，我们把this 对象賦值给了一个名叫that的变贵。而在定义了闭包之后，闭包也可以访问这个变量，因为它是我们 在包含函数中特意声名的-个变量。即使在函数返回之后，that也仍然引用着object,所以调用 object.getNameFuncO ()就返冋了"My Object"。
this和arguments也存在同样的问题。如果想访问作用域中的arguments对 象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。
在几种特殊情况下，this的值可能会意外地改变。比如，下面的代码是修改前面例子的结果。
var name = "The Window";
var object = {
name: "My Object-,
getName: function(){ return this.name;
)
};
这里的getNameU方法只简单地返回this.narae的值。以下是几种调用object.getName()的 方式以及各自的结果。
object.getName();   //"My Object"
(object.getName)(); //"My Object"
(object.getName = object.getName) {); /"The Window' 在非严格模式下
ThisObjectExampleQ3. htm
第一行代码跟平常一样调用厂object:.getName()，返回的是_1^ Object"，因为this.name 就是object.name。第二行代码在调用这个方法前先给它加上了括号。虽然加上括号之后，就好像只 是在引用-•个函数，但this的值得到了维持，因为object.getName和（object.getName)的定义 是相同的。第三行代码先执行了一条賦值语句，然后再调用賦值后的结果。因为这个賦值表达式的值是 函数本身，所以this的值不能得到维持，结果就返回了-The Window"。
当然，你不大可能会像第二行和第三行代码一样调用这个方法。不过，这个例子有助于说明即使是 语法的细微变化，都有可能意外改变this的值。
###  7.2.3 内存泄漏
由丁• IE9之前的版本对JScript对象和COM对象使用不同的垃圾收集例程（第4章曾经讨论过）,

184 第7章函数表达式
因此闭包在IE的这些版本中会导致一些特殊的问题。具体来说，如果闭包的作用域链中保存着一个 HTML元素，那么就意味着该元素将无法被销毁。来看下面的例子。
function assignHandler(){
var element = document.getElementById(_someElement"); element.onclick = function(>{ alert(element.id);
};
}
以h代码创建了一个作为element元素事件处理程序的闭包，而这个闭包则又创建了一个循环引 用（事件将在第13章讨论允由于匿名函数保存了一个对assignHandler (>的活动对象的引用，因此 就会导致无法减少element的弓丨用数。只要匿名函数存在，element的引用数至少也是1,因此它所 占用的内存就永远不会被冋收。不过，这个问题可以通过稍微改写一下代码来解决，如F所示。
function assignHandler(){
var element = document.getElement3yId{"someElement"); v&r id ■ eleaoont.id;
element.onclick = function(){ alert(id);
};
element = null;
}
在上面的代码中，通过把element.id的一个副本保存在一个变址屮，并且在闭包中引用该变M消 除了循环引用。但仅仅做到这一步，还是不能解决内存泄漏的问题。必须要记住：闭包会引用包含函数 的整个活动对象，而其中包含着element。即使闭包不直接引用element，包含函数的活动对象中也 仍然会保存一个引用。因此，有必要把element变M设置为null。这样就能够解除对DOM对象的引 用，顺利地减少其引用数，确保正常W收其占用的内存。
##  7.3 模仿块级作用域
如前所述，JavaScript没有块级作用域的概念。这意味着在块语句中定义的变量，实际上是在包含 函数中而非语句中创建的，来看下面的例子。



function outputNumbers{count){
for (var i=0; i < count; i++){
alert(i);
>
alert (i) ? //计数
BlockScopeExampleOl .htm
这个函数中定义了-个for循环，而变量i的初始值被设置为0。在Java、C++等语言中，变量i 只会在for循环的语句块中有定义，循环一旦结束，变量i就会被销毁。可是在JavaScrip中，变量i 是定义在cmputNumbersO的活动对象中的，因此从它有定义开始，就可以在函数内部随处访问它。即 使像下面这样错误地重新声明同一个变fit,也不会改变它的值。
function outputNumbers(count){
for (var i=0? i < count; i++>{ alert(i);
}
var i;  "重新声明变量
alert (i) ; //计数
BlockScopeExample02. htm
JavaScript从来不会告诉伯;是否多次声明了同一个变置;遇到这种情况，它只会对;Ti•续的声明视而不 见（不过，它会执行后续声明中的变童初始化)。匿名函数可以用来模仿块级作用域并避免这个问题。
用作块级作用域（通常称为私有作用域）的匿名函数的语法如下所示。
(function(){
//这里是块级作用域
}) ()?
以上代码定义并立即调用了一个匿名函数。将闲数卢明包含在一对圆括号中，表示它实际上是一个 函数表达式。而紧随其后的另一对圆括号会立即调用这个函数。如果有读者感觉这种语法不太好理解， 可以再看看下面这个例子。
var count = 5;
outputNumbers(count);
这里初始化了变量count,将其值设置为夂当然，这里的变量是没有必要的，因为可以把值直接 传给函数。为了让代码更简洁，我们在调用函数时用5来代替变量count,如下所示。
outputNumbers(5);
这样做之所以可行，是因为变量只不过是值的另一种表现形式，因此用实际的值替换变量没有问题。 再看下面的例子。
var someFunction = function(){
//这置是块级作用域
);
someFunction()?
这个例子先定义了一个函数，然后立即调用了它。定义函数的方式是创建一个匿名函数，并把匿名 函数賦值给变量someFunction。而调用函数的方式是在函数名称后面添加一对圆括号，即 someFunctionU。通过前面的例子我们知道，可以使用实际的值来取代变量count,那在这里是不是 也可以用函数的值/[接取代函数名呢？然而，下面的代码却会导致错误。
function(){
//这里是块級作用域 }(); //出醤！
这段代码会导致语法错误，是W为JavaScript将function关键字当作.个函数声明的开始，而函 数声明后面不能跟岡括号a然而，函数表达式的后面可以跟圆括号。要将函数声明转换成函数表达式， 只要像下面这样给它加上一对圆括号即可。
(function(){
//这里是块级作用域 >)()/

186 第7章函数表达式
无论在什么地方，只要临时需要一钱变tf，就"f以使用私布作用域，例如:



function outputNumbers(count){
(function (} {
for (var i=0; i < count; i*+){
alert(i);
J
>)<)?
alert (i);  //导致一个嫌误：!
}
BlockScopeExample03. htm
在这个甫写后的outputNumbers (>涵数中，我们在for循环外部插人r •个私有作用域!>在匿名 函数中定义的任何变M,都会在执行结束时被销毁。因此，变tt i只能在循环中使用，使用后即被销毁。 而在私有作用域中能够访问变a count,是因为这个匿名函数是一个闭包，它能够访问包含作用域中的 所有变ft。
这种技术经常在全局作用域屮被用在闲数外部，从限制向全局作用域中添加过多的变量和病数。 —般来说，我们都应该尽M少向全M作用域中添加变fl:和函数。在一个由很多开发人员共同参与的大型 应用程序中，过多的全局变童和函数很容易导致命名冲突。而通过创建私有作用域，每个开发人员既可 以使用自己的变*,又不必担心搞乱全M作用域。例如：
(function(){
var now = new DateO;
if {now.getMonth() == 0 && now.getDate()    1){
aierc("Happy new year!");
>
Ji 0;
把上面这段代码放在全局作用域中，可以用来确定哪--天是丨月丨日;如果到了这一天，就会向用 户M示一条祝贺新年的消息。其中的变觉now现在是匿名函数中的局部变a,而我们不必在全局作用域 中创建它。



这种做法可以减少闭包占用的内存问题，因为没有指向鹿名函数的5丨用。只要函
数执行完毕，就可以立即销毁其作用域链了。
##  7.4 私有变量
严格来讲，hvaScript屮没仓私相成员的概念;所冇对象M性都是公布的。不过，倒是有.个私有 变M的概念。任何在函数中定义的变M,都可以认为足私有变最，因为不能在函数的外部访问这些变M。 私有变M包括函数的参数、M部变量和在函数内部定义的K他函数。来看下面的例乎：
function add(nun»l, num2) { var sum = numl + nun2? return sum?
在这个函数内部，有3个私有变量：nuinl、num2和sum。在函数内部可以访问这儿个变进，但在 函数外部则不能访问它们。如果在这个函数内部创建-个闭包，那么闭包通过A己的作用域链也可以访 问这些变贵。而利用这一点，就可以创建用于访问私有变量的公有方法。
我们把也权访问私有变《和私有函数的公有方法称为特权方法（privilegedmethod)。有两种在对象 上创建特权方法的方式。第一种是在构造函数中定义特权方法，基本模式如下。
function MyObject(){
"私有变量和私有A数
var privatcVariable = 10;
function privateFunction(){ return false;
>
//神权方法
this.publicMethod = function (){ privateVariabXe++; return privatePunction();
>;
这个模式在构造闲数内部定义了所有私有变M和闲数。然后，乂继续创建了能够访问这些私有成员 的特权方法。能够在构造函数屮定义特权方法，是W为特权方法作为闭包有权访问在构造函数中定义的 所有变ii和函数。对这个例子而变:ftprivateVariable和函数privateFunction(>只能通过特 权方法publicMethod<)来访问。在创建MyObject的实例后，除f使用publicMeLhod()这一个途 径外，没存任何办法叶以直接访问privaieVariable和privat:eFunctionU。
利用私有和特权成员，可以隐藏那些不应该被直接修改的数据，例如：
function Person(name){
this.getName = function(){



this.setName = function (value) { name - value;
var person = new Person{"Nicholas"); alert(person.getName{)); //"Nicholas" person.setName(_GregM; alert(person.geLName()); //"Greg-
PrivilegedMethodExampleOl .htm
以丄:代码的构造函数中定义了两个特权方法：getName<)和setName()。这两个方法都可以在构 造函数外部使用，而且都有权访问私有变M name。但在Per son构造函数外部，没有任何办法访问name。 由于这两个方法是在构造函数内部定义的，它们作为闭仅能够通过作用域链访问name。私有变摄name 在Person的每一个实例中都不相同，因为每次调用构造函数都会重新创建这两个方法。不过，在构造
函数中定义特权方法也有一个缺点，那就是你必须使用构造函数模式来达到这个H的。第6章曾经i寸论 过，构造函数模式的缺点是针对每个实例都会创建同样一组新方法，而使用静态私有变M来实现特权方 法就可以避免这个问题。
###  7.4.1 静态私有变量
通过在私有作用域中定义私有变量或函数，同样也可以创建特权方法，其基本模式如下所示。
(function。{
//私有变黃和私有函数
var privateVariable = 10?
function privateFunction(){ return false;
}
//构造函数
MyCbjoct = function(){
};
//公有/特权方法
MyObject.prototype.publicMethod - function(){ privateVariable++; return privateFunction〇;
>;
} M);
这个模式创建了一个私有作用域，并在其中封装了一个构造函数及相应的方法。在私有作用域中， 首先定义了私冇变最和私有函数，然后又定义丫构造函数及其公有方法。公有方法是在原型上定义的， 这一点体现了典型的原型模式。需要注意的是，这个模式在定义构造函数时并没有使用函数声明，而是 使用了函数表达式。函数声明只能创建局部闲数，但那并不是我们想要的。出于同样的原因，我们也没 有在卢明MyObject时使用var关键字。记住：初始化未经声明的变量，总是会创建一个全局变量。 因此，KyObject就成厂一个全局变该，能够在私务作用域之外被访问到。但也要知道，在严格模式下 给未经声明的变fl賦值会导致错误。
这个模式与在构造函数中定义特权方法的主要K别，就在于私有变量和函数是由实例共享的。由于 特权方法是在原型上定义的，因此所有实例都使用同一个函数。而这个特权方法，作为一个闭包，总是 保存着对包含作用域的引用。来看一看下面的代码。
(function(){
var name = "*;
Person = function(value){ name = value;
};
Person.prototype.getName = function(){ returnname;
);
Person.prototype.setName = function (value){    •

7.4私有变量 189
name = value?
>;
})();
var personi = new Person("Nicholas"); alert{personl.getName()); //"Nicholas" personi.setName{"Greg"); alert{personi.getKame()); //"Greg"
var person2 = new Person("Michael*); alert(personi.getName()); //"Michael" alert (person2 . getName ⑴;// "Michael"
PrivilegedMethodExample02. htm
这个例T中的Person构造函数*5 getName ()和setName ( r方法一样,都有权访问私存变ft name。 在这种模式下，变最name就变成了一个静态的、由所有实例共亨的属性。也鱿是说，在一个实例上调 用setName (>会影响所有实例。而调用setName ()或新建'-个Person实例都会赋予name属性-个 新值。结果就是所有实例都会返间相同的值。
以这种方式创建静态私有变量会W为使用原型而增进代码复用，但每个实例都没有自己的私有变 fi。到底足使用实例变M,还&諍态私有变最终还足•要视你的具体需求而定。
f
多查找作用域链中的一个层次，就会在一定程度上影响查找速度。而这正是使用
闭包和私有变量的一个显明的不足之处。
###  7.4.2 模块模式
前面的模式是用f为G定义类沏创建私有变量和特权方法的。而道格拉斯所说的模块模式（module pattern)则是为单例创建私有变M和特权方法。所谓单例（singleton),指的就是只有一个实例的对象。 按照惯例，JavaScript是以对象7面量的方式来创建单例对象的。
var singleton = { name: value, method: function () {
//这里是方法的代码
}
);
模块模式通过为单例添加私有变和特权方法能够使其得到增强，其语法形式如下：
var singleton = function。{
//私有变f和私有函数
var privateVariable = 10;
function privateFunccionO { return false?

190 第7章函数表达式
//特权/公有方法和4性 return {
publicProporty： cruo,
publicMethod: function{){ privateVariable++? return privateFunctionO ;
);
X);
这个模块模式使用了一个返冋对象的匿名函数。在这个匿名函数内部，首先定义r私有变蛍和函数。 然后，将•个对象字w盘作为闲数的值返问。返回的对象字面觉中只包含可以公开的厲性和方法。由丁• 这个对象是在匿名函数内部定义的，因此它的公有方法有权访问私有变垃和函数。从本质上来讲，这个 对象字面最定义的是单例的公共接n。这种模式在需要对单例进行某些初始化，同时又需要维护其私有 变量时是非常有用的，例如：



var application = function(}{
//私有变责和函数
var components = new Array();
//初始化
components.push(new BaseComponont());
//公共 return {
getComponentCount: function(){ return components.length;
registerComponent: function(component){ if (typeof component "object*){ components.push(component);
)?
){)}
ModulePattemExampleOLhtm
在Web应用程序中，经常需要使用一个单例来管理应用程序级的信息。这个简单的例子创建了一 个用丁'管理组件的application对象。在创建这个对象的过程中，首先声明了-个私有的components 数^丨!•，并向数组中添加了一^t'BaseComponent的新实例（在这里不需要关心BaseComponent的代码，我 们只是用它来展示初始化操作)。而返回对象的getConpor-entCount (>和registerConponent:(>方法，都 是有权访问数组conponents的特权方法。前者只是返冋已汴册的组件数目，沿者用丁•注册新组件。
简言之，如果必须创建一个对象并以K些数据对其进行初始化，_、t还要公开一些能够访问这些私夯 数据的方法，那么就可以使用模块模式。以这种模式创建的每个单例都是Object的实例，W为最终要通 过一个对象字面量来表示它。亊实t,这也没有仆么;毕竞，单例通常都是作为全局对象存在的，我们不 会将它传递给一个函数。因此，也就没有什么必要使用instanceof操作符来检丧其对象类铟了。
###  7.4.3 增强的模块模式
有人进-步改进了模块模式，即在返回对象之前加人对其增强的代码。这种增强的模块模式适合那 些单例必须是某种类迤的实例，同时还必须添加某些属性和（或）方法对其加以增强的情况。来看下面 的例子。
var singleton = function^){
//私有变量和私有函数
var privateVariable = 10;
function privateFunction(){ return false;
//创建对象
var object = new CustomType();
//添加特权/公有属性和方法 object.publicProperty = true;
objecc.publicMethod = function{){ privateVariable++; return privateFunction()?
};
//返田这个对象 return object;
} 0 ;
如果前面演;模块模式的例子中的application对象必须是BaseComponent的实例，那么就可 以使用以下代码。
var application = function(){
//私有变"i■和函数
var components = new Array();
//初始化
components.push(new BaseComponent());
//创建application的一个局部副冬 var app = new BaseComponent{);
//公共接口
app.getComponentCount = function(){ return components.length;
>;
app.registerComponent = function(component){ if (typoof component =- "object” { components.push(component);
>
};
//返田这个副本

192 第7章函数表达式
return app;
M);
Module A ugmentationPattemExampleOl .htm
在这个重写后的应用程序（application)单例中，首先也是像前面例I1中一样定义了私有变虽。主 要的不同之处在于命名变量app的创建过程，因为它必须是BaseComponent的实例。这个实例实际上 是application对象的局部变M版。此后，我们又为叩p对象添加了能够访问私有变量的公有方法。 最后一步是返回app对象，结果仍然是将它賦值给全W变量application。
##  7.5 小结
在JavaScript编程屮，函数表达式是一种非常有用的技术。使用函数表达式可以无须对函数命名， 从而实现动态编程。匿名函数，也称为拉姆达函数，足种使用JavaScript函数的强大方式。以下总结 了函数表达式的特点。
- [ ] 函数表达式不同于函数声明。函数声明要求有名字，但函数表达式不需要。没有名字的函数表 达式也叫做匿名函数。
- [ ] 在无法确定如何引用函数的情况下，递归函数就会变得比较复杂;
- [ ] 递归函数应该始终使用arguments.callee来递归地调用自身，不要使用函数名——函数名可 能会发生变化。
3在函数内部定义f其他函数时，就创建了闭包。闭包有权访问包含闲数内部的所有变量，原理 如下。
- [ ] 在后台执行环境中，闭包的作阳域链包含着它&己的作用域、包含函数的作用域和全M作用域。 □通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。
- [ ] 仴是，当兩数返冋了一个闭包时，这个函数的作用域将会-直在内存中保存到闭包不存在为止。
使用闭包可以在JavaScript中模仿块级作用域（JavaScript本身没有块级作用域的概念)，要点如下。 
- [ ] 创建并立即调用一个函数，这样既可以执行其中的代码，又不会在内存中留下对该闲数的引用。 
- [ ] 结果就是函数内部的所有变最都会被立即销毁——除非将某些变量赋值给了包含作用域（即外 部作用域）中的变量。
闭包还可以用丁-在对象中创建私有变M，相关概念和要点如下。
- [ ] 即使JavaScript中没有正式的私宥对象属性的概念，但可以使用闭包来实现公有方法，而通过公 有方法可以访问在包含作用域中定义的变M。
- [ ] 有权访问私有变S的公有方法叫做特权方法5
- [ ]  口丨以使用构造函数模式、原型模式来实现自迮义类型的特权方法，也可以使用模块模式、增强 的模块模式来实现单例的特权方法。
JavaScript中的闲数表达式和闭包都足极其有用的特性，利用它们可以实现很多功能。不过，因为 创建闭包必须维护额外的作用域，所以过度使用它们可能会占用大M内存。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter6.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter8.md)