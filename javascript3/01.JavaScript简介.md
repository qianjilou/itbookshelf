#  第1章 JavaScript 简介([返回首页](https://github.com/qianjilou/javascript3))

- [第1章　JavaScript简介](chapter1.md#001)  
  - [1.1　JavaScript简史](chapter1.md#11) 
  - [1.2　JavaScript实现](chapter1.md#12) 
   - [1.2.1　ECMAScript](chapter1.md#121) 
   - [1.2.2　文档对象模型（DOM）](chapter1.md#122) 
   - [1.2.3　浏览器对象模型（BOM）](chapter1.md#123) 
  - [1.3　JavaScript版本 ](chapter1.md#13) 
  - [1.4　小结 ](chapter1.md#14) 
  
---

**本章内容**
- JavaScript历史回顾  
- JavaScript 是什么  
- JavaScript 与 ECMAScript 的关系  
- JavaScrip的不同版本  

JavaScript诞生于1995年。当时，它的主要目的是处理以前由服务器端语言（如Perl)负责的一些输入验证操作。在JavaScript问世之前，必须把表单数据发送到服务器端才能确定用户是否没有填写某个必填域，是否输人了无效的值。Netscape Navigator希望通过JavaScript来解决这个问题。
在人们普遍使用电话拔号上网的年代，能够在客户端完成一些基本的验证任务绝对是令人兴奋的。毕竞，
拨号上网的速度之慢，导致了与服务器的每一次数据交换事实上都成了对人们耐心的一次考验。  

S此以后，JavaScript逐渐成为市面上常见浏览器必备的一项特色功能。如今，JavaScript的用途早已不冉局限于简单的数据验证，而是具备了与浏览器窗口及其内容等几乎所有方面交互的能力。今天的JavaScript已经成为一门功能全面的编程语言，能够处理复杂的计箅和交互，拥有了闭包、匿名（lamda,
拉姆达）函数，甚至元编程等特性。作为Web的-个重要组成部分，JavaScript的*要性是不言而喻的，
就连手机浏览器，甚至那些专为残障人士设计的浏览器等非常规浏览器都支持它。当然，微软的例子更
为典型。虽然有自己的客户端脚本语言VBScript,但微软仍然在Internet Explorer的早期版本中加人了
d己的JavaScript实现®。  

JavaScript从一个简单的输人验证器发展成为一门强大的编程语言，完全出乎人们的意料。应该说,
它既M—门非常简单的语言，又是一门非常复杂的语言。说它简单，是因为学会使用它只需片刻功夫;
而说它复杂，是因为要真正掌握它则需要数年时间。要想全面理解和掌握JavaScript,关键在于弄清楚
它的本质、历史和局限性。
##  1.1 JavaScript 简史
在Web日益流行的同时，人们对客户端脚本语言的需求也越来越强烈。那个时候，绝大多数因特网用户都使用速度仅为28.8kbit/s的“猫”（调制解调器）上网，但网页的大小和复杂性却不断增加。为完成简单的表单验证而频繁地与服务器交换数据只会加重用户的负担。想象一下：用户填写
完一个表单，单击“提交”按钮，然后等待30秒钟，最终服务器返冋消息说有一个必填字段没有
①对IE而言，尚我们提到JavaScript时，实际上就是指IE对JavaScript ECMAScript)的实现  JScript。最甲的JScript
基于 Netscape JavaScript 1.0 开发，于 1996 年 8 月随同 Internet Exp丨orer 3.0 发布。
填好……，时走在技术革新最前沿的Netscape公司，决定着手开发一种客户端语言，用来处理这种简单的验证。  

当时就职于Netscape公巧的布兰登•艾奇（Brendan Eich)，开始着f•为计划J: 1995年2月发布的
Netscape Navigator 2开发一种名为LiveScript的脚本语言 该语吉将同时在浏览器和服务器中使用
(它在服务器h的名字叫LiveWire )。为了赶在发布H期前完成LiveScript的开发，Netscape与Sun公司
建立一个开发联盟。在Netscape Navigator 2正式发布前夕,Netscape为了搭丨:媒体热炒Java的顺风车，
临时把 LiveScript 改名为 JavaScript。  

由-丁-JavaScript 1.0获得了巨大成功，Netscape随即在NetscapeNavigator3 中又发布了 JavaScript 1.1〇
Web虽然羽翼未丰，但用户关注度却M创新在这样的背呆下，Netscape把自己定位为市场领袖瑠公
司。与此N时，微软决定向与Navigator竞争的f!家产品Internet Explorer浏览器投入更多资源。Netscape
Navigator 3发布后不久，微软就在其Internet Explorer 3中加人了名为JScript的JavaScript实现（命名为
Script是为了避开与Netscape冇关的授权问题)。以现在的眼光来看，微软1996年8月为进人Web浏览
器领域而实施的这个重大举措，是导致Netscape H后蒙羞的一个标志性事件。然而，这个重大举措同时
也标志荇JavaScript作为一门语言，其开发向前迈进了一大步。  

微软推出其JavaScript实现意味着有了 3个不同的JavaScript版木：Netscape Navigator中的
JavaScript、Internet Exp丨orcr中的Jscript和ScriptEase中的CEnvi。与C及其他编程语言不同，汽时还没
有标准规定JavaScript的语法和特性，3个不同版本并存的局面已经完全暴婼了这个问题。随着业界相
心的U益加剧，JavaScript的标准化问题被提上了议事H程。  

1997年，以;TavaScript 1.1为蓝本的建议被提交给了欧洲计算机制造商协会（ECMA，European
Computer Manufacturers Association )。该协会指定 39 号技术委员会（TC39，Technical Committee #39)
负责“标准化一种通用、跨平台、供应商中立的脚本语言的语法和语义”（
http://www.ecma
intemationaLorg/memento/TC39.htni)。TC39 由来自 Netscape、Sun、微软、Borland及其他关注脚本语言
发展的公司的程序员组成，他们经过数月的努力完成了 ECMA-262—定义一种名为ECMAScript (发
茳为“ek-ma-script”）的新胸本语言的标准。
第二年，ISO/IEC (International Organization for Standardization and International Electrotechnical
Commission, W标标准化组织和国际电I：委员会）也采用了 ECMAScript作为标准（即ISO/1EC-16262 )。
自此以后，浏览器开发商就开始致力71将ECMAScript作为各自JavaScript实现的基础，也在不问程度
上取得了成功。
##  1.2 JavaScript 实现
虽然JavaScript和ECMAScript通常都被人们用来表达
相同的含义，似JavaScrip〖的含义却比ECMA-262中规定的
耍多得多。没错，一个完整的JavaScript实现应该由下列三
个不同的部分组成（见图M )。
- [ ] 核心（ECMAScript)
- [ ] 文档对象模铟（DOM)
- [ ] 浏览器对象模塑（BOM )



图

18.2.1ECMAScript
由ECMA-262定义的ECMAScript与Web浏览器没有依赖关系。实际上，这门语言本身并不包含输 人和输出定义。ECMA-262定义的只是这门语言的基础，而在此基础之上可以构建更完善的脚本语言。 我们常见的Web浏览器只是ECMAScript实现可能的宿主环境之一。宿主环境不仅提供基本的 ECMAScript实现，同时也会提供该语言的扩展，以便语言与环境之间对接交互。而这些扩展——如 DOM,贝悧用ECMAScript的核心类型和语法提供更多更具体的功能，以便实现针对环境的操作。其他 衍主环境包括Node (—种服务端JavaScript平台）和Adobe Flash。
既然ECMA-262标准没有参照Web浏览器，那它都规定了些什么内容呢？大致说来，它规定了这 门语言的下列组成部分：
- [ ] 语法
- [ ] 类型
- [ ] 购
- [ ] 关键字
- [ ] 保留字
- [ ] 操作符
- [ ] 对象  

ECMAScript就是对实现该标准规定的各个方面内容的语言的描述。JavaScript实现了 ECMAScript, Adobe ActionSeript 同样也实现 7 ECMAScript  

**1.ECMAScript 的版本**  

ECMAScript的不同版本乂称为版次，以第x版表示（意即描述特定实现的ECMA-262规范的第jc 个版本)。ECMA-262的最近一版是第5版，发布于2009年。而ECMA-262的第1版本质上与Netscape 的JavaScript丨.1相同——只不过删除/所有针对浏览器的代码并作了-些较小的改动：ECMA-262要求 支持Unicode标准（从而支持多语言开发），而R对象也变成了平台无关的（Netscape JavaScript丨.1的对 象在不N;f•台中的实现不一样，例如Date对象)。这也是JavaScript 1.1和1.2与ECMA-262第丨版不一 致的主要原因。
ECMA-262第2版要是编辑加工的结果。这一版中内容的更新是为了与ISO/IEC-16262保持严格
一致，没有作任何新增、修改或删节处理。因此，一般不使用第2版来衡量ECMAScript实现的兼容性。
ECMA-262第3版才是对该标准第一次真正的修改。修改的内容涉及字符串处理、错误定义和数 值输出。这一版还新增了对正则表达式、新控制语句、try-catch异常处理的支持，并围绕标准的 国际化做出了一些小的修改。从各方面综合来看，第3版标志着ECMAScript成为了一门真正的编程
ECMA-262第4版对这门语言进行了一次全面的检核修订。由于JavaScript在Web上日益流行，开 发人员纷纷建议修订ECMAScript,以使其能够满足不断增长的Web开发需求。作为回应，ECMATC39 重新召集相关人员共同谋划这门语言的未来。结果，出台后的标准几乎在第3版基础上完全定义了一门 新语言。第4版不仅包含了强类型变量、新语句和新数据结构、真正的类和经典继承，还定义了与数据 交互的新方式。
与此同时，TC39下属的一个小组也提出了个名为ECMAScript 3.1的替代性建议，该建议只对这 门语言进行了较少的改进。这个小组认为第4版给这门语言带来的跨越太大了。因此，该小组建议对这
门语言进行小幅修订，能够在现有JavaScript引擎基础上实现。最终，ES3.1附屈委员会获得的支持趄过 了 TC39, ECMAS-262第4版在正式发布前被放弃。
ECMAScript 3.1成为ECMA-262第5版，并于2009年12月3日正式发布。第5版力求澄淸第3 版中已知的歧义并增添了新的功能。新功能包括原生JSON对象（用于解析和序列化JSON数据）、继 承的方法和髙级墀性定义，另外还包含一种严格模式，对ECMAScript引擎解释和执行代码进行了补充 说明。  

**2.什么是ECMAScript兼容**  

ECMA-262给出了 ECMAScript兼容的定义。要想成为ECMAScript的实现，则该实现必须做到：
□支持ECMA-262描述的所有“类型、值、对象、属性、函数以及程序句法和语义”（ECMA-262 第1页);
□支持Unicode字符标准。
此外，兼容的实现还可以进行下列扩展。
□添加ECMA-262没有描述的“更多类型、值、对象、厲性和函数”。ECMA-262所说的这些新增 特性，主要是指该标准中没有规定的新对象和对象的新属性。
□支持ECMA-262没有定义的“程序和iK则表达式语法"。（也就是说，可以修改和扩展内置的正 则表达式语法。〉
ll述要求为兼容实现的开发人员基于ECMAScript开发一门新语言提供了广阔的空间和极大的灵活 性，这也从另一个侧面说明了 ECMAScript受开发人员欢迎的原因。  

**3.Web浏览器对ECMAScript的支持**  

1996年，NetscapeNavigator3捆绑发布了 JavaScrip丨1.1。而相同的JavaScript 1.1设计规范随后作为 对新标准（ECMA-262)的建议被提交给Ecma。伴随翁JavaScript的迅速走红，Netscape豪情满怀地着 手开发JavaScript 1.2。然而，问题是Ecma当时还没有接受Netscape的建议。
Netscape Navigator 3发布后不久，微软也推出了 Internet Explorer 3。微软在的这一版中拥绑了 JScript 1.0,很多人都认为JScript 1.0与JavaScript 1.丨应该是一样的。但是，由于没有文档依据，加之不 适当的特性模仿，JScript 1.0还是很难与JavaScript 1.1相提并论。
1997 年，内置 JavaScript 1.2 的 Netscape Navigator 4发布;而到这一年年底，ECMA-262 第 1 版也 被接受并实现了标准化。结果，虽然ECMAScript被认为是基于JavaScript 1.1制定的，但;lavaScript 1.2 与ECMAScript的第1版并不兼容。
JScript的升级版是丨ntemet Explorer 4中内置的JScript 3.0 (随同微软IIS 3.0发布的JScript 2.0从来 也没有移植到浏览器中）。微软通过媒体大肆宣传JScript 3.0是世界上第一个ECMA兼容的脚本语言， 但当时的ECMA-262尚未定稿。于是，JScript3.0与JavaScript 1.2都遭遇了相同的尴尬局面一谁都没 有按照最终的ECMAScript标准来实现。
Netscape决定更新其JavaScript实现’即在Netscape Navigator 4.06中发布JavaScript丨.3,从而做到 了与ECMA-262的第•个版本完全兼容。在JavaScript 1.3中，Netscape增加了对Unicode标准的支持， 并在保留JavaScript 1.2新增特性的同时实现了所有对象的平台中立化。
在Netscape以Mozilla项目的名义开放其源代码时，预期JavaScript 1.4将随同NetscapeNavigator5 —道发布。然而，一个激进的决定，彻底M新设计Netscape代码，打乱了原有计划。后来，JavaScript 1.4 只发布了针对Netscape Enterprise Server的服务器版，而没有内置于Web浏览器中。
到了 2008 年，五大主流 Web浏览器（lE、Firefox' Safari、Chrome 和 Opera)全部做到了~ ECMA-262
兼容。IE8是第一个着手实现ECMA-262第5版的浏览器，并在IE9中提供了完整的支持。Firefox4也 紧随其后做到兼容。K表列出了 ECMAScript受主流Web浏览器支持的情况。
\*不完全兼容的实现
###  1.2.2文档对象模型（DOM)
文档对象模型（DOM, Document Object Model)是针对XML但经过扩展用于HTML的应用程序编 程接口（API, Application Programming Interface )。DOM把整个页面映射为一个多层节点结构。HTML 或XML页面中的每个组成部分都是某种类瑠的节点，这些节点又包含着不同类型的数据。看下面这个 HTML页面
```html
<html>
<head>
<title>Sample Page</title>
</head>
<body>
<p>Hello World!</p>
</body>
</html>
```
在DOM中，这个页面可以通过见图1-2所示的分层节点图表示。
通过DOM创建的这个表示文档的树形图，开发人员获得了控制页面内容和结构的主动权。借助 DOM提供的API,开发人员可以轻松自如地删除、添加、替换或修改任何节点。  

**1.为什么要使用DOM**  

在 Internet Explorer 4 和 Netscape Navigator 4 分别支持的不同形式的 DHTML ( Dynamic HTML )基 础匕，开发人员首次无需重新加载网页，就可以修改其外观和内容了。然而，DHTML在给Web技术发 展带来巨大进步的同时，也带来了巨大的问题。由于Netscape和微软在开发DHTML方面各持己见，过 去那个只编写一个HTML页面就能够在任何浏览器中运行的时代结束了。
对开发人员而言，如果想继续保持Web跨平台的天性，就必须额外多做一些工作。而人们真正担 心的是，如果不对Netscape和微软加以控制，Web开发领域就会出现技术上两强割据，浏览器互不兼容的局面。此时，负责制定Web通信标准的W3C (World Wide Web Consortium，万维网联盟）开始着 手规划DOM  

**2.DOM级别**  

DOM1级（DOM Level 1 )于1998年10月成为W3C的推荐标准。DOM1级由两个模块组成：DOM 核心（DOM Core)和DOM HTML。其中，DOM核心规定的是如何映射基于XML的文档结构，以便 简化对文档中任意部分的访问和操作。DOM HTML模块则在DOM核心的基础上加以扩展，添加了针 对HTML的对象和方法。  .

---

请读者注意，DOM并不只是针对JavaScript的，很多别的语言也都实现了 DOM。 不过，在Web浏览器中，基于ECMAScript实现的DOM的确已经成为JavaScript这 门语言的一个重要组成部分。

---

如果说DOM1级的目标主要是映射文档的结构，那么DOM2级的H标就要宽泛多了。DOM2级在 原来DOM的基础上又扩充了（ DHTML—直都支持的）鼠标和用户界面亊件、范围、遍历（迭代DOM 文杓的方法）等细分模块，而R通过对象接[I增加了对CSS (Cascading Style Sheets，层*样式表）的 支持。DOM 1级中的DOM核心模块也经过扩展开始支持XML命名空间。
DOM2级引入了下列新模块，也给出了众多新类型和新接口的定义。
- [ ] DOM视图（DOM Views):定义/跟踪不同文档（例如，应用CSS之前和之后的文柄）视图的 接n;
- [ ] DOM事件（DOMEvents):定义了事件和亊件处理的接口;
- [ ] DOM样式（DOM Style ):定义基于CSS为元素应用样式的接U ;
- [ ] DOM遍历和范围（DOMTraverealandRange):定义了遍历和操作文挡树的接口  

DOM3级则进一步扩展了 DOM,引人了以统一方式加载和保存文档的方法一在DOM加载和保 存（DOM Load and Save)模块中定义;新增了验证文档的方法——在DOM验证（DOM Validation )模块中定义。D〇M3级也对DOM核心进行了扩展，开始支持XML LO规范，涉及XML Infoset、XPath 和 XML Base。

---

在阅读DOM标准的时候，读者可能会看到DOMO级（DOMLevelO)的字眼。
实际上，DOMO级标准是不存在的;所谓DOMO级只是DOM历史坐标中的一个参照
点而已。具体说来，DOMO 级指的是 Internet Explorer 4.0 和 Netscape Navigator 4.0 最
初支持的DHTML。

---

**3.其他DOM标准**  

除了 DOM核心和DOM HTML接口之外，另外几种语言还发布了只针对自己的DOM标准。下面 列出的语言都是基于XML的，每种语言的DOM标准都添加了与特定语言相关的新方法和新接口 ：
⑪SVG (Scalable Vector Graphic,可伸缩矢M图）1.0;
Q MathML ( Mathematical Markup Language,数学标记语言）1.0;
⑫SMIL ( Synchronized Multimedia Integration Language,同步多媒体集成语言)。
还有一些语言也开发了自己的DOM实现，例如Mozilla的XUL( XML User Interface Language, XML 用户界面语言)。但是，只有上面列出的几种语言是W3C的推荐标准。  

**4.Web浏览器对DOWI的支持**  

在DOM标准出现了一段时间之后，Web浏览器才开始实现它。微软在IE5中首次尝试实现DOM, 但;ft到IE5.5才算是真正支持DOM1级。在随后的IE6和IE7中，微软都没有引人新的DOM功能，而 到了 IE8才对以前DOM实现中的bug进行;T修复。
Netscape 直到 Netscape 6 (Mozilla 0.6.0)才开始支持 DOM。在 Netscape 7 之后，Mozilla 把开发重心转 向了 Firefox浏览器。Firefox3完全支持DOM1级，几乎完全支持DOM2级，甚至还支持DOM3级的一部 分。（Mozilla开发闭队的目标是构建与标准100%兼容的浏览器，而他们的努力也得到了回报。}
目前，支持DOM已经成为浏览器开发商的首要H标，主流浏览器每次发布新版本都会改进对DOM 的支持。下表列出了主流浏览器对DOM标准的支持情况。
###  1.2.3 浏览器对象模型（BOM)
Internet Explorer 3和Netscape Navigator 3有一个共同的特色，那就是支持可以访问和操作浏览器窗 口的浏览器对象模铟（BOM, Browser Object Model)。开发人员使用BOM可以控制浏览器显示的页面 以外的部分。而BOM真正与众不同的地方（也是经常会导致问题的地方），还是它作为JavaScript实现 的一部分但却没有相关的标准。这个问题在HTML5中得到了解决，HTML5致力于把很多BOM功能写 入正式规范。HTML5发布后，很多关于BOM的困惑烟消云散。
从根本上讲，BOM只处理浏览器窗口和框架;佴人们习惯上也把所有针对浏览器的JavaScript扩展 算作BOM的…部分。下面就是一些这样的扩展：
- [ ] 弹出新浏览器窗U的功能;
- [ ] 移动、缩放和关闭浏览器窗U的功能;
- [ ] 提供浏览器详细信息的navigator对象;
- [ ] 提供浏览器所加载页面的详细信息的location对象;
- [ ] 提供用户显示器分辨率详细信息的screen对象;
- [ ] 对cookies的支持;
- [ ] 像XMLHttpRequest和IE的ActiveXObject这样的自定义对象。  

由于没有BOM标准可以遵循，因此每个浏览器都有Q己的实现。虽然也存在一些事实标准，例如 要有window对象和navigator对象等，佴每个浏览器都会为这两个对象乃至其他对象定义自己的属 性和方法。现在有了 HTML5, BOM实现的细节有望朝着兼容性越来越髙的方向发展。第8章将深人讨 论 BOM。
##  1.3 JavaScript 版本
作为Netscape “继承人”的Mozilla公司，是目前唯一还在沿用最初的JavaScript版本编号序列的浏 览器开发商。在Netscape将源代码提交给开源的Mozilla项目的时候，JavaScript在浏览器中的最后一个 版本号是1.3。（如前所述，1.4版是只针对服务器的实现。)后来，随着Mozilla基金会继续开发JavaScript, 添加新的特性、关键字和语法，JavaScript的版本号继续递增。下表列出了 Netscape/Mozilla浏览器中 JavaScript版本号的递增过程：
实际上，上表中的编号方案源自Firefox 4将内置JavaScript 2.0这一共识。W此，2.0版之前每个递 增的版本号，表示的是相应实现与JavaScript 2.0开发口标还有多大的距离。虽然原计划是这样，但 JavaScript的这种发展速度让这个计划成为不再可行。目前，JavaScript2.0还没有〇标实现。

---

请注意，只有Netscape/Moziila浏览器才遵循这种编号模式。例如，IE的JScript 就采用了另一种版本命名方案。换句话说，JScript的版本号与上表令JavaScript的版 本号之间不存在任何对应关系„而且，大多教浏览器在提及对JavaScript的支持情况 时，一般都以ECMAScript兼容性和对DOM的支持情况为准。

---

##  1.4 小结
JavaScript是一种专为与网页交互而设计的脚本语言，由下列三个不同的部分组成：
- [ ] ECMAScript,由ECMA-262定义，提供核心语言功能; 
- [ ] 文档对象模型（DOM)，提供访问和操作网页内容的方法和接口;
- [ ] 浏览器对象模型（BOM),提供与浏览器交互的方法和接口。  

JavaScript的这三个组成部分，在当前五个主要浏览器（IE、Firefox、Chrome、Safari和Opera)中 都得到了不同程度的支持。其中，所有浏览器对ECMAScript第3版的支持大体上都还不错，而对 ECMAScript 5的支持程度越来越高，但对DOM的支持则彼此相差比较多。对HTML5已经正式纳人标 准的BOM来说，尽管各浏览器都实现了某些众所周知的共同特性，但其他特性还是会因浏览器而异。
