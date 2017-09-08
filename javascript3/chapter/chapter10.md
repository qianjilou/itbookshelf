#  第10章 DOM([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 理解包含不同层次节点的DOM
- [ ] 使用不同的节点类塑
- [ ] 克服浏览器兼容性问题及各种陷阱  

DOM (文档对象模型）是针对HTML和XML文档的一个API (应用程序编程接口 DOM描
绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。DOM脱胎于
Netscape及微软公司创始的DHTML(动态HTML),但现在它已经成为表现和操作页面标记的真正的跨
平台、语言中立的方式。
1998年10月DOM 1级规范成为W3C的推荐标准，为基本的文档结构及丧询提供了接n。本章主
要讨论与浏览器中的HTML页面相关的DOM1级的特性和应用，以及JavaScript对DOM1级的实现。
IE、Firefox、Safari、Chrome 和 Opera都非常完善地实现:TDOM。
0
注意，丨E中的所有DOM对象都是以COM对象的形式实现的。这意味着IE中的 DOM对象与原生JavaScript对象的行为或活动特点并不一致。本章将较多地谈及这些 差异。
##  10.1 节点层次
DOM可以将任何HTML或XML文档描绘成一个由多M-节点构成的结构。节点分为几种不同的类 型，每种类型分别表示文杓中不同的信息及（或）标记。每个节点都拥有各自的特点、数据和方法，另 外也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点 为根节点的树形结构。以下面的HTML为例：
```
html
head
titleSample Page/title
/head
body
pHello World!/p
/body
```
可以将这个简单的HTML文档表示为一个层次结构，如阁10-丨所示。
文档节点是每个文档的根节点。在这个例子中，文档节点只有一个子节点，即html元索，我们
称之为文档元素，文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元紊中。每个文 档只能有一个文档元素。在HTML页面中，文档元素始终都是html元素。在XML中，没有预定义 的元素，因此任何元素都可能成为文档元素。
每一段标记都可以通过树中的个节点来表示：HTML元素通过元素节点表示，特性（attribute) 通过特性节点表示，文档类型通过文档类型节点表示，而注释则通过注释节点表示。总共有12种节点 类铟，这些类型都继承自--个基类型。
###  10.1.1 Node 类型
DOMl级定义了一个Node接口，该接U将由DOM中的所有节点类型实现。这个Node接口在 JavaScript中是作为Node类型实现的;除/ IE之外，在其他所有浏览器中都可以访问到这个类型。 JavaScript中的所有节点类型都继承ft Node类型，因此所有节点类型都共享着相同的基本属性和方法。
每个节点都有一个nodeType属性，用丁-表明节点的类型。节点类型由在Node类型中定义的下列 12个数值常量来表示，任何节点类型必居其一：  

Node. ELEMENT一N0DE( 1);
Q Node.ATTRIBUTE^0DE(2);
N〇de.TEXT_N0DE(3);
Node.CDATA_SECTION_NODE(4);
N〇de.ENTITY_REFERENCE_N0DE(5);
Node. ENT ITY_N0DE(6);
Node.PROCESSING一INSTRUCTION一N0DE(7);
Node.C0MMENT_N0DE(8);
Node.DOCUMENT_NODE(9);
Node.DOCUMENT一TYPE_NODE( 10);
Node. DOCUMENT_FRAGMENT_K〇DE( 11);
N〇de.N〇TATI〇N_N〇DE(12)0  

通过比较上面这邱常M,可以很容M地确定节点的类型，例如：
```
if {so^eNode.nodeType == Node - ELEMKNT_NODE) {	//在 IE 中无效
alert("Node is an element.")?
```
这个例T比较了 someKode.nodeType与Node.ELEMENT_NODE常虽。如果二#相等，则意味着 someNode确实是一个元素。然而，由于1E没有公开Node类型的构造函数，因此上面的代码在丨E中 会导致错误。为了确保跨浏览器兼容，最好还是将nodeType属性与数7值进行比较，如下所示：
if (aomeNoda.aodeType == 1){	//适用于所有洲览器
alert{"Node is an element.");
)
并不是所有节点类型都受到Web浏览器的支持。开发人员最常用的就是元素和文木节点。本章后 面将详细讨论每个节点类型的受支持情况及使用方法。
nodeNaxne 和 nodeValue 属性
要广解节点的具体信息，可以使用nodeName和nodeVa Lue这两个属性。这两个属性的值完全取 决于节点的类姻。在使用这两个值以前，敁好是像下面这样先检测一下节点的类型。
if (someNode.nodeType == 1{
value = someNode.nodeKame;	//nodeName 的值是元索的标签名
}
在这个例子中，首先检查节点类型，看它是不是一个元素。如果是，则取得并保存nodeName的值。 对T-元素节点，nodeName中保存的始终都是元紊的标签名，而nodevalue的值则始终为null。
节点关系
文档中所W的节点之间都存在这样或那样的关系。节点间的各种关系吋以用传统的家族关系來描 述，相当于把文裆树比喻成家谱。在HTML中，可以将13〇办元素看成是也化^元素的子元素;相应 地，也就可以将html元素U■成是body元素的父元素。而head元素，则以看成是body元素 的同胞元素，因为它们都是同一个父元素html的直接子元素。
每个节点都有一个childNodes属性，其中保存着一个NodeList Xf象。NodeList是一种类数组 对象，用于保存••组夯序的节点，时以通过位置来访问这些节点。请注意，虽然可以通过方括号语法来 访问NodeList的值，而fl这个对象也冇length属性，但它并不是Array的实例。NodeList对象的 独特之处在于，它实际上是姓丁• DOM结构动态执行査询的结果，W此DOM结构的变化能够ft动反映 在NodeList对象巾。我们常说，NodeList是打生命、有呼吸的对象.而不是在我们第一次访问它们 的某个瞬间拍摄下来的一张快照。
下面的例子展示了如何访问保存在NodeList中的节点——可以通过方括也可以使用item ( 方法。
var firstChild - someNode.childNodesl0];
var secondChild = someNode.childNodes.itom(l);
var count = someNode.childNodes.length;
无论使用方括号还是使用item (方法都没有问题，但使用方括号语法宥起来与访问数组相似，因 此颇受一些开发人员的W睐。另外，要注意length属性表示的是访问NodeList的那一刻，其中包含 的节点数M。我们在本书前面介绍过，对arguments对象使用Array.prototype.slice()方法可以 将其转换为数组。而采用同样的方法，也叮以将NodeList对象转换为数组。来看下面的例子：
//在IE8及之前版本中无效
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
除〖E8及更早版本之外，这行代码能在任何浏览器中运行。由于IE8及更早版本将NodeList: 实现为一个COM对象，而我们不能像使用JScript对象那样使用这种对象，W此上面的代码会导致 错误。®想在IE中将NodeList转换为数组，必须手动枚举所冇成员。下列代码在所有浏览器中都 可以运行：
function convertToArray(nodes){ var array = null; try {
array = Array.prototype.slice.call (nodes, 0); //杆对非 IE 浏览器  catch (ex) {
array = now Array();
for (var i=0, lerj=nodes. length; i  len; i++) { array.push(nodes[i]);
return array;
}
这个convertToArray (函数首先尝试了创建数组的最简单方式。如果导致了错误（说明是在 IE8及更¥版本中），则通过try-catch块来捕获错误，然后手动创建数组。这是另一种检测怪癖的 形式。
每个节点都也一个parentNode属性，该属性指向文档树中的父节点。包含在childNodes列表中 的所有节点都具有相同的父节点，W此它们的parentNode属性都指向同--个节点。此外，包含在
childNodes列表中的每个节点相互之间都是同胞节点。通过使用列表中每个节点的previousSibling 和nextsibling属性，可以访问同一列表中的其他节点。列表中第一个节点的previousSibling属性 值为null,而列表中最后一个节点的nextSibling属性的值同样也为null,如下面的例子所示：
if (someNode.nextSibling === null){
alert("Last node in the parentis chtldNodes list.")/
} else if (someNode.previousSibling === null){
alert ("First node in the parent's childNodes list，};
)
鸟然，如果列表中只有一个节点，那么该节点的nextSibling和previousSibling都为null。
父节点与其第-个和最后一个+节点之间也存在特殊关系。父节点的firstChild和lastChild 属性分别指向其childNodes列表中的第一个和最后一个节点。其中，someNode.firstChild的值 始终等于 s〇meNode.childNodes[0]，而 someNode.lastChild 的值始终等于 someNode. childNodes [someNode.childNodes.length-1]。在只有—T"子节点的情况下，firstChild 和 lastChild指向同一个节点。如果没有子节点，那么firstChild和lastChild的值均为null。明 确这些关系能够对我们査找和访问文档结构中的节点提供极大的便利。图10-2形象地展示了上述关系。
childNodes
图丨0-2
在反映这些关系的所有属性当中，childNodes属性与其他属性相比更方便一些，因为只须使用简 单的关系指针，就可以通过它访问文档树中的任何节点。另外，haSChildN〇deS()也是一个非常有用 的方法，这个方法在节点包含一或多个子节点的情况下返回true;应该说，这是比査询childNodes 列表的length属性更简单的方法。
所有节点都有的最后一个属性是ownerDocument,该属性指向表示整个文档的文档节点。这种关 系表示的是任何节点都M于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个 属性，我们可以不必在节点层次中通过层层回溯到达顶端，而是可以直接访M文捫节点。
f
	虽然所有节点类型都继承自Node,但并不是每种节点都有子节点。本章后面将
会讨论不同节点类型之间的差异。
操作节点
闵为关系指针都是只读的，所以DOM提供r一些操作竹点的方法。其中，最常用的方法是
appendChilcU), 用于向childNodes列表的末尾添加一个节点。添加节点后，childNodes的新增 节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。更新完成后，appendChildO 返回新增的节点。来看下面的例子：
var retumedNode = sonieNode.appendChild(newNode); alert(returnedNodc == newNode);	//true
alert(someNode.lastChild == newNode);	//true

如果传人到appemcJChildU中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置 转移到新位置。即使可以将DOM树看成是由一系列指针连接起来的，但任何DOM节点也不能丨司时出 现在文档中的多个位置上。因此，如果在调川appendChildO时传人了父节点的第一个子P点，那么 该节点就会成为父节点的最后一个子节点，如T•面的例子所示。
/ /someNode有多个子节点
var returnedNode = someNode.appendChiId(someNode.firstChild); alert(returnedNode == someNode.firstChild);	//false
alertreturnedNode == someNode.lastChild);	//true
如果需要把节点放在childNodes列表中某个特定的位置上，而不是放在末尾，那么可以使用 insertBeforeO方法。这个方法接受两个参数：要插人的节点和作为参照的节点。插人节点后，被插 人的节点会变成参照节点的前一个同胞节点（previousSibling),同时被方法返冋。如果参照节点是 null,则insertBefore()与appendChild()执行相同的操作，如下面的例子所亦。
//插入后成为最后一个子节点
returnedNode = someNodo.insertBefore(newNode, null); alert(newNode == someNode.lascChild); //true
//祐入后成为第一个子节点
var returnedNode = someNode.insertBefore(newNode, someNode.firstChild); alert(returnedNode == newNode);	//true
alert{newNode == someNode.firstChild); //true
//插入到最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lasCChild);
alert(newNode == someNode.childNodesfsomeNode.childNodes.length-2]); //true
前面介绍的appendChild()和insert3efore()方法都只插入节点，不会移除节点。而下面要介 绍的replaccChUdU方法接受的两个参数是：要插入的节点和要替换的节点。要替换的节点将由这个 方法返冋并从文档树中被移除，同时由要插人的节点占据其位置。来看K面的例子。
//替换第一个子节点
var returnedNode = someNode.replaceChi1d(newNode, someNode.firstChild);
//替换最后一个子节点
returnedNode = someNode.replaceChild(newNodex someNode.lastChild);
在使用repiacechiid()插人一个节点时，该节点的所有关系指针都会从被它替换的节点复制过 来。尽管从技术上讲，被替换的节点仍然还在文档中，但它在文档中已经没有了 己的位置。
如果只想移除而非替换节点，可以使用removeChildO方法。这个方法接受一个参数，即要移除 的节点。被移除的节点将成为方法的返回值，如下面的例子所示。
〃移除第一个子节点
var formerFirstChild = someNode.removeChiId(someNode.firstChild);
//移除最后一个子节点
var formerl.astChild = someNode.removeChiId(someNode.lastChild);
与使用replaceChild)方法一样，通过removeChildU移除的节点仍然为文押所有，只不过在 文档中已经没宥了自己的位置。
前面介绍的四个方法操作的都是某个节点的子节点，也就楚说，要使用这几个方法必须先取得父节 点（使用parentNodeM性）。另外，并不是所有类铟的节点都有子节点，如果在不支持子节点的节点 上调用了这些方法，将会导致错误发生。
其他方法
有两个方法趋所有类型的节点都有的。第•个就是cloneNodeU,用于创建调用这个方法的节点 的一个完全相同的副本。cloneNode ()方法接受一个布尔值参数，表示是否执行深复制。在参数为true 的情况下，执行深复制，也就是复制节点及其•整个子节点树;在参数为false的情况下，执行浅复制， 即只复制节点木身。复制后返回的节点副本属于文桤所有，但并没有为它指定父节点。因此，这个节点 副本就成为广-个“孤儿”，除非通过appendChildU、insertBefore)或replaceChild()将它

添加到文档中。例如，假设有下面的HTML代码。
ul
liitem l/li
liitem 2/li
liitem 3/li
/ul
如果我们已经将111元素的引用保存在了变量tnyList中，那么通常下列代码就可以看出使用 cloneNode )方法的两种模式。
var deepList = myList.cloneNode(true);
alert (deepList.childKodes. length) ;	//3 (IE  9)或 7 (其他到見器）
var shallowT.-ist = myList.cloneNode(false); alert(shallowList.childKodes.length); //0
在这个例子屮，deepList:中保存着一个对myList执行深复制得到的S1]本。因此，deepList中 包含3个列表项，每个列表项中都包含文本。而变量shallowList中保存费对rr^List执行浅复制得 到的副本，因此它不包含子节点。deepList.childNodes.length中的差异主要是因为IE8及更早版 本与其他浏览器处理空白字符的方式不一样。【E9之前的版本不会为空h符创建节点。
(§
cloneNodeO方法不会复制添加到DOM节点中的JavaScript属性，例如事件处
理程序等。这个方法只复制特性、（在明确指定的情况下也复制）子节点，其他一切
都不会复制。IE在此存在一个bug,即它会复制事件处理程序，所以我们建议在复制
之前最好先移除事件处理程序。
我们要介绍的最耵一个方法是normalize ()，这个方法唯一的作用就是处理文档树中的文本节点。 由于解析器的实现或DOM操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本甘点 的情况。.当在某个节点上调用这个方法时，就会在该节点的后代节点中杳找上述两种情况。如果找到了 空文本节点，则删除它;如果找到相邻的文本许点，则将它们合并为一个文本节点。本章后面还将进一 步讨论这个方法。
###  10.1.2 Document 类型
JavaScript通过Document类型表示文构。在浏览器中，document对象是HTMLDocument (继承 ft Document类型）的一个实例，表示整个1FTML贞面。rftj ll，document对象是window对象的一个 M性，因此可以将其作为全局对象来访问。Document节点具有下列特征：
nodeType 的值为 9;
nodeName 的值为"ttdocument1*;
nodeValue 的值为 null;
口 parentiNode 的值为 null;
ownerDocument	null;
□其子节点可能是一^ Documcnt^iype(最多-^XElement (最多-斗）、ProcessingTnstructiorx 或 Comment〇

254 第 10 章 DOM
Document类型可以表示HTML页面或者其他基于XML的文档。不过，最常见的应用还是作为 HTMLDocument;实例的document对象。通过这个文档对象，不仅可以取得与页面有关的信息，且还 能操作页面的外观及其底层结构。
C^\ 在Firefox、Safari、Chrome和Opera中，可以通过脚本访问Document类型的构 造函数和原型。但在所有浏览器中都可以访问HTMLDocument类型的构造函数和原型， j	包括丨E8及后续版本。
文档的子节点
M然 DOM标准规定 Document 节点的子节点可以是 DocumentType、Element、Processingln- struction或Comment,似还有两个内置的访问K-子节点的快捷方式。第一个就是documentElement 属性，该属性始终指向HTML贞面中的}^1«1元紊。另一个就是通过childNodes列表访问文档元素， 仴通过documentElexnentM性则能更快捷、更直接地访问该元素。以下面这个简单的页面为例。
hcml
body
/body
这个页面在经过浏览器解析后，其文档中只包含一个子节点，即httnl元素。可以通过 documentElement:或childNodes列表来访问这个元素，如下所示。
var htm3 = document .documencEleroent;	//取得对1^1111的？I 用
alert (html =« document.childNodesfO]) ;	//true
alert(html === document.firstChild);	//true
这个例了-说明，documentElement、firstChild 和 childNodes[0]的值相同，都指向html 元素。
作为HTMLDocument的实例，document对象还有一个body属性，直接指向body元素。因为开 发人员经常要使用这个元素，所以document.body在JavaScript代码中出现的频率非常高，其用法如下。 var body » document.body ;	//取得对body的？I 用
所有浏览器都支持 document.documentElement 和 document.body 属性。
Document另一个可能的子节点是DocumentType。通常将!DOCTYPE#签看成一个与文梢其他 部分不同的实体，可以通过doctype属性（在浏览器中是document .doctype )来访问它的信息。 var doctype = document-doctype;	//取得对!D〇CTYPK的幻用
浏览器对document.doctype的支持差別很大，可以给出如下总结。
IE8及之前版本：如果存在文档类型卢明，会将其错误地解释为一个注释并把它当作comment 节点;而document .doctype的值始终为null。
IE9+及Firefox:如果存在文档类型声明，则将其作为文档的第一个子节点;document.doctype 是一个 Documentiype 谇点，也可以通过 document • firstChild 或 document • childNodes [0 ] 访问N *个节点。
Safari、Chrome和Opera:如果存在文杓类型声明，则将其解析，但不作为文档的子节点。document .doctype 是一个 DocumentType 节点， 但该节点不会出现在 document .childNodes 中。

10.1节点层次 255
由于浏览器对document .doctype的支持不一致，因此这个厲性的用处很有限。
从技术上说，出现元素外部的注释应该算是文档的子节点。然而，不同的浏览器在是否 解析这些注释以及能否正确处理它们等方面，也存在很大差异。以下面简单的HTML页面为例。
!•-第一条注释—
html
body
/body
!- -第二条注释--
看起来这个页面应该有3个子节点：注释、html元素、注释。从逻辑上讲，我们会认为 document.childNodes中应该包含与这3个节点对应的3项。何是，现实中的浏览器在处理位于 html外部的注释方面存在如下差异。
IE8及之前版本、Safari 3.1及更高版本、Opera和Chrome只为第…条注释创建节点，不为第二 条注释创建节点。结果，第一条注释就会成为document.childNodes中的第一个子节点。
IE9及更高版本会将第一条注释创建为document. childNodes中的--个注释节点，也会将第 二条注释创建为document.childNodes中的注释子节点。
Q Firefox以及Safari 3.1之前的版本会完全忽略这两条注释。
同样，浏览器间的这种不一致性也导致T位于html元素外部的注释没有什么用处。
多数情况下，我们都用不着在document对象h调用appendChild(、removeChild(和 rCplaCeChild()方法，因为文档类型（如采存在的话）是只读的，而且它只能有一个元素子节点（该 货点通常早就已经存在了）。
文档信息
作为HTMLDocument的一个实例，document对象还有一些标准的Document对象所没有的属性。 这些属性提供了 document对象所表现的网页的一些倍息。其中第一个属性就是title,包含着 title元素中的文本——显示在浏览器窗口的标题栏或标签页上。通过这个属性可以取得当前页面的 标题，也可以修改当前页面的标题外反映在浏览器的标题栏中。修改title厲性的值不会改变title 元素。来肴下面的例子。
//取得文档标题
var originalTitle = document.title;
"设置文档标《
document.title - "New page title";
接下来要介绍的3个属性都与对网贞的请求有关，它们是URL、domain和referrer。URL M性 中包含面面完整的URL (即地址栏中显/]《的URL), domain属性中只包含页面的域名，而referrer 属性中则保存着链接到当前页面的那个页面的URL。在没有来源页面的情况下，referrer属性中可能 会包含空字符串。所有这些信息都存在于请求的HTTP头部，只不过是通过这些属性让我们能够在 JavaScrip中访问它们而G,如下面的例子所示。
//取得完整的URL
var url = document.URL;
10
//取得域名

256 第 10 章 DOM
var domain = document.domain?
//取得来源页面的URL
var referrer = document.referrer;	,
URL 与 domain M性足•相瓦关联的。例如，如果 document.l;RL 等于 

http://www.wrox.com/WileyCDA/， 那么 documont. domain 就等丁 www.wrox.com。
在这3个属性中，只有domain是可以设置的。何由于安全方面的限制，也并非可以给domain设 置任何值。如果URL中也含一个子域名，例如p2p.wrox.com，那么就只能将domain设置为com” (URL中包含"www”》如www.wrox.com时，也娃如此)。不能将这个属性设IS为URL中不包含的域， 如下面的例子所示。
//假设页面来自p2p.wroc-c〇n域
document. domain = "wrox.com";	It 成功
document.domain = ■nc20nline.net" ;	// 出嫌！
，页面中包含来fj其他子域的框架或内嵌框架时，能够设置document .domain就非常方便了。丨tj 于跨域安全限制，来自不同子域的页面无法通过JavaScript通信。而通过将每个页面的 document.domain设S为相同的值，这挫页面就nj■以互相访问对方包含的JavaScript对象了。例如， 假设有-个页面加栽Q www.wrox.com,其中含一个内嵌框架，框架内的页面加载自p2p.wrox.com。 由于document.domain字符率不一样，内外两个页面之间无法相互访问对方的JavaScript对象。但如 果将这两个页面的document .domain值都设HSMv/rox. comK,它们之间就可以通信了。
浏览器对domain属性还有•个限制，即如果域名一开始是“松散的”（loose),那么不能将它再设 置为“紧_的”（tight)3换句话说，在将document;.domain设置为"wrox.com"之后，就不能再将其 设置回-p2p.wrox.com"，否则将会导致错误，如下面的例子所示。
//假设页面来自于p2p.wrox.com域
document .domain = "wrox.con";	//松散的（成功）
document-domain = * p2p.wrox.com■;	// 紧堋的（出错！）
所有浏览器中都存在这个限制，倂ffi8是实现这一限制的最早的IE版本。
查找元素
说到最常见的DOM应用，恐怕就要数取得特定的某个或某组元素的引用，然后再执行--些操作了^ 取得元素的操作可以使用document对象的几个方法来完成。其中，Document类型为此提供了两个方
法：getEiementByld () getElementsByTagName () 〇
第一个方法，getElementBYld(，接收一个参数：要取得的元素的ID。如果找到相应的元素则 返回该元素，如果不存在带右相应：ID的元索，则返回null。注意，这里的丨D必须与页面中元素的id 特性（attribute)严格匹配，包括大小写。以下而的元素为例。
div id="rnyDivHSome texc/div
可以使用F面的代码取得这个元素：
var div = document .getEiementByld (_!TiyDiv‘_ ;	//取得(1^^元索的幻用

10.1节点层次 257
但是，下面的代码在除IE7及更早版本之外的所有浏览器中都将返回null。
var div = document .getElementByldt "mydiv");	//无效的 ID (在 IE7 及史早版本中可以〉
IE8及较低版本不K分ID的大小写，因此-myDiv•’和"reydiv•会被.当作相同的元素ID。
如果页面中多个元素的ID值相同，getElementByldt)只返回文档中第一次出现的元素。及较 低版本还为此方法添加了一个有意思的“怪癖”：name特性与给定ID匹配的表单元素（input、 textarea、buttons^select)也会被该方法返回。如果有哪个表单元素的name特性等于指 定的ID,而且该元素在文档中位带有给定ID的元素前面，那么IE就会返冋那个表单元素。来看下面 的例子。
input type="text* name="myElement" value="Text field"
div id="myElement"A div/div
基于这段 HTML代码，在 IE7 中调用 document.getElementByldrmyElement ")，结果会返 冋1叩1^元素;而在其他所有浏览器中，都会返冋对div元素的引用。为了避免IE中存在的这个问 题，最好的办法是不让表单字段的name特性与其他元素的ID相同。
另一个常用于取得元素引用的方法是getElementsByTagNameO。这个方法接受一个参数，即要 取得元素的标签名，而返回的是包含零或多个元素的NodeList。在HTML文档中，这个方法会返冋一 个HTMLCollection对象，作为--个“动态”集合，该对象与NodeList非常类似。例如，下列代码 会取得页面中所有的iing兀素，并返间一个HTMLCollection。 var images = document.getElementsByTagName{"img");
这行代码会将一个HTMLCollection对象保存在;images变里中。与NodeList对象类似，可以
使用方括号语法或方法来访问HTMLCollectioii对象中的项。而这个对象中元素的数量则可以
通过其length属性取得，如下面的例子所示。
alert {images, length} ;	//输出图像的数量
alert {images (0] .src) ;	//输出苐一个图像元素的src特性
alert (images. item{0) . src) ;	//输出第一个图像元素的src特性
HTMLCollection对象还有一个方法，叫做namedltemU ,使用这个方法可以通过元素的name 特性取得集合中的项。例如，假设上面提到的贞面中包含如下img元素：
img src="myimage.gif" name=■myImage■
那么就可以通过如下方式从images变最中取得这个img元素： var mylmage = images.namedltem(#myImage*);
在提供按索引访问项的基础上，HTMLCollection还支持按名称访问项，这就为我们取得实际想要 的元索提供了便利。而FL,对命名的项也可以使用方括号语法来访问，如下所示： var mylmage = images[•mylmage"1;
对HTMLCollection而言，我们可以向方括号中传人数值或字符串形式的索引值。在后台，对数 值索引就会调用itemU ,而对字符串索引就会调用namedltem(。
要想取得文档中的所有兀素，可以向geLElementsByTagName( 中传人• * ■•。在JavaScript及CSS 中，M号（* )通常表示“全部”。下面看一个例子。
var allElements = document.getElementsByTagName("**);

258 第 10 章 DOM
仅此一行代码返冋的HTMLCollection中，就包含丫整个页面中的所有元素	按照它们出现的
先后顺序。换句话说，第一项是html元素，第二项是〈head〉元素，以此类推。由于丨E将注释(Co聊ent ) 实现为元素（Element ),因此在IE中调用getElementsByTagName (• * ■ 将会返回所有注释节点。
f
	虽然标准规定标签名需要区分大小写，但为了最大限度地与既有HTML页面兼
容，传给getElementsByTagName()的标签名是不需要区分大小写的但对于XML W面而言（包括XHTML)，getElementsByTagName()方法就会区分大小写。
第三个方法，也是只有HTMLDocmnent类型才有的方法，是getElementsByNameO。顾名思义， 这个方法会返回带有给定name特性的所有元素。最常使用getElementsByNameO方法的情况是取得 单选按钮;为了确保发送给浏览器的值正确无误，所有单选按钮必须具有相同的name特性，如下面的 例子所示。
fieldset
legendWhich color do you prefer?/legend
ul
liinput type=_radio" value="red" narrle=''color" id= "colorRed"
label for="colorRed"Red/labelx/li
lixinput type^'radio" value= "green" name="color" id= "colorGreen"
label for=-colorGreen"Green/label/li
liinput type="radio* value="blue* name="color* id=_colorBlue_
 label for=* color Blue •Blue/labelx/li
/ul
/fieldset
如这个例子所示，;«中所有单选按钮的name特性值都是"color•，但它们的ID可以不间。丨D的 作用在于将label元素应用到每个单选按钮，而name特性则用以确保三个值中只有一个被发送给浏 览器。这样，我们就可以使用如下代码取得所有单选按钮：
var radios = document.getElementsByName^"color");
与 getElementsByTagName()类似，getElsnentsByN^roe()方法也会返回一^* HTMLCollectioin。 但是，对于这里的单选按钮来说，namedltem()方法则只会取得第一项（因为每一项的name特性都相同)0
特殊集合
除了属性和方法，document对象还有-•哗特殊的集合。这些集合都是HTMLCollection对象， 为访问文档常用的部分提供了快捷方式，包括：
□ document.anchors,包含文档中所有带name特性的a元素;
□ document.applets,包含文档中所有的applet:元素，因为不再推荐使用applet元素， 所以这个集合已经不建议使用了;
□ document; • forms，包含文档中所有的£〇1^1兀素，与 document • getElenentsByTagName f〇rm_) 得到的结果相同;
□ document .images，包含文档中所有的img 元素，与 document .getElementsByTagName ("img")得到的结果相同;
□ document, links,包含文约中所有带href特性的a元素。
这个特殊集合始终都可以通过HTMLDocument对象访问到，而R，与HTMLCollection对象类似， 集合中的项也会随着，前文档内荇的更新而史新。

10.1节点层次 259
DOM —致性检测
由于DOM分为多个级別，也包含多个部分，因此检测浏览器实现了 DOM的哪邱部分就丨-分必要 了。document, implementation屈性就足为此提供相应信息和功能的对象，与浏览器对DOM的实现 直接对应。DOM1级只为document. implementation规定r一个方法，即hasFeature()。这个方 法接受两个参数：要检测的DOM功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该 方法返回true,如下面的例子所示：
var hasXmlDom = document.implementation.hasFeature("XML", *1.0");
下表列出了可以检测的不同的值及版本g-。
尽管使用hasFeamreO确实方便，但也有缺点。因为实现者可以自行决定是否与DOM规范的不flj| 同部分保持一致。事实上，要想让hasFeartureO方法针对所有值都返回true很容易，但返回true ™ 有时候也不意味着实现与规范一致。例如，Safari 2jc及更早版本会在没有完全实现某些DOM功能的情 况下也返回true。为此，我们建议多数情况下，在使用DOM的某些特殊的功能之前，最好除了检测 hasFeat:ure(之外，还同时使用能力检测。
文档写入
有一个document对象的功能已经存在很多年了，那就是将输出流写人到网页中的能力。这个能力 体现在下列 4个方法中：write(、writelnO、open 和 close()。其中，write{)和 writeln
方法都接受一个字符串参数，即要写人到输出流中的文本。writeO会原样写人，而writelnU则会 在字符串的末尾添加一个换行符（\n)。在页面被加载的过程中，可以使用这两个方法向页面中动态地 加人内容，如下面的例子所示。

260 第 10 章 DOM



html
head
titledocument.write() Example/title
/head
body
pThe current date and time is:
script type="text/javascript"
document .write (wstrong" + (new DateO ) .toStringO + M/strong") ?
/script
/p
/body
/htrol
.	DocumentWriteExample01.htm
这个例子展示了在页面加载过程中输出当前日期和时间的代码。其中，日期被包含在一个str〇ng 元素中，就像在HTML页面中包含普通的文本一样。这样做会创建一个DOM元素，而且可以在将来访 问该元素。通过write (和writeln (输出的任何HTML代码都将如此处理。
此夕卜，还可以使用write(和writeln(方法动态地包含外部资源，例如JavaScript文件等。在包 含JavaScript文件时，必须注意不能像下面的例子那样直接包含字符串"/SCriPt",因为这会导致该 字符串被解释为脚本块的结束，它后面的代码将无法执行。



html
head
titledocument.write) Example 2/titXe
/head
body
script type="text/j avascript•
document.write(-script type=\"text/javascriptsrc=\"file.js\*, +
■/script” ;
/scri.pt
/body
/html
DocumentWriteExample02. Htm
即使这个文件看起来没错，但字符串"/script”将被解释为与外部的scriptS签匹配，结果 文本将会出现在页面中。为避免这个问题，只需把这个宇符串分开写即可;第2章也曾经提及这个 问題，解决方案如下。
html
head
titledocument.write() Example 3/citle
/head
body
script type= __ text/javascript" 
document.write{*script type=\_text/javascript\" src=\"file.js\"■ +
/script
/body
/html
DocumentWriteExample03. htm

10.1节点层次 261
字符审"\/script"不会被当作外部script#签的关闭标签，因而页面中也就不会出现多余 的内容了。
前面的例子使用document.writeO在页面被呈现的过程中直接向其中输出了内容。如果在文档 加载结束后再调用document.writeO,那么输出的内容将会重写整个页面，如下面的例子所示：
html
head
titledocument.write〇 Example 4/title
/head
body	»
pThis is some content that you won't get to see because it will be overwritten./p script type=-text/javascript•
window.onload = function(){	•
document.write("Hello world!*);
);
/script
/body
/html
Document WriteExample04. htm
在这个例子中，我们使用了 window, onload事件处理程序（事件将在第13章讨论），等到页面完 全加载之后延迟执行函数。函数执行之后，字符串"Hello world! •会重写整个页面内荇。
方法open()和close()分别用于打开和关闭网页的输出流。如果是在页面加载期间使用write() 或wrizeln()方法，则不需要用到这两个方法。
严格型XHTML文档不支持文档写入a对于那些按照application/xmi+xhtml 内容类型提供的页面，这两个方法也同样无效。
###  10.1.3 Element 类型
除了 Document类型之外，Element类型就要算是Web编程中最常用的类型了。Element类型用 丁‘表现XML或HTML元素，提供了对元素标签名、子节点及特性的访问。Element节点具有以下特征：
nodeType 的值为 1;
nodeName的值为元素的标签名;
nodeValue 的值为 null;
parentNode 可能是 Document 或 Element;
□其子节点可能是 Element、Text、Comment、Processinglnstruction、CDATASection 或 EntityReference〇
要访问元素的标签名，可以使用nodeName属性，也可以使用tagName厲性;这两个属性会返冋 相同的值（使用后者主要是为了清晰起见)。以下面的元素为例：
div id*"myDiv*x/div
可以像下面这样取得这个元素及其标签名：
var div = document*getElementById("myDiv");
alert(div.tagName);	//"DIV"
alert(div.tagName == div.nodeName); //true

262 第 10 章 DOM
这里的元素标签名是div,它拥有一个值为"myDiv•的ID。可是，div.tagName实际上输出的是 -DIV”而非"div’_。在HTML中，标签名始终都以全部大写表示;而在XML(有时候也包括XHTML) 中，标签名则始终会与源代码中的保持一致。假如你不确定自己的脚本将会在HTML还是XML文档中 执行，最好是在比较之前将标签名转换为相同的大小写形式，如下面的例子所示：
if (element.tagName == •div**){ //不能这样比较，很容易出嫌！
//在此执行某些操作
if {elerr.ent.tagName.toLo^/erCaseO == "divM{ //这样最好（适用于任何文核） //在此执行茱些操作
这个例子展示了围绕tagName属性的两次比较操作。第一次比较非常容易出错，因为其代码在 HTML文档中不管用。第二次比较将标签名转换成了全部小写，是我们推荐的做法，因为这种做法适用 于HTML文捫，也适用于XML文杓。

可以在任何浏览器中通过脚本访问Element类型的构造函数及原型，包括IE8及 之前版本.在Safari2之前版本和Opera8之前的版本中，不能访问Element类型的构 造函数。
HTML元素
所有HTML元素都由HTMLElement类型表示，不是直接通过这个类型，也是通过它的子类型来表 示。HTMLElement类型直接继承A Element并添加了一避属性。添加的这些属性分别对应于每个HTML 元素中都存在的下列标准特性。
id,元素在文档中的唯一标识符。
title,有关元素的附加说明信息，一般通过工具提示条显示出来。
lang,元素内容的语言代码，很少使用。
dir,语言的方向，值为-Itr" (left-to*right,从左至右）或” rtl- (right-to-left,从右至左）， 也很少使用。
className,与元素的class特性对应，即为元素指定的CSS类。没有将这个属性命名为class, 是因为class是ECMAScript的保留字（有关保留字的信息，请参见第1章)。
上述这些属性都可以用来取得或修改相应的特性值。以下面的HTML元素为例：
div id=*myDiv" class="bd" title=*Body text* lang=*en" dir="ltr"x/div
HTMLElementsExampleOl. htm
元素中指定的所有信息，都可以通过下列JavaScript代码取得:
var div = document.getElementByld(•myDiv");
alert{div.id);	//"myDiv""
alert(div.className);	//"bd*
alert (div.title);	/"Body	text"
alert(div.lang);	//"en*
alert(div.dir);	//"ltr"
当然，像下面这样通过为每个属性賦予新的值，也可以修改对应的每个特性:

10.1节点层次 263
div. id = "soineOtherld" /
div. classNartie = "ft";
div.title = "Some other text";
div.lang s "fr";
div.dir =-rtl";
HTMLElementsExampleOl Mm
并不是对所有属性的修改都会在页面中直观地表现出来。对id或lang的修改对用户而言是透明 不可见的（假设没有*于它们的值设置的CSS样式)，而对title的修改则只会在鼠标移动到这个元素 之上时才会示出来。对dir的修改会在属性被重写的那一刻，立即影响页面中文本的左、右对齐方式。 修改className时，如果新类关联了与此前不同的CSS样式，那么就会立即应用新的样式。
前面提到过，所有HTML元素都是由HTMLElement或者其更具体的子类甩来表示的。下表列出了 所有HTML元素以及与之关联的类型（以斜体印刷的元紊表示已经不推荐使用了 。注意，表中的这些 类型在Opera、Safari、Chrome和Firefox中都可以通过JavaScript访问，但在IE8之前的版本中不能通 过 JavaScript 访问。


264 第 10 章 DOM
(续）
表中的每一种类型都有与之相关的特性和方法。本书将会讨论其中很多类型。
2取得特性
每个元素都冇一或多个特性，这些特性的用途是给出相应元素或其内W的附加信息。操作特性的 DOM 方法主要有三个，分别是 getAttribute(、setAttribute()和 removeAttributeO。这三 个方法可以针对任何特性使W,包括那共以HTMLElement类沏属性的形式定义的特性。来看下面的例了•:
var div ：： documenc .getElementById( "myDiv"); alert(div.getAttribute("id"});	//"^yDiv"
alert(div.gecAttribute("class"));	//"bdn
alert(div.getAttribute("title");	//"Body text"
alert{div.getAttribute("lang"));	//"en"
alerc {div.geCAtCribute ("dir")) ;	/"] tr"
注意，传递给getAUributeO的特性名与实际的特性名相间。因此要想得到class特性值，应 该传人"class"而不是"className",后者只有在通过对象属性访问特性时才用。如果给定名称的特性 不存在，getAttributeU返回 nullu
通过getAttribute (方法也吋以取得自定义特性（即标准HTML语言中没有的特性）的值，以 下面的元索为例：
div id="myDiv" my_special_attribute= "hello! "x/divs
这个元素包含一个名为my_special_at;tribUte的定义特性，它的值是"hello!"。可以像取 得其他特性一样取得这个值，如下所示：
var value = div.getActribute (*iny_special_attribute°);
不过，特性的名称足不区分大小写的，即"ID"和"id"代表的都是同一个特性。另外也要注意，根 据HTML5规范，Q定义特性应该加上data-前缀以便验证。

10.1节点层次	265
任何元素的所有特性，也都可以通过DOM元素本身的属性来访问。当然，HTMLElement也会有5 个属性与相应的特性一-•对应。不过，只有公认的（非自定义的）特性才会以属性的形式添加到DOM 对象中。以下面的元素为例：
div idl= "myDiv" aligns^lefc" my_special_attribute=*hellol "/div
因为id和align在HTML中是div的公认特性，因此该元素的DOM对象中也将存在对应的属 性。不过，自定义特性my_special_al:tribute在Safari、Opera、Chrome及Firefox中是不存在的; 但IE却会为^定义特性也创建属性，如下面的例子所示：
alert(div.id);	//"myDiv"
alert (div.my_special_attribute) ;	//undefined	(IE除夕卜
alert(div.align);	//"left"
Element A ttributesExample02.htm
有两类特殊的特性，它们虽然有对应的属性名，但属性的值与通过getAttribute()返回的值并不 相同。第一类特性就是style,用于通过CSS为元素指定样式。在通过getAttributeU访问时，返 回的style特性值中包含的是CSS文本，而通过属性来访问它则会返回一个对象。由于style属性是 用于以编程方式访问元素样式的（本章后面讨论)，因此并没有直接映射到style特性。
第二类与众不同的特性是onclick这样的事件处理程序。当在元素上使用时，onclick特性中包 含的是JavaScript代码，如果通过getAttributeO访问，则会返回相应代码的字符串。而在访问 onclick属性时，则会返回一个JavaScript函数（如果未在元素中指定相应特性，则返回null)。这是 因为onclick及其他事件处理程序属性本身就应该被賦予函数值。
由于存在这些差别，在通过JavaScript以编程方式操作DOM时，开发人员经常不使用getAtCri- bute(,而是只使用对象的属性。只有在取得自定义#值的情况下，才会使用getAttributeO方法。
在IE7及以前版本中，通过getAttribute ()方法访问style特性或onclick这样 的事件处理特性时，返回的值与属性的值相同。换句话说，geHAttjribute("style"返 回一个对象，而getAttribute("onclick")返回一个函数。虽然IE8已经修复了这个 bug,但不同IE版本间的不一致性，也是导致开犮人员不使用getAttribute ()访问HTTML 特性的一个原因。
设置特性
与getAttribute (对应的方法是setAttribute(，这个方法接受两个参数：要设置的特性名和 值。如果特性已经存在,5〇1厶1;1：1：：1)3此6()会以指定的值替换现有的值;如果特性不存在，361：扯1：：1：1!3此6() 则创建该屈性并设置相应的值。来看下面的例f-:
div.setAttribute("id", "someOtherld"); div.setAttribute("class", "ft"); div.setAttribute (*'title", "Some other text"); div.scLALtribute("lang"#■frw ; div.setAttribute("dir", "rt1•);
ElementAttributesExampleOl.htm

266 第 10 章 DOM
通过setAttributeO方法既可以操作HTML特性也可以操作『丨定义特性。通过这个方法设置的 特性名会被统一转换为小写形式，即"ID"M终会变成"id"。
因为所有特性都足属件，所以直接给属性賦值可以设S特性的值，如下所示。
div.id = "someOtherld"; div.align = "left";
不过，像下®这样为DOM元素添加一个自定义的M性，该属性不会自动成为元素的特性。
div.rycolor = "red";
alert (div.gctActributct "roycolor")) ; //null (IE 除外）
这个例子添加了一个名为nvcolor的属性并将它的值设a为"red■。在大多数浏览器中，这个属 性都不会A动变成元素的特性，因此想通过getAttributen取得同名特性的值，结果会返回null。 可是，定义属性在IE屮会被当作元素的特性，反之亦然。
在1E7及以前版本中，setAttj：ibul:e()存在一些异常行为。通过这个方法设置
class和style特性，没有任何效果，而使用这个方法设置事件处理程序特性时也
一样。尽管到了 IE8才解决这些问题，但我们还是推荐通过属性来设置犄性。
要介绍的最活-个方法^removeAttributeU,这个方法用于彻底删除元素的特性。调用这个方
法不仅会清除特性的值，而且也会从元素中完全删除特性，如下所示：
div.removeAttribute("class");
这个方法并不常用，们在序列化DOM元索时，可以通过它来确切地指定要包含哪些特性。
C LE6及以前版本不支持removeAttribute(}
attributes 属性
Element类型•是使用attributes厲性的唯‘*个DOM节点类墙D attributes嵌性中包含一个
MamedNodeMap，与NodeList类似，也是一*个“动态”的集合。元素的每一个特性都由一个Attr节
点表不，毎个节点都保存在NainedNodeMap对象中。NaiaedNodeMap对象拥有下列方法。
口 getNamedltera (name;:返回 nodeName 属性等于 na/ne 的节点;
removeNamedltem 〇2aine;:从列表中移除nodeName属性等于na/ne的节点;
setNamedltem (node;:向列表中添加节点，以节点的nodeName属性为索引;
item fposj:返回位于数字pos位置处的节点。
attributes属性巾包含一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeValue
就趋特忭的值。要取得元素的id特性，可以使用以下代码。
var id = element.attributes.getNamedItem{"id").nodeValue;
以下是使用方括号语法通过特性名称访问节点的简写方式。
var id - element.attributes C * id"].nodeValue;
也可以使用这种语法來设S特性的值，即先取得特性节点，然后再将其nodevalue设置为新值，
如下所示。











10.1节点层次 267
element .attributes [ "id") .nodeValue = ■someOtherld,';
调用removeNamedltem 〇方法与在兀素上调用removeAt:tribiate ()方法的效果相同	直接删
除其有给定名称的特性。下面的例子展示了两个方法间唯一的区别，即reirioveNarnedltemO返冋表示 被删除持性的Attr■节点。
setNamedltemU是一个很不常用的方法，通过这个方法可以为元素添加•个新特性，为此 笟要为它传人一个特件节点，如下所示。
一般来说，丨于前而介绗的attributes的方法不够方便，因此开发人员更多的会使用 getAttribute U、removeAttribute ()和 setAttribute ()方法■〇
不过，如果想要遍历元素的特性，attributes属性倒是可以派h用场。在需要将DOM结构序列 化为XML或HTML字符串时，多数都会涉及遍历元素特性。以下代码展示了如何迭代元素的每一个特 性，然后将它们构选成na/ne，"va_2ue"这样的字符串格式。
var pairs = new Array{),
attrNarr.e,
attrValue,
i/
lon;
for (i-0, ler.=relement .attributes.length; i  len; i++} {
attrName = eleraent.atcributes [i] .nodeNciine;
attrValue - eler.ent .attributes [i] .nodeValuc?
pairs. push (attrName + "-X"" + attrValue + n\
}
return pairs.join(" *);
这个函数使用了-个数纟ft来保存名值对，最后再以空格为分隔符将它们拼接起来（这是序列化长字 符串时的一种常用技巧)。通过attributes • length屈性，for循环会遍历每个特性，将特性的名称 和值输出为字符申。关于以上代码的运行结果，以下是两点必耍的说明。
□针对attributes对象中的特性，不同浏览器返回的顺序+同。这些特性在XML或HTML代 码中出现的先后顺序，不一定与它们出现在attributes对象屮的顺序一致。
□ IE7及更早的版本会返M HTML元素中所有吋能的特性，包括没有指定的特性。换句话说，返 问100多个特性的情况会很常见。
针对IE7及更早版本中存在的问题，可以对」:面的函数加以改进，Lh它只返冋指定的特性。每个特 性节点都有一个名为specified的属性，这个丨4性的值如果为true,则意味宥要么是在HTML中指 定/■相应特性，要么娃通过setAttributeU方法设置丫该特性。在IE中，所有未设S过的特性的该 属性值都为false, rflf在其他浏览器中根本不会为这类特性生成对应的特性节点（W此，在这拽浏览器 中，任何特性节点的specified值始终为true)。改进沿的代码如下所示。
var oldAttr = element•attributes.removeNamedltem("id"};
element.attributes.9etNamedItem{newAttr);



function outputAttributes(element){




268 第 10 章 DOM
function outputAttributes(element){ var pairs = new Array(), attrName, attrValue,
i/
len;
for (1=0, len=element.attributes.length; i  len? i++){ attrName = elenent.at:tributesIi].nodeName; attrValue = element.attributes[i].nodeValue; if (element.attributes[i].specified) {
paira.puflh(attrName + « =	+ attrValue + n\WM);

return pairs.join("");
Element A ttributesExample04. htm
这个经过改进的函数吋以确保即使在IE7及更V•的版本中，也会只返回指定的特性。
创建元素
使用document.createElement U方法对以创建新元素。这个方法只接受一个参数，即要创建元 素的标签名。这个标签名在HTML文档中不区分大小写，而在XML (包括XHTML)文档中，则是区 分大小写的。例如，使用下面的代码可以创建一个div元素。
var div = document.createElement("div");
在使用createElement()方法创建新兀素的同时，也为新儿索设置_TovmerDocuemnt腐性。此 时，还可以操作元素的特性，为它添加更多子节点，以及执行其他操作。来看下面的例了-。
div.id = "myNewDiv0;
div.className = "box•;
在新元素上设置这些特性只楚给它们赋予了相应的信息。由于新元索尚末被添加到文档树中，因此 设a这些特性不会影响浏览器的显示。要把新元素添加到文档树，吋以使用appendChildU、inser- tBefore 〇或replaceChild ()方法。下面的代码会把新仓I]建的元素添加到文档的1〇^元素中。
document.body.appondChild(div);
CreateElementExampleO 1. htm
一A将元索添加到文档树中，浏览器就会立即呈现该元素。此后，对这个元素所作的任何修改都会 实时反映在浏览器中。
在IE中吋以以另一种方式使用createElement (,即为这个方法传人完整的元素标签，也可以包 含属性，如下面的例了-所示。
var div = document.createElement ("div id-\"myNewDiv\" class=\Mbox\,*/div ");
这种方式有助于避开在1E7及更早版本中动态创建元素的某拽问题。下面是已知的一些这类问题。 口不能设置动态创建的iframe儿素的name特性。
口不能通过表单的reset 〇方法重设动态创建的input元素（第13章将讨论reset {)方法）。

10.1节点层次 269
□动态创建的以96特性值为"^56!："的13\：^011兀素.車:设不卩表单。
□动态创建的-批name相同的单选按钮彼此奄无关系。name值相N的一组普选按钮本来应该用 于表示同一选项的不同值，们动态创建的一批这种单选按钮之间却没有这种关系。
上述所有问题都可以通过在createSlement (中指定完整的HTML标签来解决，如下面的例子所示。
if (client, browser, ie Sr& client .browser. ie =7}{
//创建一个带name特性的iframe元素
var iframe = documenc.createElenienl; ("ifrano najne=\"nvframe\"x/iframe*); //创建input元素
var input = document.createElement(#input type=\"checkbox\"");
//创建button元素
var button = document.crcateElcment {"button cype-\preset\,'/button");
//创建单选按担
var radiol = document.createElement(*input type=\ttradio\" name=\"choice\" " f Mva:ue=\"l\""};
var radio2 = document.createElement(*input type=\"radio\" nane=\"choice\""十
"value=\n2\"n);
)
与使用creaujElemenU)的惯常方式一样，这样的用法也会返回一个D〇M元素的引用。可以将 这个引用添加到文档中，也可以对«加以增强。但娃，由于这样的用法要求使)H浏览器检测，因此我们 建议只在需要避开IE及更¥.版本中h述某个H题的情况下使用。Jt•他浏览器都不支持这种用法。
元素的子节点
元素可以有任意数II的子节点和后代节点，因为元素可以足其他元素的子节点。元素的 childNodesM性中包含了它的所有子节点，这邱子节点有可能赴元素、文本节点、注释或处理指令。 不同浏览器在看待这些节点方闹存在M著的不同，以下面的代码为例。
ul id-KinyList'*
liltem l/li
liIcoin 2/li
liItem 3/li
/ul
如果是IE来解析这些代码，那么〇1元素会有3个？节点，分别是3个li元素。但如果是在K 他浏览器中，ul元素都会冇7个元索，包括3个li%索和4个文本节点（表示li元素之间的空 白符X如果像F®'这样将元素间的空丹符删除，么所有浏览器都会返回相同数H的了-节点。
ul id="myList"liXtGiti l/lixliItem 2/lixliXtero 3/lix/ul
对于这段代码，〇：1元素在任何浏览器中都会包含3个了-节点。如果需要通过childNodes属性 遍历子贳点，那么一定不要忘记浏览器间的这一差别。这怠味翁在执行某项操作以前，通常都要先检査 —下nodeTpye域性，如下而的例子所示。
for (var i=0, len=element.childNodes.length; i  丄en; i++){ if (element.childNodes[i】.nodeType — 1){
//执行某些操作

270 第 10 章 DOM
这个例子会循环遍历特定元素的每一个子节点，然后只在子货点的nodeiype等丨（表示是元素 节点）的情况下，才会执行某些操作。
如果想通过某个特定的标签名取得子节点或后代节点该怎么办呢？实际上，元素也支持 getElementsByTagNaroe(方法。在通过元素调用这个方法时，除了搜索起点是？3前元素之外，其他 方面都跟通过document调用这个方法相同，因此结果只会返W当前元素的后代。例如，要想取得前W U1元素中包含的所有lb元素，可以使用K列代码。
var ul - document .gecsleir.entByld (,fmyList") ? var items = ul.getElementsByTagName("li");
要注意的是，这里1^的后代中只包含：F[接了*元素。不过，如果它包含史'多层次的后代元索，那 么各个M次中包含的ii元素也都会返回。
10.1.4 Text 类型
文本节点由Text:类頌表示，包含的是吋以照字面解释的纯文本内容。纯文本中吋以包含转义后的 HTML字符，似不能包含HTML代码。Text f/点具有以下特征：
nodeType 的值为 3;	*
nodeName 的值为”#text";
nodeValue的值为节点所包含的文本;
parentNode 是一^ Element;
□不支持（没有）子节点。
可以通过nodeValue属忭或data 性访问Text节点中包含的文本，这两个属性中包舍的值相 同。对nodeValue的修改也会通过data反映出来，反之亦然。使用下列方法可以操作节点中的文本。 口 appendData(text):将text添加到节点的末M。
deleteData (offset, count}:从 offset 指定的位 S 开始删除 count 个字符。
insert^Data (offset, text):在 offset 指定的位S插人 text。
replaceData (offset, count, text):用 text 替换从 offset 指定的位货开始到 offset+ count为止处的文本。
spiitText (offset):从offset指定的位置将1前文本节点分成两个文本节点。
substringData(offset, cou/it):提取从 offset 指定的位置开始到 offset+count 为止 处的字符串。
除了这鸣方法之外，文本节点还心一个length JS性，保存蒋节点中字符的数目。而且， nodeValue • length 和 data • length 中也保存着M样的值。
在默认情况K，毎个可以包含内容的元索最多只能杏一个文本节点，而a必须确实有内容存在。来 宥几个例子。
!--没有内容，也就没有文本节点--
divx/div
有空格，因而有一个文本节点--
div /div
!--有内容，因而有一个文本爷点 divHello World!/div

10.1节点层次	271
上面代码给出的第一tdiv元素没有内容，因此也就不存在文本节点。开始与结束标签之间只要 存在内容，就会创建一个文本节点。W此，第二个div元素中虽然只包含-个空格，但仍然有一个文 本子节点;文本节点的nodeValue值是一个空格。第三tdiv也有…个文本节点，其nodeValue的 值为"Hello W〇r]d!"。可以使W以下代码来访问这些文本子节点。 var text Node = div. f irstChild;	//或者 div.childNodes [0]
在取得了文本节点的引用后，就可以像卜面这样来修改它了。
div.firstChild-nodeValue = "Some other message*';
TextNodeExampleOl.htm
如果这个文本节点当前存在T文档树中，那么修改文本节点的结果就会立即得到反映。另外，在修 改文本节点时还要注意，此时的字符串会经过HTML (或XML,取决于文档类型）编码。换句话说， 小于号、大于号或引号都会像下面的例T一样被转义。
//输出好果是"Some &lt ;strong&gt;other&lt; /strong&gt; message" div.firstChild.nodoValue = "Some 9trongother/strong message";
TextNodeExample02. htm
应该说，这是在向DOM文档中插人文本之前，先对艽进行HTML编码的一种有效方式。
在【E8、Fircfox、Safari、Chrome和Opera中，可以通过脚本访问Text类型的构造 函教和原®。
创達文本节点
以使用document.createTextNode()创建新文本节点，这个方法接受一个参数	要插人节点
中的文本。与设置已有文本节点的值一样，作为参数的文本也将按照HTML或XML的格式进行编码。
var textNode = document.createTextNode{"strongHello/strong world!");
在创述新文本节点的间时，也会为其设置ownerDocumenc•属性。不过，除非把新节点添加到文拽 树中已经存在的节点中，否则我们不会在浏览器窗口中看到新节点。下而的代码会创建一个div元素 并向其中添加一条消息。
var element - document.createElemcnt{"div"); elemens.className = "message";
var LcxLNodc = document.createTextNodo(-Hello world!"); element.appendChild(textNode);
document.body.appendChild(element);
TextNodeExample03. htm
这个例子创建了一个新《3^元素并为它指定f值为-message•的class特性。然后，又创建了 一个文本节点，并将其添加到前面创建的元素中。最后一步，就足将这个元素添加到了文档的b〇dy 元素中，这样就可以在浏览器中宥到新创建的元素和文本节点了。

272 第 10 章 DOM
一般情况下，每个元素只冇一个文本f节点。不过，在某拽情况下也可能包含多个文本子节点，如 卜‘面的例子所示。
var element = document.createEIement("div"); element. className = "message** ;
vartextNodesdocument.createTextNoder'Helloworld!”）; element.appendChiId(textNode)?
var anotherTextKode s document.createTextNode(nYippeeIN); element.appendChiXd(aB〇ther?extNode);
document.body.appendChi]d{element ;
TextNodeExample04.htm
如果两个文本节点是相邻的同胞节点，那么这两个节点中的文木就会连起来m示，屮间不会有空格。
规范化文本节点
dom文档中存iia邻的同胞文本节点很容易导致混乱，w为分不淸哪个文本节点表示哪个字符串。 另外，DOM文捫中出现相邻文本节点的情况也不在少数，f•是就催牛.了一个能够将相邻文本节点合并 的方法。这个方法娃由Node类勸定义的（因而在所有节点类額中都存在），名叫normalized。如果 在一个包含两个或多个文本节点的父元素上调用normalize (方法，则会将所有文本节点合并成一个 节点，结果节点的nodeValvie等'f将合并前每个文本节点的nodeValue值拼接起来的值。来看一个 例子。
var element = document.createEIement{"div*); element .classNair.e = "message" ?
var textNode - document.createTextNode("Hello world!")? element.appendChild(tcxtNodo);
var anotherTcxtNodc - document.createTextNode("Yippee!")? element.appendChild(anocherTextNode);
document.body.appendChiId(element); alert(element.childNodes.length);	//2
element.normalize();
alert(element.childNodes.length);	//I
alert(element.firetCbild.nodeValue);	// "Hello world!Yippee I
TexlNodeExample05. him
浏览器在解析文档时永远不会创建相邻的文本节点。这种情况只会作为执行DOM操作的结果出现。
在某些情况下，执行normalized方法会导致IE6崩溃。不过，在1E6后来的 补丁中，可能已经修复了这个问题（未经证实）。IE7及更高版本中不存在这个问題。

10.1节点层次 273
3.分割文本节点
Text类S!提供了一个作用与normalize()相反的方法：splitText(。这个方法会将一个文本节 点分成两个文本节点，即按照指定的位置分割tiodevalue值。原来的文本节点将包含从开始到指定位 置之前的内容，新文木节点将包含剩下的文本。这个方法会返M -个新文本节点，该节点与原节点的 parenrNode相同。来看下面的例子。
var element = document.createElement("div"); clement.className = "message";
var textNode = document.createTextNode{"Hello world!*); element.appendChild(textNode);
document.body.appendChild(element);
var zxewNode 重 oleiBent»£ir8tC!ii!ULspl;LtText5; alert(element.CirstCbild.nodeValue);	//"Hello"
alert(newNode.nodeValue);	//H world!"
alert(element.childNodes.length);	//2
TextNodeExample06. him
在这个例子中，包含-Hello woirldt*1的文本节点被分割为两个文本节点，从位置5开始。位置5 是-Hello"和_world!-之间的空格，因此原来的文本节点将包含字符串••Hello",而新文本节点将包 含文本"world!M (包含空格)。
分割文本节点是从文本节点中提取数据的一种常用DOM解析技术。
10.1.5 Comment 类型
注释在DOM中是通过Comment类型来表示的。Comment节点具有下列特征：
nodeType 的值为 8;
nodeName 的值为-# comment:";
nodeValue的值是注释的内容;
parentNode 可能是 Document 或 Element;
□不支持（没有）子节点。
Comment类型与Text类®继承f!相同的基类，因此它拥有除SP1让Text()之外的所有字符串操 作方法。与Text类型相似，也n]■以通过nodeVaiue或data属性来取得注释的内容。
注释节点可以通过其父节点来访问，以下面的代码为例。
div id="myDi.v"!--A comment ~-/div
在此，注释节点是div元索的-•个子节点，w此可以通过下面的代码来访问它。
var div = document.getElGmentById("myDiv")? var comment = div.tirstchild; alert(comment.data);	//"A comment"
CommentNodeExampleOl. ktm

274 第 10 章 DOM
另外，使用document .createComment ()并为其传递注释文本也可以创建注释节点，如下面的例 子所示。
var comment = document .createCommcr.t  "A comment ");
M然，开发人M很少会创建和访问注释节点，W为注释节点对算法鲜有影响。此外，浏览器也不会 识别位于/htir.l.签后面的注释。如果要仿问沌释节点，一定要保证它们是ht:nl元素的后代（即 位于111：11\1和/111：1：11之间）。
0
在Firefojc、Safari、Chrome和Opera中，可以访问Commenc类型的构造函数和 原型。在IE8中，注释节点被视作标签名为V"的元素。也就是说，使用 getElementsByTagName)可以取得注释节点。尽管IE9没有把注释当成元素，但 它仍然通过--个名为HTMLCommentElement的构造函数来表示注释。
10.1.6 CDATASection 类型
CDATASection类型只针对基于XML的文挡，表示的是CDATA K域。与Comment类似， CDATASection类型继承自Text类型，因此拥冇除splitText()之外的所有字符串操作方法。 CDATASecCion节点具有下列特征：
nodeType 的值为 4;
nodeKame 的值为"ttcdata-section";
nodeValue的值是CDATAK域中的内容;
parentNode 可能是 Document 或 Element;
□不支持（没有）子节点。
CDATA区域只会出现在XML文档中，因此多数浏览器都会把CDATA K域错误地解析为Comment 或Element:。以下面的代码为例：
div id-*myDiv"xi [CDATAtThis is some concent.)] /div
这个例子中的div元索应该包含一个CDATASection节点。可是，四大主流浏览器无一能够这样 解析它。即使对于有效的XHTML页面，浏览器也没有正确地支持嵌人的CDATA区域。
在真正的XML文控中，可以使用document.createCDataSection()来创建CDATAK域，只需 为其传人节点的内容即可。
在Firefox、Safari、Chrome和Opera中，可以访问CDATASection类型的构造函 數和原型。IE9及之前版本不支持这个类型。
DocumentType 类型
DocumentType类型在Web浏览器中并不常用，仅有Firefox、Safari和Opera支持它' Document-
① Chrome 4.0 也支持 D〇cujnentlType 类型。

10.1节点层次 275
Type包含着与文档的doctype有关的所有信息，它具有下列特征：
nodeType 的值为 10;
nodeName的值为doctype的名称;
Q nodeValue 的值为 null;
parentNode 是 Document;
□不支持（没有）+节点。
在DOM】级中，DocumentType对象不能动态创建，而只能通过解析文档代码的方式来创建。支 持它的浏览器会把DocumentType对象保存在document. doctype中。DOM1级描述了 Documentiype对象的3个属性：name、entities和notations。其中，name表7TC文柄类型的名称; entities是由文档类型描述的实体的NamedNodeMap对象;notations是由文档类型描述的符号的 NamedNodeMap对象。通常，浏览器中的文档使用的都是HTML或XHTML文档类型，因而entities 和notations都是空列表（列表中的项来自行内文档类型声明）。但不管怎样，只有name属性是有用 的。这个属性中保存的是文档类型的名称，也就是出现4!D〇CTYPE之后的文本。以下面严格型HTML 4.0丨的文档类型声明为例：
!DOCTYPE HTML PUBLIC ■-//W3C//DTD HTML 4.01//EN"
"http：//www.w3,〇rg/TR/html4/strict.dtd*
DocumentType的name属性中保存的就是"HTML":
alert(document.doctype.name);	//"HTML"
IE及更早版本不支持DocumentType，因此document:.doctype的值始终都等于null。可是， 这些浏览器会把文档类型声明错误地解释为注释，并且为它创建一个注释节点。IE9会给 document.doctype赋正确的对象，但仍然不支持访问DocumentType类型。
10.1.8 DocumentFragment 类型
在所有节点类型中，只有DocumentFragment在文档中没有对应的标记。DOM规定文档片段 (document fragment)是种“轻M级”的文档，可以包含和控制节点，但不会像完整的文档那样占用 额外的资源。DocumentFragmerX节点具有下列特征：
nodeType 的值为 11;
nodeName 的值为"#document-fragment";
nodeValue 的值为 null;
parentNode 的值为 null;
□子节点可以是 Element、Processinglnstruction、Coitiment、Text、CDATASection 或 EntityReference〇
虽然不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将 来可能会添加到文档中的节点。要创建文档片段，可以使用document .createDocumentFragment )方 法，如F所示：
var fragment = document.createDocumentFragment();

276 第 10 章 DOM
文档片段继承了 Node的所有方法，通常用于执行那些针对文档的DOM操作。如果将文朽中的节 点添加到文杓片段中，就会从文档树中移除该节点，也不会从浏览器中再看到该节点。添加到文档片段 中的新节点同样也不属于文括树。可以通过appendChiId 〇或insertBefore (将文梢片段中内容添 加到文捫中。在将文档片段作为参数传递给这两个方法时，实际上只会将文档片段的所有了•节点添加到 相应位3上;文档片段本身永远不会成为文档树的一部分。来看下面的HTML示例代码：
ul id="inyList "/ul
假设我们想为这个^元素添加3个列表项。如果逐个地添加列表项，将会导致浏览器反复渲染(呈 现）新信息。为避免这个问题，可以像下面这样使用一个文档片段来保存创建的列表项，然后再一次性 将它们添加到文挡中。



var fragment = documenc.createDocunentFragment(); var ul = document.gecElementByld("myList")? var li = null;
for (var i=〇; i  3? i++){
li = document.createElement("li");
li.appendChild(document.createText:Node(”工tern " + (i+l))); fragment.appendChild(li);
ul.appendChild(fragment);
DocumentFragmentExampleOLhtm
在这个例子中，我们先创建一个文f{片段并取得丫对^11元素的引用。然后，通过for循环创建 3个列表项，并通过文本表示它们的顺序。为此，箱要分别创建li元素、创建文本节点，再把文本节 点添加到li元素。接着使用appendChildf)将11元素添加到文档片段中。循环结束后，再调用 appendOiildO并传人文档片段，将所有列表项添加到111元素中。此时，文档片段的所冇子节点都 被删除并转移到了 ul元素中。
10.1.9 Attr 类型
元素的特性在DOM中以Attr类型来表示。在所有浏览器中（包括IE8),都可以访问Attr类型 的构造函数和原型。从技术角度讲，特性就是存在于元素的attributes属性中的节点。特性节点具有 T列特征：
nodeType 的值为 11;
nodeName的值是特性的名称;
nodeValue的值是特性的值;
parentNode 的值为 null;
□在HTML中不支持（没有）子节点;
□在 XML 中子节点wj■以楚 Text 或 EnLityReference。
尽管它们也是节点，仉特性却不被认为是DOM文档树的一部分。开发人员最常使用的是getAt- tribute()、setAttributeU和 remveAttributeU方法，很少直接引用特性节点3
Attr对象有3个属性：name、value和specified。其中，name是特性名称（号nodeName的
值相同value是特性的值（与nodeValue的值相同），丨fil specified是一个布尔值，用以区别特
性是在代码中指定的，还是默认的。
使用document.createAttribute()并传人特性的名称可以创建新的特性节点。例如，要为兀素
添加align特性，可以使用下列代码：
, var attr = document.creaceAtcributei"align");
f atcr.value = "left";
element.setAttributeNode(attr);
alert(element.attributes["align-].value);
alert(element.getAttributeNode("align").value)
alert(element.getAttribute("align"));
//"left"
//"left"
//"left"
A ttrExampleOl. htm
这个例子创建了一个新的特性节点。由于在调用createAttributeO时Q经为name属性賦了值, 所以后面就不必给它賦值了。之后，又把value属性的值设置为”left"。为了将新创建的特性添加到 元素中，必须使用元素的setAttributeNodeO方法。添加特性之后，可以通过下列任何方式访问该 特性：attributes 属性、getAtt:ributeNode ()方法以及 getAttribute ()方法。其中，attributes 和getAttributeNode ()都会返回对应特性的Attr节点，而geLAttribute ()则只返问特性的值。
IU ■：： -： ：-：：■：：：;■ J ■ V.，;;	 —		 			
我们并不建议直接访问特性节点。实际上，使用getAttribute (}、 setAttributie (}和removeAttribute ()方法远比操作特性节点更为方便。
##  10.2 DOM操作技术
很多时候，DOM操作都比较简明，因此用JavaScript生成那钱通常原本是用HTML代码生成的内 容并不麻烦。不过，也有一些时候，操作DOM并不像表面t看起来那么简单。由于浏览器中充斥着隐 藏的陷阱和不兼容问题，用JavaScript代码处理DOM的某些部分要比处理其他部分更复杂一些。不过, 也有一些时候，操作DOM并不像表面上看起来那么简单。
###  10.2.1动态脚本
使用3£：1^^元素可以向页面中插人JavaScript代码，一种方式是通过其SrC特性包含外部文件， 另一种方式就是用这个元素本身来包含代码。而这一节要讨论的动态脚本，指的是在页面加载时不存在， 但将来的某一时刻通过修改DOM动态添加的脚本。跟操作HTML元素一样，创建动态脚本也有两种方 式：插人外部文件和直接插人JavaScript代码。
动态加载的外部JavaScript文件能够立即运行，比如下面的SCriPt元素：
script type="text/javascript" src-- "client•js_/script
这个SCript元素包含了第9章的客户端检测脚本。而创建这个节点的DOM代码如下所示：
var script = document.createElement("script"); script.type = •text/javascript";
script.src = "client.js"? document.body.appendChila(script);
显然，这里的DOM代码如实反映了相应的HTML代码。不过，在执行最后一行代码ftscript 元素添加到页面中之前，是不会下载外部文件的。也可以把这个元索添加到head元素中，效果相同。 整个过程可以使用下面的函数来封装：
function loadScripturl)(
var script = document.createElement("script"}; script.type = "text/javascript"; script.src = url?
document.body.appendChild{script);
)
然后，就可以通过调用这个函数来加载外部的JavaScript文件丫 ：
loadScript{"client.js");
加载完成后，就可以在页面中的其他地方使用这个脚本了。问题只有一个：怎么知道脚本加载完成 呢？遗憾的是，并没有什么标准方式来探知这一点。不过，与此相关的一些事件倒是可以派上用场，但 要取决于所用的浏览器，详细讨论请见第13章。
另一种指定JavaScript代码的方式是行内方式，如下面的例子所示：
script type=•text/javascript• function sayHi(){ alerC(*hi");
}
/script
从逻辑卜.讲，下面的DOM代码是有效的：
var script = document.createElement("script■)? script.type = "text/javascript"?
script.appendChild(document.createTextNode("function sayHi(){alert('hi')? ")); document.body.appendChiId{script)?
在Hrefox、Safari、Chrome和Opera中，这些DOM代码可以正常运行。但在IE中，则会导致错误。 1£将3^^1:视为一个特殊的元索，不允许DOM访问其子节点。不过，可以使用SCriPt元素的 text属性来指定JavaScript代码，像卜_面的例子这样：
```
var script = document.createElement("script");
script., type = "text/javascript-;
script.text s "function sayHi(){alert('hi');)";
document.body.appendChiId(script)?
```
DynamicScriptExample01.htm
经过这样修改之后的代码可以在IE、Firefox、Opera和Safari 3及之后版本中运行。Safari 3.0之前 的版本虽然+能正确地支持text M性，但却允许使用文本节点技术来指定代码。如果需要兼容早期版 本的Safari,可以使用下列代码：
```
var script = document.createElement("script");
script.type = "text/javascript"?
var code = "function sayHi)aX«rt(*hi');}M;
try {
script.appendChild(document.createTextNode(ncodett));
)catch (ex){
script.text ■ "code";
document.body.appsndChiId(script)?
```
这里，先尝试标准的DOM文本节点方法，因为除了 IE (在IE中会导致抛出错误），所有浏览器 都支持这种方式。如果这行代码抛出了错误，那么说明是IE,于是就必须使用text属性了。整个过程 可以用以下函数来表示：
function loadscriptstring(code){
var script = document.createElemenL("script"); script.type = "text/javascript"; try {
script.appendChild(document.createTextNode(code));
} catch (ex){
script.text = code;
}
document.body.appendChiId(script);
)
下面是调用这个函数的示例：
loadscriptstring("function sayHi(){alert 'hi*);}");
DynamicScriptExample02. him
以这种方式加载的代码会在仝局作用域中执行，而且当脚本执行后将立即可用。实际上，这样执行 代码与在全局作用域中把相同的字符串传递给eval ()是一样的。
###  10.2.2 动态样式
能够把CSS样式包含到HTML页面中的元素有两个。其中，1^1元素用于包含来自外部的文件, 而^¥16元素用于指定嵌人的样式。与动态脚本类似，所谓动态样式是指在页面刚加载时不存在的样 式;动态样式是在页面加载完成后动态添加到贞面中的。
我们以下面这个典型的1:11^元素为例：
clink rel =11 stylesheet _ type-" text /css * href-"styles .css" 
使用DOM代码nj以很容易地动态创建出这个元素：
var* link = document.createElement("link"); link.rel = "stylesheet"; link.type = "text/css*; link.href = "style.css";
var head = document.getElementsByTagName("head")[0]; head.appendChilci(link) ?
以上代在所有主流浏览器中都可以正常运行。X要注意的是，必须将link元素添加到head 而不是body元素，才能保证在所有浏览器中的行为一致。整个过程可以用以下函数来表示：
function loadStyles(url){
var link = docujnenc .createElement ("link");

280 第 10 章 DOM
link.rel = "stylesheet*;
1 ink. r.ype = " text/css"; link.href = url?
var head = document.getElementsByTagNanet^head")[0]; head.appendChild(link);
加载外部样式文件的过程是异步的，也就是加载样式与执行JavaScript代码的过程没有固定的次序。 一般来说，知不知道样式已经加载完成并不重要;不过，也存在几种利用亊件来柃测这个过程是否完成 的技术，这些技术将在第13章讨论。
另一种定义样式的方式是使Sstyle元素来包含嵌入式CSS,如下所示：
```
style type-"text/css" body (
background-color： red;
}
/style
var style = document.createElement("style"); style.type = "tcxt/css";
style.appendChildfdocunGnt.crcaLGTexLNode{"body{background-color：red}")); var head = document,getElementsByTagN細e(**head”[0】; head.appendChiId(style);
```
以h代码可以在Firefox、Safari、Chrome和Opera巾运行，在丨E屮贝丨j会报错。视为 -个特殊的、与scriPt类似的节点，不允许访问;«子节点。事实h, 此时抛出的错误与向ScriPt 元素添加子节点时抛出的错误相同。解决IE中这个问题的办法，就是访问元素的stylesheet属性， 该属性乂有一个cssText属性，可以接受CSS代码（第13章将进一步讨论这两个属性），如下面的例 子所示。
var style = document.createElement("style*)?
style.type = "cexc/css";
try{
style.appcndChiId(document.createToxtNode("boc3y{background-color:red}"))?  catch (ex){
style.atylesheet.csBText * "body{background-colorsired}"/
)
var head = document.geCElcmGntsByTagNaxnot'head") [0J ; head.appendChild(style);
与动态添加嵌入式脚本类似，承写后的代码使用了 tjy-catch语句来捕获IE抛出的错误，然沿再 使用针对IE的特殊方式来设g样式。因此，通用的解决方案如下。
调用loadStyles ()函数的代码如下所示:
loadStyles{"styles.css");
按照相同的逻辑，下列DOM代码应该是有效的:
```
DynamicStyleExampleOI. him
function loadStyleString(css){
var style = document.createElement{"style")?
style.type = "text/css"?
try{
style.appondChild(document.createTextNode(css)); } catch (ex){
style.stylesheet.cssText = css;
}
var head = document.getElementsByTagName("head")[0]; head.appendChild(style);
```
DynamicStyleExample02.htm
调用这个函数的示例如下:
```
loadStyleString{"body{background-color：red)";
```
这种方式会实时地向页由中添加样式，w此能够马上看到变化。
如果专门针对IE编写代码，务必小心使用stylesheet.cssText属性。在重用 同一个style元素并再次设置这个属性时，有可能会导致浏览器崩溃。同样，将 cssText属性设置为空字符串也可能导致浏览器崩溃。我们希望IE中的这个bug能 够在将来被修复。
###  10.2.3 操作表格
table元素是HTML中最复杂的结构之一。要想创建表格，一般都必须涉及表示表格行、单元格、 表头等方面的标签。由于涉及的标签多，Wlfli使用核心DOM方法创建和修改表格往往都免不了要编写 大量的代码。假设我们要使用DOM来创建下面的HTML表格。
table border=-l" width="100%" tbody
tr
tdCell 1,l/td tdCell 2,l/td /tr
tr
tdCell l,2/td tdCell 2(2/td /tr
/tbody
/table
要使用核心DOM方法创建这些元素，得笛要像下面这么多的代码:
//创建 table
var table = document.createBlement("table"); table.border =1; table.width = "lOO%";
//创建 Lbody
var tbody = document.createElement(•tbody■);
table.appendChi]d(tbody);
//创建第一行
var rowl = document.croateElement{*tr"); tbody.appendChild(rowl);
var celll_l = document.createEleinent ("td");
cel11_L.appendChiId(document.createTextNode("Cell 1,1")};
rowl.appendChild(celll_l);
var cel12—1 = document.createElement("td");
cel12_1.appendChiId(document.createTexLNode("Cel1 2,1"));
rowl.appendChild(cell2_l);
//创建第二行
var row?. = document .createElement (-tr") ? tbody.appendChiId(row2);
var cel11一2 = document.createElement(•td");
celll_2.appendChiId(document.createTextKode("Cell 1,2'));
row2,appendChild(celll_2);
var cell2_2= document.createElement("td");
cell2_2.appendChild{document.createTextKode("Cell 2;2"));
row2.appendChild(cell2„2);
//将表格添加到文档主体中
document.body.appendChiId(cable);
显然，DOM代码很长，还有点不太好懂。为了方便构建表格，HTMLDOM还为table、
和tr元素添加了一些属性和方法。
Stable元素添加的属性和方法如下。
caption:保存着对caption兀素（如果有）的指针。
口 tBodies:是一个Cbody元素的 HTMlXollection。
口 tFoot:保存着对tfoot元素（如果有）的指针。
tHead:保存着对chead元素（如果有）的指针。
rows:是一个表格中所有行的HTMLCollection。
createTHead():创建thead元素，将其放到表格中，返回引用。
creat:eTFootU:创建Cfoot:元素，将其放到表格巾，返回引用。
createCaption):创建caption元素，将其放到表格中，返回引用。
deleteTHead(丨：删除thead元素。
口 deleteTFoot():删除tfoot兀素。
deJ.eteCaption ()：删除capt:ion元素〇
Q deleteRow (pos):删除指定位置的行。
insertRow(pos):向rows集合中的指定位置插人一行。
为 化〇17元素添加的属性和方法如下。
rows:保存着tbody元素中行的 HTMLCollect:ion。
dele!:eRow(pos:删除指定位置的行。
insertR〇w(pos:向rows集合中的指定位置插人一行，返回对新插人行的引用。
为11元素添加的属性和方法如下。
Lbody
cells:保存着(：1：元素中单元格的mmCollection。
deleteCell (pos):删除指定位置的单兀格。
insertCelKpos):向cells集合中的指定位置插人一个单元格，返回对新插人单元格的引用。 使用这些属性和方法，可以极大地减少创建表格所需的代码数讨。例如，使用这些属性和方法可以
将前面的代码重写如K (加阴影的部分是重H后的代码)
```
//创建 table
var table =： document .createElement ("table"); table.border = 1; table.width = "100%";
//创建 tbody
var tbody = document.creat eElement{"t body *); tabic.appendChild(tbody);
//釗建第一行
tbody.insertRow(0);
tbody.row8[0】.insertCell^O);
tbody.rows【0] .cella【0] .appen!lChild{docume!it:.createTextNode("Cell 1,1"}); tbody.rows[0].insertCell(l);
tbody.rows【0].cells【11.appendChild(document.createTextNode("cell 2,1_});
//釗建第二行
tbody.insertRowl);
tbody.rows【1】•insertCell{0;
tbody.rows[l】.cellstO】•AppendCbild(document»cre&teT«xtNode("C6ll 1,2")); tbody.rovr8【l】 .insertcell1);
tbody.rowsfl】 .cellsfl】 •app8ndChild(document,cr0ateTextNode(**C6ll 2,2m)) /
//将表格洛加到文档主体中
docmnent.body.appendChjId{table);
```
在这次的代码中，创建table*tb〇dy：^代码没有变化。不同的是创建两行的部分，其中使用 了 HTMLDOM定义的表格属性和方法。在创建第一行时，通过tbody元素调用了 insertBoW)方 法，传人r参数〇~表示应该将插人的行放在什么位置上。执行这一行代码后，就会动创建一行并 将其插入到比〇办元素的位S 〇上，因此就可以马上通过tbody.rows [0】来引用新插人的行。
创建单元格的方式也十分相似，即通过(：!：元素调用insertcell (方法并传人放置单元格的位 界。然后，就可以通过tbody.rowstOl.cellslO]来引用新插人的单元格，因为新创建的单元格被插 人到了这一行的位置0上。

总之，使用这些属性和方法创建表格的逻辑性更强，也更容易右懂，尽管技术上这两套代码都足正 确的。
###  10.2.4 使用 NodeList
理解NodeList及“近亲” NamedNodeMap和HTMLCollection,是从整体上透彻理解DOM的 关键所在。这三个集合都是“动态的”;换句话说，每当文档结构发生变化时，它们都会得到更新。因 此，它们始终都会保存着最新、最准确的信息。从本质上说，所有NodeList对象都是在访问DOM文 档时实时运行的査询。例如，下列代码会导致无限循环：
```
var divs : document .getEler.entsByTagKarne ("div1*},
for {i=0; i  divs.length; i++){
div document .createElement ("div"); document.body.appendChiId(div);
}
```
第一行代码会取得文档中所冇(3^元索的HTKLCollection。由于这个集合是“动态的”，W此 只要有新div元素被添加到页面中，这个元索也会被添加到该集合中。浏览器不会将创建的所有集 合都保存在一个列表中，而是在下一次访问集合时#吏新集介。结果，在遇到上例中所示的循环代码 时，就会导致一个有趣的问题。每次循环都要对条件1  divs. length求值，意味着会运行取得所 有15^元素的査询。考虑到循环体每次都会创建一个.diV元索并将其添加到文档中，因此 divs.length的值在每次循环后都会递增。既然i和divs.] engnh每次都会同时递增，结果它们的 值永远也不会相等。
如果想要迭代-个NodeList,锻好是使用length M性初始化第二个变乩然后将迭代器与该变 M进行比较，如下面的例子所示：
```
var divs = document.gecElementsByTagName(■div"), i,
len,
div;
for (i=0, lensdivs• length; i  len; i++K
div - document.createElcment("div*); document.body.appendchild(div);
}
```
这个例子中初始化了第二个变量len。由于len中保存着对divs.length在循环开始时的一个快 照，W此就会避免上•个例子中出现的无限循环问题。在本牵演示迭代NodeList对象的例子中，使用 的都是这种更为保险的方式。  
—般来说，应该尽谢减少访问NodeList的次数。因为每次访问NodeList,都会运行一次基于文 档的査询。所以，可以考虑将从NodeList中取得的值缓存起来。
##  10.3 小结
DOM是语y中立的API,用于访问和操作HTML和XML文档。DOM1级将HTML和XML文档 形象地宥作•个M次化的节点树，可以使用JavaScript来操作这个节点树，进而改变底层文挡的外观和 结构。
DOM由各种节点构成，简要总结如下。
- [ ] 最基本的节点类铟是Node,用于抽象地表示文档中一个独立的部分;所有其他类型都继承自 Node〇
- [ ] Document类型表示整个文柯，是一组分爲节点的根节点。在JavaScript中，document对象足 Document的一个实例。使用document:对象，有很多种方式卩I以査询和取得节点。
- [ ] Element节点表示文朽屮的所有HTML或XML元素，可以用来操作这些元素的内容和特性。 
- [ ] 另外还有一些节点类型，分別表示文本内容、注释、文档类型、CDATA区域和文挡片段。  

访问DOM的操作在多数愔况卜'都很i£观，不过在处理3£^化^和的716元素时还是存在一些 复杂性。I丨丨F这两个元素分別包含脚本和样式倌息，W此浏览器通常会将它们与其他元素区别对待。这 些区别导致了在针对这些元索使用innerHTML时，以及在创建新元素时的一些问题。
理解DOM的关键，就是理解DOM对性能的影响。DOM操作往往足JavaScript程序中开销最大的 部分，（W因访问NodeLisC导致的问题为最多。NodeLisL对象都是“动态的”，这就意味着每次访问 NodeList对象，都会运行一次丧询。有鉴T此，姓好的办法就是尽录减少DOM操作。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter9.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter11.md)

