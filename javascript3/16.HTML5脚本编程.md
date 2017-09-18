##  第16章 HTML5脚本编程([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 使用跨文档消息传递
- [ ] 拖放API
- [ ] 音频与视频  

t书前面讨论过，HTML5规范定义了很多新HTML标id。为T配合这碑标记的变化，HTML5 ^规范也用显著篇幅定义了很多JavaScript API。定义这拽API的用意就是简化此前实现起来困 难重重的任务，最终简化创建动态Web界面的工作。
16.1跨文档消息传递
跨文档消息传送（cross-documentmessaging),有时候简称为XDM,指的是在來自不同域的贞面间 传递消息。例如，www.wrox.com域中的页面与位于一个内嵌框架中的p2p.wrox.com域中的面面通信。 在XDM机制出现之前，要稳妥地实现这种通信需要花很多工夫。XDM把这种机制规范化，让我们能
既稳妥又简申地实现跨文挡通信。
XDM的核心是postMessage ()方法。在HTML5规范中，除了 XDM部分之外的其他部分也会提 到这个方法名，但都是为了同一个E3的：向另一个地方传递数据。对于XDM而言，“另--个地方”指的 是包含在当前页面中的<1【rame>元素，或者由当前页面弹出的窗U。
postMessageU方法接收两个参数：一条消息和一个表示消息接收方来自哪个域的字符串。第二 个参数对保障安仝通信非常重要，可以防止浏览器把消息发送到不安全的地方。来看下面的例子。
//注意：所有支持XDM的浏览器也支持iframe的contentWindow属性
var iframeWindow = document .getElementById{ "myfrciine") .contentWindow;
iframeWindow.postMessage{"A secret", "http：//www.wrox.com");
最后一行代码尝试向内嵌框架中发送一条消息，并指定框架中的文档必须来源T"http:// ;^^.\«"<«<.<：0111'域。如果来源匹配，消息会传递到内嵌框架中;西则，9〇311^53396()仆么也不做。 这一限制可以避免窗U中的位置在你不知情的情况下发生改变。如果传给postMessage ()的第二个参 数足"*则表示可以把消息发送给来fl任何域的文档，但我们不推荐这样做。
接收到XDM消息时，会触发window对象的message事件。这个亊件是以异步形式触发的，闵此 从发送消息到接收消息（触发接收窗门的message事件）可能要经过一段时间的延迟。触发message 礙件后，传递给omnessage处理程序的事件对象包含以下三方面的重要信息。
dara:作为postMessage()第一个参数传人的字符串数据。
origin:发送消息的文样所在的域，例如'’http://www.wrox.com"。

16.2原生拖放 481
- [ ]  source:发送消息的文档的window对象的代理。这个代理对象主要用于在发送上一条消息的 窗口中调用postMessageO方法。如果发送消息的窗口来自同一个域，那这个对象就是 window〇
接收到消息后验证发送窗n的来源是至关重要的。就像给postMessageU方法指定第二个参数， 以确保浏览器不会把消息发送给未知页面一样，在onmessage处理程序中检测消息来源可以确保传人 的消息来自已知的页面。基本的检测模式如下。
EventUtil.addHandler(window, "message", function(event){
//确保发送消息的城是已知的域
if (event.origin == "http：//www.wrox.com*){
//处理接收到的教据 processMessago(event.data);
//可选：向来源窗口发送回执
event.source,posnMessage("Received!", "

http://p2p.wrox.com")?
}
});
还是要提醒大家，event.source大多数情况下只是window对象的代理，并非实际的window对 象。换句话说，不能通过这个代理对象访问window对象的其他任何信息。记住，只通过这个代理调用 postMessage()就好，这个方法永远存在，永远可以调用。
XDM还有一些怪异之处。首先，postMessageO的第一个参数最早是作为“永远都是字符串”来实 现的。但后来这个参数的定义改了，改成允许传人任何数据结构。可是，并非所有浏览器都实现了这一 变化。为保险起见，使用postMessageU时，最好还是只传字符串。如果你想传人结构化的数据，最 佳选择是先在要传人的数据上调用JSON.stringify(),通过postMessage()传人得到的字符串，然 后再在orunessage事件处理程序中调用JSON.parse ()。
在通过内嵌框架加载其他域的内容时，使用XDM是非常方便的。因此，在混搭（mashup)和社交 网络应用中，这种传递消息的方法极为常用。有了 XDM,包含iframe的页面可以确保自身不受恶意 内容的侵扰，因为它只通过XDM与嵌人的框架通信。而XDM也可以在来自相同域的页面间使用。
支持 XDM 的浏览器有丨E8+、Firefox3.5+、Safari 4+、Opera、Chrome、iOS 版 Safari 及 Android 版 WebKit。XDM已经作为一个规范独立出来，现在它的名字叫Web Messaging,官方页面是 ht^)：//dev.w3.oiig/htmI5/postnisg/0
16.2原生拖放
最早在网页中引人JavaScript拖放功能的是丨E4。当时，网页中只有两种对象可以拖放：图像和某 些文本。拖动图像时，把鼠标放在图像上，按住鼠标不放就可以拖动它。拖动文本时，要先选中文本， 然后可以像拖动图像--样拖动被选中的文本。在IE4中，唯一有效的放置目标是文本框。到了 IE5,拖 放功能得到扩展，添加了新的事件，而且几乎网页中的任何元素都可以作为放置H标。1E5.5更进一步， 让网页中的任何元素都可以拖放。（丨E6同样也支持这些功能。）HTML5以IE的实例为基础制定拖放 规范。Firefox3.5、88丨^3+和（：111〇11«也根据10'1^5规范实现广原生拖放功能。
说到拖放，最有意思的恐怕就是能够在框架间、窗口间，甚至在应用间拖放网页元素了。浏览器对 拖放的支持为实现这些功能提供了便利。

482 第16幸HTML5脚本编程
16.2.1拖放事件
通过拖放事件，可以控制拖放相关的各个方面。其中最关键的地方在于确定哪里发生了拖放事件， 有些事件是在被拖动的元索上触发的，而冇些事件是在放置目标上触发的。拖动某元素时，将依次触发 下列亊件：
dragstart
drag
dragend
按下鼠标键并开始移动鼠标时，会在被拖放的元素h触发dragstart事件。此时光标变成“不能放” 符号（阒环中有一条反斜线），表示不能把元素放到自己上面。拖动开始时，可以通过ondragstart 事件处理程序来运行JavaScript代码。
触发dragstart事件后，随即会触发dr3g事件，而且在元素被拖动期间会持续触发该事件。这 个事件与mousemove事件相似，在鼠标移动过程中，mousemove事件也会持续发生。当拖动停止时（无 论是把元素放到了有效的放置目标，还是放到了无效的放置目标上），会触发dragend事件。
上述三个事件的H标都是被拖动的元索。默认情况下，浏览器不会在拖动期间改变被拖动元素的外 观，但你可以自己修改。不过，大多数浏览器会为正被拖动的元素创建一个半透明的副本，这个副本始 终跟随者光标移动。
当某个元素被拖动到一个有效的放置目标上时，下列事件会依次发生：
dragenter
dragover
dragleave 或 drop
只要有元素被拖动到放置目标上，就会触发dragenter事件（类似于mouseover事件)。紧随其 后的是dmgover事件，而且在被拖动的元素还在放置口标的范围内移动时，就会持续触发该事件。如 果兀素被拖出了放置H标，dragover事件不再发生，但会触发dragleave事件（类似于mouseout 事件)。如果元素被放到了放置目标中，则会触发drop事件而不是dragleave亊件。上述三个事件的 目标都是作为放置目标的元素。
16.2.2自定义放置目标
在拖动元素经过某些无效放置目标时，可以看到一种特殊的光标（圆环中有一条反斜线），表示不 能放置。虽然所有元索都支持放置目标事件，但这些元素默认是不允许放置的。如果拖动元素经过不允 许放罝的元素，无论用户如何操作，都不会发生drop事件。不过，你可以把任何元素变成有效的放置 B标.方法是重写dragenter和dragover事件的默认行为。例如，假设有•-个仍为1'droptarget11 的<div>元素，可以用如下代码将它变成一个放置目标。
var droptarget = document.getElementByld("droptarget");
EventUtil.addJiandler(droptarget, "dragover", function(event){
EventUtil.proventDefault(event);
}»;
EventUtil.addHandler(droptarget, "dragenter", function(evcnL){
EventUtil.preventDefault(event);
}>; *

16.2原生拖放 483
以上代码执行后，你就会发现当拖动聍元素移动到放置目标上时，光标变成了允许放置的符号。当 然，释放鼠标也会触发drop事件。
在Firefox 3.5+中，放置事件的默认行为是打开被放到放置目标上的URL。换句话说，如果是把图 像拖放到放置目标上，页面就会转向图像文件;而如果是把文本拖放到放置目标上，则会导致无效URL 错误。因此，为了让Firefox支持正常的拖放，还要取消drop事件的默认行为，阻止它打开URL:
EventUtil.addHandler(droptarget, "drop", function(event){
EventUtil.preventDefault(event);
));
dataTransf er 对象
只有简单的拖放而没有数据变化是没有什么用的。为了在拖放操作时实现数据交换，m 5引人了 dataTransfer对象，它是事件对象的一个属性，用于从被拖动元素向放置目标传递字符串格式的数据。 因为它是事件对象的属性，所以只能在拖放事件的事件处理程序中访问dataTransfer对象。在事件 处理程序中，可以使用这个对象的属性和方法来完善拖放功能。目前，HTML5规范草案也收人了 dataTransfer 对象0
dataTransfer对象有两个主要方法：getData()和setData()。不难想象，getDataU可以取 得由setDataU保存的值〇 setData()方法的第一个参数，也是getDataO方法唯一的一个参数，是 —个字符串，表示保存的数据类型，取值为-text•或"URL%如下所示：
"设t和接收文本数据
event.dataTransfer.setData{"text", "some text*); ?var text = event.dataTransfer.getData("text*);
//设置和接收URL
event.dataTransfer.setData(*URL", "

http://www.wrox.com/*); var url = event .dataTransfer .getData( •CJRL");
IE只定义了 ■ text"和■ URL •两种有效的数据类型，而HTML5则对此加以扩展,允许指定各种MIME 类型。考虑到向后兼容，HTML5也支持-text•和-URL*,但这两种类型会被映射为-text/plain"和 "text/uri-lisfo
实际上，dataTransfer对象可以为每种MIME类型都保存一个值。换句话说，同时在这个对象 中保存一段文本和一个URL不会有任何问题。不过，保存在dataTransfer对象中的数据只能在drop 事件处理程序中读取。如果在ondrop处理程序中没有读到数据，那就是dataTransfer对象已经被
销毁，数据也丢失了。
在拖动文本框中的文本时，浏览器会调用setDataG方法，将拖动的文本以-text•格式保存在 dataTransfer对象中。类似地，在拖放链接或阁像时，会调用setData()方法并保存URL。然后， 在这些元素被拖放到放置目标时，就可以通过getData ()读到这些数据。当然，作为开发人员，你也 可以在dragstart事件处理程序中调用setDataO,手工保存自己要传输的数据，以便将来使用。
将数据保存为文本和保存为URL是有I?(别的。如果将数据保存为文本格式，那么数据不会得到任 何特殊处理。而如果将数据保存为URL,浏览器会将其当成网页中的链接。换句话说，如果你把它放置 到另一个浏览器窗U中，浏览器就会打开该URL。
Firefox在其第5个版本之前不能正确地将-url”和"text•映射为-text/uri-list"和 •text/plain-。但是却能把"Text”（T大写）映射为_text/plain■。为了更好地在跨浏览器的情况

484 第16章HTML5脚本编程
下从dataTransfer对象取得数据，最好在取得URL数据时检测两个值，而在取得文本数据时使用
"Text"〇
var dataTransfer = event.dataTransfer;
//读取URL
var url = dataTransfer.getData("url") I IdataTransfer.getData{"text/uri-list");
//读取文本
var text = dataTransfer.getData("Text");
DataTransferExampleQl.htm
注意，一定要把短数据类型放在前面，因为IE 10及之前的版本仍然不支持扩展的MIME类型名， 而它们在遇到无法识别的数据类时，会抛出错误。
dropEffeet 与 effectAllowed
利用dataTransfer对象，可不光是能够传输数据，还能通过它来确定被拖动的元素以及作为放 置目标的元素能够接收什么操作。为此，需要访问dataTransfer对象的两个属性：dropEffect和 effectAlXowed〇
其中，通过dropEffect属性可以知道被拖动的元素能够执行哪种放置行为。这个属性有下列4 个可能的值。
不能把拖动的元素放在这里。这是除文本框之外所有元素的默认值。
"move”：应该把拖动的元素移动到放置目标。
‘icopy":应该把拖动的元素复制到放置目标。
表示放置目标会打开拖动的元素（但拖动的元素必须是一个链接，有URL)。
在把元素拖动到放置目标上时，以上每一个值都会导致光标显示为不同的符号。然而，要怎样实现 光标所指示的动作完全取决于你。换句话说，如果你不介人，没有什么会自动地移动、复制，也不会打 开链接。总之，浏览器只能帮你改变光标的样式，而其他的都要靠你自己来实现。要使用dropEffect 属性，必须在ondragenter事件处理程序中针对放置H标来设置它〇
dropEffect属性只有搭配effectAllowed属性才有用。effectAllowed属性表不允许拖动元 素的哪种dropEf feet, effectAllowed属性可能的值如下。
■uninitialized**:没有给被拖动的兀素设置任何放置行为。
”n〇ne«:被拖动的元素不能有任何行为。
口”copy":只允许值为"copy-W dropEffect。
"link”：只允许值为 **1;11^;_的 dropEffect。
"move":只允许值为"move"的 dropEffect。
口 "copyLirxk":允许值为"。。奴”和” link1•的 dropEffect。
口 _copyMove":允许值为的 dropEffect。
Ml_inkMove’：允许值为"link’和_raove"的 dropEffect:。
"all 允许任意 dropEffect。
必须在ondragstart事件处理程序中设置effectAllowed属性。

16.2原生拖放	485
假设你想允许用户把文本框中的文木拖放到一个<div>元素中。首先，必须将dropEffect和 effectAllowed设S为"move"。但是，由T<div>元素的放置事件的默认行为是什么也不做，所以文 本不可能自动移动。重写这个默认行为，就能从文本框中移走文本。然后你就可以自己编写代码将文本 插人到<<3^>中，这样整个拖放操作就完成j*。如果你将dropSffect和effectAllowed的值设置为 «copy",那就不会ft动移走文本框中的文本。
Firefox5及之前的版本在处理effectAllowed属性时有一个问题，即如果你在 代码中设置了这个属性的值，那不一定会触发drop事件。
16.2.5可拖动
默认情况下，图像、链接和文本是可以拖动的，也就是说，不用额外编写代码，用户就可以拖动它 们。文本只有在被选中的情况下•能拖动，而图像和链接在任何时候都可以拖动。
让其他元素可以拖动也是可能的。HTML5为所有HTML元素规定了一个draggable属性，表 示元素是否可以拖动。图像和链接的draggable属性自动被设置成了 true,而其他元索这个域性 的默认值都是false。要想让其他元素可拖动，或者让图像或链接不能拖动，都可以设置这个属性。 例如：
<!--让这个图像不可以拖动-->
<img src="smile.gif" draggable="false" ait=*Smiley face**>
<!--让这个元素可以拖动-->
<div draggable=*'crue">.. .</div>
支持 draggable 屁性的浏览器有 IE 10+、Firefox 4+、Safari 5+和 Chrome。Opera 11.5 及之前的版 本都不支持HTML5的拖放功能。另外，为了让Firefox支持可拖动属性，还必须添加一个ondragstart: 事件处理程序，并在dataTransfer对象中保存一些信息。
在IE9及更早版本中，通过mousedown事件处理程序调用dragDropU能够让 任何元素可拖动。而在Safari 4及之前版本中，必须额外给相应元素设置CSS样式
-khtml-user-drag： element〇
16.2.6其他成员
HTML5规范规定dataTransf er对象还应该包含下列方法和属性。
addElement (eieinent):为拖动操作添加一个元素。添加这个元素只影响数据（即增加作为拖 动源而响应问调的对象）,不会影响拖动操作时页面元素的外观。在写作本书时，只有Firefox 3.5+ 实现了这个方法。
clearDatW format):淸除以特定格式保存的数据。实现这个方法的浏览器有IE、Firef〇ra3.5+、 Chrome 和 Safari 4+。
setDragImage(eiemejjt, x, y>:指定一幅图像，当拖动发生时，显示在光标下方。这个方

486 第16章 HTML5脚本编程	
法接收的三个参数分別是要显示的HTML元索和光标在阁像中的x、jp坐标。其中，HTML元素 可以是一幅阁像，也可以是其他元素。是图像则显示阁像，是其他元索则显示渲染后的元素。 实现这个方法的浏览器有Hrefox 3.5+、Safari 4+和Chrome。
- [ ] types:当前保存的数据类型。这是一个类似数组的集合，以_text”这样的字符串形式保存着 数据类型。实现这个属性的浏览器有IE10+、Firefox3.5+和Chrome。
16.3媒体元素
随着音频和视频在Web上的迅速流行，大多数提供富媒体内容的站点为了保证跨浏览器兼容性， 不得不选择使用Flash。HTML5新增7两个与媒体相关的标签，让开发人员不必依赖任何插件就能在网 页中嵌人跨浏览器的音频和视频内容。这两个标签就是<311(^〇>和0^360>。
这两个标签除了能U;开发人员方便地嵌人媒体文件之外，都提供了用于实现常用功能的JavaScript API,允许为媒体创建自定义的控件。这两个元素的用法如下。
!--嵌入视酱—>
```
<video src="conference.mpg" id=(,myVideo0>Video player not available.</video>
<!—嵌入音頻—>
<audic src="song.mp3* id="myAudio">Audio player not available.</audio>
```
使用这两个元素时，至少要在标签中包含SI：C属性，指向要加载的媒体文件。还可以设S width 和height属性以指定视频播放器的大小，而为poster属性指定图像的UR1可以在加载视频内容期间 显示一幅图像。另外，如果标签中有controls属性，则意味着浏览器应该显示UI控件，以便用户直 接操作媒体。位于开始和结束标签之间的任何内容都将作为后备内容，在浏览器不支持这两个媒体元素 的情况下显示。
因为并非所有浏览器都支持所有媒体格式，所以可以指定多个不同的媒体来源。为此，不用在标签 中指定src属性，而是要像卜面这样使用一或多个<s〇UrCe>元素。
!—嵌入视傾—>
```
<video id= wmyVideo•>
<source src="conference.wetMn" types■video/webm; codecs*'vp8, vorbis'">
<sourcc src="conference.ogv" type-"video/ogg; codecs= rtheora, vorbis'">
<source src="conference.mpg*>
Video player not available.
</video>
<!—在入音頻-->
<audio id=*ntyAudio">
<sourcc src=*song.ogg" typc=•audio/ogg">
<source src«"song.mp3 * type="audio/mpeg">
Audio player not available.
</audio>
```
关于视频和音频编解码器的内容超出了本书讨论的范围。作者在此只想告诉大家，不同的浏览器支 持不同的编解码器，w此一般来说指定多种格式的媒体来源是必需的。支持这两个媒体元素的浏览器有 IE9+、Firefox 3.5+、Safari 4+、Opera 10.5+、Chrome、iOS 版 Safari 和 Android版 WebKit。
16.3.1属性
<vide〇>W<audi〇>元素都提供了完善的JavaScript接门。下表列出了这两个元素共有的属性，通 过这些属性可以知道媒体的当前状态。
其中很多属性也可以直接在<audi〇：^<vide〇>元素中设置。

488 第16章HTML5脚本编程
16.3.2事件
除了大M屈性之外，这两个媒体元索还可以触发很多事件。这些事件监控着不同的M性的变化，这 些变化可能是媒体播放的结果，也可能是用户操作播放器的结果。下表列出了媒体元素相关的事件。
这些事件之所以如此具体，就是为了让开发人员只使用少景HTML和;JavaScript (与创建Flash影 片相比）即可编写出自定义的音频A视频播放器。
16.3.3自定义媒体播放器
使用audio元素的play (和pause ()方法，可■以手X控制媒体文件的播放。组合使 用属性、事件和这两个方法，很容易创建一个定义的媒体播放器，如K面的例子所示。

16.3媒体元素	489
<div class=*mediaplayer,'>
<div class="video">
<video id="player" src="movie.mov" poster?mymovie.jpg" width="300" height="200">
Video player not available.
</video>
</div>
<div class=*controls">
<input type=*button" vaLue="Play" id=*video~btn">
<span idss"curcime">0</span>/<span id= "duration">0</span> </div>
</div>
VideoPlayerExampleOl .htfn
以上基本的HTML再加上一些JavaScript就可以变成一个简单的视频播放器。以下就是JavaScript 代码。
//取得元索的引用
var player = document.getElementById("player"), btn = docmnent.getElementById{"video-btn"), curtime = document.getElementById("curtime")/ duration = document.getElementById("duration*);
//更新播放时间
duration.innerHTML = player.duration;
//为按钮添加事件处理程序
EventUtil.addHandler(btn, "click", function(event){ if (player.paused){ player.play(J; btn.value = "Pause*;
} else {
player.pause(); btn.value = "Play"j
}
});
//定时更新当前时间 setlnterval< function{){
curtime.innerHTML = player.currentTime; 250);
VideoPlayerExampleOh him
以上JavaScript代码给按钮添加了一个事件处理程序，单击它能让视频在暂停时播放，在播放时暂 停。通过<vide〇>元素的load事件处理程序，设置了加载完视频后显示播放时间。最后，设置了一个 计时器，以更新当前显示的时间。你可以进一步扩展这个视频播放器，监听更多事件，利用更多属性。 而同样的代码也可以用于<aUdio>元素，以创建自定义的音频播放器。
16.3.4检测编解码器的支持情况
如前所述，并非所有浏览器都支持<vide〇：^<audi〇>.的所有编解码器，而这基本上就意味着你必 须提供多个媒体来源。不过，也有一个JavaScript API能够检测浏览器是否支持某种格式和编解码器。

490 第16章HTML5脚本编程
这两个媒体元索都宥一个canPlayTypeO方法，该方法接收一种格式/编解码器字符审，返冋 "probably"、"maybe"或""（空字符串）。空字符串是假值，W此可以像下面这样在if语句中使用 canPlayiype():
if (audio.canPlayType{"auaio/mpeg")){
//进一步处理
>
[fifwprot>ablyn和"maybe"都楚真值，因此在if语句的条件测试中〇]*以转换成true。
如果给canPlayTypeO传人r一种MIME类则返回值很可能是"maybe"或空字符串。这是W 为媒体文件本身只不过是荇频或视频的一个容器，而真|卜:决定文件能否播放的还是编码的格式。在同时 传人MIME类玴和编解码器的情况下，可能性就会增加，返W的字符串会变成"probably"。下面来看 几个例子。
var audio = document.getElementByldt"audio-player");
//很可能"maybe"
if (audio.canPlayType{"audio/mpeg")){
//进一步处理
}
// 可能是 * probably __
if (audio.canPlayType{"audio/ogg? codecs-\-vorbisX""))<
//进一步处理
}
注怠，编解码器必须用引号引起来才行。下表列出了已知的已得到支持的音频格式和编解码器。
当然，也可以使用canPlaylYpeO来检测视频格式。F表列出了已知的已得到支持的音频格式和 编解码器。
16.3.5 Audio 类型
<audi〇>元素还有一个原生的JavaScript构造函数Audio,可以在任何时候播放荇频。从同为DOM 元紊的角度肴，Audio与Image很相似，但Audio不用像Image那样必须插人到文档中。只要创建一

16.4历史状态管理 491
个新实例，并传人音频源文件即可。
var audio = new Audio ("sound.r\p3");
EventUtil.addHandler(audio, "canplaythrough", function(event){ audio.play();
)i;
创建新的Audio实例即可开始F载指定的文件。下载完成后，调用play (>就可以播放音频。
在iOS中，调用playu时会弹出一个对话框，得到用户的许可后才能播放声音。如果想在一段音 频播放后再播放另一段咅频，必须在onfinish亊件处理程序中调用play (>方法。
16.4历史状态管理
历史状态管理是现代Web应用开发中的一个难点。在现代Web应用中，用户的每次操作不一定会 打开--•个全新的页面，W此“后退”和“前进”按钮也就失去了作用，导致用户很难在不同状态间切换。 要解决这个问题，首选使用hashchange事件（第13章曾讨论过)。HTML5通过更新history对象为 管理历史状态提供了方便。
通过hashchange事件，可以知道URL的参数什么时候发生丫变化，即什么时候该有所反应。而 通过状态管理AP丨，能够在不加载新页面的情况下改变浏览器的URL。为此，箝要使用 histcry .pushState (>方法，该方法可以接收三个参数：状态对象、新状态的标题和可选的相对URL。 例如：
history.pushState({name："Nicholas">, "Nicholas' page", "nicholas.html");
执行pushStateO方法后，新的状态信息就会被加人历史状态栈，而浏览器地址栏也会变成新的 相对URI^但是，浏览器并不会真的向服务器发送请求，即使状态改变之后査询location.href也会 返冋与地址栏中相同的地址。另外，第二个参数目前还没有浏览器实现，因此完全可以只传人一个空字 符串，或者一个短标题也可以。而第-个参数则应该尽可能提供初始化页面状态所需的各种信息。
因为pushStateO会创建新的历史状态，所以你会发现“后退”按钮也能使用了。按下“后退” 按钮，会触发window对象的popstate事件p。popstate事件的來件对象有一个state M性，这个 属性就包含着当初以第一个参数传递给pushStateU的状态对象。
EventUtil.addHandler(window, "popstate"# function(event){ var state = event.state; if (state) {	//第一个页面加栽时state为空
processState(state);
}
);
得到这个状态对象后，必须把页而甩罝为状态对象屮的数据表示的状态（因为浏览器不会自动为你 做这拽)。记住，浏览器加载的第一个页面没有状态，因此单击“后退"按钮返回浏览器加载的第一个 页面时，event .state 值为 null。
要更新当前状态，n丨以调用repiaceState(>,传人的参数ijpushstate()的前两个参数相同。 调用这个方法不会在历史状态栈中创建新状态，只会重写3前状态。
①popstate事件发生后，亊件对象中的状态对象（event.state)是当前状态。
history.replaceState({name：*Greg"), "Greg's page-);
支持HTML5历史状态管理的浏览器有Firefox 4+、Safari 5+、Opera 11.5+和Chrome。在Safari和 Chrome中，传递给pushState()或replacestate()的状态对象中不能包含DOM兀素。而Firefox 支持在状态对象中包含DOM元素。Opera还支持一个history.state属性，它返回当前状态的状态 对象。
f
 在使用HTML5的状态管理机制时，请确保使用pushState()创造的每一个“假”
URL,在Web服务器上都有一个真的、实际存在的URL与之对应。否则，单击“刷 新”按钮会导致404错误。
###  16.5 小结
HTML5除了定义了新的标记规则，还定义了一些JavaScript API。这些API是为了让开发人员创建 出更好的、能够与桌面应用媲美的用户界面而设计的。本章讨论了如下API。
- [ ] 跨文挡消息传递API能够让我们在不降低同源策略安全性的前提下，在来自不同域的文档间传 递消息。
- [ ] 原生拖放功能让我们可以方便地指定某个元素可拖动，并在操作系统要放置时做出响应。还可 以创建自定义的可拖动元素及放置S标。
- [ ] 新的媒体元素<311虹〇>和^1扣〇>拥有自己的与音频和视频交互的API。并非所有浏览器支持所 有的媒体格式，因此应该使用canPlayType 〇检査浏览器是否支持特定的格式。
- [ ] 历史状态管理让我们不必卸载当前页面即可修改浏览器的历史状态栈。有了这种机制，用户就 可以通过“后退”和“前进”按钮在页面状态间切换，而这些状态完全由JavaScript进行控制。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter15.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter17.md)