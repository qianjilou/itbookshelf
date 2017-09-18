## 第2章 在 HTML 中使用 JavaScript([返回首页](https://github.com/qianjilou/javascript3))
**本章内容** 
- [ ] 使用script兀素
- [ ] 嵌人脚本与外部脚本
- [ ] 文档模式对JavaScript的影响
- [ ] 考虑禁用JavaScript的场景  

只要一提到把JavaScript放到网页中，就不得不涉及Web的核心语言一HTML。在当初开发
Z S JavaScript的时候，Netscape要解决的一个重要问题就是如何做到让JavaScript既能与HTML 页面共存，又不影响那些页面在其他浏览器中的呈现效果。经过尝试、纠错和争论，最终的决定就是 为Web增加统一的脚本支持。而Web诞生-9-期的很多做法也都保留了下来，并被正式纳人HTML规 范，中。
###  2.1 script 元素
向HTML页面中插人JavaScript的主要方法，就是使用script元素。这个元素由Netscape创造 并在Netscape Navigator 2中首先实现。后来，这个元素被加人到iE式的HTML规范中。HTML 4.01为 script定义了下列6个属性。
③asyric:可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或 等待加载其他脚本。只对外部脚本文件有效。
④charset:可选。表示通过src属性指定的代码的字符集。由于大多数浏览器会忽略它的值， 因此这个属性很少有人用。
⑤defer:可选。表示脚本可以延迟到文档完全被解析和显示之后再执彳f。只对外部脚本文件有 效。IE7及更早版本对嵌人脚本也支持这个属性。
⑥language:已废弃。原来用于表示编写代码使用的脚本语言（如JavaScript、JavaScriptl.2 或VBScript)。大多数浏览器会忽略这个属性，因此也没有必要再用了。
⑦src:可选。表示包含要执行代码的外部文件。
⑧type:可选。可以看成是language的替代属性；表示编写代码使用的脚本语言的内容类型（也 称为MIME类盤）。虽然text/javascript和text/ecmascript都已经不被推荐使用，但人 们一盘以来使用的都还是text/javascript。实际上，服务器在传送JavaScript文件时使用的 MIME类型通常足application/x-javascript,但在type中设置这个值却可能导致脚本被 忽略。另夕卜，在非IE浏览器中还可以使用以下值:application/javascript和application/
ecmascript。考虑到约定俗成和最大限度的浏览器兼容性，S前type属性的值依旧还是 text/javascript。不过，这个属性并不是必需的，如果没有指定这个属性，则其默认值仍为 text/javascript
使用script元素的方式有两种：直接在页面中嵌人JavaScript代码和包含外部JavaScript
文件。
在使用script素嵌人JavaScript代码时，只须Sscript指定type属性。然后，像下面这 样把JavaScript代码ft接放在元素内部即可：
```javascript
function sayHi(){
	alert("Hi!");
}
```
包含在script元素内部的JavaScript代码将被从h至下依次解释。就拿的面这个例子来说，解释 器会解释到一个函数的定义，然后将该定义保存在自己的环境当中。在解释器对scriPt元素内部的 所有代码求值完毕以前，页面中的其余内容都不会被浏览器加载或显示。
在使用script嵌人JavaScript代码时，记住不要在代码中的任何地方出现"</script>"字符串。 例如，浏览器在加载下面所示的代码时就会产生一个错误：
```javascript
function sayScript(){
	alart("<\/script>");
}
```
W为按照解析嵌人式代码的规则，当浏览器遇到字符串"</SCript> ”时，就会认为那是结束的 </script>标签。而通过把这个字符串分隔为两部分可以解决这个问题，例如：
<script type="text/javascript•> function sayScript(){
alert("<\/ocript > n)/
)
</script>
像这样分成两部分来写就不会造成浏览器的误解，因而也就不会导致错误了。
如果要通过索来包含外部JavaScript文件，那么src属性就是必需的。这个®性的值 是…个指向外部JavaScript文件的链接，例如：
〈script typos" text/javascript* src="example. js"x/script>
在这个例子中，外部文件example, js将被加载到当前页而中。外部文件只须包含通常要放在开始 的<3〇1-^|；>和结束的</script>之间的那些JavaScript代码即hJ。与解析嵌人式JavaScript代码一样， 在解析外部JavaScript文件（包括下载该文件）时，贞面的处理也会暂时停止。如果是在XHTML文档 中，也可以省略前面示例代码中结束的</scriPt>标签，例如：
escript type=•text/javascripc" src="example.js" />
但是，不能在HTML文档使用这种语法。原因是这种语法不符合HTML规范，而且也得不到某些 浏览器（尤其是1E)的正确解析。
按照惯例，外部JavaScript文件带有.js扩展名。但这个扩展名不是必需的，因为 浏览器不会检查包含JavaScript的文件的扩展名。这样一来，使用JSP、PHP或其他 服务器端语言动态生成JavaScript代码也就成为了可能。但是，服务器通常还是需要 看扩展名决定为响应应用哪种MIME类型。如果不使用.js扩展名，请确保服务器能 返回正确的MIME类型。
需要注意的是，带有src属性的script元素不应该在其<3£：:^9<;>和</8(：3^91:>标签之间再 包含额外的JavaScript代码。如果包含了嵌人的代码，则只会下载并执行外部脚本文件，嵌人的代码 会被忽略。
另外，通过script元索的src属性还■以包含来自外部域的JavaScript文件。这一点既使 script元素倍M强大，乂让它备受争议。在这一点上，script~<inig>元素非常相似，即它的src M性可以是指向当前HTML页面所在域之外的某个域中的URL,例如：
<script type=Mtext/javascript" src="http：//

www.somewhere.com/afile.js•></script>
这样，位于外部域中的代码也会被加载和解析，就像这些代码位于加载它们的页面中一样。利用这 一点就可以在必要时通过不同的域来提供JavaScript文件。不过，在访问肖己不能控制的服务器上的 JavaScript文件时则要多加小心。如果不幸遇到了怀有恶意的程序员，那他们随时都可能替换该文件中 的代码。因此，如果想包含來自不同域的代码，则要么你是那个域的所有者，要么那个域的所有者值得 信赖。
无论如何包含代码，只要不存在defer和async属性，浏览器都会按照script元素在页面中 出现的先后顺序对它们依次进行解析。换句话说，在第一个SCriPt元素包含的代码解析完成后，第 二个SCript&含的代码才会被解析，然后才是第=_个、第四个……
####  2.1.1 标签的位置
按照惯例，所有script元素都应该放在页面的<1^3(1>元素中，例如：
```html
<!DOCTYPE html>
<html>
<head>
<title>Example HTML Page</title>
<script type="text/javascript" src="exajnplel.js"></script>
<script type=" text/javascript" src="example2.js"></script>
</head>
<body>
</body>
</html>
```
这种做法的目的就是把所有外部文件（包括CSS文件和JavaScript文件）的引用都放在相同的地方。 可是，在文档的<head>S素中包含所有JavaScript文件，意味着必须等到全部JavaScript代码都被下载、 解析和执行完成以后，才能开始S现页面的内容（浏览器在遇到<13〇(^>标签时才开始呈现内容)。对于 那些需要很多JavaScript代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟 期间的浏览器窗口中将是-片空白。为丫避免这个问题，现代Web应用程序--般都把全部JavaScript引 用放在<body>S素中页面的内容后面，如下例所示： 
```html
<!DOCTYPE html>
<htral>
<head>
<title>Example HTML Page</title>
</head>
<body>
<!--这里放内容-->
<e^crdlpt type囂籯t/javascript’ srcBaexanqplel.js*></script>
<«cript typa«"text/javascriptM src騰"exaisple2.：j8"></8cript>
</body>
```
这样，在解析包含的JavaScript代码之前，页面的内容将完全呈现在浏览器中。而用户也会因为浏 览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了。
2.1.2延迟脚本
HTML4.01 Sscript标签定义了 defer属性。这个属性的用途是表明脚本在执行时不会影响页 面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在script元素中设置 defer属性，相当于告诉浏览器立即下载，但延迟执行。
```html
<!DOCTYPE html>
<html>
<head>
<title>Example HTML Page</title>
<flcript type*ntext/java8cript" de£er«nde£erN src^^examplel.janx/Bcript> <Bcript typea^text/javABcript- defers^defor" Brc=**«xaai>lo2.ja-></0cript>
</head>
<body>
</body>
```
在这个例子中，虽然我们把素放在/文档的<head>元素中，但其中包含的脚本将延迟 到浏览器遇到</htna>#签后再执行。HTML5规范要求脚本按照它们出现的先后顺序执行，因此第一 个延迟脚本会先于第二个延迟脚本执行，而这两个脚本会先丁• DOMContentLoaded事件(详见第13章） 执行。在现实当中，延迟脚本并不一定会按照顺序执行，也不一定会在DOMContentLoaded事件触发 前执行，因此最好只包含一个延迟脚本。
前面提到过，defer属性只适用于外部脚本文件。这一点在HTML5中已经明确规定，因此支持 HTML5的实现会忽略给嵌 +人脚本设置的defer属性。IE4-IE7还支持对嵌人脚本的defer属性，但 IE8及之后版本则完全支持HTML5规定的行为。
IE4、Firefox 3.5、Safari 5和Chrome是最早支持defer属性的浏览器。其他浏览器会忽略这个属 性，像平常一样处理脚本。为此，把延迟脚本放在页面底部仍然是最佳选择。
</htral>
在XHTML文档中，要把defer属性设置为defer=，defer*
2.1.3异步脚本
HTML5Sscript兀素定义了 async属性。这个属性与defer属性类似，都用于改变处理脚本

14 第2章在HTML中使用JavaScript
的行为。同样与defer类似，async只适用于外部脚本文件，并告诉浏览器立即下载文件。但与defer 不丨9]的是，标记为async的脚本并不保证按照指定它们的先后顺序执行。例如：
```
<!DOCTYPE html>
<html>
<head>
<title>Exaxple HTML Page</title>
</head>
<body>
</body>
```
在以上代码中，第二个脚本文件可能会在第•个脚本文件之前执行。因此，确保两者之间互不依赖 非常重要。指定async M性的H的是不让页面等待W个脚本下载和执行，从时异步加载页面其他内容。 为此，建议异步脚本不要在加载期间修改DOM。
异步脚本一定会在页面的load事件前执行，似可能会在DOMContentLoaded事件触发之前或之 后执行。支持异步脚本的浏览器有Firefox 3.6、Safari 5和Chrome。
在XHTML文挡中，要把async属性设置为async= ’ async"
2.1.4在XHTML中的用法®
可扩展超文本标记语言，即 XHTML (Extensible HyperText Markup Language),是将 HTML 作为 XML的应用而重新定义的一个标准。编写XHTML代码的规则要比编写HTML严格得多，而且直接影 响能否在嵌人JavaScript代码时使用<SCript/>标签。以下面的代码块为例，虽然它们在HTML中是苻 效的，但在XHTML屮则是无效的。
```
function compare(a/ b) { if (a < b) {
alert{"A is less than B")；
} else if (a > b) {
alert(*A is greater than Bn)；
} else {
alert("A is equal to B")；
}
}
```
在HTML中，有特殊的规则用以确定<5^3：1?<:>元素中的哪些内容可以被解析，但这些特殊的规则 存:XHTML中不适用。这里比较语句a < b巾的小于号（<)在XHTML中将被为作开始一个新标签来 解析。但是作为标签来讲，小于号后面不能跟空格，因此就会导致语法错误。
①HTML5正快速地被前端开发人员采用，建议读者在学习和开发中遵循HTML5标准，本节内容可以跳过。
避免在XHTML中出现类似语法错误的方法苻两个。一是用相应的HTML实体（&lt;)替换代码 中所有的小于号（<),替换后的代码类似如下所示：
```
function compare(a, b) {
if (a &lt; b) {
alert("A is less than B");
)else if (a > b) {
alert("A is greater than B*)?
} else {
alerc("A is equal to B*);
}
}
```
虽然这样可以让代码在XHTML中正常运行，但却导致代码不好理解了。为此，我们可以考虑采用 另一个方法。
保证让相间代码在XHTML中正常运行的第二个方法，就是用一个CData片段来包含JavaScript代 码。在XHTML (XML)中，CData片段是文档中的-个特殊K域，这个区域中可以包含不需要解析的 任意格式的文本内容。因此，在CData片段中就可以使用任意字符一小于号当然也没有问题，而且不 会导致语法错误。引人CData片段后的JavaScript代码块如下所示：
```
<)[CDATA[ function compare(a, b) { if (a < b) {
alert{*A is less than B-);
} else if {a > b) {
alert("A is greater than B")?
} else {
alert("A is equal co B-);
}
>>
```
在兼容XHTML的浏览器中，这个方法可以解决问题。但实际上，还有不少浏览器不兼容XHTML, Wlffi不支持CData片段。怎么办呢？再使用JavaScript注释将CData标记注释掉就可以了：
```
//<! [CGATA(
function compare(a# b) { if (a < b) {
alert("A is less than B")；
} else if (a > b) {
alert {**A is greater than B*)； } else {
alert("A is equal to B")?
//]}>
```
这种格式在所有现代浏览器中都可以正常使用。虽然有几分hack的味道，佴它能通过XHTML验 证，而且对XHTML之前的浏览器也会平稳退化。
16 第2章在HTML中使用JavaScript
在将英面的MIME类型指定•为'•application/xhcml+xml11的情况下会独发 XHTML模式。并不是所有浏览器都支持以这种方式提供XHTML文档0
2.1.5不推荐使用的语法
在最早引人scriPt元素的时候，该元素与传统HTML的解析规则是有冲突的。由丁-要对这个元 索应用特殊的解析规则，因此在那些不支持JavaScript的浏览器（最典型的是Mosaic )中就会导致问题。 具体来说，不支持JavaScript的浏览器会把scriptS素的内容直接输出到页面中，因而会破坏页面的 布局和外观。
Netscape与Mosaic协商并提出了一个解决方案，让不支持素的浏览器能够隐藏嵌人的 JavaScript代码。这个方案就是把JavaScript代码包含在一个HTML注释中，像下面这样：
script<1--
function sayHi〇{
alertCHi!")；
}
//--></script>
给脚本加上HTML注释后，Mosaic等浏览器就会忽略<script:vg签中的内容；而那些支持 JavaScript的浏览器在遇到这种情况时，则必须进一步确认其中是否包含霜要解析的JavaScript代码。
虽然这种注释JavaScript代码的格式得到了所有浏览器的认可，也能被正确解释，但由于所有浏览 器都已经支持JavaScript,因此也就没有必要再使用这种格式了。在XHTML模式下，因为脚本包含在 XML注释中，所以脚本会被忽略。
### 2.2  嵌入代码与外部文件
在HTML中嵌人JavaScript代码虽然没有问题，但一般认为最好的做法还是尽可能使用外部文件来 包含JavaScript代码。不过，并不存在必须使用外部文件的硬性规定，但支持使用外部文件的人多会强 调如下优点。
- [ ] 可维护性：遍及不|s] HTML页而的javaScript会造成维护问题。但把所有JavaScript文件都放在 一个文件夹中，维护起来就轻松多了。而且开发人员W此也能够在不触及HTML标记的情况下, 集中精力编辑JavaScript代码。
- [ ] 可缓存：浏览器能够根据具体的设置缓存链接的所有外部JavaScript文件。也就是说，如果有两个 页面都使用同一个文件，那么这个文件只笛下载一次。W此，最终结果就是能够加快页面加载的 速度。
- [ ] 适应未来：通过外部文件来包含JavaScript无须使用前面提到XHTML或注释hack。HTML和 XHTML包含外部文件的语法是相同的。
### 2.3  文档模式
IE5.5引人了文档模式的概念，而这个概念是通过使用文档类型（doctype)切换实现的。最初的两
种文烈模式是：混杂模式（quirks mode) ®和标准模式（standards mode )。混杂模式会让IE的行为与（包 含非标准特性的）IE5相同，而标准模式则让IE的行为更接近标准行为。虽然这两种模式主要影响CSS 内容的呈现，但在某些情况下也会影响到JavaScript的解释执行。本书将在必要时冉讨论这些因文档模 式而影响JavaScript执行的情况。
在丨E引人文档模式的概念后，其他浏览器也纷纷效仿。在此之后，1E 乂提出一种所谓的准标准模 式（almost standards mode)。这种模式下的浏览器特性有很多都是符合标准的，但也不尽然。不标准的 地方主要体现在处理阁片间隙的时候（在表格中使用图片时间题最明M )。
如果在文档开始处没有发现文档类型声明，则所有浏览器都会默认开启混杂模式。但采用混杂模式 不是什么值得推荐的做法，因为不同浏览器在这种模式下的行为差异非常大，如果不使用某拽hack技 术，跨浏览器的行为根本就没有一致性可言。
对于标准模式，可以通过使用下面任何一种文档类瑠来开启：
```
<!DOCTYPE HTML PUBLIC •-//W3C//DTD HTML 4.01//EN*
"http://www.w3,org/TR/hLml4/strict.dtd">
http://www.w3.org/TR/xhtmll/DTD/xhtmll-strict.dtd">
http://www.w3.org/TR/xhtmll/DTD/xhtmll-frameset.dtd">
```
准标准模式与标准模式非常接近，它们的差异几乎可以忽略不计。因此，当有人提到“标准模式” 时，有可能是指这两种模式中的任何一种。而且，检测文档模式（本书后面将会讨论）时也不会发现什 么不同。本书后面提到标准模式时，指的是除混杂模式之外的其他模式。
①这里quirics mode的译法源自Firefox 3.5.5中文版。
###  2.4  noscript元素
早期浏览器都面临一个特殊的问题，即当浏览器不支持JavaScript时如何让页面平稳地退化。对这 个问题的最终解决方案就是创造-•个<n〇SCript>元素，用以在不支持JavaScript的浏览器中显示替代 的内容。这个元素可以包含能够出现在文档<13〇办>中的任何HTML元素一素除外。包含 在<〇〇5<:^9^>元素中的内容只有在下列情况下才会显示出来：
- [ ] 浏览器不支持脚本；
- [ ] 浏览器支持脚本，但脚本被禁用。
符合上述任何一个条件，浏览器都会显示的内容。而在除此之外的其他情况下，浏览器不会呈现noscript中的内容。
请看下面这个简单的例子：
```
<html>
<head>
<title>Example HTML Page</title>
<script type="text/javascript• defer= "defer" src=*examplel.js"x></script>
< script type=” text/javascript* def er="defer" src="example2 . js"x/script>
</head>
<body>
<aoscript>
<p>本買面需要■支持{启用> JavaScript.
</noacript>
</body>
</html>
```
这个页面会在脚本无效的情况下向用户显示一条消息。而在启用了脚本的浏览器中，用户永远也不 会看到它——尽管它是页面的一部分。
### 2.5 小结
把JavaScript插人到HTML页面中要使用script元素。使用毕个元素可以把JavaScript嵌人到 HTML页面中，让脚本与标记混合在一起；也可以包含外部的JavaScript文件。而我们需要注意的地方有:
- [ ] 在包含外部JavaScript文件时，必须将src厲性设置为指向相应文件的URL。而这个文件既可 以是与包含它的页面位于同一个服务器上的文件，也可以是其他任何域中的文件。
- [ ] 所有SCript元素都会按照它们在页面中出现的先后顺序依次被解析。在不使用defer和 async属性的情况下，只有在解析完前面script元素中的代码之后，才会开始解析后面 cscript:;素中的代码。
- [ ] 由于浏览器会先解析完不使用defer属性的SCriPt元索中的代码，然后再解析后面的内容， 所以一般应该把script元素放在页面后，即主要内容后面，body标签前面。
- [ ] 使用defer属件可以让脚本在文档完全呈现之后再执行。延迟脚本总是按照指定它们的顺序执行。
- [ ] 使用async属性可以表示当前脚本不必等待;ft他脚本，也不必阻塞文档呈现。不能保证异步脚 本按照它们在页面中出现的顺序执行。  
另外，使用noSCript元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本 的情况下，浏览器不会M示noSCript元素中的任何内容。
---
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter1.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter3.md)