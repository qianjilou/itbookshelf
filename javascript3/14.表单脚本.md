#  第14章 表单脚本([返回首页](https://github.com/qianjilou/javascript3))
本章内容
- [ ] 理解表单 口文本框验证与交瓦
- [ ] 使川其他表单控制  

JavaScript最初的一个应用，就是分担服务器处理表单的责任，打破处处依赖服务器的局面。尽
管H前的Web和JavaScript已经有了长足的发展，但Web表单的变化并不明显。由于Web表单
没有为许多常见任务提供现成的解决手段，很多开发人员不仅会在验证表单时使用JavaScirpt,而且还
增强了一些标准表单控件的默认行为。
##  14.1 表单的基础知识
在HTML中，表单是由formX素来表乃的，而在JavaScript中，表单■对应的则是HTMLForm- Element类玴。HTMLFormElement继承了 HTMLElement,因而与其他HTML元素具有相同的默认属 性。不过，HTMLFormElement也有它自己下列独有的属性和方法。
口 acceptCharset:服务器能够处理的字符集;等价于HTML中的accept-charset特性。
- [ ]  action:接受请求的URL;等价于HTML中的action特件。
- [ ]  elements:表单中所有控件的集合（HTMLCollection)。
口 enctype:请求的编码类组;等价于HTML中的enctype特性。
口 length:表单中控件的数fl。
- [ ]  method:要发送的HTTP请求类型，通常是"get"或-post”;等价于HTML的method特性。
- [ ]  name:表单的名称;等价于HTML的name特性。
- [ ]  reset():将所有表单域重置为默认值。
- [ ]  submit ():提交表单。
- [ ]  target:用于发送请求和接收响应的窗口名称;等价于HTML的target特性。
取得£〇^1!元索引用的方式有好几种。其中最常见的方式就是将它看成与其他元素一样，并为其 添加id特性，然后再像下面这样使用getElementByld()方法找到它。
var form = docuffieiU..getElementById{*forml");
其次，通过document, forms可以取得页面中所有的表单。在这个集合中，可以通过数值索引或 name值来取得特定的表单，如下面的例子所示。

14.1 表单的基础知识	413
var'firstForm = docuinent. forms [0];	//取得页面中的第一个表单
var myForm = document. forms t"form2" 1 ;	//取得页面中名称为 *form2"的表单
另外，在较早的浏览器或者那些支持向后兼容的浏览器中，也会把每个设置了 name特性的表单作 为属性保存在document对象中。例如，通过document.form2可以访问到名为"form2"的表单。不 过，我们不推荐使用这种方式：一是容易出错，二是将来的浏览器可能会不支持。
注意，可以同时为表单指定id和name属性，但它们的值不一定相同。
14.1.1提交表单
用户单击提交按钮或图像按钮时，就会提交表单。使用1叩1^或1^1：〇11都可以定义提交按钮， 只要将其type特性的值设置为"submit"即可，而图像按钮则是通过将:1叩此的type特性值设置为 •image’’来定义的。W此，只要我们单击以下代码生成的按钮，就可以提交表单。
!--通用提交按--
input type=* submit* values "Submit Forr."
!--自定义提交按扭--
button type?submit _Subfnit： Form/button
!--图像桉4a --
input types"image* src="graphic.gif"
只要表单中存在上而列出的任何一种按钮，那么在相应表单控件拥有焦点的情况下，按回车键就可 以提交该表单。（textarea是一个例外，在文本区中回车会换行。）如果表单里没有提交按钮，按回车 键不会提交表单。
以这种方式提交表单时，浏览器会在将请求发送给服务器之前触发submit事件。这样，我们就有 机会验证表单数据，并据以决定是否允许表单提交。阻止这个事件的默认行为就可以取消表单提交。例 如，下列代码会阻止表单提交。
var form = document.getElementById("myForm");
EventUtil.addHandler(form, "submit", function(event){
//取得事件对象
event = EventUtil.getEvent(event);
//Ea止默认事忤
EventUtil.preventDefault(event);
));
这里使用了第13章定义的EventUtil对象，以便跨浏览器处理事件。调用prevetnDefaultU 方法阻止了表单提交。一般来说，在表单数据无效而不能发送给服务器时，可以使用这一技术。
在JavaScript中，以编程方式调用submit 〇方法也可以提交表单。而且，这种方式无需表单包含 提交按钮，任何时候都可以正常提交表单。来看一个例子。
var form = docuinent .getElementById ("myForm*);
//提交表单 form, aulxolt ();
在以调用submit ()方法的形式提交表单时，不会触发submit事件，因此要记得在调用此方法之 前先验证表单数据。

414 第14章表单脚本
提交表单时可能出现的最大问题，就是重复提交表单。在第一次提交表单后，如果长时间没有反 应，用户可能会变得不耐烦。这时候，他们也许会反复单击提交按钮。结果往往很麻烦（因为服务器 要处理重a的请求），或者会造成错误（如果用户是下订单，那么可能会多订好几份)。解决这一问题 的办法有两个：在第•次提交表单后就禁用提交按钮，或者利用onsubmit唞件处理程序取消后续的 表单提交操作。
14.1.2重置表单
在用户单di•重置按钮时，表单会被車:置。使用type特性值为-reset^Jcinputi^cbutton:^ 可以创建®置按钮，如F面的例子所示。
!--通用重置接相一
input types"reset* value="Reset Form"
!--自定义重置按扭--
button type="reset"Reset Form/button
这两个按钮都可以用来重置表单。在重置表单时，所有表单字段都会恢复到页面刚加载完毕时的初 始值。如果某个字段的初始值为空，就会恢复为空;而带有默认值的字段，也会恢复为默认值。
用户单;fc®置按钮取置表单时，会触发reset事件。利用这个机会，我们可以在必要时取消重置 操作。例如，下面展示T阻止*置表单的代码。
var form = document.getElemenCByld("rnyForm-);
EventUtil.addHandler(form, "reset", function(event){
//取得事件对象
event = Ever.tUtil.getEver.t{event) ?
//阻止表单重置
EventUtil.preventDefault(event);
};
与提交表单一样，也可以通过JavaScript来熏置表单，如下面的例子所示。
var form = document.getElomentByld("myForm*);
//重置表单 fora.reset();
与调用submit ()方法不同，调用reset ()方法会像申-击重置按钮一样触发reset事件。
在Web表单设计中，重置表单通常意味着对已经填写的數据不满意。重置表单 经常会导致用户摸不着头脑，如果意外地触发了表单重置事件，那么用户甚至会#■恼 火。事实上，重置表单的需求是很少见的。更常见的做法是提供一个取消按紐，让用 户能够固到前一个页面，而不是不分青红皂白地重置表单中的所有值。
14.1.3表单字段
可以像访问页面中的其他元素一样，使用原生DOM方法访问表单元素。此外，每个表单都有



14.1 表单的基础知识	415
elements属性，该属性是表单中所有元素的集合。这个elements集合是一个有序列表，其中包含着 表单中的所有宇段，例如input、textarea、button3^fieldset。每个表单字段在elements 集合中的顺序，与它们出现在标记中的顺序相同，可以按照位置和name特性来访问它们。下面来看一 个例子。
var form = document.getElementByld{•forrol*);
//取得表单中的第一个字段
var fieldl = form.elements(0]?
/ /取得名为” t extboxl •的字段
var field2 = form.elements["textboxl"];
//取得表单中包含的字段的数量
var fieldCount = form.elements.length;
如果有多个表单控件都在使用一个name (如单选按钮），那么就会返回以该name命名的一个 NodeList。例如，以下面的HTML代码片段为例。
form method="post" id=*myFormM ul
lixinput type= "radio" name="color" lixinput type=*radio" naroe=**color" lixinput type=_radio* naine="color"
/ul
/form
value=■red"Red/1i  value="green"Green/li value="blue"Blue/li
FormFieldsExample01.htm
在这个HTML表单中，有3个单选按钮，它们的name都是"color •，意味着这3个字段是一起的。 在访问elements color1*]时，就会返冋一下NodeList，其中包含这3个兀素;不过，如果访问 elements[0],贝(1只会返回第一个兀素。来看下面的例子。
var form = docxunent .getElementById{ •rnyForm");
var colorFields * form.elements["color*]? alert(colorFields.length);	//3
var firstColorField = colorFields[OJ;
var firstFormField = form.elements(OJ;
alert(firstColorField === firstFormField); //true
FormFieldsExampleOI.htm
以上代码显通过form.elements[0】访问到的第一个表单字段，与包含在form.elements [-color-]中的第一个元素相同。
也可以通过访问表单的属性来访问元素，例如f〇rm[0]可以取得第一个表单字 段，而forxn[*color*]则可以取得第一个命名字段。这些属性与通过elements集 合访问到的元素是相同的。但是，我们应该尽可能使用elements,通过表单属性访 问元素只是为了与旧浏览器向后兼容而保留的一种过渡方式。

416 第14章表单脚本
共有的表单字段厲性
除了 £^1加的元素之外，所有表单字段都拥有相同的一组属性。由于input，型可以表示多 种表单字段，因此有拽属性只适用于某些字段，m还有一些属性是所有字段所共有的。表单字段共有的 属性和方法如下。
disabled:布尔值，表示当前字段是否被禁用。
form:指向当前字段所属表单的指针;只读。
name:当前字段的名称。
readonly:布尔值，表示当前字段是否只读。
tablndex:表7TC冉前字段的切换（tab)序号。
口 type:肖前字段的类型，如"checkbox"、"radio_,等等。
value:当前字段将被提交给服务器的值。对文件字段来说，这个属性是只读的，包含着文件 在计算机中的路径。
除了 form属性之外，可以通过JavaScript动态修改其他任何属性。来看下面的例子：
var form = document.getElementById(MmyFonn"); var field = form.elements[0];
//修改value属性
field.value = "Another value";
//检查form属性的值
alert(field.form form); //true
//把焦点设置到每前字段 field.focus();
//禁用当前字段
field.disabled = true;
//修改type属性（不推荐，但对input:来说是可行的 field.type = "checkbox";
能够动态修改表单字段《性，意味着我们可以在任何时候，以任何方式来动态操作表单。例如，很 多用户可能会重复单击表单的提交按钮。在涉及信用卡消费时，这就是个问题：因为会导致费用翻番。 为此，最常见的解决方案，就是在第一次单击后就禁用提交按钮。只要侦听submit事件，并在该亊件 发生时禁用提交按钮即可。以下就是这样一个例子。
//进免多次提交表单
EventUtil. addHandler {f ornr# "submit", funcc ion (event) { event = EventUt i1.ge t Event(event); var target = EventUtil.getTarget(event)?
//取得提交按钮
var btn = target.elements t"submit-btn"J;
//禁用它
btn.disabled = true?
FormFielchExampk02.htm

14.1 表单的基础知识	417
以上代码为表单的submit事件添加了一个事件处理程序。事件触发后，代码取得了提交按钮 并将其disabled属性设置为true。注意.不能通过onclick事件处理程序来实现这个功能，原 因是不同浏览器之间存在“时差”：有的浏览器会在触发表单的submit事件之前触发click事件， 而有的浏览器则相反。对于先触发click事件的浏览器，意味着会在提交发生之前禁用按钮，结果 永远都不会提交表单。因此，最好是通过submit事件来禁用提交按钮。不过，这种方式不适合表 单中不包含提交按钮的情况;如前所述，只有在包含提交按钮的情况下,才有可能触发表单的submit 事件。
除了之外，所有表单字段都有type属性。对于input元素，这个值等于HTML特 性type的值。对于其他元素，这个type属性的值如下表所列。
此夕卜，:1叩111;和131比;:〇11元素的type属性是可以动态修改的，而select元素的type属性 则是只读的。
共有的表单字段方法
每个表单字段都有两个方法：focus (和blur (。其中，focus ()方法用于将浏览器的焦点设置 到表单字段，即激活表单字段，使其可以响应键盘枣件。例如，接收到焦点的文本框会显示插入符号， 随时可以接收输人。使用focus ()方法，可以将用户的注意力吸引到页面中的某个部位。例如，在页面 加载完毕后，将焦点转移到表单中的第-个字段。为此，可以侦听页面的load事件，并在该事件发生 时在表单的第一个字段上调用focus 〇方法，如下而的例子所示。
EventUtil.addHandler(window, "load™ # function(event){ document.forms[0]♦elements【OJ•focus(;
}};
要注意的是，如果第一个表单字段是一个input元素，且其type特性的值为"hidden,那么 以上代码会导致错误。另外，如果使用CSS的display和visibility属性隐藏了该字段，同样也会
导致错误。
HTML5为表单字段新增了一个autofocus属性。在支持这个属性的浏览器中，只要设置这个属性, 不用JavaScript就能自动把焦点移动到相应字段。例如：
input type="textu autofocus
为了保证前面的代码在设置autofocus的浏览器中正常运行，必须先检测是否设置了该属性，如 果设置了，就不用再调用f〇CUS( 了D



EventUtil.addHandler(window, "load", function(event){ var element = document.forms[0].elements[0];
14

418 第14章表单脚本
));
if (element.autofocus i == true){
element. focus (); console, log (**JS focus");
FocusExample01.htm
因为autofocus是一个布尔值《性，所以在支持的浏览器中它的值应该是true。（在不支持的浏 览器中，它的值将是空字符串。）为此，上面的代码只有在autofocus不等于true的情况下才会调用 focus ()，从而保证向前兼容。支持autofocus M性的浏览器有Firefox 4+、Safari 5+、Chrome和Opera 9.6。

在默认情况下，只有表单字段可以获得焦点。对于其他元素而言，如果先将其 tablndex属性设置为-1,然后再调用focus(方法，也可以让这些元素获得焦点。 只有Opera不支持这种技术。
与focus (方法相对的是blur (方法，它的作用是从元素中移走焦点。在调用blur 〇方法时， 并不会把焦点转移到某个特定的元素上;仅仅是将焦点从调用这个方法的元素上面移走而已。在早期 Web开发中，那时候的表单字段还没有readonly特性，W此就可以使用blur ()方法来创建只读字段。 现在，虽然需要使用blur (的场合不多了，但必要时还可以使用的。用法如下：
document.formsCO].elements[0].blur();
共有的表单字段事件
除了支持E标、键盘、吏改和HTML事件之外，所有表单字段都支持下列3个亊件。
blur:当前字段失去焦点时触发。
 change:对于input*textarea兀素，在它们失去焦点且value值改变时触发;对于 Select元素，在其选项改变时触发。
focus :当前字段获得焦点时触发。
当用户改变了当前字段的焦点，或者我们调用了 blur ()或f〇CUS(方法时，都可以触发blur和 focus事件。这两个事件在所有表单字段中都是相同的。但是，change事件在不同表单控件中触发的 次数会有所不同。对于inputWtextarea元素，当它们从获得焦点到失去焦点.fl value值改变时, 才会触发change事件。对于select:元素，只要用户选择了不同的选项，就会触发change事件; 换句话说，不失去焦点也会触发change事件。
通常，可以使用focus和blur事件来以某种方式改变用户界面，要么是向用户给出视觉提示，要 么是向界面中添加额外的功能（例如，为文本框®示-个下拉选项菜单)。而change事件则经常用丁- 验证用户在字段中输人的数据。例如，假设有一个文本框，我们只允许用户输人数值。此时，可以利用 focus事件修改文本框的背景颜色，以便吏清楚地表明这个字段获得/焦点。可以利用blur事件恢复 文本框的背景颜色，利用change事件在用户输人了非数值字符时再次修改背景颜色。下面就给出了实 现上述功能的代码。
var textbox = document.forms[0].elements 10);
EventUtil.addHandler(textbox, "focus", function(event){ event = EventUtil.getEvent(event);

14.2 文本框脚本	419
var target = HventUtii.getTarget(event)?
if (target.style*backgroundcolor != Ted"){ target.style.backgroundColor = "yellow";
)
});
EventUtil.addHandler(textbox, "blur", function(event){ event = EventUtil.getEvent(event); var target = EventUtil.getTarget(event);
if (/[A\d]/.test(target.value)){
target.style.backgroundColor = ■red•;
} else {
target.style.backgroundColor = ""?
}
});
EventUtil.addHandler(textbox, "change", function(event)( event = EventUtil.getEvent(event); var target = EventUtil.getTarget(event);
if (/IA\d]/.test(target.value){
target.style.backgroundColor = *red";
} else {
target.style.backgroundColor = "•;
});
FormFieldEventsExample01.htm
在此，onfocus事件处理程序将文本框的背景颜色修改为黄色，以清楚地表单当前字段已经激活。 随后，onblur和onchange事件处理程序则会在发现非数值字符时，将文本框背景颜色修改为红色。 为了测试用户输入的是不是非数值，这里针对文本框的value属性使用了简单的正则表达式。而且，
为确保无论文本框的值如何变化，验证规则始终如一，onblur和onchange事件处理程序中使用了相 同的正则表达式。
关于blur和change事件的关系，并没有严格的规定。在某些測览器中，blur 事件会先于change事件发生;而在其他浏览器中，則恰好相反。为此，不能假定这 两个事件总会以某种顺序依次触发，这一点要特别注意。
14.2文本框脚本
在HTML中，有两种方式来表现文本框：一种是使用input元素的单行文本框，另一种是使用 textarea的多行文本框。这两个控件非常相似，而且多数时候的行为也差不多。不过，它们之间仍 然存在一些重要的区別。
要表现文本框，必须将:1叩11;元素的type特性设置为-text*。而通过设置size特性，可以指 定文本框中能够显示的字符数。通过value特性，可以设置文本框的初始值，而maxlength特性则用 于指定文本框可以接受的最大字符数。如果要创建一个文本框，让它能够显示25个字符，但输人不能 超过50个字符，可以使用以下代码：

420 第14章表单脚本
ir.pu'; t:ype="text" size="25" maxlength- "50" values "initial value"
相对而言,textarea元素则始终会呈现为-个多行文本框。要指定文本框的大小，可以使用rows 和cols特性。其中，rows特性指定的是文本框的字符行数，而cols特性指定的是文本框的字符列数 (类似于inpu兀素的size特性）。与1叩111：兀素不同，textarea的初始值必须要放在 textarea^/texCarea之间，如卜面的例子所示。
textarea rows-•25" cols="5"initial value/textarca
另一个与input^[K别在于，不能在HTML中给textarea指定最大字符数。
无论这两种文本框在标记中有什么区别，但它们都会将用户输人的内容保存在value属性中。可 以通过这个属性读取和设置文本框的值，如下而的例子所示：
var textbox = document. forms [0] .eier.ents [" toxtboxl"]; alert(textbox.value);
textbox.value = "Some new value";
我们建议读者俅h面这样使用value属性读取或设置文本框的值，不建议使用标准的DOM方法。 换句话说，不要使用setAttributeO设置input:元素的value特性，也不要去修改textarea 元素的第-个？节点。原W很简单：对value属性所作的修改，不一定会反映在DOM中。因此，在处 理文本框的值时，最好不要使用DOM方法。
14.2.1选择文本
h述两种文本框都支持select ()方法，这个方法用于选择文本框中的所有文本。在调用select () 方法时，大多細览器（Opera除外）都会将焦点设S到文本框中。这个方法不接受参数，可以在任何 时候被阔用。下面来看一个例子。
var textbox = document.forms[0].elements["textboxl"J; textbox.select();
在文本框获得焦点时选抒其所有文本，这是一种非常常见的做法，特别是在文本框包含默认值的时 候。w为这样做可以让用户不必一个一个地删除文本。下面展示了实现这-操作的代码。
EventUtil.addHandler{textbox, "focus", function(event){ event = EventUt i X.getEvent(event); var target = EventUtil.getTarget(event)?
target.select{);
});
TextboxSelectExampleOl. him
将上面的代码应用到文本框之后，只要文本框获得焦点，就会选择其中所有的文本。这种技术能够 较大幅度地提升表单的易w性。
1.选择（select)事件
与select (方法对应的，是一个select事件。在选抒了文本框中的文本时，就会触发select 事件。不过，到底什么时候触发select事件，还会W浏览器而异。在IE9+、Opera、Firefox、Chrome 和Safari中，只有用户选择了文本（而且要释放鼠标），才会触发seiect事件。而在IE8及更早版本中，

14.2 文本框脚本	421
只要用户选择了一个字母（不必释放鼠标)，就会触发select事件。另外，在调用select 〇方法时也 会触发select事件。下面是-个简单的例子。
〇
var textbox = docuinent. forms [0] .elements ["textboxl") ?
EventUtil.addHandler(textbox, "select", function(event){
var alert("Text selected" + textbox.value);
));
SelectEventExQmpleOl.htm
取得选择的文本
虽然通过select事件我们可以知道用户什么时候选择了文本，但仍然不知道用户选择了什么文本。 HTML5通过一些扩展方案解决了这个问题，以便更顺利地取得选择的文本。该规范采取的办法#添加 两个属性：selectionStart和selectionEnd。这两个属性中保存的是基于0的数值，表示iff选择 文本的范围（即文本选区开头和结尾的偏移fi)。因此，要取得用户在文本框中选择的文本，可以使用 如下代码。
function getSelectedText(textbox){
return textbox.value.substring(textbox.selectionStart, textbox.selectionEnd);
}
因为 substring () 方法基于字符串的偏移最执行操作，所以将selectionStart和 selectionEnd直接传给它就可以取得选中的文本。
IE9+、Firefox、Safari、Chrome和Opera都支持这两个属性〇 IE8及之前版本不支持这两个属性， 而是提供了另一种方案。
IE8及更早的版本中有一个document, selection对象，其中保存着用户在整个文档范围内选择 的文本信息;也就是说，无法确定用户选择的是页面中哪个部位的文本。不过，在与select亊件一起 使用的时候，可以假定是用户选择了文本框中的文本，因而触发了该事件。要取得选择的文本，首先必 须创建一个范围（第12章讨论过)，然后再将文本从其中提取出来，如下面的例T所示。
function getSelectedText(textbox){
if (typeof textbox.selectlonStart ■■ *numbor"){
return textbox.value.substring{textbox.selectionStart,
textbox.selectionEnd)?
 else if (document.selection){
return document.select ion.createEange().text;

TextboxCetSelectedTextExampIeOJ, him
这里修改7•前面的闲数，包括/■在IE中取得选择文本的代码。注意，调用document.selection 时，不需要考虑textbox参数。
选择部分文本
HTML5也为选择文本框中的部分文本提供了解决方案，即最早由Firefox引人的 setSelectionRange ()方法。现在除select )方法之^卜，所有文本框都有一个setSelect:ionRange () 方法。这个方法接收两个参数：要选择的第一个字符的索引和要选择的最后一个字符之后的字符的索引 (类似于substring ()方法的两个参数)。来看一个例子。

422 第14章表单脚本
textbox.value = "Hello world!
//选择所有文表
textbox.setSelectionRange(0, textbox.value.length);	//"Hello world!"
//选择前3个字符
textbox.setSelectionRange(0, 3);	//"Hel"
//选择第4到第6个字符
textbox.setSelectionRange(4, 7);	//"o w"
要看到选择的文本，必须在调用setSelectionRange ()之前或之后立即将焦点设置到文本框。 IE9、Firefox、Safari、Chrome 和 Opera 支持这种方案。
IE8及更早版本支持使用范围（第12章讨论过）选择部分文本。要选择文本框中的部分文本，必须 先使用IE在所有文本框上提供的createTextRangeO方法创建一个范围，并将其放在恰当的位置 上。然后，再使用moveStartiU和moveEndO这两个范围方法将范围移动到位。不过，在调用这两个 方法以前，还必须使用collapseO将范围折叠到文本框的开始位置。此时，moveStartO将范围的起
点和终点移动到了相同的位置，只要冉给moveEndO传人要选择的字符总数即可。最后一步，就是使 用范围的select (方法选择文本，如下面的例子所示。
textbox.value = "Hello world!*;
var range = textbox.createTextRangeO ;
//选择所有文本
range.collapse(true);
range.moveStart("character", 0);
range.moveEnd("character", textbox.value.length); //"Hello world!" range.select();
//选择前3个字符
range.collapse{true);
range.moveStart("character"# 0);
range.moveEnd(* character", 3);
range.select{);	//"Hel"
//选择第4到第6个字符
range.collapse(true);
range.moveStart(*character*, 4);
range.moveEnd("character", 3);
range.select();	//"〇 wn
与在其他浏览器中一样，要想在文本框中看到文本被选择的效果，必须让文本框获得焦点。 为了实现跨浏览器编程，可以将上述两种方案组合起来，如下面的例子所示。
function selectText(textbox, startIndex, stoplndex){ if (textbox.setSelectionRange){
textbox.setSelectionRange(startIndex, stoplndex);
} else if (textbox.crcateTextRange){
var range = textbox.createTextRangeO ? range.collapse(true);
range.moveStart("character™, startlndex); range.moveEnd("character", stoplndex — startlndex}; range.select{);

14.2文本框脚本	423
textbox.focus();
TexiboxPartialSelectionExample01.htm
这个selectTexU)函数接收三个参数：要操作的文本框、要选择文本中第一个字符的索引和要选 择文本中最后一个字符之后的索引。首先，函数测试了文本框是否包含setSelectionRangeO方法。 如果有，则使用该方法。否则，检测文本框是否支持createTextRange()方法。如果支持，则通过创 建范W来实现选择。最后一步，就是为文本框设置焦点，以便用户看到文本框中选择的文本。可以像下 面这样使用selectText (方法。
textbox.value = ftHello world!"
//选择所有文本
selectText(textbox, 0, textbox.value.length); //"Hello world!"
//选择前3个字符
selectText(textbox, 0, 3)?	//"He!"
//选择第4到第6个字符
selectText(textbox, 4, 7);	//"〇 w*
选择部分文本的技术在实现髙级文本输入框时很有阳，例如提供0动完成建议的文本框就可以使用 这种技术。
14.2.2过滤输入
我们经常会要求用户在文本框中输人特定的数据，或者输人特定格式的数据。例如，必须包含某些 宇符，或者必须匹配某种模式。由于文本框在默认情况K没有提供多少验证数据的手段，因此必须使用 JavaScript来完成此类过滤输人的操作。而综合运用事件和DOM手段，就可以将普通的文本框转换成能 够理解用户输入数据的功能型控件。
屏蔽字符
有时候，我们需要用户输入的文本中包含或不包含某些字符。例如，电话号码中不能包含非数值字 符。如前所述，响应向文本框中插人字符操作的是keypress事件。因此，可以通过阻止这个事件的默 认行为来屏蔽此类字符。在极端的情况下，可以通过下列代码屏蔽所有按键操作。
EventUtil.addHandler(textbox, "keypress", function{event){ event = EventUtil.getEvenc(event);
EventUtil.preventDefault(event);
);
运行以上代码后，由于所有按键操作都将被屏蔽，结果会导致文本框变成只读的。如果只想屏蔽特 定的字符，则需要检测keypress事件对戍的字符编码，然后再决定如何响应。例如，下列代码只允许 用户输人数值。
EventUtil.addHandler(textbox, "keypress", function(event){ event = EventUtil.getEvent(event); var target ■ BventUtil*getTarget(evont); var cbarCode « BventUtil.getCharCode(event:);
if (l/\d/.teat(String,£romCharCode(charCodd)))

424 第14章表单脚本
BventUtil.preventDefault(event);

});
在这个例子中，我们使用EventUtil.getCharCodeU实现了跨浏览器取得字符编码。然后，使 用String. f romcharcode ()将字符编码转换成字符串，再使用正则表达式八d/来测试该字符串，从 而确定用户输人的是不是数值。如果测试失败，那么就使用EventUtil.preventDefault()屏蔽按键 事件。结果，文本框就会忽略所有输人的非数值。
虽然理论h■只应该在用户按F字符键时才触发keypress事件，但有些浏览器也会对其他键触发此 亊件。Firefox和Safari (3.1版本以前）会对向上键、向下键、退格键和删除键触发keypress事件; Safari 3.1及更新版本则不会对这踔键触发keypress事件。这意味着，仅考虑到屏蔽不是数值的字符还 不够，还要避免屏蔽这些极为常用和必要的键。所幸的是，要检测这些键并不困难。在Firefox中，所 有由非字符键触发的keypress事件对应的字符编码为0,而在Safari 3以前的版本中，对应的字符编 码全部为8。为了it代码更通用，只要不屏蔽那些字符编码小于10的键即可。故而，可以将上面的函数 重写成如下所示。
EventUtil.addHandler(textbox, "keypress", function(event){ event = EventUtil.gecEvent(event); var target = EventUtil.getTarget(event); var charCode = EventUtil.getCharCode(event);
if (!/\d/.teat(String.fromCharCode(charCode)) && charCode  9){
EventUtil.preventDefault(event);
}
});
这样，我们的事件处理程序就可以适用所有浏览器了，即可以屏蔽非数值字符，但不屏蔽那些也会 触发keypress事件的基本按键。
除此之外，还有一个问题需要处理：复制、粘贴及其他操作还要用到Ctrl键。在除IE之外的所有 浏览器中，前面的代码也会屏蔽Ctrl+C、Ctrl+V,以及其他使用Ctrl的组合键。因此，最后还要添加一 个检测条件，以确保用户没冇按下Ctrl键，如下面的例子所示。
EventUtil.addHandler(textbox, "keypress", function(event){ event = EventUtil.getEvenc(event); var target = EventUtil.getTarget{event); var charCode = EventUtil.getCharCode{event);
if  !/\d/.test(String.froiaCbarCode (charCode)  charCode  9 1event.ctrXKey)
EventUtil.preventDefault(event);
TextboxInpuiFilteringExampleO 1. him
经过最后一点修改，就耐以确保文本框的行为完全正常/。在这个例子的基础上加以修改和调整， 就可以将同样的技术运用于放过和屏蔽任何输人文本框的字符。
操作剪貼板
IE是第一个支持与剪貼板相关事件，以及通过JavaScript访问剪貼板数据的浏览器。IE的实现成为 了亊实上的标准，不仅Safari 2、Chrome和Firefox 3也都支持类似的事件和剪贴板访问（Opera不支持

14.2 文本框脚本	425
通过JavaScript访问剪贴板），HTML 5后来也把剪贴板事件纳人/规范。下列就是6个剪贴板事件。
beforecopy:在发生复制操作前触发。
copy:在发生复制操作时触发。
beforecut:在发生剪切操作前触发。
cut:在发生剪切操作时触发。
beforepaste:在发生粘贴操作前触发。
paste:在发生粘贴操作时触发。
由于没冇针对剪贴板操作的标准，这些事件及相义对象会因浏览器时异。在Safari Xhrotne和Firefox 中，beforecopy、beforecut和beforepaste事件只会在fiTT?针对文本柩的上下文菜单（预期将发 生剪贴板事件）的情况下触发。但是，IE则会在触发copy、cut和paste亊件之前先行触发这些事件。 至于copy、cut和paste亊件，只耍是在上下文菜单中选择了相应选项，或者使用了相应的键盘组合 键，所有浏览器都会触发它们。
在实际的事件发生之前，通过beforecopy、beforecut和beforepaste事件可以在向剪贴板发 送数据，或者从剪贴板取得数据之前修改数据。+过，取消这些事件并不会取消对剪贴板的操作——只 有取消copy、cut和paste事件，才能阻止相应操作发生。
要访问剪贴板中的数据，可以使用clipboardData对象：在IE中，这个对象是window对象的 属性;而在Firefox 4+、Safari和Chrome中，这个对象是相应event对象的属性。_伹是，在Firefox、 Safari和Chorme中，只有在处理剪贴板事件期间clipboardData对象才有效，这是为了防止对剪贴板 的未授权访问;在中，则可以随时访问clipboardData对象。为了确保跨浏览器兼容性，最好只 在发生剪贴板事件期间使用这个对象。
这个 clipboardData 对象有三个方法：getData U、setData ()和 clearData ()。其中，getData () 用于从剪贴板中取得数据，它接受•个参数，即要取得的数据的格式。在IE中，有两种数据格式："text” 和"URL"。在Firefox、Safari和Chrome中，这个参数是+•种MIME类型;不过，可以用"text'•代表 "text/plain'o
类似地，SetData()方法的第一个参数也是数据类型，第二个参数是要放在剪贴板中的文本。对于 第一个参数，IE照样支持-text"和"URL”，而Safari和Chrome仍然只支持MIME类型。佴是，与 getData ()方法不同的是，Safari和Chrome的setDaCa ()方法不能识别"text1■类33。这两个浏览器在 成功将文本放到剪贴板中后.都会返囬true;否则，返回false。为了弥合这些差异，我们可以向 EventUtil中再添加下列方法。
var EventUtil = {
//决略的代码
getClipboardText.i function(event) 
var clipboardData 羼(event.clipboardData II window.clipboardData)/ return clipboardData.getDataCtext");
//省略的代码
setClipboardText: function(event, value){ if (event.clipboardData){
return event.olipbo&rdBata. setData(Rtext/plain*', value);

426 第14章表单脚本
 else if (window.clipboardData){
return window*clipboardData.setData("textR« value)?
)
//省略的代码
);
EventUtil.js
这里的getClipboardText)方法相对简单;它只要确定clipboardData对象的位置，然后再 以"text1•类截调用getData)方法即可。相/左地，setClipboardText()方法则要稍微复杂一些。在 取得clipboardData对象之后，需要根据不同的浏览器实现为setData (传入不同的类型(对于Safari 和 Chrome,是"text/plain”;对于 IE,是•text”）〇
在需要确保粘贴到文本框屮的文本中包含某些字符，或者符合某种格式要求时，能够访问剪贴板是非 常有用的。例如，如果…个文本框只接受数值，那么就必须检测粘贴过来的值，以确保有效。在paste 事件中，可以确定剪贴板中的值是否有效，如果无效，就可以像F面示例中那样，取消默认的行为。
EventUtil.addHandler(textbox, "paste”， function(event){ event = SventUtil.getEvent(event); var text = EventUtil.getClipboardText(event);
if 〇/^\d*$/.cest{text)) {
EventUtil.preventDefault(event);

TextboxClipboardExampleOl. htm
在这里，onpaste事件处理程序可以确保只有数值才会被粘貼到文本框中。如果剪贴板的值与正 则表达式不匹配，则会取消粘贴操作。Firefox、Safari和Chrome只允许在onpaste事件处理程序中访 问 getData ( 方法。
由于并非所有浏览器都支持访问剪贴板，所以更简单的做法是屏蔽一或多个剪贴板操作。在支持 copy、cut和paste事件的浏览器中（IE、Safari、Chrome和Firefox3及更高版本），很容易阻止这些 事件的默认行为。在Opera中，则需要阻止那些会触发这些事件的按键操作，同时还要阻止在文本框中 显示上下文菜单。
14.2.3自动切换焦点
使用JavaScript可以从多个方面增强表单字段的易用性。艽中，最常见的•种方式就是在用户填写 完当前字段时，自动将焦点切换到下一个宇段。通常，在自动切换焦点之前，必须知道用户已经输人了 既定长度的数据（例如电话号码)。例如，美W的电话号码通常会分为三部分：区号、局号和另外4位 数字。为取得完整的电话号码，很多网页中都会提供下列3个文本框：
input type=-text" name="tell" id="txtTell_ maxlength=•3•
input types "text" name="tel2 * id="txt.Tel2" maxlength= • 3 ■ 
input type="text" name-"tel3" id="txtTe!3" maxiength=•4•
Textbox TabForwardExampleOl. htm

14.2文本框脚本	427
为增强易用性，同时加快数据输人，可以在前一个文本框中的字符达到最大数每:后，A动将焦点切 换到下.个文本框。换句话说，用户在第•个文本框中输人了 3个数字之后，焦点就会切换到第二个文 本框，再输人3个数卞，焦点又会切换到第三个文本框。这种“IH动切换焦点”的功能，吋以通过卜'列 代码实现：



{function。（
function tabForward{event){
event BverxtUtil .geCEvent (event);
var target s EventUtil.getTarget(evenc);
if (target.value, length == target .maxLengr.h) {
var form = target.form?
for (var i=0, len=form.elements.length; i  len; i++) {
if (form.elements[i) == target) {
if (form.elements[i+1]){
forni.elements [i+1] . focus ();
}
return;



var textboxl = document.getElementByldt^txtTell"); var textbox2 = document .getElementById( •txtTel2,'); var textbox3 = documer.c.getEiementById(,,txtTel3");
EventUtil.addHandler(textboxl, "keyup", tabForward)? EventUtil.addHandler(textbox2/ "keyup", tabForward〉; EventUti1.addHandler(textbox3, "keyup", tabForward);
 0 ?
TextboxTabForwardExampleOl. htm
开始的tabForward (确数是实现“自动切换焦点”的关键所在。这个函数通过比较用户输人的值 与文本框的maxlength特性，可以确定是杏已经达到最大长度。如果这两个值相等（因为浏览器最终 会强制它们相等，因此用户绝不会多输人字符），则耑要査找表单字段集合，直至找到下一个文本框。 找到下一个文本框之后，则将焦点切换到该文本框。然后，我们把这个闲数指定为每个文本框的onkeyup 亊件处理程序。由于keyup事件会在用户输人r新字符之后触发，所以此时是检测文本框中内容K度 的最佳时机。这样一来，用户在填写这个简单的表单时，就不必再通过按制表键切换表单字段和提交表 甲-了。
不过清记住，这些代码只适用f前面给出的标记，而且没有考虑隐藏宁•段。
HTML5 约束验证 API
为了在将表单提交到服务器之前验证数据,HTML5新增了一些功能。有了这些功能，即便JavaScript 被禁用或者出于种种原W未能加载，也可以确保基本的验证。换句话说，浏览器A己会根据标记中的规 则执行验证，然后自己显示适3的错误消息（完全不用JavaScript插手)。当然，这个功能只有在支持 HTML5这部分内容的浏览器中才有效，这些浏览器有Firefox4+、Safari 5+、Chrome和Opera 10+。

428 第14章表单脚本
只有在某些情况下表单字段才能进行A动验证。具体来说，就是要在HTML标记中为特定的字段 指定-些约束，然后浏览器才会H动执行表单验证。
必填字段
第一种情况是在表单字段中指定了 required厲性，如下而的例子所示：
 input type-"text" name - "use r narr.e" required〉
任何标注有required的字段，在提交表单时都不能空若。这个属性适用于input、textarea 和select字段（Opera 11及之前版本还不支持5616^的required屈性)。在JavaScript中，通过 对应的required属性，可以检査某个表单字段是否为必填字段。
var isUserna^eRequired - document,formsfOJ.elements["username"].required;
另外，使用下面这行代码可以测试浏览器是否支持required属性。
var isRequiredSupported = "required" in document.createElement("input");
以上代码通过特性检测来确定新创建的input:^索中是否存在required属性。
对丁-空着的必填字段，不同浏览器有不同的处理方式。Firefbx 4和Opera 1丨会阻止表单提交并在相 应字段下方弹出帮助框，而Safari ( 5之前）和Chrome (9之前）则什么也不做，而且也不阻止表单提 交。
其他输入类型
HTML5 *input元索的type 14性又增加了几个值。这些新的类型不仅能反映数据类型的信息， 而且还能提供•跸默认的验证功能。其中，"email•和〜rl"是两个得到支持M多的类型，各浏览器也 都为它们增加Y定制的验证机制。例如：
input type="eir.ail" name ^nemailB
input type="url" name="homepage•
顾名思义，”emai 1 ’’类型要求输人的文本必须符合电子邮件地址的模式，而"url"类型要求输入的 文本必须符介URL的模式。不过，本节前面提到的浏览器在恰当地匹配模式方面都存在问题。最明显 的是会被当成-个有效的电子邮件地址。浏览器开发商还在解决这些问题。
要检测浏览器是否支持这些新类沏，可以在JavaScript创建一个inpUt元素，然后将type属性 设置为"email”或"url",最后冉检测这个M性的值。不支持它们的旧版本浏览器会自动将未知的值设 置为"text而支持的浏览器则会返回正确的值。例如：
var input = document.createEiemenc(■input")?
input.type - "email*;
var isSraailSupported - (input.type == "email”）;
要注意的足，如果不给inpUt:^素设置required M性，那么空文本框也会验证通过。另一方面， 设置特定的输人类型并不能阻止用户输人无效的值，只是应用某些默认的验证而Q。
数值范围
除/"email"和"url% HTML5还定义/另外几个输人元素。这几个元素都要求填写某种基于数

14.2文本框脚本	429
input.stepUp();
input.stepUp(5);
input.stepDown();
input.stepDown(10;
输入模式
HTML5为文本字段新增r pattern属性。这个属性的值是一个正则表达式，用于匹.配文本框中的 值。例如，如果只想允许在文本字段屮输入数值，可以像下面的代码-样应用约朿：
input type="text" pattern-"\d+'r name="count" 
注意，模式的开头和末《不用加^和$符兮（假定已经有了）。这两个符号表示输人的值必须从头到 尾都与模式四配。
与其他输人类型相似，指定pattern也不能阻止用户输人无效的文本。这个模式应用给值，浏览 器来判断值是有效，还是无效。在JavaScript中可以通过pattern属性访问模式。
var pattern = docunK5nt.fonns【0】.elcment:s【*count:_】 .pattern;
使用以下代码可以检测浏览器是否支持pattern属性。
var isPatternSupportcd = "pattern" in document.createElcnient("input");
检测有效性
使用checkValidityU方法可以检测表单中的某个字段足-否有效。所有表单字段都有个方法，如 果字段的值有效，这个方法返冋true,否则返回false。字段的值足否有效的判断依据是本节前面介 绍过的那些约束。换句话说，必填字段中如果没有值就是无效的，而字段中的值与pattern ®性不闪 配也是无效的。例如：
if (document.forms[0】•elements[〇1.checkValidity⑴{
//字段有效，继续
} else {
//字段无效
字的值："number,f ^ ” range' "datetime" "datetime-localw ^ ’date”、"month、''week", 还有"time•。浏览器对这儿个类型的支持情况并不好，闪此如果真想选用的话，要特别小心。S前， 浏览器开发商主要关汴更好的跨平台兼容性以及更多的逻辑功能。W此，本节介绍的内容某种程度上有 按超前，不一定马丨:就能在实际开发中使用。
对所有这些数值类型的输人元索，可以指定min属性（最小的可能值）、max属性（最大的可能值） 和step属性（从min到max的两个刻度间的差值)。例如，想让用户只能输人0到100的值，而且这 个值必须是5的倍数，可以这样写代码：
input type="number" min="0" max="100" step="5" name=*countp
在不同的浏览器中，可能会也可能不会肴到能够自动递增和递减的数值调节按钮（向上和向下按
钮)。
以上这些属性在JavaScript中都能通过对应的元素访问（或修改)。此外，还有两个方法：stepUp ( 和stepDown(),都接收•个可选的参数：要在当前值基础上加上或减去的数值。（默认是加或减丨。） 这两个方法还没有得到任何浏览器支持，但下面的例子演示了它们的用法。
加加減减
/ / / / / / / /

430 第14幸表单脚本
要检测整个表单是否有效，可以在去单自身调用checkValidityU方法。如果所有表单字段都有 效，这个方法返冋true;即使有冲字段无效，这个方法也会返问Ealse。
if(document.forms[0).checkValidity()){
"表单有效，继续 } else {
"表单无效

与checkValidity)方法简申.地转诉你字段足否有效相比，validity属性则会告诉你为什么字 段夯效或无效。这个对象中包含一系列M性，每个诚性会返回一个布尔值。
customError:如果设置了 setCustomValidity(,则为 true, 侧返回 false。
patternMismatch:如果值与指定的pattern属性不匹配，返冋true。
rangeOverflow:如果值比max 值大，返Pltrue。
rangeUnderflow:如果值比min 值小，返回 true。
stepMisMatch:如果min和max之间的步长值不合理，返回true。
tooLong:如果值的长度超过rmaxlengch属性指定的长度，返Mtrue〇有的浏览器（如Fitefox4) 会自动约束因雌^返冋false。
 typeMismacch:如果值不是•.或.’url"要求的格式，返冋true。
valid:如果这里的其他诚性都是fa】se，返回true。checkValidity()也要求相同的值。
valueMissing:如果标注为required的字段中没有值，返回true。
因此，要想得到更具体的信息，就应该使用validity属性来检测表单的有效性。下面是一个例子。
if (input.validity && !input.validity.valid){ if (input.validity.valueMissing){ alert("Please specify a value.")
} else if (input.validity.typeMismatch){
alert("Please enter an email address.");
} else {
alert("Value is invalid.");
禁用验证
通过设置noval idate属性，可以街诉表单不进行验证。
fom method-"post" actions■ signup.php" novalidate
!--这里祐入表单元素—
/form
在JavaScript中使用noValidate属性可以取得或设置这个值，如果这个属性存在，值为crue, 如果不存在，值为false。
document. forms [0] .noValidate = true? //禁用验证
如果一个表单中有多个提交按钮，为了指定点击某个提交按钮不必验证表单，可以在相应的按钮上
添加 formnovalidate M性。
form method= "post" action= '*foo.php"
!--这里插入表单元素-
input type=s"submit" value*"Regular SubmitN

14.3选择框脚本	431
input type=Bsubmit" formnovalidate neuoe«nbtnNoValidaten
value="Non-validating Submit"
/fonn
在这个例子中，点击第一个提交按钮会像往常一样验证表单，而点击第二个按钮则会不经过验证iftf 提交表单。使用JavaScript也可以设a这个属性。
//禁用脍证
document • forms【0】.elements【**btnKoValidate_ ]• formNoValidate = true;
14.3选择框脚本
选择框是通过36：^^和〇?1^〇11元素创建的。为了方便与这个控件交瓦，除了所有表单宇段共 有的属性和方法外，HTMLSelectElement类塑还提供了下列属性和方法。
add(neM^Ption, reiQjtionh向控件中插人新《^*:1〇11元素，其位置在相关项（reJQption) 之前。
multiple:布尔值，表示是否允许多项选择;等价于HTML中的multiple特性。
options:控件中所有option兀素的 HTMLCollection。
口 remove (i/idex):移除给定位置的选项。
selectedlndex:基于0的选中项的索引，如果没有选中项，则值为-1。对于支持多选的控件,• 只保存选中项中第一项的索引。
size:选择框中可见的行数;等价于HTML中的size特性。
选择框的type属性不是"select-one1•，就是"select-multiple”，这取决于HTML代码中有 没有multiple特性。选择框的value属性由当前选中项决定，相应规则如下。
- [ ] 如果没有选中的项，则选择框的value属性保存空字符串。
- [ ] 如果冇一个选中项，而且该项的value特性已经在HTML中指定，则选择框的value属性等 于选中项的value特性。即使value特性的值是空字符串，也同样遵循此条规则。
- [ ] 如果有一个选中项，但该项的value特性在HTML中未指定，贝lj选择框的value属性等于该
项的文本。
- [ ] 如果有多个选中项，则选择框的value属性将依据前两条规则取得第一个选中项的值。
以下面的选择框为例：
select names * location" id="selLocati.on,'
option value=*Sunnyvale, CA*Sunnyvale/option
option value-"Los Angeles, CA"Los Angeles/option coption value=•Mountain View, CA"Mountain View/option
option value="■China/option
opt ionAust ra1ia/opt ion
/select
如果用户选择了其中第一项，则选择框的值就是"Sunnyvale, CA"。如果文本为"China"的选项 被选中，则选择框的值就是-个空字符串，W为其value特性是空的。如果选择了最后一项，那么由
于option中没有指定value特性，则选择框的值就是"Australia*。
在DOM中，每个option元素都有一个HTMLOptionElement对象表示。为便于访问数据， HTMLOpticmElement对象添加了下列属性：

432 第14幸表单脚本
index:当前选项在options集合中的索引。
label:当前选项的标签;等价于HTML中的label特性。
selected:布尔值，表示当前选项是否被选中。将这个属性设置为true可以选中当前选项。 口 text:选项的文本。
value:选项的值（等价'T HTML中的value特性）0
其中大部分属性的目的，都是为了方便对选项数据的访问。虽然也可以使用常规的DOM功能来访 间这些信息，但效韦是比较低的，如下面的例子所示：
var selectbox = document.forms(0]. elements["location"];
//不推苒
var tex匕=selectbox.options[〇KfirstChilc3.nodeValue;	//选巧的文本
var value = selectbox.options [0] .getAttribute ("value") ;	//选喷的值
以上代码使用标准DOM方法，取得了选择框中第一项的文本和值。可以与下面使用选项属性的代 码作一比较：
var selectbox = document.forms[0]. elements["locacion"];
//推荟
var text « selectbox.options[0] .text;	//选嚷的文本
var value = selectbox*options [01 .value;	"选項的值
在操作选项时，我们建议最好是使用特定于选项的属性，因为所有浏览器都支持这些属性。在将表 单控件作为DOM节点的情况下，实际的交互方式则会因浏览器而异。我们不推荐使用标准DOM技术 修改opt ionJE素的文本或者值。
最后，我们还想提醒读者注意一点：选择框的change事件与其他表单字段的change事件触发的 条件不一样。其他表单字段的change事件是在值被修改且焦点离开当前字段时触发，而选择框的 change亊件只要选中了选项就会触发。
$ 不同浏览器下f选项的value属性返回什么值也存在差别。但是，在所有浏览 ^器中，value属性始终等于value持性。在未指定value特性的情况下，IE8会返 回空字符串，而IE9+、Safari、Firefox、Chrome和Opera则会返田与text特性相同 的值。
14.3.1选择选项
对于只允许选择一项的选择框，访问选中项的最简单方式，就是使用选择框的selectedlmiex届 性，如下面的例子所示：
var selectedOption = seleccbox.options[selectbox.seleccedlndex];
取得选中项之后，可以像下面这样显示该选项的信息：
var seloctedlndex = selectbox.selectedlndex;
var selectedOption = selectbox.options[selectedlndex);
alert("Selected index： • + seleccedlndex + "\nSelected text： " +
selectedOption.text + "\nSclocted value： " f selectedOption.value);
SelectboxExampleOl.htm

14.3选择框脚本 433
这里，我们通过一个瞥告框显示f选中项的索引、文本和值。
对于可以选择多项的选择框，selectedfindex属性就好像只允许选择一项一样。设置 selectedindex会导致取消以前的所有选项并选择指定的那一项，而读取selectedindex则只会返 回选中项中第一项的索引值。
另一种选择选项的方式，就是取得对某一项的引用，然后将其selected属性设置为true。例如， 下面的代码会选屮选择框中的第一项：
selectbox.options[0].selected = true;
与selectedlndex不同，在允许多选的选择框中设置选项的selected属性，不会取消对其他选中项 的选择，因而可以动态选中任意多个项。但是，如果是在单选选择框中，修改某个选项的selected属性则 会取消对其他选项的选择。需要注意的是，将selected属性设置为false对单选选择框没有影响。
实际上，selected属性的作用主要是确定用户选择了选择框中的哪一项。要取得所冇选中的项， 可以循环遍历选项集合，然后测试每个选项的selected M性。来看下面的例子。
function getSelectedOptions(selectbox){ var result = new Array(); var option = null;
for (var i=0, len=selectbox.options.length; i  len; i++){ option = selectbox.options[i]? if (option.selected){ result.push(option);
return result;
SelectboxExample03. htm
这个函数可以返回给定选择框中选中项的-个数组。首先，创建一个将包含选中项的数组，然后使 用for循环迭代所有选项，同时检测每一项的selected属性。如果有选项被选中，则将其添加到 result数组中。最后，返回包含选中项的数组。下面是一个使用getSelectedOptions (}函数取得 选中项的示例。



var selectbox = document.getElementById("selLocation*); var selectedOptions = getSelectedOpcior.s (selectbox); var message = H";
for (var i=0, len=selectedOptions.length? i  len; i++){
message "Selected index： • + selectedOptions|i}•index + •\nSelected text: • + selectedOptions[i].text +
■\nSelected value: " + selectedOptionsfil.value + "\n\n"?

alert(message)?
SeiectboxExample03.htm
在这个例子中，我们首先从一个选择框中取得了选中项。然后，使用for循环构建了一条消息，包 含所有选中项的信息：每一项的索引、文本和值。这种技术适用于单选和多选选择框。
14

434 第14章表单脚本
14.3.2添加选项
可以使用JavaScript动态创建选项，并将它们添加到选择框中。添加选项的方式有很多，第一种方 式就是使用如下所示的DOM方法。
var newOption = document.createElement("option"); newOption.appendChild(document.createTextNode("Option text")); newOption.setAttribute{"value", "Option value");
soloctbox.appendCh ild(newOption);
SelectboxExamp 丨e04.htm
以h代码创建了一个新的option元素，然后为它添加了一个文本节点，并设肾其value特性， 最后将它添加到了选择框中。添加到选择框之后，用户立即就可以看到新选项=»
第一.种方式是使用Option构造函数来创建新选项，这个构造函数是DOM出现之前就有的，一 直遗留到现在。Option构造函数接受两个参数：文本（text)和值（va^ue);第二个参数可选3 虽然这个构造函数会创建一个Object的实例，但兼容DOM的浏览器会返回一个奶扣^於元索。 换句话说，在这种情况下，我们仍然可以使用appendChildO将新选项添加到选择框中。来看下面 的例子。
var newOption = new Option{"Option text*, "Option value"); selectbox.appendChi Id (newOption) ;	//在 IE8 及之前版本中有问題
SelectboxExample08.htm
这种方式在除IE之外的浏览器中都可以使用。由于存在bug, IE在这种方式下不能正确设置新选 项的文本。
第三种添加新选项的方式是使用选择框的add ()方法。DOM规定这个方法接受两个参数：要添加 的新选项和将位于新选项之后的选项。如果想在列表的最后添加-•个选项，应该将第二个参数设置为 mill。在丨£对add()方法的实现中，第二个参数是可选的，而R如果指定，该参数必须是新选项之后 选项的索引。兼容DOM的浏览器要求必须指定第二个参数，因此要想编写跨浏览器的代码，就不能只 传人一个参数。这时候，为第二个参数传人undefined,就可以在所有浏览器中都将新选项插人到列 表最后了。来宥个例子。
var newOption ^ new Option("Option text", "Option value"); selectbox. add (newOption, undefined) ; //最佳方案
SekctboxExample04.htm
在IE和兼容DOM的浏览器中，上面的代码都可以正常使用。如果你想将新选项添加到其他位置(不 是最后一个），就应该使用标准的DOM技术和insertBef ore )方法。
就和在HTML中一样，此时也不一定要为选项指定值。换句话说，只为option 构造函数传入一个参数（选项的文本）也没有问题。

14.3选择框脚本	435
14.3.3移除选项
Si添加选项类似，移除选项的方式也有很多种^ n先，可以使用DOM的removeChildU方法， 为Jt•传人要移除的选项，如下面的例子所示：
14
selectbox. removeChild (selectbox. opt ions [ 0 ]};	//移除第一个选嘴
其次，可以使用选择框的remove()方法。这个方法接受一个参数，即要移除选项的索引，如下面 的例子所示：
selectbox.remove (0);	//移除第一个选續
最后一种方式，就是将相应选项设置为null。这种方式也是DOM出现之前浏览器的遗留机制。 例如：
selectbox.optionsiOl = null;	//移除第一个选項
要清除选择框中所有的项，需要迭代所有选项并逐个移除它们，如下面的例子所示：
function clearSelectbox(selectbox}{
for(var i=0, len=selectbox.options.length; i  leu; i++){ selectbox.remove(i);
}
}
这个函数每次只移除选择框中的第一个选项。由于移除第一个选项后，所有后续选项都会自动向上 移动一个位s,因此承复移除第一个选项就可以移除所有选项了。
14.3.4移动和重排选项
在DOM标准出现之前，将--个选择框中的选项移动到另一个选择框中是非常麻烦的。整个过程要 涉及从第一个选择框中移除选项，然后以相同的文本和值创建新选项，最后再将新选项添加到第二个选 择框中。而使用DOM的aPPendChild(方法，就可以将第一个选择框中的选项直接移动到第二个选 择框中3我们知道，如果为appendChildO方法传人一个文朽中已有的元素，那么就会先从该元素的 父节点中移除它，再把它添加到指定的位置。下面的代码展示了将第一个选择框中的第一个选项移动到 第二个选择框中的过程。



var selectboxl = document.getElementById("selLocationsl");
var selectbox2 = document.getElementByldt*selLocations2");
selectbox2.appendChild(selectboxl.options[0]);
SelectboxExample05. htm
移动选项与移除选项有一个共同之处，即会重置每•个选项的index属性。
:®排选项次序的过程也十分类似，最好的方式仍然是使用DOM方法。要将选择框中的某一项移动 到特定位置，最合适的DOM方法就是insertBeforeO; appendChild()方法只适用于将选项添加 到选择框的最后。要在选择框中向前移动一个选项的位置，可以使用以下代码： var optionToMove = selectbox.options[11;
selectbox.insertBefore(optionToMove, selectbox.options[optionToMove.index-11};
SelectboxExample06.htm

436 第14章表单脚本
以上代码首先选择了要移动的选项，然后将其插人到r排在它前面的选项之前。实际上，第二行代 码对除第•个选项之外的其他选项是通用的。类似地，可以使用下列代码将选择框中的选项向后移动一 个位置。
var optionToMove = selectbox.options[1J;
selectbox.insertBefore(optionToMove, selectbox.options[optionToMove,index+2】;
SelectboxExample06.htm
以上代码适用于选择框中的所有选项，包括最后一个选项。



IE7存在一个页面重给问題，有时候会导致使用DOM方法重排的选项不能冯上
正确显示。
14.4表单序列化
随着Ajax的出现，表单序列化已经成为一种常见需求（第21章将讨论Ajax )。在JavaScript中，可 以利用表单字段的type属性，连同name和value属性一起实现对表单的序列化。在编写代码之前， 冇必须先搞清楚在表单提交期间，浏览器是怎样将数据发送给服务器的。
- [ ] 对表单字段的名称和值进行URL编码，使用和号（&)分隔。 a不发送禁用的表单字段。
- [ ] 只发送勾选的复选框和单选按钮。
- [ ] 不发送type为"reset••和"button-的按钮。
- [ ] 多选选择框中的每个选中的值準独一个条U。
- [ ] 在单击提交按钮提交表平的情况下，也会发送提交按钮;否则，不发送提交按钮。也包括type 为M image"的 input  元素。
- [ ] 〈select:^素的值，就是选屮的optionJE：素的value特性的值。如果coption;^素没有 value特性，则是optionX索的文本值。
在表单序列化过程中，一般不包含任何按钮字段，因为结果字符串很可能是通过其他方式提交的。 除此之外的其他h述规则都应该遵循。以下就是实现表单序列化的代码。
function serialize{form){ var parts =[], field = null, iy
len,
j.
optLen,
option,
optValue;
for (i=0, len=form.elements.length; i  len; i++{ field s form.elements[i];
switch(field.type){ case "select-one-： case "select-multiple-:
if (field.name.length){
for (j=s〇# optLen = 6ield.options.length; j  optLen? j++) {

14.4表单序列化	437
option = field.optionsfj]; if {option.selected){ optValue = •■; if {option.hasAttribute){
optValue = {option.hasAttribute(•value-) ?
option.value ： option.text)?
)else {
optValue = (option.attributesf"value*J.specified ? option.value ： option.text);
}
parts.push(encodeURIComponent(field.name) + _=• + encodeURIComponent(optValue));
break;
case undefined:
case "file*:
case "submit"：
case *reset":
case •button’：
break?
//字段集 //文件搶入 //提交按相 //重置按4a //6定义^te
case "radio"：	// 单选接相
case "checkbox*:	// 复选框
if t!field.checked){ break;
)
/*执行跃认操作*/
default:
//不包含没有名字的表单字段 if (field.name.length){
parts.push(encodeURIComponent(field.name) +	+
encodeURIComponent(field.value))?
return parts.join{"&■);
FormSerializationExampleOL him
上面这个serialize ()函数首先定义了一个名为parts的数组，用于保存将要创建的字符串的各 个部分。然后，通过for循环迭代每个表单字段，并将其保存在field变量中。在获得了一个字段的 引用之后，使用switch语句检测其type属性。序列化过程中最麻烦的就是361扣(:元素，它可能 是单选框也可能是多选框。为此，需要遍历控件中的每一个选项，并在相应选项被选中的情况下向数组 中添加一个值。对于单选框，只可能有-个选中项，而多选框则可能有零或多个选中项。这里的代码适 用丁这两种选择框，至于可选项的数童则是由浏览器控制的。在找到一个选中项之后，需要确定使用什 么值。如果不存在value特性，或者虽然存在该特性，但值为空字符串，都要使用选项的文本来代替。 为检杳这个特性，在DOM兼容的浏览器中需要使用hasAttribute ()方法，而在IE中需要使用特性 的 specif ied 属性。
如果表单中包含^61;^|:-元素，则该元素会出现在元素集合中，但没有type属性。因此，如果type 属性未定义，则不需要对其进行序列化。同样，对于各种按钮以及文件输人字段也是如此（文件输人宇段在 表单提交过程中包含文件的内容;但是，这个字段是无法模仿的，序列化时一般都要忽略)。对于单选按钮 和复选框，要检査其checked属性是否被设置为false,如果是则退出switch语句。如果checked属性
14

438 第14章表单脚本
为true,则继续执行default语句，即将当前字段的名称和值进行编码，然后添加到parts数组中。函数 的ft后-步，就是使用join()格式化整个宁符串，也就是用和号来分隔毎个&单字段。
最后，serialize)函数会以丧询字符串的格式输出序列化之后的7符串。当然，要序列化成其他 格式，也不是什么困难的事。
14.5富文本编辑
富文本编辑，乂称为WYSIWYG ( What You See Is What You Get,所见即所得)。在网页中编辑富 文本内容，是人们对Web皮用程序最大的期待之一。虽然也没有规范，仴在ffi最年引入的这一功能基 础上，已较出现了事实标准。而IX, Opera、Safari、Chrome和Firefox都已经支持这一功能。这一技术 的本质，就是在页面中嵌人一个包含空HTML页面的iErame。通过设置designMode属性，这个空白 的HTML页面可以被编辑，而编辑对象则是该页面body兀素的HTML代码。designMode属性有两 个口J能的值："ofE» (默认值）和-on-。在设置为"on"时，整个文档都会变得可以编辑（显示插人符 号)，然后就可以像使用字处理软件一样，通过键盘将文本内容加粗、变成斜体，等等。
可以给iframe指定一个非常简单的HTML页面作为其内容来源。例如：
!DOCTYPE hcml
head
ticleBlank Page for Rich Text Editing/title /head
body
/body
/htir.l
这个页面在iframe中可以像其他页而一样被加载。要让它可以编辑，必须要将designMode设置 为仞只有在贞面完全加载之后才能设置这个属性。因此，在包含页面中，需要使用onload事件 处理程序来在恰当的时刻设S designMode,如下面的例了•所示：
iframe n«i^e="richedif style="height ： lOOpx?width： lOOpx? * src=*blank.htm*/iframe script type="text/javascript"
EventUtil-addKandler(window, "load"( function〇( frames["richediC].document.designMode = "on*;
});
/script
等到以上代码执行之后，你就会在页面中看到一个类似文本框的可编辑K字段。这个区宇段具有与 其他网页相同的默认样式;不过，通过为空[4页面戍用CSS样式，可以修改可编辑区字段的外观。
使用 contenteditable 属性
另•种编辑富文本内容的方式是使用名为contenteditable的特殊属性，这个属性也是由IE最 苹实现的。可以把contenteditable属性应用给页面中的任何元索，然后用户立即就可以编辑该元素。 这种方法之所以受到欢迎，是因为它不需要iframe、空內页和JavaScript,只要为元素设置 contenteditable M性即 〇J。
div class="editable" id="richeditB contenteditablax/div

14.5富文本编辑	439
这样，元素中包含的任何文本内容就都可以编辑广，就好像这个元素变成rtextarea元索一样。 通过在这个元素上设置contenteditable属性，也能打开或关闭编辑模式。
var div = document.getElementById("richedif) ? richedit.contentEditable = "true";
contenteditable 属性有三个可能的值：wtrue"表/K打开、Mfalsew：fcK关闭，"inherit"表/K 从父元索那里继承（因为可以在contenteditable元尜中创建或删除元索)。支持contenteditable 属性的元素有丨E、Firefox、Chrome、Safari和Opera。在移动设备上，支持contenteditable屈性的 浏览器有iOS 5+中的Safari和Android 3+中的WebKit。
14.5.2操作亩文本
与富文本编辑器交互的主要方式，就是使用document.execCommand()。这个方法可以对文档执 行预定义的命令，而且可以应用大多数格式。可以为document.execCommandU方法传递3个参数： 要执行的命令名称、表示浏览器是否应该为当前命令提供用户界面的-个布尔值和执行命令必须的一个 值（如果不需要值，则传递mill)。为了确保跨浏览器的兼容性，第二个参数应该始终设置为false, 因为Firefox会在该参数为true时抛出错误。
不同浏览器支持的预定义命令也不一样。下表列出了那些被支持最多的命令。

440 第14章表单脚本
其中，与剪貼板有关的命令在不同浏览器屮的差异极大。Opera根本没有实现任何剪贴板命令，而 Firefox在默认情况F会禁用它们（必须修改用户的首选项来启用它们)。Safari和Chrome实现了 cut和 copy,但没有实现paste。不过，即使不能通过docuuient.execConwnandO来执行这些命令，但却可 以通过相应的快捷键来实现同样的操作。
可以在任何时候使用这些命令来修改富文本K域的外观，如下面的例子所示。
//转换权体丈本
frames ["richedit" ] .documenc.execCommand("bold,', false, null);
//转換斜体文本
frames("richedit"].document.execConunand("italic", false, null);
//创建指向www.wrox.com的链接
frames["richedit"J.document.execCommand{"createlink*, false,
"

http://www.wrox.com");
//格式化为1级标題
frames[•richedit"].docviment.execCommand("formatblock", false, "hl*);
Rich TextEditingExampleOl, htm
同样的方法也适用于页面中contenteditable属性为"true"的K块，只要把对框架的引用替换 成当前窗口的document:对象即町。
//转换扭体文本
document»execCommand"bold", false, null);
//转换斜体文本
document.execCommand("italic", false, null);
//创建指向www.wrox.com的链接
document. execCortunand ("create 1 ink", false,
"http： //www.wrox.corn");
//格式化为1级标题
document.execCommand(■formatblock", false, "hl");
Rich TextEditingExampleOl. htm
需要注意的是，虽然所有浏览器都支持这些命令，但这些命令所产生的HTML仍然有很大不同。 例如，执行bold命令时，IE和Opera会使用strong标签包闱文本，Safari和Chrome使用b标签，

14.5 富文本编辑	441
而Firefox则使用span标签。由于各个浏览器实现命令的方式不同，加上它们通过innerHTML实现 转换的方式也不一样，因此不能指望富文本编辑器会产生-致的HTML。
除了命令之外，还有一些与命令相关的方法。第一^方法就是0[11611：〇1«1131116113)3161()，可以用它来检 测是否可以针对当前选择的文本，或者当麵人字符所在位置撕谋个命令。这个方法接收一即要 检测的命令。果当前编辑K域允许执行传人的命令，这个方法返回true,否则返回false。例如：
var result = frames["richedit"].document.queryCommandEnabled("bold");
如果能够对?*1前选择的文本执行_bold"命令，以上代码会返N true。需要注意的是，guery- CommandEMbledO方法返回true,并不意味着实际上就可以执行相应命令，而只能说明对当前选择 的文本执行相应命令是杏合适。例如，Firefox在默认情况下会禁用剪切操作，但执行queryc0^lmar«i- Enabled( "cut")也可能会返冋 true。
另外，queryCommandStateO方法用于确定是否已将指定命令应用到了选择的文本。例如，要确 定当前选择的文本是否已经转换成了粗体，可以使用如下代码。
var isBold = frames [■ richedit •] .document .queryCommandS.tate(-bold*);
Rich TextEditingExample01.htm
如果此前已经对选择的文本执行了”bold-命令，那么上面的代码会返冋true。一些功能全面的富 文本编辑器，正是利用这个方法来更新粗体、斜体等按钮的状态的。
最后一个方法是queryCommandValue(,用于取得执行命令时传人的值（即前面例子中传给 document .execCommanci (的第三个参数)。例如，在对一段文本应用"fontsize"命令时如果传人了 7,那么下面的代码就会返回"7":
var fontSize = frames["richedit"].document.queryConunandValue("fontsize");
RichTextEditingExampleOI.htm
通过这个方法可以确定某个命令是怎样应用到选择的文本的，可以据以确定再对其应用后续命令是 否合适。
14.5.3富文本选区
在富文本编辑器中，使用框架（ifran\e)的getSelection(方法，可以确定实际选择的文本。 这个方法是window对象和document对象的属性，调用它会返回一个表示当前选择文本的Selection 对象。每个Selection对象都有下列属性〇
anchorNode:选区起点所在的节点。
anchorOffset:在到达选区起点位ft之前跳过的anchorNode中的字符数量。
focusNode:选区终点所在的节点。
focusOffset: focusNode中包含在选区之内的字符数贵。
口 isCoUapsed:布尔值，表示选区的起点和终点是否重合。
rangeCount:选区中包含的DOM范围的数敏。
Selection对象的这些属性并没有包含多少有用的信息。好在，该对象的下列方法提供了更多信 息，并且支持对选区的操作。
14

442 第14章表单脚本
addRange(range):将指定的DOM范顶添加到选K中。
col Lapse (node, offset):将选区折魯到指定节点中的相应的文本偏移位置。
coliapseToEndU :将选区折香到终点位S〇
col]apseToStart():将选区折叠到起点位置。
口 containsNode (node):确定指定的节点是否包含在选K中c
deLeteFromDocument:():从文档中删除选区中的文本，与document .execCommand( delete", false, null}命令的结果相同。
extend(node, offset):通过将focusNode和focusOf fset移动到指定的值来扩展选区。
getRangeAt (index):返[nl索引对应的选区中的DOM范围。
 removeAllR抓ges():从选K中移除所有DOM范闱。实际上，这样会移除选K,因为选K中 至少要有一个范围。
reomveRange (range):从选区中移除指定的DOM范
selectAllChildren(node):清除选K并选择指定节点的所有子节点。
toStringO:返M选K所包含的文本内容。
Selection对象的这些方法都极为实用，它们利用了（第12章讨论过的）DOM范围来管理选K。 由于可以直接操作选择文本的DOM表现，W此访问DOM范围与使用execCommandO相比，能够对富 文本编辑器进行更加细化的控制。下面来ft—个例子。
var selection = frames["richedit").getSelection();
//取得选择的文本
var selectedText = selection.toString(;
//取得代表选区的范围
var range = selection.getRangeAt(0);
//突出昱示选择的文本
var span = frames("richedic"].document.croateEloment(*span"); span.style.backgroundColor = "yellow"; range.surroundConteats(span);
RichTextEditingExampieOJ.htm
以上代码会为富文本编辑器中被选择的文本添加黄色的背景。这里使用了默认选区中的DOM范围, 通过surroundContents (}方法将选区添加到丫带有黄色背景的3口311元素中。
HTML5将 getSelection()方法纳人了标准，而且 1E9、Firefox、Safari、Chrome和 0pera8都实 现f它。由于历史原因，在Firefox3.6+中调用document.getSelection()会返间一个字符串。为此， 可以在 Firefox3.6+中改作调用 window.getSelectionU ,从而返回 selection 对象。Firefox8修复 了 document.getSelection()的 bug,能返回与 window*getSelection()相同的值。
1E8及更早的版本不支持DOM范围，伉我们町以通过它支持的selection对象操作选择的文本。 IE中的selection对象是document的厲性，本章前面曾经讨论过。要取得富文本编辑器中选择的文 本，首先必须创建*个文本范围（请参考第12章中的相关内容），然后再像下面这样访问其text属性。
var raige = frames["richedit-].document.selection.createRange(); var selectedText = range.text;

14.6 小结 443
虽然使用1E的文木范围来执行HTML操作并不像使用DOM范围那么可靠，但也不失为一种有效 的途径。要像前面使用DOM范围那样实现相同的文本高亮效果，nj以组合使用htmlText属性和 pasteHTMIi ( 方法〇
var range = frames【*richedit** 1 .document. selection.creatcRange (}; range.pasteHTML(-span style=\"background-color:yellow\" _ + range.htmlText + •/span");
以上代码通过htmlText取得r当前选K中的HTML,然后将其放在r一对spam•标签中，最后 乂使用pasteHTKL (将结果重新插入到了选区中。
14.5.4表单与富文本
由于富文本编辑是使用iframe而非表笮控件实现的，因此从技术上说，富文本编辑器并不属了-表 单。换句话说，富文本编辑器中的HTML不会被自动提交给服务器，而需要我们手工来提取并提交 HTML。为此，通常可以添加一个隐藏的表单字段，让它的值等于从iframe中提取出的HTML。具体 来说，就是在提交表单之前，从iframe中提取出HTML,并将其插人到隐藏的字段中。下面就是通过 表单的onsubmit事件处理程序实现上述操作的代码。



EventUti1.addHandler(form, "submit", function(event){ event = EventUtil.getEvent(event); var target - EventUtil.getTarget(event);
target.elements[•comments*].value = frames t"richedit"].document.body.innerHTML;
});
RichTextEditingExampleOl .htm
在此，我们通过文拷主体的innerHTML屈性取得了 iframe中的HTML,然后将其插人到名为 •comments"的表单字段中。这样可以确保恰好在提交表单之前填充"comments•字段。如果你想在代 奶中通过submit ()来手工提交表单，那么一定不要忘记事先执行上面的操作。对于contenteditable 元素，也可以执行类似操作。
EventUtil.addKandLer{form, "submit", function(event){ event = EventUtil.getEvent(event); var target = EventUtil.getTarget(event);
tarsretuleaentst^cosmenta*1】 .value ■
docuaent.getElementByld("richedit*)«izmerRTHL;
i);
###  14.6 小结
虽然HTML和Web应用自诞生以来已经发生了天翻地覆的变化，但Web表单相对却没有什么改 变。使用JavaScript可以增强已有的表单字段，从而创造出新的功能，或者提升表单的易用件。为此, 表单、表单字段都引人f相应的M性和方法，以便JavaScript使用。下面是本章介绍的儿个概念。 
- [ ] 可以使用一些标准或非标准的方法选择文本框中的全部或部分文本。
- [ ] 大多数浏览器都采用了 Firefox操作选择文本的方式，但IE仍然坚持己的实现。
- [ ] 在文本框的内容变化时，可以通过侦听键盘事件以及检测插人的字符，来允许或禁止用户输人 某些字符。
除Opera之外的所有浏览器都支持剪贴板亊件，包括copy、cut和paste。其他浏览器在实现剪 贴板事件时也可以分为几种不同的情况。
- [ ] IE、Hrefox、Chrome和Safari允许通过JavaScript访问剪贴板中的数据，而Opera不允许这种访 问方式。
- [ ] 即使是IE、Chrome和Safari,它们各ft的实现方式也不相同。
- [ ] Firefox、Safari和Chrome只允许在paste事件发生时读取剪貼板数据，而IE没有这个限制。
- [ ] Firefox、Safari和Chrome只允许在发生剪贴板事件时访问与剪貼板相关的信息，而IE允许在任 何时候访问相关信息。
在文本框内容必须限制为某些特定字符的情况下，就可以利用剪贴板事件来屏蔽通过粘贴向文本框 中插人内容的操作。
选择框也是经常要通过JavaScript来控制的一个表单宇段。由于有了 DOM,对选择框的操作比以前 要方便多了。添加选项、移除选项、将选项从一个选择框移动到另一个选择框，甚至对选项进行排序等 操作，都可以使用标准的DOM技术来实现。
富文本编辑功能是通过一个包含空HTML文档的iframe元素来实现的。通过将空文档的 designMode属性设置为”on",就可以将该贞面转换为可编辑状态，此时其表现如同宇处理软件。另外， 也可以将某个元素设置为ccmtenteditable。在默认情况下，可以将字体加粗或者将文本转换为斜体， 还可以使用剪贴板。JavaScript通过使用execComman3()方法也可以实现相同的一些功能。另外，使用 gueryCommandEnabled (、queryCorranandState ( 和 queryConunandValue ()方法则可以取得有关 文本选区的信息。由于以这种方式构建的富文本编辑器并小'是一个表单字段，因此在将其内容提交给 服务器之前，必须将iframe或contenteditable兀素中的HTML复制到一个表单字段中。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter13.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter15.md)