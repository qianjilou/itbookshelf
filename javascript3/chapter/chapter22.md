##  第22章 高级技巧([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 使用髙级函数
- [ ] 防篡改对象
- [ ] Yielding Timers  
avaScript是-种极其灵活的语言，具有多种使用风格。一般来说，编¥ JavaScript要么使用过 U程方式，要么使用面向对象方式。然而，由于它天生的动态M性，这种语言还能使用更为复杂 和有趣的模式。这些技巧要利用ECMAScript的语言特点、BOM扩展和DOM功能来获得强大的效果。
22.1高级函数
函数是JavaScript中最有趣的部分之一。它们本质上娃f •分简单和过程化的，但也可以是非常复杂 和动态的。一些额外的功能可以通过使坩闭包来实现。此外，由于所有的函数都是对象，所以使用函数 指针非常简单。这些令JavaScript函数不仅有趣而且强大。以下几节描绘了几种在JavaScript中使用函数 的高级方法。
22.1.1安全的类型检测
JavaScript内置的类型检测机制并非完全可靠。亊实h,发生错误杏定及错误肯定的情况也不在少 数。比如说typeof操作符吧，由于它有-邛无法预知的行为，经常会导致检测数据类型时得到不靠谱 的结果。Safari (立至第4版）在对正则表达式应用；:ypeof操作符时会返回-function”，因此很难确 定某个值到底是不是函数。
再比如，instanceof操作符在存在多个全局作用域（像一个页面包含多个框架）的情况下，也娃 问题多多。一个经典的例子（第5章也提到过）就是像下面这样将对象标识为数组。
var isArray « value instanceof Array；
以上代码要返M true, value必须是--个数组，而II还必须与Array构造函数在同个全局作用域 中。（别忘了，Array是window的属性。）如果value是在另个框架中定义的数组，那么以上代码 就会返回fal.se。
在检测某个对象到底是原生对象还是开发人员(1定义的对象的时候，也会有问5^出现这个问题的
原因是浏览器开始原生支持JSON对象了。因为很多人一直在使用Douglas Crockford的JSON库，时该
库定义丫一个全局JSON对象。于是开发人员很难确定页面屮的JSON对象到底是不是原生的。
解决上述问题的办法都一样。大家知道，在任何值上调用Object原生的t〇String()方法，都会
«

22.1高级函数	597
返回一个[object MativeConstructorName]格式的字符串。每个类在内部都有一个[[Class] J属 性，这个属性中就指定了上述字符串中的构造函数名。举个例子吧。
alert(Object.prototype.toString.call(value))；	//"(object Array1 *
由于原生数组的构造函数名与全局作用域无关，因此使用t〇String()就能保证返回一致的值。利 用这一点，可以创建如下函数：
function isArray(value){
return Object.prototype.toString.call(value) == *【object Array]”；
}
同样，也可以基丁•这一思路来测试某个值是不是原生函数或正则表达式：
function isFunction{value){
return Object.prototype.toString.call(value) == •[object Function)"；
)
function isRegExp(value){
return Object.prototype.toString.call(value) == "(object RegExp]•；
}
不过要注意，对于在IE中以COM对象形式实现的任何函数，iSFuncti〇n()都将返回false (因 为它们并非原生的JavaScript函数，请参考第10章中更洋细的介绍）。
这一技巧也广泛应用于检测原生JSON对象。Object的toString U方法不能检测非原生构造闲 数的构造函数名。因此，开发人员定义的任何构造函数都将返回[object Object]。有些JavaScript库会包 含与下面类似的代码。



var isNaCiveJSON = window,JS0N && Object.prototype.toString.call(JS0N) "(object JSON]';
在Web开发中能够区分原生与非原生JavaScript对象非常重要。只有这样才能确切知道某个对象到 底有哪些功能。这个技巧可以对任何对象给出正确的结论。
请注意，Object.prototpye.t:oSt;ring()本身也可能会被修改。本节讨论的 技巧假设Obj ect. prototpye. toString ()是未被修改过的康生版本。
22.1.2作用域安全的构造函数
第6章讲述了用于自定义对象的构造函数的定义和用法。你应该还记得，构造函数其实就是一个使 用new操作符调用的函数。当使用new调用时，构造函数内用到的this对象会指向新创建的对象实 例，如下面的例子所示：



function Person(name, age, job){ this.name * name； this.age = age； this.job = job;
var person = new Person("Nicholas", 29, "Software Engineer*)；
ScopeSafeConstructorsExampIeO /. htm
22

598 第22章高级技巧
h面这个例子中，Person构造蝴数使用this对象给三个属性赋值：name、age和job。鸟和new 操作符连用时.则会创建一个新的Person对象，冏时会给它分配这些属件。问题出在当没办使用new 操作符来调用该构造函数的情况上。由于该this对象是在运行时绑定的，所以直接调用Person (), this会映射到全局对象window丨:，导致错误对象M性的意外增加。例如：
var person = Person ("Nicholas", 29, "Software Engineer*1);
alert(window.name);	//"Nicholas*
alert(window.age)；	//29
alert{window.job);	//"Software Engineer”
ScopeSafeConstructorsExampleOl. htm
这里，原本针对Person实例的三个域性被加到window对象上，丙为构造函数是作为鞞通函数调 ffl的，忽略丫 new操作符。这个N题是由this对象的晚绑定造成的，在这里this被解析成丫 window 对象。由r-Window的name属性是用于识别链接R标和框架的，所以这里对该属性的偶然覆盖可能会 导致该页曲h出现其他错误。这个问题的解决方法就足创建一个作用域安全的构造函数。
作用域安全的构造函数在进行任何史改前，酋先确认this对象足•正确类铟的实例。如果不是，那 么会创建新的实例并返冋。请看以下例子：
function Person(name, age, job){
If (this instanceof Person){ this, name = r.air.e； this.age = age； this.job - job；
} else {
return new Person(name, age, job)；
)
var peraonl s Person("Nicholas", 29, uSoftware Engineern>；
alert (window.nsune);	//""
alert(personl.name);	//"Nicholas"
var person2 = new Person("Shelby", 34, "Ergonomist®)； alert <person2.name} /	//"Shelby*1
ScopeSafeConstructornExampk02.htm
这段代码中的Person构造函数添加丫一个检査并确保this对象是Person实例的if语句，它 表示要么使用new操作符，要么在现有的Person实例环境中调用构造函数。任何一种情况下，对象初 始化都能:iK常进行。如果this并非Person的实例，那么会再次使用new操作符调用构造函数并返冋 结采。最后的结果是，调用Person构造函数时尤论是否使用new操作符，都会返冋一个person的新 实例，这就避免了在仝W对象上意外设置14性。
关于作用域安仝的构造函数的贴心提示。实现这个模式后，你就锁记了可以调用构造函数的环境。 如采你使ffl构造函数窃取模式的继承J1+使用®型链，那么这个继承很可能被破坏。这里有个例子：
function Polygon.(sides) {
if (this instanceof Polygon) { this.sides = sides? this.getArea - function(){



22.1高级函数	599
return 0；
)；
} else {
return new Polygon(sides);



function Rectangle(width, height){ Polygon.call{this, 2); this.width = width； this*height = height; this.getArea = function(){
return this.width * this.height;
var rect = new Rectangle(5# 10)； alert(rect.sides);	//undefined
ScopeSafeConstructorsExample03. htm
在这段代码中，Polygon构造函数是作用域安全的，然而Rectangle构造函数贝!|不是。新创建一 个Rectangle实例之后，这个实例应该通过Polygon.call ()来继承Polygon的sides属性。但是， 由于Polygon构造函数是作用域安全的，this对象并非Polygon的实例，所以会创建并返回一个新 的Polygon对象。Rectangle构造闲数中的this对象并没有得到增长，同时Polygon.call (>返回 的值也没有用到，所以Rectangle实例中就不会有sides属性。
如果构造函数窃取结合使用原型链或者寄牛组合则可以解决这个问题。考虑以下例子：



function Polygon(sides){
if (this instanceof Polygon) {
this.sides = sides;
this.getArea = function{){
return 0;
>;
} else {
return new Polygon(sides);
}
function Rectangle(width, height){ Polygon.call(this, 2)? this.width = width； this.height = height； this.getArea = function{){
return this.width * this.height;
};
)
Rectangle.prototype s new Polygon();
var rect = new Rectangle(5, 10)； alert(rect,sides)；	//2
ScopeSafeConstructorsExample04. htm
22

600 第22章高级技巧
上面这段S写的代码巾，一个Rectangle实例也同时是••个polygon实例，所以polygon.call (> 会照原意执行，最终为Rectangle实例添加了 sides M性。
多个程序员在同一个页面上写JavaScript代码的环境中，作用域安全构造函数就很有用了。届时， 对全《对象意外的更改可能会导致一些常常难以追踪的错误。除非你单纯基于构造函数窃取來实现继 承，推荐作;《域安全的构造涵数作为最佳实践。
22.1.3惰性载入函数
W为浏览器之间行为的差异，多数JavaScript代码包含了大麽的if语句，将执行引导到正确的代 码中。宥看下面来自上一皁的creal:eXHR()函数。
function creat〇XHR{){
if (typeof XKLHttpRequest l= "undefined"){ return new XKLHttpReguost();
} else if (typcoC ActiveXObjecr 1= *undefinod"){
if (typeof arguments.calico.activeXString "string") {
var versions = [ -MSXML2 .XMLKcr.p.6.0", "MSXML2 .XMLHttp.3.0",
-MSXML2 .XMLHttp**!,
i,len ?
for (i=0,ler.-verisions.length； i < len； i++) { try {
new AccivoXObject(versions(i])； arguments.callee.act iveXString = versionsti); break；
> catch (eix) {
//跳过
}
return new ActiveXObject(arguments.callee.activeXString)；
> else {
throv/ now Error {"No XHR object available.")；
}
}
每次调用createXHRO的时候，它都要对浏览器所支持的能力仔细检査。首先检査内置的XHR, 然后测试冇没有基于ActiveX的XHR,最沿如果都没有发现的话就抛出一个错误。每次调用该函数都是 这样，即使每次调用时分支的结果都不变：如果浏览器支持内K XHR,那么它就一直支持了，那么这 种测试就变得没必要J*。即使只有一个if语句的代码，也肯定要比没有if语的慢，所以如果if语 句不必每次执行，那么代码可以运行地更快-鸣。解决方案就是称之为惰性载人的技巧。
惰性栽人表示函数执行的分支仅会发生一次。有两种实现惰性载入的方式，第一种就是在确数被调 用时再处理函数。在第一次调用的过程中，该函数会被覆盖为另外一个按合适方式执行的函数，这样任 何对原函数的调用都不用再羟过执行的分支了。例如，可以用下面的方式使用惰性载人重写 createXHRO 〇
function createXHR(){
if (typeof XMLHttpRequest 1= "undefinedw){ createXHR s function()<

22.1 高级函数	601
return new XMLHttpReouestC);
>;
} else if (typeof ActiveXObject != "undefined"){ createXHR s £unction(){
if (typeof argumentB.callee.activeXString 1= "string"){
var versions » ["MSXML2.XKLHttp.6.0N, nMS£ML2.XKLHttp.3.0Mr HMSXML2 .ZMLHttp"],
i, len;
for (!■0,lensversiona.length; i < len; i++){ try {
new ActiveXObject(versions[i]); arguments.callee.activeXString = versionsti]? break；
catch (ex){
//skip
>
)
}
return new ActiveXObject (argrvuments.callee.activeXString);
}；
} else {
createXHR * function(){
throw new Error(nNo XHR object available*");
);
)
return createXHR<)；
LazyLoadingExampleOJ.htm
在这个惰性载人的createXHR()中，if语句的每一个分支都会为createXHR变量赋值，有效覆 盖了原有的函数。最后一步便是调用新賦的函数。下一次调用createXHR()的时候，就会直接调用被 分配的函数，这样就不用再次执行if语句了。
第二种实现惰性栽人的方式是在声明函数时就指定适当的闲数。这样，第一次调用函数时就不会损 失性能了，而在代码首次加载时会损失一点性能。以下就是按照这一思路重写前面例子的结果。
var createXHR * <£unction()<
if (typeof XMLHttpRequest != "undefined"){ return function(){
return new XMLHttpRequest();
>;
)else if (typeof ActiveXObject != "undefined*){ return function(){
if (typeof arguments.callee.activeXString != •string*){
var versions = [-MSXML2.XMLHCCp.6.0% -MSXML2.XMLHttp.3.〇■, ■MSXML2.XMLHttp-]/
i, len;
for (i=0,Xen=versions.length； i < len； i++){ try {
new ActiveXObject(versions[i])；
arguments.callee.activeXString = versionsti);
break;
} catch (ex){
//skip

602 第22章高级技巧
return new ActiveXOb：iect:(argument:s.callee.acciveXString};
};
} else {
return. function(){
throw new Error ("No XHR object available. **)；
};
}
}> ();
LazyLoadingExample02. him
这个例子中使用的技巧是创建一个匿名、自执行的函数，用以确定应该使用哪一个函数实现。实际 的逻辑都一样。不一样的地方就是第一行代码（使用var定义函数）、新增了自执行的匿名函数，另外 每个分支都返H正确的函数定义，以便立即将其賦值给createXHR()。
惰性载人函数的优点是只在执行分支代码时牺牲一点儿性能。至于哪种方式更合适，就要看你的具 体需求而定了。不过这两种方式都能避免执行不必要的代码。
22.1.4函数鄉定
另一个日益流行的髙级技巧叫做喊数绑定。函数绑定要创建一个函数，可以在特定的this环境中 以指定参数调用另•个函数。该技巧常常和回调函数与事件处理程序一起使用，以便在将函数作为变量 传递的同时保留代码执行环境。清宥以1'例子：
var handler ^ {
message: "Event handled",
handleClick： function(event){ alert(this.message)?
}
};
var btn - document.getElementById(*my-btn")；
EventUt.il .addHandler (btri/ "click*, handler .handleCiick)；
在h面这个例子中，创建了一个叫做handler的对象。handler.handleClick()方法被分配为 一个DOM按钮的事件处理程序。.当按下该按钮时，就调用该函数，砥示一个警告框。虽然貌似警告框 应该显不Event handled,然而实际上显示的是undefiend。这个问题在于没有保存 handler.handleClickO的环境，所以chis对象最后是指向了 D0M按钮而非handler (在IE8中， this指向window。）可以如下面例子所示，使用个闭包来修正这个问题。
var handler = {
message： "Kvent handled",
handleCiick: function(event){ alert{this.message)；
>
};
var btn = document.getElementByldt"my-bcn")； Eventntil. addHandler (htn, "clicJc" $ function (event) { handler .handleClich(evexit);
})>

22.1 高级函数	603
这个解决方案在onclick事件处理程序内使用T一个闭包立接调用handler.handleClick(>。 当然，这是特定于这段代码的解决方案。创建多个闭包可能会令代码变得难于理解和调试。W此，很多 JavaScript库实现了一个可以将函数绑定到指定环境的函数。这个函数一般都叫bind (>。
一个简单的bind ()函数接受一个函数和一个环境，并返回一个在给定环境中调用给定函数的函数, 并且将所有参数原封不动传递过去。语法如下：
function bind(fn, context){
return function(){
return fn.apply(context, arguments);
};
FunctionBindingExampleOl. htm
这个函数似乎简单，但其功能是非常强大的。在bind ()中创建了 -个闭包，闭包使用apply (>调 用传人的兩数，并给apply ()传递context对象和参数。注意这里使用的arguments对象是内部函 数的，而非bind()的。当调用返回的函数时，它会在给定环境中执行被传人的函数并给出所有参数。 bind (>函数按如下方式使用：
var handler = {
message： "Event handled*,
handledick: function(event){
alert(this.message)；
}
};
var btn = document.getElementById("my-btn-)；
BventUtil.addHandler(btn, "click*, bind(handler.handleClicX, handler));
FunctionBindingExampleOl. htm
在这个例子中，我们用bind()函数创建了一个保持了执行环境的函数，并将其传给EventUtU. addHandlerU。event对象也被传给了该阐数，如下所示：
var handler = {
message： "Event handled",
handleClick： function(event){
alert<tbiB.message	": *• + event.type)；
}
};
var btn = document .gctElementByld(*iny-btn-);
EventUtil.addHandler{btn, "click*, bind{handler.handledick, handler))；
FunctionBindingExampleOl. htm
handler.handleClickO方法和平时--样获得了 event对象，因为所有的参数都通过被绑定的函 数直接传给了它。
ECMAScript 5为所有闲数定义了一个原生的bind ()方法，进一步简单了操作。换句话说，你不用 再fi己定义bind (>函数了，而是可以直接在函数上调用这个方法。例如：
22

604 第22章高级技巧
var handler = {
message： "Event handled",
handleClick： function(event){
alert(this.message + n：n + event.type);
}
};
var btn = document.getElementByld("my-btn");
EventUtil. addHar^dler (totn, "click", handler. handleClick. bind (handler))；
FunctionBindingExample02.htm
原生的oind (>方法与前面介绍的f丨定义bind ()方法类似，都是要传人作为this值的对象。支持 原生bind ()方法的浏览器有IE9+、Firefox 4+和Chrome。
只要是将某个函数指针以值的形式进行传递，同时该函数必须在特定环境中执行，被绑定函数的效 用就突M出来了。它们主要用于亊件处理程序以及setTimeouM)和setlntervalO。然而，被绑 定闲数与普通函数相比有史多的开销，它们®要更多内存，同时也W为多歌函数调用稍微悛一点，所 以M好只在必要时使用。
22.1.5函数柯里化
与闲数绑定紧密相关的主题是函数柯里化（fiinction cunying),它用丁-创建已经设置好了一个或多 个参数的函数。函数柯里化的基本方法和函数绑定必一样的：使用一个闭也返冋一个函数。两者的区别 在T,当函数被调用时，返回的函数还需要设H—鸣传人的参数。请看以下例子。
function add(numl, num2){ return numl + num2；
)
function curriedAad(num2){ return add(5, num2)?
>
alort(add(2, 3));	//5
alerc(curriedAdd(3)); //8
这段代码定义了两个函数：add <>和curriedAdd (>。后者本质上是在任何情况下第一个参数为5 的add()版本。尽管从技术上来说curriedAdcK)并非柯里化的函数，但它很好地展示了其概念。
柯串.化函数通常由以下步骤动态创建：调用另-个函数并为它传人要柯里化的函数和必要参数。下 W足创建柯里化闲数的通用方式。
function curry(fn){
var args = Array.prototype.s1ice.call(arguments, 1)； return function(){
var innerArgs •• Array.prototype.slice.call (arguments); var finalArgs = args.concat(innerArgs); return fn.apply(null, finalArgs);
}；
FurtctionCurryingExampleOl .htm

22.1 高级函数 605
curry ()阐数的主要T作就是将被返间函数的参数进行排序。curry U的第一个参数是要进行柯里 化的函数，其他参数是要传人的值。为了获取第-个参数之耵的所有参数，在arguments对象h调用 了 slice()方法，并传人参数1表示被返回的数组包含从第二个参数开始的所有参数。然后argS数组 包含了来自外部函数的参数。在内部函数中，创建了 innerArgS数组用来存放所有传人的参数（乂一 次用到了 slice(> )。有了存放来自外部函数和内部函数的参数数组后，就可以使用concatO方法将 它们组合为finalArgs,然后使用apply ()将结果传递给该函数。注意这个函数并没有考虑到执行环 境，所以调用applyO时第一个参数是mall。CUrry()函数可以按以下方式应用。



function add(numl/ num2){ return numl + num2?
var curriedAdd = curry(add, 5); alert(curriedAdd(3))t //8
FunctionCurryingExampIeOJ. htm
在这个例子中，创建了第一个参数绑定为5的add (>的柯里化版本。当调用curriedAdd()并传 人3时，3会成为add()的第二个参数，同时第一个参数依然是5,最后结果便是和8。你也可以像下 面例子这样给出所有的函数参数：
function add{numl, num2){ return numl + num2;
}
var curriedAdd » curry(add, 5, 12); alert(curriedAdd()>；	//17
FunctionCunyingExample01.htm
在这里，柯里化的add ()函数两个参数都提供了，所以以后就无需再传递它们了。
函数柯里化还常常作为函数绑定的-部分包含在其中，构造出更为复杂的bind<)函数。例如:
function bind(£n, context){
var args = Array.prototype.slice.call(argumenta, 2);
return function(){
var innerArgs = Array.prototype.slice.call{arguments)； var finalArgs = args.concat(innerArgs); r«tum £n.apply(context» final Args);
}；
22
FunctiortCurryingExample02. htm
对curry ()函数的主要更改在于传入的参数个数，以及它如何影响代码的结果。curry ()仅仅接受 -个要包裹的函数作为参数，而bindO同时接受函数和…个object对象。这表示给被綁定的函数的参 数是从第三个开始而不是第二个，这就要更改slice ()的第一处调用。另一处史'改是在倒数第3行将 object对象传给apply ()。$使用bind (>时，它会返回绑定到给定环境的函数，并且可能它其中某些 确数参数已经被设好。当你想除了 event对象再额外给事件处理程序传递参数时，这非常有用，例如：

606 第22章高级技巧



var handler = {
message： "Event handled",
handleClick: function(name, event){
alert (this .message + **:**+ name + ":*♦ event, type);
>
>；
var btn = document.getEleroentById("my-btn")；
EventUti1.addHandler(btn, "click", bind(handler.handleClick, handler, "iay-btn"))；
FunctionCurryingExample02.htm
在这个更新过的例子中，handler.handleClickO方法接受了两个参数：要处理的元素的名字和 event对象。作为第三个参数传递给bind()闲数的名字，又被传递给了}iandler.hancileCliclc()， 时handler .handleClick<)也会同时接收到event:对象。
ECMAScript5的bind()方法也实现函数柯里化，只要在tMS的值之后再传人另一个参数即可。
var handler = {
message: "Event handled1*#
handleClick： function(name, event){
alert(this.message + ":" ♦ name + "：" + event.type);
)
var btn = documenc. getElementByld ("rry-htn")；
SventUtil.addHandler(btn, "click", handler.handleClick.bind(handler# "my-btn"))/
FunctionCurryingExampie03.htm
JavaScript中的柯里化函数和绑定函数提供了强大的动态函数创建功能。使用bind U还是curry () 要根据是否需要object对象响应来决它们都能M于创建复杂的算法和功能，当然两者都不应滥用， 闵为每个阐数都会带来额外的开销。
22.2防篡改对象
JavaScript共享的本质一直足开发人员心头的痛。因为任何对象都可以被在同一环境中运行的代码 修改。开发人员很可能会意外地修改别人的代码，共至更糟糕地，用不兼容的功能®写原生对象。 ECMAScript 5致力于解决这个问题，可以让开发人员定义防萬改对象（tamper-proof object)。
第6章讨论/对象属性的问题，也讨论了如何手_ r.设置每个属性的[[Configurable]]、 [[Writable]]、[[Enumerable]]、[[Value】]、[【Get]]以及[[Set]]特性，以改变属性的行为。 类似地，ECMAScript5也增加了儿个方法，通过它们可以指定对象的行为。
不过请注意：一曰.把对象定义为防®改，就无法撤销了。
22.2.1不可扩展对象
默认情况下，所宥对象都是可以扩展的。也就是说，任何时候都可以向对象屮添加属性和方法。例 如，可以像F面这样先定义一个对象，后来W给它添加-个属性。

22.2防篡改对象 607
var person = { name： "Nicholas" J; person.age = 29?
即使第一行代码已经完幣定义person对象，但第二行代码仍然能给它添加属性。现在，使用 Object .preventExtensions ()方法可以改变这个行为，让你不能再給对象添加属性和方法。 例如：
var person = { name： "Nicholas- }; Object.preventExtensions(person);
person.age = 29；
alert(person.age)； //undefined
NonExiensibleObjectsExampleOI. htm
在调用i" Object .preventExtensions ()方法后，就不能给person对象添加新属性和方法了〇 在非严格模式下，给对象添加新成员会导致静默失败，因此person.age将是undefined。而在严格 模式下，尝试给不可扩展的对象添加新成员会导致拋出错误。
虽然不能给对象添加新成员，但C有的成员则丝毫不受影响。你仍然还可以修改和删除已有的成员。 另外，使用Object. istExtensiblet)方法还可以确定对象是否可以扩展〇



var person = { name: "Nicholas" )?
al«rt (Object • iaBxtezxsible(person)) ；	//true
Object.preventExtensions(person);
alert(0bject*is8xtensible(perB〇n))；	//false
NonExtensibieObjectsExample02.htm
22.2.2密封的对象
ECMAScript 5为对象定义的第二个保护级別是密封对象（sealed object)。密封对象不可扩展，而
且已有成员的[[Configurable]]特性将被设S为false。这就意味着不能删除堝性和方法，因为不能 使用Object_.defineProperty(>把数据屈性修改为访问器M性，或者相反。诚性值是可以修改的。 要密封对象，可以使用Object. seal ()方法。



var person = { name： •Nicholas");
Object.seal(person);
person.age = 29；
alert(person.age)?	//undefined
delete person.name;
alert{person.name)；	//"Nicholas"
SealedObjectsExampleO l. htm
在这个例子中，添加age屑性的行为被忽略了。而尝试删除name属性的操作也被忽略J",因此这 个属性没有受任何影响。这焰在非严格模式下的行为。在严格模式下，尝试添加或删除对象成员都会导 致抛出错误。
22

608 第22章高级技巧
使用Object.isSealedO方法可以确定对象是否被密封了。因为被密封的对象不可扩展，所以用 Object;. isExtensible U检测密封的对象也会返冋false。 var person = { name： "Nicholas" }; alert(Objact.i丨Bxten0iblo<peraon)); //true alert (objact .iaSealedtpersozi));	//false
Object.seal(person)；
alert (Object. i8Bxten.8lble (person)) 7 //falpe alert(Object.i8SeaXed(person))；	//true
SeakdObjectsExample02.htm
22.2.3冻结的对象
最严格的防篡改级别是冻结对象（frown object)。冻结的对象既不可扩展，又是密封的，而且对象 数据属性的I [Writable]]特性会被设置为false。如果定义[[Set]]函数，访问器属性仍然是可写的。 ECMAScript 5定义的Object. freeze ()方法可以用来冻结对象。
var person * ( name: "Nicholas* };
Object.freeze(pereon)/
person.age = 29；
alert{person.age);	//undefined
delete person.name；
alert(person.name)；	//"Nicholas*
person.name = "Greg"；
alert (person.name) ； /"Nicholas*
FrozenObjectsExampleQl .htm
与密封和不允许扩展一样，对冻结的对象执行非法操作在非严格模式下会被忽略，而在严格模式下 会抛出错误。
当然，也有一个Object.isFrozenU方法用丁*检渕冻结对象。因为冻结对象既是密封的又是不可 扩展的，所以用Object:.isExtensible<)和Object.isSealedt)检测冻结对象将分別返回false 和 true〇
var person = { name： "Nicholas" }； alert(Object.isExtensible(person))；	//true
aler^(Object.isSealed(person));	//false
aler-(Object.isFrozen(person));	//false
Object.freeze(person)；
alcr- (Object. isExtensible (person) J	；	"false
aler^(Object.isSealed(person))；	//true
alert(Object.isVr〇2en(person));	//true
FrozenObjectsExampleQ2.htm

22.3 高级定时器 609
对JavaScript库的作者而言，冻结对象是很冇用的。因为JavaScript库最怕有人意外（或有意）地修 改了库中的核心对象。冻结（或密封）土要的库对象能够防止这些问题的发生。
22.3高级定时器
使用setTimeout()和setlntervalU创建的定时器可以用于实现有趣丑有用的功能。虽然人们 对JavaScript的定时器存在普遍的误解，认为它们是线程，其实JavaScript是运行于单线程的环境中的， 而定时器仅仅只是计划代码在未来的某个时间执行。执行时机是不能保证的，因为在页面的生命周期中， 不同时间可能有其他代码在控制JavaScript进程。在页面下载完后的代码运行、事件处理程序、Ajax冋 调函数都必须使用同样的线程来执行。实际上，浏览器负责进行排序，指派某段代码在某个时间点运行 的优先级。
可以把JavaScript想象成在时间线上运行的。当页面载人时，首先执行是任何包含在<3«41：>元索 中的代码，通常是页面生命周期后曲要用到的•些简单的函数和变tt的卢明，不过有时候也包含一些初 始数据的处理。在这之后，JavaScript进程将等待更多代码执行。当进程空闲的时候，F—个代码会被 触发并立刻执行。例如，当点击某个按钮时，onclick亊件处理程序会立刻执行，只要JavaScript进程 处于空闲状态。这样一个页曲的时间线类似丁-阁22-1。
JavaScript进程时间线
页面的初始加栽	空闲	handle Click))
0	100	200	300	400	500	600	700	800
电位：奄秒 W 22-1
除丫主JavaScript执行进程外，还有一个«要在进程下一次空W时执行的代码队列。随着页面在其 生命周期中的推移，代码会按照执行顺序添加人队列。例如，当某个按钮被按下时，它的事件处理程序 代码就会被添加到队列中，并在下••个可能的时间里执行。当接收到某个Ajax响应时，回调函数的代 码会被添加到队列。在JavaScript中没有任何代码是立刻执行的，佴一 11进程空闲则尽快执行。
定时器对队列的工作方式是，气特定时间过去后将代码插人。注意，给队列添加代码并不意味着对 它立刻执行，而只能表示它会尽快执行。设定一个150ms后执行的定时器不代表到了 150ms代码就立刻 执行，它表示代码会在150ms后被加人到队列中。如果在这个时间点上，队列中没有其他东西，那么这 段代码就会被执行，表面上看上去好像代码就在精确指定的时间点上执行1%其他情况下，代码可能明 显地等待更长时间才执行。
请ft以下代码：
var btn = document .gotElemcntByld (*ir.y-btn");
btn.onclick = function(){ setTimeout(f uncLion()(
document: .get Rl ementByld (•message*') . style .visibility = "visible"；
}, 250);
//其他代码
22

610 第22章高级技巧
在这里给-个按钮设置了一个亊件处理程序。事件处理程序设置了一个250ms后调用的定时器。 点击该按钮后，首先将onclick事件处理程序加人队列。该程序执行后才设置定时器，再有250ms 后，指定的代码才被添加到队列中等待执行。实际上，对setTimeoutU的调用表示要晚点执行某些 代码。
关丁-边时器要记住的最*要的事情是，指定的时间间隔表示何时将定时器的代码添加到队列，而不 足何时实际执行代码。如果前面例子中的onclick事件处理程序执行了 300ms,那么定时器的代码至 少要在定时器设置之后的300ms后才会被执行。队列中所有的代码都要等到JavaScript进程空闲之后才 能执行，而不管它们楚如何添加到队列中的。见闬22-2。
JavaScript进程时间线 onclick	_时剧弋巧
1 I	I ~ri	I ~~|	|	|
0	100	200	300	400	500	600	700	800
255
定时器^码添加到队列中
单位：毫秒 5
创建/间隔为250的定时器
图 22-2
如阁22-2所示，尽管在255ms处添加丫定时器代码，但这时候还不能执行，因为onclick事件处 理程序仍在运行。定时器代码最早能执行的时机是在300ms处，即onclick事件处理程序结束之后。
实际上Firefox屮定时器的实现还能让你确定定时器过了多久才执行，这需传递一个实际执行的时 间与指定的间隔的差值。如下面的例子所示。
//仅 Firefox 中 setTimeout(function(diff){ if (diff > 0) {
//«t婀用
> else if (diff < 0){
//早调用 } else {
//调用及时
}
}, 250)；
执行完一套代码后，JavaScript进程返N—段很短的时间，这样页面上的其他处理就可以进行了。 由于JavaScript进程会限塞其他页面处理.所以必须有这些小间隔来防止用户界面被锁定（代码长时间 运行中还有可能出现）。这样设置一个定时器，可以确保在定时器代码执行前至少有个进程间隔。
22.3.1重复的定时器
使用setlntervalO创建的定时器确保了定时器代码规则地插人队列中。这个方式的问题在于， 定时器代码可能在代码再次被添加到队列之前还没有完成执行，结果导致定时器代码连续运行好儿次， 而之间没朽任何停顿。幸好，JavaScript引擎够聪明，能避免这个问题。当使用secintervalU时，仅 气没有该定时器的任何其他代码实例时，才将定时器代码添加到队列中。这确保了定时器代码加人到队

22.3	高级定时器	611
列中的最小时间间隔为指定间隔。
这种重复定时器的规则有2点问题：（1)某些间隔会被跳过；（2)多个定时器的代码执行之间的间隔 可能会比预期的小。假设，某个onclick亊件处理程序使用setlnterval ()设置了一个200ras间隔 的重复定时器。如果事件处理程序花了 300ms多一点的时间完成，N时定时器代码也花了差不多的时间， 就会跳过一个间隔同时运行着一个定时器代码。参见图22-3。
JavaScript进程时间线
onclick	定时器代码	|定时器代码
0	100	200	300	400	500	600	700	800
405	定时器代码被跳过
205	定时器代码添加到队列中
定时器代码添加到队列中
：,	单位：毫秒
创建了间隔为200的定时器
图 22-3
这个例？中的第丨个定时器是在205ms处添加到队列中的，{U是直到过了 300ms处才能够执行。当 执行这个定时器代码时，在405ms处又给队列添加了另外一个副本。在下一个间隔，即605ms处，第一 个定时器代码仍在运行，同时在队列中已经有f -个定时器代码的实例。结果是，在这个时间点h的定 时器代码不会被添加到队列中。结果在5ms处添加的定时器代码结束之后，405ms处添加的定时器代码 就立刻执行。
为了避免setlnterval < >的重复定时器的这2个缺点，你nj*以用如下模式使用链式setTimeout U 调用。
setTimeout(function(){
//处理中
setTimeout(arguments.callee, interval)；
}, interval);
这个模式链式调用了 seCTimeouU),每次函数执行的时候都会创建一个新的定时器。第二个 setTimeoutU调用使用了 arguments.callee来获取对当前执行的函数的引用，并为其设置另外一 个定时器。这样做的好处是，在前一个定时器代码执行完之前，不会向队列插人新的定时器代码，确保 不会有任何缺失的间隔。而且，它可以保证在下一次定时器代码执行之前，至少要等待指定的间隔，避 免了连续的运行。这个模式主要用于重复定时器，如下例所示。



setTimeout(function(>{
var div = document.getElementByld("myDiv")； left = parselnt(div.style.left) + b; div.style.left = left + "px *；
22
if (left < 200){

612 第22章高级技巧
setTimeout(arguments.callee, 50);
}
}, 50);
RepeatingTimersExampie.htm
这段定时器代码每次执行的时候将一个<div>元素向右移动，当左坐标在200像索的时候停止。 JavaScript动両中使用这个模式很常见。
(^\	每个浏览器窗口、标签页、或者框架都有其各自的代码执行队列。这意味着，进
行跨框架或者跨窗口的定时调用，当代码同时执行的时候可能会导致竞争条件。无论 何时需要使用这种通信类型，最好是在接收框架或者窗口中创建一个定时器来执行 代码。
Yielding Processes
运行在浏览器中的JavaScript都被分配了一个确定数fi的资源。不同于桌面应用往往能够随意控制 他们要的内存大小和处理器时间，JavaScript被严格限制了，以防止恶意的Web程序员把用户的计算机 搞挂了。其中一个限制是长时间运行脚本的制约，如果代码运行超过特定的时间或者特定语句数贵就不 让它继续执行。如果代码达到丫这个限制，会弹出一个浏览器错误的对话框，告诉用户某个脚本会用过 长的时间执行，询问是允许其继续执行还是停止它。所有JavaScript开发人员的目标就是，确保用户永 远不会在浏览器中看到这个令人费解的对话框。定时器是绕开此限制的方法之一。
脚本长时间运行的问题通常是由两个原W之一造成的：过长的、过深嵌套的函数调用或者是进行大 ft处理的循环。这两者中，后者是较为容易解决的问题。长时间运行的循环通常遵循以下模式：
for {var i=0, len=data.length； i < Ten； i++){ process(data[iI>;
)
这个模式的问题在于要处理的项0的数tt在运行前是不可知的。如果完成process ()要花丨00ms, 只有2个项U的数组可能不会造成影响，但是1〇个的数组可能会导致脚本要运行一秒钟才能完成。数 组中的项目数量直接关系到执行完该循环的时间长度。同时由于JavaScript的执行是一个阻塞操作，脚 本运行所花时间越久，用户无法与贞面交.互的时间也越久。
在M开该循环之盼，你7S要冋答以下两个重要的问题。
- [ ] 该处理是否必须同步完成？如果这个数据的处理会造成其他运行的阻塞，那么最好不要改动它。 不过，如果你对这个问题的凹答确定为“否”，那么将某些处理推迟到以后是个不错的备选项。
- [ ] 数据是否必须按顺序完成？通常，数组只是对项目的组合和迭代的一种简便的方法而无所谓顺 序。如果项{J的顺序不是非常重要，那么可能吋以将某#处理推迟到以后。
当你发现某个循环占用了人1时间，同时对于上述两个问题，你的回答都是“否”，那么你就可以 使用定时器分割这个循环。这是一种叫做数组分块（arraychunking)的技术，小块小块地处理数组，通 常每次一小块。基本的思路是为要处理的项目创建一个队列，然后使用定时器取出下一个要处理的项目 进行处理，接若再设畀另一个定时器。基本的模式如F。

22.3 高级定时器	613
setTimeout(function(){
//取出下一个条目并处理 var item = array.shift()； process(item)?
//若还有条目，再设置另一个定时器 if{array.length > 0){
setTimeout(arguments.callee, 100)?
}
).100);
在数组分块模式中，array变最本质上就是一个“待办事宜”列表，它包含了要处理的项0。使用 shift (1方法可以获取队列中下一个要处理的项0，然后将其传递给某个函数。如果在队列中还有其他 项目，则设置另一个定时器，并通过arguments.callee调用同一个匿名函数。要实现数组分块非常 简单，可以使用以下函数。



function chunk(array* process, context){
setTimeout(function(){
var item = array.shift()?
process.call(context, item);
if (array.length > 0){
setTimeout{argument s.cal1ee, 100)；
}
}f 100);
ArrayChunkxngExample. htm
chunk ()方法接受三个参数：要处理的项U的数组，用丁-处理项口的函数，以及可选的运行该函数 的环境。函数内部用了之前描述过的基本模式，通过call (>调用的process (>函数，这样可以设置一 个合适的执行环境（如果必须）。定时器的时间间隔设置为了 l〇〇ms,使得JavaScript进程有时间在处 理项目的事件之间转人空闲。你可以根据你的耑要更改这个间隔大小，不过100ms在大多数情况下效果 不错。可以按如下所示使用该函数：



var data = [12,123,1234,453,436,23,23,5,4123,45,346,5634,2234,345,342];
function printValue(item){
var div = document.getElementById("myDiv")；
div.innerHTML += item + "<br>";
}
chunk(data, printValue);
ArrayChunkingExampie.htm
这个例子使用printValue ()函数将data数组中的每个值输出到一f<div>元素。由于函数处在 全局作用域内，因此无需给chunk ()传递一个context对象。
必须当心的地方是，传递给chunM)的数组是用作…个队列的，因此当处理数据的同时，数组中的 条目也在改变。如果你想保持原数组不变，则应该将该数组的克隆传递给chunk(),如下例所示： chunk(data.concat(), printValue)；
22

614 第22章高级技巧
当不传递任何参数调用某个数组的concatO方法时，将返冋和原来数组中项0—样的数组。这样 你就可以保证原数组不会被该函数更改。
数组分块的重要性在于它可以将多个项R的处理在执行队列上分开，在每个项H处理之后，给予其 他的浏览器处理机会运行，这样就可能避免长时间运行脚本的错误。
(^\	一旦某个函数需要花50ms以上的时间完成，那么最好看看能否将任务分割为一
系列可以使用定时器的小任务。
22.3.3函数节流
浏览器中某些计界和处理要比其他的昂贵很多。例如，DOM操作比起非DOM交互需要更多的内 存和CPU时间。连续尝试进行过多的DOM相关操作可能会导致浏览器挂起，有时候其至会崩溃。尤其 在丨E中使用onresizc事件处理程序的时候容易发生，当调整浏览器大小的时候，该事件会连续触发。 在onresize亊件处理程序内部如果尝试进行DOM操作，其高频率的更改可能会让浏览器崩溃。为了 绕开这个问题，你吋以使用定时器对该函数进行节流。
函数节流ff后的基本思想是指，某呰代码不可以在没有间断的情况连续重复执行。第一次调用函数， 创建一个定时器，在指定的时间间隔之后运行代码。肖第二次调用该函数时，它会清除前一次的定时器 并设S另一个。如果前一个定时器已经执行过了，这个操作就没有任何意义。然而，如果前一个定时器 尚未执行，其实就是将其替换为一个新的定时器。H的是只也在执行函数的请求停止了一段时间之后才 执行。以下是该模式的基本形式：
var processor = {
timeoutId： null,
//实际进行处理的方法 performProccssing： function(){
//实际执行的代码
).
/7初始处理调用的方法 process： functionO {
clearTimeout(this.timeoutld);
var that = this;
this.timeoutld = setTimeout(function(){ that.performProcessing()?
}, l〇〇);
}
};
//尝试开始执行
processor.process();
在这段代码中，创建了 —t*叫做processor对象。这个对象还有2个方法：process ()和 performProcessingU。前者是初始化任何处理所必须调用的，后者则实际进行应完成的处理。当调 用了 process (>,第一步是清除存好的timeoutld,来阻止之前的调用被执行。然后，创建一个新的 定时器调用performProcessing (>。由于setTimeout ()中用到的函数的环境总是window,所以有 必要保存this的引用以方便以后使用。	z

22.3高级定时器	615
时间间隔设为丫 100ms,这表示最后一次调用process (1之后至少100ms后才会调用perform- Processing()。所以如果 100ms 之内调用了 process^)共 20 次，performanceProcessingO 仍只 会被调用一次。
这个模式可以使用throttle()函数来简化，这个函数可以自动进行定时器的设S和清除，如下例 所示：
function throctle(method, context) { clearTimeout(method.tld); method,tld= setTimeout{function <>{ method.call(context)；
100);
ThrotdingExample.htm
throttle (>函数接受两个参数：要执行的函数以及在哪个作用域中执行。上面这个函数首先清除 之前设置的任何定时器。定时器ID是存储在函数的tld属性巾的，第一次把方法传递给throttle () 的时候，这个属性可能并不存在。接下来，创建一个新的定时器，并将其id储存在方法的tldJS性中。 如果这是第一次对这个方法调用throttieO的话，那么这段代码会创建该属性。定时器代码使用 call ()来确保方法在适当的环境中执行。如果没有给出第二个参数，那么就在全局作用域内执行该方 法。
前面提到过，节流在resize事件中是最常用的。如果你基丁-该亊件来改变页面布局的话，最好控 制处理的频率，以确保浏览器不会在极短的时间内进行过多的计算。例如，假设有一个<div/>元素需 要保持它的髙度始终等同于宽度。那么实现这一功能的JavaScript可以如下编写：
window.onresize = function{){
var div = document.getElementById("myDiv")； div.style.height = div. offsetwidth + "px";
这段非常简单的例子有两个问题可能会造成浏览器运行缓慢。抟先，要计算offsetwidth属性， 如果该元素或者页面上其他元素有非常复杂的CSS样式，那么这个过程将会很复杂。其次，设某个元 素的高度需要对页面进行回流来令改动生效。如果页面有很多元素同时应用了相当数最的CSS的话，这 又需要很多计算。这就可以用到throttle (>函数，如下例所示：
22
function resizeDiv(}{
var div = document.getElementByldC'nvDiv*); div.style.height = div.offsetwidth + "px*;
window.onresize = function〇{
throttle(resi2eDiv)；.
);
ThrottlingExample.htm
这里，调整大小的功能被放人了一个叫做resizeDiv()的单独闲数中。然后onresize亊件处理 程序调用throttleO并传人resizeDiv闲数，而不是直接调用resizeDiv()。多数情况下，用户是 感觉不到变化的，虽然给浏览器节省的计算可能会非常大。

616 第22章高级技巧
只要代码是周期性执行的，都应该使用竹流，但是你不能控制请求执行的速率。这里展示的 throttle ()函数用f 100ms作为间隔，你，然可以根据你的需要来修改它。
22.4自定义事件
在本书前面，你已经学到事件是JavaScript与浏览器交互的主要途径。事件始一种叫做观察者的设 计模式，这是一种创建松散稱合代码的技术。对象可以发布事件，用来表示在该对象生命周期中某个有 趣的时刻到了。然后其他对象可以观察该对象，等待这些有趣的时刻到来并通过运行代码来响应。
观察者模式由两类对象组成：主体和观察者。主体负贞发布事件，N时观察者通过订阅这些事件来 观察该主体。该模式的一个关键概念是主体并不知道观察者的任何事情，也就是说它吋以独自存在并正 常运作即使观察者不存在。从另一方面來说，观察者知道主体并能注册事件的回调函数(亊件处理程序)。 涉及D0M .hB、f, D0M元索便是主体，你的事件处理代码便足观察者。
事件是与D0M交互的最常见的方式，但它们也可以用于非D0M代码中——通过实现S定义事件。 自定义事件背后的概念是创建一个管理事件的对象，ih其他对象监听那些事件。实现此功能的基本模式 可以如下定义：
function KventTargetO{ this.handlers = {};
EventTarget.prototype = {
constructor： EventTarget, addHandler: function(type, handler){
if (typeof this.handlersftyp^) -= "undefined'*) { this.handlers丨type]=(];
}
this.handlers I type].push(handler);
fire： function(event){ if {(event.target){
event.target = this；
)
if (this.handlersJevent.type] instanceof Array){ var handlers = this.handlersfevent.type]; for {var i=0, len=handlcrs.length； i < len; i++){ handlers[i](event)；
removeHandler： function{type, handler){
if {this.handlers[type] instanceof Array){ var handlers = this.handlers(type]； for (var i=0, ien=handlers.length; i < len; i+今){ if (handlers[i] === handler){ break ?
}
handlers- splice(i, 1);

22.4自定义事件 617

EventTarget.js
EventTarget类型有一个.中.独的属性handlers,用T储存事件处理程序。还有三个方法： addHandlerO，用f注册给定类型事件的亊件处理程丨t:; fire(),用于触发一个事件； removeHandlerU ,用于注销某个水件炎壤的事件处理程序。
addHemcUerU方法接受两个参数：事件类型和用丁处理该事件的闲数。当调用该方法时，会进行 一次检査，宥看handlers属性中是否已经存在一个针对该事件类型的数组；如果没有，则创建一个新 的。然后使用pushO将该处理程序添加到数组的*M。
如果要触发-个事件，要调用fire ()函数。该方法接受一个单独的参数，是一个至少包含type 属性的对象。fire ()方法先给event对象设S—个target厲性，如果它尚未被指定的话。然后它就 査找对应该亊件类壞的一组处理程序，调用各个函数，并给出event对象。因为这些都是G定义事件， 所以event对象h还需要的额外信息出你fl己决定。
removeHandler()方法是addHancHerO的辅助，它们接受的参数一样：事件的类型和事件处理 程序。这个方法搜索事件处理程序的数组找到要删除的处理程序的位罟。如果找到f，则使用break 操作符退出for循环。然后使用splice()方法将该项U从数组中删除。
然后，使用EventTarget类型的A定义事件可以如下使用：



function handleMessage(event;> {
alert("Message received: " + event.mossage)；
//创建一个新对象
var target = new EventTarget();
//添加一个事件处理程序
target.addHanaler("message"# handleMessage)；
//触发事件
target.fire({ type: "message", message： "Hello world!"});
//硎除事件处理程序
target.removeHandler("message", handleMesaage)?
//再次，应没有处理程序
target.fire({ type： "message”， message： "Hello world!"));
EventTargetExampleOl .him
在这段代码中，定义了 handleMessageU函数用于处理message事件。它接受event对象并输 出 message 属性3调用 target 对象的 addHandler ()方法并传给-message”以及 handleMessage () 函数。在接下来的一行h,调用丫 fire (>函数，并传递了包含2个厲性，BP type和message的对象 直接量。它会调用message艰件的事件处理程序，这样就会秘示一个赘告框(来自handleMessage ())。 然后删除了#件处理程序，这样即使事件再次触发，也不会显示任何替告框。
因为这种功能是封装在一种自定义类劫中的，其他对象可以继承EventTarget并获得这个行为，

618 第22章高级技巧
如下例所示:



function Person(name, age){
EventTarget.call(this);
this.name - nane；
this.age = age；
inheritPrototype(Person,EventTarget)；
Person.prototype.say = function{message){
this.fire({type： "message", message： message));
}?
EventTargetExampk02.htm
Person类型使用i"寄生组合继承（参见第6韋）方法来继承EventTarget。一曰.调用f say () 方法，便触发了亊件，它包含了消息的细节。在某种类®的另外的方法中调用fire ()方法是很常见的, 同时它通常不是公开调用的。这段代码可以照如下方式使用：
function handleMessage(event){
alert(event.target.name + _ says： * + event.message)；
//创建新person
var person = new Person("Nicholasn, 29);
//添加一个事件处理程序
person.addHandler("message", handleMessage)；
//在该对象上调用1个方法，它觖发消息事件 person.say(*Hi there.")；
EventTargetExample02.htm
这个例子中的handleMessage 〇函数显取了某人名字（通过event. target .name获得）的-个 警告框和消息正文。当调用Say()方法并传递一个消息时，就会触发message事件。接下来，它又会 调用handleMessage ()函数并献那督告框。
当代码中存在多个部为•在特定时刻相互交互的情况下，自定义事件就非常有用了。这时，如果每个 对象都有对其他所有对象的引用，那么整个代码就会紧密耦含，同时维护也变得很困难，因为对某个对 象的修改也会影响到其他对象。使用自定义事件有助于解耦相关对象，保持功能的隔绝。在很多情况中， 触发亊件的代码和监听亊件的代码是完全分离的。
22.5拖放
拖放是一种非常流行的用户界面模式。它的概念很简单：点击某个对象，并按住鼠标按钮不放，将 鼠标移动到另一个冈域，然后释放鼠标按钮将对象“放”在这里。拖放功能也流行到了 Web上，成为 了一些更传统的配置界面的一种候选方案。
拖放的基本概念很简单：创建一个绝对定位的元索，使其可以用鼠标移动。这个技术源自一种叫做

22.5 拖放 619
"鼠标拖尾”的经典网页技巧。親标拖尾是一个或者多个图片在页面上跟着M标指针移动。单元素鼠标 拖尾的基本代码需要为文杓设K一个onmousemove亊件处理程序，它总是将指定元素移动到鼠标指针 的位3,如下面的例子所示。
EventUtil.addHandler(document, "mousemove", function(event){ var inyDiv = document.getElementById myDiv.style.left = event.clientX + "px"； inyDiv. style, top = event .clientY + "px"；
})；
DragAndDropExampleOI.htm
在这个例子中，元素的left和top坐标设置为f event对象的clientX和clientY属性，这 就将元素方到了视口中指针的位置上。它的效果是一个元素始终跟随指针在页面上的移动。只要正确的 时刻（当鼠标按钮按下的时候）实现该功能，并在之后删除它（当释放鼠标按钮时），就可以实现拖放 了。最简单的拖放界面可用以下代码实现：
var DragDrop = function(>{
var dragging = null； function handleEvent(event){
//获取事件和目标
event = EvencUtil.getEvent(event)； var target = EventUtil.getTarget(event)；
//确定事件类型 switch (event.. type) { case ■mousedown*：
if (target.className.indexOf("draggable"> > -1){ dragging = target;
>
break?
case ■mousemove*：
if (dragging !== null){
//assign location
dragging.style.left = event.clientX + "px"; dragging.style.top = event.clientY + "px";
)
break；
case "mouseup":
dragging * null； break;
}
>;
//公共接口 return {
enable： function(){
EventUti1.addHandler(document, •mousedown"# handleEvent)? EventUtil.addHandler(document/ •mousemove", handleEvent)； EventUtil.addHandler(document, "mouseup", handleEvent)?

620 第22章高级技巧
disable： function{){
EventUtil.removeHandler(documenc, "mousedown", handleEvent)； EventUt il»renoveHcindler {document, "mousercove", handleKvent); EventUtil.removeHandler(document, "mouseup", handleEvent);
}();
DragAndDropExample02, him
DragDrop对象封装了拖放的所有基本功能。这是-个单例对象，并使用丫模块模式来隐藏某些实 现细节。dragging变货起初是null,将会存放被拖动的元岽，所以当该变量不为nuU时，就知道正 在拖动某个东西。iiamileEventO函数处理拖放功能中的所有的三个鼠标亊件。它酋先获取event对 象和亊件U标的引用。之后，用-个switch语句确定要触发哪个事件样式。当mousedown事件发生 时，会检査target的class是否包含”draggable"类，如果是，那么将target存放到dragging 中。这个技巧可以很方便地通过标记语言而非JavaScript脚本来确定可拖动的元素。
handleEvent ()的mousemove情况和前面的代码一样，不过要检査dragging是•为null。当 它不是null,就知道dragging就是要拖动的元素，这样就会把它放到恰3的位S上。mouseup情况 就仅仅是将dragging電置为null,让mousemove事件中的判断失效。
DragDrop还有两个公共方法：enable 〇和disable 〇 ,它们只是相应添加和删除所有的事件处 理程序。这两个函数提供了额外的对拖放功能的控制手段。
要使用DragDrop对象，只要在贞面上包含这些代码并调用enable ()。拖放会自动针对所有包含 "draggable"类的元素妇用，如下例所示：
«3iv class-"draggable° style-"position：absolute; background：red"> </div>
注意为了元素能被拖放，它必须垃绝对定位的。
22.5.1修缮拖动功能
当你试了上面的例子之后，你会发现元索的左h角总是和指针在一起。这个结果对用户来说有一点 不爽，W为3鼠标开始移动的时候，元素好像是突然跳丫一下。理想情况是，这个动作应该看上去好像 这个元索是被指针“拾起”的，也就是说当在拖动元素的时候，用户点击的那一点就是指针应该保持的 位置（见阁22-4)。



图224
要达到浓要的效果，必须做一些额外的计算。你宙要计算元衮左上角和指针位置之间的差值。这个 差值应该在nxMisedown亊件发生的时候确定，并且一直保持，.貞到mouseup事件发生。通过将event 的client:X和clients屈性与该元素的offsetLeft和of fsetTop厲性进行比较，就可以算出水平

22.5 拖放 621
方向和垂直方向上需要多少空间，见图22-5。
W 0» 在 Dr»e 細 Drop Example
etement offsetTop
mi
event
element offselLeft I
even! ciienlX	I
阁 22-5
为了保存x和y坐标上的差值，还需要几个变量。diffX和diffY这些变童箱要在omnouseroove 亊件处理程序中用到，来对元素进行适.当的定位，如下面的例子所示。
var DragDrop = functionO {
var dragging = null; diffZ = 0; diffY - 0;
function handleEvsnt(event){



//获取事件和目标
event = EventUtil.getEvent{event)? var target = EventUtil.getTarget(event);
//确定事件类贺 switch{event.type){ case "mousedown"：
if {target.className.indexOf("draggable") > -1){ dragging = target;
diffx * event.clientx - target.offsetLeft; dif£Y 囂 event.clientY - t丨rget.offsetTop;
)
break;
case "mousemove":
if {dragging !== null){
//指定位里
dr&flging.style.left = (event.clients - diffX) + "px"； dragging.atyle.top s (event.clientY - dlf£Y) + _px”；
)
break；
22
case "mouseup*：

622 第22章高级技巧
dragging = null； break;
}
};
//公共接口 return {
enable： function{){
EventUtil.addHandler(document, "mousedown", handleEvent)； KvcntUtil.addHandlcr(document, "mousemove", handleEvent)； EvencUcil.addHandler(document, "mouseup", handleEvent);
)0；
disable： function。{
KvcntUtil.removeHandler(document, "mousedown", handleEvent); EventUtil.removeHandler(document, "mousemove", handleEvent)； EventUtil.removeHandler(document, "mouseup", handleEvent);
}
DragA ndDropExample03. htm
diffX和diffY变tt是私有的，因为只有handleEvent ()函数®要用到它们。.当mousedown事 件发生时，通过clientX减去目标的offsetLeft, clientY减去口标的offsetTop，可以计算到这 两个变童的值。当触发了 mousemove事件后，就可以使用这些变量从指针坐标中减去，得到最终的坐 标。最后得到一个更加平滑的拖动体验，更加符合用户所期望的方式。
22.5.2添加自定义事件
拖放功能还不能真正应用起来，除非能知道什么时候拖动开始了。从这点上看，前面的代码没有提 供任何方法表示拖动开始、正在拖动或者已经结束。这时，可以使用自定义事件来指示这几个事件的发 生，让应用的其他部分与拖动功能进行交互。
由于DragDrop对象是-个使用了模块模式的单例，所以需要进行一些更改来使用EventTarget 类勸。首先，创建+ •个新的EventTarget对象，然后添加enableO和disable(>方法，最后返冋这 个对象。看以下内容。
var DragDrop = function(){
var dragdrop « new EventTarget(),
dragging = null, diffx = 0, diffY = 0;
function handleEvent(event){
//获取r件和对象
event = EventUtil.getEver.t (event) ? var target = EventUtil.getTarget{event);
//确定事件类逛

22.5 拖放 623
switch(event.type){ case "mousedown"：
if (target .classNar.e. indexOf ("draggable") > -1) { dragging = target;
d.iffX = event.clientx - target.offsetLeft; diffY = event.clientY - target.offsetTop;
dragdrop.fire({type："dragatarttarget： dragging,
x: event•clientx, y: event.clientY));
>
break;
case "mousemovo"：
if (dragging l== null){
//指定位置
dragging.style.left = (event.clientx - diffX) + "px"； dragging.style.top = (event.clientY - diffY) + "px"?
"触发自定义事件
dragdrop.fire({types"drag"# target: dragging,
x: event. clientX# y: event •clientY'});
>
break?
case "mouseup":
dragdrop.fire({type:"dragend", target: dragging,
x: event.clientx, y: event.clientY)); dragging ■ null;
break;
}
};
//公共接〇
dragdrop.enable » function<){
BventUti1•addHandler(document, •mousedown", handleBvent); BventUt 11 .addHandler(document* "xaouscunoveN, handl«Bvent); Bvent0til»addHandler(docmnentf "mouseup", handleBvent);
)/
dragdrop,disable « function〇{
EventUtil.removeHandler(document,
EventUtil.renoveHandler(document,
BventUtil.removeHandler(document,
)；
"mousedown1
•mouseinove1
"mouseup"#
,handleBvent)； ,haxidleBvent); handleBvent);
return dragdrop;
} O ;
DragAndDropExampie04.htm
这段代码定义了三个事件：dragstart:、drag和dragend。它们都将被拖动的元素设置为了 target, 并给出了 x和y属性来表示当前的位置。它们触发于dragdrop对象h,之后在返回对象前给对象增 加enable<>和disable()方法。这些模块模式中的细小更改令DragDrop对象支持T事件，如卜‘：

624 第22章高级技巧
DragDrop.addHandler(*dragstart", function<event >( var status = document. .getElementById("status'*); status.innerHTML = "Started dragging * t event.target.id?
});
DragDrop.addHand1er(■drag", function(event){
var status = document.getElementById("status*);
status.innerHTML += "<br/> Dragged " + event.target.id + " to (" + event.x
event ,y +">’•；
});
DragDrop.addHandler("dragend", function(event){
var status = document.getElementById(•status*);
status.innerHTKL += *<br/> Dropped " + event.target.id +" at ("+ event.x ","+ event.y + ")"；
});
DragA ndDropExample04. htm
这里，为DragDrop对象的毎个事件添加了事件处理程序。还使用了一个元素来实现被拖动的元素 当前的状态和位置。一只元素被放下了，就可以看到从它一开始被拖动之后经过的所有的中间步骤。
为DragDrop添加定义事件可以使这个对象更健壮，它将可以在网络应用中处理复杂的拖放 功能。
###  22.6 小结
JavaScript中的函数非常强大，因为它们是第一类对象。使用闭包和函数环境切换，还可以有很多 使用函数的强大方法。可以创建作用域安全的构造函数，确保在缺少new操作符时调用构造函数不会改 变错误的环境对象。
- [ ] 可以使用惰性载人函数，将任何代码分支推迟到第一次调用函数的时候。
- [ ] 函数绑定可以让你创建始终在指定环境中运行的函数，同时函数柯里化可以让你创建已经填了 某些参数的闲数。
- [ ] 将绑定和柯里化组合起来，就能够给你一种在任意环境中以任意参数执行任意函数的方法。 ECMAScript 5允许通过以下几种方式来创建防篡改对象。
- [ ] 不可扩展的对象，不允许给对象添加新的属性或方法。
- [ ] 密封的对象，也是不可扩展的对象，不允许删除已有的属性和方法。
- [ ] 冻结的对象，也是密封的对象，不允许重写对象的成员。
JavaScript中可以使用seCTimeouL ()和setlnterval (>如下创建定时器。
- [ ] 定时器代码是放在一个等待区域，直到时间间隔到了之后，此时将代码添加到JavaScript的处理 队列中，等待下一次JavaScript进程空闲时被执行。
- [ ] 每次一段代码执行结束之后，都会有一小段空闲时间进行其他浏览器处理。
- [ ] 这种行为意味着，可以使用定时器将长时间运行的脚本切分为一小块一小块可以在以后运行的 代码段。这种做法有助于Web应用对用户交互有更积极的响应。
JavaScript中经常以事件的形式应用观察者模式。虽然事件常常和DOM —起使用，但是你也可以通过实现自定义事件在S己的代码中应用。使用自定义事件有助于将不同部分的代码相互之间解耦，让维 护更加容易，并减少引人错误的机会。
拖放对于桌面和Web应用都是一个非常流行的用户界面范例，它能够让用户非常方便地以一种直 观的方式重新排列或者配置东西。在JavaScrip中可以使用鼠标事件和一些简中-的计算来实现这种功能 类型。将拖放行为和自定义亊件结合起来可以创建一个可重复使用的框架，它能应用于各种不N的情 况下。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter21.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter23.md)