##  第12章 DOM2和 DOM3([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] DOM2和D0M3的变化
- [ ] 操作样式的DOM API
- [ ] DOM遍历与范闱  

DOM1级主要定义的是HTML和XML文档的底层结构。
DOM2和DOM3级则在这个结构的基础上引人f更多的交互能力，也支持了更高级的XML特性。为此，DOM2和DOM3
级分为许多模块（模块之间具有某种关联），分别描述了 DOM的某个非常具体的子集。这些模块
如下。
- [ ] DOM2级核心（DOMLevel2Core):在1级核心基础上构建，为节点添加了更多方法和属性。 □ DOMg级视图（DOM Level 2 Views ):为文档定义了基于样式信息的不同视图。
- [ ] DOMi级事件（DOM Level 2 Events ):说明了如何使用事件与DOM文档交互。
- [ ] DOM2级样式（DOM Level 2 Style):定义了如何以编程方式来访问和改变CSS样式信息。
- [ ] DOM2级遍历和范围（DOM Level 2 Traversal and Range ):引人了遍历DOM文档和选择其特定 部分的新接口。
- [ ] DOM2级HTML (DOMLevel2HTML):在1级HTML基础上构建，添加了更多属性、方法和
新接n。
本章探讨除“DOM2级事件”之外的所有模块，“DOM2级事件”模块将在第13章进行全面讲解。
(^^00\0级又增加了“乂?3出”模块和“加栽与保存”（1^0(1311(18^6)模块。这 些模块将在第丨8章讨论。
DOM 变化
DOM2级和3级的fl的在于扩展DOMAP丨，以满足操作XML的所有需求，同时提供更好的错误处 理及特性检测能力。从某种意义上讲，实现这一目的很大程度意味着对命名空间的支持。“DOM2级核 心”没有引人新类型，它只是在DOM1级的基础上通过增加新方法和新属性来增强了既有类型。“DOM3 级核心”间样增强了既有类型，但也引人了--些新类型。
类似地，“DOM2级视图”和“DOM2级HTML”模块也增强了 DOM接N,提供了新的属性和方 法。由于这两个模块很小，因此我们将把它们与“DOM2级核心”放在一起，讨论基本JavaScript对象 的变化。可以通过下列代码来确定浏览器是否支持这些DOM模块。
var supportsDOM2Core = d〇cument.implementation.hasFeature(*Core*/ ■2.0"); var supportsDOM3Core s document•implementation.hasFeaLure(*Core*, *3.0"); var supportsDOM2HTML = document.implementation.hasFeature(■HTML", "2.0"); var supportsDOM2Views = document. impleir.Gntation.hasFeature ("Views", "2.0"); var supportSDOM2XMI. = document.implementation.hasFeature ("XML", **2.0*);
(^\	本幸只讨论那些已经有浏览器实现的部分，任何浏览•器都没有实现的部分将不作
Y讨论。
###  12.1.1 针对XML命名空间的变化
有了 XML命名空间，不同XML文档的元素就可以混合在一起，共同构成格式良好的文档，而不 必担心发生命名冲突。从技术上说，HTML不支持XML命名空间，仴XHTML支持XML命名空间。 因此，本节给出的都是XHTML的示例。
命名空间要使用xmlns特性来指定。XHTML的命名空间是

http://www.w3.org/1999/xhtml,在任何 格式良好XHTML页面中，都应该将其包含在html元素中，如下而的例子所示。
html xmlns=*

http://www.w3.org/1999/xhCmi"
head
titleExample XHTML page/title
/head
body
Hello world I /body
/html
对这个例子而言，其中的所有元素默认都被视为XHTML命名空间中的元索。要想明确地为XML 命名空间创建前缀，可以使用xmlns后跟冒号，再后跟前缀，如下所示。
xhtml:html xmlns:xhtml=•

http://www.w3.org/1999/xhtml•
xhtml:head
xhtrol:titleExample XHTML page/xhtml:title
/xhtml:head
xhtml:body
Hello world!
/xhtml:body
/xhtml:html
这里为XHTML的命名空间定义了一个名为xhtml的前缀，并要求所有XHTML元素都以该前缀 开头。有时候为了避免不同语言间的冲突，也需要使用命名空间来限定特性，如下面的例子所示。
xhtml:html xmlns:xhtml*"http://www.w3,org/1999/xhtml•
xhtml:head
xhtml:titleExample XHTML page/xhtml:title
/xhtml:head
xhtml:body xhtBlicla8e«"hooie"
Hello world!
/xhtml:body
/xhtml:html
这个例子中的特性class带有一个xhtml前缀。在只基于一种语言编写XML文档的情况下，命 名空间实际上也没有什么用。不过，在混合使用两种语言的情况下，命名空间的用处就非常大了。来宥
一肴下面这个混合了 XHTML和SVG语言的文档:
hcml xmlns="http://www.w3,org/1999/xhtml"
head
titleExample XHTML page/title
/hcad
body
svg xmlnB*,'

http://www.w3.org/2000/svg* version*Ml. 1"
viowBox=n0 0 100 100" styles"width:100%; height:100%R
ract x=”0" y="0" width=_100"	style="fil1:red"/
/avg
/body
/html
在这个例子中，通过设置命名空间，将svg#识为了与包含文档无关的元素。此时，svg元素的 所有子元素，以及这些元素的所有特性，都被认为属于

http://www.w3.org/2000/svg命名空间。即 使这个文档从技术上说是一个XHTML文档，但W为有了命名空间，其中的SVG代码也仍然是有效的。
对于类似这样的文档来说，最有意思的亊发生在调用方法操作文档节点的情况下。例如，在创建一 个元素时，这个元素属于哪个命名空间呢？在查询一个特殊标签名时，应该将结果包含在哪个命名空间 中呢？ “DOM2级核心”通过为大多数DOM1级方法提供特定于命名空间的版本解决了这个问题。
Node类型的变化
在DOM2级中，Node类型包含下列特定于命名空间的属性。
localName:不带命名空间前缀的节点名称。
a namespaceURI:命名空间URI或者（在未指定的情况T是）null。
prefix:命名空间前缀或者（在未指定的情况下是）mill。
肖节点使用了命名空间前缀时，其nodeName等于prefix+":"+localName。以下面的文构为例:



html xmlns="http: //

www.w3 .org/1999/xhtml', head
titleExample XHTML page/citle
/head
body
8:8v? xmlns:s=Hhttp://www-.w3.org/2000/svgN veraion=MX.l"
vimfB〇x="0 0 100 100" Btyle«Nwidth2100%; height1100%N
s:rect x=*0” 分篇_0_ width*_100" heigbt="100* style="fill:red"/
/s:svg
/body
/html
NamespaceExample.xml
对于加1〇1兀素来说，它的 localName 和 tagName 是化1:1111" ,namespaceURI 是-http: //www. w3.org/1999/xht:inl",而 prefix 是 null。对于s:svg元素而言，它的 localName 是-svg"， tagName 是"s : svg”，namespaceURI 是•丨http: "

www.w3 • org/2000/svg",而 prefix 是"s" 〇 DOM3级在此基础上更进一步，又引人广F列与命名空间有关的方法D
isDefaultNamespacefna/nespaceUKJ):在指定的/3ainespacet7i?J:是+当前节点的默认命名空 间的情况下返回true。
lookupNamespaceURI (prefix):返回给定 prefix 的命名空间。
Q lookupPrefixnamespacet/RI):返回给定 namespaceORJ 的前缀。
12

308 第 12 章 DOM2 和 DOM3
针对前面的例子，可以执行下列代码:
alert(document.body.isDefaultNamespace("http://www.v;3.org/1999/xhLml"); //true
//假设svg中包含着对s:svg的？丨用
alert{svg.lookupPrefix("

http://www.w3.org/2000/svg")); //"s" alert(svg.lookupNamespaceURI("s*)); //"hctp://

www.w3.org/2000/svg"
在取得了一个节点，但不知道该节点与文档其他元素之间关系的情况下，这些方法趄很有用的。
Document类型的变化
DOM2级中的Document类型也发生了变化，包含了下列与各名空间有关的方法。
createElementNStnaznespaceyKI，tagWame}:使用给定的 CagWame 创建一个属于命名空 间namespaceORT的新元素。
 createAttributeNS(na/nespaceyi?工，attributeName) ••使III给定的 attributeWame 仓ll 建一个属于命名空间namespaceURI的新特性。
口 getElementsByTagNameNS (namespaceyi?!:/ tafirWame):返问属于命名空间 na/nespacelWI 的 tagWa/ne 兀素的 NodeListo
使用这些方法时需要传人表示命名空间的URI (而不是命名空间前缀），如下面的例子所示。
//创建一个新的SVG元索
var svg = document.createElementNS("http://

www.w3.org/2000/svg","svg")?
//创建一个属于茱个命名空间的新特性
var att = document.createAttributeNS("http://www,somewhere.com", "random");
//取得所有XHTML元素
var oloms = document.gctElcmontsByTagNamoNS("hccp://

www.w3.org/1999/xhcml*#
只冇在文档中存在两个或多个命名空间时，这些与命名空间有关的方法才是必需的。
Element类型的变化
“DOM2级核心”中有关Element的变化，主要涉及操作特性。新增的方法如下。
getAttributeNS(■namespaceC/RUocalWa/ne):取得属于命名空间 namespaceUSI 且名为 locaiWajne 的特性。
getActributeNodeNS (na/nespaceUi?I，JocaJiVame):取得属于命名空间 narm?spacet//?I 且 名为的特性节点。
getElementsByTagNarneNS {iia^nespacettRI, tagWa/ne):返回属名空间 namespacet/KI" 的 fcagWaine 兀素的 NodeList。
口 hasAttributeNSfnajnespaceC/RJ^JocaZWaine):确定?^前元素是否冇一个名为 localName 的特性，而且该特性的命名空间是namespacelW2。注意，“DOM2级核心”也增加了一个 hasAttribute ()方法，用于不考虑命名空间的情况。
removeAttriubteNS (naTnespacet/KI', Joca_ZiVa/ne :删除属于命名空间 name印aceORI 且名 为 2oca_2_Wa;ne 的特性。
 setAttributeNS (namespacel/RI, quali fiedName^ value) :	namespace -
且名为guahfiedi你ne的特性的值为vaiue。
口 setAttributeNodeNS(aCtWode:设置厲于命名空间 najnespaceURT 的特性节点。

12.1 DOM 变化 309
除了第一个参数之外，这些方法与DOM1级中相关方法的作用相N;第一个参数始终都是一个命 名空间URI。
4. NamedNodeMap类型的变化
NainedNodeMap类型也新增了下列与命名空间有关的方法。由于特性是通过NamedNodeMap表示 的，W此这些方法多数情况下只针对特件使用。
getMamedItemNS(na/nespacet/i?I, localWame):取得属于命名空间 na7nespace£/RI 且名为 localName
 removeNamedltemNS i7amespaceC/RJ,Joca!Wame):移除属于命名空间	且名 为 _ZocaJWame 的项。
setNamedltemNS (node):添加node,这个节点已经亊先指定了命名空间信息。
由于一般都是通过元素访问特性，所以这些方法很少使用。
12.1.2其他方面的变化
D0M的其他部分在“D0M2级核心”中也发生了一些变化。这些变化与XML命名空间无关，而是 更倾向于确保API的可靠性及完整性。
1. DocumentType类型的变化
DocumentType 类型新增了 3 个属件:publicld、systemld 和 internalSubset。其中，前两 个庙性表示的是文档类■声明中的两个信息段，这两个信息段在DOM1级中是没有办法访问到的。以 下面的HTML文档类型声明为例。
!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
"

http://www.w3.org/TR/htmi4/strict.dtd"
对这个文档类®声明而言，publicld是_-//W3C//DTDHTML 4.01//EN_,[flfsystemId是-http: //www• w3 .org/TR/html4/strict .dt:d_。在支持DOM2级的浏览器中，应该可以运行下列代码。
alert{document.doctype.publicld); alert(documGnt.doctype.systemld);
实际h，很少需要在网页中访问此类倌息。
最后一个属性internalSubset,用丁•访问包含在文档类型声明中的额外定义，以下面的代码为例。
!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN' "http://

www.w3.org/TR/xhtmll/DTD/xhtmll-strict.dtd"
(!ELEMENT name (#PCDATA)) 
访问 document.doctype.internalSubset 将得到"!ELEMENT name {#PCDATA}_0 这种内部 子集（internal subset)在HTML中极少用到，在XML中可能会更常见一些。
Document类型的变化
Document类型的变化中唯一与命名空间无关的方法是importNode ()。这个方法的用途是从一个 文档中取得•个节点，然后将其导入到另一个文档，使其成为这个文档结构的-部分。需要注意的是， 每个节点都冇•个ownerDocument属性，表示所属的文档。如果调用appendCMld()时传人的节点 属于不N的文档（ownerDocument属性的值不一样），则会导致错误。但在调用importNode (时传人 不同文杓的节点则会返回一个新节点，这个新节点的所有权归当前文档所宥。
说起来，importNodeU方法与Element的cloneNode()方法非常相似，它接受两个参数:要复
12

310 第 12 章 DOM2 和 DOM3	
制的节点和-个表示是否复制T-节点的布尔值。返冋的结果是原来节点的副木，但能够在当前文档中使 用。来者F面的例子:
var newNode = document. importNode (oldNode, true) ; //导入节点及其所有子节点
document.body.appendChild(newNode);
这个方法在HTML文档中并不常用，在XML文档中用得比较多（更多讨论请参见第18章)。
“DOM2级视图”模块添加了一个名为defaultView的届性，其中保存着一个指针，指向拥有给 定文档的窗口（或框架)。除此之外，“视图”规范没冇提供什么时候其他视图可用的信息，W而这是唯 一一个新增的屈性。除IE之外的所有浏览器都支持defaultview属性。在IE中有一个等价的属性名 叫parentWindowCOpera也支持这个属性)。因此，要确定文档的归M窗门，可以使用以下代码。
var parentWindow = document.defaulcview I 1 document.parentWindow;
除f 1:述一个方法和一个厲性之外，“DOM2级核心”还力document • implementation对象规定了 两个新方法:createDocuxnentType ( 和 createDocument)。前者用于创建一个新的 DocumentType 节点，接受3个参数:文档类型名称、publicld、systeiuld。例如，下列代码会创建•个新的HTML
Strict文档类型。
var doctype = document. iniplementation. createDocumentType ("html" #
"-//W3C//DTD HTML 4.01//EN",
"http: //www. w3 . org/TR/htrai4/strict .dtd11);
由于既有文档的文括类型不能改变，因此createDocumentType()只在创建新文档时有用;创建 新文约时需要用到creatcDocumentU方法。这个方法接受3个参数:针对文样中元素的namesp， aceURI、文档元素的标签名、新文档的文档类型。下面这行代码将会创建一个空的新XML文捫。
var doc = document.implementation.createDocument{mn, "root", null);
这行代码会创建一个没存命名空间的新文档，文档元素为r〇〇t,而且没有指定文裆类瑠。要想 创建一个XHTML文档，可以使用以下代码。
var doctype = doeximent .implementation.createDocurr.entType ("htni",
"-//W3C//DTD XHTML 1.0 Strict//ENB,
"

http://www.w3.org/TR/xhtmll/DTD/xhtmll-strict.dtd");
var doc = document. implementation.createDocument ("http://

www.w3 .org/1999/xhtrnl*',
"html", doctype);
这样，就创建了一个带有适当命名空间和文档类型的新XHTML文档。不过，新文档当前只有文档 元素剩下的所有元素都需要继续添加。
“DOM2级 HTML” 模块也为 document • implementation 新增了一个方法，名叫 createHTML- DocumentO。这个方法的用途是创建一个完整的HTML文档，包括html、head、ctitle^U body元素。这个方法只接受一个参数，即新创建文档的标题（放在1^^元素中的字符串），返回 新的HTML文档，如下所示:
var htmldoc = document .iirplemer.tation.createHTKLDociment ("New Doc"); alert: (htmldoc.citie) ;	//"Now Doc•
alGrctypeof hcmldoc.body);	//"object"
CreateHTMLDocumentExampie.htm

12.1 DOM 变化 311
通过调用createHTMLDocumentO创建的这个文抒，是HTMLDocument类型的实例，因而具有该 类艰的所有厲性和方法，包括title和body属件。只有Opera和Safari支持这个方法。
Node类型的变化
Node类型中唯一与命名空间无关的变化，就是添加了 isSupported U方法。与DOM1级为document. implementation 引人的 hasFeature () 方法类似， isSupportedO 方法用于确定肖前节点具有 什么能力。这个方法也接受相N的两个参数:特性名和特性版本号。如果浏览器实现了相应特性，而且 能够基于给定节点执行该特性，isSupported()就返回true。来看一个例子:
if (document.body.isSupported("HTML", "2.0")){
//执行只有_DOM2级HTML •才支持的操作
)
由于不同实现在决定对什么特性返回true或false时并不一致，这个方法同样也存在与hasFeat;ure(} 方法相N的问题。为此，我们建议在确定某个特性是否可用时，最好还是使用能力检测。
DOM3级引人了两个辅助比较节点的方法:isSameNode(r和isEqualNodeO。这两个方法都接受 一个节点参数，并在传人节点与引用的节点相同或相等时返冋true。所谓相同，指的是两个节点引用的 是同一个对象。所谓相等，指的是两个节点是相同的类型，具有相等的属性（nodeName、nodeValue, 等等)，而fi它们的attributes和childNodes属性也相等（相同位K包含相同的值)。来看一个例子。
var divl = document.createElement("div*); divl.setAttribute("class", "box");
var di.v2 = document .createElement ("div"); div2.secAttribute("class", "box");
alert(divl.isSameNode(divl))? //true alert(divl.isEqualNode(div2)); //true alert(divl.ieSameNode{div2)); //false
这里创建了两个具有相同特性的div元素。这两个元素相等，佴不相同。
DOM3级还针对为DOM节点添加额外数据引人了新方法。其中，setUscrData (方法会将数据指 定给节点，它接受3个参数:要设置的键、实际的数据（可以是任何数据类型）和处理函数。以下代码 可以将数据指定给一个节点。
document.body.setUserData("name•, "Nicholas", function(){});
然后，使用getUserDataU并传人相同的键，就可以取得该数据，如下所示: var value = document.body.getUserDataCnajr.e");
传人setUserDataO中的处理函数会在带有数据的节点被复制、删除、重命名或引人一个文档时 调用，丨村而你可以亊先决定在上述操作发生时如何处理用户数据。处理函数接受5个参数:表示操作类 塑的数值（1表示复制，2表示导人，3表示删除，4表示電命名）、数据键、数据值、源节点和目标节 点。在删除节点时，源节点是null;而在复制节点时，P标节点是null。在函数内部，你可以决定如 何存储数据。来宥下面的例子。
var div = document.createElement("div");
1 div.sotUserData{"name", "Nicholas' function(operation, key, value, src( dest){ if (operation == 1){
dest.setUserData(key, value, function(){});	)
12

312 第 12 章 D0M2 和 D0M3
});
var newDiv = div.cloneNode(true);
alert{newDiv.getUserData("name"));	//"Nicholas
LkerDataExampk.htm
这里，先创建了一fdiv元素，然后乂为它添加了一些数据（用户数据)。在使用cloneNodeO 复制这个元素时，就会调用处理函数，从而将数据自动复制到了副本节点。结果在通过副本节点调用 getUserData ()时，就会返间与原始节点中包含的相同的值。
框架的变化	_
框架和内嵌框架分別用HTMLFraitieElement和HTMLIFrameElement表水,它们在DOM2级中都有 了一个新属性，名叫contentDocument。这个属性包含•-个指针，指向表示框架内容的文桂对象。在此 之前，无法6:接通过元素取得这个文档对象（只能使用frames集合)。可以像下面这样使用这个属性。



var iframe = document.getElementByldCmylframe-);
var iframeDoc = iframe.contentDocument;	//在 IE8 以前的版表中无效
IFrameElementExampie.htm
由T contentDocument属性是Document类型的实例，因此可以像使用其他HTML文們一样使 用它，包括所有属性1(丨方法。Opera、Firefox、Safari和Chrome支持这个属性。IE8之前不支持框架中 的contencDocument属性，似支持一个名叫contentWindow的域性，该属性返tlj框架的window对 象，而这个window对象乂存一个document属性。因此，要想在上述所有浏览器中访问内嵌框架的文 档对象，可以使用下列代码。
var iframe = document.getElemeritById("myIfrair.e,');
var iframeDoc = iframe.contentDocuinent II iframe.contentWindow.document;
IFrameElementExample2.htm
所有浏览器都支持contentWindow属性。
f
	访问框架或内嵌框架的文档对象要受到跨城安全策略的限制。如果某个框架中的
页面来自其他域或不同子域，或者使用了不同的协议，那么要访问这个枢架的文档对
象就会导致错谋。
##  12.2 样式
在HTML中定义样式的方式有3种:通过link/元素包含外部样式表文件、使用^716/元素 定义嵌人式样式，以及使用style特性定义针对特定元素的样式。“DOM2级样式”模块围绕这3种应用 样式的机制提供了一套API。要确定浏览器是杏支持DOM2级定义的CSS能力，'nj■以使用下列代码。
var supportsD0M2CSS = docroment. implementation.hasFeature (*,CSSt,, -2.0"); var supportsDOM2CSS2 = document.implementation.hasPcaturc("CSS2 *, "2.0")?

12.2 样式 313
12.2.1访问元素的样式
任何支持style特性的HTML元素在JavaScript屮都有一个对应的style厲性。这个style对象 是CSSStyleDeclaration的实例，包含着通过HTML的style特性指定的所有样式信息，但不包含 与外部样式表或嵌人样式表经叠而来的样式。在style特性中指定的任何CSS属性都将表现为这个 style对象的相应M性。对于使用短划线（分隔不同的词汇，例如background-image)的CSS属性 名，必须将其转换成驼峰大小写形式，才能通过JavaScript来访问。下表列出了几个常见的CSS属性及 其在style对象中对应的属性名。
多数情况下，都可以通过简单地转换属性名的格式来实现转换。其中一个不能直接转换的CSS属性 就是float。由于float是JavaScript中的保留字，因此不能用作属性名。“D0M2级样式”规范规定 样式对象上相应的属性名应该是cssFloat; Firefox、Safari、Opera和Chrome都支持这个属性，而IE 支持的则是styleFloat。
只要取得一个有效的DOM元素的引用，就可以随时使用JavaScript为其设置样式。以下是几个例子。
var myDiv = document.getE丄ementById(*myDiv*}?
//设里背景颜色
myDiv.style.backgroundColor = "red";
//改变大小
myDiv.style.width = _100px_; myDiv.style.height = *200px*;
//指定边框
myDiv.style.border = "lpx solid black";
在以这种方式改变样式时，元素的外观会肖动被更新。
在标准模式下，所有度量值都必须指定一个度量单位。在混杂模式下，可以将 style.width设置为lO'浏览器会假设它是•ZOpx*;但在标准模式下，将
style.width设置为"20•会导致被忽略	因为没有度量单位。在实战中，最好始
终都指定度量单位。
通过style对象同样可以取得在style特性中指定的样式。以下面的HTML代码为例。
div id="myDiv" style= "background-color :blue? width: lOpx; height :25px"x/div
在style特性中指定的样式信息可以通过下列代码取得。
alert(myDiv.style.backgroundColor);	//"blue"
alert(myDiv.style.width);	//*10px"
alert(myDiv.style.height);	//*25px*

314 第 12 章 DOM2 和 DOM3
如果没有为元素设许style特性，那么style对象中可能会包含-些默认的值，但这些值并不能 准确地反映该元素的样式信息。
DOM样式属性和方法
“DOM2级样式”规范还为style对象定义了一些属性和方法。这些属性和方法在提供元素的style 特性值的同时，也可以修改样式。下面列出了这些属性和方法。
cssText:如前所述，通过它能够访问到style特性中的CSS代码。
length:应用给元素的CSS属性的数量。
parentRule:表7TCCSS信息的CSSRule对象。本节后面将讨论CSSRule类型。
getPropertyCSSValue(propertyWame):返冋包含给定属性值的 CSSValue 对象。
getPropertyPriority(propertyWaj7ie:如果给定的属性使用了！ important 设®，则返回 "iir.portant";否则，返回空宁符串。
口 getPropertyValue(propertyWame):返回给定属性的字符串值。
item (index):返冋给定位置的CSS属性的名称。
removeProperty(propertyWame):从样式中删除给定属性。
setProperty(propertyWajne,vaiue,priority):将给定属性设置为相应的值，并加上优先 权标志（"important“或者一个空字符串）。
通过cssText属性可以访问style特性中的CSS代码。在读取模式下,cssText:返回浏览器对style 特性中CSS代码的内部表示。在写人模式下，賦给cssText的值会重写整个style特性的值;也就是 说，以前通过style特性指定的样式信息都将丢失。例如，如果通过style特性为元素设置了边框， 然后再以不包含边框的规则重写cssText,那么就会抹去元素上的边框。下面是使用cssText属性的 一个例子。
myDiv.style.cssText = "width: 2bpx; height: lOOpx; background-color: green"? alert(myDiv.style.cssText);
设置cssText是为元素应用多项变化最快捷的方式，因为可以一次性地应用所有变化。
设汁length属件的S的，就是将其与item )方法配套使用，以便迭代在元素中定义的CSS属性。 在使用length和item)时，style对象实际上就相当于一个集合，都可以使用方括号语法来代替 item (来取得给定位®的CSS届性，如下面的例子所示。
for (var i=0, len=myDiv.style.length; i  len; i++){ alert {myDiv.style[i] ?//或者 ntyDiv.style.item(i
}
无论是使用方括号语法还是使用item()方法，都可以取得CSS属性名（"background-color", 不是”backgroundColorM )。然后，就可以在getPropertiyValue ()中使用了取得的厲性名进一步取 得属性的值，如下所示。	■
var prop, value, 1, len;
for (i=0, len=myDiv.style.length; i  len; i++){
prop - my-Div.style [i] ;	//或者 myDiv. style .item (i)
value ■ 〇vXiv*0tyle.9«tPropertyValue(pr^); alert(prop + » ; ■ + value);
}
getPropertyValueU方法取得的始终都是CSSW性值的字符串表示。如果你需要更多信息，可 以使用getPropertyCSSValue()方法，它返回一个包含两个属性的CSSValue对象，这两个属性分

12.2 样式 315
别是:cssText 和 cssValueType。其中，cssText 属性的值SgetPropertyValueU返冋的值相同. 而cssValueType属性则是--•个数值常M,表示值的类0表示继承的值，1表示基本的值，2表示 值列表，3表示自定义的值。以下代码既输出CSS属性值，也输出值的类型。
var prop, value, len;
for (i=0, len=myDiv.style.length; i  len; i++){
prop = myDiv.style [il ; //或者 myDiv.style. item(i value = rayDiv.style.getPropertyCSSValue(prop);
alertprop + n : • + value.cssText + ■ (" + value.cosValueType +	;
DOA4StyieObjectExample.htm
在实际开发中，getPropertyCSSValueU 使用得比 getPropert:yValue()少得多。IE9+、Safarie 3+以及Chrome支持这个方法。Firefox 7及之前版本也提供这个访问，但调用总返回null。
要从元素的样式中移除某个CSS属性，需要使用removePropertyU方法。使用这个方法移除一 个属性，意味着将会为该属性应用默认的样式（从其他样式表经层叠而来)。例如，要移除通过style 特性设置的border属性，可以使用下面的代码。
rayDiv.style.removeProperty("border■);
在不确定某个给定的css属性拥有什么默认值的情况下，就可以使用这个方法。只要移除相应的属 性，就可以为元素应用默认值。
除非另有说明，本节讨论的属性和方法都得到了丨E9+、Firefox、Safari、Opera9+
以及Chrome的支持。
计算的样式
虽然style对象能够提供支持style特性的任何元素的样式信息，它不包含那些从其他样式表 层叠而来并影响到当前元素的样式信息。“DOM2级样式”增强了 document.defaultView,提供了 getComputedStyleU方法。这个方法接受两个参数:要取得计算样式的元素和一个伪元素字符串（例 如-:after")。如果不需要伪元素信息，第二个参数可以是null。getComputedStyle(方法返回一 个CSSStyleDeclaration对象（与style属性的类型相N ),其中包含当前元素的所有计算的样式。 以下面这个HTML页面为例。
```
!DOCTYPE html
html
head
titleComputed Styles Example/title
style type="text/css" tmyDiv {
background color: blue; width: lOOpx; height: 200px;
}
/style
/head
body
div id=*myDiv* style="background-color: red; border: lpx solid black**/div /body
/html
```
应用给这个例子中^^元素的样式一方面来Q嵌入式样式表（style兀素中的样式），另一方 面來自其style特性。但是，style特性中设S了 backgroundlColor和border，没有设ft width 和height,后者是通过样式表规则应用的。以下代码可以取得这个元素计算后的样式。



var myDiv = dociiinent.geCElementByldfinyDiv") ?
var computedStyLe = document.dofaultView.gecCoir.putedStyle(myDiv, null);
alert(computedScylo.backgroundColor)? alert(computedScyle.width); alert(computedScyle.height); alert(computedStyle.border);
// "red”
// "lOOpx"
// "200px”
//在某些浏見器中是-lpx solid black"
ComputedStylesExample. htm
在这个元素i丨嘴后的样式中，背景颜色的值是"red»，宽度值是nOOpK•，高度值是^OOpx"。我 们注意到，背景颜色不是"blue",丙为这个样式在fl身的style特性中已经被覆盖了。边框属性能 会也可能不会返W样式表中实际的border规则（Opera会返回，但其他浏览器不会)。存在这个差别的 原因是不同浏览器解释综合（rollup )属性（如border )的方式不同，因为设H这种属性实际上会涉及 很多其他属性。在设置border时，实际上是设置了四个边的边框宽度、颜色、样式属性 (border-left-width % border-top-color、border-bottom-style ,等等）〇 因此，.即使 computers- tyle.border 不会在所有浏览器中都返冋值，似 computedStyle.borderLeftWidth 则会返问值。
需要注意的是，即使有些浏览器支持这种功能，但表示值的方式可能会有所区别。 1例如，Firefox和Safari会将所有颜色转换成RGB格式（例如红色是rgb(255,0,0 )。 因此，在使用getComputedStyle()方法时，最好多在几种浏览器中測试一下6
IE不支持getComputedStyle()力'法，但它冇一种类似的概念。在IE中，每个具冇styleM性 的元素还朽一个currentStyle M性。这个属性是CSSStyleDeclaration的实例，包含^前元索全 部计算后的样式。取得这些样式的方式也差不多，如下_的例子所示。
var myDiv = document. gctEiementByld (BrcyDiv"); var computedStyle » myDiv.currentStyle;
alert(oomputedStyle.backgroundColor);
alert(computedStyle.width);
alert(compuCedStyle.height)?
alert(corapuCedStylG.border);
//"red"
//"lOOpx"
//fl200px"
//undefined
lEComputedStylesExample. htm
与DOM版本的方式一样，IE也没有返冋border样式，因为这是一个综合属性。
无论在哪个浏览器中，呆道要的一条是要记住所有计算的样式都是只读的;不能修改计算后样式对 象中的CSS属性。此外.计算后的样式也包含属于浏览器内部样式表的样式信息，因此任何具右默认值 的CSS属性都会表现在计算后的样式中。例如，所有浏览器中的visibility属性都冇-个默认值， 但这个值会因实现而异。在默认情况下，宥的浏览器将visibility属性设置为"visible'而宥的 浏览器则将其设置为"inherit•。换句话说，不能指堵某个CSS M性的默认值在不同浏览器中是相同 的。如采你需要元素具有某个特定的默认值，应该手丁在样式表中指定该值。

12.2 样式 317
12.2.2操作样式表
CSSStyieSheeC类型表示的是样式表，包括通过link元素包含的样式表和在311^兀素中定义 的样式表。有读者可能id得，这两个元素本身分别是由HTMLLi'nkElement和™StyleElement类塑 表示的。们.是，CSSStyleSheeL类相对更加通用一些，它只表示样式表，而不管这些样式表在HTML 中是如何定义的。此外，丨:述两个针对元索的类塑允许修改IITML特性，但CSSStyleSheet对象则是-- 套只读的接口（有一个属性例外)。使用下面的代码可以确定浏览器是否支持COM2级样式表。
var supportsD0M2StyleSheets =
docurr.ent. implementation.hasFGaturc ("Stylesheets", ■ 2.0•);
CSSSLyleSheet继承H Stylesheet,后者nj以作为一个基础接门来定义非CSS样式表。从 Stylesheet接I丨继承而来的屈性如下。
disabled:农示样式表是否被禁用的布尔值。这个属性是可读泻的，将这个值设置为true可 以禁用样式表。
href:如果样式表是通过linkfe含的，则是样式表的URL;否则，是null。
media:当前样式表支持的所冇媒体类型的集合。与所存DOM集合一样，这个集合也有一个 length属性和一个itenU)方法。也可以使用方括号语法取得集合中特定的项。如果集合是空 列表，表示样式表适用丁-所有媒体。在IE中，media是•一个反映link^flstyle元索media 特性值的字符串。
ownerNode:指向拥有当前样式表的货点的指针，样式表可能是在HTML中通过link;^ style/引入的（在XML中可能是通过处理指令引人的)。如果当前样式表是其他样式表通过 ©import导入的，则这个属性值为null。不支持这个属性。
parentStyleSheet:在治前样式表是通过@import导入的情况下，这个属性是一个指向导人 它的样式表的指针。
title: ownerNode 中 tidle M性的值。
type:表7TC样式表类灌的字符串。对CSS样式表而言，这个字符
除丫 disabled属性之外，其他厲性都是只读的。在支持以上所有这些属性的基础卜.， CSSStyleSheet类型还支持下列属性和方法:
cssRules:样式表中包含的样式规则的集合。1E不支持这个M性，但有一个类似的rules属性。
ownerRule:如果样式表造通过@import导人的，这个属性就是一个指针，指向表示导人的规 则;否则，值为null。^不支持这个属性。
deleteRule(index:删除cssRules集合中指定位置的规则。IE不支持这个方法，但支持 •一^类似的 rereoveRule )方法。
insertRule (rule, index):向cssRules集合中指定的位置插人ruie字符电。丨E不支持这 个方法，倂支持一个类似的addRule 〇方法。
应用于文們的所有样式表是通过document. styleSheeCs集合来表示的。通过这个集合的length M性吋以获知文杓中样式表的数最，而通过方括号语法或item()方法可以访问每-个样式表。來看一个 例了-。
var sheet = null;
for (var	len=document.stylesheets.. 1 ength; i  len; i++) {

318 第 12 章 D0M2 和 DOM3
sheet = document•stylesheets⑴; alert: (sheet .href);
StyleSheetsExample. htm
以上代码可以输出文杓中使用的每一个样式表的href属性（style元素包含的样式表没衧 href厲性)。
不同浏览器的document.stylesheets返M的样式表也不同。所有浏览器都会包Astyle兀素 和reJL特性被设置为-stylesheet"的Iink元素引入的样式表。IE和Opera也包含rel特性被设置为 "alternate stylesheet"的clinloX索引入的样式表。
也可以直接通过link^styleX素取梅CSSStyleSheet对象。DOM规定了一个包含 CSSStyleSheet对象的厲性，名叫sheet;除丫 IE,其他浏览器都支持这个属性。IE支持的是 stylesheet属性。要想在不同浏览器中都能取得样式表对象，可以使用下列代码。
function getStyleSneet(element)(
return elexent. sheet I I elerr.enc. styleSheec;
) •
//取得第一个link/元素以入的样式表
var link = docuir‘enc.getElemer_tsByTagNane("link”【0];
var sheet = getStylesheet(link)?
StykSheetsExamph2.htm
这里的getStylesheet (返W的样式衷对象与document • sty] eSheets集合中的样式表对象相丨司。
1. CSS规则
CSSRule对象表示样式表中的每-条规则。实际h, CSSRule是-个供其他多种类型继承的基类 型，其屮坡常见的就是CSSStyleRule类型，表示样式倍息（其他规则还有@111^〇1:1;、@font-face、 @page *@charset,但这些规则很少有必要通过脚本来访问）。CSSStyleRule对象包含F列属性。
cssText:返M整条规则对应的文本。由于浏览器对样式表的内部处理方式不M,返M的文本 可能会与样式表中实际的文本不一样;Safari始终都会将文本转换成全部小写。丨£不支持这个 属性。
parentRule:如果肉前规则是导人的规则，这个属性引用的就是导人规则;否则，这个值为 null。IE不支持这个属性。
parentStyleSheet: 3前规贝丨〗所屈的样式表。IE不支持这个属性c
selectorText:返H当前规则的选择符文本„由于浏览器对样式表的内部处理方式不同，返M 的文本吋能会与样式表中实际的文本不一样（例如，Safari 3之前的版本始终会将文本转换成全 部小写)。在Firefox、Saferi、Chrome和IE中这个属性是只读的。Opera允许修改selectorText:。
style:-个CSSStyleDeclaration对象，可以通过它设置和取得规则中特定的样式值。
a匕ype:表示规则类坻的常量值。对于样式规则，这个值是丨。IE不支持这个属性。
K中二个最常用的间性是 cssText、selectorText 和 style。cssText 性与!style.cssText 厲性类似，似并不相同。前者包含选择符文本和围绕样式倍总的花括兮，A?杏只包含样式信息（类似于 冗素的style.cssText)。此外，cssText是只读的，而style.cssText也可以被重W。

12.2 样式 319
大多数情况下，仅使用style属性就可以满足所有操作样式规则的需求了。这个对象就像每个元 素上的style属性一样，UJT以通过它读取和修改规则中的样式信息。以下面的CSS规则为例。
div.box {
background-color: blue; width: lOOpx; height: 200px;
CSSRuksExample.htm
假设这条规则位于页面中的第一个样式表中，而且这个样式表中只有这一条样式规则，那么通过下 列代码可以取得这条规则的各种信息。
var sheet = document.stylesheets(0);
var rules = sheet.cssRules I I sheet.rules?	//取得规刻表
var rule = rules [0];	//取得第一■条规則
alert(rule.selectorText);
alert(rule.style.cssText);
alert(rule.style.backgroundColor);
alert(rule.style.width);
alert(rule.style.height);
//"div.box"
//完整的CSS代码 /"blue" //■100px" //_200px"
CSSRuiesExample.htm
使用这种方式，可以像确定元素的行内样式信息一样，确定与规则相关的样式信息。与使用元素的 方式一样，在这种方式下也可以修改样式信息，如下面的例子所示。
var sheet = document.styleSheets(0J;
var rules = sheet.cssRules II sheet, rules;	//取得规則表
var rule = rules[0J;	//取得第一条规則
rule.style•backgroundColor•厲 ” red"
CSSRulesExample. htm
必须要注意的是，以这种方式修改规则会影响页面中适用于该规则的所有元素。换句话说，如果有 两个带有box类的div元素，那么这两个元素都会应用修改后的样式。
2.创建规則
DOM规定，要向现有样式表中添加新规则，需要使用insertRuleO方法。这个方法接受两个参 数:规则文本和表示在哪里插人规则的索引。下面是一个例子。
sheet. insertRule("body { background-color: silver }", 0) ; //DOM 方法
这个例子插人的规则会改变元素的背景颜色。插人的规则将成为样式表中的第一条规则（插人到了 位0)	规则的次序在确定层叠之后应用到文档的规则时至关重要。Firefox、Safari、Opera和Chrome
都支持insertRule )方法。
IE8及更早版本支持一个类似的方法，名叫addRuleO,也接收两必选参数:选择符文本和CSS 样式信息;一个可选参数:插人规则的位置。在IE中插人与前面例子相同的规则，可使用如下代码。 sheet. addRu 1 e {* body", "background-color: silver*, 0) ; //仅对 IE 有效 有关这个方法的规定中说，最多可以使用addRule()添加4 095条样式规则。超出这个上限的调用 将会导致错误。
12

320 第 12 章 DOM2 和 DOM3
要以跨浏览器的方式向样式衣中插人规则，可以使用下面的函数。这个函数接受4个参数:要向其 中添加规则的样式表以及与addRule (相同的3个参数，如下所示。



function insertKule(sheet, scleccorText, cssText, position){
d f (sheet.insertRule){
sheet. insertRulo (sclectorToxt + " {* + cssText +	, position);
} else if (sheet.addRulc){
sheet.addRule(selGCtorTcxt# cssText, position);



CSSRidesExampk2.htm
下面是调用这个函数的示例代码。
insertRule(document.styleSheets10], -body", "background color: silver", 0);
虽然可以像这样来添加规则，似随着要添加规则的增多，这种方法就会变得非常繁琐。因此，如果 要添加的规则非常多，我们建议还是采用第10章介绍过的动态加载样式表的技术。
删除规则
从样式表屮删除规则的方法是deleteRuieU,这个方法接受•个参数:要删除的规则的位置。例 如，要删除样式表中的第一条规则，可以使用以下代码。
sheet .deleteRule (0) ;	//DOM 方法
[E支持的类似方法叫removeRuleO,使用方法相同，如下所示:
sheet. removeRule (0) ;	//仅对 IE 有效
下面是一个能够跨浏览器删除规则的函数。第一个参数是要操作的样式表，第二个参数是要删除的 规则的索引。
function deleteRule(sheetf index){ if (sheet.deleteRule){
sheet.deleteRule(index);
} else if (sheet.removeRule){ sheet.removeRule{index);
)
调用这个函数的方式如下。
CSSRulesExample2.htm
deleteRule(document.stylesheets(0], 0);
与添加规则相似，删除规则也不是实际Web开发中常见的做法。考虑到删除规则可能会影响CSS 的效果，因此请大家慎重使用。
12.2.3元素大小
本节介绍的属性和方法并不属于“DOM2级样式”规范，但却与HTML元素的样式息息相关。DOM 中没有规定如何确定页面中元素的大小。丨E为此率先引人丫一些属性，以便开发人员使用。目前，所有 主要的浏览器都已经支持这些属性。

12.2 样式 321
偏移置
首先要介绍的届性涉及偏移量（offsetdimension),包括元素在屏幕上占用的所有可见的空间。元素 的可见大小由其高度、宽度决定，包括所有内边距、滚动条和边框大小（注意，不包括外边距)。通过 下列4个属性可以取得元素的偏移量。
offsetHeight:元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的） 水平滚动条的髙度、上边框高度和下边框高度。
of fsetWidth:元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂 直滚动条的宽度、左边框宽度和右边框宽度。
offsetLeft:元素的左外边框至包含元素的左内边框之间的像素距离。
offsetTop:元素的上外边框至包含元素的上内边框之间的像素距离。
其中，offsetheft和offsetTop属性与包含元素有关，包含兀素的引用保存在offsetParent 属性中。of fsetParent属性不一定与parentNode的值相等。例如，1;1元•素的offsetParent是 作为其祖先元素的table元素，因为table是在DOM层次中距4(3最近的一个具有大小的元素。 图i2-i形象地展示r上面几个属性表示的不同大小。
offaetFarent



图丨2-1
要想知道某个元索在页面上的偏移it,将这个兀索的offsetLeft和offsetTop与其offsetParent 的相同属性相加，如此循环直至根元索，就可以得到一个基本准确的值。以下两个函数就可以用于分别 取得元索的左和上偏移量。
function getElementLeft(element){
var actualLeft = element.offsetLeft; var current = element.offsetParent?
while (current !== null){
actualLeft += current.offsetLeft; current = current.offsetParent?
}
12
}
return actualLeft;

322 第 12 章 D0M2 和 D0M3
function getElementTop(element){
var accualTop = element.offsetTop? var current = element.offsetParent;
while (current	null){
actualTop += current, offsetTop; current = current.offsetParent;
}
return actualTop;
OffsetDimensionsExampie.htm
这两个函数利用of f setParent属性在DOM M次中逐级向h回溯，将每个层次中的偏移M属性 合计到一块。对丁•简单的CSS布局的页面，这两函数可以得到非常精确的结果。对于使用表格和内嵌 框架布局的贞面，由于不同浏览器实现这些元素的方式不丨因此得到的值就不太精确了。•一般来说, 页面中的所有元素都会被包含在几个《1^元素中，而这些div元素的offsetParent 乂是 body元素，所以 getElementLeft )与 getElementTop ( 会返冋与 offsetLeft 和 of fsetTop 相同的值。
(^\	所有这些偏移量属性都是只读的，而且每次访问它们都需要重新计算。因此，应
^该尽量避免重复访问这些属性;如果需要重复使用其中某些属性的值，可以将它们保 存在局部变量中，以提高性能。
客户区大小
元素的客户区大小（clientdimension),指的是元索内容及其内边距所占据的空间大小。有关客户IX 大小的属性有两个:clientwidth和clientHeight。其中，clientwidth属性是兀素内容区宽度加 上左右内边距宽度;clientHeight属性是元素内容K髙度加上上下内边距高度。團12-2形象地说明 了这些属性表示的大小。
offsetParent



图 12-2

12.2 样式 323
从字面上看，客户K大小就是元素内部的空间大小，因此滚动条占用的空间不计算在内。最常用到 这些属性的情况，就是像第8章讨论的确定浏览器视口大小的时候。如下面的例子所示，要确定浏览器 视U大小，可以使用document.documentElement或docuinent.body (在IE7之前的版本中）的 clientWidth 和 clientHeight。
function getviewport(){
if (document.compatMode == "BackCompat"){ return {
width: docximent.body.clientWidth, height: document.body.cliontHeight
;
} else {
return {
width: document.documentElement.clientwidth, height: document.documentElement.clientHeight
这个函数首先检査document.compatMode属性，以确定浏览器是否运行在混杂模式。Safari 3.1 之前的版本不支持这个属性，W此就会自动执行else语句。Chrome、Opera和Firefox大多数情况下都 运行在标准模式下，因此它们也会前进到else语句。这个函数会返冋一个对象，包含两个属性:width 和height;表示浏览器视(J (html;^body:兀素）的大小。
与偏移量相似，客户区大小也是只读的，也是每次访问都要重新计算的。
滚动大小
最后要介绍的是滚动大小（scrolldimension),指的是包含滚动内容的元素的大小。有些元素（例如 html元素），即使没有执行任何代码也能自动地添加滚动条;但另外一些元素，则需要通过CSS的 overflow M性进行设置才能滚动。以下是4个与滚动大小相关的属性。
scrollHeight:在没有滚动条的情况下，元素内容的总高度。
scroUWidth:在没有滚动条的情况下，元素内容的总宽度。
scrollLeft:被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元索的滚动位置。
scrollTop:被隐藏在内容区域上方的像素数。通过设S这个属性可以改变元素的滚动位置。
图12-3展示了这些属性代表的大小。
scrollWidth和scrollHeight主要用于确定元素内容的实际大小。例如，通常认为html元素 是在Web浏览器的视口中滚动的元素（IE6之前版本运行在混杂模式下时是body元素)。因此，带有 垂直滚动条的页面总髙度就是document .documentElement .scrollHeight。
对于不包含滚动条的页面而言，scrollWidth和scrollHeight与clientwidth和 clientHeight之间的关系并不十分清晰。在这种情况下，基于document.documentElement査看 这些厲性会在不同浏览器间发现一些不一致性问题，如下所述。
Firefox中这两组属性始终都是相等的，但大小代表的是文档内容区域的实际尺寸，而非视口的 尺寸。
 Opera、Safari 3.1及更高版本、Chrome中的这两组属性是有差别的，其中scrollWidth和 scrollHeight等于视口大小，而clientwidth和clientHeight等于文档内容区域的大小。
12

324 第 12 章 D0M2 和 D0M3
□ IE (在标准模式）中的这W组属性不相等，其中scrollWidth和scrollHeight等丁‘文样内 容K域的大小，而clientWidth和cljentHeight等于视n大小0
I	scrolIWidth	|



scrollLeft
图 12-3
在确定文档的总高度时（包括基T1视口的最小高度时)，必须取得scrollWidth/clientWidth和 scrollHeight/clientHeigh匕中的最大值，才能保证在跨浏览器的环境下得到梢确的结果。下面就 是这样一个例子。
var docHeight = Math. max (document. documencE 1 emer.t. scroliHeight,
document.docuraentElement.clientHcight);
var docWidth = Mach.max(doc\iment .documerxElement .scrollwidth,
document.docuraentElement.clientwidth);
注意，对71运行在泥杂模式下的IE，则需要用document, body代替document, document- Element〇
通过scrollLeft和scrollTop属性既可以确定元素当前滚动的状态，也可以设a元素的滚动位 置。在元素尚未被滚动时，这两个属性的值都等于0。如果元素被垂直滚动丫，那么scrollTop的值 会大于0,且表示元素上方不可见内容的像素高度。如果元素被水平滚动了，那么scrollLeft的值会 大于0,且表示元素左侧不可见内容的像素宽度。这两个厲性都是可以设置的，因此将元素的 scrollLeft和scrollTop设置为0,就可以®置元素的滚动位置。下面这个函数会检测元素是否位 于顶部，如果不是就将其回滚到顶部。
function scrollT〇Top(element){ if {element.scrollTop != 0) element.scrollTop = 0;

}
这个函数既取得了 scrollTop的值，也设置了它的值。
确定元素大小
EE、Fircfox3+' Safari4+、Opera9_5及 Chrome为毎个元素都提供了一个getBoundingClientRect。方 法〇这个方法返回会一个矩形对象，包含4个属性:left、top、right和bottom。这作属性给出了

12.2 样式 325
元素在页面中相对F视口的位置。但是，浏览器的实现稍有不同。丨E8及更早版本认为文档的左上角坐 标是(2,2),而其他浏览器包括IE9则将传统的(0,0)作为起点坐标。W此，就潘要在一开始检查一下位丁 (0,0)处的元素的位S,在IE8及更早版本中，会返W(2,2),而在其他浏览器中会返回(0,0)。来肴下面的 函数:
function getBoundingClientRect(element){
if (typeof argiunents.callee.ofCset != "number"){
var scrollTop = document.documentElenent.scrollTop? var temp = document.createKlement("div"; temp.style.cssText = "position:absolute;left:0;top:0?"? document.body.appendChiId(temp);
arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop; document.body.removeChiId{t emp); temp = null;
var rect s element.getBoundingClientRect(); var offset = arguments.callee.offset;
return {
left: rect.left + offset, right: rect.right + offset, top: rect.top + offset, bottom: rect.bottom + offset
};
)
GetBoundingClientRectExample.htm
这个函数使用了它自身的属性来确定是否要对坐标进行调整。第一步是检测属性是否有定义，如果 没夯就定义一个。S终的offset会被设置为新元素上坐标的负值，实际上就是在IE中设置为-2,在 Firefox和Opera中设置为-0。为此，需要创建一个临时的元岽，将其位置设置在(0,0),然后再调用其 getBoundingClientRect ()。而之所以要减去视H的scrollTop,是为丫防止调用这个函数时窗口 被滚动了。这样编写代码，就无需每次调用这个函数都执行两次getBoundingClientRect () 了。接 下来，再在传人的元素上调用这个方法并基丁新的H•算公式创建一个对象。
对于不支持getBoundingClientRect (的浏览器，可以通过其他手段取得相丨的信息。一般来 说，right和left的差值与offsetwidth的值相等，而bottom和top的差值与offsetHeight 相等。而且,lef t和top戚性大致等于使用本章前面定义的getElementLeft (和getElementTop ( 函数取得的值。综合上述，就可以创建出下面这个跨浏览器的函数:
function getBoundingClientRect(element){
var scrollTop = document.documentBlement.scrollTop; var scrollLeft = document »docuaentBleinent. acrollLeft;
if (element•getBoundingClientRect)
if (typeof arguments.callee.offset 1 * "number"){ var temp = document.createElement("div"); temp.style.cssText = "position:absolute;left:0;top:0;"; document•body♦appendChild{temp);
arguments.ca1lee.o£fset = -temp.getBoundingClientRect{).top - scrollTop; document.body.reraoveChiId(temp);
12

326 第 12 幸 D0M2 和 D0M3
temp = null;
var rect = element.getBoundingClientRoct{); var offset = arguments.callee.offsec;
return {
left : rect. left +• offset, righc: rect.right + offset, top: rect.top + offset, bottom: rect.bottom + offset
var actualLaft ■ getElementLeft(element); var actualTop « getBleraentTop(element);
return {
left: actualLeft - ecrollLeft,
right: actualLeft -t- element»offsetwidth - scrollLeft, top: actualTop - scrollTop,
bottom: actualTop + element.offsetHeight - scrollTop
这个函数在getBoundingClientRect()有效时，就使用这个原生方法，而在这个方法无效时贝lj 使用默认的计算公式。在某些情况下，这个函数返冋的值可能会有所不同，例如使用表格布局或使用滚 动元素的情况下。
由于这里使用了 arguments.callee，所以这个方法不能在严格接式下使用。
##  12.3 遍历
“DOM2级遍历和范围”模块定义了两个用于辅助完成顺序遍历DOM结构的类型:Nodeiterator 和TreeWalker。这两个类型能够基于给定的起点对DOM结构执行深度优先（depth-first)的遍历操作。 在与DOM兼容的浏览器中（Firefox 1及更高版本、Safari 1.3及更高版本、Opera 7.6及更高版本、Chrome
2及更高版本），都可以访问到这些类型的对象。IE不支持DOM遍历。使用下列代码可以检测浏览器 对DOM2级遍历能力的支持情况。
var supportsTraversals = document.implementation.hasFeature("Traversal", "2.0"); var supportsNodelterator = (typeof document.createNodelterator == "function"}? var supportsTreeWalker =■ (typeof document.createTreeWalker == * function"};
如前所述，DOM遍历是深度优先的DOM结构遍历，也就是说，移动的方向至少有两个（取决 丁-使用的遍历类型)。遍历以给定节点为根，不可能向上超出DOM树的根节点。以下面的HTML页 面为例。
};
} else {
GetBoundingClientRectExample.htm




]2.3 遍历 327
!DOCTYPE html
html
head
/hea3
body
pxbHello/b world!/p /body
/html
图124展示了这个页面的DOM树。



阁 12-4
任何节点都可以作为遍历的根节点。如果假设body元素为根士点,那么遍历的第一步就是访问p 元素，然后再访问同为body元素后代的两个文本节点。不过，这次遍历永远不会到达html、head 元素，也不会到达不属于body元素+树的任何节点。而以document为根节点的遍历则可以访问到文 档中的全部节点。图12-5展示了对以document:为根节点的DOM树进行深度优先遍历的先后顺序。
G
Documeat
| El 邮title]	^7^ j Bl^nent P j
(?) [Text Example | (^)| ziement^h'⑭
world!
| Text Hel lo [
12
阁 12-5

328 第 12 章 D0M2 和 D0M3
从document开始依序向前，访问的笫一个节点是document，访问的最后一个节点是包含 "world!"的文本节点。从文档最;H•的文本货点开始，遍历可以反向移动到DOM树的顶端。此时，访 问的第一个节点是包含"Hellow的文本节点，访问的摄G—个节点足-document节点。Nodellerator 和TreeWalker都以这种方式执行遍历〇
###  12.3.1 Node 工 terator
Nodeltzerator类?S是网者中比较简单的一个，可以使用docuiaent.createNodelterator (}方 法创建它的新实例。这个方法接受下列4个参数。
root:想要作为搜索起点的树中的节点。
whatToShow:表示要访问哪些节点的数7代码。
filter*: ;&—个NodeFilter对象，或者一个表示应该接受还是拒绝某种特定节点的函数。
entityReferenceExpansion:布尔值，表示是否要扩展实体引用。这个参数在HTML页面 中没有用，因为艽中的实体引用不能扩展。
whatToShow参数是一个位掩码，通过应用一或多个过滤器（filter)来确定要访问哪些节点。这个 参数的值以常量形式在NodeFilter类型中定义，如下所示。
NodeFilter.SHOWjVLL:显示所有类型的节点。
 NodeFilter.SHOW^ELEMENT:	示元素节点0
N〇deFilter.SH〇W_ATTRIBUTE:敁示特性节点。由于DOM结构原因，实际」:不能使用这个值。
NodeFilter.SHOW_TEXT:显示文本节点。
NodeFilt:er.SKOW_CDATA_SECTION:.秘.示 CDATAf/点。对 HTML 贞面没在用。
N〇deFilter.SHOW_ENTITY_REFERENCE:显示实体引用节点。对HTML页面没有用。
NodeFi:Lter.SHOW_ENTITYE: M示实体节点。对HTML页面没有用。
口 N〇deFilter.SH0W_PR0CESSIMG_INSTRUCT10N:显示处理指令节点。对 HTMLM面没有用。
口 N〇deFilter.SHOW_COMMENT:显示注释节点。
N〇deFilter.SHOW_DOCUMENT:显示文抬节点。
N〇deFilter.SHOW_DOCUMENT_TYPE:显示文档类型节点。
NodeFilter.SHOW+DOCUMENT+FRAGMEN?:显示文构片段节点。对 HTMI/jtf面没有用。
NodeFilter.SHOW_NOTATION: a示符兮节点。对HTML页面没有用。
除了 K〇deFilter.SHOW_ALL之外，町以使用按位或操作符来组合多个选项，如下面的例子所示:
var whatToShow = NodoFilter.SHOW_ELEMENT I NodeFileer.SHOW_TEXT;
可以通过createNodelterator 〇方法的filter参数来指定ft定义的NodeFilter对象，或者 指定*个功能类似节点过滤器（nodefilter)的函数。每个NodeFilter对象只有-个方法，即accept- Node();如果应该访问给定的节点，该方法返回NodeFilter.FILTER_ACCEPT，如果不应该访问给 定的节点，该方法返回NodeFilter.FILTER_SKIP。由于NodeFilter是一个抽象的类型，因此不能 直接创建它的实例。在必要时，只要创建一个包含acceptNodeO方法的对象，然后将这个对象传入 createNodelterator()中即nj■。例如，下列代码展示了如何仓ij建一个只显示p元素的节点迭代器。
var filter = {
acceptNode; function(node){
return node.tagName.toLowerCaseO == wp" ?

12.3 遍历 329
NodeFilter.FlLTSR_ACCEPT : NodGF i i t er.FILTER_S KIP;
var iterator - document.createNodelterator(root, NodeFilter.SHOW 一ELEMENT, filter, false);
第三个参数也可以是•个■facceptUodeU方法类似的函数，如下所示。
var filter * function(node){
return node.tagName. toLowerCaae () *:= Mp” ?
NodeFilter.PILTBR_ACCEPT :
NodeFilter.FILTBR_SKIP;
 I
var iterator ^ document.createNodelterator(root, NodeFilter.SHOW_ELEMENT, filter, false);
—般来说，这就是在JavaScript中使用这个方法的形式，这种形式比较简单，而且也跟其他的 JavaScript代码很相似。如果不指定过滤器，那么应该在第三个参数的位置上传人null。
下面的代码创建了一个能够访问所有类型节点的简单的Nodeiterator。
var iterator = document.createNodelterator(document, NodeFilter.SHOW_ALL, null, false);
Nodeiterator类型的两个主要方法是nextNode ()和previousNode () »顾名思义，在深度优先 的DOM子树遍历中，nextNode()方法用于向前前进一步，而previousNode()用于向后后退一步。 在刚刚创建的Nodeiterator对象中，有一个内部指针指向根节点，因此第一次调用nextNodeU会 返回根节点。当遍历到DOM子树的最后--个节点时，nextNode()返冋null。previousNodle方法 的工作机制类似。当遍历到DOM子树的最后一个节点，aPrevi〇uSN〇de()返冋根节点之后，冉次调 用它就会返回mall。
以下面的HTML片段为例。
div id="divl"
pxbHello/b world!/p
ul
liList item l/li
liList item 2/li
liList item 3/li
/ul
/div
NodeIteratorExampieJ.htm
假设我们想要遍历div元素中的所有元素，那么可以使用下列代码。
var div = document.getElementById("3ivl ■*);
var iterator = document.createNodelterator(div, NodeFilter.SHOW_ELEMENT, null, false);
var node = iterator.nextNodeO ? while (node !== null) {
alert {node. tagName) ;	//输出标签名
node = iterator.nextNodeO ?
}
12
NodeIteratorExamplel.htm

330 第 12 章 DOM2 和 DOM3
在这个例子中，第一次调用nexCNode (返冋p元素。因为在到达DOM子树末端时nextNode ( 返冋null,所以这里使用了 while语句在每次循环时检丧对nextNode()的调用是否返问了 null。 执行上面的代码会显示如K标签名:
DIV
P
B
UL
LI
LI
LI
也许用不着显示那么多信息，你只想返丨"丨遍历中遇到的li元素。很简单，只要使用一个过滤器 即可，如下面的例子所示。
var div = document,getElementById(Bdivl")? var filter = function(node){
return node.ta0N&me.toLowerCase() == nli" ?
NodeFilter.FILTER—ACCEPT :
NodeFilter.FILTER_SKIP)
/
var Iterator ■ document.createNodeiterator(div, NodeFilter.SH〇w_ELEMEN'7f filter, false"
var node = iterator .r.extNode (); while (node !-= null) {
alert (node. tagName) ;	//输出标签名
node = iterator.nextNode();
)
NodeIteratorExampie2.htm
在这个例子中，迭代器只会返元索。
由于nextNode()和previousNodeU方法都基于Nodeiterator在DOM结构中的内部指针工 作，所以DOM结构的变化会反映在遍历的结果中。
Firefox 3.5之前的版木没有实现createNodeIterator)方法，但却支持下一 节要讨论的createTreeWalker()方法。
12.3.2 TreeWalker
TreeWalker 是 Nodeiterator 的一■个更高级的版本。除了包括 nextNode ()和 previousNode () 在内的相同的功能之外，这个类型还提供了下列用于在不同方向上遍历DOM结构的方法。
parenLNodGO:遍历到当前节点的父节点;
firstChildU):遍历到当前节点的第-个子节点;
lastChildU:遍历到当前节点的最后一个子节点;
nextSiblingO:遍历到当前节点的下一个同辈节点;
previousSiblingU :般历到当前节点的上一个同辈节点。

12.3 遍历 331
创建TreeWalker对象要使用document • createTreeWalker ()方法，这个方法接受的4个参数 与document.createNodelterator《）方法相同:作为遍历起点的根节点、要显疋的节点类型、过滤 器和一个表示是否扩展实体引用的布尔值。由丁-这两个创建方法很相似，所以很容易用TreeWalker 来代替Nodeiterator,如T*面的例子所/五〇
var div = document .getElementById ("3iv1 ■); var filter = function(node){
return node.tagName.tol.owerCase() == •li"?
NodeFi Iter. FILTERACCEPT :
NodeFilL〇r.FIIjTER_SKI?;
var walkers document.createTreaWftlkdr(div, NodeFilter.SHOW BLSMEHT, filter, £alse);
var node = iterator.nextKode(); while (node i== null) {
alert (node.tagNanie);	//搶出标签名
node = iterator.nextNodeO ;
Tree WalkerExamplel.htm
在这里，filter吋以返冋的值冇所不同。除丫 NodeFilter.FILTER_ACCEPT和NodeFilter. FILTER^SKIP 之外，还可以使用 N〇deFilter.FILTER_REJECT。在使用 Nodeiterator 对象时， N〇deFilt:er.FILTER_SKn 与 N〇deFilter.?ILTER_REJECT 的作用相同:跳过指定的节点。但在使 用TreeWalker对象时，NodeFiUer.FILTER_SKIP会跳过相应节点继续前进到子树中的下一个节点， 而N〇deFiltei:.FILTER_REJECT则会跳过相应节点及该节点的整个子树。例如，将前面例子中的 NodeFilter.FILTER_SKIP修改成NodeFilter_FI:LTER_REJECT，结果就是不会访问任何节点。这是 因为第一个返回的节点是div,它的标签名不是"Li",于是就会返回N〇deFilter.FILTER_REJECT， 这意味着遍历会跳过整个子树。在这个例子中，div元素是遍历的根节点，于是结果就会停止遍历。
当然，TreeWalker真IH强大的地方在于能够在DOM结构中沿任何方向移动。使用TreeWalker 遍历DOM树，即使不义过滤器，也可以取得所有li元索，如下面的代码所示。 var div = document.getElenentByld(*divlh);
var walker = document.createTreeWalker(div, NodeFilter.SHOW_ELEMENT, null, false);
walker.rstChild();	//持到p
walker.nextSiblingt) /	//梓到ul
var node ?walker. firetChildt);	//神到第一个li
while (node !== null) { alert(node.tagName); node ?walker.aextSibling();
}
Tree WalkerExample2. him
因为我们知道li元素在文档结构中的位置，所以可以直接定位到那里，即使用firStChild() 转到p元素，使用nextSibling ()转到ul元素，然后再使用f irstChild (转到第一个li元素。 注意，此处TreeWalker只返[5丨元素（由传人到createTreeWalker (的第二个参数决定）。因此，可 以放心地使用nextSibling ()访问每一个li元素，直至这个方法最后返回mill。
12

332 第 12 幸 DOM2 和 D0M3
TreeWalker类型还有一个属性，名叫currentNode,表示任何遍历方法在上一次遍历中返回的 节点。通过设置这个厢性也可以修改遍历继续进行的起点，如下面的例子所示。
var node s walker.nextNodeO ;
alert(node === walker.currentNode);	//true
walker .currentNode = document .body;	//修改起点
与Nodeiterator相比，TreeWalker类型在遍历DOM时拥有更大的灵活性。由于IE中没有对 应的类铟和方法，所以使用遍历的跨浏览器解决方案非常少见。
##  12.4 范围
为丫让开发人员更方便地控制页面，“DOM2级遍历和范围”模块定义了 “范围"（range)接口。通 过范围可以选择文档中的-个K域，而不必考虑节点的界限（选择在后台完成，对用户是不可见的)。 在常规的DOM操作不能更有效地修改文档时，使用范围往往可以达到目的。Firefox、Opera、Safari和 Chrome都支持DOM范围。丨E以专有方式实现了自己的范fffl特性。
DOM中的范围
DOM2级在Document类型中定义fcreateRangen方法。在兼容DOM的浏览器中，这个方法
属于document对象。使用hasFeatureO或者直接检测该方法，都坷以确定浏览器是否支持范围。
var supportsRango = document.implementation.hasFeature(-Range*, "2.0"); var alsoSupportsRange = (typeof docuinent.createRange == •function”;
如果浏览器支持范围，那么就可以使用createRange (来创建DOM范围，如下所示:
var range = document.createRange();
与节点类似，新创建的范围也直接与创建它的文档关联在一起，不能用于其他文档。创建了范围之 后，接下来就可以使用它在后台选择文档中的特定部分。而创建范围并设置了其位置之后，还可以针对 范围的内容执行很多种操作，从而实现对底层DOM树的更精细的控制。
每个范围由一个Range类型的实例表示，这个实例拥有很多属性和方法。下列属性提供了当前范 围在文档中的位置信息。
startContainer:包含范围起点的节点（即选区中第一个节点的父节点)。
startOffset::范围在startContainer中起点的偏移童。如果startContainer是文本节 点、注释节点或CDATA节点，那么startOffset就是范围起点之前跳过的字符数量。否则， startOffset就是范围中第一个子节点的索引。
endContainer:包含范围终点的节点（即选区中最后一个节点的父节点）。
enfiOffset:范围在endContainer中终点的偏移萤（与startOffset遵循相同的取值规则）。
commonAncestorContainer: startContainer fU endContainer 中位置最深的那个。
在把范围放到文档屮特定的位置时，这些属性都会被賦值。
用DOM范围实现简单选择
要使用范围来选择文档中的一部分，坡简的方式就是使用selectNode ()或selectNtodeContents ()。 这两个方法都接受一个参数，即一个DOM节点，然后使用该节点中的信息来填充范围。其中，

12.4 范围 333
selectNode(方法选择整个节点，包括其子节点;而selectNodeContentsU方法则只选择节点的 子节点以下面的HTML代码为例。
!DOCTYPE html
html
body
p id="pl*xbHello/b world!/p
/body
/html
我们可以使用下列代码来创建范围:



var rangel = document.createRange();
range2 = document.createRange();
pi = document ,getElementById(*pl•*);
rangel.selectNode(pl);
range?., selectNodoContents (pi);
DOMRangeExample. htm
这里创建的两个范岡包含文杓中不同的部分:rangl包含p/元素及其所有子元素，而rang2包 含b/元素、文本节点"Hello*和文本节点-world!-(如图12-6所示)。
rangel
I	—	1
p id=**pl*,bHell〇/b world!/p
I	I
range2
图 12-6
在调用 selectNode (}时，startContainer、endContainer 和 coiranonAncestorContainer 都等于传人节点的父节点，也就是这个例子中的document.body。丨WstartOffset属性等于给定节 点在其父节点的childNodes集合中的索引（在这个例子中是1 —-因为兼容DOM的浏览器将空格算 作•个文本节点），endOffset等于startOf fset加1 (因为只选择了一个节点）。
selectNodcContents {) W^startContainor ^endContainer ffl commonAncestorConta- 土1^等于传入的节点，即这个例子中的p元素。而startOffset属性始终等于0,闶为范围从给定节 点的第一个了•节点开始。最后，endOff set等于子节点的数量（node.childNodes .length ),在这个例 子中是2。
此外，为了更精细地控制将哪些节点包含在范围中，还可以使用下列方法。
setStartBefore(refWode):将范的起点设置在refWode之前，因此refWode也就是范围 选1^中的第一个子节点。同时会将startContainer属性设置为refWode.parentWode,将 startOffset属性设置为refwode在其父节点的cMJdWodes集合中的索引。
setStartAfter (refWode):将范围的起点®置在refWode之后，因此refWode也就不在范 围之内了，其下一个同辈节点才是范闱选区中的第-个子节点。同时会将starCContainer屈 性设H为refWbde.parenti\tode,将startOffset属件设置为refWode在其父节点的 chUdNodes集合中的索引加U
setEndBefore (refWode):将范围的终点设置在refwode之前，因此re^Nbde也就不在范围 之内了，其上一个同辈节点才是范围选K中的最AT—个子节点。同时会将endContainer属性

334 第 12 章 D0M2 和 DOM3
设reflVoc3e._parentWode，将 endOffset 属性设置为 ref^ode在其父节点的 chiidWocies 集合中的索引。
□ setEndAfter (re£Wode:将范围的终点设置在refWode之后，因此refWode也就是范围选区 中的最后—子节点。同时会将endContainer M性设置为refATode.parentWode，将 endoffset腐性设置为refWode在Jt:父节点的c_hi2dWodes集合中的索引加1。
在调用这些方法时，所存属性都会动为你设置好。不过，要想创建复杂的范围选区，也可以直接 指定这些属性的值。
用DOM范围实现复杂选择
要创建复杂的范围就得使用setStart()和setEnd()方法。这两个方法都接受两个参数:一个参 照节点和一个偏移it值。对setStart()来说，参照节点会变成startContainer，而偏移量值会变成 startOffset。对于setEnd()来说，参照节点会变成endContainer，而偏移量值会变成endOffset。 可以使用这两个方法来模仿selectNode()和select:NodeContexits)。来看下面的例了
var rangel = document.createRange{);
1	range2 = document.crcateRange{);
pi = documont.getElementById("pl"); pllndex = -1$ i, len;
for (i=0, lezispi.parentKode.childNodes.length; i  len; i++) { if (pl.parentNode.childNodes [i】:■ pi) { pllndex & i; break;

)
rangel.setStart(pi.parentNode, pllndex); rangel.aetEndtpi.parentNode, pllndex * 1); raBge2.setStart(pi, 0);
range2.flet£nd(plr pi.childNodes.length);
DOMRangeExample2. htm
显然，要选择这个节点（使用rangel ),就必须确定当前节点（pi)在其父节点的childNodes 集合中的索引。而要选择这个节点的内容（使用range2),也不必计算什么;只要通过setStart () 和 setEnd()设S默认值即叶。模仿 select;No3e()和 selectNodeContentsO并不是 setStartO 和setEnd()的主要川途，它们更胜一筹的地方在于能够选择节点的一部分。
假设你只想选择前面HTML示例代码中从MHellow的"11〇"到-world!"的"〇"	很容易做到。
第一步是取得所有节点的引用，如下面的例T所示:
var pi = document.getElementById{"pi"); helloNode = pi. firstChild. firsstChild; worldNode = pl.lastChild;
DOMRangeExampie3.htm
实际上，"Hello•’文本节点是p元素的孙子节点，因为它本身是b元素的一个子甘点。W此， 口1.£^8(:〇111(3取得的是13,而91.打『31:(:1^13^:1^1;(:)^113取得的才是这个文木节点。'^〇1:1(3!" 文本节点是P元素的第二个子节点（也是最后一个子节•点），因此可以使用pl.lastChild取得该节

点。然后，必须在创建范围时指定相应的起点和终点，如下面的例子所示。
12.4 范围 335
var range = document.createRange(); range.secStart(heiloNodo, 2); range.setEnd(worldNode, 3);
D0MRangeExampie3.htm
因为这个范围的选区应该从"Hello■中-e-的后面开始，所以在setStartO中传人helloNode 的同时，传人了偏移M 2 (即"e"的下一个位置;的位置是0)。设置选区的终点时，在setEnd() 中传人worldNode的同时传人了偏移M3,表示选K之外的第一个字符的位置，这个字符是》r»,它的 位K是3 (位》0上还有一个空格)。如图12-7所示。
range
I	I
p id="pl"blHlellllld/bl lwlolrlildirk/D
01234	0123456
阁 12-7
由T helloNode和worldNode都是文本节点，因此它们分别变成新建范围的startContainer 和endContainer。此时startOf fset和endOf fset分别用以确定两个节点所包含的文本中的位置， 而不是用以确定子节点的位置（就像传人的参数为元索节点时那样）。此时的corranonAncestor- Container是〇元素，也就是同时包含这两个节点的第一个祖先兀索。
当然，仅仅是选择了文档中的某••部分用处并不大。似重要的是，选择之后才可以对选区进行操作。
操作DOM范围中的内容
在创建范围时，内部会为这个范闱创建一个文档片段，范围所属的全部节点都被添加到了这个文档 片段中。为了创违这个文档片段，范围内容的格式必须正确有效。在前面的例子中，我们创建的选K分 别开始和结束于两个文木节点的内部，因此不能算是格式良好的DOM结构，也就无法通过DOM来表 示。但是，范围知道自身缺少哪栈开标签和闭标签，它能够重新构建有效的DOM结构以便我们对其进 行操作。
对于前面的例子而言，范围经过计算知道选区中缺少-个开始的b标签，因此就会在后台动态加 人一个该标签，同时还会在前面加人一个表示结束的/b标签以结束，_He”。于是，修改后的DOM就 变成了如下所示。
pxbHe/bxbllo/b world !/p
另外，文本节点_world!"也被拆分为两个文本节点，一个包含-WO”，另•-个包含"rid! •。最终的 DOM树如图12-8所示，右侧是表示范围的文档片段的内容^
像这样创建了范ffl之后，就可以使用各种方法对范围的内容进行操作了（注意，表示范围的内部文 档片段中的所有节点，都只是指向文档中相应节点的指针)。
第一个方法，也是最容易理解的方法，就是deleteContents ()。这个方法能够从文档中删除范 m所包含的内容。例如:
var pi = document.getElementById("pi*);
*	helloNode = pi.firstChild.firstChild;
worldNode = pt.lastChild; range = document.createRange()?

336 第 12 章 D0M2 和 D0M3
range.setstart(helloNode/ 2); range.setEndworldNodo, 3);
range.deleteContents();
DOMRangeExample4.htm
文档	范围



图 12-8
执行以上代码后，页面中会M示如下HTML代码:
pxbHe/brld! /p
由于范围选区在修改底S DOM结构时能够保证格式良好，因此即使内容被刪除了，最终的DOM 结构依旧是格式良好的。
与deleteContents (方法相似，extractContents (也会从文构中移除范围选区。但这两个方 法的区别在于，extractContentsO会返(P丨范围的文杓片段。利用这个返冋的值，可以将范围的内容 插人到文档中的其他地方。如下面的例子所示:
var pi = document .getBlemer.tByld ("pi"  ; helloNode = pi.firscchild.firscchild; worldNode = pi.lastChild; range = document.createRange();
range.setstart(helloNode, 2); range.setEnd(worldNode/ 3);
var fragment s range.extractContents()? pi.parentNod«.appendChild(fragment);
DOMRangeExample5.htm

12.4 范围 337
在这个例子中，我们将提取出来的文裆片段添加到了文Sbody元素的末尾。（记住，在将文档片 段传人appendChild(方法中时，添加到文档中的只是片段的子节点，而非片段本身。）结果得到如下 HTML代码:
pxbHe /brld!  /p
bllo/b wo
还一种做法，即使用cloneContentsO创建范围对象的一个副本，然后在文档的其他地方插人该 副本。如下面的例子所示:
var pi = document.getElementById("pi"), helloNode = pi.firstChild.firstChild, worldNode = pi.lastChild, range = document.createRange〇?
range.seLStart{helloNode, 2); renge.setEnd(worldNode, 3);
var fragment - range.cloneContents(); pi.p&r«ntNode.appendChild(fragment);
DOMRangeExampleS. htm
这个方法与extractContentsO非常类似，因为它们都返间文档片段。它们的主要区别在于， cloneContents ()返回的文档片段包含的是范围中节点的副本，而不是实际的节点。执行上面的操作 后，贞面中的HTML代码应该如下所示:
pxbHello/b world!/p
bllo/b wo
有一点请读者注意，那就是在调用上面介绍的方法之前，拆分的节点并不会产生格式良好的文档片 段。换句话说，原始的HTML在DOM被修改之前会始终保持不变。
4.插入DOM范围中的内容
利用范围，可以删除或复制内容，还可以像前面介绍的那样操作范围中的内容。使用insertNodeO 方法可以向范围选区的开始处插人一个节点。假设我们想在前面例子中的HTML前面插人以下HTML 代码:
span style="color: red"Inserted text/span
那么，就可以使用下列代码:
var pi = document.getElementById(*pl")? helloNode = pi.firstChild.firsLChild; worldNode s pi.lastChild; range = document.createRange();
range.setStart(helloNode, 2); range.setEnd(worldNode, 3);
var span = document.createElement("span"); span.style.color s Bred";
span.appendChild(document.createTextNode("Inserted textM)); range.insertNode(Bp2〇z);
DOh4RangeExample7.htm
12

338 第 12 幸 D0M2 和 D0M3
运行以上JavaScript代码，就会得到如下HTML代码:
p id="piubHespan style="color: red*Inserted text/spanllo/b world/p
注意，Span正好被插人到r"Hell〇_•中的"llo‘•前面，而该位置就是范围选区的开始位置。还要 注意的足，由•这里没有使用上一节介绍的方法，结果原始的HTML并没有添加或删除b元素。使用 这种技术町以插人一些帮助提示信息，例如在打开新窗H的链接旁边插人一幅图像。
除了向范围内部插人内容之外，还可以环绕范围插人内容，此时就要使用surrcmmiCcmtentsO 方法。这个方法接受一个参数，即环绕范围内容的节点。在环绕范围插人内容时，后台会执行下列 步骤。
(1提取出范围中的内容（类似执行找:^3£:1:(:01^6111;));
将给定节点插人到文档中原来范围所在的位置上;
将文档片段的内容添加到给定节点中。
可以使用这种技术来突出显示网页中的某些词句，例如下列代码:
var pi = document.getEIcmentById{"pl"); helloNode = pt.firstChild.firstChild; worldNode s pi.lastChild; range = documenc.createRangeO ;
range.salectNode(helloNode);
var span = document.createBlement("span")t span* style. backgroimdColor = •yellow*'/ range.aurroundContents(span);
DOMRangeExample8.htm
会给范围选区加h—个黄色的背景。得到的HTML代码如下所示:
pxbHe/bxspan styles*background-color:yellow"bllo/b wo/spanrld!/p
为了插人39811,必须将13元素拆分成两个b元素，一个包含"He%另一个包含"11〇"。拆分 之后，就可以稳妥地插人span 了。
5.折叠DOM范围
所谓折叠范围，就是指范围中未选择文档的任何部分。可以用文本框来描述折叠范围的过程。假设 文本框中有一行文本，你用跃标选择了其中一个完整的单词。然后，你单击鼠标左键，选K消失，而光 标则落在了其中两个字母之间。同样，在折叠范围时，其位置会落在文档中的两个部分之间，可能是范 围选冈的开始位置，也可能是结束位置。图12-9展示了折叠范围时发生的情形。
使用collapseU方法来折叠范围，这个方法接受一个参数，一个布尔值，表示要折叠到范围的哪 —端。参数true表示折叠到范围的起点，参数false表示折叠到范围的终点。要确定范围已经折叠完 毕，可以检査collapsed属性，如下所示:
range.collapse(true) ;	//折叠到起点
alert (range, col lapsed) ;	//搶出 true

12.4 范围 339
p id="pl"bHdllo/b
原始范围
wcjrld! /p
p id=HplMbH
lo/b worldi/p
折香到开始位置
p id="pl"bHello/b wc|rld!/p
折昼到结束位罝 图 12-9
检测某个范围是否处于折叠状态，可以帮我们确定范围中的两个节点是否紧密相邻。例如，对于下 面的HTML代码:
p id="pl "Paragraph l/pxp id="p2"Paragraph 2/p
如果我们不知道其实际构成（比如说，这行代码是动态生成的）,那么可以像下面这样创建-个范围。
var pi = document.getElementById{"pi")/ p2 = document.getElementById{*p2^# range = document.createRange{); range.setStartAfter(pi); range.setStartBefore(p2)? alert (range.collapsed);	//输出 true
在这个例子中，新创建的范围是折叠的，因为pi的后面和p2的前面什么也没有。
比较DOM范围
在有多个范围的情况下，可以使用compareBoundaryPointsU方法来确定这些范围是否有公共 的边界（起点或终点)。这个方法接受两个参数:表示比较方式的常童值和要比较的范围。表示比较方 式的常量值如下所示。
Range • START_TO_STAHT (0  :比较第一个范围和第二个范围的起点;
Range.START_TO_END(l):比较第…个范围的起点和第二个范围的终点;
Range. END_TO_END (2):比较第一个范围和第二个范围的终点;
Range. END_TO_START (3  :比较第一个范围的终点和第一个范围的起点。
compareBoundaryPoints()方法可能的返回值如下:如果第一个范围中的点位于第二个范围中的
点之前，返冋-1;如果两个点相等，返回0;如果第一个范围中的点位于第二个范围中的点之后，返回
1。来看下面的例子。
var rangel = document.createRange{); var range2 = document.createRange ); var pi = document.getElement;ById("pl");
rangel.selectNodeContents(pi); range2.selectNodeContents{pi); range2.setEndBefore(pi.lastChild);
alert(rangel.compareBoundaryPoints(Range.START_TO_START, range2));	//〇
alert(rangel.comparcBoundaryPoints(Range.END_T0_END, range2));	//I
DOMRangeExampk9.htm

340 第 12 章 POM2 和 DOM3	
在这个例子中，两个范围的起点实际上是相同的，因为它们的起点都是由selectNodeContents (} 方法设置的默认值来指定的。因此,第一次比较返H 〇。但是，range2的终点由于调用setEndBeforeU 已经改变了，结果是rangel的终点位于range?的终点后面（见图12-10 )，W此第二次比较返回1。
rangdl
(	
p id~"pl"bHello/b world!/p
range2
阁 12-10
复制DOM范围
可以使用cloneRangeO方法复制范围。这个方法会创建调用它的范围的•个副本。
var newRange = range.cloneRangeO ;
新创建的范围与原来的范闱包含相同的《性，而修改它的端点+会影响原来的范围。
清理DOM范围
在使用完范围之后，最好是调用detach(方法，以便从创建范围的文档中分离出该范围。调用 detachU之后，就可以放心地解除对范围的引用，从而让垃圾回收机制回收其内存了。来看下面的 例子。
range.detach(} ;	//从文档中分离
range = null;	//解除■引用
在使用范w的最后再执行这两个步骤是我们推#的方式。一旦分离范围，就不能再恢复使用了。
12.4.2旧8及更早版本中的范围
虽然IE9支持DOM范围，但IE8及之前版本不支持DOM范围。不过，IE8及早期版本支持一种类 似的概念，即文本范围（textrange)。文本范围是IE专有的特性，其他浏览器都不支持。顾名思义，文 木范阐处理的主要是文本（不一定是DOM节点)。通过body、button、input^textarea 等这几个元索，+nT以调用createTextRangeU方法来创建文本范丨W。以K是一个例子:
var range = document.body.createTextRangeO?
像这样通过document创建的范围可以在贞面中的任何地方使用（通过其他元素创建的范围则只能 在相应的元素中使用)。与DOM范围类似，使用IE文本范围的方式也有很多种。
用旧范围实现简单的选择
选择页面中某一区域的最简单方式，就是使用范围的findTextO方法。这个方法会找到第一次出 现的给定文本，并将范闱移过来以环绕该文本。如果没有找到文本，这个方法返回false;否则返回 true。同样，仍然以下面的HTML代码为例。
p id=Mpl"bHello/b world!/p
要选择"Hello%可以使用下列代码。
var range = document.body.createTextRange(); var found = range. findText ("Hello**);
lERangeExamplel. htm

12.4 范围 341
在执行完第二行代码之后，文本"Hello•就被包围在范围之内f。为此，可以检査范闱的text属 性来确认（这个M性返问范W屮包含的文本），或者也可以检查f indText ()的返1«1值——在找到了文 本的情况卜返回值为true。例如:
alert(found);	//true
alert(range.text);	//"Hello*
还可以为findTextO传人另一个参数，即一个表示向哪个方向继续搜索的数值。负值表示应该从 当前位置向后搜索，而正值表示应该从H前位置向前搜索。因此，要査找文档中前两个"Hello"的实例, 应该使用下列代码。
var found = range. f.ind?ext ("Hello");
var foundAgain = range.findText("Hello", 1}?
IE中与DOM中的selectNode ()方法最接近的方法是moveToElementText ()，这个方法接受一 个DOM元素，并选择该元素的所有文本，包括HTML标签。下面是一个例子。
var range = document.body.createTextRangeO ; var pi = document.getElementByldCpl"); range.moveToElemenzText(pi);
fERangeExample2.htm
在文本范M中包含HTML的情况下，可以使用htmlText;属性取得范Ifl的全部内容，包括HTML 和文本，如下面的例子所示。
alert(range.htrolText);
IE的范围没有任何属性可以随着范围选区的变化而动态更新。不过，其parent Element (方法倒 是与 DOM 的 coramonAncestorCont_ainer 属性类似。
var ancestor = range.parentElement();
这样得到的父元素始终都可以反映文本选IK的父节点。
使用IE范围实现复杂的选择
在ffi中创建复杂范围的方法，就是以特定的增董向四周移动范围。为此，IE提供了 4个方法: move。、moveStart()、moveEnd(和expand"。这些方法都接受两个参数:移动单位和移动单位 的数fl。其中，移动单位是下列一种字符串值。
"character":逐个字符地移动。
"word":逐个单同（一系列非空格字符）地移动。
"sentience":逐个句子（一系列以句号、问号或叹号结尾的字符）地移动。
-textedit-:移动到当前范围选K的开始或结束位S。
通过moveStart ()方法卩J以移动范围的起点，通过moveEnd ()方法可以移动范围的终点，移动■■的 幅度fi丨单位数量指定，如下面的例子所示。
range.moveStart ("word*, 2) ;	//起点移动 2 个单iq
range.moveEnd( "character", 1);	//终点移动 1 个符
使用expand 〇方法nf以将范围规范化。换句话说，expand ()方法的作用是将任何部分选择的文 本全部选中。例如，当前选择的是一个单词中间的两个字符，调用expand ("word")可以将整个单词都 包含在范围之内。

342 第 12 章 DOM2 和 D0M3	
而move ()方法则首先会折叠当前范围（U:起点和终点相等)，然后再将范围移动指定的单位数董， 如下面的例子所示。
range.move{ "character" , S) ;	//移动 5 个字符
调用move ()之后，范围的起点和终点相N，因此必须再使用moveStart ()或moveEnd ()仓丨』建新 的选区。
操作IE范围中的内容
在IE中操作范围中的内容卩了以使用text属性或pasteHTML()方法。如前所述，通过textM性 可以取得范围中的内容文本;但是，也可以通过这个属性设置范围中的内容文本。来看一个例子。
var range = document.body.createTextRange()? range.findText("Hello"); range.text = "Howdy";
如果仍以前面的Hello World代码为例，执行以上代码后的HTML代码如下。
p id="pl"xbHowdy/b world!/p
注意，在设置text属性的情况下，HTML标签保持不变。
要向范围中插人HTML代码，就得使用pasteHTMLU方法，如下面的例子所示。
var range = document.body.createTextRange〇;
range.findText("Hello");
range.paateHTML(nemHowdy/em");
IERangeExample3. htm
执行这些代码后，会得到如下HTML。
p id= *pl • bemHowdy/emx/b world! /p
不过，在范围中包含HTML代码时，不应该使用pasteHTMLU，因为这样很容易导致不可预料的 结果——很可能是格式不正确的HTML。
折SHE范围
IE为范围提供的col lapse ()方法与相应的DOM方法用法一样:传人true把范围折叠到起点， 传人false把范围折叠到终点。例如:
range, col lapse (true);	/ /折4到起点
可惜的是，没有对应的collapsed属性让我们知道范围是否已经折叠完毕。为此，必须使用 boundingWidth属性，该属性返回范围的宽度（以像素为单位)。如果boundingWidtix属性等于0, 就说明范围已经折香了:
var isCollapsed = (range.boundingWidth == 0);
此外，还有bouhdingHeight、boundingLeft和boundingTop等属性，虽然它们都不像 boundingWidth那么有用，但也可以提供一些有关范围位置的信息。
比较IE范围
IE 中的 compareEndPoints ()方法与 DOM 范闽的 compareBoundaryPoints ()方法类似。这个 方法接受两个参数:比较的类型和要比较的范围。比较类型的取值范围是下列几个字符串值:
"StartToStart"、-StartToEnd"、_ErwiToEnd"和"EndToStart”。这几种比较类型与比较 DOM 范 围时使用的儿个值是相同的。
同样与DOM类似的是，compareEndPoints (方法也会按照相同的规则返回值，即如果第一个范 围的边界位于第二个范围的边界前面，返回-1;如果二者边界相同，返回〇;如果第一个范围的边界位 于第二个范的边界后面，返回1。仍以前面的Hello World代码为例，下列代码将创建两个范围，一个 选择"Hello world!"(包括b标签），另一个选择"Hello"。
var rangel = document.body.createTextRangeO ; var range2 = document.body.createTextRange(};
rangel.findText《"Hello world!•); range2.findText("Hello");
alert(rangel.compareEndPoints("StartToStart", range2));	//0
alert(rangel.comparaEndPoints("EndToEnd", range2));	//I
IERangeExample5.htm
由于这两个范围共享同…个起点，所以使用compareEndPoints ()比较起点返冋0。而rangel 的终点在range2的终点^]面，所以compareEndPoints (返M 1。
IE中还冇两个方法，也是用于比较范闱的:iSEqual(用于确定两个范围是否相等，inRangeO 用于确定一个范围是否包含另一个范围。下曲是相应的示例。
var rangel = document.body.createTextRange{)? var range2 = document.body.createTextRange{); rangel.findText{"Hello world"); range2.findText{"Hello");
alert (Nrangel.isBzual(ronge2): " * rangel.isBqpial (ruge2)); //false alert"rangel• InKange(range2): M -i* rangel. inRange(rangeS));	//true
lERangeExample6.htm
这个例子使用了与前面相间的范围来示范这两个方法。山于这两个范围的终点不同，所以它们不相 等，调用isEqualU返Lnl false。由于range2实际位于rangel内部，它的终点位于后者的终点之 前、起点之A7,所以range2被包含在rangel内部，调用inRange()返回true。
6.复制IE范围
在丨E中使用duplicated方法可以复制文木范围，结果会创建原范围的斗副本，如下面的例子 所示。
var newRange = range.duplicate();
新创建的范围会带有与原范围完全相同的属性。
##  12.5 小结
DOM2级规范定义了一些模块，用于增强DOM1级。“DOM2级核心”为不同的DOM类型引人了 —些与XML命名空间有关的方法。这些变化只在使用XML或XHTML文档时才有用;对于HTML文 档没有实际意义。除了与XML命名空间有关的方法外，“DOM2级核心”还定义了以编程方式创建 Document实例的方法，也支持了创建Document Type对象。
“D0M2级样式”模块主要针对操作元素的样式信息而开发，其特性简要总结如下。
- [ ] 每个元素都有一个关联的style对象，可以用来确定和修改行内的样式。
- [ ] 要确定某个元素的计算样式(包括应用给它的所有CSS规则）,nj•以使用getcomputedstyle () 方法。
EE不支持getComputedStyle (方法，但为所有兀索都提供了能够返M相同信息currentStyle 属性。
nj以通过document .styieSheets集合访问样式表。
- [ ] 除IE之外的所有浏览器都支持针对样式表的这个接口，IE也为几乎所冇相应的DOM功能提供 了 ft d的--套属性和方法。
“D0M2级遍历和范围"模块提供了与DOM结构交瓦的不同方式，简要总结如下。
- [ ] 遍历即使用Nodeiterator或TreeWalker对DOM执行深度优先的遍历。
Nodelterator是■-个简单的接U,只允许以一个节点的步幅前后移动。而TreeWalker在提 供相N功能的同时，还支持在DOM结构的各个方向上移动，包括父节点、同辈节点和子节点等 方向。
- [ ] 范围是选择DOM结构中特定部分，然后再执行相应操作的一种手段。
- [ ] 使用范围选K可以在删除文档中某些部分的同时，保持文档结构的格式良好，或者复制文档中 的相应部分。
及更早版本不支持“DOM2级遍历和范围”校块，但它提供丫一个专有的文本范围对象，可 以用来完成简中.的基丁•文本的范围操作。IE9完全支持DOM遍历。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter11.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter13.md)
