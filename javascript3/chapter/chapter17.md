##  第17章 错误处理与调试([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 理解浏览器报告的错误
- [ ] 处理错误
- [ ] 调试JavaScript代码  

S
 丁-JavaScript本身是动态语言，而且多年来一直没有固定的开发工具，因此人们普遍认为它是
—种最难T调试的编程语言。脚本出错时，浏览器通常会给出类似T “objectexpected”（缺少
对象）这样的消息，没有上下文信息，让人摸不着头脑。ECMAScript第3版致力于解决这个问题，专
门引人了 try-catch和throw语句以及一些错误类型，意在让开发人员能够适当地处理错误。几年之
后，Web浏览器中也出现了一些JavaScript调试程序和T具。2008年以来，大多数Web浏览器都已经具
备了一些调试JavaScript代码的能力。	'
在有了语言特性和工具支持之后，现在的开发人员已经能够适当地实现错误处理，并且能够找到错
误的根源。
17.1浏览器报告的错误
IE、Firefox、Safari、Chrome和Opera等主流浏览器，都具有某种向用户报告JavaScript错误的机 制。默认情况下，所有浏览器都会隐藏此类信息，毕竟除了开发人员之外，很少有人关心这些内容。 因此.在基于浏览器编写JavaScript脚本时，别忘了启用浏览器的JavaScript报告功能，以便及时收到 错误通知。
IE
IE是唯	个在浏览器的界面窗体(chrome)中ffi示JavaScript错误信息的浏览器。在发生JavaScript
错误时，浏览器左下角会出现一个黄色的图标，團标旁边则显示着"Error on page_ (贞面中有错误)。 假如不是存心去看的话，你很可能不会注意这个图标。双击这个图标，就会肴到一个包含错误消息的对 话框，其中还包含诸如行号、字符数、错误代码及文件名（其实就是你在査看的页面的URL)等相关信 息。图17-1展示了 IE的错误消息对话框。
这些信息对于一般用户还算说得过去，但对Web开发来说就远远不够了。可以通过设置让错误对 话框一发生错误就M不出来。为此，要打开“Tools”（工具）菜单中的“IntemetOptions" (Internet选项） 对话框，切换到 “Advanced”（高级）选项卡，选中 “Disp丨ay a notification about every script enror”（ S 示每个脚本错误的通知）复选框（参见图17-2 )。单击“OK”（确定）按钮保存设置。















494 第17章错误处理与调试

Tlii» n«bp«y«	«>'(xv
Pf〇bl«m» with th» w«bpa(je miqht prevcm M (runi <ji>pl»vir>q <k worfunq conecify. OouWe- click th« warning icon dbplayvd m (h« stMut b«r to **e thit	in th« Kmir«.



3	Oinw nm	Inr	»rrn<«
图 17-1
hit»i n#i_Og#iSiev'
Li f*r**Tf* ♦iMTMj AlT >c*
[」Wfiv»	(MM «m^K
R*m*	to RM^jm l<» new wndDvn *»(f >4
^7| v〇«( lett««r〇ne«um t*We .wnrw* p) •«»«(	!00、fu <«» f**«Ju*TV«>Jl/3
} Ai/Mkjbc^ 4^o<Vf〇r Vnccmct (<«>)〇•«*
^ 〇0«•	r> Httor/ P«v4nt»(*
i	iitbuggr^	£vpler*f)
J fkuM»4rrr«<M*9xq〇hi*)
j en«l»f1P f(M(r VWW<MK«» « prwnat txplc 1 tbHttr*i^nte

I « 11
阁 17-2
保存了设置之后，通常要双击黄色图标才会显示的对话
框，就会变成一有错误发生随即自动显示出来。
另外，如果启用了脚本调试功能的话（默认是禁用的)，那
么在发生错误时，你不仅会M示错误通知，而n.还会看到另一
个对话柩，询问是否想要调试错误（参见图n-3 X
要后用脚本调试功能，必须要在IE中安装某种脚本调试
器。（IE8和丨E9自带调试器。）本章后面会单独讨论调试器。
| 在IE7及更早版本中，如果错误发生在位于外部文件的脚本中，行号通常会与 误所在的行号差丨。如果是嵌入在页面中的脚本发生错误，则行号就是错误所在的 行号。



图 17-3
Firefox
默认情况下，Firefox在JavaScript发生错误时不会通过浏览器界面给出提示。但它会在后台将错误

17.丨浏览器报告的错误 495
记录到错误控制台中。单击“Tods”（工具）菜单中的“ErrorConsole”（错误控制台）可以M示错误控 制台（见围174)。你会发现，错误控制台中实际上还包含与JavaScript、CSS和HTML相关的替告和 信息，可以通过筛选找到错误。



图174
在发生JavaScript错误时，Firefox会将其记录为一个错误，包括错误消息、引发错误的URL和错误 所在的行号等信息。单击文件名即可以只读方式打开发生错误的脚本，发生错误的代码行会突出极示。
目前，最流行的Firefox插件Firebug,已经成为开发人员必备的JavaScript纠错工具。这个可以从 www.getfirebug.com下载到的插件，会在Firefox状态栏的右下角K域添加一个阁标。默认情况下，右下 角区域显示的是一个绿色对勾图标。在有JavaScript绪误发生时，图标会变成红叉，同时旁边ffl示错误 的数量。单击这个红叉会打开Firebug控制台，其中显示有错误消息、错误所在的代码行（不包含上下 文)、错误所在的URL以及行号（参见图17-5 )。	•



阁 17-5

496 第17章错误处理与调试
在Firebug中单击导致错误的代码行，将在一个新Firebug视图中打开整个脚本，该代码行在其中突
出显示。
(^\	除了显示错误之外，Firebug还有更多的用处。实际上，它还是针对Firefox的成
熟的调试环境，为调试JavaScript、CSS、DOM和网络连接错误提供了诸多功能9
Safari
Windows和Mac OS平台的Safari在默认情况下都会隐藏全部JavaScript错误。为丫访问到这些信 息，必须培用“Develop”（开发）菜单。为此，需要单击“Edit”（编辑）菜单中的“Preferences”（偏 好设置），然后在“Advanced”（高级）选项卡中，选中“Show develop menu in menubar”（在菜单栏中 显示“开发”菜单)。启用此项设置之后，就会在Safari的菜单栏中看到-个“Develop”菜单（参见 阁 17-6 )。



“Develop”菜单中提供了一些与调试有关的选项，还有一些选项可以影响当前加载的页面。单击 “Show Error Console”（显示错误控制台）选项，将会看到一组JavaScript及其他错误：控制台中显示着 错误消息、错误的URL及错误的行号（参见图17-7)。
单击控制台中的错误消息，就可以打开导致错误的源代码。除了被输出到控制台之外，JavaScript 错误不会影响Safari窗U的外观。

17.1浏览器报告的错误 497











MJ^|gj
Thrc>gliigggc〇rj<UBDl-e01 .h*:?; 11
图 17-7
Opera
Opera在默认情况下也会隐藏JavaScript错误，所有错误都会被记录到错误控制台中。要打开错误 控制台，需要单击“Tools”（工具）菜单，在“Advanced”（高级）子菜单项T面再单击“Error Console”
(错误控制台)。与Firefox—样，Opera的错误控制台中也包含了除JavaScript错误之外的很多来源（如 HTML、CSS、XML、XSLT等）的错误和警告信息。要分类査看不同来源的消息，可以使用左下角的 下拉选择框（参见图17-8)。
错误消息中显示着导致错误的URL和错误所在的线程。有时候，还会有栈跟踪信息。除了错误控 制台中显示的信息之外，没有其他途径可以获得更多信息。
也可以让Opera —发生错误就弹出错误控制台。为此，要在"Tools”（ X具)菜单中单击“Preferences” (首选项）,再单击"Advanced”（高级）选项卡，然后从左侧菜单中选择“Content”（内容)。单击“JavaScrip Options”（JavaScript选项）按钮，显示选项对话框（如图17-9所示)。
在这个选项对话框中，选中“Open console on error”（出错时打开控制台），单击“OK”（确定） 按钮。这样，每当发生JavaScript错误时，就会弹出错误控制台。另外，还可以针对特定的站点来作 此设置，方法是单击 “Tools”（工具）、“Quick Preferences”（快速参数）、“Edit She Preferences”（编 辑站点首选项），选择“Scripting”（脚本）选项卡，最后选中“Open console on error”（出错时打开 控制台）。



498 第17章错误处理与调试





MOT 3Vr^0irTT» CM
^	- txl«://lo«alb〇JC/l^/0r^2OVc}( ibQ/Br^^oD^iM/fsc Cc^Aioivtti...
ty«*t tt>re«4i click
■MM .	Ct4'»(




〇«« ■ CtcoK
»CT 3CT?0«TCT_tfr»



v* AIV/w (r»gw>9 vi
aiiumt iMsmg 9f wntfcwre
AHriw inwoimg nf«M(wknw«
fiMrm rttan^rg
Ai»w ctnpt to
>/ Alow tcnpt 1«
•y 0pC»C4MK*«
*籧*fw?ngM ct
ut«f jmScnpi «h



图 17-8	图 17-9
Chrome
与Safari和Opera —样，Chrome在默认情况下也会隐藏JavaScript错误。所有错误都将被记录到 Web丨nspector控制台中。要査看错误消息，必须打开Web Inspector。为此，要单击位于地址栏右侧的 “Control this page”（控制当前页）按钮，选择 “Developer”（开发人员）、“JavaScript console”（ JavaScript 控制台），参见图17-10。



C 〇IH®/<Wflirty%2〇Wrilny>!vV%20Ek>〇><s-Piof«s$tcnal%2C,Jer/3S«f*pt/rhif<J}#20GditioniExamp4(f?/Ch17/rhfc*wngJ：ir<x$E*
M,w inc, wh»4*w



图 17-10

17.2错误处理	499
打开的Weblnspector中包含着有关页面的信息和JavaScript控制台。控制台中显示着错误消息、错 误的URL和错误的行号（参见阁17-11 )。
.fil«：///I^Wy%70Wrilin9^»y%208oofcs^ro1e«sionalS?OJ«vftSerlt>iahir..
_ @	G U C3
• Re»oupc*» Nntwort Script* t»ne»y PtoWw Mas Ccrocie
i.S
v«body>

3玽4scfjp-"> ..</scrn </body>
;/html >

G>
[▼Stytes
d)$p^v： block; ^►籖Argin: 8px;
IKa<Bfea(Kp〇>iis
 typql Ustwief s	.
hSecnents
〇S»M>Wi
inMrrtAd
.—•;〇,
user agerrt yty1e*h«et
图 17-11
单击JavaScript控制台中的错误，就可以定位到导致错误的源代码行。
17.2错误处理
错误处理在程序设计中的重要性是勿庸置疑的。任何有影响力的Web应用程序都需要一套完善的 错误处理机制，当然，大多数校佼者确实做到了这一点，但通常只有服务器端应用程序才能做到如此。 实际上，服务器端团队往往会在错误处理机制上投人较大的箱力，通常要考虑按照类型、频率，或者其 他重要的标准对错误进行分类。这样一来，开发人员就能够理解用户在使用简单数据库査询或者报告生 成脚本时，应用程序可能会出现的问题。
虽然客户端应用程序的错误处理也同样重要，但真正受到®视，还是最近JL年的事。实际上，我们 要面对这样-个不争的事实：使用Web的绝大多数人都不是技术高手，其中甚至有很多人根本就不明 白浏览器到底是什么，更不用说让他们说喜欢哪一个了。本章前面讨论过，每个浏览器在发生JavaScript 错误时的行为都或多或少有一些差异。有的会显示小图标，有的则什么动静也没有，浏览器对JavaScript 错误的这些默认行为对最终用户而言，毫无规律可循。最理想的情况下，用户遇到错误搞不清为什么，

500 第17章错误处理与调试
他们会再试着重做一次;最糟糕的情况下，用户会恼羞成怒，一去不复返了。良好的错误处理机制可以 让用户及时得到提醒，知道到底发生了什么事，因而不会惊惶失措。为此，作为开发人员，我们必须理 解在处理JavaScript错误的时候，都有哪些手段和X具可以利用。
try-catch 语句
ECMA-262第3版引人了 try-catch语句，作为JavaScript中处理异常的一种标准方式。基本的语 法如下所示，姑而易见，这与Java中的try-catch语句是完全相同的。
try{
//可能会导致钱误的代码 } catch(error){
//在铋误发生时怎么处理
}
也就是说，我们应该把所有可能会拋出错误的代码都放在try语句块中，而把那些用于错误处理的
代码放在catch块中》例如:
window.someNonexistentPunction();
)catch (errpr){
alert("An error happened!");
如果try块中的任何代码发生了错误，就会立即退出代码执行过程，然后接着执行catch块。此 时，catch块会接收到一个包含错误信息的对象。与在其他语言中不同的是，即使你不想使用这个错误 对象，也要给它起个名字。这个对象中包含的实际信息会因浏览器而异，但共同的是有一个保存着错误 消息的message属性。ECMA-262还规定了一个保存错误类型的name属性;当前所有浏览器都支持这 个属性（Opem9之前的版本不支持这个属性)。因此，在发生错误时，就可以像下面这样实事求是地显 示浏览器给出的消息。



try {
window.someNonexistentFunction();
} catch (error){
alert(error.message);
}
TryCatchExampleOJ. him
这个例子在向用户显示错误消息时，使用了错误对象的message属性。这个message属性是唯一 一个能够保证所有浏览器都支持的属性，除此之外，IE、Firefox、Safari、Chrome以及Opera都为事件 对象添加了其他相关信息。添加了与message厲性完全相同的description属性，还添加了保存 着内部错误数最的number属性。Firefox添加了 fileName、lineNumber和stack(包含栈跟踪信息） 属性。Safari添加了 line (表示行号）、sourceld (表示内部错误代码）和sourceURL属性。当然， 在跨浏览器编程时，最好还是只使用message属性。
1. finally 子句
M然在try-catch语句中是可选的，但finally子句一经使用，其代码无论如何都会执行。换句 话说，try语句块中的代码全部正常执行，finally子句会执行;如果因为出错而执行了 catch语句

17.2错误处理	501
块，finally子句照样还会执行。只要代码中包含finally子句，则无论try或catch语句块中包 含什么代码	甚至return语句，都不会阻止finally子句的执行。来看下面这个函数。
function testFinally(){ try {
return 2;
} catch (error){ return 1;
} finally ( return 0;
}
TryCatchExampie02.htm
这个函数在try-catch语句的每一部分都放了-条return语句。表面上看，调用这个函数会返 M2,因为返回2的return语句位于try语句块中，而执行该语句又不会出错。可是，由于最后还有 一个finally子句，结果就会导致该return语句被忽略;也就是说，调用这个函数只能返回0。如杲 把finally子句拿掉，这个函数将返回2。
如果提供finally子句，则catch子句就成了可选的（catch或finally有一个即可)。IE7及 更早版本中有一个bug:除非有catch子句，否则finally中的代码永远不会执行。如果你仍然要考 虑IE的早期版本，那就只好提供一个catch子句，哪怕里面什么都不写。IE8修复了这个bug。
者务必要只中包含finally子句，那么无论try还是catch语句块 中的return语句都将被忽略。因此，在使用finally子句之前，一定要非常清楚你想让代 码怎么样。
2.错误类型
执行代码期间可能会发生的错误有多种类型。每种错误都有对应的错误类型，而当错误发生时，就 会抛出相应类型的错误对象。ECMA-262定义丫下列7种错误类型：
Error
EvalError
RangeError
ReferenceError
SyntaxError
TypeError
URXKrror
其中，Error是基类型，其他错误类型都继承自该类型。因此，所有错误类型共享了一组相同的属 性（错误对象中的方法全是默认的对象方法)。Error类型的错误很少见，如果有也是浏览器抛出的; 这个基类型的主要目的是供开发人员抛出自定义错误。
EvalError类型的错误会在使用eval <>函数而发生异常时被抛出。ECMA-262中对这个错误有如 下描述：“如果以非直接调用的方式使用eval属性的值（换句话说，没有明确地将其名称为作一个 Identifier,即用作 CallExpression 中的 MemberExpression),或者为 eval 属性賦值。”简单 地说，如果没有把evalU当成函数调用，就会抛出错误，例如：

502 第17章錯误处理与调试
new eval (); "抛出 EvalError eval a foo; //4fe出 KvalError
在实践中，浏览器不一定会在应该抛出错误时就抛出EValError。例如，Firefox4+和〖E8对第一 种情况会抛出TVPeError,而第二种情况会成功执行，不发生错误。有鉴于此，加上在实际开发中极 少会这样使用eval (),所以遇到这种错误类塑的可能性极小。
RangeError类型的错误会在数值超出相应范时触发。例如，在定义数绀时，如果指定了数组不 支持的项数（如-20或NUmber.MAX_VALl;E),就会触发这种错误。F面是具体的例子。
var itemsl = new Array{-20);	//ifeife RangeError
var it:ems2 = new Array (Number ,MAX_VALUE) ;	//抛出 RangeError
JavaScript中经常会出现这种范围错误。
在找不到对象的情况下，会发生ReferenceError (这种情况下，会直接导致人所共知的-object expected”浏览器错误)。通常，在访问不存在的变量时，就会发生这种错误，例如：
var obj = x;	//在x并未声明的情况下相出ReferenceError
至于SyntaxError,.当我们把语法错误的JavaScript字符申传人eval ()函数时，就会导致此类错 误。例如：
eval ("a ++ b") ;	//槐出 SyritaxError
如果语法错误的代码出现在evalO函数之外，则不太可能使用SyntaxError，因为此时的语法错 误会导致JavaScript代码立即停止执行。
TVpeError类型在JavaScript中会经常用到，在变M中保存着意外的类型时，或者在访问不存在的 方法时，都会导致这种错误。错误的原因虽然多种多样，但归根结底还是由于在执行特定于类型的操作 时，变tt的类型并不符合要求所致。下面来看几个例子。
var 〇 = new 10;	//抛出 TypeError
alert ("name" in true) ;	//塊出 TypeError
Function.prototype. toString.call (Tiame");	//抵出 TypeError
最常发生类《错误的情况，就娃传递给函数的参数啦先未经检査，结果传人类型与预期类型不相符。 在使用encodeURI ()或decodeURI {)，rftf URI格式不1H确时，就会导致URIError错误。这种 错误也很少见，因为前面说的这两个函数的容错性非常高。
利用不同的错误类型，可以获悉更多有关异常的信息，从而有助于对错误作出恰当的处理。要想知 道错误的类型，可以像下面这样在try-catch语句的catch语句中使用instanceof操作符。
try {
someFunction()?
} catch (error){
if (error in*tanc©o£ TypeError){
//处理类塱械谈
)else if (error inatanceof ReferenceError){
//处理引用嫌谈
elso {
//处a其他典型的嫌误
>
>

17.2错误处理	503
在跨浏览器编程中，检査错误类型是确定处理方式的最简便途径;包含在message属性中的错误 消息会因浏览器而异。
合理使用try-catch
当try-catch语句中发生错误时，浏览器会认为错误已经被处理了，因而不会通过本章前面讨论 的机制记录或报告错误。对于那邱不要求用户懂技术，也不需要用户理解错误的Web应用程序，这应 该说是个理想的结果。+过，try-catch能够让我们实现自己的错误处理机制。
使用try-catch最适合处理那些我们无法控制的错误。假设你在使用一个大型JavaScript库中的 函数，该函数可能会有意无意地抛出一些错误。由于我们不能修改这个库的源代码，所以大可将对该函 数的调用放在try-catch语句当中，万一有什么错误发生，也好恰当地处理它们。
在明明內内地知道自己的代码会发生错误时，再使用try-catch语句就不太合适了。例如，如果 传递给函数的参数是字符串而非数值，就会造成函数出错，那么就应该先检查参数的类型，然后再决定 如何去做。在这种情况下，不应用使用try-catch语句。
17.2.2抛出错误
与try-catch语句相配的还有一个throw操作符，用于随时抛出自定义错误。抛出错误时，必须 要给throw操作符指定一个值，这个值是什么类型，没有要求。下列代码都是有效的。
throw 12345;
throw "Hello world!■;
throw true;
throw { name： "JavaScript"};
在遇到throw操作符时，代码会立即停止执行。仅当有try-catch语句捕获到被抛出的值时，代 码才会继续执行。
通过使用某种内置错误类型，可以更真实地模拟浏览器错误。每种错误类型的构造阐数接收一个参 数，即实际的错误消息。下面是一个例子。
throw new Error{"Something bad happened.");
这行代码抛出了--个通用错误，带有一条自定义错误消息。浏览器会像处理自己生成的错误一样， 来处理这行代码抛出的错误。换句话说，浏览器会以常规方式报告这一错误，并且会显示这里的自定义 错误消息。像下面使用其他错误类型，也可以模拟出类似的浏览器错误。
throw new SyntaxError("I don#c like your syntax.");
throw new TypeError(*What type of variable do you take me for?");
throw new RangeError(-Sorry, you just don，t have the range.-);
throw new EvalError('That doesn^t evaluate.■);
throw new URIError("Uri, is that you?-);
throw new ReferenceError{"You didn't cite your references properly.");
在创建fl定乂错误消息时最常用的错误类细是Error、KangeError、ReferenceError和TypeError。 另外，利用原型链还可以通过继承Error来创建自定义错误类塑（原型链在第6章中介绍)。此时， 需要为新创建的错误类咽指定name和message属性。来看一个例子。
function CustomError(message){ this.name = "CustomError*; this.message = message;

504 第17章錯误处理与调试
CusCoroError.prototype = new SrrorO; throw r.ew CustomError {"My message*);
ThrowingErrorsExampleOJ. htm
浏览器对待继承A Error的定义错误类型，就像对待其他错误类型一样。如果要捕获自d抛出 的错误并且把它与浏览器错误IX:别对待的话，创迚定义错误是很冇用的。
IE只有在抛出Error对象的时候才会显示自定义饼谋消息。对于其他类型，它 郝无一例外地显示"exception thrown and not caught*1 (抛出了异常，且未被
捕获）。
抛出错误的时机
要针对阑数为什么会执行失败给出吏多信息，抛出自定义错误是一种很方便的方式。应该在出现某 种特定的已知错误条件，导致函数无法正常执行时抛出错误。换句话说，浏览器会在某种特定的条件下 执行函数时抛出错误。例如，下面的函数会在参数不是数组的情况下失败。



function process(values){ values.sort{);
for (var i=0, len-values.length; i < len; i++){ if (values(i) > 100){ return values[i];
>
return -1;
ThrowmgErrorsExample02. htm
如果执行这个函数时传给它一个字符屯参数，那么对sort ()的调用就会失败。对此，不同浏览器 会给出不同的错误消息，但都不是特别明确，如下所示c - [ ] 丨E:厲性或方法不存在。
Firefox: values.sort ()不是闲数。
Safari:值undefined (表达式values.sort的结果）不是对象。
Chrome:对象名没有方法,sort.。
Opera:类型不匹配（通常是在需要对象的地方使用f非对象值)。
尽管Firefox、Chrome和Safari都明确指出了代码中导致错误的部分，伹错误消总并没有清楚地告 诉我们到底出了什么问题，该怎么修奴问题。在处理类似前面例子中的那个函数时，通过调试处理这些 错误消息没冇什么困难。倂是，在面对包含数T•行JavaScript代码的复杂的Web应用程序时，要想査找 错误来源就没有那么容易了。这种悄况下，带有适3信息的『丨定义错误能够敁著提升代码的可维护性。 来看下M的例子。

17.2错误处理	505



function process(values){
if (1(values inatanceof Array)}{
throw new Error(nprocess()s Argument must be an array.")?
values•sort 〇;
for (var i=0, len=valucs.length; i < ien; i++>{ if <values[i] > 100){ return valuesfi];
return -1;
ThrowingEnvrsExcmpie02.htm
在重写后的这个函数中，如果values参数不是数组，就会抛出一个错误。错误消息中包含了函数 的名称，以及为什么会发生错误的明确描述。如果•个复杂的Web应用程序发生了这个错误，那么査 找问题的根源也就容易多了。
建议读者在开发JavaScript代码的过程中，重点关注函数和可能导致函数执行失败的因素。良好的 错误处理机制应该可以确保代码中只发生你自己抛出的错误。
在多框架环境下使用
『22丄1节。
instanceof来检测数组有一些问题。详细内容请参考
抛出错误与使用try-catch
关于何时该抛出错误，而何时该使用try-catch来捕获它们，是一个老生常谈的问题。一般来说, 应用程序架构的较低M次中经常会抛出错误，但这个M次并不会影响.当前执行的代码，因而错误通常得 不到真正的处理。如果你打算编写一个要在很多应用程序中使用的JavaScript库，甚至只编写一个nj•能 会在应用程序内部多个地方使用的辅助函数，我都强烈建议你在抛出错误时提供详尽的信息。然后，即 可在应用程序中捕获并适当地处理这些错误。
说到抛出错误与捕获错误，我们认为只应该捕获那件你确切地知道该如何处理的错误。捕获错误的 目的在于避免浏览器以默认方式处理它们;而抛出错误的目的在于提供错误发生具体原W的消息。
错误（error)事件
任何没有通过try-catch处理的错误都会触发window对象的error事件。这个亊件是Web浏览 器最早支持的亊件之一，IE、Firefox和Chrome为保持向后兼容，并没有对这个事件作任何修改（Opera 和Safari不支持error事件)。在任何Web浏览器中，onerror事件处理程序都不会创建event对象， 但它可以接收三个参数：错误消息、错误所在的URL和行号。多数情况只有错误消息有用，因为 URL只是给出了文档的位置，而行号所指的代码行既可能出自嵌人的JavaScript代码，也可能出自外部 的文件。要指定onerror事件处理程序，必须使用如下所示的DOMO级技术，它没有遵循“DOM2级 事件”的标准格式。

506 第17章错误处理与调试
window.onerror - function(message, url, line)< alert(message);
>;
只要发生错误，无论足不是浏览器生成的，都会触发error亊件，并执行这个事件处理程序。然 后，浏览器默认的机制发挥作用，像往常一样i示出错误消息。像下面这样在事件处理程序中返回 false, n了以阻止浏览器报告错误的默认行为。
window.onerror = function(message, url, line){ alert(message); return false;
>;
QnErrorExample01.htm
通过返回false,这个函数实际上就充当了整个文挡中的try-catch语句，可以捕获所有无代码 处理的运行时错误。这个班件处理程序是避免浏览器报告销误的最后一道防线，理想情况下，只要可能 就不应该使用它。只要能够适当地使用try-catch语句，就不会有错误交给浏览器，也就不会触发 error事件。
浏览器在使用这个事件处理错误时的方式有明显不同。在IE中，即使发生error 事件，代码仍然会正常执行;所有变量和数据都将得到保留，因此能在onerror事 件处理程序中访问它们。但在Firefox中，常规代码会停止执行，事件发生之前的所 有变量和数据都将被销毁，因此几乎就无法判断错误了。
图像也支持error事件。只要阁像的src特性中的1不能返回可以被识别的图像格式，就会触 发error事件。此时的error事件遵循D0M格式，会返W—个以图像为目标的event对象。下面是 一个例子。
var image = new linage () ?
EventUtil.addHandler(image, •load", function(event){ alert("Image loaded!");
});
EventUtil.addHandler(image, "error", function(event){ alert("Image not loaded!"};
>);
image.src = •smilex.gif"; //指定不存在的文件
QnErrorExample02. him
在这个例+中，当加载图像失败时就会显示•个背告框。需要注意的是，发生error亊件时，图 像T载过程已经结束，也就是说不能再重新F载了。
17.2.4处理错误的策略
过去，所谓Web应用程序的错误处理策略仅限于服务器端。在谈到错误与错误处理时，通常要考 虑很多方面，涉及一些工ft,例如记朵和监控系统。这拽工具的用途在于分析错误模式，追査错误原因， 同时帮助确定错误会影响到多少用户。

17.2错误处理	507
在Web应用程序的JavaScript这一端，错误处理策略也同样重要。由于任何JavaScript错误都可能 导致网页X法使用，W此搞淸楚何时以及为什么发生错误至关重要。绝大多数Web应用程序的用户都 不懂技术，遇到错误时很容易心烦意乱。有时候，他们可能会刷新页面以期解决问题，而有时候则会放 弃努力。作为开发人员，必须要知道代码何时可能出错，会出什么错，同时还要有-个跟踪此类问题的 系统。
17.2.5常见的错误类型
错误处理的核心，是疗先要知道代码里会发生什么错误。由于JavaScript是松散类型的，而且也不 会验证闲数的参数，因此错误只会在代码运行期间出现。一般来说，需要关注三种错误：
- [ ] 类型转换错误 - [ ] 数据类型错误 - [ ] 通信错误
以上错误分别会在特定的模式下或者没冇对值进行足够的检査的情况下发生。
类型转换错误
类沏转换错误发生在使用某个操作符，或者使用其他可能会自动转换值的数据类型的语言结构时。 在使用相等（=)和不相等（!=)操作符，或者在if、for及while等流控制语句中使用非布尔值时， M常发生类玴转换错误。
第3章讨论的相等和不相等操作符在执行比较之前会先转换不同类型的值。由于在非动态语言中， 开发人M都使用相同的符3执行A观的比较，W此在JavaScript中往往也会以相N方式错误地使用它们。 多数情况下，我们建议使用全等（=)和不全等（!==)操作符，以避免类型转换。来肴-个例子。
这里使用了相等和全等操作符比较了数值5和7•符串"5•。相等操作符首先会将数值5转换成字符 串"5",然后再将其与另一个字符华"5"进行比较，结果是true。全等操作符知逍要比较的是两种不同 的数据类型，因而貞接返M false。对于1和true也是如此：相等操作符认为它们相等，而全等操作 符认为它们不相等。使用全等和非全等操作符，可以避免发生因为使用相等和不相等操作符引发的类逛 转换错误，因此我们强烈推荐使用。
容易发生类型转换错误的另一个地方，就是流控制语句。像if之类的语句在确定下一步操作之前, 会II动把任何值转换成布尔值。尤其是if语句，如果使用不当.最容易出错。来看下面的例子。
function concat(strl, str2, sc.r3) {
var resulc. = scrl <■ str2 ;
if (str3) i	//绝对不要这样！ J !
result += str3;
)
return result;
这个函数的用意是拼接两或三个字符串，然汨返回结果。其中，第三个字符中是可选的，闶此必须 要检丧。第3章呰经介绍过，未使用过的命名变M会动被赋了，undefined值。而undefined值可以 被转换成布尔值false,因此这个涵数屮的if语句实际上只适用于提供了第三个参数的情况。问题在
alert(5 == "5");
alert(5 === "5");
alert(1 == true);
alert(1 === true)
//true
//false
//true
//false

508 第17章错误处理与调试
于，并不是只有undefined才会被转换成false,也不是只冇字符率值才可以转换为true。例如， 假设第三个参数是数{ft 0,那么if语句的测试就会失败，而对数值1的测试则会通过。
在流控制语句中使用非布尔值，是极为常见的一个错误来源。为避免此类错误，就要做到在条件比 较时切实传入布尔值。实际上，执行某种形式的比较就可以达到这个目的。例如，我们可以将前面的函 数歌写如下。
function concat(strl, str2, str3){ var result - strl + str2;
if (typeof str3 == "string") {	//恰当的在匕杖
result += str3;
>
return result?
在这个重写后的凼数中，if语句的条件会基于比较返N—个布尔值。这个函数相对可靠得多，不 容易受非正常值的影响。
数据类型错误
JavaScript是松散类型的，也就是说，在使用变量和函数参数之前，不会对它们进行比较以确保它 们的数据类型正确。为了保证不会发生数据类型错误，只能依靠开发人员编写适当的数据类型检测代码。 在将预料之外的值传递给函数的情况下，最容易发生数据类铟错误。
办:前面的例子中，通过检测第三个参数可以确保它是一个字符串，但是并没有检测另外两个参数。 如果该函数必须要返回一个字符串，那么只要给它传人两个数值，忽略第三个参数，就可以轻易地导致 它的执行结果错误。类似的情况也存在于下面这个闲数屮。
//不安全的禹数，任何非字符_值郝会导致错误
function getQueryString(url){ var pos a url.indexOf{"?"}? if (pos > -1){
return url.substring(pos +1);
}
return "*;
}
这个函数的用意是返M给定URL中的査询字符串。为此，它首先使用indexOf ()寻找字符串中的 问号。如果找到了，利用substring(>方法返回问号ftT面的所有字符串。这个例子中的两个函数只能 操作字符串，因此只要传人其他数据类型的值就会导致错误。而添加一条简竽的类型检测语句，就可以 确保函数不那么容易出错。
function getQueryString(url){
if (typeof url == "string**){	//通过址查类型确保安全
var pos = url. .indexOf (^?"); if (pos > -1)(
return url.substring(pos +1);
}
>
return "•?
}
重写后的这个函数首先检丧了传人的值是不是字符串。这样，就确保了函数不会因为接收到非字符 串值而导致错误。

17.2錯误处理	509
前一节提到过，在流控制语句中使用非布尔值作为条件很容易导致类型转换错误。同样，这样做也 经常会导致数据类铟错误。来# RW的例+。
//不安全的*数，任何非数組值都会导致钳误 function reverscSort: (values) {
if (values) {	//绝对不要这样！ ！ ！
values.sort(); values•reverse(};
>
)
这个reverseSort ()涵数nj以将数组反向排序，中用到了 sort ()和reverse 〇方法。对于if 语句中的控制条件而言，任何会转换为true的非数组值都会导致错误。另一个常见的错误就是将参数 与null值进行比较，如下所示0
//不安全的函数，任何非敫组值都会导致错误 function reverseSort(values){
if (values 1= null) {	//絶对不要这样Ul
values.sort 0; values.reverse();
与mill进行比较只能确保相应的值不是mall和undefined (这就相当于使用相等和不相等操 作)。要确保传人的值布效，仅检测mill值是不够的;因此，不应该使用这种技术。同样，我们也不 推荐将某个值与undefined作比较。
另一种错误的做法，就是只针对要使用的某-个特性执行特性检测。来看下面的例子。
//还是不安全，任何非数组值都会导致辑误 function reverseSort(values){
if (typeof values.sort == "function") {	//绝对不要这样！！ 1
values.sore()? values.reverse();
在这个例子中，代码IT先检测f参数中是否存在sort ()方法。这样，如果传人一个包含sort (> 方法的对象（而不是数组）当然也会通过检测，佴在调用reversed函数时可能就会出错了。在确切 知道应该传人什么类型的情况下，最好是使用instanceof来检测其数据类型，如K所示。
//安全，非数组值将被忽略 function reverseSort(values){
i£ (values instanceof Array) {	//问題解决了
values.sort(); values.reverse()?
}
}
最后• -^reverseSort <1威数是安全的：它检测了 values,以确保这个参数是Array炎壞的实 例。这样一来，就可以保证函数忽略任何非数组值。
大体上来说，基本类型的值应该使用typeof来检测，而对象的值则应该使用instanceof来检测。 根据使用函数的方式，有时候并不需要逐个检测所有参数的数据类型。但是，面向公众的API则必须无 条件地执行类型检査，以确保函数始终能够正常地执行。

510 第丨7章错误处理与调试
通信错误
随着Ajax编程的兴起（第21章讨论Ajax), Web应用程序在其生命周期内动态加载信息或功能， 已经成为一件司空见惯的亊》不过，JavaScript与服务器之间的任何一次通信，都有可能会产生错误。
第一种通信错误与格式不iF.确的URL或发送的数据有关。最常见的问题是在将数据发送给服务器 之前，没有使用encodeURIComponent (>对数据进行编码。例如，下面这个URL的格式就是不正确的：
http: //

www.yourdomain.com/?redir=http： //www. soitieotherdomain.com?a-b&c=d
针对_redir=”后面的所有字符串调用encodeURIComponentO就可以解决这个问题，结果将产生 如下字符串：
http： //www. yourdomain.c〇m/?redir=http%3A%2F%2Fwww.someotherdomain.coin%3Fa%3Db%26c%3Dd
对于査询卞符串，应该记住必须要使用encodeURIComponentU方法。为确保这一点，有时候 可以定义一个处理査询字符串的函数，例如：
function addQueryStringArg<url# name, value){ if (ur1.indexOf{"?") == -1){ url += " ? ;
} else {
url += "&";
}
url += encodeURIComponent(name) +	+ encodeURIComponent(value);
return url;
>
这个函数接收三个参数：要追加查询字符串的URL、参数名和参数值。如果传人的URL不包含问 号，还要给它添加问号;否则，就要添加一个和兮，因为有问号就意味着有其他査询字符串。然后，再 将经过编码的査询字符串的名和值添加到URL后面。可以像下而这样使用这个函数：
var url = "

http://www.somedomain.com";
var newUrl = addQueryStringArg(url, "redir",
■http： / /www.sorr.eotherdorrain.com?a=b&c=d")
alert(newUrl);
使用这个闲数而不是手T构建URL,可以确保编码正确并避免相关错误。
另外，在服务器响应的数据不正确时，也会发生通信错误。第10章曾经讨论过动态加载脚本和动 态加载样式，运用这两种技术都有可能遇到资源不可用的情况。在没有返回相应资源的情况下，Firefox、 Chrome和Safari会默默地失败，IE和Opera则都会报错。然而，对于使用这两种技术产生的错误，很 难判断和处理。在某些情况下，使用Ajax通信可以提供有关错误状态的更多信息。
(^\ 在使用Ajax通信的情况下，也可能会发生通信钱误。相关的问题和错误将在第 2!幸讨论。
17.2.6区分致命错误和非致命错误
任何错误处理策略中最重要的一个部分，就是确定错误是否致命。对于非致命错误，可以根据下列 一或多个条件来确定：

17互错误处理	511
- [ ] 不影响用户的主要任务;
- [ ] 只影响页面的一部分;
口可以恢
- [ ] 重复相间操作可以消除错误。
本质上，非致命错误并不是需要关注的问题。例如，Yahoo! Mail (http://mail.yahoo.com)有一项功 能，允许用户在其界面t:发送手机短信。如果丨丨丨于某种原因，发不/手机短信了，那也不算是致命错误, 因为并不是应用程序的主要功能有问题。用户使用Yahoo! Mail主要是为了査收和撰写电子邮件。只在 这个主要功能正常，就没有理山打断用户。没有必耍W为发生了非致命错误而对用户给出提示一可以 把页面中受到影响的区域替换掉，比如替换成说明相应功能无法使用的消息。但是，如果因此打断用户， 那确实没W必要。
致命错误，可以通过以下一或多个条件来确定：
- [ ] 应用程序根本无法继续运行;
- [ ] 错误明显影响到了用户的关要操作;
- [ ] 会导致K他连带错误。
要想采取适当的措施，必须要知道JavaScript在什么情况下会发生致命错误。在发生致命错误时， 应该立即给用户发送一条消息，告诉他们无法#继续7•头的亊情了。假如必须刷新页面才能让应用程序 正常运行，就必须通知用户，间时给用户提供一个点击即可刷新页面的按钮。
K分非致命错误和致命错误的主要依据，就娃看它们对用户的影响。设计良好的代码，可以做到应 用程序某一部分发生错误不会不必要地影响另一个实际h毫不相十的部分。例如，My Yahoo! (http://my.yahoo.com)的个性化主贝上包含了很多互不依赖的模块。如果每个模块都需要通过JavaScript 调用来初始化，那么你可能会看到类似下面这样的代码：
for (var i=0, len-mods.length; i < len? i++){ mods[i] .init();//可能会导致致命播误
)
表面h看，这些代码没什么问题:依次对每个模块调用init (>方法。问题在丁，任何模块的init () 方法如果出错，都会导致数组中后续的所有模块无法再进行初始化。从逻辑上说，这样编写代码没有什 么意义。毕竞，每个模块相互之间没有依赖关系，各自实现不同功能。可能会导致致命错误的原因是代 码的结构。不过，经过下面这样修改，就可以把所有模块的错误变成非致命的：
for (var i*0, len-mods.length; i < len; i++){ try {
mods ti J.in.it ();
> catch (ex) {
//在这里处理错误
}
}
通过在for循环屮添加tiy-catch语句，任何模块初始化时出错，都不会影响其他模块的初始化。 在以上重写的代码中，如果有错误发生，相应的错误将会得到独立的处理，并不会影响到用户的体验。
17.2.7把错误记录到服务器
17
开发Web应用程序过程中的一种常见的做法，就是集屮保存错误H志，以便査找5:要错误的原因。

512 第17章错误处理与调试
例如数据库和服务器错误都会定期写人R志，而fl会按照常用API进行分类。在复杂的Web应用程序 中，我们M样推荐你把JavaScript错误也冋写到服务器。换句话说，也要将这些错误写人到保存服务器 端错误的地方，只不过要标明它们来fi前端。把前后端的错误集中起来，能够极大地方便对数据的分析。
要诖立这样一种JavaScript错误记录系统，首先需要在服务器上创建一个页面（或者一个服务器人 II点）.用于处理错误数据。这个贞面的作用无非就是从査询字符串中取得数据，然后再将数据写人错 误H志中。这个页面可能会使用如下所示的函数：
function logError(sev, msg){ var img = new Image(};
img.src = "log.php?sev=" + encodeURIComponent(sev) + "&msg=" + encodeURIComponent(msg);
}
这个]ogError ()函数接收两个参数：表示严敢程度的数值或字符串（视所用系统而异）及错误消 息。K中，使用了 image对象来发送请求，这样做非常灵活，主要表现如下几方面。
- [ ] 所有浏览器都支持Image对象，包括那些不支持XMLHttpRecjuest对象的浏览器。
- [ ] 可以避免跨域限制。通常都是一台服务器要负责处理多台服务器的错误，而这种情况下使用 XMLHttpRequost 是不行的。
- [ ] 在记录错误的过程中出问题的概率比较低。大多数Ajax通信都是由JavaScript库提供的包装函 数来处理的，如果库代码本身存问题，而你还在依赖该库记录错误，可想而知，错误消息是不 可能得到记录的。
只要是使用try-catch语句，就应该把相应错误记录到日志中。来看下面的例子。
for (var i=0, len=roods.length; i < len; i++){ cry {
mods[i】•init(};
} catch (ex){
logBrror("nonfatal", "Module init failed: H <f ex,message);
在这M, —旦模块初始化失败，就会调用logErr〇r()。第一个参数是’nonfatal，（非致命），表 示错误的严重程度。第二个参数是h下文信息加上真正的JavaScript错误消息。记录到服务器中的错误 消息应该尽可能多地带有上下文信息，以便鉴别导致错误的真正原因。
17.3调试技术
在不那么容易找到JavaScript调试程序的年代，开发人员不得不发挥CJ己的创造力，通过各种方法 来调试〖丨己的代码。结果，就出现了以这样或那样的方式置入代码，从而输出调试信息的做法。其中， 最常见的做法就是在要调试的代码中随处插人alert ()函数。但这种做法一方曲比较麻烦（调试之后还 需要淸理)，另一方面还吋能引人新问题（想象一下把某个alert (>函数遗留在产品代码中的结果)。如 今，已经有T很多更好的调试工具，因此我们也不再建议在调试中使用alert (> 了。
17.3.1将消息记录到控制台
IE8、Firefox、Opera、Chrome 和 Safari 都有 JavaScript 控制台，可以用来査看 JavaScript 错误。而

17.3调试技术	513
且，在这些浏览器中，都可以通过代码向控制台输出消总。对Fircfox而言，需要安装Firebug (www.getfirebug.com),因为 Firefox 要使用 Firebug 的控制台。对 IE8、Firefox、Chrome 和 Safari 来说， 则可以通过console对象向JavaScript控制台中写人消息，这个对象具有下列方法。
error(inessage):将错误消息记录到控制台
info (message):将信息性消息记录到控制u
log (message):将一般消息记录到控制台
warn (message):将警告消息记录到控制台
在IE8、Firebug、Chrome和Safari中，用来记录消息的方法不同，控制台中显示的错误消息也不 一样。错误消息带存红色图标，而警告消息带有黄色图标。以下函数展示了使用控制台输出消息的一 个示例。
function sum(numl, num2){
console.log(_Entering sum{), arguments are " + numl + •,_ + num2);
console.log("Before calculation"); var result = numl + num2; console.log{"After calculation"};
console.log{"Exiting sum()"); return result;
}
在调用这个sumU函数时，控制台中会出现一些消息，可以用来辅助调试。在Saferi中，通过 “Develop”（开发）菜单可以打开其JavaScript控制台（前面讨论过);在Chrome中，单击“Control this page”（控制当前页）按钮并选择“Developer”（开发人员）和“JavaScript console”（JavaScript控制台） 即可;而在Firefox中，要打开控制台®要单击Firefox状态栏右F角的图标。m8的控制台是其Developer TooM开发人员工具）扩展的一部分，通过“Too丨s”（工具）菜单可以找到，其控制台在“Script”（脚 本）选项卡中。
Opera 10.5之前的版本中，JavaScript控制台可以通过opera.postError{)方法来访问。这个方法 接受一个参数，即要写入到控制台中的参数，其用法如下。
function sum(numl, num2){
opera*postBrror("Entering sum(), arguments are _ * numl * m,m * num2)/
opera*poatSrror("Before calculation*);
var result = numl + num2;
opera.postError("After calculation");
opera«postError("Exiting eua()n);
return result;
别看opera.postError()方法的名字好像是只能输出错误，但实际上能通过它向JavaScript控制 台中写人任何信息。
还有一种方案是使用LiveConnect,也就是在JavaScript中运行Java代码。Firefox、Safari和Opera 都支持LiveConnect,因此可以操作Java控制台。例如，通过下列代码就可以在JavaScript中把消息写 人到Java控制台。
java.lang.System.out.println("Your message");

514 第17章错误处理与调试
可以用这行代码替代console.log()或opera.postError (}，如下所7K〇
function sum(mur.l, nuin2) {
java.lang.System.out.println("Bntering b\uq(), arguinsnte are " + numl + M," + nuin2);
java*lang.System.out.println("Before calculation0);
var result - numl + r.um2;
java.lang.System.out.println("After calculation");
j&v&.lang.Sy8tem.out.println("Exiting Bum<)”;
return result;
>
如果系统设置恰■当，吋以在调用LiveConnect时就立即显示Java控制台。在Firefox中，通过揟ools?(工具）菜单可以打开Java控制台;在Opera中，要打开Java控制台，可以选择菜单揟ools?（工具） 及揂dvanced?（高级)。Safari没有内置对Java控制台的支持，必须年-独运行。
不存在一种跨浏览器向JavaScript控制台写人消息的机制，但下面的函数倒可以作为统一的接口。
function log(message){
if (typeof console == "object"){ console.log(message);
} else if (typeof opera == "object" opera.postError(message);
} else if (typeof java == "object" && typeof java.lang == "object")?java.lang.System.out.println(message);
}
ConsofeLoggingExample01.htm
这个log()函数检测了哪个JavaScript控制台接U可用，然后使用相应的接口。可以在任何浏览器 中安全地使用这个函数，不会导致任何错误，例如：
function, sum (numl, num2> {
log("Entering sum(), arguments ara " * numl 4 "# 0 + num2);
log(**Before calculation"); var result = numl + num2; log("After calculation");
log(MBxiting 8\im()"); return result;
ConsoleLoggingExampleOl. htm
向JavaScript控制台中写人消息可以辅助调试代码，但在发布应用程序时，还必须要移除所有消息。 在部署应用程序时，可以通过手丁或通过特定的代码处理步骤来ft动完成清理T作。
记录消息要比使用alert ()函数更可取，因为警告框会阻断程序的执行，而在测 定异步处理对时间的影响时，使用警告框会影响结果。

17.3调试技术 515
17.3.2将消息记录到当前页面
另一种输出调试消息的方式，就是在页面中开辟一小块冈域，用以显示消息。这个区域通常是一个 元索，而该元素可以总是出现在页面屮，但仅用于调试H的;也可以是一个根据®要动态创建的元素。 例如，可以将log ()函数修改为如下所示：
function log{message){
var console = document.getElementById("dobuginfo")
if (console === null){
console = document.createElement("div");
console.id = "debuginfo";
console.style.background = "#dedede"?
console.style.border = Mlpx solid silver";
console.style.padding = "Spx";
console.style.widch = "400px";
console.style.position = "absolute";
console.style.right = •Opx";
console.style.Cop = "Opx";
docxufient. body. appendChi Id (console);
}
console.innerHTML += _<p>” + message + "</p>";
17
PageLoggingExampleOl. htm
这个修改后的log U函数首先检测是否已经存在调试元素，如果没有则会新创建一个<<^^>元索， 并为该元素应用一些样式，以便与页面中的其他元索区别开。然后，乂使用innerHTML将消息写人到 这个<div>元素中。结果就是页面中会有一小块区域显示错误消息。这种技术在不支持JavaScript控制 台的IE7及更早版本或其他浏览器中十分有用。
与把锊谈消息记录到控制台相似，把褚误消息榆出到瓦面的代码也要在发布前
鐲除。
17.3.3抛出错误
如前所述，抛出错误也是一种调试代码的好办法。如果错误消息很具体，基本上就可以把它当作确 定错误来源的依据。但这种错误消息必须能够明确给出导致错误的原因，才能省去其他调试操作。来看 下面的函数：
function divide(numl, num2){ return numl / num2;
>
这个简单的函数计算两个数的除法，但如果有一个参数不是数值，它会返回NaN。类似这样简单的 计算如果返回NaN,就会在Web应用程序中导致问题。对此，可以在计算之前，先检测每个参数是杏都 是数值。例如：
function divide(numl, nuro2){
if (typeof numl Is "niiober" || typeof nux&2 ! = "number") {
throw new Error("divide<): Both argiunents snxst be numbers.");
>
return numl / num2;




516 第17章错误处理与调试
在此，如果有一个参数不是数值，就会抛出错误。错误消息中包含了函数的名字，以及导致错误的 真正原W。浏览器只要报告了这个错误消息，我们就可以立即知道错误来源及问题的性质。相对来说， 这种具体的错误消息要比那些泛泛的浏览器错误消息更有用。
对干大迆应用程序来说，自定义的错误通常都使用assertO函数抛出。这个阐数接受两个参数， 一个是求值结果应该为true的条件，另-个是条件为false时要抛出的错误。以下就是一个非常基本 的assert U闲数。
function assert(condition, message){ if (!condition){
throw new Error(message);
}
}
AssertExampleOLhtm
可以用这个assert ()函数代替某些函数中需要调试的if语句，以便输出错误消息。下面是使用 这个成数的例子。
function divide(numl, nurr.2) {
assert (typeof numl == "number" && typeof mm2 ■■ wnumber",
"divide(): Both arguments must be numbers.N);
return numl / num2;
}
AsseriExampleOL htm
可见，使用assert ()函数可以减少抛出错误所脔的代码量，而且也比前面的代码更容易看慷。
17.4常见的旧错误
多年以来，IE —直都是最难于调试JavaScript错误的浏览器。丨E给出的错误消息一般很短又语焉不 详，而且h K文信息也很少，有时甚至一点都没存。但作为用户最多的浏览器，如何看懂丨E给出的错 误也是最受关注的。下面几小节将分別探讨一些在IE中难于调试的JavaScript错误。
17.4.1操作终止
在IE8之前的版本中，存在一个相对丁-其他浏览器而言，最令人迷惑、讨厌，也最难丁调试的错误: 操作终止（operation aborted)。在修改尚未加载完成的页面时，就会发生操作终止错误。发生错误时， 会出现一个模态对话框，告诉你“操作终止。”单击确定（0K)按钮，则卸载整个页面，继而显示一张 空A屏幕;此时要进行调试非常困难。下面的示例将会导致操作终止错误。
<!OOCTYPE html>
<htmi>
<head>
<title>Operation Aborted Example</title>
</head>
<body>
<p>The following code should cause an Operation Aborted error in IE versions prior to 8.</p>

17.4 常见的IE错误 517
<div>
<script type="text/javascripC>
document.body.appcn<a〇hil<3(c3ocuiner.c.createEle.Ttent {"div")); </script>
</div>
</body>
</html>
OperationAbortedExampleOI.htm
这个例子中存在的问题是：JavaScript代码在页面尚未加载完毕时就要修改document.body,而且 <script>元素还不是<body>元素的直接子元素。准确一点说，当<SCripc>节点被包含在某个元素中， 而且JavaScript代码又要使用appendChild()、innerHTMli或其他DOM方法修改该兀素的父兀素或 祖先元素时，将会发生操作终止错误（W为只能修改已经加载完毕的元素)。
要避免这个问题，可以等到H标元素加载完毕后再对它进行操作，或者使用其他操作方法。例如， 为document.body添加一个绝对定位在页面上的覆盖M,就足一种非常常见的操作。通常，开发人员 都是使用appendChildO方法来添加这个元素的，但换成使用insertBeforeU方法也很容场。因此， 只要修改前面例子中的一行代码，就可以避免操作终止错误。
<!DOCTYPE html>
<html>
<head>
<title>Operation Aborted Example</title>
</head>
<body>
<p>The following code should not cause an Operation Aborted error in IE versions prior to 8.</p>
<div>
<script type="text/j avascriptM >
document.body•insertBefore(document.createElement(HdivN),
docunent.body.firetChild) t
</script>
</div>
</body>
</html>
OperationAbortedExampie02.htm
在这个例子中，新的<div>兀素被添加到document .body的开头部分而不是未尾。因为完成这一 操作所需的所有信息在脚本运行时都是已知的，所以这不会引发错误。
除了改变方法之外，还可以把Script元索从包含元素中移出来，直接作为的子元素。例如:
<!DOCTYPE html>
<html>
<head>
<title>Operation Aborted Example</title>
</head>
<body>
<p>The following code should not cause an Operation Aborted error in IE versions prior Co 8.</p>
<div>
</<3iv>
<scrij>t type:”text/javaecript”>
document.body.appendChild(document.createElezaent(Mdivn));
</0cript>

518 第17章错误处理与调试
</body>
OperafionAbortedExampie03.htm
这一次也不会发生错误，因为脚本修改的是它的直接父元岽，而不再是间接的祖先元素。
在同样的情况下，IE8不再抛出操作终止错误，而是抛出常规的JavaScript错误，带有如下错误消息:
HTML Parsing Error: Unable to modify the parent container element before the child
element is closed (KB9279X7).
不过，虽然浏览器抛出的错误不同，怛解决方案仍然是-样的。
17.4.2无效字符
根据语法，JavaScript文件必须只包含特定的字符。在JavaScript文件中存在无效字符时，ffi会抛出 无效字符（invalid character)错误。所谓无效字符，就是JavaScript语法中未定义的字符。例如，有一 个很像减号佴却由Unicode值821丨表示的字符（\u2013 ),就不能用作常规的减号（ASCII编码为45), 因为JavaScript语法中没有定义该字符。这个字符通常是在Word文挡中自动插人的。如果你的代码是 从Word文档中复制到文本编辑器中，然后又在IE中运行的，那么就n丨能会遇到无效字符错误。其他浏 览器对无效字符做出的反应.*5 IE类似，Firefox会抛出非法字符（illegal character)错误，Safari会报告 发生了语法错误，而Opera则会报告发生了 ReferenceEnroi•(引用错误)，因为它会将无效字符解释 为未定义的标识符。
17.4.3未找到成员
如前所述，IE中的所有DOM对象都是以COM对象，而非原生JavaScript对象的形式实现的。这 会导致一些与垃圾收集相关的非常奇怪的行为。IE中的未找到成员（Member not found)错误，就是由 于垃圾收集例程配合错误所直接导致的。
具体来说，如果在对象被销毁之后，又给该对象赋值，就会导致未找到成M错误。而导致这个错误 的，一定是COM对象。发生这个错误的最常见情形是使用event对象的时候。IE中的event对象是 window的属性，该对象在事件发生时创建，在最;T;••个事件处理程序执行完毕后销毁。假设你在一个 闭包中使用了 event对象，而该闭包不会立即执行，那么在将来调用它并给event的属性賦值时，就 会导致未找到成员错误，如下面的例子所示。
document.onclick = function(){ var event = window.event; seCTimeout(function〇{
event .returnValue = false;	//未找 i1】成员•^误
}, 1000);
>;
在这段代码中，我们将一个单击車件处理程序指定给了文杓。在事件处理程序中，window.event 被保存在event变S中。然后，传人setTimeoutU中的闭包里义包含了 event变量。当单击事件处 理程序执行完毕后，event对象就会被销毁，因而闭包中引用对象的成员就成了不存在的了。换句话说, 由于不能在COM对象被销毁之后再给其成员賦值，在闭包中给returnValue陚值就会导致未找到成 员错误。

17.4常见的IE错误 519
17.4.4未知运行时错误
.当使用innerHTML或outerHTML以下列方式指定HTML时，就会发生未知运行时错误（Unknown runtime error): —是把块元素插人到行内元索时，二是访问表格任意部分（<table>、<比〇办>等> 的 任意属性时。例如，从技术角度说，<span>标签不能包含<diV>2类的块级元索，因此下面的代码就会 导致未知运行时错误：
span. innerHTML = *<div>Hi</div>*;	//这4, span 包含了 <div>元素
在遇到把块级元素插人到不恰当位a的情况时，其他浏览器会尝试纠正并隐藏错误，而ie在这一 点上反倒很较具儿。
17.4.5语法错误
通常，只要IE —报告发生了语法错误（syntax error),都可以很快找到错误的原因。这时候，原因 可能是代码中少了一个分号，或者花括号前后不对应。然而，还有一种原因不I-分明显的情况诺要格外 注意。
如果你引用了外部的JavaScript文件，而该文件最终并没有返回JavaScript代码，丨E也会抛出语法 错误。例如，<SCript:>元素的src特性指向广一个HTML文件，就会导致语法错误。报告语法错误的 位置时，通常都会说该错误位于脚本第一行的第一个字符处。Opera和Safari也会报告语法错误，但它 们会给出导致问题的外部文件的信息;丨E就不会给出这个信息，W此就®要我们Nd重复检査一遍引用 的外部JavaScript文件。但Firefox会忽略那些被4作JavaScript内容嵌人到文档中的非JavaScript文件中的 解析错误。
在服务器端组件动态生成JavaScript的情况下，比较容易出现这种错误。很多服务器端语言都会在 发生运行时错误时，向输出中插人HTML代码，而这种包含HTML的输出很容易就会违反JavaScript 语法。如果在追査语法错误时遇到了麻烦，我们建议你再仔细检査一遍引用的外部文件，确保这些文件 中没有包含服务器W错误而插人到其中的HTML。
17.4.6系统无法找到指定资源
系统无法找到指定资源（The system cannot locate the resource specified)这种说法，恐怕要第是IE 给出的最有价值的错误消息了。在使用JavaScript谙求某个资源URL,而该URL的长度超过了 IE对URL 最长不能超过2083个字符的限制时，就会发牛这个错误。IE不仅限制JavaScript中使用的URL的长度， 而且也限制用户在浏览器自身中使用的URL长度(其他浏览器对URL的限制没有这么严格UE对URL 路径还有一个不能超过2048个字符的限制。下面的代码将会导致错误。
function createLongUri(url){ var s = " ? ••;
for (var i=0, len=2500; i < len; i++){
return url + s;
var x = new XMLHttpRequest〇;

520 第17章错误处理与调试
x.open(•get”， createLongUrl(Mhctp：//

www.somedomain.con/"), true); x.send(null);
LongURLErrorExampleOI.htm
在这个例子中，XMLHttpRequest对象试图向一个超出最大长度限制的URL发送请求。在调用 open (>方法时，就会发生错误。避免这个M题的办法，无北就是通过给査询字符串参数起更短的名字, 或者减少不必要的数据，来缩短査询字符串的长度。另外，还可以把淸求方法改为POST,通过请求体 而不是査询字符串来发送数据。有关Ajax (或者说XMLKttpRequest对象）的详细内容，将在第21 章全面讨论。
###  17.5 小结
错误处理对于今天复杂的Web应用程序开发ift"3•至关重要。不能提前预测到可能发生的错误，不 能提前采取恢复策略，可能导致较差的if}户体验，M终引发用户不满。多数浏览器在默认情况下都不会 向用户报告错保，因此在开发和调试期间需要启W浏览器的错误报告功能。然而，在投人运行的产品代 码中，贝怀应该再有诸如此类的错误报告出现。
下面是几种避免浏览器响应JavaScript错误的方法。
- [ ] 在可能发生错误的地方使用try-catch语句，这样你还有机会以适汽的方式对错误给出响应， 而不必沿用浏览器处理错误的机制。
- [ ] 使用window.onerror事件处理程序，这种方式可以接受try-catch不能处理的所有错误（仅 限于 IE、Firefox 和 Chrome )。
另外，对任何Web应用程序都应该分析可能的错误来源，并制定处理错误的方案。
- [ ] 首先，必须要明确什么是致命错误，什么是非致命错误。
- [ ] 其次，再分析代码，以判断最可能发生的错误。JavaScript中发生错误的主要原因如下。
- [ ] 类塑转换
- [ ] 未充分检测数据类型
- [ ] 发送给服务器或从服务器接收到的数据冇错误
IE、Firefox、Chrome、Opera和Safari都有JavaScript调试器，有的是内置的，冇的是以需要下载的 扩展形式存在的。这些调试器都支持设置断点、控制代码执行及在运行时检测变最的值。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter16.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter18.md)