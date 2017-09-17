# 第 2 章
# 20
GitBook allows you to organize your book into chapters, each chapter is stored in a separate file like this one.

## 21

---

## 22

---

## 23

---

## 24


---
## 25


---
## 26

以K足兄W站h的一个品牌列表的展示效果，用户进入该页面时，品牌列表默认是精简 K示的（即不完整的品牌列表），如图2-5所示。
用户可以单卅商品列表下方的“站示全部品牌”按钮来敁示全部的品牌。
革击“敁示令部品牌”按钮的同时，列表会将推荐的品牌的名字高亮显示，按钮ift的文 字也换成了“精简M示品牌”，如图2-6所示。
阁2-5品牌展示列表（粘简）	阁2-6品牌展示列表（全部)
再次单击“粘简敁示品牌”按钮，即可问到图2-5所示的页面。
为了实现这个例子，酋先耑要设计它的HTML结构。HTML代码如下:
```html
<div class="SubCategoryBox">
<ul>
<li><a href=n#">佳能</a><i>(30440)</i></li>
<li><a hrref="#">索尼</a><i><27220)</i></li>
<li><a href="#">三星</a><i>(20808)</i></li>
<li><a href=n#">尼康</a><i>(17821)</i></li>
<li><a href=n#">松下</a><i>(12289)</i></li>
<li><a href="#">卡西欧</a><i>(8242)</i></li>
<li><a href="#">富士</a>（14894)</i><li>
<li><a href="#">柯达</a><i>(9520)</i></li>
<li><a href="#n>宾得</a><i>(2195)</i></li>
<li><a href="#">理光</a><i>(4114} </i></li>
<li><a href="#">奥林巴斯</a><i>(12205>
</i></li> <li><a href ="#">明基</a><i><1466》</i></li>
<li><a href="#">爱国者</a><i> (3091)</i>
</li> <li><a href="#">其他品牌相机</a><i>(7275) </i></li>
</ul>
<div class=Hshowmoren>
<a href="more.ht:ml"><span>显示全部品牌</span></a>
</div>
</div>
```
![images](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/2/2.6.gif "显示与隐藏")

然后为上面的HTML代码添加CSS样式  
页面初始化的效果如图2-7所示。  
接下来为这个页面添加一些交互效果，  
要做的工作有以下儿项。  
2.从第7条开始隐藏后面的品牌（最  
后一条“其它品牌相机”除外）。  
3.当用户单击“显示伞部品牌”按钮  
时，将执行以下操作。  
1.跶示隐藏的品牌。  
2.“显示全部品牌”按钮文本切换成“粘•简显示品牌”。  
3.高亮推荐品牌。    
4.当用户单击“精简敁示品牌”按钮时，将执行以下操作。  
(1)从第5条开始隐藏后面的品牌（最后一条“其它品牌相机”除外)。  
(2)“精简显示品牌”按钮文本切换成“显示全部品牌”。
(3)去掉高亮显示的推荐品牌。
5.循环进行第（2)步和第（3)步。
下面逐步来完成以上的效果。
(1 )从第5条开始隐藏后面的品牌（圾后一条“其他品牌相机”除外）。

(ilfe 〇〇^«»
ami) a_> ISA mi)
爱撕"
07330) WF 02369) ft 达
S 林 Gfl /MOW
^AWm.(n?s>
0B示全
图2-7品牌展示列表（褚简）
三S
卡西
^0469)

```javascript
var ^category = S('ul li:gt(5):not(:last)1);
Scategory.hide () ;	//隐藏上面获取到的jQuery对象
```
SOI li:gt(5):nm(:last)')的意思是先获取<11丨>元素下索引值大丁• 5的<丨丨>元素的集合元素，
然后去掉集合元素中的最后一个元素。这样，即可将从第7条开始至倒数第2条的所有品牌
都获取到。
最后通过hide〇方法隐藏这些元素。
1.当用户单击“敁示全部品牌”按钮时，执行以下操作。
首先获取到按钮，代码如下
```
var StoggleBtn = $('div.showmore > a1)；	//获取“显示全部品牌”按钮
```
然后给按钮添加珙件，使用show()方法把隐藏的品牌列表M示出来，代码如下：
```
StoggleBtn.click(function (){
Scategory.show();
return false;
})；
```
由T给超链接添加one丨ick祺件，因此需要使用“etum false”语句让浏览器认为用户没
有单击该超链接，从而阻止该超链接跳转。
之后，需要将“显示伞部品牌”按钮文本切换成“精简显示品牌”，代码如下：
```
S{'.showmore a span1)
css("background’、"url(img/up.gif) no-repeat 0 0")
text ("精简显示品牌;	//这里使用了链式操作
```
这里完成了两步操作，即把按钮的背景图片换成向上的阁片，同时也改变了按钮文本内
容，将其替换成“粘:简显示品牌”。
接下来耑要高亮推荐品牌，代码如下：
```
S (’ul li ’ > • filter (’’ ：contains (丨佳能 ’ >,:contains 尼康’ > ^contains (•奥林巴
斯，> ">
• addClass ("promoted");;	//添加高亮样式
```
使用filM)方法筛选出符合要求的品牌，然后为它们添加promoted样式。在这里推荐了
3个品，即牌佳能、尼谈和奥林巴斯。
此时，完成的jQuery代码如下：
```
$(function () {	//等待DOM加载完毕
var Scategory = $('ul li:gt (51) :not (:last) 1);
//获得索引值大于5的品牌集集合对象（除最后一条）
//显示全部品牌 //超链接不跳转
Scategory.hide ();	//隐藏上面获取到的jQuery对象
var StoggleBtn = $ ('div, showmore > a1)；	//获取“显示全部品牌”按钮
StoggleBtn.click(function(){
Scategory.show ();	//显示^category
$(1 .showmore a span”
.css ("background", Murl (img/up.gif) no-repeat 0 0,r)
.text r精简显示品牌"）；	"改变背景图片和文本
$ (1 ul lifilter (" :contains (1 佳能:contains 尼康 1) , :contains (* 奥林巴斯'> n)
.addClass ("promoted11);	//添加高亮样式
return	false;	//超链接不跳转
})
})
```
运行上面的代码，单击“敁示全部品牌”按钮后，显示图2-8所示的效果，此时已经能 够正常敁示全部品牌了。
卫*1^鸿中用到尚九个jQuery方洼葯意恿如卞。
(1)show():显示隐藏的匹配元素。
(2)css(name，value):给元素设置样式.
^注意	■ text(string):设置所有匹配元素的文本内容.
(3)fiUer(Cxpr):筛选出与指定表达式匹配的元素集合，其中expr可以是多个选择器
的组合.
	■ addClass(class): 为匹配的元素添加指定的类名。	
2.当用户单击“精简显示品牌”按钮时，将执行以下操作。
由丁户单市的足同一个按钮，因此車件仍然是在刚才的按钮元素上。要将切换两种状
态的效果在一个按钮上进行，可以通过判断元素的显示或者隐藏来达到U的，代码结构如下:
if《元素显示> {
//元素隐藏①
}else{ •
//元素显示②


jQuery选择器
		—_				
代码②就足第（2)步的内容，接下来只需要完成代码①的内容即可。
在jQuery中，与show()方法相反的适hide()方法，因此可以使用hide()方法将品牌隐藏 起来，代码如卜‘：
Scategory.hide () ;	//隐藏Scategory
然后将“精简显示品牌”按钮文本切换成“显示全部品牌”，同时按钮图片换成向下的 图片，这一步与前面类似，只不过是阁片路径和文本内容不N而已，代码如下：
S('.showmore a span')
.css("background","url(img/down.gif) no-repeat 0 0")
.text (”显示全部品牌//改变背景图片和文本
接下来耑要去掉所柯品牌的高亮显示状态，此时可以使用removeClassO方法来完成，代 码如下：
S(Tul li') .removeClass ("promoted");	//去掉高亮样式
它将去掉所有<1>元素上的“promoted”样式，即去掉了品牌的高亮状态。
removeClass(c丨ass)的功能和 addClass(class)的功能正好相反 e addClass(class)的功 ^注意能是为匹配的元‘添加指定的类，而removeCiass(class)则是从匹配的元素中删除
魅的^	—	 __		
至此完成代码①。
扱后通过判断元素足否显示来分别执行代码①和代码②，代码如下：
```javascript
if (^category判断.is (" :visible"M {	//如果元素显示，则执行对应的代码
之后即可将代码①和代码②插入相应的位置。jQuery代码如下：
if (Scategory. is : visible") > {	//如果元素显示
Scategory .hide () ;	//隐藏Scategory
$ {'.showmore a span1)
•css("background","url(img/down.gif> no-repeat 0 0")
.text ("显示全部品牌//改变背景图片和文本 'ul li1 > .removeClass ("promoted1') ;//去掉高亮样式
}else{
Scategory. show () ;	//显示$category
$('.showmore a span*)
.css("background","url(img/up.gif) no-repeat 0 0")
.text ("精简显示品牌//改变背景图片和文本 $<’ ul li 1 )• filter (" :contains ('佳能：contains (*尼康 ’），：contains (*
奥林巴斯*)")
• addClass ("promoted");	//添加髙亮样式
}
```
至此任务完成，完整的jQuery代码如下：
```javascript
$ (function {	//等待 DOM 加载完毕.
var Scategory = $('ul li:gt(5):not{:last)1);
//获得索引值大于5的品牌集合对象（除最后一条> Scategory .hide ();	//隐藏上面获取到的jQuery对象
var StoggleBtn = $(’div.showmore > a’>; //获取“显示全部品牌”按纽 StoggleBtn.click(function(> {	//给按钮添加 onclick 事件
if (Scategory.is (" :visible")){//如果元素显示 Scategory. hide ();	//隐藏Scategory
$'(.showmore a span1)
.css("background","url(img/down.gif)norepeat 0 0")
.text("显示全部品牌"）；//改变背景图片和文本 $ (1 ul li ’）• removeClass 丨promoted">; //去掉高亮样式
}else{
repeat 0 0")
尼康M , :contains (*奥林巴斯1 > "
}
return false
})
})
```
运行代码后，单击按钮，品牌列表会在“全部”和“精简”两种效果之间循环切换，显
示效果如图2-9和图2-10所示。
Scategory.show() ;	//显示Scategory
$(' .showmore a span”
•css("background”，"url(img/up.gif>no-
.text <"精简显示品牌">;//改变背景图片和文本 $( *ul li •>. filter <■’ ：contains (1佳能	:contains ( !
• addClass ("promoted”）；//添加高亮样式 //超链接不跳转
► 58

«< <<<<<第j章jQuery选择器
fIR 〇〇44〇>	^ 〇722〇)	三5 oceoe)
ami)	wp 〇2289)	卡西 k 败切
算它岛嫌相机<7?W
@5示全部品牌
阁2-9粘简模式
阁2-丨0全部模式
在jQuery屮有一个方法更适合上面的情况，它能给一个按钮添加一组交互琪件，而不需 要像丨:例一样去判断，上例的代码如下：
toggleBtn.click(function (){
if (Scategory. is (" : visible")) {	//如果元素显示
//元素隐藏	代码①
}else{
//元素显示	代码@	•
)
})
如采改成togglc()方法，代码则可以直接写成以下形式：
StoggleBtn. toggle (function () {	//toggle ()方法用来交替一组动作
//显示元素	代码③
},function(){
//隐藏元素	代码④
})
当单击按钮后，脚本会对代码③和代码④进行交替处理。
jQuery还提供了很多简单易用的方法，上面讲解的toggleO方法只是其中的一种，这些 方法将在后面的章节屮进行详细介绍。
—垚本例中7如東角户禁用了 JavaScripr的功能，品牌列表仍叙函¥完"¥1示，-用户 单击“显示全部品牌”按钮的时候，会跳转到more.html页面来显示品牌列表•作为 ^注意一名专业的开发者，必须要考虑到禁用或者不支持JavaScript的浏览器（用户代理）• 另外，这点对于搜索引擎优化也特别有帮助，毕竟当前的搜索引擎爬虫基本都不支 持 JavaScript.

---

## 27

---

## 28

---

选抒器足jQuery的根接，在jQuery中，对事件处理、遍历DOM和Ajax操作都依 赖于选杼器。如采能熟练地使用选杼器，不仅能简化代码，而且可以达到事半功倍的 效果。

### 21

#### 1.CSS选择器  
在开始学习jQuery选择器之前，有必要简单了解前儿年流行起来的CSS (Cascading Style Sheets，层脅样式表）技术。  
CSS适一项出色的技术，它使得网页的结构和表现样式完全分离。利用CSS选择器能轻 松地对某个元素添加样式而不改动HTML结构，只需通过添加不同的CSS规则，就可以得 到各种不同样式的M页。  
耍使菜个样式应用于特定的HTML元素，首先需要找到该元素。在CSS中，执行这一 任务的表现规则称为CSS选杼器。学会使用CSS选择器是学习CSS的基础，它为在获取目 标元素之后施加样式提供了极大的灵活性。常用的CSS选择器分类如表2-1所示。  

|---|---|---|---|
|选择器|语法|描述|示例|
|选择器|语法|描述|示例|
|---|---|---|---|


儿乎所W主流浏览器都支持L:面这些常川的选择器。此外CSS屮还有伪类选杼器 (E:Pseudo-Elements{ CssRules })、子选择器(E > F{ CssRules })、临近选杼器(E + F {CssRules})和属性选杼器(E[attr] {CssRules})等。但遗憾的是，主流浏览器并非完全支持 所有的CSS选择器。


更加详细的介绍可以参考 

http://www.w3.org/TR/CSS2/selector.html 网址。
了解这些相关知识后，來#一个有关CSS类选择器的简单例子，代码如下：

<p style=,'color: red; font-size: 30px; ">CSS Demo</p>

l：面代码的意思是将<p>元素里的文本颜色设背为红色，字体大小设置为30px。
像L面这样把CSS代码和HTML代码混杂在一起的做法足非常不妥的，它并不符合表 现和内容相分离的设计原则，因此建议使用下面的方法，代码如下：

.demo{	//给class为demo的元素添加样式
color:red; font-size:30px;
}


<p class=ndemo">CSS Demo.</p>


先把样式写在<style>标签里，然后用class M性将元素和样式联系起來，class作为连接 样式和网页结构的纽带。这样的写法不仅容易理解和阅读，而且当需要改变一些样式的时 候，只要^<style>#签里改变相关的样式即可。
例如耍使所有class为demo的<p>元素里的字体加粗，可以直接在<style>里编写，而不 需要去网页串.寻找所柯class为demo的<p>元素再逐个添加样式，代码如下：

.demo{	//给class为demo的元素添加样式
color:red; font-size:30px; font-weight: bold;	/ / 字体加粗
)


<p class=ndemo">CSS Demo.</p>

H 把CSS应用到网页中有3种方式，即行间样式表、内部样式表和外部样式表。

先把样式写在<style>标签里，然后用class M性将元素和样式联系起來，class作为连接 样式和网页结构的纽带。这样的写法不仅容易理解和阅读，而且当需要改变一些样式的时 候，只要^<style>#签里改变相关的样式即可。
例如耍使所有class为demo的<p>元素里的字体加粗，可以直接在<style>里编写，而不 需要去网页串.寻找所柯class为demo的<p>元素再逐个添加样式，代码如下：

.demo{	//给class为demo的元素添加样式
color:red; font-size:30px; font-weight: bold;	/ / 字体加粗
)

<p class=ndemo">CSS Demo.</p>
H 把CSS应用到网页中有3种方式，即行间样式表、内部样式表和外部样式表。上例 J/±M	中使用的是内部样式表，内部样式表的缺点是不能被多个页面重复使用的.	
2.jQuery选择器
jQuery中的选择器完全继承了 CSS的风格。利用jQuery选杼器，可以非常便捷和快速 地找出特定的DOM元素，然后为它们添加相应的行为，而无需担心浏览器是否支持这一选 杼器。学会使用选杼器足学jQuery的基础，jQuery的行为规则都必须在获取到元素后方能 生效。
下面来苻一个简单的例子，代码如下：
〈script type="text/javascript"> function demo(){
alert('JavaScript demo.');
}
</script>
<p onclick="demo(> ;n>点击我.</p>
本段代码的作用足为<p>元素设贾一个onclick事件，当单击此元素吋，会弹出一个对话 框，显示效果如图2-1所示。
像上面这样把JavaScript代码和HTML代码混杂在一起的做法同样也非常不妥，因为它 并没有将网页内容和行为分离，所以建议使用下面的方法，代码如下：


<p class="demo">jQuery Demo</p>
<script type="text/javascript">
$ (" .demo") .click (function () {	//给 class 为 demo 的元素添加行为
alert("jQuery demo!")；
})
</script>

此时，可以对CSS的写法和jQuery的写法进行比较。
CSS获取到元素的代码如下：
.demo{	//给class为demo的元素添加样式
jQiieiy获取到元素的代码如下：
$ (H .demo" {	//给class为demo的元素添加行为
jQuery选择器的写法与CSS选择器的写法十分相似，只不过两者的作川效果不同，CSS 选抒器找到元素后足添加样式，而jQuery选抒器找到元素后足添加行为。需要特别说明的 是，jQuery中涉及操作CSS样式的部分比单纯的CSS功能更为强大，并且拥有跨浏览器的 兼容性。
2.jQuery选择器的优势
2.简洁的写法
$()函数在很多JavaScript类俾中都被作为一个选择器函数来使用，在jQuery屮也不例外。 其屮，$("#1D")用来代替document.getE丨ementById〇函数，即通过1D获取元素；$(ntagName") 用来代替document.getElemcntsByTagName()函数，即通过标签名获収HTML兀素：其他选择 器的写法可以参见第2.3节。



3.支持CSS彳到CSS3选择器
jQuery选抒器支持CSS 1、CSS2的全部和CSS3的部分选择器，冋吋它也柯少％独有 的选杼器，因此对拥有一定CSS接础的开发人贝来说，学jQuery选择器楚件非常容易的 事，而对于没W接触过CSS技术的幵发人员来说，在学>J jQueiy选杼器的同时也可以帘捤 CSS选杼器的堪木规则。
使用CSS选杼器时，开发人员需要考虑主流浏览器足否支持某些选杼器。而在 jQuery中，幵发人员则可以放心地使用jQuery选择器而无耑考虑浏览器是否支持这些 选择器。
为了能有更快的选择器解析速度，从1.1.3.1版以后，jQuery废弃了不常使用的XPath ^	选择器，但在引用相关插件后，依然可以支持XPath选择器（详见第2.7.1小节）.

4.完善的处理机制
使用jQucry选杼器不仅比使用传统的getElementById〇和getElementsByTagName()函数 简洁得多，而ii还能避免臬些错误。C K面这个例子，代码如下：
<div>test</div>
<script type=,,text/javascript,,>
document•getElementByld("tt">•style.color="red";
</script>
运行上面的代码，浏览器就会报错，原因足网页屮没有id为“tt”的元素。 改进后的代H如下：
<div>test</div>
<script type="text/javascript">
if (document. getElementById{,,ttM)) {
document.getElementByld("tt").style.color="red";
}
</script>
这样就可以避免浏览器报错，但如果要操作的元素很多，可能对每个元素都要进行 一次判断，大苋奴的工作会使幵发人员感到厌倦，而jQuery在这方面问题上的处理足 非常不错的，即使川jQuery获取网页中不存在的元素也不会报错，釕下面的例子，代码 如下：
<div>test</div>
<script type="text/javascript">


vi： jQuer/ »»> »>	
$(，#tt*) .css ("color", "red");	//这里无需判断$(• #tt1 >是否存在
</script>
荇了这个预防措施，即使以后因为某种原闽删除网页上某个以前使用过的元素，也不用 祀心这个网页的JavaScript代码会报错。
需要注意的是，获収的永远足对象，即使N页上没有此元素。因此当要用jQuery 检金策•个元素往网页上是否存在时，不能使用以下代码：
if ( $("#tt") ) {
//do something
)
而应该根据获取到元素的长度来判断，代码如下:
if ( $(n#ttn).length > 0 )	{
//do something
成者转化成DOM对象来判断，代码如下:
if ( S("#ttn)[0]	) {
//do something
2.jQuery选择器
在:正式学4 jQuery选杼器之前，先希儿组用传统的JavaScript方法获取页面屮的元素， 然后给元素添加行为琪件的例子。
例子〗：给网页屮的所W<p>元素添加onclick祺件。
HTML代码如下：
<p>测试 l</p>
<p>测试 2</p>
要做的工作有以下几项。
①获取所有的<p>元素。
②对<p>元素进行循环（因为获取的是数组对象）。
③给每个<p>元素添加行为牢件。
JavaScript代码如下：
var items = document.getElementsByTagName("p"); //获取网页中所有的 p 元素 for(var i=0;i < items.length;i++){ //由于获取的是数组对象，因此需要把它循环出来



items [i] .onclick = function (> {	//给每个对象添加 onclick 事件
//doing something
例子2:使…个特定的表格隔行变色。
HTML代码如下：
〈table id=”tb">
<tbody>
<tr><td>|^—行 </td><td> 第一行 </td></tr> <tr><td> 第二行 </td><td> 第二行 </td></tr> <tr><td> 第三行 </td><td> 第三行 </td></tr> <tr><td> 第四行 </td><td> 第四行 </td></tr> <tr><td> 第五行 </td><td> 第五行 </td></tr> <tr><td> 第六行 </td><td> 第六行 </td></tr> </tbody>
</table>
要做的工作有以下儿项。
(2)根据表格id获取表格。
(3)在表格内获収<tbody>元素。
(4)在<化〇(^>元素K获取<过>元素。
(5)循环输出获取的<饮>元素。
(6)对<tr>元素的索引值除以2并取模，然后根据奇偶设置不同的背景色。
JavaScript 代码如 F:
var item = document. getElementByld ("tb") ;	//获取 id 为 tb 的元素（table )
var tbody = item.getElementsByTagName("tbody">[0]; //获取表格的第 1 个tbody元素 var trs = tbody.getElementsByTagName (ntrM) ； //获取 tbody 元素下的所有 tr 元素 for(var i=0;i < trs . length; i++) {	//循环 tr 元素
if (i%2==0) {	//取模（取余数.例如 0%2==01 %2==1 2%2==0 3%2==1)
trs[i].style.backgroundColor = "#888"; //改变符合条件的tr元素的背景色



例子3:对多选框进行操作，输出选中的多选框的个数。
HTML代码如K:
<input type='fcheckbox" value=n 1" name=f,check" checked/〉
cinput type="checkboxn value=M2" name="check" />
<input type="checkbox" value="3M name="check" checked/〉
〈input type=”buttonf丨 value=”你选中的个数n id="btn"/>
要做的工作有以K儿项。
(1 )新建一个空数组。
■获取所有name为“check”的多选框。
■循环判断多选框是否被选中，如果被选中则添加到数组里。
■获取输出按钮，然后为按钮添加onclick事件，输出数组的长度即可。
JavaScript代码如下：
var btn = document. getElementById (,fbtn")
btn.onclick = function(){
var arrays = new Array();
var items = document.getElementsByName("check");
//获取 name 为 check 的一组元素（checkbox ) for(i=0; i<items.length; i++) {	//循环这组数据
if (items [i] .checked) )	//判断是否选中
arrays .push (items [i] .value) ;//把符合条件的数据添加到数组中 //push ()是JavaScript数组中的方法
}
}
alert ("选中的个数为：”+arrays.length )
}
上而的儿个例子都是用传统的JavaScript方法进行操作，屮间使用了 getElernent Byld()、getElementsByTagNameO和 getElementsByName()等方法，然后动态地给元素添 加行为或者样式。这些虽然都是JavaScript中敁简单的操作，但不断重M使用 getElementById()和getElementsByTagName()等冗长而难记的名称，使越来越多的开发人 员开始厌倦这种枯燥的写法，并有时候为了获取N页屮的某个元素，需要编写很多的 getElementById()和 getElementsByTagName()方法。然而在 jQuery 屮，类似的这些操作 则是非常简洁。
下面学>』如何使〗tj jQiiery获収这些元素。
//获取id为btn的元素(button ) //给元素添加onclick事件 //创建一个数组对象



jQuery选抒器分为祛本选杼器、层次选杼器、过滤选杼器和表单选杼器。在下面的章节 中将分別用不同的选抒器来沓找HTML代码中的元素并对其进行简单的操作。为了能更涪 晰、直观地讲解选杼器，泣先耑要设计一个简单的页面，里面包含各种<(1^>元素和<span> 元素，然后使用jQuery选杼器来匹配元素并调整它们的样式。
新违-个空闩页面，输入以下HTML代码：
```html
<div class="one" id-none" >
id 为 one, class 为 one 的 div <div class="mini">class 为 mini</div>
</div>
<div class="one" id="two" title="test" > id 为 two, class 为 one, title 为 test 的 div.
<div class="mini" title="other">class 为 mini,title 为 other</div> <div class="mini" title="test">class 为 mini，title 为 test</div> </div>
<div class=,,one,'>
<div class="mini">class 为 mini</div>
<div class="mini">class 为 mini</div>
<div class="mini’’>class 为 mini</div>
<div class="mini"></div>
</div>
<div class="one,,>
<div class="mini’'〉class 为 mini</div>
<div class=Mmini">class 为 mini</div>
<div class=’’mini">class 为 mini</div>
<divclass="mini"title="tesst">classSmini,titleStesst</div>
</div>
<div style="display:none; " class="none">style 的 display 为’’none"的 div </div>
<div class="hide">class 为"hide"的 div</div>

<div>
包含 input 的 type 为"hidden"的 div<input type="hidden" size="8n/>
</div>
〈span id="mover">i在执行动画的 span 元素.〈/span〉
然后用CSS对这些元素进行初始化大小和背景颜色的设置，CSS代码如卜_:



div,span,p { width:140px; height:140px; margin:5px; background:#aaa; border:#000 lpx solid; float:left; font-size:17px; font-family:Verdana;
}
div.mini { width:55px; height:55px; background-color: #aaa;
* font-size:12px;
}
div.hide {
display:none;
}

根据以上HTML+CSS代码，可以生成图2-2所示的页面效果。
Id 为 one,class 为
one 的 dlv
(class 为
Id 为 two,class 为 one,t 丨 tie 为 test 的 dlv.
tt 含 input 的 type 为"hidden"的 div



正在执行动画的
span元素.
图2-2初始状态
2.3.1基市选择器
棊本选择器是jQuery中最常用的选杼器，也是最简单的选杼器，
和标签名等来查找DOM元素。在网页中，每个id名称只能使用一次，
它通过元素id、class class允许重复使用。
► 34







基本选杼器的介绍说明如表2-2所示
«<<<<«第j章jQuery选择器
表2-2	基本选择器
可以使用这些蕋本选择器来完成绝大多数的T:作。K面用它们来匹配刚才HTML代码屮 的<div>, <span>等元素并进行操作（改变背景色），示例如表2-3所示。
表2-3
基本选择器示例
功 能
代 码
执行后
改变id为one的元 桌的竹欺色
S(,#one,)
.css("background","tbbffaa")



改变class为mini
的所有元桌的竹
贵色
$('.mini1)
.css ("background1*, "#bbffaa");
>cb^tkVO,ClASS 为 mr山 tie 为testiB
生含l叩ut的type
^-hldden^^dlv
改变元系名是<div> 的所打元素的 识色
SCdiv')
• css ("background", "#bbffaa_’>
id 为 two,class 为 »ne,tltte 为test的
3ses5l
««WtU
tMfm

ft^inputaitype
AMhiddenMB^dlv
BBFrtilsr
man*.-
35 ◄-



iQuer/ »»> »>
续表
功能
改变所荇元素的竹$(, 妖色
代码
.css ('background'*, H#bbffaaM);
执行后
改变所冇的<span> SC span, #twof)
元桌和id为two的
元索的最色	.css ("background", "Ibbffaa");
2.3.2层次选择器
如果想通过DOM元素之间的层次关系来获取特定元素，例如后代元素、子元素、相邻 元素和兄弟元素等，那么层次选择器是一个非常好的选择。层次选抒器的介绍说明如表2-4 所示。
继续沿用刚才例屮的HTML和CSS代码，然后用层次选杼器来对N页中的<div>， <span>g元素进行操作，示例如表2-5所示。
► 36











:<<<<<<<
第2章jQuery选择器
表2-5
层次选择器示例
功 能
代 码
执行后
改变<body>内所 背撗色
S(*body div')
.css ("background,,/ "Ibbffaa")
3555* iauA

id 为 one,class 为 one9ldtv
改变<body>内子 <div>元素的背贵色
$ ('body > div')
.css("background",M#bbffaa")
id 为 two,class 为
为 test 的
div.
hSt
si«n
SJBF
3MA
dm*
ttrt
改变class为one 的卜‘一个<€^>元 系背贵色
rani
^]|
國匿
& 金 input 的 type 为 *hld<Jen”的 div
Vg»rOL9.

idAon^ciaw^r
»ne 的 div
S('.one + div'
id 为 two'lass 力 | one.tltle力 test 的 j div.	!
MW* j wtijMi
«rt««
HE
M\E
改变id为two的元 棄后面的所符4iv> 兄弟元素的背欺色
•css("background","#bbffaa")
」i	i
i3F1 I3?5r
wmM#
& 含 Input 的 type ABhJdden"tfJdiv
E^K9$Bi5~~I
span 元••
在层次选杼器中，第1个和第2个选择器比较常用，而后面两个W为在jQuery里可以
用更加简单的方法代替，所以使用的儿率相对少些。
可以使用next()方法来代替$('prev + next1)选择器，如表2-6所示。
表2_6	$(’prev + nexf>选择器与nex丨()方法的等价关系
可以使用nextAII()方法來代替$('prev〜siblings')选择器，如表2-7所示。
37 <

^ jQuerY »»> »>-
表2-7	$('prev〜s丨blings'^择器与nextAH(>方法的等价关系
在此将siblings()方法与$('prev〜siblings’)选杼器进行比较。
$("#prev〜div")选择器只能选择“#prev”元素后面的同_<div>元素。而siblings()方法与 前后位胥无关，只要足同辈节点就都能匹配。
$(l#prev〜div丨）.css("background丨V’#bbffaa");//选取#prev之后的所有同辈div元素 $(' #prev') .nextAll ("div") .css ("background", "#bbffaan);"同上 $(,,#prevn) .siblings ("div") .css {"background", "tbbffaa");
//选取#prev所有的同辈div元素，无论前后位置
2.3.3过滤选择器
过滤选杼器主要足通过特定的过滤规则來筛选出所需的DOM元素，过滤规则与CSS中 的伪类选择器语法相同，即选杼器都以-个冒号(:)开头。按照不同的过滤规则，过滤选杼器 可以分为基本过滤、内容过滤、可见性过滤、属性过滤、子元素过滤和表单对象M性过滤选 杼器。
1.基本过滤选择器
表2-8	基本过滤选择器
► 38





«< ««<第^章jQuery选择器
续表
接下来，使川这些装本过滤选杼器来对网页屮的<div>, <印311>等元素进行操作，示例 如表2-9所示。
表2-9
基本过滤选择器示例
功
能
代
码
改变第 1 t<div> $(，div:first'>
元蒺的W眾色	.css ("background",
••#bbffaa">
改变坡后-个<div> $ (' div: last1 >
元素的背思色	.css ("background", "#bbffaa")
改变class不为one 的<^卜>元尜的背 燉色
$('div:not(.one)')
.css("background","#bbffaa")
改变索引值为偶 数的<div>元紊的 背眾色
$('divieven*)
.css("background",
M#bbffaaM)
执行后
d^tw〇#d«MA
为 test 的
wi XM4
VT
tM4 mtA
(a•金 Input 的 type 为"hidden•的 dlv
id 为 one,class 为 oneftdlv
9B
■
id 为 two,c 丨 ass 为 Dnc^title 为 testBl div.
Stun
Wi«Q〇«
iSSn
5*Spi5Svpe
McWin•的 div
ipniTcit.
iM
rtmM
明MU
39 ◄

^ jQuerY »»> »>
续表
功能
改变索引值为夼 数的<div>元桌的 背規色
代 码
S('diviodd1)
.css("background",H#bbffaa")
B^oneidess*
MilkBy
执行后
d力 two,class 力
m
hocrw mtu



lSBTI 植含 Input的typo h«	jb*hldden-«d.v

fimtn
mni.Qtlf
mttsr

改变索引值等r 3 的<(^>元素的背 撗色
$(?div:eq(3)')
.css("background",M#bbffaa")



tftv.
•为 tPfit ft
Tvm.tjOt
iM»n
机，st
E^l
3m£7
B*1
E
p5-
H
EmsT*
l^hldden'i
■put 的 type:
rdlv；
(panfc?

改变索引值人于3 的<(^>元素的背 设色
S( 'div:gt (3)')
.css("background",n#bbffaa")
JatiA
S^two,dass 为
boMMr
m
35ST1
W.w«
5h>i
wi.ttM

pr
3«M%
&含丨nput约typ« AMhlddenN»div
改变索引值小r 3 的<£^>元桌的f? 景色
S(，div:lt ⑶’）
.css("background","#bbffaan)

含 lnputr：typ«
udcfeiV* 的 div
HSR^SsT
Vwb^«.
改变所荇的标题 元素，例如<hl>，
<h2>, <h3>	这
些元素的背枭色
S(1:header*)
.css("background","#bbffaa");
基本过滤选择器.
改变当前正在执 行动iWi的元紊的 背贺色
S(1:animated1)
.css("background",H#bbffaaM);
IdijoneeCUws.'S
S3T
r,ttl•为 hstn

kimxn \ Ura«〇0« M.tOt hoftm MM

好 input 的 type MtiMMandiv
E5S?F»M$r
ipan 元 ft.










—<< <«<<第j章jQuery选择器
2.内容过滤选择器
内容过滤选杼器的过滤规则主要体现在它所包含的子元素或文本内容上。内容过滤选杼
器的介绍说明如表2-10所示。
表2-丨〇	内容过滤选择器
接下来使用内界过滤选杼器来操作页面中的元素，示例如表2-11所示。
表 2-11
内容过滤选择器示例
功
能
代
码
执
行
后
改变含有文本“di”
的<div>元索的背
景色
•css("background","fbbffaa”）；
MTU
p»5S"
htttn
&含丨nput的type
为•htdderTRdlv

改变不包含子元
桌（包括文本元Srdiv:empty’）
素）的<如>空兀	.css ("background", "#bbffaa")
素的竹展色
改变^T class为$( mini元素的<div>
元素的背敖色
*div:has(mini)T)
.css("background",M#bbffaa")
41 ◄	



^ jQueiY
功能
改变含冇子元尜 (包括文本元素） 的<div>元素的竹 贵色
»»> »>-
续表
代 码
('div:parent')
.css("background","tbbffaa")
执行后
丨dAtwo,c 丨 ass 为
为 tost 的

pt««
ft^lflputncype
A"hlddena»dlv
mm
3.可见性过滤选择器
可见性过滤选杼器是根据元素的可见和不可见状态来选择相应的元素。可见性过滤选杼 器的介绍说明如表2-12所示。
表2-丨2	可见性过滤选择器
在例子中使用这些选杼器来操作DOM元素，示例如表2-13所示。
表:M3
可见性过滤选择器示例
功 能
代 码
执行后
改变所有可见的
<div>元素的背
贵色
$(* div:visible')
.css ("background",”#FF6500")
显示隐藏的<div> 元素
$('div:hidden').show(3000);
KWinpulWvpe
■'hidttefi'if'Klfv


{>42



		<« ««<第j章jQuery选择器
在可见性选择器中，需要注意选择器：hidden,它不仅包括样式属性display为“none” 的兀素，也包柄文本隐藏域(〈input type="hidden" />)和visibility:hidden之类的元素。
^注意	show〇是jQuery的方法，它的功能是显示元素，3000是时间，单位是毫秒。	•
..	I»_	_，■	«1	—~	一	•一	■ ■	■■丨		
4.属性过滤选择器
M性过滤选杼器的过滤规则是通过元素的M忭来获取相应的元素。属性过滤选杼器的介 绍说明如表2-14所示。
表2-14	属性过滤选择器
接F来使用域性过滤选杼器來对《^>和<叩311>等元素进行操作，示例如表2-15所示。
表 2-15
属性过滤选择器示例
功 能
代 码
执行后
改变含冇W性title 的<(^>元素的背 景色
S('div[title]')
.css("background","Ibbffaa")
丨d为tm>,dass为 one,Mtle 为 test 的 jiv.
EusST*! BJS5T
pn,Mtl vnmfMt
brother bit^k
mH
:Us坊
tltfnputet^i
大"hidden•的 dfv
sp9n^X.
43 ◄





^ jQuerY »»> »>	
续表
功
能
代
码
改变M性title值等 丁• “test” 的<div> 元素的背贺色
$(1div[title=test]')
.css("background",
"tbbffaa");
执行后
改賴性title值不 等于 “test” 的 <div> 元紊的背景色
$(1 div[title!=test]1)
.css("background","tbbffaa")
改变试性title值以 “te”开始的<div> 元素的背景色
$('div[titleA=te]')
.css ("background", "fbbffaa1');
改变屈性title值
以 “est” 结束的 $("div[title$=est]，_)
<div> 元素的背• css ("background", "#bbffaa") 欺色
id 为 two,class 为
MniMto
Mother
Hr
丨 nput 的 type H’hkWerTWcMv
改变属性title值$ 含冇 “es”&<div>
元索的背景色
"div[title*=es]")
.css ("background", "tbbffaa'*);
»pw 死
► 44

«<<<<<<第^章jQuery选择器
续表
功能	代码
改变含荇M性id.
并且M性 title 值 sr，div[id] [title*=es]">
含冇“es” 的<div>	.css ("background", "#bbffaa")
元素的竹贳色
执行后
5.子元素过滤选择器
子元素过滤选抒器的过滤规则相对于其它的选杼器稍微有些M杂，不过没关系，只要将 元素的父元素和子元素区分淸楚，那么使用起来也非常简单。另外还要注意它与普通的过滤 选择器的R别。
子元素过滤选择器的介绍说明如表2-16所示。
表2_丨6	子元素过滤选择器
:mh-child()选杼器足很常用的子元素过滤选杼器，详细功能如下。
1.:mh-child(even)能选取每个父元素下的索引值是偶数的元素。
2.:nth-Child(odd)能选取每个父元素下的索引值是奇数的元素。
45 ◄









»»> »>



3.:nth-child(2)能选収每个父元素下的索引值等于2的元素。
4.:nth-child(3n)能选収锊个父元素下的索引值楚3的倍数的元素，（n从0开始)。
5.:mh-chiki(3n+丨)能选取每个父元素卜‘的索引值是（3n+l)的元素。（n从0开始) 接K来利用刚j所讲的选择器来改变<div>元素的背景&，示例如表2-17所示。
表2-重7
子元素过滤选择器示例
功 能
改变每个class为 one的<div>父元 索卜'的第2个子 元素的背贺色
S(
代 码
div.one :nth-child(2)')
.css("background",M#bbffaa")
改变每个class为
one 的 <div> 父元 S (丨div.one :first-child’>
疾卜的第丨个子	.css ("background丨丨，"#bbffaa">
元#的竹規色
改变每个class为 one的<div>父元 尜K的M后一个 子元素的背展色
$(
如果class为one
的<(^>父元素下
只打一个子元素，
那么则改变这个
子元素的背景色
S(
div.one :last-child*)
.css("background",M#bbffaa")
div.one :only-child*)
.css("background","#bbffaa">
执彳〒后
S
* Bssjn
Um,lAWl

.^rinputntvpe
|"hkJdon*f：olv
丨的1
eq(index)只匹配一个元素，而:nth-child将为每一个符合条件的父元素匹配子元素. 注意同时应该注意到nth-chiid(index)的index是从1开始的，而:eq(index)是从0开始的• 同理：first 和:first-child，：last 和:last-child 也类似•
► 46

«<««<第^章jQuery选择器
6.表单对象属性过滤选择器
此选杼器主要足对所选杼的表单元素进行过滤，例比如选择被选中的下拉框，多选框等等。 表单对象屈性过滤选杼器的介绍说明如表2-18所示。
表:表单对象属性过滤选择器
为了演示这些选杼器，需耍制作一个包含表单的网页，里面要包含文本框、多选框和K 拉列表，HTML代码如卜‘：
```style
<form id="formlM action=n#n>
可用元素：〈input name="add" value="可用文本框”/> <br/>
不可用元素：〈input name="email” disabled=ndisabled丨’ value=”不可用文本 框"/><br/>
可用元素：〈input name="che" value=n可用文本框"/><br/>
不可用元素：〈input name="name" disabled="disabled" value="不可用文本 框"/><br/>
<br/>
多选框：<br/>
<input type=ncheckbox" name="newsletter" checked="checked" value=’’testl" />testl
<input type=Hcheckbox" name=’’newsletter" value=r'test2" />test2 <input type=f,checkbox" name= "newsletter" value="test3" />test3 <input type=ncheckbox" name^"newsletter" checked=’’checked" value="test4" />test4
<input type=’，checkbox" name="newsletter" value=ntest5n />test5 <div></div>
<br/xbr/>
```
下拉列表1: <br/>
47 ◄

```style
<select name="testn multiple=nmultiplen style=Hheight:100pxM>
<option> 浙江 </option〉
<option selected="selected">湖南</option>
<option> 北京 </option>
<option selected^"selected’）天津</option>
<option> 广州 </option>
<option> 湖北</option>
</select>
<br/xbr/>
下拉列表2: <br/>
<select name="test2" >
<option> 浙江</option>
<option>湖南 </option>
<option selected^"selected">北京</option> <option>天津 </option>
<option>r" jH1</〇ption>
<option> 湖北</option>
</select>
<divx/div>
</form>
```
生成的效果图如图2-3所示。
可用元素.	：
不可用元']
可用元素I >用文稍 不可用元素丨
多选枢<
Otestl Dtest2 Dtest3 0test4 Dtest5 有2个被选中！
下拉列表1:
浙江、|
EOI
W
W
湖北丨
下拉列表
，北京V
供迭中的是》天律.北京.
阁2-3初始状态
现在用jQuery的表单过滤选择器来操作它们，示例如表2-19所示。
► 48

<"««<第^章jQuery选择器
表2-19	表单对象属性过滤示例
2.3.4表单选择器
为了使用户能够更加灵活地操作表单，jQuery中专门加入了表单选杼器。利用这个选杼
器，能极典方便地获取到表单的某个或菜类型的元素。 表单选杼器的介绍说明如表2-20所示。
表2-20	表单对象属性过滤示例
下面把这些表单选抒器运用到下面的表单屮，对表单进行操作。 表单HTML代码如卜：
49 <

```style			
〈form id="formln action:"#"〉
.	cinput type="button" value=MButtonn/xbr/>
<input type=,fcheckbox" name=’’c"/>l 〈input type=Hcheckbox" name:=,,c,'/>2 <input type="checkbox" name="c"/>3<br/>
<input type="filen /><br/>
〈input type="hidden" /><div style="display:none">test</div><br/> 〈input type=Himage" /><br/>
<input type="password" /><br/>
〈input type=nradio" name="a"/>l<input type="radio” name="a"/>2<br/> <input type=”reset" /><br/>
〈input type="submit" value="提交"/><br/>
<input type="textn /><br/>
<select><option>Option</optionx/selectxbr/>
<textarea></textarea><br/>
<button>Button</button><br/>
</form>
```
根据以h HTML代码，可以生成图2-4所示的页面效果。
如果想得到表单内表单元素的个数，代码如K:
$("#forml :input") • length;	//注意与$("#forml input")的区别
如果想得到表单内单行文本框的个数，代码如下.•
$r'#forml :textn) .length;
如果想得到表单内密码框的个数，代码如下：
$(M#forml :password").length;
► 50

同理，其他表单选杼器的操作与此类似
<<<



## 23

![baidu](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/2/50.jpg "百度logo")

## 24  

![baidu](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/2/51.jpg "百度logo")

## 26  

![baidu](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/2/57.jpg "百度logo")

## 27  

![baidu](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/2/61.jpg "百度logo")
