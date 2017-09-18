##  第20章 JSON([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 理解JSON语法
- [ ] 解析JSON
- [ ] 序列化JSON  

经有一段时间，XML是互联网上传输结构化数据的事实标准。Web服务的第一次浪潮很大程 田度上都是建立在XML之上的，突出的特点是服务器与服务器间通信。然而，业界•-直不乏质 疑XML的声音。不少人认为XML过于烦琐、冗长。为解决这个问题，也涌现f 一些方案。不过，Web 的发展方向已经改变了。
2006 年，Douglas Crockford 把•TSON( JavaScript Object Notation, JavaScript 对象表示法）作为 IETF RFC4627提交给丨ETF,而JSON的应用早在200丨年就已经开始了。JSON是JavaScript的一个严格的子 集,利用了 JavaScript中的_ --些模式来表示结构化数据。Crockford认为与XML相比，JS0N是在JavaScript 中读写结构化数据的更好的方式。因为可以把JSON直接传给eval (),而且不必创建DOM对象。
关于JS0N,最重要的是要理解它是一种数据格式，不是一种编程语#。虽然具有相同的语法形式， 但JSON并不从M于JavaScript。而且，并不是只冇JavaScript才使用JSON,毕竟JS0N只是一种数据 格式。很多编程语言都有针对JS0N的解析器和序列化器。
20.1语法
JSON的语法可以表示以下三种类型的值。
- [ ] 简单值：使用与JavaScript相闻的语法，可以在JSON中表示字符串、数值、布尔值和null。
{K JSON不支持 JavaScript 中的特殊值 undefined。
口对象：对象作为一种复杂数据类型，表示的是一组冇序的键值对儿。而每个键值对儿中的值可 以是简单值，也可以是复杂数据类型的值。
- [ ] 数组：数组也是一种复杂数据类型，表示一组也序的值的列表，可以通过数值索引来访问其中 的值。数组的值也可以是任意类型——简单值、对象或数组。
JSON不支持变*、函数或对象实例，它就是--种表示结构化数据的格式，虽然与JavaScript中表示 数据的某些语法相同，但它并不局限于JavaScript的范畴。
20.1.1简单值
最简中.的JSON数据形式就是简单值。例如，下面这个值是有效的JSON数据:

20.1 语法 563
5
这是JSON表示数值5的方式。类似地，F面是JSON表示字符串的方式：
"Hello world!"
JavaScript字符串与JSON字符串的最大区别在于，JSON字符串必须使用双引号（单引号会导致语 法错误)。
布尔值和null也是有效的JSON形式。但是，在实际应用中，JSON更多地用来表示更复杂的数据 结构，而简单值只是整个数据结构中的一部分。
20.1.2对象
JSON中的对象与JavaScript字面童稍微有一®不下面是一个JavaScript中的对象字面量:
var person = {
name： "Nicholas*,
age： 29
>;
这虽然是开发人员在JavaScript中创建对象字面量的标准方式，但JSON中的对象要求给属性加引 号。实际上，在JavaScript中，前面的对象字面量完全可以写成下面这样：
var object = {
"name": "Nicholas",
"age": 29
);
JSON表示上述对象的方式如F:
"name": "Nicholas",
"age": 29
}
与JavaScript的对象字面量相比，JSON对象有两个地方不一样。首先，没有声明变量（JSON中没 有变*的概念)。其次，没有末尾的分号（因为这不是JavaScript语句，所以不需要分号X再说一遍， 对象的属性必须加双引号，这在JSON中是必需的。属性的值可以是简单值，也可以是复杂类型值，因 此可以像下面这样在对象中嵌人对象：
{
■name": -Nicholas",
•age*: 29,
•school"： {
•na^e"s "Merrimack College"/
"location*： "North Andover, MA"



这个例子在顶级对象中嵌人了学校（"school")信息。虽然有两个》name"属性，但由于它们分别 属于不同的对象，因此这样完全没冇问题。不过，N -个对象中绝对不应该出现两个同名属性。
与JavaScript不同，JtSON中对象的属性名任何时候都必须加双引号。手工编写JSON时，忘了给对 象属性名加双引号或者把双引号写成单引号都是常见的错误。

564 第 20 章 JSON
20.1.3数组
JSON中的第二种复杂数据类型是数组。JSON数组采用的就是JavaScript中的数组字面置形式。例 如，下面是JavaScript中的数组字面量：
var values = 125, "hi", true]；
在JSON中，可以采用同样的语法表示同一个数组：
[2S, "hi", true)
同样要注意，JSON数组也没有变量和分兮。把数组和对象结合起来，可以构成更复杂的数据集合, 例如：
[
"title*: "Professional JavaScript", "authors":(
"Nicholas C. Zakas"
edition: 3, year: 2011
•title”： "Professional JavaScript", "authors":[
•Nicholas C. Zakas"
].
edition: 2, year： 2009
■title*: 'Professional Ajax", •authors"：[
■Nicholas C. Zakas", "Jeremy McPeak"*
"Jog Fawcett"
edition： 2, year： 2008
"title"： "Professional Ajax", •authors*:[
"Nicholas C. 2akas"/ "Jeremy McPeak",
"Joe Fawcett"
edition： 1, year： 2007
"title"： "Professional JavaScript", "authors":[
•Nicholas C. Zakas"
],
edition： 1,

year： 2006
20.2解析与序列化 565
这个数组中包含一些表示图书的对象。每个对象都有几个属性，其中*个M性是"authors",这个 属性的值又足一个数组。对象和数组通常是JS0N数据结构的最外层形式（当然，这不是强制规定的）, 利用它们能够创造出各种各样的数据结构。
20.2解析与序列化
JS0N之所以流行，拥有与JavaScript类似的语法并不是全部原W。更車:要的一个原W是，对以把 JS0N数据结构解析为有用的JavaScript对象。~ XML数据结构要解析成D0M文档而且从中提取数据 极为麻烦相比，JS0N可以解析为JavaScript对象的优势极其明砀。就以上一节中包含一组图书的JS0N 数据结构为例，在解析为JavaScript对象后，只需要下面一行简单的代码就可以取得第H本书的书名：
books[2].title
当然,这里是假设把解析后JSON数据结构得到的对象保存到r变贵books中。再看看下面在DOM 结构中查找数据的代码：
doc.getElementsByTagName<"book”）【2].getAttribute{"title")
看看这些多余的方法调用，就不难理解为什么JSON能得到JavaScript开发人员的热烈欢迎了。从 此以后，JSON就成了 Web服务开发中交换数据的事实标准。
JSON 对象
■¥■期的JSON解析器基本上就是使用JavaScript的eval ()函数。由于JSON是JavaScript语法的子 集，因此eval(>闲数可以解析、解释并返回JavaScript对象和数组。ECMAScript5对解析JSON的行 为进行规范，定义了全局对象JSON。支持这个对象的浏览器有IE8+、Firefox3.5+、Safaris、Chrome 和 Opera 10.5+。对于较早版本的浏览器，可以使用'-个 shim:

https://github.com/douglascrockford/JSON-js。 在旧版本的浏览器中，使用eval 〇对JSON数据结构求值存在风险，W为可能会执行一些恶意代码。 对丁不能原也支持JSON解析的浏览器，使用这个shim是最佳选择。
JSON对象有两个方法：stringifyU和parse()。在最简单的情况下，这两个方法分别用于把 JavaScript对象序列化为JSON字符串和把JSON字符串解析为原生JavaScript值。例如：
var book = {
title： "Profess Lonal JavaScript"# authors:[
"Nicholas C. Zakas"
edition： 3, year: 2011
};
var jsonText = JSON.stringify(book)；
JSONStringifyExampleOl. him

566 第 20 章 JSON
这个例子使用JSON. stringify ()把一个JavaScript对象序列化为一个JSON字符串，然后将它保 存在变M jsonText中。默认情况+卜'，JSON.stringifyO输出的JSON字符串不包含任何空格字符或 缩进，因此保存在jscmText中的字符串如下所示：
{"title"："Professional JavaScript"authors":["Nicholas C. Zakas"],"edition*：3,
-year*:2011>
在序列化JavaScript对象时，所有函数及原型成员都会被有意忽略，不体现在结果中。此外，值为 undefined的任何属性也都会被跳过。结果中最终都是值为有效JSON数据类型的实例属性。
将JSON字符串直接传递给JSON.parse ()就可以得到相应的JavaScript值。例如，使用下列代码 就可以创建与book类似的对象：
var bookCopy = JSON.parse{j sonText)；
注意，虽然book与bookCopy具有相同的属性，但它们是两个独立的、没有任何关系的对象。
如果传给JSON.parse ()的字符串不是冇效的JSON,该方法会抛出错误。
20.2.2序列化选项
实际i:，JSON.stringifyO除了要序列化的JavaScript对象外，还可以接收另外两个参数，这两 个参数用于指定以不同的方式序列化JavaScript对象。第一个参数是个过滤器，可以是一个数组，也可 以是一个函数；第二个参数是-个选项，表示是否在JSON字符串中保留缩进。单独或组合使用这两个 参数，可以更全面深人地控制JSON的序列化。
1.过滤结果
如果过滤器参数是数组，那么JSON.stringifyO的结果中将只包含数组中列出的属性。来看下 面的例子。
var book = {
•title"： "Professional JavaScript",
"authors*：(
■Nicholas C. Zakas"
],
edition： 3, year： 2011
};
var jsonText = JSON.stringify(book,	"edition"])；
JSONStringifyExampleOI, htm
JSON.stringifyf)的第二个参数是一个数组，其中包含两个字符串："title•和"edition-。这 两个厲性与将要序列化的对象中的属性是对应的，因此在返冋的结果字符串中，就只会包含这两个属性:
{"title":"Professional JavaScript"#"edition"：3>
如果第二个参数是函数，行为会稍有不同。传人的函数接收两个参数，属性（键）名和属性值。根 据厲性（键〉名可以知道应该如何处理要序列化的对象中的属性。属性名只能是字符串，而在值并非键 值对儿结构的值时，键名可以是空字符串。
为了改变序列化对象的结果，函数返回的值就是相应键的值。不过要注意，如果函数返回了

20.2解析与序列化 567
undefined，那么相应的属性会被忽略。还是看…个例子吧〇



var book = {
"title": "Professional JavaScript",
■authors":[
"Nicholas C. Zakas"
),
edition: 3, year： 2011
var jsonText = JSON.stringify{book, functioa(key, value){ switch(key){
c&0e "authors":
return value.join("#")
case •year":
return 5000;
case "edition"s
return undefi ned；
>);
default：
return value;
JSONStringifyExampleQ2.htm
这里，函数过滤器根据传人的键来决定结果。如果键为"authors%就将数组连接为一个字符串； 如果键为-year•，则将其值设置为5000;如果键为"edition",通过返回undefined删除该属性。 最后，一定要提供default项，此时返回传人的值，以便其他值都能正常出现在结果中。实际上，第 一次调用这个函数过滤器，传人的键是一个空字符串，而值就是book对象。序列化后的JSON字符串 如下所示：
{"title":*Professional JavaScript"#"authors"："Nicholas C. Zakas","year*:5000}
要序列化的对象中的每一个对象都要经过过滤器，因此数组中的每个带有这些属性的对象经过过滤 之后，每个对象都只会包含1'title”、"authors"和•year"1属性。
Firefox 3.5和3.6对JSON. stringify ()的实现有一个bug,在将函数作为该方法的第二个参数时 这个bug就会出现，即这个函数只能作为过滤器：返冋undefineci意味着要跳过某个属性，而返W其 他任何值都会在结果中包含相应的属性。Firefox 4修复了这个bug。
2.字符串缩进
JSON. stringify ()方法的第三个参数用于控制结果中的缩进和空白符。如果这个参数是一个数 值，那它表示的是每个级别缩进的空格数。例如，要在每个级别缩进4个空格，可以这样写代码：
var book
◎
"title"： "Professional JavaScript"#
"authors*：[
■Nicholas C. Zakas*
],
edition： 3,
year： 2011
20

568 第 20 章 JSON
}；
var jsonText = JSON.stringify (boo)c, null, 4}；
JSONStringifyExample03.htm
保存在jsonText中的字符市如下所示：
{
"title*： "Professional JavaScript",
"authors'* ：[
"Nicholas C. Zakas"
],
"edicion": 3,
"year": 2011
}
不知道读者注意到没有，JSON. stringifyO也在结果字符串中插人了换行符以提高可读性。只要 传人有效的控制缩进的参数值，结果字符串就会包含换行符。（只缩进而不换行意义不大。）最大缩进空 格数为10,所有大丁• 10的值都会A动转换为10。
如果缩进参数迠一个字符中而非数值，则这个字符串将在JSON字符串中被用作缩进字符（不再使 用空格)。在使用字符串的情况下，可以将缩进字符设置为制表符，或者两个短划线之类的任意字符。
var jsonText = JSON.stringify(book, null, 这样，jsonText中的字符串将变成如下所示：
--"Citle": "Professional JavaScript",
--"authors":[
	"Nicholas C. Zakas*
--]t
--*edition": 3,
--"year": 2011 }
缩进字符串运长不能超过10个字符长。如果字符串长度超过了 10个，结果中将只出现前丨〇个字 符。
toJS0N()方法
冇时候，“0131：^1^&()还是不能满足对某些对象进行自定义序列化的怎求。在这些情况下， 可以通过对象上调用t〇JS0N( >方法，返回其身的JSON数据格式。原生Date对象有一个t〇JS0N() 方法，能够将JavaScript的Date对象自动转换成丨SO 8601 U期字符串（与在Date对象上调用 LoISOString ()的结果完全一样)。
可以为任何对象添加toJS0N (>方法，比如：
var book = {
"tide" ： "Professional JavaScript", "authors"：[
"Nicholas C. Zakas"
K
edition： 3, year： 2011, toJSONi fuactionO {

return this.title；
20.2解析与序列化 569
>
var jsonText = JSON.scringify{book)；
JSONStringifyExampie05.htm
以上代码在book对象上定义/•一个toJSONO方法，该方法返回图书的书名。与Date对象类似, 这个对象也将被序列化为一个简单的字符串而非对象。可以让t〇JSON( >方法返回任何序列化的值，它 都能正常工作。也可以让这个方法返回undefined,此时如果包含它的对象嵌人在另一个对象中，会 导致该对象的值变成null,而如果包含它的对象是顶级对象，结果就是undefined。
t〇JSON()可以作为函数过滤器的补充，因此理解序列化的内部顺序十分《要。假设把一个对象传 人JSON.stringifyO,序列化该对象的顺序如卜_。
(1>如果存在t〇JS0N()方法而且能通过它取得有效的值，则调用该方法。否则，按默认顺序执行序 列化。
如果提供了第二个参数，应用这个函数过滤器。传人函数过滤器的值是第(1)步返冋的值。
对第(2)步返冋的每个值进行相应的序列化。
如果提供了第三个参数，执行相应的格式化。
无论是考虑定义t〇JS0N()方法，还是考虑使用函数过滤器，亦或需要同时使用两者，理解这个顺 序都是至关重要的。
20.2.3解析选项
20
JSON.parseO方法也可以接收另一个参数，该参数是一个函数，将在每个键值对儿上调用。为了 K别JSON.stringifyt>接收的替换（过滤）函数（replacer),这个函数被称为还原函数（reviver), 但实际上这两个函数的签名是相同的——它们都接收两个参数，一个键和一个值，而且都需要返问一个 值。
如果还原函数返回undefined，则表示要从结果中删除相应的键；如果返回其他值，则将该值插 人到结果中。在将日期字符串转换为Date对象时，经常要用到还原函数。例如：



var book = {
};
"title": "Professional JavaScript", "authors"：[
"Nicholas C. Zakas"
).
edition: 3, year： 2011,
releaaeDatet new Dat«(2011, 11, 1)
var jsonText = JSON.stringify(book);
var bookCopy b JSON.pares (jsoziText, function〇cey, value) { if (key «« "releaseDate"){ return new Date(value)；




570 第 20 章 JSON
});
alert(bookCopy.releaeeDate.getPullYear())；
JSONParseExamp 丨e02.htm
以i:代码先是为book对象新增了一个releaseDate属性，该属性保存若一个Date对象。这个 对象在经过序列化之后变成T有效的JSON字符ffl,然后经过解析又在boolccopy中还原为一个Date 对象。还原函数在遇到"releaseDate•键时，会基于相应的值创建一个新的Date对象。结果就是 bookCopy.releaseDate属性中会保存一个Date对象。正因为如此，才能基于这个对象调用 getFullYear (> 方法〇
20.3小结
JS0N是-个轻量级的数据格式，可以简化表示复杂数据结构的工作童。JS0N使用JavaScript语法 的T集表示对象、数组、字符串、数值、布尔值和null。即使XML也能表示同样复杂的数据结果，但 JSON没有那么烦琐，而.丨i在JavaScript中使用更便利。
ECMAScript 5定义了一个原生的JS0N对象，可以用来将对象序列化为JSON字符串或者将JSON 数据解析为JavaScript对象。JS0N. stringify 和JSON.parse 0方法分别用来实现上述两项功能。 这两个方法都有一些选项，通过它们〇丨以改变过滤的方式，或者改变序列化的过程。
原生的JSON对象也得到了很多浏览器的支持，比如IE8+、Firefox3.5+、Safari 4+、Opera 10.5和 Chrome。




Ajax 与 Comet
本章内容
- [ ] 使用 XMLHttpRequest 对象 - [ ] 使用 XMLHttpRequest 事件 - [ ] 跨域Ajax通信的限制
2
005 年，Jesse James Garrett 发表了一篇在线文章，题为 “Ajax: A new Approach to Web
Applications”（ 

http://www.adaptivepath.com/ideas/essays/archivcs/00〇385.php )。他在这篇文章
里介绍了一种技术，用他的话说，就叫Ajax,是对Asynchronous JavaScript + XML的简写。这一技术
能够向服务器请求额外的数据而无须卸载页面，会带来史‘好的用户体验。Garrett还解释了怎样使用这
一技术改变自从Web诞生以来就一直沿用的“单击，等待”的交互模式。
Ajax技术的核心是XMLHttpRequest对象（简称XHR),这是由微软首先引人的一个特性，其他
浏览器提供商后来都提供了相同的实现。在XHR出现之前，Ajax式的通倍必须借助一些hack手段来实
现，大多数是使用隐藏的框架或内嵌框架。XHR为向服务器发送请求和解析服务器响应提供了流畅的
接口。能够以异步方式从服务器取得更多信息，意味翁用户单击后，nj以不必刷新页面也能取得新数据。
也就是说，可以使用XHR对象取得新数据，然后再通过DOM将新数据插入到贞面中。另外，虽然名
字中包含XML的成分，但Ajax通信与数据格式无关；这种技术就是尤须刷新页面即可从服务器取得数
据，但不一定是XML数据。
实际上，Garrett提到的这种技术已经存在很长时间了。在Garrett撰写那篇文章之前，人们通常将
这种技术叫做远程脚本（remote scripting),而且曱.在1998年就有人采用不同的•于段实现了这种浏览器
与服务器的通信。再往前推，JavaScript需要通过Java applet或Flash电影等中间层向服务器发送请求。
而XHR则将浏览器原生的通倌能力提供给丫开发人员，简化了实现同样操作的任务。
在B命名为Ajax之后，大约是2005年底2006年初，这种浏览器与服务器的通信技术可谓红极一
时。人们对JavaScript和Web的全新认识，催生了很多使用原有特性的新技术和新模式。就前来说，
熟练使用XHR对象li经成为所有Web开发人员必须笮捤的一种技能。
21
XMLHttpRequest 对象
IE5是第-款引人XHR对象的浏览器。在IE5屮，XHR对象是通过MSXML库中的一个ActiveX 对象实现的。因此，在IE中可能会遇到三种不同版本的XHR对象，即MSXML2.XMLHttp、 MSXML2.XMLHt：tp.3.0 和 MXSML2.XMLHttp.6.0。要使用 MSXML 库中的 XHR 对象，耑要像第 18 章讨论创建XML文档时一样，编写一个闲数，例如：

572 第 21 章 Ajax 与 Comet
//适用于丨E7之前的版未 function createXKR(){
if (typeof arguments.callee.activeXString i= "string"){
var versions = (-MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLKttp"1#
i, len；
for (i=0,len=versions.length； i < len； i++){ try {
new ActiveXObject(versions[i]);
arguments.callee.activeXString = versionslij?
break；
} catch (ex){
//踺过
}
return new ActiveXObject(arguments.callee.activeXString);
}
这个函数会尽力根据IE中可用的MSXML库的情况创建最新版本的XHR对象。
IE7+、Firefox、Opera、Chrome和Safoi都支持原生的XHR对象，在这些浏览器中创建XHR对象 要像下面这样使用XMLHttpRequest构造闲数。
var xhr = new XMLHttpRequest{);
假如你只想支持IE7及更高版本，那么大n丨丢掉前面定义的那个函数，而只用原生的XHR实现。 但是，如果你必须还要支持IE的早期版本，那么则可以在这个createXHR()函数中加人对原生XHR 对象的支持。
function createXHR(){
if (typeof ZMLHttpRequeat I繼"undefined"){ return new XKLHttpRa<zuest {);
} else if (typ«o£ ActlvaXObject I = "undefined”（
if (typeof arguments.callee.activeXString l= "string"){
var versions = [ ”MSXML2*XMLHttp* 6•0•, "MSXML2.XMLHt tp.3.0", *MSXML2.XMLHttp*]#
i. len；
for (i=0,len=versions.length； i < len; i++){ try {
new ActiveXObject(versions[ij)?
arguments.callee.activeXString = versions(i]；
break；
} catch (ex){
//敌过
return new ActiveXObject(argomencs.callee.activeXString)；
else {
throw new Error(KNo XHR object available.1*)；
>
XHRExample01.htm

21.1 XMLHttpRequest 对象 573
这个函数屮新增的代码首先检测原生XHR对象是否存在，如果存在则返冋它的新实例。如果原生 对象不存在，则检测ActiveX对象。如果这两种对象都不存在，就抛出-个错误。然后，就可以使用下 面的代码在所有浏览器中创建XHR对象了。 var xhr = createXHR();
由于其他浏览器中对XHR的实现与IE最曱.的实现是兼容的，W此就可以在所存浏览器屮都以相N 方式使用上面创建的xhr对象。
XHR的用法
在使用XHR对象时，要调用的第一个方法是open 〇 ,它接受3个参数：要发送的请求的类型 ("get__、"post"等)、请求的URL和表示是杏异步发送请求的布尔值。下面就是调用这个方法的例子。
xhr .open ("get", *• exar.pl e.php", false)；
这行代码会启动-个针对example .php的GET请求。有关这行代码，需要说明两点：一是URL 相对了•执行代码的当前页面（当然也可以使用绝对路径）；二是调用open ()方法并不会真正发送请求， 而只是启动一个谙求以备发送。
只能向同一个域中使用相同端口和协议的URL发送请求。如果URL与启动请求 的页面有任何差别，都会引发安全错误。
要发送特定的请求，必须像下面这样调用send 〇方法:



xhr.open("get", "example.txt", false); xhr.send(null);
XHRExampleO l, him
21
这里的sen<a()方法接收一个参数，即要作为请求主体发送的数据。如果不需要通过请求主体发送 数据，则必须传人null,因为这个参数对有些浏览器来说是必裔的。调用sendU之后，谙求就会被分 派到服务器。
由于这次埔求是同步的，JavaScript代码会等到服务器响应之后再继续执行。在收到响应后，响应 的数据会自动填充XHR对象的属性，相关的属件简介如下。
responseText::作为响应主体被返回的文本。
responseXML:如果响应的内容类型是"next/xml"或"application/xml■，这个属性中将保 存包含籽响应数据的XML DOM文杓。
status:响应的HTTP状态。
statusText: HTTP 状态的说明。
在接收到响应后，第一步是检査status属性，以确定响应已经成功返冋。-般来说，可以将丨OTP 状态代码为200作为成功的标志。此时，responseText属性的内容已经就绪，而且在内容类型正确的 情况下，responseXML也应该能够访问了。此外，状态代码为304表示请求的资源并没有被修改，可 以直接使用浏览器中缓存的版本；3然，也意味着响应是存效的。为确保接收到适？f的响应，应该像F 面这样检査上述这两种状态代码：

574 第 21 章 Ajax 与 Comet
xhr.open{"get*4 "example.txt"# false); xhr.send(null);
if ((xhr.statue >s 200 && xhr.status < 300) I! xhr.status	304){
alert(xhr.responaeText);
)else {
alert ("Request vas unsuccessful: " +■ xhr.status);
XHRExample01.htm
根据返回的状态代码，这个例子可能会显示由服务器返回的内容，也可能会显示一条错误消息。我 们违议读#要通过检测status来决定下一步的操作，不要依赖statusText,因为后者在跨浏览器使 JIJ时不太可靠。另外，无论内容类型是什么，响应主体的内容都会保存到responseText属性中；而 对f*非XML数据而言，responseXML属性的值将为null。
(^\	有的浏览器会错误地报告204状态代码。IE中XHR的ActiveX版本会将204设
置为1223,而丨E中原生的XHR则会将204规范化为200。Opera会在取得204时报 告status的值为0。
像前面这样发送同步请求当然没有问题，但多数情况下，我们还是要发送异步请求，才能让 JavaScript继续执行而不必等待响应。此时，可以检测XHR对象的readyStateM性，该属性表示请求 /响应过程的当前活动阶段。这个展性可取的值如下。
口 0:未初始化。尚未调用〇pen(>方法。
1:启动。已经调用〇pen<)方法，但尚未调用send()方法。
2:发送。已经调用send()方法，但尚未接收到响应。
3:接收。已经接收到部分响应数据。
4:完成。已经接收到全部响应数据，而且已经可以在客户端使用了。
只要readyState属性的值由••个值变成另一个值，都会触发一次readystatechange事件。可 以利用这个事件来检测每次状态变化后readyState的值。通常，我们只对readyState值为4的阶 段感兴趣，因为这时所打数据都已经就绪。不过，必须在调用〇pen(>之前指定onreadystatechange 爭件处理程序才能确保跨浏览器兼容性。下面来看一个例子。
var xhr = createXHR(); xhr.onreadystatechange = function{){ if {xhr.readyState == 4){
if ((xhr.status >- 200 && xhr.status < 300) II xhr.status == 304) { alert{xhr.responseText)；
} else {
alert("Request was unsuccessful: " + xhr.status);
}
}
};
xhr.openCgef, "example.txt•, true)； xhr.send(null)?
XHRAsyncExample01.htm

21.1 XMLHttpRequest 对象 575
以上代码利用DOM 0级方法为XHR对象添加了事件处理程序，原因是并非所有浏览器都支持DOM 2 级方法。与其他事件处理程序不同，这里没有向onreadystatechange亊件处理程序中传递event对象； 必须通过XHR对象本身来确定K—步该怎么做。
这个例子在onreadystatechange事件处理程序中使用了 xhr对象，没有使用 对象，原因是onreadysta^echange事件处理程序的作用域问题》如果使用 this对象，在有的浏览器中会导致函數执行失敗，或者导致错误发生。因此，使用 实际的XHR对象实例变量是较为可靠的一种方式。
另外，在接收到响应之前还可以调用abort ()方法来取消异步请求，如下所示：
xhr.abort{);
调用这个方法后，XHR对象会停止触发事件，而且也不再允许访问任何与响应有关的对象属性。在 终止请求之后，还应该对XHR对象进行解引用操作。由于内存原因，不建议重用XHR对象。
HTTP头部信息
每个HTTP请求和响应都会带有相应的头部信息，其中有的对开发人员冇用，有的也没冇什么用。 XHR对象也提供了操作这两种头部（即请求头部和响应头部）信息的方法。
默认情况下，在发送XHR请求的同时，还会发送下列头部信息。
Accept:浏览器能够处理的内容类型。
Accept-Charset:浏览器能够fi示的字符集。
Accept-Encoding:浏览器能够处理的压缩编码。
口 Accept-Language:浏览器当前设置的语言。
Connection:浏览器与服务器之间连接的类型。
Cookie:肖前页面设置的任何Cookie。
Host:发出请求的页面所在的域。
 Ref erer:发出诸求的页面的URI。注意，HTTP规范将这个头部字段拼写错而为保证与规 范一致，也只能将错就错了。（这个英文单间的正确拼法成该是referrer。）
User-Agent:浏览器的用户代理字符串。
虽然不同浏览器实际发送的头部信息会冇所不同，但以上列出的基本上是所有浏览器都会发送的。 使用setRequestHeaderO方法卩丨以设置自沄义的请求头部信息。这个方法接受两个参数：头部字段 的名称和头部字段的值。要成功发送请求头部倍息，必须在调用open (>方法之沿且调用send ()方法 之前调用set_RequestHeader (> ,如下面的例子所7K。
var xhr = createXHR(); xhr.onreadystatechange = function(){ if (xhr.readyState == 4)(
if ((xhr.status >= 200 && xhr.status < 300) II xhr.status == 304) { alert(xhr.responseText)；
)else {
alert("Request was unsuccessful: ■ + xhr.status);
)
}
21

576 第 21 章 Ajax 与 Comet
>;
xhr.open("get*, "exanple.pap", true);
xhr. 8etRe<zuestHeader ("MyUeadern, "MyValue" >;
xhr.send(null);
XHRRequestHeadersExampleOl. htm
服务器在接收到这种fl定义的头部信息之后，nr以执行相应的后续操作。我们建议读者使用自定义 的头部字段名称，不要使用浏览器正常发送的字段名称，否则W可能会影响服务器的响应。有的浏览器 允许开发人员®写默认的头部倍息，似有的浏览器则不允许这样做。
调用XHR对象的getResponsef[eader()方法并传人头部字段名称，可以取得相应的响应头部信 息。而调用getAllResponseHeaders(>方法则可以取得一个包含所有头部倍息的长字符串。来看下 面的例子。
var myHeader = xhr.getResponscHcader("MyHeader")? var allHeaders xhr.getAllRespor.seHeaaorsO ；
在服务器端，也可以利用头部信息向浏览器发送额外的、结构化的数据。在没有自定义信息的情况 下，getAllResponse- HeadersO方法通常会返回如下所/K的多行文本内容：
Date： Sun, 14 Nov 2004 18:04:03 GMT Server： Apache/1.3.29 (Unix)
Vary: Accept X-Powered-By： PHP/4.3.8 Connection： close
Content-Type: text/html； charset=iso-8859-1
这种格式化的输出可以方便我们检杳响应中所有头部字段的名称，而'不必一个一个地检査某个字段
是否存在。
GET 请求
GET足最常见的清求类埋，最常用于向服务器査询某些信息。必要时，可以将查询字符串参数追加 到URL的末尾，以便将信息发送给服务器。对XHR而R,位于传人〇pen〇方法的URL末尾的査询字 符串必须经过正确的编码才行。
使用GET请求经常会发生的一个错误，就是丧询宁符串的格式冇问题。査询字符串中每个参数的名 称和值都必须使用encodeURlComponentU进行编码，然后才能放到URL的末尾；而且所有名-值对 儿都必须由和号（&)分隔，如下面的例了-所示。
xhr.open("get", ■example.php?nanicl=valuel&name2=value2*, true);
下而这个函数可以辅助向现有URL的末尾添加査询字符串参数：
function adaURTjParam<url, name, value)(
url += (url.indexOf{"?") == -1 ? ■?" ： "&-)；
url += encodeURIComponent(name) + "=■ + encodeURIComponent(value)； return url；
}
这个addURLParaiM)函数接受三个参数：要添加参数的URL、参数的名称和参数的值。这个函数 苒先检査URL是卉包含问号（以确定是否已经有参数存在)。如果没有，就添加-个问号；否则，就添 加一个和纾。然后，将参数名称和值进行编码，#添加到URL的末尾。最后返回添加参数之后的URL。

21.1 XMLHttpRequest 对象 577
下面是使用这个函数来构建请求URL的示例。
var url = "example.php";
//添加参数
url = addURLParamfurl, "name-, "Nicholas")；
url = addURLParam(url, "book"# "Professional JavaScript-);
//初始化请求
xhr.open<"get•, url, false);
在这里使用addURLParamn函数可以确保査询字符串的格式良好，并可靠地用于XHR对象。 21.1.4 POST 请求
使用频率仅次于GET的是POST请求，通常用于向服务器发送应该被保存的数据。POST请求应该 把数据作为请求的主体提交，而GET请求传统上不是这样。POST请求的主体可以包含非常多的数据， 而且格式不限。在open()方法第一个参数的位置传人"post”，就可以初始化一个POST请求，如下面 的例子所示。
xhr.open("post*, "example.php", true)；
发送POST请求的第二步就是向sendU方法中传人某些数据。由于XHR最初的设计主要是为了处 理XML,因此可以在此传人XMLDOM文档，传人的文档经序列化之后将作为请求主体被提交到服务 器。当然，也可以在此传人任何想发送到服务器的字符串。
默认情况下，服务器对POST请求和提交Web表单的请求并不会一视同仁。因此，服务器端必须有 程序来读取发送过来的原始数据，并从中解析出有用的部分。不过，我们可以使用XHR来模仿表单提 交：首先将Content-Type头部信息设置为application/x-www-form-urlencoded,也就是表单 提交时的内容类型，其次是以适当的格式创建…个字符串。第14章曾经讨论过，POST数据的格式与査 询字符串格式相同。如果需要将页面中表单的数据进行序列化，然后冉通过XHR发送到服务器，那么 就可以使用第14章介绍的serialized函数来创建这个字符串：
function submiLData(){
var xhr = createXHRO? xhr.onreadystatechange = function{){ if (xhr.readyState == 4){
if {(xhr.status >=200 && xhr.status < 300) I| xhr.status == 304){ alert(xhr.responseText);
} else {
alert("Request was unsuccessful: ” + xhr.status)；
>;
xhr. open ("post”, "p〇8texastple«phpM, true)；
xlir ,setRe<zueatHeader ("Contont-Typ«" # Napplication/x-www-form-urXencodedn); var form » docuznont.g«tElementByXd( "user-info")； xhr•send(serialize(form));
XHRPostExampleOI.htm

578 第 21 章 Ajax 与 Comet
这个闲数可以将ID为wuser-info”的表单中的数据序列化之后发送给服务器。而下面的本例PHP 文件postexample .php就可以通过$_POST取得提交的数据广：
<?php
header("Content-Type: text/plain")； echo <«EOF
Name：
Email ：
{5.P0ST[ *user-name* ] >
{S_POST[ 'user-email* 】}
EOF;



如果不设置Content-Type头部信息，那么发送给服务器的数据就不会出现在$_POST超级全周变 M中。这时候，要访问同样的数据，就必须借助$HTTP_RAW_POST_DATA。
(^\ 与GET请求相比，POST请求消耗的资源会更多一些。从性能角度来看，以发送 相同的数据计，GET请求的速度最多可达到POST请求的两倍。
XMLHttpRequest2 级
鉴于XHR已经得到广泛接受，成为了亊实标准，W3C也着手制定相应的标准以规范其行为。 XMLHttpRcquest 1级只是把已有的XHR对象的实现细节描述了出来。而XMLHttpRequcst2级则进一步 发展了 XHR。并非所有浏览器都完整地实现了 XMLHttpReqUeSt2级规范，但所有浏览器都实现了它规 定的部分内容。
21.2.1 FormData
现代Web应用中频繁使用的一项功能就是表单数据的序列化，XMLHttpRequest 2级为此定义了 FormData类型。FormData为序列化表单以及创建与表单格式相同的数据（用于通过XHR传输）提供 了便利。下面的代码创建了一个FormData对象，并向其中添加了一些数据。
var data = new FormData()； data.append(_name■, "Nicholas");
这个append (>方法接收两个参数：键和值，分别对应表单字段的名字和字段中包含的值。可以像 这样添加任意多个键值对儿。而通过向FormData构造函数中传人表单元素，也可以用表单元素的数据 预先向其中填人键值对儿：
var data = new FormData(document.forms(0])；
创建r FormData的实例后，可以将它直接传给XHR的send <>方法，如下所示：
var xhr = createXHRO ; xhr.onrcadystatechange = function(}{ if (xhr.readyState -r 4)(
if ((xhr.status >= 200 && xhr.status < 300) I I xhr.status == 304){ alert(xhr.responseText)；

21.2 XMUIttpRequest2 级 579
} else {
alert("Request was unsuccessful： ™ 十 xhr.status};
}
xhr .open( "post", "postexair.plc.php", true);
var form s docrument >getBlementByld( "user*Info");
xhr. send (new ForznData(£orm});
XHRFormDataExampleOl. htm
使用FormData的方便之处体现在不必明确地在XHR对象上设置请求头部。XHR对象能够识别传 入的数据类型是FormData的实例，并配置适当的头部信息。
支持 FormData 的浏览雜冇 Firefox 4+、Safari 5+、Chrome 和 Android 3+版 WebKit。
21.2.2超时设定
IE8为XHR对象添加了 + •个timeout属性，表示清求在等待响应多少毫秒之后就终止。在给 timeout设置一个数值后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发timeout事 件，进而会阚fflontimeout事件处理程序。这项功能后来也被收人了 XMLHttpRequest2级规范中。来 看下面的例子。
var xhr = createXHRO; xhr,onreadystatechange = function(){ if {xhr.readyState == 4){ try (
if ((xhr.status >= 200 && xhr.status < 300) I I xhr.status == 304){ alert(xhr.responseText)；
} else {
alert("Request was unsuccessful: " + xhr.status)；
}
} catch (ex){
//假设由ontimeout事件处理程序处理
}
}
>;
xhr.open("get", "timeout.php", true)；
xhr.timeout ■ 1000; //将超时设置为1秒钟（仅追用于IB8+)
xhr•ontimeout ■ function(){
alert (nReqrueat did not return in a second. n);
>;
xhr.send(null)；
KHRTimeoutExampleOl.htm
这个例子示范了如何使用timeout属性。将这个属性设置为1000奄秒，意味着如果请求在丨秒钟 内还没有返回，就会自动终止。请求终止时，会调用ontimeout事件处理程序。但此时readyState 可能已经改变为4 丫，这意味狞会调用onreadystatechange事件处理程序。可是，如果在超时终止 请求之后再访问status属性，就会导致错误。为避免浏览器报告错误，可以将检査status属性的语

580 第 21 章 Ajax 与 Comet
句封装在…个try-catch语句当中。
在写作本书时，IE	然是唯一支持超时设定的浏览器。
overrideMimeType (> 方法
Firefoxii早引人了 overrideMimeType()方法，用丁•重HXHR响应的MIME类型。这个方法后
来也被纳人/XMLHttpRequest2级规范。R为返回响应的MIME类型决定了 XHR对象如何处理它，所
以提供…种方法能够重写服务器返回的MIME类型是很有用的。
比如，服务器返回的MIME类型是text/plaiia,但数据中实际包含的是XML。根据MIME类型， 即使数据是XML, responseXML属性中仍然是null。通过调用overrideMimeType ()方法，可以保 证把响应出作XML而非纯文本来处理。
var xhr = createXHRO? xhr.open("get", "text.php", true)j xhr.overrideMimeType("text/xmln)；
xhr.send(null)；
这个例了‘强迫XHR对象将响应当作XML而非纯文本来处理。调用overrideMimeType <)必须在 send()方法之前，才能保证重写响应的MIME类型。
支持 overrideMiir.eType ()方法的浏览器有 Firefox、Safari 4+、Opera 10.5 和 Chrome。
21.3进度事件
Progress Events规范娃W3C的一个I:作草案，定义了与客户端服务器通信有关的事件。这些事件最 早其实只针对XHR操作，但目前也被并他API借鉴。有以下6个进度事件。
loadstart:在接收到响应数据的第一个字节时触发。
progress:在接收响应期间持续不断地触发
error:在请求发生错误时触发。
abort:在因为调用abort <)方法而终止连接时触发。
load:在接收到完整的响应数据时触发。
!Loadend:在通信完成或者触发error、abort或load事件后触发。
每个请求都从触发loadstart事件开始，接下来是一或多个progress事件，然后触发error、 abort或load亊件中的一个，最后以触发loadend事件结束。
支持前 5 个事件的浏览器有 Firefox 3.5+、Safari 4+、Chrome、iOS 版 Safari 和 Android 版 WebKit。 Opera (从第11版开始）、IE8+只支持l〇ad事件。目前还没有浏览器支持loadend事件。
这些:书件大都很K观，似其中两个事件釘一些细节需要注意。
load 事件
Firefox在实现xhr对象的某个版本时，曾致力于简化异步交互模遨。最终，Firefox实现中引人了 load事件，用以替代readystatechange事件。响府接收完毕后将触发load事件，因此也就没有必 要去检査readyState属性了。而onload事件处理程序会接收到—个event对象，其target属性 就指向XHR对象实例，因而可以访问到XHR对象的所有方法和属性。然而，并非所有浏览器都为这个

21.3进度事件	581
事件实现了适当的事件对象。结果，开发人员还是要像下面这样被迫使用xhr对象变M。



var xhr = createXHRO ；
xhr.onload « £unction(){
if <(xhr.Btatus >« 200 && xhr.status < 300) II xhr.status sc 304){
alert{xhr.reaponseText);
)else {
alert(^Request was unsuccessful： _ ♦ xhr.status);
)
);
xhr.open("get"# _altevents.php", true)? xhr.send(null);
XHRProgressEventExampleOL htm
只要浏览器接收到服务器的响应，不管其状态如何，都会触发load事件。而这意味着你必须要检 査status属性，才能确定数据是否真的已经可用了。Firefox、Opera、Chrome和Safari都支持load 事件。
progress 事件
Mozilla对XHR的另一个革新是添加了 progress事件，这个事件会在浏览器接收新数据期间周期 性地触发。而onprogress事件处理程序会接收到一个event对象，其target屈性是XHR对象，但 包含着三个额外的属性：lengthComputable、position 和 totalSize。其中，lengthComputable 是一个表示进度信息是否可用的布尔值，position表示已经接收的字节数，totalSize表示根据 Content-Length响应头部确定的预期字节数。有了这些信息，我们就可以为用户创建-个进度指示器 了。下面展示了为用户创建进度指示器的-个示例^
var xhr = createXHR(); xhr.onload = function(event){
if ((xhr.status >= 200 && xhr.status < 300) II xhr.status == 304){ alert(xhr.responseText);
)else {
alert(^Request was unsuccessful: • + xhr.status)；
xhr.onprogress 重 function(evexxt>{
▼ar divStatus * document.getBlementByldCstatua"); i£ (event.lengthComputalsle) {
divStatus.innerHTML » *'Received " * event.position N of N 4 event.totalSize ♦" byt«sn;
)
>;
xhr.open(■get•, "altevents.php", true); xhr.send(null);
XHRProgressEventExampleOL htm
为确保正常执行，必须在调用open()方法之前添加onprogress事件处理程序D在前面的例子中,

582 第 21 章 Ajax 与 Comet
每次触发progress事件，都会以新的状态倌息更新HTML元素的内容。如果响应头部中包含 Content-Length字段，那么也吋以利用此倍息来计算从响应中已经接收到的数据的CT分比。
21.4跨源资源共享
通过XHR实现Ajax通信的一个主要限制，来源于跨域安全策略。默认情况下，XHR对象只能访 问与包含它的页面位于同一个域中的资源。这种安全策略可以预防某些恶意行为。但是，实现合理的跨 域请求对开发某些浏览器应用程序也是至关重要的。
CORS( Cross-Origin Resource Sharing,跨源资源共享）是W3C的•个工作草案，定义了在必须访 问跨源资源时，浏览器与服务器应该如何沟通。CORS背后的基本思想，就是使用自定义的HTTP头部 让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。
比如一个简单的使用GET或POST发送的请求，它没冇C1定义的头部，而主体内容是text/plain。在 发送该请求时.X要给它附加一个额外的Origin头部，其中包含请求页面的源信息（协议、域名和端 U),以便服务器根据这个头部倍息来决定是否给予响应。下面是Origin头部的一个示例：
Origin： http：//

www.nczonline.net
如果服务器认为这个请求可以接受，就在Access-Control-Allow-Origin头部中回发相同的源 信息（如果是公共资源，可以间发。例如：
Access-Control-Allow-Origin： 

http://www.nczonline.net:
如果没有这个头部，或者有这个头部但源位息不匹配，浏览器就会驳w请求。正常情况下，浏览器 会处理请求。注意，请求和响应都不包含cookie信息。
旧对CORS的实现
微软在IE8中引人r XDR ( XDomainReguest )类铟。这个对象与XHR类似，但能实现安全可靠 的跨域通信。XDR对象的安全机制部分实现了 W3C的CORS规范。以下是XDR与XHR的一些不同之 处。
- [ ]  cookie不会随请求发送，也+会随响应返回。
口只能设置请求头部信息屮的Content-Type字段。
- [ ] 不能访问响应头部信息。
口只支持GET和POST请求。
这些变化使 CSRF (Cross-Site Request Forgery,跨站点请求伪造）和 XSS (Cross-Site Scripting,跨 站点脚本）的问题得到了缓解。被请求的资源可以根据它认为合适的任意数据（用户代理、来源页面等） 来决定是否设置Access-Control - Allow-Origin头部。作为请求的一部分，Origin头部的值表示 请求的来源域，以便远程资源明确地识别XDR请求。
XDR对象的使用方法与XHR对象非常相似。也是创建一个XDomair.Request的实例，调用open (> 方法，冉调用send (>方法。但与XHR对象的open ()方法不同，XDR对象的open ()方法只接收两个 参数：请求的类型和URL。
所有XDR请求都是异步执行的，不能用它来创建同步请求。请求返冋之后，会触发load事件， 响应的数据也会保存在responseText属性屮，如下所示。

21.4跨源资源共享 583



var xdr = new XDomainRequest{)?
xdr.onload = function(){
alert(xdr.responseText)?
};
xdr.open("get", -http: "

www.somewhere-else.com/page/->; xdr.send(null)；
XDomainRequestExampleOL htm
在接收到响应后，你只能访问响应的原始文本；没有办法确定响应的状态代码。而且，只要响应有 效就会触发load事件，如果失败（包括响应中缺少Access-Control-Allow-Origin头部）就会触 发error事件。遗憾的是，除了错误本身之外，没有其他信息可用，W此唯一能够确定的就只有请求 未成功了。要检测错误，可以像下而这样指定一个onerror事件处理程序。
var xdr » new XDomainRequest{); xdr.onload = function。{
alert(xdr.responseText);
};
xdr.onerror » function(){
alert("An error occurred♦);
);
xdr. open ("get", "http： //www.somewhere'-else.com/page/ *); xdr.send(null)；
XDomainRequestExampleOl. htm
鉴于导致XDR请求失敗的因素很多，因此建议你不要忘记通过onerror事件处 理程序来捕获该事件；否则，即使请求失敗也不会有任何提示。
在请求返回前调用abort ()方法可以终止请求:
xdr. abort (); //终止请求
与XHR—样，XDR对象也支持timeout属性以及ontimeout事件处理程序。下面是一个例子。
var xdr = new XDomainRequest{)； xdr.onload = function(){
alert(xdr,responseText}；
);
xdr.onerror = function(){
alert("An error occurred.");
}；
xdr.timeout ■ 1000; xdr.ontlmeout = £unction(){
alert("Request took too long.M);
);
xdr .open ("get", _

http://www.somewhere-else.coin/page/"); xdr.send(null)；
这个例子会在运行丨秒钟后超时，并随即调用ont imeout事件处理程序。
为支持POST请求，XDR对象提供了 contentType属性，用來表示发送数据的格式，如下面的例 子所示。
21

584 第 21 章 Ajax 与 Comet
var xdr = new XOcnainRequest(); xdr.onload = function{){
alert(xdr.responseText)；
>;
xdr.onerror * function(){
alert("An error occurred.");
>;
xdr.open{"post", "http：//

www.somewhere-else.com/page/")； xdr .content'IVpe = "application/x-www-form-urlencoded"； xdr. send (,'nair.el=valuel&r.aine2=value2")；
这个属性是通过XDR对象影响头部信息的唯一方式。
21.4.2其他浏览器对CORS的实现
Firefox3.5+、Safari4+、Chrome, iOS版 Safari 和 Android平台中的 WebKit都通过 XMLHttpRequest 对象实现了对CORS的原生支持。在尝试打开不间来源的资源时，无需额外编写代码就可以触发这个行 为。要请求位于另••个域中的资源，使用标准的XHR对象并在open ()方法中传人绝对URL即可，例如:
var xhr = createXHRO； xhr.onreadystatechange = function(){ if (xhr.readyState -= 4){
if ((xhr,status >- 200 && xhr.status < 300) I I xhr.status == 304){ alert(xhr.rcsponseText);
> else {
alert ("Request was unsuccessful： •• + xhr. status);
xhr♦open{"get", nhttp;//

www.eomewhere-else.com/page/M, true);
xhr.send(null)?
与IE巾的XDR对象不同，通过跨域XHR对象nj以访问status和statusText属性，而且还支 持同步清求。跨域XHR对象也有一*限制，但为了安全这些限制是必需的。以下就是这些限制。
- [ ] 不能使用setRequestHeader()设置自定义头部。
- [ ] 不能发送和接收cookie。
口调用getAllResponseHeaders (>方法总会返回空字符串。
由了-无论同源请求还是跨源请求都使用相同的接口，因此对于本地资源，_使用相对URL,在访 问远程资源时冉使用绝对URL。这样做能消除歧义，避免出现限制访问头部或本地cookie信息等问题。
Preflighted Reqeusts
CORS通过一种叫做Preflighted Requests的透明服务器验证机制支持开发人员使用自定义的头部、 GET或POST之外的方法，以及不冏类型.的主体内容。在使用下列高级选项来发送请求时，就会向服务 器发送一个Preflight请求。这种请求使用OPT丨ONS方法，发送下列头部。
Origin:与简单的请求相同。
Access-Control-Request-Method:请求自身使用的方法。
口 Access-Control-Request-Headers:(可选）ft定义的头部信息，多个头部以逗号分隔。 以下是一个带耵自定义头部NCZ的使用POST方法发送的请求。

21.4跨源资源共享	585
Origin: 

http://vAtfw.nczonline.net Access-Control-Request-Method： POST Access-Control-Request-Headers： NCZ
发送这个请求后，服务器可以决定是否允许这种类型的请求。服务器通过在响应中发送如下头部与
浏览器进行沟通。
口 Access-Control-Allow-Origin:与简单的请求相同。
Access-Control-Allow-Methods:允许的方法，多个方法以逗号分隔。
Access-Control-Allow-Headers:允许的头部，多个头部以逗号分隔。
Access-Control-Max-Age:应该将这个Preflight请求缓存多长时间（以秒表。
例如：
Access-Control-Allow-Origin： 

http://www.nc20nline.net;
Access-Control-Allow-Methods: POST, GET Access-Control-Allow-Headers: NCZ Access-Control-Max-Age： 1728000
Preflight请求结束后，结果将按照响应中指定的时间缓存起来。而为此付出的代价只是第一次发送 这种请求时会多一次HTTP请求。
支持Preflight请求的浏览器包括Firefox 3.5+、Safari 4+和Chrome。IE 10及更早版本都不支持。
21.4.4带凭据的请求
默认情况下，跨源请求不提供凭据（cookie、HTTP认证及客户端SSL证明等）。通过将 withCredentials厲性设置为true,可以指定某个请求应该发送凭据。如果服务器接受带凭据的请 求，会用下面的HTTP头部来响应。
Access-Control-Allow-Credentials： true
如果发送的是带凭据的请求，但服务器的响应中没宥包含这个头部，那么浏览器就不会把响应交给 JavaScript (于是，responseText中将是空字符串，status的值为0，而且会调用onerror(>事件处 理程序)。另外，服务器还可以在Preflight响应中发送这个HTTP头部，表示允许源发送带凭据的请求。
支持^化^6扣加1313属性的浏览器:^^^^<^3.5+、8&丨3114+和(^111〇1116。1£10及更早版本都不
支持。
21.4.5跨浏览器的CORS
即使浏览器对CORS的支持程度并不都-•样，但所有浏览器都支持简单的（非Preflight和不带凭据 的）请求，丙此有必要实现一个跨浏览器的方案。检测XHR是否支持CORS的最简单方式，就是检査
足否存在withCredentials属件。再结合检测XDomainRequest对象是否存在，就可以兼顾所冇浏 览器了。
function createCORSRequest(method, url){ var xhr = new XMLHttpRequest〇； if ("withCredentials" in xhr){ xhr.open(method, url, true)；
} else if (typeof XDomainRequest !- "undefined"){ ’ vxhr = new XDomainRequest()； xhr.open(method, url)；

586 第 21 章 Ajax 与 Comet
} else {
xhr = null；
}
return xhr；
}
var request = croateCORSRequest("get", "htcp://

www.somewhere-else.com/page/"); if (request){
request .onload = fur.ct icn (} {
//对 reguest • responseText 进行处理
};
request.send()?
}
CrossBrowserCORSRequestExample01.htm
Firefox、Safari 和 Chrome 屮的 XMT.HttpRequest■对象与 IE 中的 XDomainReqiiest 对象类似，都 提供了够用的接u,因此以上模式还是相当有用的。这两个对象共同的属性/方法如下。
abort U :用于停止正在进行的请求。
onerror:用’f•特代 onreadystatechange 检测错误。
onload:用于替代 onreadystatechango 检测成功。
responseText:用'丁-取得响应内容。
Send().:用f•发送请求。
以h成员都包含在createCORSRequest (>闲数返间的对象中，在所有浏览器中都能正常使用。
21.5其他跨域技术
在CORS出现以前，要实现跨域Ajax通信颇费一邺周折。开发人员想出了一些办法，利用DOM中 能够执行跨域请求的功能，在+依赖XHR对象的情况下也能发送某种请求。虽然CORS技术已经无处 不在，但幵发人员己发明的这些技术仍然被广泛使用，毕竞这样不需要修改服务器端代码。
图像 Ping
h述第一种跨域请求技术是使用<^9>标签。我们知道，一个网贞可以从仟何网页中加载图像，不 用枳心跨域不跨域。这也是在线广告跟踪浏览M的主要方式^正如第13章讨论过的，也可以动态地创 违阁像，使用它们的onload和onerror书件处理程序来确定娃否接收到丫响应。
动态创建图像经常用于图像Ping。图像Ping是与服务器进行简单、单向的跨域通信的一种方式。 请求的数据足通过查询字符审形式发送的，而响应吋以是任意内容，但通常是像素图或204响应。通过 罔像Ping,浏览器得不到任何具体的数据，但通过侦听load和error事件，它能知道响应是什么时 候接收到的。来看下面的例
var img = new ImageO; f img.onload = isr.g.onerror - function () { alert("Done!");
>;
img.src = "

http://www.exa-Ttple.com/test7namesNicholas"；
ImagePingExampleOl .htm

21.5其他跨域技术	587
这电创建了-个Image的实例，然后将onload和onerror事件处理程序指走为同-个闲数。这 样无论是什么响应，只要请求完成，就能得到通知。请求从设3 src属性那一刻开始，而这个例f在请 求中发送了一^I' name参数。
图像PingM常用于跟踪用户点df贞面或动态广告曝光次数。图像Ping介两个主要的缺点，一M只 能发送GET谙求，二是无法访问服务器的响应文本。因此，阁像Ping只能用f浏览器勾服务器间的单 向通信。
JSONP
JSONP是JSON with padding (填充式JSON或参数式JSON )的简2/,是应用JSON的一种新方法， 在后来的Web服务屮非常流行。JSONP养起来与JSON差不多，只不过是被包含在函数调用中的JSON, 就像下面这样。
callback({ "name"： "Nicholas" })；
JSONP由两部分组成：回调阐数和数据。回调函数是当响应到来时应该在页面中调用的闲数。回调 函数的名字一般是在请求中指定的。而数据就是传入间调函数中的JSON数据。下面是-••个典坻的JSONP 请求。
http：//freegeoip.net/j son/?callback=handleResponse
这个URL是在请求一个JSONP地理定位服务。通过査询字符串来指定JSONP服务的回调参数是很 常见的，就像卜.面的URL所示，这里指定的【P丨调函数的名字叫handleResponseU。
JSONP是通过动态<Script>元索（要广解详细信息，请参考第13章）来使用的，使用时可以为 src属性指定一个跨域URL。这里的<SCriPt>^索与<^9>元素类似，都朽能力不受限制地从其他域 加载资源。W为JSONP楚有效的JavaScript代码，所以在请求完成后，即在JSONP响应加载到页面中 以后，就会立即执行。来宥一个例子。



function handleResponse(response){
alert ("You* re at IP address " response, ip + ■, which is in " + response.city + ", " + response.region_name);
var script = doci^nent.createEXement("script")；
script.src = "http：//freegeoip.net/json/?callback：handLeResponse'? document.body.insertBefore(scripc, document.body.firstChild)?
JSONPExampleOLhtm
这个例子通过杳询地理定位服务来狃不•你的ip地址和位置信总。
JSONP之所以在开发人员中极为流行，主要原W是它非常简单M用。与图像Ping相比，它的优点 在于能够直接访问响应文本，支持在浏览器与服务器之间双向通倍。不过，JSONP也有两点不足。
首先，JSONP是从其他域中加载代码执行。如果艿他域不安全，很可能会在响应中夹带一些恶怠代 码，而此时除了完全放弃_rSONP调用之外，没有办法追究。W此在使用不是你(^己运维的Web服务时, 一定得保证它安全可靠。
其次，要确定JSONP诸求足否失败并不容易。虽然町1^5给<3£：:^?1:>元素新增了-个 onerror 亊件处理程序，但目前还没有得到任何浏览器支持。为此，开发人员不得不使用计时器检测指定时间内

588 第 21 章 Ajax 与 Comet
是否接收到了响应。佝就弈这样也不能尽如人意，毕竞不是每个用户上网的速度和带宽都一样。
Comet
Comet是Alex Russell®发明的-个词儿，指的是一种更高级的Ajax技术（经常也有人称为“服务器 推送”)。Ajax是一种从页面向服务器请求数据的技术，而Comet则是一种服务器向页面推送数据的技 术。Cornet能够让信息近7•实时地被推送到页面上，非常适合处理体t比赛的分数和股票报价。
有两种实现Comel的方式：长轮询和流。长轮询是传统轮询（也称为短轮询）的一个翻版，即浏览 器定时向服务器发送请求，看有没有更新的数据。阁21-1展示的是短轮询的时间线。



有数据可发送。发送完数据之后，浏览器关闭连接，随即又发起一个到服务器的新请求。这一过程在页 面打开期间一直持续不断。阄21-2展示了长轮询的时间线。



阁 21-2
无论娃短轮询还是长轮询，浏览器都要在接收数据之前，先发起对服务器的连接。两者最大的区别 在于服务器如何发送数据。短轮询足服务器立即发送响应，无论数据是否有效，而长轮询是等待发送响 应。轮询的优势是所冇浏览器都支持，W为使用XHR对象和SetTime〇ut()就能实现。而你要做的就 是决定什么时候发送谛求。
第二种流行的Comet实现是HTTP流。流不同丁•上述两种轮洵，芮为它在页面的整个牛命周期内只 使用一个HTTP连接。具体来说，就是浏览器向服务器发送一个淸求，而服务器保持连接打开，然后周 期性地向浏览器发送数据。比如，下而这段PHP脚本就是采用流实现的服务器中常见的形式。
<?php
$i = 0; while(true){
①Alex Russc〗]姓著名JavaScript框架Dojo的创始人,

21.5其他跨域技术	589
//输出一些数据，然后立即刷新输出缓存 echo "Number is $i *; flush()；
//等几秒钟
sleep(10);
$i + + ;
}
所有服务器端语言都支持打印到输出缓存然后刷新（将输出缓存中的内容一次性全部发送到客户 端）的功能。而这正是实现HTTP流的关键所在。
在 Firefox、Safari、Opera 和 Chrome 中，通过侦听 readystatechange 事件及检测 readyState 的值是否为3,就可以利用XHR对象实现HTTP流。在上述这些浏览器中，随着不断从服务器接收数 据，readyState的值会周期性地变为3。.当readyState值变为3时，responseText属性中就会保 存接收到的所有数据。此时，就需要比较此前接收到的数据，决定从什么位置开始取得最新的数据。使 用XHR对象实现HTTP流的典型代码如下所示。
fur.ction createStreamingClient (url, progress, finished) {
var xhr = new XMLHttpRequesc(), received = 0；
xhr.open("get*, url, true); xhr.onreadystatechange = function(){ var result;
if (xhr.readyState == 3){
//只取得最新数据并谓整计数器
result s xhr.responseText.substring(received); received += result.length?
//鋼用progress ®调函数 progress(result);
} else if (xhr.readyState == 4){ finished(xhr.responseText);
}
};
xhr.send(null)? return xhr；
}
var client = createStreamingClient(-streaming.php", function(data){ alert{"Received： " + data);
} / function (<2ata) { alert("Done!");
})；
HTTPStreamingExample01.htm
这个createStreamingClient O函数接收三个参数：要连接的URL、在接收到数据时调用的函 数以及关闭连接时调用的闲数。冇时候，当连接关闭时，很可能还®要重新建立，所以关注连接什么时

590 第 21 章 Ajax 与 Comet
候关闭还娃打必耍的。
只要reacJystatechange书件发生，而.丨i readyScate值为3，就对responseText进行分割以 取得M新数据=> 这见的received变fi用T-记录已经处理了多少个字符，毎次readySLate值为3时都 递增。然后，通过progress回调函数来处理传人的新数据。而当readyState值为4时，则执行 finished冋调闲数，传人响应返丨!!丨的全部内容。
M然这个例/■•比较简甲.，而且也能在大多数浏览器中正常运行（IE除外），倂管理Comet的连接是 很容易出错的，要时间不断改进才能达到完美。浏览器社区认为Comet是未来Web的-个重要组成 部分，为了简化这一技术，又为Comet创逑了两个新的接U。
21.5.4服务器发送事件
SSE ( Server-Sent Events,服务器发送事件）是围绕只读Comet交推出的API或者模式。SSE API 用r创建到服务器的单向连接，服务器通过这个连接可以发送任意数最的数据。服务器响应的MIME 炎喂必须足texc/event-stream, Ifr丨且足浏览器中的JavaScript API能解析格式输出。SSE支持短轮 询、长轮询和HTTP流，而且能在断开连接时自动确定何时東新连接。有了这么简单实用的API,再实 现Comet就容易多丫。
支持 SSE 的浏览器有 Firefox 6+、Safari 5•卜、Opera 11+、Chrome 和 iOS 4+版 Safari。
1. SSE API
SSH的JavaScript API与其他传递消息的JavaScript API很相似。要预订新的事件流，首先要创建一 个新的EventSource对象，并传进一个人U点：
var source = now EventSource("myevents.php");
注意，传人的URL必须与创逑对象的贞面同源（相同的URL模式、域及端口）。EventSource的 实例有•个，值为0表示正连接到服务器，值为1表示打开了连接，值为2表示关闭 了连接。
另外，还有以下三个事件。
open:在建立连接时触发。
message:在从服务器接收到新事件时触发。
e^ror :在无法建立连接时触发。
就一般的用法而言，oranessago亊件处理程序也没有什么特別的。
source.onmessage = function{event)( var data = event.data；
//处理数据
};
服务器发冋的数据以字符串形式保存在event .data中。
默认怡况下，EventSource对象会保持与服务器的活动连接。如果连接断开，还会重新连接。这 就意味斿SSE适合长轮洵和HTTP流。如果想强制立即断开连接并且不再重新连接，可以调用ci〇se () 方法。
source.close{) ?
事件流
所谓的服务器事件会通过一个持久的HTTP响应发送，这个响应的MIME类型为text/event-

21.5其他跨域技术	591
stream。响应的格式是纯文本，最简单的怡况是每个数据项都带有前缀data:，例如：
data： foo
data： bar
data： foo
data： bar
对以上响应而事件流中的第一个message事件返回的event.data值为"foo",第二个 message事件返回的event.data值为"bar",第三个message事件返M的event «data值为 ”fcxAnbar-(注意中间的换行符)。对于多个连续的以data:开头的数据行，将作为多段数据解析， 每个值之间以一个换行符分隔。只有在包含data:的数据行后面有空行时，才会触发message事件， 因此在服务器上生成事件流时不能忘了多添加这一行。
通过id:前缀可以给特定的事件指定一个关联的ID,这个丨D行位于data:行前而或后面泞可：
data: foo
id: 1
■设置了 ID后，EventSource对象会跟踪上一次触发的亊件。如果连接断开，会向服务器发送一个 包含名为Last-Event-ID的特殊HTTP头部的请求，’以便服务器知道下一次该触发哪个事件。在多次 连接的事件流中，这种机制可以确保浏览器以正确的顺序收到连接的数据段。
Web Sockets
要说最令人津津乐道的新浏览器API,就得数Web Sockets 了。Web Sockets的目标是在一个单独的 持久连接上提供全双T、双向通信。在JavaScript中创建了 Web Socket之后，会冇一个HTTP谙求发送 到浏览器以发起连接。在取得服务器响应后，建立的连接会使用HTTP升级从HTTP协议交换为Web Socket协议。也就是说，使用标准的HTTP服务器尤法实现Web Sockets,只有支持这种协议的专门服 务器才能正常工作。
由于Web Sockets使用了自定义的协议，所以URL模式也略有不同。未加密的连接不再是http: //， 而是ws://;加密的连接也不是https://,而是wss://。在使用WebSocketURL时，必须带着这个 模式，因为将来还有可能支持其他模式。
使用自定义协议而非HTTP协议的好处是，能够在客户端和服务器之间发送非常少就的数据，而不 必担心HTTP那样字节级的开销。由T传递的数据包很小，因此Web Sockets非常适合移动应用。毕竟 对移动应用而言，带宽和网络延迟都是关键问题。使用自定义协议的缺点在于，制定协议的时间比制定 JavaScript API的时间还要长。Web Sockets曾儿度搁浅，就因为不断有人发现这个新协议存在一致性和 安全性的问题。Firefox 4和Opera丨1都曾默认启用Web Sockets,但在发布前夕乂禁用了，因为又发现 了安全隐患。H 前支持 Web Sockets 的浏览器YfFirefox6+、Safari 5+、Chrome 和 iOS 4+版 Safari。
1. Web Sockets API
要创建Web Socket,先实例一个WebSocket对象并传人要连接的URL:
var socket = new WebSockec (_ws: "

www.example.com/server ♦php” ；
注意，必须给WebSocket构造闲数传人绝对URL。同源策略对Web Sockets不适用，因此可以通 过它打开到仟何站点的连接。至于是杏会与某个域中的页面通信，则完全取决于服务器。（通过握手信 息就可以知道请求来自何方。）
实例化了 WebSocket对象后，浏览器就会马上尝试创建连接。与XHR类似，WebSocket也有一 个表示当前状态的readyState属性。不过，这个属性的值与XHR并不相同，而是如下所示。
WebSocket.OPENING (0>:正在建立连接。
WebSocket.OPEN(l):已经建立连接。
WebSocket:.CLOSING⑵：正在关闭连接。
WebSocketi.CLOSE (3):已经关闭连接。
WebSocket没有readystatechange事件；不过，它布其他事件，对应着不同的状态。readySt:ate 的值永远从0开始。
要关闭Web Socket连接，可以在任何时候调用close(>方法。 socket.closeO ;
调用了 close (>之后，readyState的值立即变为2 (正在关闭），而在关闭连接后就会变成3。
2.发送和接收数据
Web Socket打开之后，就可以通过连接发送和接收数据。要向服务器发送数据，使用send U方法 并传人任意字符串，例如：
var socket = new WebSocket("ws：//

www.example.com/server.php")； socket.send("Hello worldl");
W为Web Sockets只能通过连接发送纯文本数据，所以对于复杂的数据结构，在通过连接发送之前， 必须进行序列化。下面的例子展示了先将数据序列化为一个JSON字符串，然后再发送到服务器：
var message = {
time: new Date(), text: "Hello world!*, clientld： "asdfp8734rew"
};
socket.send{JSON.stringify(message))；
接下来，服务器要读取其中的数据，就要解析接收到的JSON字符串。
当服务器向客户端发来消息时，WebSocket对象就会触发message事件。这个message事件与 艽他传递消息的协议类似，也是把返回的数据保存在event.data «性中。
socket.onmessage = function(event)( var data = event.data;
//处理数提
};
与通过sen<3<)发送到服务器的数据一样，event.data中返冋的数据也是卞•符串。如果你想得到 «他格式的数据，必须手丁_解析这些数据。
其他事件
WebSocket对象还有其他三个事件，在连接生命周期的不同阶段触发。
open:在成功建立连接时触发。
error:在发生错误时触发，连接不能持续。
close:在连接关闭时触发。
WebSocket对象不支持DOM 2级亊件侦听器，W此必须使用DOM 0级语法分别定义每个事件处理程序。
var socket = new WcbSocket {"ws://www. exair.ple.com/server .php");
socket.onopen = function()(
alerc("Connection established.”；
socket.onerror = function(){
alert("Connection error.")；
};
socket.onclose = function(H
alert("Cormection closed.-);
};
在这三个事件中，只有close亊件的event对象冇额外的倍息。这个事件的事件对象有三个额外 的属性：wasClean、code和reason。•中，wasClean是…个布尔值，表示连接是否已经明确地关 闭；code是服务器返M的数值状态码；而reason是一个字符串，包含服务器发回的消息。可以把这 共佶息显示给用户，也奵以记录到日志中以便将来分析。
socket.onclose = function(event>{
console.log("Was clean? • + event.wasClean + ” Codes* + event.code 4 ■ Reason="
+ event.reason)；
};
SSE 与 Web Sockets
面对某个異体的用例，在考虑是使用SSE还是使用WebSockets时，可以考虑如下几个因素。疗先， 你是否存II由度建立和维护Web Sockets服务器？因为Web Socket协议不MT HTTP,所以现冇服务器 不能)丨丨于Web Socket通信。SSE倒是通过常规HTTP通信，因此现有服务器就可以满足需求。
第二个要考虑的问题是到底稱不需要双向通信。如果用例只霜读取服务器数据（如比赛成绩），那 么SSE比较容易实现。如果用例必须双向通信（如聊天宰），那么Web Sockets显然更好。别忘了，在 不能选择Web Sockets的情况下，组合XHR和SSE也是能实现双向通信的。
21.6安全
讨论Ajax和Comet安全的文章nj谓连篇累牍，而相关主题的卞也已经出了很多本f。大型Ajax应 用程序的安全问题涉及面非常之广，倂我们可以从普遍意义h探讨一拽基本的问题。
首先，可以通过XHR访问的任何URL也叮以通过浏览器或服务器来访问。下面的URL就是一个 例了-〇
/getuserinfo.php?id=23
如果是向这个URL发送谙求，吋以想象结果会返til ID为23的用户的某些数据。谁也无法保证别 人不会将这个URL的用户ID修改为24、56或其他值。W此，getuserinfo.php文件必须知道请求者 是否真的办权限访问要请求的数据；否则，你的服务器就会门户大开壬何人的数据都可能被泄漏出去。
对于未被授权系统冇权访问某个资源的情况，我们称之为CSRf (Cross-Site Request Forgery,跨站 点请求伪造)。未被授权系统会伪装A己，U:处理请求的服务器认为它是合法的。受到CSRF攻击的Ajax

594 第 21 章 Ajax 与 Comet
程序有大布小，攻击行为既有旨在揭示系统滅洞的恶作剧，也有恶意的数据窃取或数据销毁。
为确保通过XHR访问的URL安全，通行的做法就是验证发送请求者是否有权限访问相应的资源。 有下列几种方式可供选择。
- [ ] 耍求以SSL连接来访问可以通过XHR请求的资源。
- [ ] 要求每-次请求都要附带经过相应算法计算得到的验证码。
请注意，下列措施对防范CSRF攻击不起作用。
- [ ] 要求发送POST而不是GET请求■ ~很容易改变。
- [ ] 检査来源URL以确定是否可信一一来源记录很容易伪造。
- [ ] 基于cookie信息进行验证一■同样很容易伪造。
XHR对象也提供了 一些安全机制，虽然表面上看可以保证安全，但实际上却相当不可靠。实际上, 前面介绍的open ()方法还能再接收两个参数：要随请求一起发送的用户名和密码。带有这两个参数的 请求可以通过SSL发送给服务器上的页面，如下面的例子所示。
xhr.open{ "get*, "example.php", true, "username", "password") ? //不要这样做！ ！
f
	即便可以考虑这种安全机制，但还是尽量不要这样做。把用户名和密码保存在
JavaScript代码中本身就是极为不安全的。任何人，只要他会使用JavaScript调试器，
就可以通过查看相应的变量发现纯文本形式的用户名和密码。
###  21.7 小结
Ajax是无需刷新页面就能够从服务器取得数据的一种方法。关于Ajax,可以从以下几方面来总结 —下。
- [ ] 负责Ajax运作的核心对象是XMLHttpRequest (XHR)对象。
- [ ]  XHR对象由微软最早在IE5中引入，用于通过JavaScript从服务器取得XML数据。
- [ ] 在此之后，Firefox、Safari、Chrome和Opera都实现了相同的特性，使XHR成为了 Web的一个 亊实标准。
- [ ] 虽然实现之间存在差异，但XHR对象的基本用法在不同浏览器间还是相对规范的，因此可以放 心地用在Web开发酋中。
同源策略是对XHR的一个+:要约束，它为通信设置了“相同的域、相同的端口、相同的协议”这一 限制。试图访问上述限制之外的资源，都会引发安全错误，除非采用被认可的跨域解决方案。这个解决 方案叫做 CORS (Cross-Origin Resource Sharing,跨源资源共享），IE8 通过 XDomainRequest对象支持 CORS.其他浏览器通过XHR对象原生支持CORS。_Ping和JSONP是另外两种跨域通信的技术， 但不如CORS稳妥。
Comet是对Ajax的进一步扩展，让服务器几乎能够实时地向客户端推送数据。实现Comet的手段 主要有两个：长轮询和HTTP流。所有浏览器都支持长轮询，而只有部分浏览器原生支持HTTP流。SSE (Server-Sent Events,服务器发送事件）是一种实现Comet交互的浏览器API,既支持长轮询，也支持 HTTP®。
Web Sockets是一种与服务器进行全双工、双向通信的信道。与其他方案不同，Web Sockets不使用 HTTP协议，而使用一种ft定义的协议。这种协议专门为快速传输小数据设计。虽然要求使用不同的 Web服务器，但却具有速度上的优势。
各方面对Ajax和Comet的鼓吹吸引了越来越多的开发人员学习JavaScript,人们对Web开发的关注 也再度升温。与Ajax有关的概念都还相对比较新，这些概念会随着时间推移继续发展。
Ajax是一个非常庞大的主题，完整地讨论这个主题超出了本书的范围。要想了解 有关Ajax的更多信息，请读者参考《Ajax高级程序设计（第2版）>。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter19.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter21.md)