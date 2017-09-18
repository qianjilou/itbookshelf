ECMAScript Harmony([返回首页](https://github.com/qianjilou/javascript3))
# 2004年Web开发重新焕发生机的大背景下，浏览器开发商和其他相关组织之间进行了一系列 H会谈，讨论应该如何改进JavaScript。ECMA-262第四版的制定工作就建立在两大相互竞争的 提案基础上：一个是Netscape的JavaScript 2.0,另一个是Microsoft的JScript.NET。各方抛开在浏览器 领域的竞争，聚集在ECMA麾下，提出了希望能以JavaScript为蓝本设计出一门新语言的建议方案。最 初的工作草案叫做ECMAScript4,而且很长时间以来，它好像就是JavaScript的下一个版本。后来，一 个叫ECMAScript3.1反提案的加人，令JavaScript的未来再次充满了疑问。在反复争论之后,ECMAScript
3.1成为了 JavaScript的下一个版本，rftf且未来的工作成果  代号Harmony (和请），将力争让
ECMAScript 4 向 ECMAScript 3.1 靠拢。
ECMAScript 3.1最终改名为ECMAScript 5,很快就完成了标准化。ECMAScript 5的详细内容本书 已经介绍过丫。ECMAScript5的标准化工作一完成，Harmony立即被提上日程。Harmony与ECMAScript 5的指导思想比较一致，就是只进行增量调整，不彻底改造语言。虽然到2011年的时候，Harmony,也 就是未来的ECMAScript 6,还没有全部制定完成，但其中的几个部分已经尘埃落定。本附录所要介绍 的就是那些将来肯定能进人最终规范的部分。不过也提醒一下大家，在将来的实现中，这些内容的细节 有可能与你在这里看到的不一样。
1 —般性变化
Haimcmy为ECMAScript弓|人了一些基本的变化。对这门语言来说，这些里然不算是大的变化，但 的确也弥补了它功能上的一些缺憾。
1.1常量
没有正式的常M是JavaScript的一个明显缺陷。为了弥补这个缺陷，标准制定者为Harniony增加了 用const关键字卢明常量的语法。使用方式与var类似，但const声明的变ft在初始赋值后，就不能 再重新赋值了。来看一个例子。
const MAX_SIZE = 25;
可以像声明变fi—样在任何地方声明常锨。但在同一作用域中，常*名不能与其他变量或函数名重 名，因此下列声明会导致错误：
const FLAG = true;
var FLAG = false; //械误！

702 附录 A ECMAScript Hannony
除了值不能修改之外，可以像使用任何变最一样使用常ft。修改常最的值，不会有任何效果，如下 所示：
const FLAG = true?
FLAG = false； alert (FLAG) ； //正确
支持常量的浏览器有 Firefox、Safari 3+、Opera 9+和 Chrome。在 Safari 和 Opera 中，const 与 var 的作用一样，因为前者定义的常量的值是可以修改的。
1.2块级作用域及其他作用域
本书时不时就会提醒读者一句：JavaScript没有块级作用域。换句话说，在语句块中定义的变量与 在包含函数中定义的变量共享相同的作用域。Hannony新增了定义块级作用域的语法:使用let关键字。
与const和var类似，可以使用let在任何地方定义变量并为变量賦值。区别在于，使用let定 义的变M在定义它的代码块之外没有定义。比如说吧，下面是一个非常常见的代码块：
for (var i=0； i < 10； i++) {
//执行茱些操作
>
alert(i)； //10
在h面的代码块中，变M i是作为代码块所在函数的《部变童来声明的。也就是说，在for循环 执行完毕后，仍然能够读取i的值。如果在这里使用let代替var,则循环之后，变量i将不复存在。 看下面的例子。
for (let i翼0; i < 10; i++)  {
//执行某些操作
}
alert(i>, //#*!变量i没有定义
以上代码执行到最后-行的时候，就会出现错误，因为for循环一结束，变量i就已经没有定义 了。因为不能对没有定义的变量执行操作，所以发生错误是&然的。
还有另外一种使用let的方式，即创建let语句，在其中定义只能在后续代码块中使用的变量， 像下面的例了•这样： var num = 5;
let (num=10, mul匕iplier=2>{
alert (nuirs * multiplier) ； //20
}
alert(num)； //5
以上代码通过let语句定义了一个区域，这个区域中的变量num等丁- 10, multiplier等于2。 此时的num覆盖了前面用var声明的同名变ft,因此在let语句块中，num乘以multiplier等于20。 而出了let语句块之后，num变量的值仍然是5。这是因为let语句创建了自己的作用域，这个作用域 里的变量与外面的变关。

附录 A ECMAScript Harmony 703
使用丨4洋的语法还可以创建let表达式，冗中的变量只在表达式中有定义。再看一个例子。
var result = let(num=10# multiplier=2) num * multiplier? alert(result); //20
这M的let表达式使用两个变贵计算后得到一个值，保存在变量result中。执行表达式之后，num 和multiplier变最就不存在了。
在JavaScript中使用块级作用域，可以更精细地控制代码执行过程中变ft的存废。
2函数
大多数代码都是以函数方式编写的，因此Harmony从几个方面改进广闲数，使其更便于使用。与 Harmony中其他部分类似，对函数的改进也集中在开发人员和实现人员共同面临的难题上。
A.2.1剩余参数与分布参数
Harmony中不再有arguments对象，因此也就无法通过它来读取到未声明的参数。不过，使用剩 余参数（restarguments)语法，也能表示你期待给函数传人可变数量的参数。剩余参数的语法形式是三 个点后跟一个标识符。使用这种语法可以定义可能会传进来的更多参数，然后把它们收集到一个数组中。 来肴一个例子。
function sum(numl, num2# ...nuras}{
var result = numl + num2;
for (let i=0, len=nums.length； i < len； i++){ result += nums[i];
}
return result；
}
var result = sum(l, 2, 3, 4, 5, 6)；
以上代码定义了一个sumO函数，接收至少两个参数。这个函数还能接收更多参数，而其余参数都 将保存在nums数组中。与原来的arguments对象不同，剩余参数都保存在Array的一个实例中，因 此可以使用任何数组方法来操作它们。另外，即使并没有多余的参数传入函数，剩余参数对象也是Array 的实例。
与剩余参数紧密相关的另一种参数语法是分布参数（spread arguments)。通过分布参数，可以向函 数中传人一个数组，然后数组中的元素会映射到闲数的每个参数上。分布参数的语法形式与剩余参数的 语法相同，就是在值的前面加三个点。唯一的区別是分布参数在调用函数的时候使用，而剩余参数在定 义函数的时候使用。比如，我们可以不给sum()函数---个一个地传人参数，而是传人分布参数： var result = sum{.,.[1, 2, 3, 4, 5, 6]};
在这里，我们将-•个数组作为分布参数传给了 sumU函数。以上代码在功能h与下面这行代码等价：
var result = sum.apply(this, [1, 2, 3, 4, 5, 6));
A.2.2默认参数值
ECMAScript函数中的所有参数都是可选的，因为实现不会检査传人的参数数童。不过，除了手工

704 附录 A ECMAScript Harmony
检丧传人了哪个参数之外，你还可以为参数指定默认值。如果调用函数时没有传人该参数，那么该参数 就会使用默认值。
要为参数指定默认值，可以在参数名后面盘接加上等于号和默认值，就像下面这样：
funccion sum(numl, num2*〇>{
return numl + num2；
>
var resultl = sum(5); var result2 = sum(5, S);
这个sum(>函数接收两个参数，佴第二个参数娃可选的，因为它的默认值为0。使用可选参数的好 处是开发人员不用冉去检査是否给某个参数传人了值，如果没有的话就使用某个特定的值。默认参数值 帮你解除了这个困扰。
A.2.3生成器
所谓生成器，其实就是一个对象，它每次能生成一系列值中的一个。对Harmony而言，要创建生成 器，可以让闲数通过yield操作符返回某个特殊的值。对于使用yield操作符返回值的函数，调用它 时就会创建并返回一个新的Generator实例。然后，在这个实例上调用nexU)方法就能取得生成器 的第一个值。此时，执行的足原来的函数，似执行流到yield语句就会停止，只返回特定的值。从这 个角度看，yield与return很相似。如果再次调用next<)方法，原来闲数中位于yield语句后的代 码会继续执行，直到洱次遇见yield语句时停止执行，此时再返回一个新值。来看下面的例子。
function ir^Numbers () {
for (var i=0； i < 10? i++){ yield i * 2；
var generator = nyNumbers{)；
try {
while(true){
document.write(generator.next() + M<br />")；
}
} catch(ex){
//有意没有写代码
} finally {
generator,close();
}
调用myNumbers <)函数后，会得到一个生成器。myNumbers ()函数本身非常简单，包含一个每次 循环都产生一个值的for循环。每次调用next (>方法都会执行一次for循环，然后返回下一个值。第 一个值是0,第二个值是2,第三个值是4,依此类推。在myNumbers (>函数完成退出而没有执行yield 语句时（最后一•次循环判断i不小于10的时候），生成器会抛出Stopiteration错误。因此，为了输 出生成器能产生的所有数值，这里用.个try-catch结构包装了--个while循环，以避免出错时中断 代码执行。
如果不再®要某个生成器，最好是调用它的close (>方法。这样会执行原始函数的其他部分，包括 try-catch相关的finally语句块。

附录 A ECMAScript Harmony 705
在需要一系列值，而每一个值又与前…个值存在某种关系的情况下，可以使用生成器。
3数组及其他结构
Harmony的另一个電点是数组。数组是JavaScript使用域频繁的一种数据结构，因此定义-些更直 观更方便地使用数组的方式，绝对是改进这门语1时最优先考虑的事。
3.1迭代器
迭代器也是一个对象，它能迭代一系列值并每次返回其屮一个值。想象一下使用for或for-iii循 环，这时候就是在迭代一批值，时且每次操作其中的一个值。迭代器的作用相同，只不过用不着使用循 环了。Harmony为各种类型的对象都定义了迭代器。
要为对象创建迭代器，可以调用Iterator构造函数，传人想要迭代其值的对象。要取得对象中的 T一个值，可以调用迭代器的next 〇方法。默认情况K,这个方法会返回一个数组。如果迭代的是数 组，那么返回数组的第一个元素是值的索引，如果迭代的是对象，那么返回数组的第一个元素是值的属 性名；返冋数组的第二个元素是值本身。如果所有值都己经迭代了一遍，则再调用next(i会抛出 Stoplteration错误〇卷下面这个例子〇
var person = {
name: "Nicholas**, age； 29
};
var iterator = new Iterator(person); try {
while(true){
let value = iterator.next()；
document.write(value.join(":") + *<br>");
}
} catch(ex){
//有意没有写代码
}
以t代码为person对象创建了一个迭代器。第一次调用next ()方法，返回数组["name”， •Nicholas"],第二次调用返冋数组["age", 29]。以上代码的输人结果为：
name；N icholas age：29
如果为非数组对象创建迭代器，则迭代器会按照与使用for-in循环一样的顺序，返回对象的每个 屈性。这就意味着迭代器也只能返回对象的实例屈性，而且返回属性的顺序也会因实现而异。为数组创 建的迭代器也类似，即按数组元素顺序依次返回值，下面是一个例了-。
var colors ■ ["red", •'green", "blue"]; var iterator * new Iterator(colors);
try {
while(true){
let value = iterator.next(i； doc\ament.write(value. joinC* ： *) + "<br>");

706 附录 A ECMAScript Harmony
} catch{ex)(
以上代码的输出结果如F:
0 ：red 1：green 2：blue
如果你只想让next ()方法返回对象的属性名或者数组的索引值，吋以在创建迭代器时为Iterator 构造函数传人第二个参数true,如下所示：
var iterator •=： new Iterator (colors, true)；
在这样创的迭代器上每次调用)方法，只会返回数组中每个值的索引，而不会返回包含索 引和值数组。
如果想为自定义类型创建迭代器，需要定义一个特殊的方法_iterator_( >, 这个方法应该返回一个包含next O方法的对象。当把自定义类里传給Iterator构 造函数时，就会调用那个特殊的方法。
A.3.2数组领悟
所谓数组领悟（array comprehensions),指的是用一组符合某个条件的值来初始化数组〇 Harmony 定义的这项功能借鉴了 Python屮流行的一个语言结构。JavaScript中数组领悟的基本形式如下： array ~ [ value for each (variaixle in values} condition ]；
其中，value是实际会包含在数组中的值，它源A values数组。for each结构会循环values 中的每一个值，并将每个值保存在变量variable中。如果保存在variable中的值符合condition 条件，就会将这个值添加到结果数组中。下面是一个例子〇
//原始數组
var numbers = [0,1,2,3,4,5/6,7,8/9,10]；
//把所有元素复制到新数组
var duplicate = (i for each (i in numbers)J;
//只把供数复制到新数组
var evens = [i for each (i in numbers) if (i % 2 == 0)];
//把每个数乘以2后的结果放到新数纽中
var doubled = [i*2 for each (i in numbers}];
//把每个奇数乘以3后的结果放到新数组中
var tripledOdds = [i*3 for each (i in numbers) if (i % 2 > 0)];
在以上代码的数组领悟部分，我们使用变fli迭代r numbers中的所有值，而其巾.些语句给出了 条件，以筛选最终包含在数组中的结采。本质上讲，只要条件求值为true,该值就会添加到数组中。与 自己编写同样功能的for循环相比，数组领悟的语法稍有不同，何却更加简洁。Firefox 2+是唯一支持数

附录 A HCMAScript Harmony 707
组领悟的浏览器，而且要使用这个功能，必须将<3〇：1：1口1：>的type属性值指定为”application/
javascript;version=l.7M c



数组领悟语法的values也可以是一个生成器或者一个迭代器。
A.3.3解构賦值
从一组值中挑出一或多个值，然后把它们分别賦给独立的变量，这也是一个很常见的需求。就拿迭 代器的next ()方法返冋的数组来说，假设这个数组包含着对象中一个属性的名称和值。为了把这个属 性和值分别保存在各自的变ft中，需要写两个语句，如K所示。
var nextValue = ["color", "red"]; var name = nextValue[0J; var value = nextValue[11j
而使用解构賦值（destructuringassignmems)语法，用一条语句即可解决问题：
var (name, value] = ["color", "red_】； alert{name);  //"color*
alert (value) ； /"red"
在传统的JavaScript中，数组字面M是不能出现在等于号（賦值操作符）左边的。解构賦值的这种 语法表示的是把等于号右边数组中包含的值，分别賦给等于号左边数组中的变M。结果就是变量name 的值为"color"，变世value的值为"red"。
如果你不想取得数组中所有的值，可以只在数组字面摄中给出对应的变量，比如：
var [, value) = ("color", "red"J； alert(value); //"red”
这样就只会给变ft value賦值，值为•red"。
有了解构賦值，还可做点有创意的事儿，比如交换变量的值。在ECMAScript 3中，要交换两个变 搋的值，一般是要这样写代码的：
var valuel = 5; var value2 = 10；
var temp = valuel； valuel = value2； value2 = temp?
利用解构后的数组赋值，可以柯掉那个临时变贵temp,比如:
var valuel = 5; var value2 =10?
【value2, valuel] = [valuel* value2];
解构賦值同样适用于对象，看下面这个例子：
var person - {
name： "Nicholas*,
age； 29
};

708 附录 A ECMAScript Harmony
var { name： personName, age: personAge ) = person?
alert(personName); //"Nicholas" alert(personAge); //29
与使用数组字面K一样，看到等于号左边出现了对象字面M,那就是解构賦值表达式。这条语句实 际上定义了两个变贵，personName和personAge,它们分别得到了 person对象中对应的值。与数 组解构賦值一样，在对象解构賦值中也可以选择要取得的值，比如：
var { age: personAge } s person； alert(personAge); //29
以上代码只取得了 person对象中age属性的值，将它赋给丫变M personAge。
A.4新对象类型
Harmony为JavaScript定义了几个新的对象类型。这几个新类型提供/以前只有JavaScript弓|擎才能 使用的功能。
A.4.1代理对象
Harmony为JavaScript引人了代理的概念。所谓代理（proxy),就是一个表示接口的对象，对它的 操作不一定作用在代理对象本身。举个例子，设置代理对象的一个属性，实际上可能会在另一个对象上 调用一个函数。代理是一种非常有用的抽象机制，能够通过API只公开部分信息，同时还能对数据源进 行全面控制。
要创建代理对象，可以使用Proxy .create ()方法，传入一个handler (处理程序）对象和一个 uf选的protoliype (原型）对象：
var proxy = Proxy.create(handler)；
//创建一个以myObject为原型的代理对象
var proxy = Proxy.create(handler, myObject);
其中，handler Xt象包含用于定义捕捉器（trap)的属性。捕捉器本身是函数，用于处理（捕捉） 原生功能，以便该功能能够以另一种方式来处理。要确保代理对象能够按照预期工作，至少要实现以下 7种基本的捕捉器。
口 getOwnPropertyDescriptor:当在代理对象上调用Object .getOwnPropertyDescriptor <) 时调用的函数。这个函数以接收到的屑性名作为参数，返回属性描述符，或者在属性不存在时返 M mill。
getPropert:yDescriptor:.当在代理对象上调用 Object.getPropertyDescriptorM)时调 用的函数。（这是Harmony中的新方法。）这个函数以接收到的厲性名作为参数，返回属性描述 符，或者在属性不存在时返W null。
getOwnPropertiyNames:翌在代理对象上调用 Object.getOwnPropertyNames ()时调用的 闲数。这个函数以接收到的属性名作为参数，应该返回一个字符串数组。
getPropertyNames:肖在代理对象上调用Object.getPropertyNames ()时调用的函数。 (这是Harmony中的新方法。）这个函数以接收到的M性名作为参数，应该返回一个字符串数组。

附录 A ECMAScript Harmony 709
defineProperty:,当在代理对象上调用Object.definePropertyU时调用的函数。这个困 数以接收到的属性名和属性描述符作为参数。
delete:定义在对象属性上使用delete操作符时调用的函数。属性名以参数形式传进来，如 果删除成功则返回true,删除失败返回false。
fix:当调用 Object.freeze()、Object.seal(> 或 Object.preventExtensions()时调 用的函数。当在代理对象上调用这几个方法时，返回undefined以抛出错误。
除了这7个基本的捕捉器，还有6个派生的捕捉器（derivedtrap)。与基本捕捉器不同，少定义一个 或几个派生捕捉器不会导致错误。每个派生的捕捉器都会覆盖一种默认的JavaScript行为。
口 has在对象上使用in操作符（例如"name” in object)时调用的闲数。以接收到的属性名作 为参数，返回true表示对象包含该属性，否则返回false。
hasown:在代理对象h调用hasOwnPropertyO方法时调用的函数。以接收到的属性名作为参 数，返回true表示对象包含该属性，否则返回false。
get:在读取属性时调用的函数。这个函数接收两个参数，即包含被读属性的对象的引用及属性 名。这个对象引用可能是代理对象本身，也可能是继承了代理对象的对象。
set:在写人属性时调用的函数。这个函数接收三个参数，即包含被写属性的对象的引用、属性名 和属性值。与get类似，这个对象引用可能是代理对象本身，也可能是继承了代理对象的对象。
enumerate:当代理对象被放在for-in循环中时调用的函数。这个函数必须返回一个字符串 数组，其中包含在for-in循环中使用的相应属性名。
keys:当在代理对象上调用Object; .keys <)时调用的函数。与enumerate类似，这个闲数也 必须返回一个字符串数组。
在需要公开API,而同时乂要避免使用者直接操作底层数据的时候，可以使用代理。例如，假设你 想实现一个传统的栈数据类型。虽然数组可以作为找来使用，但你想保证人们只使用push<)、pop〇 和length。在这种情况下，就可以基于数组创建一个代理对象，只对外公开这三个对象成员。
*实验ES 6代理对象•这个实验在数纽的基鈾上创建一个栈数搞结构•
*代理在此用于从接〇过i«*push*、-pop■和-length•之外的成员，让数奴成为一个炖粹的栈， *任何人不能i接操作其内容，
*/
var Stack = {function(){
var stack = []#
allowed = [wpush-, "pop", "length* ]；
return Proxy.create{{
get: function(receiver, name){;
if (allowed.indexOf(name) > -1){
if(typeof stack[name] == ■function"){
return stack[name].bind(stack)；
} else {
return stack(name];
}
} else {
return undefined；
>
}

710 附录 A ECMAScript Harmony
})；
var irystack = new StackO;
mystack.push("hi"); mystack.push("goodbye");
console•log(mystack.length);  //I
console.log(mystack[0】）；  //未定义
console.log(mystack.pop〇);  //"goodbye"
以上代码创建了一个构造函数Stack。但它没有使用this,而是返回了一个对数组操作进行包装 的代理对象。这个代理对象只定义了一个get捕捉器，该函数检测了一组允许的属性，然后才返回相应 的值。如果弓丨用的是不被允许的属性，那么捕捉器就返回undefined;如果引用的是push 〇、pop <) 和length,则一切正常。这里的关键是get捕捉器，它根据允许的成员过滤了对象的成员。如果该成 员是函数，就返回一个与底层数组对象绑定的函数，这样操作针对的就是数组而非代理对象。
A.4.2代理函数
除了创建代理对象之外，Harmony还支持创建代理函数（proxy function)。代理函数与代理对象的 区别是它可以执行。要创建代理困数，可以调用Proxy.createFunction(>方法，传人一个handler (处理程序^对象、-个调用捕捉器函数和一个可选的构造函数捕捉器函数。例如： var proxy = Proxy.createFunction(handler, function!){}, function(){));
与代理对象一样，handler对象也有同样多的捕捉器。调用捕捉器函数是在代理函数执行（如 proxy (>)时运行的代码。构造函数捕捉器是在用new操作符调用代理函数（如new proxy ()) B寸运 行的代码。如果没有指定构造函数捕捉器，则使用调用捕捉器作为构造函数。
A.4.3映射与集合
Map类型，也称为简单映射，只有一个目的：保存一组键值对儿。开发人员通常都使用普通对象来 保存键值对儿，但问题是那样做会导致键容易与原生属性混淆。简单映射能做到键和值与对象属性分离, 从而保证对象属性的安全存储。以下是使用简单映射的几个例子。 var map = new Map C);
map.set (•nafne" , "Nicholas");
map.set("book", "Professional JavaScript");
console.log(map.has('name"))； //true console.log(map.get("name"))； //"Nicholas*
map.delete{"name")；
简单映射的基本AP丨包括getO、set(>和delete。，每个方法的作用看名字就知道了。键可以 是原始值，也可是引用值。
与简单映射相关的是Set类型。集合就是组不重复的元素。与简单映射不间的是，集合中只有键，

附录 A ECMAScript Harmony 711
没有与键关联的值。在集合中，添加元素要使用add(>方法，检査元素是否存在要使用has 〇方法，而
删除元素要使用delete (>方法。以下是基本的使用示例。
var sec = new Set{)? set,add(_name;
console.log{set.has{"name")); //true set.delete(*name");
console.log(set.has{"name")); //false
截止到2011年10月，规范中关于Map和set的内容还没有最后定稿。因此，在JavaScript引擎实 现该规范时，有些细节可能会发生变化。
A.4.4 weakHap
WeakMap是ECMAScript中唯一一个能让你知道什么时候对象已经完全解除引用的类型。WeakMap 与简单映射很相似，也是用来保存键值对儿的。它们的主要K别在于，WeakMap的键必须是对象，而 在对象已经不存在时，相关的键值对儿就会从WeakMap中被刪除。例如：
var key * {},
map = new WeakMap{)；
map.set<key, "Hello!■);
//解除对键的幻用，从而*1除该值 key = null;
至于什么情况下适合使用WeakMap,目前还不清楚。不过，Java中倒是有一个相同的数据结构叫 WeakHashMap;于是，JavaScript乂多了一种数据类型。
A.4.5 StructType
JavaScript —个最大的不足是使用一种数据类型表示所有数值。WebGL为解决这个问题引人了类型 化数组，而ECMAScript 6则引入了类型化结构，为这门语言带来了更多的数值数据类型。结构类塑 (StructType )与C语言中的结构类似；在C语rf中，可以把多个属性组合成一条记录。对于JavaScript 的结构类型，通过指定属性及其保存的数据类型，也可以创建类似的数据结构。早期的实现定义了以下 几种块类型。
uintS:无符号8位整数。
口 int8:有符号8位整数。
uintl6:无符号16位整数。
intl6:有符号16位整数。
uint32:无符号32位整数。
int 32:有符号32位整数。
float:32: 32 位浮点数。
float64 : 64位浮点数。
这些块类型都只能包含一个值。将来还有银在这8种类型基础上进一步扩展。
要创建结构类型的对象，可以使用new操作符调用StructType,传人对象字面量形式的属性定义。

712 附录 A ECMAScript Hannony
var Size s new StructType({ width: uint32, height： uint32 >)?
以上代码创建了一个名为Size的新结构类型，该类型带有两个属性：width和height。这两个 属性都应该保存无符号32位整数。而变Size实际上是一个构造函数，可以像使用对象的构造函数 一样使用它。要实例化这个结构类型，需要向构造函数中传人一个带属性值的对象字面量。
var boxSize = new Size({ width： 80, height: 60 }); console.log(boxSize.width);  //80
console, log (boxSize.height) ； "60
这样，就创建了 Size的一个宽为肋、高为60的实例。实例的属性可以被读写，但始终都必须包 含32位无符号整数。
将厲性定义为另一个结构类型，可以得到更复杂的结构类型。例如：
var Location = new StructType({ x： int32, y： int32 });
var Box = new StructType({ size： Size, location： Location })；
var boxlnfo = new Box{{ size: { width:80, height:60 ), location； { x： 0t y： 0 }))； console. log{boxInfo.si2e.width) ； "80
这个例子创建了一个简申-的结构类型Location, 乂创建了一个复杂的结构类型Box。Box的属性 本身也是结构类型。Box构造函数仍然接收对象字面量参数.以便为每个属性定义值，但它会检査传人 值的数据类型，以确保作为屑性值的数据类型正确。
A.4.6 ArrayType
与结构类型密切相关的是数组类型。通过数组类型（ArrayType)可以创建一个数组，并限制数组 的值必须是某种特定的类型（与WebGL中的类型化数组很相似）。要创建新的数组类型，可以调用 Arrayiype构造函数，并传人它应该保存的数据类型以及应该保存的元素数目。例如： var SizeArray = new ArrayType(Size, 2);
var boxes = new BoxArray([ { width： 80, height： 60 ), { width： b0, height： 50 }]);
以上代码创建了一个名为SizeArray的数组类型，这个数组类哦只能保存Size的实例，同时也 给数组分配了两个该实例的位置。要实例化数组类型，可以传人一个数组，其中包含应该转换的数据。 数据可以是字面量，只要该字面量能提升为正确的数据类型即可（比如在这个例子中，传人的字面量可 以提升为结构类型)。
A.5类
开发人员一直吵着要在JavaScript中增加一种语法，用于定义类似于Java的类。ECMAScript6最终 确实定义了这种语法。促JavaScript中的类只是一种语法糖，覆盖在目前基于构造函数和基于原型的方 式和类型之上。先看看下面的类型定义。
function Person{name, age){ this.name = name； this.age = age;
>
Person.prototype.sayName = function(){ alert(this.name)；

附录 A ECMAScript Harmony 713
)；
Person.prototype.getOlder s function(years){ this.age += years；
再宥看使用新语法定义的类：
class Person {
constructor(name/ age){ public name = name； public age = age；
sayName(){
alert(this.name)；
getOlder(years){
this.age += years；
新语法以关键字class开头，然后就是类型名，而花括号中定义的是属性和方法。定义方法不必 再使用function关键宇，有方法名和圆括号就可以。如果把方法命名为constructor,那它就是这 个类的构造函数（与前一个例子中的Person函数一样)。在这个类中定义的方法和属性都会添加到原 型上，具体来说，sayName<)和getOlderU都是在Person .prototype上定义的。
在构造闲数中，public和private关键字用于创建对象的实例厲性。这个例子中的name和age 都是公有属性。
A.5.1私有成员
关于类语法的建议是默认支持私有成员的，包括实例中的私有成员和原型中的私有成员。private
关键字表示成员是私有的，不能在类方法之外访问。要访问私有成员，可以使用一种特殊的语法，即调
用private (>函数并传人this对象，然后再访问私有成员。例如，下面这个例子把Person类的age
改成为私有属性：
class Person {
constructor(name, age){ public name = name; priv&te age = age；
sayName(){
alert(this.name)；
}
getOlder(years){
private(this>.&9e += years；

714 附录 A ECMAScript Harmony
这种用于访问私有成员的语法还没冇定论，将来很可能会改变。 A.5.2 getter 和 setter
新的类语法支持商接为属性定义getter和setter，从而避免/调用Object .defineProperty () 的麻烦。为屈性定义getter和setter与定义方法类似，只不过要在方法名前加上get和set关键 字。例如：
class Person {
constructor(name# age){ public name = name； public age = age; private innerTitle =""；
get title(){
return InnerTitle；
)
set title(value){
innerTitle = value/
>
>
sayName(){
alert(this.name)；
getOlder(years){
this.age += years；
这个Person类为CiCle属性定义了一个getter和一"t' setter。这两个操作innerTitle变设 的函数都定义在了构造函数中。耍为原型属性定义getter和setter,语法相同，但要在构造函数外 部定义。
A.5.3继承
使用类语法而不是过去那种JavaScript语法，最大的好处是容易实现继承。有了类语法，只要使用 与其他语言相同的extends关键字就能实现继承，而不必去考虑借用构造函数或者原型连缀。例如：
class Employee extends Person {
constructor(name, age){
super(name,age)；
以丨:代码创建了一个新类Employee,它继承了 Person类。在简单的语法背后，已经A动实现了 原型连缀，而ii通过使用super ()函数，也正式支持了借用构造函数。从逻辑上看，面的代码与下面 的代码是等价的：
function Employee(name, age){
Person.call(this, name, ago)；

附录 A ECMAScript Harmony 715
Employee.prototype = new Person()?
除了这种风格的继承，类语法还允许直接将对象指定为其原型，方法就是用prototype关键字代 替 extends:
var basePerson = {
sayName: function(){ alert(this.name)；
getOlder： function(years){ this.age += years；
)
}；
class Employee prototype basePerson { constructor(name, age){ public name = name； public age = age；
这个例子将basePerson对象直接指定为Employee.prototype,从而实现了与目前使用 Obj ect. create <)实现的一样的继承。
A.6模块
模块（或者“命名空间”、“包”）是组织JavaScript应用代码的重要方法。每个模块都包含着独立于 其他模式的特定、独一无二的功能。JavaScript开发中曾出现过一些临时性的模块格式，而ECMAScript6 则对如何创建和管理模块给出了标准的定义。
模块在其自己的顶级执行环境中运行，因而不会污染导人它的全局执行环境。默认情况下，模块中 声明的所有变最、函数、类等都是该模块私有的。对于应该向外部公开的成员，可以在前面加上export 关键字。例如：
module MyModule {
//公开这痊成员
export let myobject = {)?
export function hello{){ alert("hello")； }；
//隐藏这痊成员 function goodbye(){ II…
这个模块公开了一个名为myobject的对象和一个名为hello <)的函数。可以在页面或其他模块 中使用这个模块，也可以只导人模块中的一个成员或者两个成员。导人模块要使用in^ort命令：
//只导入 myobject
import myobject from MyModule;
console.log(myobject);
//导入所有公开的成员 import * from MyModule?

716 附录 A ECMAScript Harmony
console.log{myobject); console.log(hello);
//列出要导入的成员名
import {myobject, hello) from MyModule； console.log(myobject); console.log(hello)；
//不导入，直接使用模块
console.log(MyModule.myobject);
console.log(MyModule.hello);
在执行环境能够访问到模块的情况下，可以直接调用模块中对外公开的成员。导人操作只不过是把 模块中的个别成员拿到当前执行环境中，以便直接操作而不必引用模块。
外部模块
通过提供模块所在外部文件的URL,也可以动态加载和导人模块。为此，首先要在模块声明后面加 上外部文件的URL,然后再导人模块成员：
module MyModule from "mymodule.js"; import myobject from MyModule；
以上声明会通知JavaScript引擎F载!nymodule. js文件，然后■从中加载名为MyModule的模块。 请读者注意，这个调用会阻塞进程。换句话说，JavaScript引擎在下载完外部文件并对其求值之前，不 会处理后面的代码。
如果你只想包含模块中对外公开的某些成员，不想把整个模块都加载进来，可以像下面这样使用 import 指令：
import nyobject from "mymodule.js"?
总之，模块就是•种组织相关功能的手段，而且能够保护全局作用域不受污染。

附录
B
严格模式
E
CMAScript 5最早引人了 “严格模式”（strict mode)的概念。.通过严格格式，可以在函数内部
选择进行较为严格的全局或局部的错误条件检测。使用严格模式的好处是可以提早知道代码中
存在的错误，及时捕获一些可能导致编程错误的KCMAScript行为。
理解严格模式的规则非常重要，ECMAScript的下一个版本将以严格模式为基础制定。支持严格模 式的浏览器包括丨E10+、Firefox 4+、Safari 5.1+和 Chrome。
1选择使用
要选择进人严格模式，可以使川严格模式的编译指示（pragma),实际上就是一个不会陚给任何变 量的字符串：
"use strict"；
这种语法（从ECMAScript3开始支持）HJ以向后兼容那些不支持严格模式的JavaScript引擎。支持 严格模式的引擎会启动这种模式，而不支持该模式的引擎就当遇到了一个未賦值的字符串字面贷，会忽 略这个编译指示。
如果是在全局作用域中（函数外部）给出这个编译指示，则整个脚本都将使用严格模式。换句话说， 如果把带有这个编译指示的脚本放到其他文件中，则该文件中的JavaScript代码也将处于严格模式下。 也可以只在函数中打开严格模式，就像下面这样：
function doSomething(){
"use strict"?
//其他代码
如果你没有控制页面中所有脚本的权力，建议只在需要测试的特定函数中开启严格模式。
B.2变量
在严格模式下，什么时候创建变量以及怎么创建变M都是有限制的。首先，不允许意外创建全局变 童。在非严格模式下，可以像下面这样创建全局变M:
//表声明变量
//非严格模式：创建全而变量
//严格模式：槐出ReferenceError
message = "Hello world! n;

718 附录B严格模式
即使message前面没有var关键字，即使没有将它定义为某个全局对象的属性，也能将message 创建为全局变量。但在严格模式下，如果给一个没有声明的变贵賦值，那代码在执行时就会抛出
ReferenceError〇
其次，不能对变量调用delete操作符。非严格模式允许这样操作，但会静默失败（返回false)。 而在严格模式下，删除变量也会导致错误。
//刪除变f
//非严格模式：静默失敗
//严格糢式：槐出ReferenceError
var color = "red*； delete color;
严格模式下对变量也有限制D特别地，不能使用implements、interface、let、package、 private、protected、public、static和yield作为变置名c这些都是保留字，将来的ECMAScript 版本中可能会用到它们。在严格模式下，用以上标识符作为变量名会导致语法错误。
3对象
在严格模式下操作对象比在非严格模式下更容易导致错误。一般来说，非严格模式下会静默失败的 情形，在严格模式下就会抛出错误。因此，在开发中使用严格模式会加大早发现错误的可能性。
在下列情形下操作对象的属性会导致错误：
□为只读届性賦值会抛出TypeError;
□对不可配置的（nonconfigurable)的属性使用delete操作符会拋出TypeError;
□为不可扩展的（nonextensible)的对象添加属性会抛出TypeError。
使用对•象的另一个限制与通过对象字面量声明对象有关。在使用对象字面量时，属性名必须唯一。 例如：
//重名属性
//非严格檨式：没有#误，以第二个属性为准 //严格棋式：抛出语法锊误
var person = {
name： •Nicholas' name: "Greg"
}；
这里的对象person有两个属性，都叫name。 在非严格模式下，person对象的name属性值是第 二个，而在严格模式下，这样的代码会导致语法错误。
4函数
首先，严格模式要求命名函数的参数必须唯一。以下面这个函数为例：
//重名麥数
//非严格模式：没有錯误，只能访问第二个参數 //严格糢式：抛出语法鐯误

附录B严格模式 719
function sum (num, num){
//do something
}
在非严格模式下，这个函数卢明不会抛出错误。通过参数名只能访问第二个参数，要访问第一个参 数必须通过arguments对象。
在严格模式下，arguments对象的行为也有所不同。在非严格模式下，修改命名参数的值也会反 映到arguments对象中，而严格模式下这两个值是完全独立的。例如：
//修改命名参数的值
//祁严格摟式：修改会反块到arguments中 //严格模式：修St不会反映到arguments +
function showValue(value){ value = "Foo";
alert(value); //"Foo"
alert (arguments [0]);//非严格模式：MFoou //严格模式："Hi"
}
showValue("Hi");
以上代码中，函数showValue ()只有一个命名参数value。调用这个函数时传人了一个参数"Hi", 这个值赋给r value。而在函数内部，value被改为”Foo"。在非严格模式下，这个修改也会改变 arguments[0]的值，似在严格模式下，arguments[0]的值仍然是传人的值〇
另-个变化是淘汰了 arguments.callee和arguments.caller。在非严格模式下，这两个属 性一个引用函数本身，一个引用调用函数。时在严格模式下，访问哪个属性都会抛出TypeError。 例如：
//访问 arguments .callee //砟严格模式：没有问题 //严格模式：振出TypeError
function factorial(num){ if (num <- 1) { return 1；
} else {
return num * arguznents.callee(num-l)
var result=factorial(5);
类似地，尝试读写函数的caller■属性，也会导致抛出TypeError。所以，对于上面的例子而言， 访问factorial .caller也会抛出错误。
与变贵类似，严格模式对函数名也做出r限制，不允许用implements、interfac:e、let、package、 private、protected、public、static 和 yield 作为函数名。
对函数的最后一点限制，就JS只能在脚本的顶级和在确数内部声明函数。也就足说，在if语句中 声明函数会导致语法错误：

720 附录B严格模式
//在if语句中声明為数
//非严格模式：将轟数提升到if语句外部 //严格糢式：抛出语法镨误
if (true) {
function doSomeChingO {
"...
}
在非严格模式下，以上代码能在所有浏览器中运行，但在严格模式F会导致语法错误。
5 eval ()
饱受垢病的evai 〇兩数在严格模式下也得到/提升。最大的变化就是它在包含上下文中不再创建 变呆或函数。例如：
//使用eval〇创建变量
//非严格模式：弹出对话框農示10
//严格樓式：调用alert tx>时会抛出ReferenceError
function doSomething0{ eval(Mvar x=l〇")； alert <x)；
}
如果是在非严格模式K，以上代码会在函数doSomething U中创建一个局部变董x，然后alert O 还会显示该变M的值。但在严格模式下，在doSomethingU阐数中调用evalU不会创建变最X，因此 调用alert ()会导致抛出HeferenceError,因为x没有定义。
坷以在eval()屮声明变M和涵数，何这些变檄或函数只能在被求值的特殊作用域中有效，随后就 将被销毁。因此，以下代码以运行，没有问题：
"use strict"；
var result = eval(Mvar X--10, y=ll? x+y")； alert(result)； //21
这里在eval ()中声明了变Mx和y，然后将它们加在一起，返回了它们的和。于是，result变 量的值是21,即x和y相加的结果。而在调用alert ()时，尽Ux和y已经不存在了，result变最 的依仍然是有效的。
6 eval 与 arguments
严格模式已经明确禁止使用eval和arguments作为标识符，也不允许读写它们的值。例如：
//把eval和arguments作为变爱引用 //非严袼禊式：没问题，不出鉗 //严格禊式：抛出语法错误
var eval = 10;
var arguments - "Hollo world!";

附录B严格模式 721
在非严格模式下，可以重写eval,也可以给arguments赋值。但在严格模式下，这样做会导致语 法错误。不能将它们用作标识符，意味者以F几种使用方式都会抛出语法错误：
口使用var声明；
□賦予另一个值；
□尝试修改包含的值，如使用—h □用作函数名；
□用作命名的函数参数；
□在try-catch语句中用作例外名。
7 抑制 this
JavaScript中•个最大的安全问题，也是敁容易让人迷茫的地方，就是在某些情况下如何抑制this 的值。在非严格模式下使用函数的apply()或call()方法时，null或undefined值会被转换为全局 对象。而在严格模式下，函数的this值始终是指定的值，无论指定的是什么值。例如：
//访问属性
//非严格棋式：访问全局属性
//严格模式：抛出铋误，因为this的值为null
var color = "red";
function di.splayColor<)( alert(this.color);
}
displayColor.call(null);
以上代码向displayColor .call <)中传人'/"null，如果在是非严格模式下，这意味着函数的this 值是全M对象。结果就是弹出对话框《示-red%而在严格模式下，这个函数的this的值是null,因 此在访问mxl】的属性时就会抛出错误。
B.8其他变化
严格模式还有其他一些变化，希望读者也能留意。首先是抛弃了 with语句。非严格模式下的with 语句能够改变解析标识符的路校，但在严格模式下，with被简化掉了。W此，在严格模式下使用with 会导致语法错误。
//with的语句用法 //非严格模式：允许 //严格糢式：抛出语法锊误
with(locacion){ alert(href)；
)
严格模式也去掉了 JavaScript中的八进制字面量。以0开头的八进制字面最过去经常会导致很多错 误。在严格模式下，八进制字面®已经成为无效的语法了。

722 附录B严格模式
//使用八进制字面贵 /，非严格楼式：值为8 //严格模式：抛出语法锊谋
var value = 010?
本书前面提到过，ECMAScript5也修改了严格模式下parselnU)的行为。如今，八进制字面量在 严格模式下会被当作以0开头的十进制字面量。例如：
/ /使用parselnt U觯析八进制字面量 //非严格模式：值为8 //严格模式：值为10
var value = parseIntC'010");

附录l
JavaScript 库
m.、.、
'V-

J
avaScript库可以帮助我们跨越浏览器差异的鸿沟，并对复杂的浏览器功能提供更为简便的访问
方式。程序库存两种形式：通用库和专用库。通用JavaScript库提供了对常见浏览器功能的访
问，可以作为网站或者Web应用的基础。专用库则只做特定的事，仅用于网站或者Web应用的某些部
分。本附录给出了这些库与其功能的概况，并提供/相关网站作为你的参考资源。
1通用库
通州JavaScript库提供横跨几个主题的功能。所有的通用库都尝试通过使用新API包装常见功能来
统一浏览器的接口、减小实现差异。某些API肴上去与原生功能很相似，而另一件则完全不同。通用库
一般提供与DOM交互的功能、支持Ajax、同时还有一些协助常见任务的工具方法。
1.1 YUI
它是一个开源JavaScript与CSS库，以一种组件方式设计的a这个库不只有一个文件；它包含了
很多文件，提供各种不同的配S,让你可以按需载人。YUI(Yahoo!User Interface Library,雅虎用户
界面库）涵盖了 JavaScript的所有方面，从基本的工具及帮助函数到完善的浏览器部件。在雅虎有一支
专门的软件n程师团队负责YUT,他们提供r优秀的文档和支持。
□协议：BSD许可证
□网站：ht^>://yuilibrary.com
1.2 Prototype
它是一个提供了常见任务API的开源库。最初是针对Ruby on Rails框架中的使用而开发的,Prototype
是类驱动的，旨在为JavaScript提供类定义和继承。因此，Prototype提供了很多类，用于将常见或复杂
功能封装为简单的API调用。Prototype只有一个单独的文件，可以很容易地放入任意页面。它是由Sam
Stephenson撰写并维护的。
□协议：MIT 许可证或者是 Creative Commons Attribution-Share Alike 3.0 Unported
□网站：

http://www.prototypejs.org/
C.1.3 Dojo Toolkit
Dojo Toolkit开源库基于一种包系统建模，一组功能组成-个包，可以按需载人。Dojo提供了范围
广泛的选项和配S,几乎涵盖T你要用JavaScript做的任何亊情。Dojo Toolkit由AJex Russell创建，并

724 附录 C JavaScript 库
由Dojo基金会的雇员和志愿者维护。
□协议：新BSD许可证或学术A由协议2.丨版 (3 网站：

http://www.dojotoolkit.oig/
C.1.4 MooTools
MooTools是一个为了精简和优化而设计的开源库，它为内置JavaScript对象添加了各种方法，以通 过接近的接U提供新功能，或者直接提供新的对象。MooTools的短小精悍受到一些Web开发者的青睐。 □协议：MIT许;SJ证 口 网站：

http://www.mootools.net/
C.1.5 jQuery
jQuery是一个给JavaScript提供了闲数式编程接口的开源库。它是一个完整的库，其核心是构建于 CSS选择器上的，用来操作DOM元素。通过链式调用，jQuery代码看J:去更像是对于应该发生什么的 描述而不是JavaScript代码。这种代码风格在设计师和原型制作人中非常流行。jQuery是由John Resig 撰写并维护的。
□协议：MIT许可证或通用公共许可证（GPL)
口网站：http:〃jquery.com/
C.1.6 MochiKit
MochKit是一个山一些小T具组成的开源库，它以完善的文档和完整的测试见长，拥有大量API及 相关范例文档以及数百个测试来确保质贵。MochiKit是由Bob Ippolito撰写并维护的。
□协议：MIT许可莕或学术由许可证2.丨版 I□网站：http:〃

www.mochikit.com/
C.1.7 Underscore.js
虽然严格来讲Underscorejs并不是一个通用的库，但它的确为JavaScript中的功能性编程提供了很 多额外的功能。其文样称Underscore.js是对jQuery的补充，提供了操作对象、数组、函数和其他JavaScript 数据类想的更多的低级功能。Underscore.js由DcoumentCloud的Jeremy Ashkenas维护。
□协议：MIT许可证
□网 i占：

http://documentcloud.github.com/underscore/
2互联网应用
互联网应用库是针对于简化完整的Web应用开发而设计的。它们并不提供应用问题的小块组件， 而是提供了快速应用开发的整个概念框架。虽然这®库也可能提供一些底层功能，但他们的H标是帮助 用户快速开发Web应用。
C.2.1 Backbone.js
Backbonejs是构建于Underscore.js基础之丨:的一个迷你MVC开源库，它针对单页应用进行优化，

附录 C JavaScript 库 725
让你能够随着应用状态变化方便地更新页面的任意部分。Backbone.js由DcoumentCloud的Jeremy Ashkenas 维护。
□协议：MIT许可证
C1 网站：http:〃documentcloud.github.com/backbone/
C.2.2 Rico
Rico是一个开源库，旨在让行为丰富的互联网应用的开发更加简单。它提供了 Ajax、动画、样式 以及部件的丁具。这个库由一些志愿者组成的小团队维护，但是2008年起开发速度大大减慢 □协议：Apache许可证2.0 CI 网站：http:〃openrico.org/
C.2.3 qooxdoo
它是一个旨在协助整个Ji联网应用开发周期的开源库。qooxdoo实现了它G己的类和接n,用于创 建类似于传统面向对象语言的编程模型。这个库包含了一个完整的GUT工具包以及用于简化前端构建过 程的编译器。qooxdoo起初是l&lwebhosting公司（www.landl.com)的内部使用库，后来基于开源协 议发布了。1&丨骋用了一些全职开发者来维护和开发这个库。
□协议：GNU较宽松公共许可证（LGPL)或者Eclipse公共许可证（EPL)
C3 网站：

http://www.qooxdoo.org/
3动画和特效
动画和其他视觉特效也成为了 Web开发的5:要部分。在网页上做出流畅的动画是一个很重要的任 务，一些开发者已经做出了易用的库，提供流畅的动画和特效。前面提到的很多通用JavaScript库也有 动両功能。
C.3.1 script.aculo.us
script.aculo.us是Prototype的“同伴”，它提供了出色特效的简单使用方式，使用的东西不超过是CSS 和DOM。Prototype必须在使用script.aculo.us之前载人。script.aculo.us是最流行的特效库之一，世界上 很多网站和Web应用都在使用它。它的作者Thomas Fuchs积极地维护着script.aculo.us。
口协议：MIT许可证 U 网站：

http://script.aculo.us/
C.3.2 moo.fx
moo.fx开源动画库是设计在Prototype或者MooTools之上运行的。它的目标是尽可能小（最新的版 本是3KB),并支持开发人员用尽可能少的代码创建动画。MooTools是默认包含moo.fic的，但也可以单 独卜载用于Prototype中。
□协议：M1T许可证 口 网站：

http://moofic.inad4milk.net/

726 附录 C JavaScript 库
C.3.3 Lightbox
Lightbox是一个用于在任何页面上创建图像浮动层的JavaScript库，依赖于Prototype和 scriptaculaus来实现它的视觉特效。基本的理念是让用户在-个浮动层中浏览-个或者一系列图像， 而不必离开当前页面。Lightbox浮动层无论是外观还是过渡效果都可以自定义。Lightbox由Lokesh Dhakar开发并维护。
□协议：创作共用协议2.5
□网站：

http://www.huddletotegher.com/projects/lightbox2/
4加密
随着Ajax应用的流行，对于浏览器端加密以确保通讯安全的需求也越来越多。幸好，一些人已经 在JavaScript中实现了常用的安全算法。这些库大部分并没有其作者的正式支持，但还是被广泛应用着。
C.4.1 JavaScript MD5
该开源库实现了 MD4、MD5以及SHA-1安全散列函数。作者Paul Johnston和其他一些贡献者每个 算法一个文件的创建了这么丰富的库，它可以用于Web应用。主页上提供了散列算法的概述，对于其 弱点的讨论以及适当的使用方法。
□协议：BSD许可证 O 网站：http:〃pajhome.org.uk/crypt/md5
C.4.2 JavaScrypt
该JavaScript库实现了 MD5和AES (256位）加密算法。JavaScrypt的网站提供了很多关于密码学 历史及其在计算机中应用的信息。但是缺乏关于如何将该库集成到Web应用中的基本文档，JavaScrypt 的代码里面全都是深奥的数学处理和计算。
口协议：公共域
口网站：ht中://

www.fourmilab.ch/javascrypt/

附录
JavaScript 工具
•^JavaScript代码和用其他语言编写代码很像，使用工具能够提高工作效率。JavaScript开发人 4员可用的工具数量一度爆发性增长，使得查找问题、优化和部署基于JavaScript的解决方案更 为简单。艽中一些工具是专为JavaScript设计使用的，而其他一些吋以在浏览器之外运行。本附录对其 中一些工具给出了概述，并额外提供了信息资源。
1校验器
JavaScript调试有一个问题，很多IDE并不能在输人的时候Q动指出语法错误。大多数开发者写了 一部分代码之后要将其载人到浏览器中査找错误。你可以通过在部署之前校验JavaScript代码，以便显 著地减少此类错误。校验器提供了基本的语法检査，并给出某些风格的警告。
1.1 JSLint
JSLint是一个由Douglas Crockford撰写的JavaScript校验器。它可以从核心层次上检査语法错误， 伴随跨浏览器问题的最小共同点检查（它遵循最严格的规则来确保代码到处都能运行)。你可以启用 Crockford对于代码风格的替告,包括代码格式、未声明的全局变量的使用以及其他更多警告。尽管JSLint 是用JavaScript写的，但是通过基于Java的Rhino解释器，它可以在命令行中运行，或者通过WScript 或者其他JavaScript解释器。网站上提供了针对各种命令行解释器的自定义版本。
口价格.•免费
口网站：http:〃

www.jslint.com/
1.2 JSHint
JSHint是JSLint的一个分支，为应用规则提供了更多的自定义功能。与JSLint类似，它首先检査语 法错误，然后检査有问题的编码模式。JSLint的每一项检丧JSHint都有，但开发人员可以更好地控制应 用什么规则。与JSLint—样，JSHint也能使用Rhino在命令行中运行。
□价格：免费
□网站：http:〃

www.jshint.com/
1.3 JavaScript Lint
它和JSLint完全不相干，JavaScript Lint是Matthias Miller写的一个基于C的JavaScript校验器。它 使用了 SpiderMonkey (即Firefox所用的JavaScript解释器）来分析代码并査找语法错误。这个工具包含

728 附录 D JavaScript 工具
大fi选项，可以启用额外关丁•编码风格的替吿，以及未声明的变摄和不可到达的代码替吿。Windows和 Macintosh h都有可用的JavaScript Unt,源代码也nj以fi由取得。
□价格：免费
□网站：

http://www.javascriptlint.com/
2压缩器
JavaScript构建过程中很重要的一部分，是压缩输出并移除多余的字符。这样做可以确保传送到浏 览器的字节数最小化，最终加速了用户体验。有几种压缩比率不同的工具可以选择。D.2.1 JSMin JSMin是由DouglasCrockford写的_ -个基于C的压缩器，进行最基本的JavaScript/R缩。它主要是 移除空白和注释，确保最终的代码依然町以被顺利执行》JSMiti有Windows执行程序，包括C版本代码， 还有其他语言的代码：
口价格：免费
口 网站：

http://www.crockford.com/javascript/jsmin.html
D.2.2 Dojo ShrinkSafe
负责Dojo Toolkit的同一批人开发丫…个叫做ShrinkSafe的工具，它使用/ Rhino JavaScript解释器 首先将JavaScript代码解析为记号流，然后用它们来安全压缩代码。和JSMin—样，ShrinkSafe移除多 余的空内符（不包括换行）和注释，但是还更进一步将局部变贵替换为两个字符长的变ft名。最后可以 比JSMin产生更小输出，W没有引人语法错误的风险。
□价格：免费
I□网站：http:〃 shrinksafe .dojotoolkit.org/
D.2.3 YU I Compressor
YUI 小组有一个叫做 YUI Compressor 的压缩器。和 ShrinkSafe 类似，YUI Compressor利用了 Rhino 解释器将JavaScript代码解析为记号流，并移除注释和空内字符并替换变量名。与ShrinkSafe不同，YUI Compressor还移除换行并进行一些细微的优化进一步节省字节数。一般来说，YUI Compressor处理过的 文件要小于JSMin或者ShrinkSafe处理过的文件。
□价格：免费
□网站：

http://yuilibrary.com/projects/yuicompressor
3单元测试
TDD (Test-drivendevelopment,测试驱动开发）是一种以单元测试为核心的软件开发过程。直到最 近，才出现了一些在JavaScript进行单元测试的工具。现在多数JavaScript库都在它们自已的代码中使用 了某种形式的单元测试，其中一些发布了单元测试框架让他人使用。
D.3.1 JsUnit
最早的JavaScript承元测试框架，不綁定于任何特定的JavaScript库。JsUnit是Java知名的JUnit测

附录 D JavaScript 工具 729
试框架的移植。测试在页面中运行，并可以设置为自动测试并将结果提交到服务器。它的网站上包含了 例子和基本的文档：
□价格：免费
Q 网站：

http://www.jsunit.net/
D.3.2 YUI Test
作为YU丨的一部分，YUl Test不仅可以用于测试使用YUI的代码，也可以测试网站或者应用中的 任何代码。YUI Test包含了简单和复杂的断言，以及一种模拟简单的鼠标和键盘事件的方法。该框架在 Yahoo!DeveloperNetwork上有完整的文的描述，包含了例子、API文灼和更多内容。测试时在浏览器中 运行，结果输出在页面上。YU1便使用YUI Test来测试整个库。
□价格：免费
□网站：

http://yuilibrary.com/projects/yuitest/
D.3.3 D0H
DOH( Dojo Object Harness )在发布给大家使用之前，最初是作为Dojo内部的单元测试工具出现的。 和其他框架一样.单元测试是在浏览器中运行的。
□价格：免费
口网站：

http://www.dojotoolkit.oig/
D.3.4 qUnit
qUnit是为测试jQuery而开发的…个单元测试框架。jQuery本身的确使用qUnit进行各项测试。除 此之外，qUnit与jQuery并没有绑定关系，也可以用它来测试所有JavaScript代码。qUnit的特点是简单 易用，一般开发人员很容易上手。
□价格：免费
口网站：

https://github.com/jquery/qunit
D.4文档生成器
大多数IDE对于主流语言都包含f文档生成器。由于JavaScript并没有官方的IDE,过去文档一般 都是手工完成，或者是利用针对其他语言的文档生成器。然而，现在终于冇一些专门针对JavaScript的 文档生成器了。
D.4.1 JsDoc Toolkit
JsDoc Toolkit是最早出现的JavaScript文档生成器之一。它要求你在代码中输人类似Javadoc的注释, 然后处理这些注释并输出为HTML文件。你可以自定义HTML的格式，这®使用预定义的JsDoc模板 或者创建自己的模版。JsDoc Toolkit可以以Java包的形式获得。
□价格：免费
□网站：

http://code.google.eom/p/jsdoc-toolkit/

730 附录 D JavaScript 工具
D.4.2 YU I Doc
YUI Doc是YUI的文挡生成器。该生成器以Python书写，所以它要求安装冇Python运行时环境。 YUI Doc b了以输出集成了属性和方法搜索（用YUI的自动完成挂件实现的）的HTML文件。和JsDoc 一样，YUTDoc要求源代码中使用类似Javadoc的注释。默认的HTML可以通过修改默认的HTML模板 文件和相关的样式表来更改。
□价格：免费
Q 网站：

http://www.yuilibrary.com/projects/yuidoc/
D.4.3 AjaxDoc
AjaxDoc的H标和前+tf提到的牛成器有些差异。它不为JavaScript文档生成HTML文件，而是创建 与针对.NET语0 (如C#和Visual Basic. NET)所创建文件相同格式的XML文件。这样做就可以由标准 的.NET文档牛:成器创建HTML文件形式的文档。AjaxDoc使用类似•丁•所有.NET语言用到的文档注释格 式。创建AjaxDoc是针对ASP.NET的Ajax解决方案，但是它也可以用于单独的项目。
□价格：免费
口网站：

http://www.codeplex.com/ajaxdoc
D.5安全执行环境
随着mashup应用越来越流行，对于允许来A外界的JavaScript存在于同一个页面上并执行有着越来 越多的需求3这导致了一些访问受限功能的安全问题。以下工具旨在创建安全的执行环境，其中不同来 源的JavaScript可以共存，而不会互相影响。
D.5.1 ADsafe
由DouglasCrockford创建，ADsafe是JavaScript的了-集，这个子集被认为可以被第三方脚本安全访 问。对于用ADsafe运行的代码，页面必须包含ADsafe JavaScript库并标记为ADsafe挂件格式。因此， 代码可以在任何页面上安全执行。
□价格：免费
(□网站：

http://www.adsafe.org/
D.5.2 Caja
Caja用一种独特的方式来确保JavaScript的安全执行。类似于ADsafe, Caja定义了 JavaScript的一 个可以用安全方式使用的子集。Caja继而可以清理JavaScript代码并验证它只按照预期的方式运行。作 为该项H的一部分，有一种叫做Cajita的语言，它是JavaScript功能的一种更小的子集。Caja还处于幼 年期，但是B经展示了很多前景，允许多个脚本在同一个页面执行而没有恶意活动的可能。
□价格：免费
□网站：http:〃code.googlc.com/p/google-caja/  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter25.md)