#  第24章 最佳实践([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 可调试性
- [ ] 当有地方出错时，代码可以给予你足够的信息来尽可能It接地确定问题所在。  

对于专业人士而言，能写出可维护的JavaScript代码是非常重要的技能。这正是周末改改网站的爱 好者和真正理解自己作品的开发人员之间的K別。
## 24.1 代码约定
一种让代码变得可维护的简单途径是形成一套JavaScript代码的书写约定。绝大多数语言都开发出 了各自的代码约定，只要在网上-•捜就能找到大相关文捫。专业的组织为开发人员制定了详尽的代码 约定试图让代码对任何人都对维护。杰出的开放源代码项tl有着严格的代码约定要求，这让社区中的任 何人都吋以轻松地理解代码是如何组织的。
由于JavaScript的可适应性，代码约定对它也很重要。由于和大多数面向对象语言不丨SI, JavaScript 并不强制开发人员将所有东西都定义为对象。语言可以支持各种编程风格，从传统面向对象式到声明式 到函数式。只要快速浏览一下一些开源JavaScript库，就能发现好几种创建对象、定义方法和管理环境 的途径。
以下小节将讨论代码约定的概论。对这些主题的解说非常重要，虽然可能的解说方式会有区别，这 取决于个人需求。
司•读性
要让代码可维护，首先它必须n丨读。可读性与代码作为文本文件的格式化方式有关。吋读性的大部 分内容都是和代码的缩进相关的。当所有人都使用一样的缩进方式时，整个项R中的代码都会更加易于 阅读。通常会使用若干空格而非制表符来进行缩进，这是因为制表符在不同的文本编辑器中显示效果不 同。一种不错的、很常见的缩进大小为4个空格，当然你也可以使用其他数量。
可读性的另一方面是注释。在大多数编程语言中，对每个方法的注释都视为一个可行的实践。因为 JavaScript可以在代码的任何地方创建函数，所以这点常常被忽略/。然而正因如此，在JavaScript中为 每个函数编写文档就更加重要了。一般而言，冇如下一些地方需要进行注释。  
- [ ] 函数和方法一每个函数或方法都应该包含一个注释，描述其目的和用于完成任务所可能使用 的算法。陈述事先的假设也非常東要，如参数代表什么，闲数是否有返回值（因为这不能从函 数定义中推断出来)。  
- [ ] 大段代码一用于完成单个任务的多行代码应该在前面放-个描述任务的注释。
口复杂的算法一如果使用了一种独特的方式解决某个问题，则要在注释中解释你是如何做的。 这不仅仅可以帮助其他浏览你代码的人，也能在下次你自己査阅代码的时候帮助理解。  
- [ ]  Hack~因为存在浏览器差异，JavaScript代码一般会包含残hack。不要假设Jt-他人在看代 码的时候能够理解hack所要应付的浏览器问题。如果因为某种浏览器无法使用普通的方法， 所以你需要用一些不同的方法，那么请将这些信息放在注释中。这样可以减少出现这种情况的 可能件：有人偶然宥到你的hack,然后“修正” 了它，最后遂新引人了你本来修iE了的错误。
缩进和注释可以带来更可读的代码，在未来则更容易维护。
变置和函数命名
适15给变ft和函数起名字对丁•坊加代码可理解性和可维护性是在常甩要的。由于很多JavaScript开 发人员最初都只是业余爱好者，所以有一种使)U尤意义名字的倾向，诸如给变量起”foo-、"bar•等名 字，给函数起"doSometting•这样的名字。专业JavaScript开发人员必须克服这鸣恶习以创建可维护的 代码。命名的一般规则如K所示。  
- [ ] 变M名应为名词如car或person。
- [ ] 涵数名应该以动词开始，如getNameO。返问布尔类制值的函数一般以is■幵头，如 isEnable()0
- [ ] 变M和函数都应使用合乎逻辑的名字，不要担心长度。长度问题可以通过后处理和压缩（本章 后面会讲到）来缓解。
必须避免出现无法表示所包含的数据类型的无用变世名。宥了合适的命名，代码阅读起来就像讲述 故亊-样，更容易理解。
变量类型透明
由于在JavaScript中变董是松散类铟的，很容易就忘记变量所应包含的数据类型。合适的命名方式 可以一定程度上缓解这个问题，但放到所冇的情况下看，还不够。有三种表示变fi数据类型的方式。
第一种方式是初始化。VI定义了一个变量后，它戍该被初始化为一个值，来暗示它将来应该如何应 用。例如，将来保存布尔类型值的变请应该初始化为true或者false，将来保存数字的变跫就应该初 始化为一个数字，如以下例子所示：
//通过初始化指定变*类S
```
var found = false;	//布尔里
var count = -1;	"数字
var name = "";	//字符串
var person = nul1.;	"对象
```
初始化为一个特定的数据类型可以很好的指明变ia的类型。但缺点是它无法用f函数声明中的函数 参数。
第二种方法是使州匈牙利标记法来指定变*类型。匈牙利标记法在变it名之前加上一个或多个字符 来表示数据类型。这个标记法在脚本语言中很流行，铃经很长时间也是JavaScript所推崇的方式。 JavaScript中最传统的甸牙利标记法是用单个字符表示基本类型："〇■代表对象，”s”代表字符串，-i" 代表整数，"f1■代表浮点数，*b"代表布尔型。如下所示：
//用于指定数描类型的与牙利标记法 var bFound;	//布尔2
var iCount;	"整数
var sKame;	"宇符串
var oPerson;	//对象
JavaScript中川匈牙利标记法的好处是函数参数一样可以使用。但它的喊点是让代码某种程度上难 以阅读，阻碍了没有用它时代码的直观性和句子式的特质。因此，匈牙利标记法失去了一杵开发者的 宠爱。
最后一种指定变fi类型的方式是使用类铟注释。类塑注释放在变M名右边，但是在初始化前面。这 种方式起在变S旁边放--段指定类剞的注释，如下所示：
//用于指定类3!的类曳注释
var found	/*:Boolean*/	=	false;
var count	/*:int*/	=	10;
var name	/*:String*/	=	"Nicholas";
var person	/*：Object*/	二	null;
类型注释维持了代码的整体w读性，同时注人r类迆信息。类型注释的缺点足你不能用多行注释一 次注释大块的代码，因为类型.注释也是多行注释，M者会冲突，如下例所泣所示：

24.1 可维护性 659
//以下代码不能正确运行 "
var found	/* ：Boolean*/	=	false;
var count.	/* ： int */	=	1C;
var name	/* ：String*/	=	"Nicholas";
var person	/*：〇bject*/ = null;
*/
这里，试图通过多行注释注释所有变量。类型注释与其相冲突，w为第一次出现的/*(第二行） 匹配r第一次出现的*/(第3行），这会造成一个语法错误。如果你想注释掉这些使用类型注释的代码 行，最好在每一行上使用单行注释（很多编辑器可以帮你完成)。
这就是最常见的三种指定变M数据类型的方法。每种都有各A的优势和劣势，要自己在使用之前进 行评估。最重要的娃要确定哪种《适合你的项目并一致使用。
24.1.3松散耦合
只要应用的某个部分过分依赖于另一部分，代码就足-耦合过紧，难于维护。典型的问题如：对象直 接引用另一个对象，并且当修改艽中-个的同时耑要修改另外-个。紧密耦合的软件难于维护并且需要 经常重写。
因为Web应用所涉及的技术，有多种情况会使它变得耦合过紧。必须小心这些情况，并尽可能维 护弱耦合的代码。
1.解耦 HTML/JavaScript
一种最常见的耦合类劭是HTML/JavaScript耦合。在Web上，HTML和JavaScript各自代表了解决 方案中的不间层次：HTML足数据，JavaScript是行为。因为它们天牛就需要交瓦，所以有多种不同的 方法将这两个技术关联起來。但是，凊一些方法会将HTML和JavaScript过于紧密地耦合在一起。
直接写在HTML中的JavaScript,使用包含内联代码的script:JE素或者是使用IITML属性来分 配亊件处理程序，都是过于紧密的耦合。请石以K代码。
!—使用 r script 的紧密麵合的 HTML/JavaScript:--
script type="text/javascript" document.write("Hello world!");
/script
!--使用事件处理槎序属性值的紧密耦合的HTML/JavaScript --
input type="buttonH value="Click Me" onclick="doSomething{) '• /
虽然这些从技术h来说都是正确的，但是实践中，它们将表示数据的HTML和定义行为的JavaScript 紧密耦合在了一起。理想情况是，HTML和JavaScript应该完全分离，并通过外部文件和使用DOM附 加行为来包含JavaScript。
.当HTML和JavaScript过丁-紧密的耦合在一起时，出现JavaScript错误时就要先判断错误是出现在 HTML部分还是在JavaScript文件中。它还会引人和代码是杏可用的相关新问题。在这个例子中，可能 在doSomethingO函数可用之前，就已经按下了按钮，引发了一个JavaScript错误。因为任何对按钮 行为的更改要N时触及HTML和JavaScript, W此影响了可维护性。而这些更改本该只在JavaScript巾 进行。
HTML和JavaScript的紧密耦合也可以在相反的关系上成立：JavaScript包含了 HTML。这通常会出 现在使用innerHTML来插人一段HTML文本到贞面上这种情况中，如下面的例子所示：

660	第24章最佳实践
//将HTML紧密輛合到JavaScript function insertMessage(rr.sg) {
vac container - document .getiElenentByld ("container");
container. innerHTML = ndiv class-\"msg\"p class=\"p〇st\",* + msg +• "/p" + ■pxeir;Latest message above./em/px/div";
}
一般来说，你应该避免在JavaScript中创建大a HTML。再一次重申要保持M次的分离，这样可以 很容易的确定错误来源。当使用hW这个例子的时候,有-个页面布M的问题，可能和动态创建的HTML 没有被]H确格式化冇关。不过，要定位这个错识可能非常闲难，因为你讨能一般先狞页面的源代码来査 找那段烦人的HTML,但是却没能找到，W为它足动态生成的。对数据或者布局的吏改也会要求更改 JavaScript,这也表明了这两个反次过丁-紧密地耦合了。
HTML S现应该尽可能~ JavaScript保持分离。当JavaScript用于插人数据时，尽tt不要直接插人 标记。•般可以在页面中直接包含并隐藏标UI,然后等到整个页面渲染好之后，就可以用JavaScript M 示该标记，而非生成它。另一种方法是进行Ajax请求并获取更多要M示的HTML,这个方法可以让同 样的逭染层（PHP、JSP、Ruby等等）来输出标记，而不是A接嵌在JavaScript中。
将HTML和JavaScript解耦可以在调试过程中节咨时间，更加容易确定错误的来源，也减轻维护的 难度：更改行为只需要在JavaScript文件中进行，而更改标E则只要在渲染文件中。
2.解賴 CSS/JavaScript
另个Web层则是CSS，它主要负责贞面的Wvl^JavaScript和CSS也是非常紧密相关的：他们都 是HTML之上的足次，因此常常一起使用。佰是，和HTML与JavaScript的情况一样，CSS和JavaScript 也可能会过T紧密地賴合在一起。最常见的紧密稱合的例子是使用JavaScript来更改某些样式，如下所示：
"CSS对JavaScript的紧窀輛合
element .style.color - ■*red";
element.style.backgroundColor = "blue";
由丁 CSS负责页面的显示，当M示出现任何间题时都应该只是査# css文件来解决。然而，当使 用了 JavaScript来更改某些样式的时候，比如颜色，就出现丫第二个吋能已更改和必须检査的地方。结 果是JavaScript也在某种程度上负责了页面的M示，并与CSS紧密耦合了。如果未来需要更改样式表， CSS和JavaScript文件可能都需要修改。这就给开发人员造成了维护上的噩梦。所以在这两个层次之间 必须有淸晰的划分。
现代Web应HJ常常要使用JavaScript来吏改样式，所以虽然不n丨能完全将CSS和JavaScript解耦， 何是还是能比辆合更松散的。这是通过动态史改样式类而非特定样式来实现的，如下例所示：
//CSS对JavaScript的松散相合 element. className ° edit";
通过K修改某个元索的CSS类，就可以让大部分样式倍息严格保留在CSS中。JavaScript可以更改 样式类，但并不会笠接影响到元素的样式。只要应用了正确的类，那么任何M示问题都可以直接追溯到 CSS rMIK JavaScript〇
第二类紧密稱合仅会在IE中出现（似运行于标准模式下的〖E8不会出现），它可以在CSS中通过表 达式嵌入JavaScript,如下例所示：
/* JavaScript 对 CSS 的紧$耦合 */ div {
width： expression(document.J^〇dy.off3etWidth - 10 + ”px_;

24.1 可维护性 661
通常要避免使用表达式，因为它们不能跨浏览器兼容，还因为它们所引人的JavaScript和CSS之间 的紧密糊合。如果使用J*表达式，那么可能会在CSS中出现JavaScript错误。由于CSS表达式而追踪过 JavaScript错误的开发人员，会告诉你在他们决定肴一下CSS之前花了多长时间来査找错误。
W次提醒，好的层次划分是非常®要的。显示问题的唯一来源应该是CSS,行为问题的唯一来源应 该足JavaScript。在这些M次之间保持松散耦合吋以让你的整个炖用更加易于维护。
3.解锅应用逻辑/事件处理程序
每个Web应用一般都办相当多的亊件处理程序，监听着无数不同的事件。然而，很少有能仔细得 将应用逻辑从事件处理程序中分离的。请看以下例子：
function handleKeyPress{event)(
event = EventUCil.getEvent(event); if (event.keyCode == 13){
var target - EventUtil.gctTargot(event); var value - b * parselnt (target;.value); if (value  10){
document.getEXementById{"error-msg*).style.display = "block";

}

这个事件处理程序除了包含/应用逻辑，还进行了事件的处理。这种方式的问题有其双重性。首先， 除了通过亊件之外就再没有方法执行应用逻辑，这让调试变得困难。如果没有发生预想的结果怎么办？ 是不是表示亊件处理程序没冇被调用还是指应用逻辑失败？其次，如果一个后续的事件引发同样的应用 逻辑，那就必须复制功能代码或者将代码抽取到-个单独的函数中。无论何种方式，都要作比实际所需 更多的改动。
较好的方法是将应用逻辑和事件处理程序相分离，这样两者分別处理各G的东西。一个亊件处理程 序应该从亊件对象中提取相关信息，并将这拌信息传送到处理应用逻辑的某个方法中。例如，前面的代 码可以被重写为：
function validateValue(value){ value = 5 * parselnt(value); if (value  10){
document.gecElerr.entById("error-nsg*) .style.display = "block";
function handleKeyPress(event){
event s EventUtil.geCEvent(event); if (event.keyCode =- 13){
var target = EventUtil.getTarget(event); validateValue{target.value);
}
}
改动过的代码合评将应用逻辑从亊件处理程序中分离了出来。handleKeyPress ()函数确认足按 Ff Entei•键（event. keyCode 为丨3 ),取得了#件的口标并将 value 属性传递给 validateValue () 函数，这个函数包含了应用逻辑。注怠validateValue ()中没冇任何东西会依赖丁•任何事件处理程序 逻辑，它只足接收•个值，并根据该值进行其他处理。
从亊件处理程序中分离成用逻辑有几个好处。酋先，可以让你更容易吏改触发特定过程的事件。如 果最开始由鼠标点击事件触发过程，但现在按键也要进行同样处理，这种更改就很容易。其次，可以在

662 第24章最佳实践
不附加到事件的情况下测试代码，使其更易创建单元测试或者是A动化应用流程。
以下是要牢记的应用和业务逻辑之间松散耦合的几条原则：
- [ ] 勿将event对象传给其他方法;只传来A event对象中所需的数据;
- [ ] 任何可以在应用层面的动作都应该可以在不执行任何事件处理程序的情况下进行;
口任何事件处理程序都应该处理事件，然后将处理转交给应用逻辑。
牢记这几条可以在任何代码中都获得极大的可维护性的改进，并且为进一步的测试和开发制造了很 多可能。
24.1.4编程实践
书写可维护的JavaScript并不仅仅是关T如何格式化代码;它还关系到代码做什么的问题。在企业 环境中创建的Web应用往往同时由大量人员一M创作。这种情况F的目标是确保每个人所使用的浏览 器环境都有一致和不变的规则。因此，最好坚持以下一些编程实践。
1.尊重对象所有权
JavaScript的动态性质使得几乎任何东西在任何时间都可以修改。有人说在JavaScript没有什么神圣 的东西，因为尤法将某些东西标记为最终或恒定状态。这种状况在ECMAScript 5中通过引人防篡改对 象（第22章讨论过）得以改变;不过，默认情况下所有对象都是可以修改的。在其他语言中，当没有 实际的源代码的时候，对象和类是不可变的。JavaScript可以在任何时候修改任意对象，这样就可以以 不可预计的方式埴写畎认的行为。W为这门语言没有强行的限制，所以对于开发者来说，这是很重要的， 也是必要的。
也许在企业环境中最重要的编程实践就是炸取对象所有权，它的意思是你不能修改不属于你的对 象。简单地说，如果你不负责创逑或维护某个对象、它的对象或者它的方法，那么你就不能对它们进行 修改。更具体地说：
- [ ] 不要为实例或原型添加属性;
- [ ] 不要为实例或原型添加方法;
- [ ] 不要重定义已存在的方法。
问题在于开发人员会假设浏览器环境按照菜个特定方式运行，而对于多个人都用到的对象进行改动 就会产生错误。如果某人期望叫做stopEventO的函数能取消某个事件的畎认行为，但是你对其进行 了更改，然后它完成了本来的任务，后来还追加丫另外的事件处理程疼，那肯定会出现问题了。其他开 发人员会认为函数还是按照原来的方式执行，所以他们的用法会出错并有可能造成危害，因为他们并不 知道有副作用。
这些规则不仅仅适用于&定义类型和对象，对于诸如Object、String、document、window等 原生类型和对象也适用。此处潜在的问超可能吏加危险，W为浏览器提供者可能会在不做宣布或者是不 可预期的情况下更改这些对象。
著名的Prototype JavaScript库就出现过这种例子：它为document:对象实现了 getElements- ByClassName。方法，返回一个Array的实例并增加了一个each()方法。JohnResig在他的博客上叙 述 了产生这个问题的-•系列亊件。他在帖子（

http.V/ejohn.oi^g/blog^getelementsbydassname-pre-prototypc-ie/) 中说，他发现当浏览器开始内部实现getiElementsByClassNameO的时候就出现问题了，这个方法并 不返0—个Array而是返回一个并不包含each(方法的NodeList。使用Prototype库的开发人M习惯 于写这样的代码：

24.1可维护性	663
doctunent .gecElementsByClassNane {"selecced") . each (Element .hide);
M然在没有原生实现getElementsByClassName (的浏览器中可以正常运行，但对于支持的了浏 览器就会产牛错误，因为返M的值不同。你不能预测浏览器提供者在未来会怎样更改原生对象，所以不 管用任何力•式修改他们，都可能会导致将来你的实现和他们的实现之间的冲突。
所以，最佳的方法便是永远不修改不是由你所有的对象。所谓拥有对象，就是说这个对象是你创建 的，比如你■创建的自定义类细或对象字面tt。而Array、document•这些S然不是你的，它们在你 的代码执行前就存在了。你依然可以通过以下方式为对象创建新的功能：
- [ ] 创建包含所需功能的新对象，并用它与相关对象进行交乜;
- [ ] 创建n定义类型，继承耑要进行修改的类锄。然后可以为H定义炎迺添加额外功能。
现在很多JavaScript库都赞同并遵守这条开发原理，这样即使浏览器频繁更改，库本身也能继续成 长和适应。
2.避免全局量
与芎重对象所冇权密切相关的是尽可能避免全W变tt和函数。这也关系到创建一个脚本执行的一致 的和可维护的环境。最多创建一个全局变ft,让其他对象和闲数存在其中。请看以下例子：
//两个全而f	避免！！
var name = "Nicholas"; function sayName(){ alert (najne);
}
这段代码包合了两个全局量：变贷name和函数sayNameU。其实可以创建一个包含两者的对象， 如下例所示：
/ /一个全局量	推其
var MyApplication = { nanie: "Nicholas", sayNane: function(){ alert(this.name);
}
};
这段重写的代码引人一个单一的全局对象MyApplication，name和sayName()都附加到其_h。 这样做消除了一鸣存在于前一段代码中的一些问题。首先，变i name搜盖了 window.name属性，可 能会与其他功能产生冲突;其次,它有助消除功能作用域之间的混淆。调用MyApplication. sayName () 在逻辑上暗杀丫代码的任何问题都可以通过检査定义MyApplication的代码来确定。
甲-一的全Mfl的延伸便是命名空间的概念，由YUI (Yahoo! User Interface)库普及。命名空间包括 创建一个用丁放置功能的对象。在YUI的2.x版本中，有若干用于追加功能的命名空间。比如：
YAHOO.util .Dom	处理 DOM 的方法;
YAHOO.util.Event	与事件交互的方法;
YAHOO, iang-——用于底M语H■特性的方法。
对于YUI,单一的全局对象YAHOO作为一个容器，其中定义了其他对象。用这种方式将功能组合 在一起的对象，叫做命名空间。整个YUI库便是构建在这个概念上的，让它能够在M—个页面上与其他 的JavaScript库共存。
命名空间很重要的一部分是确定每个人都同意使用的全局对象的名字，并且尽可能唯一，让其他人

664 第24章最佳实践
不太可能也使用这个名字。在大多数悄况下，可以是开发代码的公司的名字，例如YAHOO或者Wrox。 你uj■以如K例所示开始创命名空间来组合功能„
//创建全局对象 var Wrox =( ;
//为 Professional JavaScript 创達命名空间 Wrox.?roJS = {);
//将书中用到的对象附加上去
Wrox.?roJS.KventUtiX = ( ... };
Wrox.ProJS.CookieUt il ® { ... };
在这个例子中,Wr〇X是全局tt，其他命名空间在此之上创建。如果本书所有代码都放在Wrox. pro JS 命名空间，那么其他作者也应把fid的代码添加到Wrox对象中。只耍所有人都遵循这个规则，那么就 不用拘心其他人也创建叫做EventUtil或者Cookieutil的对象，因为它会存在于不N的命名空间中。 请肴以下例了•：
//为Professional Ajax创建命名空间 Wrox.ProAjax = {};
//W加该书中所使用的其他对象
Wrox.ProAj ax.EventUt i1 - { ... };
Wrox.ProAjax.CookieUtil = {...);
//ProJS还可以继续分别访问
Wrox.ProJS.HventUtil.addHandler{.
"以及 ProAjax
Wrox.ProAjax.HventUtil.addHandler(...);
M然命名空间会谣要多写一些代码，何是对于4维护的的而旮是值得的。命名空间冇助于确保代 码町以在个页面h与其他代码以无害的方式一起丁_作。
避免与null进行比较
由于JavaScript不做任何自动的类规检夜，所存它就成了开发人M的责任。W此，在JavaScript代码 中其实很少进行类型检测。最常见的类塑检测就是丧宥某个值是否为null。但是，直接将值与null 比较足使用过度的，并且常常由于不充分的类型检查导致错误。看以下例了
function sortArray(values){
if (values != null){	//避免！
values.sort(comparator);
该涵数的H的是根据给定的比较子对-个数组进行排序。为了函数能iH确执行，values参数必需 是数组，怛这里的if语句仅仅检丧该values是否为mill。还有其他的值可以通过if语句，包括字 符串、数宁，它们会导致函数抛出错误。
现实中，与null比较很少适合情况而被使川。必须按照所期筚的对值进行检丧，而非按照不被期 望的那些。例如，在前面的范例中，values参数应该足--个数组，那么就要检査它是不是一个数组， 而不足检查它是否非mill。函数按照下面的力■式修改会更加合适：

24.1 可维护性 665
function sortArray(values){
if (values instanceof Array) {	//推荐
values.sort(comparator);
}
}
该函数的这个版本可以阻止所有非法值，而且完全用不着null。
这种验证数组的技术在多枢架的网页中不一定正确工作，因为每个框架都有其自 己的全局对象，因此，也有自己的Array构造函数。如果你是从一个框架将数组传 送到另一个框架，那么就要另外检査是否存在sort 〇方法。
如果看到了与null比较的代码，尝试使用以下技术替换：
- [ ] 如果值应为一个引用类型，使用instanceof操作符检杳其构造闲数;
口如果值应为一个基本类型，使用typeof检査其类型;
口如果是希望对象包含某个特定的方法名，则使用typeof操作符确保指定名字的方法存在于对 象上。
代码中的null比较越少，就越容易确定代码的B的，并消除不必耍的错误。
使用常置
尽管JavaScript没冇常量的正式概念，但它还是很有用的。这种将数据从应用逻辑分离出来的思想， 可以在不3引人错误的风险的同时，就改变数据。请看以下例子：
function validate(value){ if (lvalue){
alert("Invalid value!");
location.href = "/errors/invalid.php";
在这个函数中冇两段数据：要显示给州户的信息以及URL。显示在用户界面上的字符串应该以允许 进行语言国际化的方式抽取出来。URL也应被抽取出来，因为它们存随着应用成长而改变的倾向。基本 上，有着可能由于这样那样原因会变化的这些数据，那么都会需要找到函数并在其中修改代码。而每次 修改应用逻辑的代码，都可能会引人错误。可以通过将数据抽取出来变成单独定义的常《的方式，将应 用逻辑与数据修改隔离开來。请看以K例子： var Constants = {
INVALID一VALtJE」HSG: "invalid value!", lwrvALlD_VALtJE_UHL： "/orrors/invalid.php"
};
function validate(value){ if (lvalue){
alert(Constants.INVALID_VALUB_MSG); location.Ixref = Constante.INVALID_VAL0B_0RL;
在这段重写过的代码中，消息和URL都被定义于constants对象中，然后函数引用这些值。这些 设置允许数据在无须接触使用它的函数的情况下进行变更。Constants对象甚至可以完全在单独的文

666 第24章最佳实践
件屮进行定义，同时该文件可以由包含正确值的其他过程根据国际化设置来生成。
关键在于将数据和使用它的逻辑进行分离。要注意的值的类型如下所示。
- [ ] 重复值一任何在多处用到的值都应抽取为一个常《。这就限制了当一个值变了而另一个没变 的时候会造成的错误。这也包含了 CSS类名。
- [ ] 用户界面字符串一任何用于显示给用户的字符串，都应被抽取出来以方便国际化。
- [ ]  URLs—在Web应用中，资源位置很容易变更，所以推荐用-个公共地方存放所有的URL。 口任意可能会更改的值^每当你在用到字面M值的时候，你都要问一下自己这个值在未来是不 是会变化。如果答案是“是”，那么这个值就应该被提取出来作为一个常量。
对于企业级的JavaScript开发而言，使用常盘是非常重要的技巧，因为它能让代码更容易维护，并 且在数据更改的同时保护代码。
24.2性能
自从JavaScript诞生以来，用这门语言编写网页的开发人员有了极大的增长。与此同时，JavaScript 代码的执行效率也越来越受到关注。因为JavaScript最初是~个解释塑语言，执行速度要比编译型语言 慢得多。Ckome是第一款内背优化引擎，将JavaScript编译成本地代码的浏览器。此后，主流浏览器纷 纷效仿，陆续实现了 JavaScript的编译执行。
即使到了编译执行JavaScrip丨的新阶段，仍然会存在低效率的代码。不过，还是有一些方式可以改 进代码的整体性能的。
24.2.1注意作用域
第4章讨论了 JavaScript中“作用域”的概念以及作用域链是如何运作的。随着作用域链中的作用 域数fi的增加，访问当前作用域以外的变量的时间也在增加。访问全局变量总是要比访问局部变量慢， 因为需要遍历作用域链。只要能减少花费在作用域链上的时间，就能增加脚本的整体性能。
1.避免全局查找
可能优化脚本性能最重要的就是注意全局査找。使用全局变童和函数肯定要比局部的开销更大，因 为要涉及作用域链上的査找。请看以下函数：
function updateUI(){
var imgs = document.getElementsByTagName{*img"); for {var i=0, len=imgs.length; i  len? i++){
imgs【il■title = document.title + • image ■ + i;
}
var msg = document: .getElementById( "msg"); msg.innerHTML = "Update complete.";

该函数可能看上去完全正常，但是它包含三个对于全局document对象的引用。如果在5T面上有 多个图片，那么for循环中的document引用就会被执行多次甚至上百次，每次都会要进行作用域链 丧找。通过创建一个指向document:对象的局部变M,就可以通过限制一次全局査找来改进这个函数的 性能：

24.2 性能 667
function updateUI(){
vftir doc s document;
var imgs = doc.9etElementBByTagNaine(nimgn); for var i-0t len=iings. length; i  len; i++) { lings[i] .title s doc.title ♦ " image " + i;
}
var msg » doc.getElementById("msgN);
msg.innerHTML = "Updace complete.";
}
这里,首先将document对象存在本地的doc变置中;然后在余下的代码中替换原来的document。 与原来的的版本相比，现在的函数只有一次全局查找，肯定更快。
将在一个函数中会用到多次的全局对象存储为局部变R总是没错的。
2.避免with语句
在性能非常S要的地方必须避免使用with语句。和阐数类似，with语句会创建自己的作用域， 因此会增加其中执行的代码的作用域链的长度。由于额外的作用域链査找，在with语句中执行的代码 肯定会比外面执行的代码要慢。
必须使用with语句的情况很少，因为它主要用于消除额外的宁符。在大多数情况下，可以用局部 变M完成相同的事情而不引人新的作用域。下面足一个例子：
function updateBody(){ with(document.body){ alort(tagName); irmerHTML = "Hello world!";

}
这段代码屮的With语句让document .body变得更容易使用。其实nr以使用局部变量达到相同的 效果，如F所示：
function updateBody(){
var body - document.body
alert{body.tagName);
body.innerHTMI. = "Hello world!";
J
虽然代码稍微长了点，但是阅读起来比with语句版本更好，它确保让你知道tagName和 innerHTML是W于哪个对象的。同时，这段代码通过将document .body存储在局部变量中省去了额 外的全M査找。
24.2.2选择正确方法
和其他语言一样，性能问题的一部分足和用于解决问题的算法或者方法有关的。老练的开发人员根 据经验可以得知哪种方法可能获得更好的性能。很多应用在其他编程语言中的技术和方法也可以在 JavaScript 中使用。
1.避免不必要的厲性查找
在计算机科学中，算法的复杂度是使川O符号来表示的。M简单、最快捷的算法是常数值即0(1)。 之后，算法变得越来越复杂并花更长时间执行。下面的表格列出f JavaScript中常见的算法类型。

668	第24章最佳实践
描 述
+铃有多少值，执行的时间都是佾定的。一般表示简单值和存储在变M中的值
总的执行时N和伉的数贷相关.侶楚要完成算法并不一定要获取每个值。例如：二分杏找
总执行时间和值的数虽直接相关。例如：遍历某个数组中的所冇元素
总执行时间和值的数M有关，每个值至少要获取》次。例如：插人排序
常数值，即0(1),指代字面值和存储在变a中的值。符〇(!)表示无论在多少个值，需要获取常 M值的时间都一样。获取常ft值足非常高效的过程。请看下面代码：
var value = 5?
var sum = 10 + value;
alert(sum);
该代码进行了四次常值査找：数字5,变兩value，数字10和变S sum。这段代码的整体复杂 度被认为是0(1)。
在JavaScript中访问数组元素也是一个0(1)操作，和简单的变量查找效麥一样。所以以下代码和前 面的例+效韦一样：
var values = [5, 10];
var sum =： values [0] + values [1];
alert(sum);
使用变和数组要比访问对象h的属性史冇效申•，后者是-个0(«)操作。对象上的仟何诚性査找都 要比访问变M或者数组花费更长时间，因为必须在原型链中对拥有该名称的M性进行一次搜索。简而言 之，属性査找越多，执行时间就越K。W宥以下内容：
var values = { first: 5, second: 10}; var sun = values.first + values.second; alerc(sum);
这段代码使用两次属性找来汁算sum的值。进行一两次属性査找并不会导致M著的性能问题，供 是进行成TT上千次则肯定会减慢执行速度。
注意获取单个值的多重M性査找。例如，请荇以下代码：
var query » window, location .href. substring (window, location, href. ir.dexOf (*?"));
在这段代码中，在 6 次厲性杏找：window.location.href.substxing()^f 3 次，window. JLocation.hrfef • indexOf () 乂冇3次d只要数一数代码中的点的数ft,就可以确定属性査找的次数了。 这段代码由于两次用到/window, location.href,同样的査找进行了两次，因此效率特别不好。
一旦多次用到对象属性，应该将K存储在局部变H中。第一次访问该值会是〇(«)，然而后续的访问 都会足0(1),就会节伢很多^例如，之前的代码可以如下重％:
var url = window.location.href;
var query = url.substring(url.indexOf("?"));
这个版本的代码只有4次属性杏找，相对于原始版本节省f 33%。在更大的脚本中进行这种优化， 倾向丁•获得史多改进。

24.2 性能 669
一般来讲，只要能减少算法的复杂度，就要尽可能减少。尽可能多地使用W部变M将属性査找替换 为值丧找。进-步讲，如果即可以用数字化的数组位置进行访问，也可以使用命名属性（诸如NodeList 对象)，那么使用数字位置。
2.优化循环
循环是编程中最常见的结构，在JavaScript程序中H样随处可见。优化循环是性能优化过程中很重 要的一个部分，由于它们会反复运行N_段代码，从而自动地增加执行时间。在其他语言中对于循环优 化有大S研究，这些技术也可以应用于JavaScript。一个循环的基本优化步骤如下所示。
(1)减值迭代一大多数循环使用一个从0开始、增加到某个特定值的迭代器。在很多情况下，从 烺大值开始，在循环中不断减值的迭代器更加高效。
(2简化终止条件——由于每次循环过程都会计算终止条件，所以必须保证它尽可能快。也就是说 避免属性查找或其他〇(/〇的操作。
(3简化循环体一循环体是执行最多的，所以要确保其被最大限度地优化。确保没有某些可以被 很容易移出循环的密集计算。
(4)使用后测试循环	最常用for循环和while循环都是前测试循环。而如do-whi le这种后测
试循环，可以避免最初终止条件的计算，因此运行更快。
用一个例子来描述这种改动。以下是一个基本的for循环：
for (var i=0; i  values.length; i++){ process(valuesli));
)
这段代码中变量i从0递增到values数组中的元素总数。假设值的处理顺序无关紧要，那么循环 可以改为i减值，如下所示：
for (var i=values.length -1; i ■ 0; i--){
process(values[i]);
)
这里，变量i每次循环之后都会减1。在这个过程中，将终止条件从value, length的O〇〇调用 简化成了 0的0(1)调用。由于循环体只有一个语句，无法进一步优化。不过循环还能改成后测试循环, 如下：
var isvaluea.length *1; if (i  -1H do {
process(values[i1); while(—i = 0);
此处主©:的优化是将终止条件和n减操作符组合成/单个语句。这时，任何进一步的优化只能在 process ()函数中进行了，因为循环部分已经优化完全了。
记住使州“后测试”循环时必须确保要处理的值至少有一个。空数组会导致多余的一次循环而“前 测试"循环则可以避免。
展开循环
当循环的次数是确定的，消除循环并使用多次函数调用往往更快。请看一下前面的例子。如果数组 的长度总是一样的，对每个元素都调用process ()可能更优，如以下代码所示：
24

670 第24章最佳实践
//消除搞环
process(values(0】}; process(values[1]); process(values 12]);
这个例子假设values数组里面只有3个元素，直接对每个元素调用process (。这样展开循环 可以消除建立循环和处理终止条件的额外开销，使代码运行得更快。
如果循环中的迭代次数不能事先确定，那可以考虑使用一种叫做Duff装饩的技术。这个技术是以 其创违者Tom Duff命名的，他最早在CilfW中使用这项技术。正是Jeff Greenberg用】avaScript实现了 Duff装置。Duff装S的基本概念是通过计算迭代的次数是否为8的倍数将一个循环展开为一系列语句。 请狞以下代码：
//credit： Jeff Greenberg for JS implementation of Duff's Device //假设 values.length  0
var iterations = Xath.ceil(values.length / 8); var szarcAt = values.length % 8; var i = 0;
do {
switch(startAt){
case 0: process {values	};
case 1： process(values); case 6: process(values[i+-])? case b： process(values[i++}); case 4： process{values[i++]); case 3： process{vaiues[i++J); case 2： process (values	);
case 1: process(values[i++】;
}
startAt - 0;
} while {--iterations  0);
Duff装黃;的实现是通过将values数组中元素个数除以8来计算出循环需要进行多少次迭代的。然 后使用取整的上限函数确保结果足整数。如果完全根据除8来进行迭代，町能会有一些不能被处理到的 元索，这个数M保存在startAt变M中。首次执行该循环吋，会检査StartAt变M宥冇需要多少额外 调用。例如，如果数组屮有〗0个值，startAt则等于2,那么敢开始的时候process(则只会被谰用 2次。在接下来的循环中，startAt被：fi*为0,这样之后的锊次循环都会调用8次processO。展开 循环吋以提升大数据集的处理速度。
由 Andrew B. King 所著的	吵 y〇«r 57/e (New Riders，2〇03 )提出丫个更快的 Duff 装.置技术，
将do-while循环分成2个单独的循环。以卜姑例子：
//credit: Speed Op Your site (New Riders. 2003) var iterations ：= Math, floor (values • length / 8) ? var leftover = values.length % 8; var i - 0;
if (leftover  0){ do {
}



process(values!i++]); while (--leftover  0);
process(values【it+l;

24.2 性能 671
process(values Ii++]); process{values(i++)); process(values[i + + ]); process{values Ii++J}; process(values[i++]); process(valuesfi++]); process(values【i+本】）;
} whi]c (--iterations  0);
在这个实现中，軔余的计笄部分不会在实际循环中处理，而是在一个初始化循环中进行除以8的操 作。当处理掉了额外的元素，继续执行每次调用8次process ()的主循环。这个方法几乎比原始的Duff
装置实现快上40%。
针对大数据集使用展开循环可以节省很多时间，但对于小数据集，额外的开销则可能得不偿失。它 是要花更多的代码来完成同样的任务，如果处理的不是大数据集，一般来说并不值得。
避免双重解释
当JavaScript代码想解析JavaScript的时候就会存在双*解释惩罚。当使用evaiu函数或者是 Function构造函数以及使用setTimeout ()传一个字符串参数时都会发生这种情况。下面有一些例子：
//茱些代码求值——避免I!
eval("alert('Hello worldl')");
//创建舞*敖——避免n
var sayHi = new Function(*alert(*Hello world!')');
//设置超时——避免！！
setTimcout("alert('Hello world!')", 500);
在以上这些例+中，都要解析包含了 JavaScript代码的字符串。这个操作是不能在初始的解析过程 中完成的，因为代码是包含在字符串中的，也就是说在JavaScript代码运行的同时必须新启动一个解 析器来解析新的代码。实例化-个新的解析器有不容忽视的开销，所以这种代码要比直接解析慢得多。
对于这几个例子都有另外的办法。只有极少的情况下eval ()是绝对必须的，所以尽可能避免使用。 在这个例子中，代码其实可以直接内嵌在原代码中。对于Function构造函数，完全可以直接写成一般 的函数，调用setTimemitO可以传人函数作为第-个参数。以下是一些例子：
//已修正
alert('Hello world!');
//创建新函数——已修正 var sayHi = function(){
alert('Hollo world!');
};
//设置一个超时一己修正 £5etTiincout (functiont) {
alert('Hello world!1);
500);
如果要提高代码性能，尽可能避免出现锻要按照JavaScript解释的字符串。
性能的其他注意事项
3评估脚本性能的时候，还有其他一些可以考虑的东丙。下面并非主要的问题，不过如果使用得与 也会有相当大的提升。
24

672 第24章最佳实践
- [ ] 原生方法较快——只要有可能，使用原牛方法而不是A己用JavaScript重写-个。原生方法是用 诸如C/C++之类的编译型语言写出来的，所以要比JavaScript的快很多很多。JavaScript中最容 易被忘记的就是可以在Math对象中找到的复杂的数学运算;这些方法要比任何用JavaScript写 的同样方法如正弦、余弦快的多。
- [ ]  Switch语句较快——如果有一系列复杂的if-else语句，可以转换成单个switch语句则可 以得到更快的代码。还可以通过将case语句按照最吋能的到最不可能的顺序进行组织，来进一 步优化switch语句〇
- [ ] 位运算符较快——当进行数学运算的时候，位运算操作要比任何布尔运算或者算数运算快。选 择性地用位运算替换算数运算nj以极大提升复杂if•算的性能。诸如取模，逻辑和逻辑或都可 以考虑用位运算来替换。
24.2.3最小化语句数
JavaScript代码中的语句数置也影响所执行的操作的速度。完成多个操作的单个语句要比完成单个 操作的多个语句快。所以，就要找出可以组合在一起的语句，以减少脚本整体的执行时间。这黾有几个 可以参考的模式。
1.多个变量声明
有个地方很多开发人员都容易创建很多语句，那就是多个变量的卢明。很容易看到代码中由多个 var语句来声明多个变M,如F所示：
//4个语句一很浪费
var count = 5;
var color =. "blue";
var values = [1,2,3];
var now = new Date〇;
在强类型语言中，不同的数据类型的变量必须在不同的语句屮声明。然而，在JavaScript中所有的 变:t都可以使用单个var语句来声明。前面的代码可以如下重写：
//—个语句
var count = 5,
color = "blue", values = [1,2,3], now = new Date();
此处，变量声明只用了一个var语句，之间由逗号隔开。在大多数情况F这种优化都^常容易做, 并且要比单个变量分别声明快很多。
2.插入迭代值
当使用迭代值（也就是在不同的位置进行W加或减少的值）的时候，尽可能合并语句。请看以下 代码：
var name = values Ii 】; i+ + ;
前面这2句语句各只有一个目的：第一个从values数组中获取值，然后存储在naTne中;第二个 给变量i增加1。这两句可以通过迭代值插人第一个语句组合成一个语句，如下所示：
var name = values[i++3;

24.2 性能 673
这一个语句可以完成和前面两个语句一样的事情。因为A增操作符是后缀操作符，i的值只有在语 句其他部分结束之后才会增加。一旦出现类似情况，都要尝试将迭代值插人到最后使用它的语句中去。 3•使用数组和对象字面置
本书中，你可能看过两种创建数组和对象的方法：使用构造函数或者是使用字面量。使用构造函数 总是要用到更多的语句来插人元素或者定义属性，而字面量可以将这些操作在一个语句中完成。请看以 下例子：
//用4个语句鉬建和初始化数组——浪» var values = new Array(); values[01 = 123; values[1] = 456; values【2】=789;
//用4个语句创建和初始化对象一浪费 var person = new Object(}; person.name = "Nicholas"? person.age = 29; person.sayName = function(){ alert(this.name);
};
这段代码中，只创建和初始化了一个数组和一个对象。各用了4个语句：-•个调用构造函数，其他 3个分配数据。其实可以很容易地转换成使用字面量的形式，如下所示：
//只用一条语句创建和初始化数纽
var values = [123, 456, 789];
//只用一条语句创建和初始化对象 var person = {
name ; "Nicholas*( age : 29,
sayName : function(){ alert(this.name)?
}
);
重写后的代码只包含两条语句，一条创建和初始化数组，另一条创建和初始化对象。之前用了八条 语句的东西现在只用r两条，减少了 75%的语句It。在包含成千上万行JavaScript的代码库中，这些优 化的价值更大。
只要有可能，尽量使用数组和对象的字面量表达方式来消除不必要的语句。
在IE6和更早版本中使用字面量有微小的性能惩罚。不过这些问题在IE7中已经 解决。
24.2.4优化D0M交互
在'JavaScript各个方面中，DOM毫无疑问是最慢的一部分。DOM操作与交互要消耗大量时间， W为它们往往笛要重新渲染整个页面或者某一部分。进一步说.看似细&的操作也可能要花很久来执 行，因为DOM要处理非常多的信息。理解如何优化与DOM的交互可以极大得提高脚本完成的速度。

674 第24章最佳实践
1.最小化现场更新
一曰_你需要访问的DOM部分是已经ft示的页面的一部分，那么你就是在进行一个现场更新。之所 以叫现场史新，始闪为需要立即（现场）对页面对用户的显示进行更新。每一个更改，不管是插人单个 字符，还是移除整个片段，都有一个性能惩罚，因为浏览器要重新计算无数尺寸以进行更新。现场更新 进行得越多，代码完成执行所花的时间就越长;完成•个操作所需的现场更新越少，代码就越快。请看 以下例子：
var list = document.getElementRyId("myList"), item,
i;
for (i=0; i  10; i++) {
item s document.crcateSlement{"li"}; list.appendChilciUtem};
item.appendChild(document.createTextNode{"Item " + i));

这段代码为列表添加了 10个项U。添加每个项H时，都有2个现场史新：一个添加ii元素，另 一个给它添加文本节点。这样添加10个项Q,这个操作总共要完成20个现场更新。
要修正这个性能瓶颈，佑要减少现场史新的数M。一般冇2种方法。第一种是将列表从页面上移除， 最后进行®新，最后再将列表插间到同样的位.置。这个方法不是非常理想，因为在每次页面更新的时候 它会不必要的闪烁。第二个方法是使用文裆碎片来构建D0M结构，接着将其添加到List元素中。这 个方式避免了现场更新和页面闪烁问题。请石•下曲'内容：
var list = document.getSlenentByld{"myList"),
fragment « doctuaent.createDocumentFragment(), item,
i;
for (i=0; i  10; i+十）{
item = document.createElement("li"); fragzQent.appendChild(item);
item.appendChild(document.createTextKode(•Iteni "十 i};
}
list.appendChildfragment);
在这个例+中只冇一次现场更新，它发生在所有项u都创违好之后。文档碎片用作一个临时的占位 符，放置新创述的项H。然后使用appendChild (将所街项H添加到列表用〇记住，当给appendChiId ( 传人文档碎片时，只有碎片中的子节点被添加到H标，碎片本身不会被添加的。
一曰.需要更新DOM,谙考虑使用文抖碎片来构建DOM结构，然后再将其添加到现存的文档中。
2.使用 irmerHTML
右两种在j)l'面上创建DOM节点的方法：使用诸如createElementO和appendChild()之类的 DOM方法，以及使JlUnnerHTML。对于小的D0M更改而言，两种方法效韦都裘不多。然而，对于大 的DOM更改，使用innerHTML要比使用标准DOM方法创建间样的DOM结构快得多。
4把innerHTML设置为某个值时，后台会创建一个HTML解析器，然后使用内部的DOM调用来 创建DOM结构，而非基于JavaScript的DOM调用。由于内部方法姑编译好的而非解释执行的，所以执 行快得多。前面的例子还可以用innerHTML改写如下：

24.2 性能 675
var list = document.getElementByld("myLisf) f
for (i=0; i  10; i++) {
htial +*	M + i + "/lin;
)
list.innerHTHL * btml;
这段代码构建了一个HTML字符串，然后将其指定到list .irmerHTML,便创建了需要的DOM结 构。虽然宇符串连接上总是存点性能损失，似这种方式还是要比进行多个DOM操作更快。
使用innerHTML的关键在于（和其他DOM操作一样）最小化调用它的次数。例如，下面的代码 在这个操作中用到innerHTML的次数太多了 ：
var list = document. getEle,TienCById ("rtyList"),
i;
for (i=0; i  10; i++-) {
list.innerHTML += "liItem » + i + "/llmt	//遇免HJ
)
这段代码的问题在于每次循环都要调用innerHTOL,这足极其低效的。调用innerHTML实际上就 足一次现场更新，所以也要如此对待。构建好一个字符串然后一次性调用innerHTML要比调用 innerHTML多次快得多。
使用事件代理
大多数Web应用在用户交互上大贵用到事件处理程序。贞面上的事件处理程序的数贵和页面响应 用户交互的速度之间有个负相关。为了减轻这种惩罚，最好使用亊件代理。
爭件代理，如第13章中所讨论的那样，用到了事件冒泡。任何可以胃泡的书件都不仅仅可以在亊 件H标上进行处理，3标的任何祖先节点h也能处理。使用这个知识，就可以将亊件处理程序附加到更 髙M的地方负责多个R标的事件处理。如果可能，在文档级别附加亊件处理程序，这样可以处理整个页 面的事件。
注意 HTMLCollection
HTMLCollection对象的陷阱已经在本书中讨论过了，因为它们对丁 Web应用的性能而宵是巨大 的损富。记住，任何时候要访问HTMLCollection,不管它是一个属性还是一个方法，都是在文档上进 行一个査询，这个査询开销很昂贵。最小化访问HTMLCollection的次数Pi以极大地改进脚本的性能。
也许优化HTMLCollectio：!访M最重要的地方就是循环了。前面提到过将长度计算移人for循环 的初始化部分。现在肴一下这个例了•：
var images = document .getElementsByTagName ("iing" , i, len;
for (i=0, len=iraages.length; .i  len; i + + ) {
//处《
)
这里的关键在于长度length存人了 len变敏,而不追每次都去访问HTMLCollection的length 屈性〇 H在循环中使用HTMLCollection的时候，下一步应该是获取要使川的项H的引川，如下所示， 以便避免在循环体内多次调用HTMLCollection。
24

676 第24章最佳实践
var images = document .getElementsByTagNa/ne{ "img"), image,
i, len;
for (i=0# len=images.length; i  len; i++){ image b images【ij;
//处理
}
这段代码添加了 image变U,保存了当前的图像。这之后，在循环内就没打理由再访问images的 HTMLCollection T 〇
编写JavaScript的时候，一定要知道何时返冋HTMLCollection对象，这样你就可以最小化对他们 的访问。发生以F情况时会返冋HTMLCollection对象：
- [ ] 进行对 getElementsByTagName( 的调用;
口获取了元素的childNodes属性;
- [ ] 获取/元素的attributes属性;
口访问了特殊的集合，如 document.forms、document.images 等。
要了解当使用HTMLCollection对象时，合理使用会极大提升代码执行速度。
24.3部署
也许所有JavaScript解决方案最重要的部分，便足最后部署到运营中的网站或茗是Web应用的过程。 在这之前可能你已经做了相当多的r作，为普通的使用进行架构并优化一个解决方案。现在是时候从开 发环境中走出来并进人Web阶段了，在此将会和真正的用户交々:。然而，在这之前还有一系列需要解 决的问题。
24.3.1构建过程
完备JavaScript代码可以用于部署的一件很重要的事情，就是给它开发某些类型的构建过程。软件 开发的典型模式是写代码-编译-测试，即首先书写好代码，将其编译通过，然后运行并确保其正常工作。 由于JavaScript并非一个编译型语?T,模式变成丫写代码-测试，这里你写的代码就是你要在浏览器中测 试的代码。这个方法的问题在于它不是最优的，你写的代码不应该原封不动地放人浏览器中，理由如下 所示。
- [ ] 知识产权问题——如果把带有完整注释的代码放到线上，那别人就更容易知道你的意图，对它 再利用，并且可能找到安全漏洞。
- [ ] 文件大小——书写代码要保证容易阅读，j能更好地维护，但是这对于性能是不利的。浏览器 并不能从额外的空字符或者是冗长的函数名和变量名中获得什么好处。
- [ ] 代码组织——组织代码要考虑到可维护性并不一定是传送给浏览器的最好方式。
基于这些原W,最好给JavaScript文件定义一个构建过程。
构建过程始于在源控制中定义用于存储文件的逻辑结构。最好避免使用一个文件存放所有的 JavaScript,遵循以下面向对象语言中的典型.模式：将每个对象或f丨定义类型分别放入其单独的文件中。 这样可以确保每个文件包含最少M的代码，使其在不引人错误的情况下更容易修改。另外，在使用像 CVS或Subversion这类并发源控制系统的时候，这样做也减少f在合并操作中产生冲突的风险。

24.3 部署 677
记住将代码分离成多个文件只是为f提髙吋维护性，并非为了部署。要进行部署的时候，需要将这 些源代码合并为一个或几个归并文件。推荐Web应用中尽可能使用最少的JavaScript文件，是因为HTTP 请求是Web中的主要性能瓶颈之一。记住通过scriPt#记引用JavaScript文件是一个阻塞操作，当 代码下载并运行的时候会停止其他所有的下载。因此，尽量从逻辑上将JavaScript代码分组成部署文件。
一旦组织好文件和H录结构，并确定哪些要出现在部署文件中，就可以创建构建系统了。Ant构建 丁具（http://ant.^)aChe.〇rg)是为了自动化Java构建过程而诞生的，不过因为其易用性和应用广泛，而 在Web应用开发人M中也颇流行，诸如Julien Lecomte的软件工程师，已经写了教程指导如何使用Ant 进行 JavaScript 和 CSS 的构建自动化（Lecomte 的文章在 

www.julienlecomte.net/blog/2007/09/16/ )。
Ant由于其简便的文件处理能力而非常适合JavaScript编译系统。例如，可以很方便地获得目录中 的所有文件的列表，然后将其合并为一个文件，如卜'所示：
project name=•JavaScript Project- defaults"js.concatenate"
!--输出的目录
property name="build.dir" value="./js" /
!--色含*文件的目录--
property name="src.dir* values"./dev/src" /
 !—合并，有JS文件的目标一
!-- Credit: Julien Lecomte, 

http://www.julienlecomte.net/blog/2007/09/16/ -- target naitie=*js.concatenate■
concat destfile="${build.dir}/output.js"
filelist dir=-${src.dir}/js" files="a.js, b.js*/
fileset dir="${src.dir}/js" includes="*.js" excludes:_a.js, b.js*/ /concat
/target
/project
SampleAntDir/build.xml
该build.xml文件定义了两个属性：输出最终文件的构建目录，以及JavaScript源文件所在的源目录。 目标js.concatenate使用了C〇ncat元素来指定®要进行合并的文件的列表以及结果文件所要输 出的位置。filelist元素用于指定a.js和b.js要S■先出现在合并的文件中，fileSet元素指定了 之后要添加到目录中的其他所有文件，a.js和b.js除外。结果文件最后输出到/js/outputjs。
如果安装了 Ant,就可以进人bUild.xml文件所在的B录，并运行以下命令：
ant
然后构建过程就开始了，最后生成合并了的文件。如果在文件中还有其他目标，可以使用以下代码
仅执行;js.concatenate 目标：
ant j s.concatenate
可以根据需求，修改构建过程以包含其他步骤。在开发周期中引入构建这一步能让你在部署之前对 JavaScript文件进行更多的处理。
24.3.2验证
尽管现在出现了一®可以理解并支持JavaScript的〗DE,大多数开发人员还是要在浏览器中运行代

678 第24章最佳实践
码以检査其语法。这种方法有•邱问题。首先，验证过程难以S动化或者在不同系统间直接移植。其次, 除了语法错误外，很多问题只有在执行代码的时候才会遇到，这给错误留下了空间;有些丁.具可以帮助 确定JavaScript代码中潜在的问题，其中最著名的就是Douglas Crockford的JSLint (www.jslint.com)。
JSLint可以丧找JavaScript代码中的语法错误以及常见的编码错误。它可以发掘的一些潜在问题 如下：
eva 1 (的使用;
- [ ] 未声明变fi的使用;
- [ ] 遗漏的分号;
口不恰当的换行;
- [ ] 错误的逗号使用;
- [ ] 语句周围遗漏的括号;
switch分支语句中遗漏的break;
- [ ] 重复声明的变tt;
口 with的使用;
- [ ] 错误使用的等号（替代了双等号或=等号）;
- [ ] 无法到达的代码。
为了方便访问，它有一个在线版本，不过它也可以使用基于Java的Rhino JavaScript引擎(

www.mozilla. oig/rhino/)运行于命令行模式下。要在命令行中运行JSLint，首先要下载Rhino,并从wwwjslint.com/下 载Rhino版本的JSLint。一 H安装完成，便可以使用下面的语法从命令行运行JSLint 了：
java -jar rhino-1.6R7.jar jslint.js [input files1 如这个例子：
java -jar rhino-1.6R7.jar jslint.js a.js b.js c.js
如果给定文件中有任何语法问题或者是潜在的错误，则会输出有关错误和警告的报告。如果没有问 题，代码会直接结束而不显示任何信息。
玎以使用Ant将JSLint作为构建过程的一部分运行，添加如下一个0标：
Carget name="js.verify"
apply executable="java* parallel^"false"
fileset dir="${build.dir}" includes="output.js*/
arg line-"-jar"/
arg paths"${rhino.jar}*/
arg path="${jslint.js" /
srcfile/
/apply
/Larget
SampleAntDir/buildxml
这个目标假设Rhino jar文件的位置已经由叫做rhino, jar的属性指定了，同时JSLint Rhino文件 的位置由叫做jslint. js的属性指定了。output, js文件被传递给JSLint进行校验，然后显示找到的 任何问题。
给开发周期添加代码验证这个环节有助于避免将来可能出现的-些错误。建议开发人员给构建过程 加人某种类型的代码验证作为确定潜在问题的一个方法，防患于未然。

24.3 部署 679
JavaScript代码校验工具的列表可以在附录D中找到。
24.3.3压缩
当谈及JavaScript文件压缩，其实在讨论两个东西：代码长度和配重（Wircweight)。代码长度指的 是浏览器所需解析的字节数，配甫指的娃实际从服务器传送到浏览器的字节数。在Web开发的早期， 这两个数字几乎是一样的，因为从服务器端到客户端原封不动地传递了源文件。而在今天的Web上， 这两者很少相等，实际h也不应相等。
1.文件压缩
W为JavaScript并非编译为字节码，而是按照源代码传送的，代码文件通常包含浏览器执行所不溶 要的额外的信息和格式。注释，额外的空[1,以及长长的变名和闲数名虽然提高了可读性，何却是传 送给浏览器时不必要的字节。不过，我们可以使用压缩工具减少文件的大小。
压缩器一般进行如下•些步骤：
- [ ] 删除额外的空『丨（包括换行）;
- [ ] 删除所存注释;
- [ ] 缩短变量名。
JavaScript有不少压缩丨:具可用（附录D中有一个完整列表)，其中最优秀的（有争议的）是YUI压 缩器，http://yuilibrary_com /projccts/yuicompressor。YUI 压缩器使用了 Rhino JavaScript 解析器将 JavaScript 代码令牌化。然后使用这个令牌流创建代码不包含空白和注释的优化版本。与一般的基于表达式的压缩 器不同的地方在于，YUI压缩可以确保不引人仟何语法错误，并可以安全地缩短局部变敏名。
YUI压缩器是作为Java的一个jar文件发布的，名字叫yuicompressor-x.y.z. jar,其中x.y .z 是版本号。在写本1?的时候，2.3.5是最新的版本。可以使用以下命令行格式来使用YUI压缩器： java -jar yuicompressor-x.y.z.jar [options] [input files]
YUI压缩器的选项列在了下面的表格内。



24

680 第24章最佳实践
例如，以下命令行可以用来将CookieUtil.js压缩成…个叫做cookie, js的文件：
java -jar yuicompressor-2.3.5.jar -o cookie.js CookieUtii.js
YUI压缩器也可以通过直接调用java可执行义件在Ant中使用，如K面的例子所示:



!-- Credit： Julien Leconte/ http：//

www.julienlecomtc.net/blog/2007/09/16/ --
〈target naine=11 js • compress"
apply executable-*java" parallel^"false"
fileset dir="${build.dir" includes^"output. js'1 / 
arg line-"-jar"/
arg path="${yuicompressor.jar)"/
arg line="~o ${build.dir)/output-min.js"/
srcfile/
/apply
/target
SampleAntDir/buildxml
该H标包含丫一个文件output.js,由构建过程生成的，并传递给YUI压缩器。输出文件指定为 同一U录下的〇utput-min. js。这里假设yuicompressor _ jar属性包含了 YUI压缩器的jar文件 的位置。然后可以使用以下命令运行这个目标：
ant js.compress
所有的JavaScript文件在部署到牛.产环境之前，都应该使用YUI压缩器或者类似的工具进行压缩。 给构建过程添加一个/玉缩JavaScript文件的环节以确保每次都进行这个操作。
2. HTTP压缩
配m指的是实际从服务器传送到浏览器的字节数。因为现在的服务器和浏览器都有压缩功能，这个 字节数不一定和代码松度一样。所有的五大Web浏览器（IE、Firefox、Safari、Chrome和Opera)都支 持对所接收的资源进行客户端解压缩。这样服务器端就可以使用服务器端相关功能来压缩JavaScript文 件。一个指定/文件使川了给定格式进行了压缩的HTTP头包含在了服务器响应中。接着浏览器会査看 该HTTP头确定文件是否已被H{缩，然后使用合适的格式进行解压缩。结果是和原来的代码量相比在网 络中传递的字节数量大大减少了。
对于 Apache Web 服务器，有两个模块wj■以进行 HTTP 压缩:mod_g2ip( Apache] .3.x 和 mod_def late (Apache2_0.x)。对于可以给httpd.conf文件或者是.htaccess文件添加以下代码启用对 JavaScript的自动压缩：
#告诉:rod_zip要包含任何以.js结尾的文件
mod_g z i p_i t em^include	file \.js$
该行代码告诉m〇d_zip要包含来ft浏览器请求的任何以.js结尾的文件。假设你所有的JavaScript 文件都以.js结尾，就可以压缩所有请求并应用合适的HTTP头以表示内容已被H{缩。关于modLzip 的更多倍息，请访问项目网站 http: //www. sourceforge.net/projects/znoc3-gz.ip/。
对于mod_def late,可以类似添加一行代码以保证JavaScript文件在被发送之前已被压缩。将以下 这一行代码添加到httpd.conf文件或者是.htaccess文件中：
丼告诉rr.od_deflate要包含所有的JavaScript文件 AddOutputFilterByType DEFIATE application/x-javascript
注意这一行代码用到了响应的MIME类型来确定是否对其进行压缩。记住虽然script：^ type
属性用的是 text/javascript,但是 JavaScript 文件一般译是用 application/x-javascript 作为 其服务的 MIME 类型。关于 moi_def late 的更多信息，请访问 

http://httpd.apache.Org/docs/2.0/mod/ mod_deflate.html 〇
mod^gzip和mod_deflate都可以节爸大约70%的JavaScript文件大小。这很大程度上是因为 JavaScript都是文本文件，因此可以非常有效地进行压缩。减少文件的配®可以减少需要传输到浏览器 的时间。记住舍一点点细微的代价，因为服务器必须花时间对每个请求压缩文件，当浏览器接收到这些 文件后也®要花-些时间解压缩。不过，一般来说，这个代价还是值得的。
(^\ 大部分Web服务器，开源的或是商业的，都有一些HTTP压缩功能。请查看服 务器的文档说明以确定如何合适地配置压缩。
##  24.4 小结
随着JavaScript开发的成熟，也出现了很多最佳实践。过去一度认为只是一种爱好的东西现在变成 了正当的职业，同时还需要经历过去其他编程语存要做的一些研究，如可维护性、性能和部署。 JavaScript中的可维护性部分涉及到下面的代码约定。
- [ ] 来自艽他语言中的代码约定可以用丁-决定何时进行注释，以及如何进行缩进，不过JavaScript 需要针对其松散类塑的性质创造一些特殊的约定。
- [ ] 由于JavaScript必须与HTML和CSS共存，所以让各自完全定义其ft己的目的非常重要： JavaScript应该定义行为，HTML应该定义内容，CSS应该定义外观。
- [ ] 这些职责的混淆会导致难以调试的错误和维护上的问题。
随着Web应用中的JavaScript数虽的增加，性能变得更加重要，因此，你盖要牢记以下事项。
- [ ] JavaScript执行所花费的时间直接影响到整个Web页面的性能，所以其重要性是不能忽略的。 
- [ ] 针对基于C的语言的很多性能的建议也适用于JavaScript,如有关循环性能和使用switch语句 替代if语句。
- [ ] 还有一个要记住的重要事情，即D0M交互开销很大，所以需要限制DOM操作的次数。
流程的最后一步是部署。本章讨论了以下一些关键点。
- [ ] 为了协助部署，推荐设置一个可以将JavaScript合并为较少文件（理想情况是一个）的构建过程。 
- [ ] 有了构建过程也可以对源代码自动运行额外的处理和过滤。例如你可以运行JavaScript验证器 来确保没有语法错误或者是代码没有潜在的问题。
- [ ] 在部署前推荐使用压缩器将文件尽可能变小。
- [ ] 和HTTP压缩一起使用可以让JavaScript文件尽可能小，因此对整体页面性能的影响也会最小。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter23.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter25.md)
