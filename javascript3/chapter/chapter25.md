##  第25章 新兴的API([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 创建平滑的动
- [ ] 操作文件
- [ ] 使用Web Workers在后台执行JavaScript 
 
着HTML5的出现，面向未来Web应用的JavaScript API也得到了极大的发展。这些API没有 包含在HTML5规范中，而是各自有各C!的规范。但是，它们都属于“HTML5相关的API”。 本章介绍的所有API都在持续制定中，还没有完全固定下来。
无论如何，浏览器已经着手实现这些API,而Web应用开发人员也都开始使用它们了。读者应该能 够注意到，其中很多AP丨都带祚特定于浏览器的前缀，比如微软是ms,而Chrome和Safari是webkit。 通过添加这些前缀，不同的浏览器n丨以测试还在开发中的新API,不过请£住，去持前缀之后的部分在 所有浏览器中都是一致的。
reguestAnimationFrame()
很长时间以来，计时器和循环间隔一直都是JavaScript动画的最核心技术。虽然CSS变换及动W为 Web开发人员提供了实现动画的简单手段，但JavaScript动両开发领域的状况这些年来并没有大的变化。 Firefox 4最早为 JavaScript 动脚添加,一个新 API,即 mozRequestAnimationFrame ()。这个方法会 吿诉浏览器：有一个动W开始进而浏览器就可以确定重绘的S佳方式。
###  25.1.1 早期动画循环
在JavaScript中创建动両的典增方式，就是使用setinterval ()方法来控制所有动画。以下是一个 使用setlnterval (的基本动圃循环：
(functionO {
function updateAnimations(){ doAnimationl{); doAniraation2();
//其他动®
setlnterval{updateAnimations, 100);
为了创建—t'小泡动|B|库，updateAnimations 〇方法就得不断循环地运行每个动W，并相应地改 变不同元素的状态（例如，同时显示一个新闻跑马灯和一个进度条）。如果没有动画®要更新，这个方 法可以退出，什么也不用做，甚至可以把动IW丨循环停下来，等待下一次需要更新的动両。
编写这种动画循环的关键是要知道延迟时间多长合适。一方面，循环间隔必须足够短，这样才能让 不同的动両效果M得史平滑流畅;另一方面，循环间隔还要足够长，这样才能确保浏览器有能力渲染产 生的变化。大多数电脑显示器的刷新频率是60Hz,大概相当于每秒钟重绘60次。大多数浏览器都会对 重绘操作加以限制，不超过显示器的重绘频率，因为即使超过那个频率用户体验也不会有提升。
因此，坡平滑动両的最佳循环间隔是l〇〇〇ms/60,约等于17ms。以这个循环间隔重绘的动画是最平 滑的，因为这个速度最接近浏览器的最高限速。为了适应17ms的循环间隔，多重动_可能需要加以节 制，以便不会完成得太快。
虽然与使用多组setTimeout(的循环方式相比，使用setlnterval)的动画循环效率更高，伍 后者也不是没有问题。无论是setlnterval ()还是setTimeout (都不十分精确。为它们传入的第二 个参数，实际上只是指定了把动両代码添加到浏览器UI线程队列中以等待执行的时间。如果队列前面 已经加人了其他任务，那动画代码就要等前面的任务完成后再执行。简言之，以毫秒表示的延迟时间并 不代表到时候一定会执行动画代码，而仅代表到时候会把代码添加到任务队列中。如果UI线程繁忙， 比如忙于处理用户操作，那么即使把代码加人队列也不会立即执行。
###  25.1.2循环间隔的问题
知道什么时候绘制下一帧是保证动両平滑的关键。然而，直至最近，开发人员都没有办法确保浏览 器按时绘制下一帧。随狞£：31^33元素越来越流行，新的基于浏览器的游戏也开始崭餌头脚，面对不 十分格确的setlnt:erval (和setTimeout (),开发人员一筹莫展。
浏览器使用的计时器的稍度进一步恶化了问题。具体地说，浏览器使用的计时器并非稍确到毫_ 别。以下是几个浏览器的计时器精度。
1E8及更早版本的计时器精度为15.625ms。
□圧9及更晚版本的计时器精度为4ras。
Firefox和Safari的计时器精度大约为10ms。
Chrome的计时器精度为4ms。
IE9之前版本的计时器稍度为15.625ms,因此介于0和15之间的任何值只能是0和15。IE9把计时 器精度提高到了 4ms,但这个精度对于动阃来说仍然不够明确。Chrome的计时器精度为4ms,而Firefox 和Safari的精度是lOins。更为复杂的是，浏览器都开始限制后台标签页或不活动标签页的计时器。因此， 即使你优化了循环间隔，结果仍然只能接近你想要的效果。
mozReqtuestAnimationFrame
Mozilla的RobertO’Callahan认识到了这个问题，提出了一个非常独特的方案。他指出，CSS变换 和动画的优势在于浏览器知道动li什么时候开始，因此会计算出正确的循环间隔，在恰3的时候刷新 UI。而对于JavaScript动_，浏览器无从知晓什么时候开始。因此他的方案就是创造一个新方法 mozRequestAnimationFrame()，通过它ft■诉浏览器某些JavaScript代码将要执行动幽。这样浏览器 可以在运行某些代码后进行适当的优化。

684 第25章新兴的API
mozRequestAnimationFrame (方法接收--个参数，即在重绘屏幕前调用的一个闲数。这个函数 负责改变下一次重绘时的DOM样式。为广创建动W循环，可以像以前使用setTimeoutO—样，把多 个对mozReguestAnimationFrame ()的调用连缀起来。比如：
function updateProgress{){
var div = document.getElementById("status");
div.style.width = (parselnt(div.style.width, 10) +5) + "%*;
if (div.style.left 1= "100%"){
mozRequestAnimationFrame(updateProgross)?
}

mozRequestAni^iationFrame(updateProgress);
W为mozRequestAnimationFrame (只运行一次传人的函数，闵此在需要冉次修改UI从而生成 动画时，笛要冉次手工调用它。同样，也策要同时考虑什么时候停止动脚。这样就能得到非常平滑流畅 的动_。
口前来肴，mozRequestAnimaticmFjrameO解决了浏览器不知道JavaScript动画什么时候开始、 不知道最佳循环间隔时间的问题，但不知道代码到底什么时候执行的问题呢？同样的方案也可以解决这 个问题。
我们传递的mozRequestAnimationFrame ()涵数也会接收一个参数，它是一个时间码（从1970 年1月1F1起至今的毫秒数），表示下一次重绘的实际发生时间。注意，这一点很重要： mozRequestAnimationFrame )会根据这个时间码设定将来的某个时刻进行重绘，而根据这个时间 码，你也能知道那个时刻是什么时间。然后，再优化动闕效果就有f依据。
要知道距离上一次重绘已经过去/"多K;时间，nj"以査询mozAnimationStartTime,其中包含上一 次重绘的时间码。用传人回调确数的时间码减去这个时间码，就能汁算出在屏幕上重绘下一组变化之前 要经过多长时间。使用这个值的典铟方式如下：
function draw(timestamp){
"计算两次重绘的时间间塥
var diff = timestamp - starcTime;
//使用diff确定下一步的绘制时间
//把startTime重写为这一次的绘制时间 startTime = timestamp;
//重绘UI
mozRequestAr.imationFrame(draw);
var startTime = mozAniinationStarCTime; mozRequcstAnimationFrair.e (draw);
这里的关键是第一次读取mozAnimationStartTime的值,必须在传递给mozRequestAnimation Frame()的回调函数外面进行。如果足-在凹调函数内部读取mozAnimationStartTime，得到的值与传 人的时间码是相等的。
webkitReiuestAniinationFraine msRequestAnimationFreune
基于 mozRequestAnimationFrame () , Chrome 和 IE10+也都给出 r 自己的实现，分别叫 webkit- RequestAnimationFrame〇 不 11 msRequestAnimatzionFrame ( 〇这两个版本^ Mozilla的版本存两个 方面的微小差异。酋先，不会给回调函数传递时间码，因此你无法知道下一次柬绘将发生在什么时间。 其次，Chrome乂增加了第二个可选的参数，即将要发生变化的DOM元素。知道了:®绘将发生在页面中 哪个特定元素的区域内，就可以将重绘限定在该区域中。
既然没冇下一次重绘的时间码，那Chrome和IE没有提供mozAnimationStartTime的实现也就 很容易理解了一没有那个时间码，实现这个属性也没有什么用。不过，Chrome倒是乂提供了另一个 方法\^匕}^1：0311〇61如11^1：1〇1^^〇1116()，用于取消之前计划执行的重绘操作。
假如你木需要知道精确的时间差，那么吋以在Firefbx4+、IE10+和Chrome中可以参考以下模式创 建动Mi循环。
(function(){
function draw(timestamp){
//计算两次重绘的时间间隔
var drawStart - {timestamp II Date.now()), diff = drawStart - startTiinc;
//使用diff确定下一步的绘制时间
//把starcTime重写为这一次的绘制时间 startTime = drawStart;
//玄绘UI
requestAnimacionPrawe (draw);
var requestAnimationFrame = window. recjuestAnimationKrame | I
window.mozRequestAnimationFrame II window.webkitRequestAnimationFranie | I window.msRequestAnimat ionFrame, startTime = window.mozAnimationStartTime jI Date.nowO; requestAnimationFrame(draw);
}) 0;
以上模式利用已有的功能创迚了一个动脚循环，大致计算出了两次重绘的时间间隔。在Firefox中, 计算时间间隔使用的是既有的时间码，而在Chrome和IE中，则使用不卜分稍确的Date对象。这个模 式可以大致体现出两次重绘的时间间隔，但不会吿诉你在Chrome和IE中的时间M隔到底是多少。不过, 大致知进时间间隔总比一点儿概念也没有好些。
因为首先检测的是标准函数名，其次才是特定于浏览器的版本，所以这个动酬循环在将来也能够 使用。
fi 前，W3C 已经着手起草 requestAnimationFrame ( API，而且作为 Web Performance Group 的 一部分，Mozilla和Google正共同参与该标准草案的制定:1:作。





686 第25章新兴的API
Page Visibility API
不知道用户足不是正在与贞面交互，这是困扰广大Web开发人员的一个主要问题。如果页面最小 化了或者隐藏在了其他标签页后面，那么有些功能是可以停下来的，比如轮询服务器或者某些动画效果。 而Page Visibility AP丨（页面可见性API)就是为丫ih开发人员知道页面是否对用户可见而推出的。
这个AP丨本身非常简单，由以下三部分组成。
口 document.hidden:表示页面是否隐藏的布尔值。页面隐藏包括页面在后台标签页中或者浏览 器最小化。
口 document.visibilityState:表示下列4个可能状态的值。
■页面在后台标签页中或浏览器最小化。
■页面祚前台标签页中。
■实际的页面已经隐藏，但用户可以看到贞曲的预览（就像在Windows 7中，用户把鼠标移动到 任务栏的图标上，就以敁示浏览器中3前页面的预览)。
■页面在屏幕外执行预渲染处理。
□ visibilitychange事件：，文档从可见变为不可见或从不可见变为可见时，触发该韦件。 在编写本书时，只有IE10和Chrome支持Page Visibility API。IE的版本是在每个屑性或亊件前面加 上ms前缀，rflj Chrome则是加上webkit前缀。因此document.hidcien在丨E的实现中就是 document.msHidden,而在Chrome的实现中则足document.webkitHidden。检査浏览器是否支持 这个API的最佳方式如下：
类似地，使用同样的模式可以检测页面是否隐藏：
if (document.hidden I I document.msHidden II document.webKitHidden){ //页面隐藏了 } else {
//页面未隨藏
注意，以上代码在不支持该AP丨的浏览器中会提示页面未隐藏。这是Page Visibility AP丨有意设计的 结果，目的是为了向后兼容。
为了在贝面从可见变为不可见或从不可见变为可见时收到通知，可以侦听visibilitychange亊 件。在丨E中，这个亊件叫rosvisibilitychange,而在Chrome中这个事件叫webkitvisibility- change。为了在网个浏览器中都能侦听到该事件，可以像下面的例子一样，为每个事件都指定相同的 事件处理程序：



function isHiddenSupported(){
return typeof (document.hidden I I document.msHidden I document.webkitHidden} != ■undefined";



PageVisibilityAPIExampleOl. htm

25.3 Geolocation API	687
function handleVisibilityChange(){
var output = document.getEJementById("output"), msg;
if (document.hidden I I document.msHidden I I docuroenc.webkitHidden){ msg "Page is now hidden. " + (new DateO) + Mbr";
} else (
ir.sg = "Page is now visible. " + (new Date ()) + "br" ;	*

output.innerHTML += msg;
)
//要为两个事件都指定事件处理程序
EventUtil.addHandler(dociment, "msvisibilitychange*, handleVisibilityChange); Eventutil.addHandler(document, "webkitvisibilitychange", handleVisibilityChange);
Page Visi bilityAPIExampleOl. htm
以上代码同时适用于IE和Chrome。而且，API的这一部分已经相对稳定，因此在实际的Web开发 中也可以使用以上代码。
关于这一API的实现，差异■最大的是document.visibilityState属性。IE〗0 PR 2的 document.msVisibilityState娃一个表示如下4种状态的数字值。
document.MS„PAGE_HIDDEK (0)
document.MS_PAGE_VISIBLE (1)
document.MS_PAGE_PREVIEW (2)
document.MS_PAGE_PRERENDER(3)
在 Chrome 中，documenL.wei)kit;VisibilityState 可能是卜列 3 个字符串值：
"hidden*
"visible"
M prerender *
Chrome并没有给每个状态定义对应的常M，伹最终的实现很可能会使用常量。
由于存在以丨•-差好，所以建议大家先不要完全依赖带前缀的document.visibilityState,最好 只使用 document: .hidden 属性。
Geolocation API
地理定位（geolocation)是最令人兴奋，而且得到了广泛支持的-个新API。通过这套API, JavaScript 代码能够访问到用户的当前位置信息。当然，访问之前必须得到用户的明确许可，即间意在页面中共享 其位S信息。如采贞面尝试访问地理定位信E,浏览器就会显示一个对话框，请求用户许可共享其位置 信息。阐25-1展示了 Chrome中的这样■•个对话框。
-	〇 dfve_nto_6	h附	☆、
图 25-I
Geolocation API在浏览器中的实现是navigator.geolocation对象，这个对象包含3个方法。 第_-个方法是961：0^^加？03：11：1〇11(，调用这个方法就会触发猜求用户共享地理定位信息的对话框;

688 第25章新兴的API
这个方法接收3个参数：成功W调函数、可选的失败回调函数和可选的选项对象。
其中，成功W调函数会接收到一个Position对象参数，该对象有两个属性:coords和timestamp。 而coords对象中将包含K列与位置相关的信息。
latitude:以十进制度数表示的纬度。
口 longitude:以十进制度数表示的经度。
accuracy:经、纬度坐标的精度，以米为单位。
有些浏览器还可能会在coords对象中提供如下属性。
altitude:以米为单位的海拔高度，如果没有相关数据则值为null。
altitudeAccuracy:海拔高度的粮度，以米为单位，数值越大越+精确。
heading:指南针的方向，0。表示正北，值为NaN表示没有检测到数据。
speed:速度，即每秒移动多少米，如果没有相关数据则值为null。
在实际开发中，latitude和longitude是大多数Web应用最常用到的属性。例如，以下代码将 在地阁上绘制用户的位置：
navigator.geolocation.getCurrentPosition{function(position){
drawMapCenteredAt{position.coords.latitude, positions.coords.longitude};
});
以上介绍的是成功冋调函数。getCurrentPosition()的第二个参数，即失败Is]调函数，在被调 用的时候也会接收到一个参数。这个参数是一个对象，包含两个属性:message和code。其中，message 属性中保存着给人看的文本消息，解释为什么会出错，时code属性中保存着一个数值，表示错误的类 型：用户拒绝共亨（1)、位置无效（2)或者超时（3)。实际开发中，大多数Web应用只会将错误消息 保存到日志文件中，而不一定会因此修改用户界面。例如：
navigator.geolocation.getCurrentPosition(function(position){
drawMapCenteredAt(position.coords.latitude, positions.coords.longitude);
}, function(error){
console.log('r2rror codes n error.code); coQSole.logC^Errox* messages " + error.message);
));
getCurrentPosiLi〇n()的第三个参数是一个选项对象，用于设定信息的类型。可以设置的选项 有三个：enableHighAccuracy是一个布尔值，表示必须尽可能使用最准确的位置信息;timeout是 以毫秒数表示的等待位置信息的最长时间;maxiimmAge表示t一次取得的坐标信息的有效时间，以毫 秒表示，如果时间到则重新取得新坐标信息。例如：
navigator.geolocation.getCurrentPosition(function(position){
drawKapCenteredAt(position.coords.latitude, positions.coords.longitude); }, £unction(error){
console.log("Error codes " + error.code); console•log(_Error message: " + error.message);
{
enableHighAccuracy： true, timeouti 5000, maxlmusiAge： 25000
);

25.4 File API 689
这三个选项都是可选的，可以单独设置，也可以与其他选项一起设置。除非确实需要非常精确的信 总，否则建议保持enableHighAccuracy的false值（默认值)。将这个选项设置为true需要更长 的时候，而且在移动设备上还会导致消耗更多电ffi。类似地，如果不X要频繁更新用户的位置信息，那 么可以将maximumAge设置为Infinity,从而始终都使用上一次的坐标信息。
如果你希望跟踪用户的位置，那么可以使用另一个方法watchPositionU。这个方法接收的参数 与getCurrentPosition(方法完全相同。实际上，wat:chPosition()与定时调用 getCurrentPosition()的效果相同。在第一次调用watchPositionO方法后，会取得.当前位置，执 行成功回调或者错误回调。然后，watchPositionU就地等待系统发出位置已改变的信号（它不会A 己轮询位置）。
调用watchPositionU会返M—个数值标识符，用于跟踪监控的操作。基于这个返丨"1值可以取消 监控操作，只要将其传递给clearWatch()方法即可（与使用setTimeoutO和clearTimeout()类 似)。例如：
var watchld = navigator.geolocation.watchPosition(function(position){
drawMapCenteredAt(position.coords.latitude, positions.coords.longitude); }, function(error){
console. l〇ff(MError code: M ■¥ error .code}; console«l〇9("Error message: H + error.message);
);
clsarWatch(watchld);
以上例子调用了 watchPositionO方法，将返回的标识符保存在了 watchld中。然后，又将 watchld传给■TclearWatchO,取消了监控操作。
支持地理定位的浏览器冇丨E9+、Firefox 3.5+、Opera 10.6+、Safari 5+、Chrome、iOS 版 Safkri、Android 版WebKit。要了解使用地理定位的吏多精彩范例，请访问

http://html5demos.com/geo。
File API
不能直接访问用户il•算机中的文件，一fl都是Web应用开发中的一大障码。2000年以前，处理文 件的唯一方式就是在表单中加人input type="file"字段，仅此而已。FileAPI (文件API)的宗旨 是为Web开发人员提供一种安全的方式，以便在客户端访问用户计算机中的文件，并更好地对这些文 件执行操作。支持File AP丨的浏览器有IE 10+、Firefox 4+、Safari 5.0.5+、Opera丨丨.1+和Chrome。
File API在表单中的文件输人字段的基础h, 乂添加了一些直接访问文件信息的接U。HTML5在 DOM中为文件输人元素添加了一个files集合。在通过文件输人字段选择了一或多个文件时，files 集合中将包含一组File对象，每个File对象对应着个文件。每个File对象都有下列只读属性。
name:本地文件系统中的文件名。
size:文件的宁节大小。
type:字符串，文件的MIME类型。
lastModifiedDate:字符串，文件上一次被修改的时间（只有Chrome实现了这个属性)。
举个例子，通过侦听change事件并读取files集合就可以知道选择的每个文件的信息：
25
〇
var filesList = document.getElementById("files-list");
EventUtil.addHandler(filesList, "change", function{event){

690 第25章新兴的API
var files = EventUtil.getTarget(event).files,
i = 0,
len = files.length; while (i  len){
console.log(filesti].name + ™ {" + files[i].type +	+ files[i].size +
"bytes)");
}
}) ?
FileAPIExampleOJ.htm
这个例子把每个文件的信息输出到了控制台中。仅仅这一项功能，对Web应用开发来说就已经是 非常大的进步了。不过，File AP丨的功能还不止于此，通过它提供的FileReader类型甚至还可以读取 文件中的数据。
25.4.1 FileReader 类型
FileReader类纽实现的是•种异步文件读取机制。可以把FileReader想象成XMLHttpRequest， 区别只是它读取的是文件系统，而不是远程服务器。为了读取文件中的数据，FileReader提供了如下 几个方法。
UreadAsText( file, encoding):以纯文本形式读取文件，将读取到的文本保存在result属 性中。第二个参数用于指定编码类型，是可选的。
readAsDataURL ( fiJe):读取文件并将文件以数据URI的形式保存在result属性中。
readAsBinaryString (file):读取文件并将一个字符串保存在result属性中，字符串中的 每个字符表示一字节。
readAsArrayBuffer(fi2e :读取文件并将一个包含文件内容的ArrayBuffer保存在 result属性中。
这些读取文件的方法为灵活地处理文件数据提供了极人便利。例如，可以读取图像文件并将其保存 为数据URI,以便将其®示给用户，或者为了解析方便，可以将文件读取为文本形式。
由于读取过程是异步的，因此FileReader也提供了几个事件。其中最有用的三个事件是 progress、error和load,分別表示是否又读取f新数据、是否发生了错误以及是否已经读完了整个 文件。
每过50ros左右，就会触发一次progress事件，通过事件对象可以获得与XHR的progress事 件相同的信息（属性）：lengthComputable、loaded和total。另外，尽管可能没有包含全部数据， 但每次progress事件中都以通过FileReader的result属性读取到文件内容。
由于种种原因无法读取文件，就会触发error事件。触发error事件时，相关的信息将保存到 FileReader•的error属性中。这个属性中将保存一个对象，该对象只有一个属性code,即错误码。 这个错误码是1表示未找到文件，是2表示安全性错误，是3表示读取中断，是4表示文件不可读，是 5表示编码错误。
文件成功加载后会触发load事件;如果发生了 error事件，就不会发生load事件。以下是一个 使用上述个事件的例+。

25.4 File API	691



var filesLisc - document.getElementById("files-list")?
EventUcil.addHandler(filesList# "change", function(event){
var info = •n•
output = document.getElementById("output"), progress = document.getElementByldf"progress*), files = EventUtil.getTarget(event).files, type = "default *, reader = new FileReader(};
if {/image/.test(files[0).type)){ reader.readAsDataURL(files[0]); type = ”image";
} else {
reader.readAsTexc(files(0]); type = "text";
}
reader.onerror = function。{
output.innerHTML = "Could not read file, error code is " +
reader.error.code;
};
reader.onprogress = function(event){ if (event.lengthComputable){
progress.innerHTML = event.loaded + ”/• + event.total;
}
};
reader.onload = function(){
var html = ”";
switch(type){
case "image■：
hcml = *img src=\"* + reader.result ♦	?
break;
case *text":
htnl = reader.result; break;

output.innerHTML = html;
;
});
FikAPIExample02.htm
这个例子读取了表单字段中选择的文件，并将其内容M示在了页面中。如果文件有MIMI类型，表 示文件是图像，因此在load事件中就把它保存为数据URI,并在页面中将这幅图像显示出来。如果文 件不是图像，则以字符串形式读取文件内容，然后如实在页面中ii示读取到的内容。这里使用了 progress亊件来跟踪读取了多少字节的数据，而error事件则用于监控发生的错误。
如果想中断读取过程，可以调用abort 〇方法，这样就会触发abort亊件。在触发load、error 或abort事件后，会触发另一个事件loadend。loadend事件发生就意味着已经读取完整个文件，或 者读取时发生f错误，或者读取过程被中断。
实现File API的所有浏览器都支持readAsText ()和readAsDataURL (方法。但IE10 PR 2并未 实现 readAsBinaryString()和 readAsArrayBuffer (方法。
25

692 第25章新兴的API
25.4.2读取部分内容
有时候，我们只想读取文件的-部分而不足全部内容。为此，File对象还支持••个slice (方法， 这个方法在Firefox中的实现叫mozSlice ),在Chrome中的实现叫webkitSlice ( , Safari的5.1及 之前版本+支持这个方法。slice )方法接收两个参数：起始字节及要读取的字节数。这个方法返冋一 个Blob的实例,Blob足File类型的父类型。F面足一个通用的函数，可以在不N实现中使用slice 〇 方法：
function blobSlice{blob/ startByco, length){ if {blob.slice){
return blob.slice{st:artByte, length);
} else if (blob.webkitSlice){
return blob.webkitSlice{startByte, length);  else if (blob.mozSlice){
return blob.mozSlice^startByte, length);
} else {
return null;
FUeAPIExample03.htm
Blob类型备一个size属性和一个type属性，而FL它也支持slice()方法，以便进一步切割数 据。通过FileReader也可以从Blob中读取数据。下面这个例子只读取文件的32B内容。
var filesList = document.getElomontByld("files-1ist");
EventUtil.addHandler(filesList, "change", function(event){ var info = ""#
output = document.getFlementById{"output"), progress = document.getElementById("progress")# files = EventUtil.getTarget(event).files, reader = new FilcReader{)# blob = blobSlicetflies[0], 0, 32);
if (blob){
reader.readAsText(blob);
reader.onerror = function(){
output.inncrHTML = "Could not read file, error code is " +•
reader.error.code;
reader.onload = function(){
output.innerHTML = reader.result;
};
} else {
alert("Vour browser doesn' t support si ice O.h);
}
});
FVeAPIExampie03.htm
只读取文件的--部分可以节省时间，非常适合只关注数据中某个特定部分（如文件头部）的情况。

对象 URL
25.4 File API 693
对象URL也被称为blob URL,指的是引用保存在F'ile或Blob中数据的URL。使用对象URL的 好处是可以不必把文件内容读取到JavaScript巾而直接使用文件内容。为此，只要在需要文件内容的地 方提供对象URL即可。要创建对象URL,可以使用window.URL.createObjectURL()方法，并传人 File 或 Blob 对象。这个方法在 Chrome 中的实现叫 window.webkitURL.createObjectURL (),因 此可以通过如下函数來消除命名的差异：
function createObjectURL(blob) if {window.URL){
return window.URL.createObjectURL(blob);
} else if (window.webkitURL){
return window.webkitURL.createObjectURL(blob); } else {
return null?
}
FileAPIExampk04.htm
这个函数的返M值是-个字符串，指向一块内存的地址。因为这个字符串是URL,所以在DOM中 也能使用。例如，以下代码可以在页面中敁示一个阁像文件：



var £ilesList = document.geLElGmcntByld{'files-list");
EventUtil.addHandler(filesList, "change*, function(event){
var info s ■",
output = document.getElementById("output"),
progress = document.getElemenCById("progress#
files = EventUtil.getTarget(event).files,
reader = new FileReader{),
url = create〇bjectURL(£lles[0]);
if (urlH
if (/image/.test(files【OJ.type”{
output.innerHTHL • "img Brc=\"" + url *■ w\""/
else {
output.innerHTML = "Not an image.";

} else {
output.innerHTML = 'Your browser doesn't support object URLs.";
}
});
FileAPIExample04,htm
直接把对象URL放在img标签中，就省i-了把数据先读到JavaScript中的麻烦。另一方面，img 标签则会找到相应的内存地址，直接读取数据并将阁像显示在页面中。
如果不再需要相应的数据，最好释放它占用的内容。但只要有代码在引用对象URL,内存就不会释 放。要手.丨.释放内存，可以把对象URL传给window.URL.revokeOjbectURLU (在Chrome中是 window.webkitURL.revokeObjectURL())。要兼容这两种方法的实现，可以使用以下函数：
25

694 第25幸新兴的API
function revokeObjeccURL(url){ if (window.URL1{
window.URL.revokeObjectURL(url);
} else if (window.webkitURL){
window.webki tURL.revokeObj ectURL(url);

)
页面卸载时会自动释放对象URL占用的内存。不过，为了确保尽可能少地占用内存，最好在不需 要某个对象URL时，就马上手工释放其占用的内存。
支持对象URL的浏览器有IE10+、Firefox 4和Chrome。
25.4.4读取拖放的文件
围绕读取文件信息，結合使用HTML5拖放API和文件API,能够创造出令人瞩Q的用户界面：在页 面上创逮了自定义的放标之后，你可以从桌面上把文件拖放到该M标。与拖放一张阁片或者一个链接 类似，从桌面上把文件拖放到浏览器屮也会触发drop來件。而R可以在event .dataTiransfer. files 中读取到被放置的文件，当然此时它是一个File对象，与通过文件输人字段取得的File对象-样。
下面这个例子会将放置到页面中自定义的放®g标中的文件信息显示出来：
var droptarget - document.getHlementById( "droptargec");
function handleEvent(event){ var info = ■_•
output = document.getElementByld("output"), files, i, len;
EventUtil.preventDefault(event);
if (event.type == "drop"){
filea - event.dataTranB£er.£ilea;
i = 0;
len = files.length; while (i  len){
info += files【i] .name + " (" + files[i] .type +*,'' + files^i] .size + "bytes)br"?
i++;
output. innerHTMI. = info;
EventUtil.addHandler(droptarget, "dragenter", handleEvent)?
EventUcil.addHandler{droptarget# *dragover", handleEvent);
KventUtil.addHandler{dropLarget, "drop"; handleEvent);
FileAPIExample05.htm
与之前展不的拖放示例一样，这里也必须取消dragenter、dragover和drop的默认行为。在 drop事件中，口1以通过event.dataTransfer.files读取文件信息。还有-•种利用这个功能的流行 做法，即结合XMLHttpRequest和拖放文件来实现上传。

25.4.5使用XHR上传文件
25.4 File API	695
通过File API能够访问到文件内容，利用这一点就可以通过XHR直接把文件上传到服务器。芍然 啦，把文件内容放到send ()方法中，再通过POST请求，的确很容易就能实现上传。但这样做传递的 是文件内容，因而服务器端必须收集提交的内容，然后W把它们保存到另一个文件中。其实，更好的做 法是以表单提交的方式来h传文件。
这样使用FormData类型就很容易做到/*(第21章介绍过FormData )。首先,要创建一个FormData 对象，通过它调用appemi()方法并传人相应的File Xt象作为参数。然后，再把FormData对象传递 给XHR的send ()方法，结果与通过表单上传一模一样。
vac droptarget = document.getElemontByldt"droptarget");
function handleEvent(event){ var info = ""#
output = document.getElementById("output■)/ data; xhr, files, i, len?
EventUtil.preventDefault(event);
if (event.type == "drop” { data s new FormData(); files = event.dataTrans fer.files;
i = 0?
len = files.length; while (i  len){
data.append("file" + i, filesCi]); i++;
xhr = new XMLHttpRequest();
xhr .open("post", "FileAPIExajnple06Upload.phpB, true); xhr.onreadystatechange = function(){ if {xhr.readyState	4) {
alert(xhr.responseText);
}
};
xhr•send(dateO ;
EventUtil.addKandler{droptarget, "dragenter1*, handleEvent); EventUtil.addHandler(droptarget, "dragover•# handleEvent); EventUtil.addHandler(droptarget, "drop", handleEvent);
FileAPIExample06.htm
这个例了•创建一个FormDatia对象，4毎个文件对应的键分別是fiie〇、fiiei、fiie2这样的格 式。注意，不用额外写任何代码，这些文件就吋以作为表单的值提交。而且，也不必使用FileReader, 只要传入File对象即可。
25

696 第25章新兴的API
使用FormData上传文件，在服务器端就好像是接收到了常规的表单数据一样，-切按部就班地处 理即可。换句话说，如果服务器端使用的是PHP,那么$_FILES数组中就会保存着上传的文件。支持 以这种方式上传文件的浏览器有Firefox 4+、Safari 5+和Chrome。
Web 计时
页面性能一直都是Web开发人员最关注的领域。但直到最近，度童页面性能指标的唯一方式，就 是提高代码复杂程度和巧妙地使用JavaScript的Date对象。Web Timing AP丨改变/这个局面，让开发 人员通过JavaScript就能使用浏览器内部的度K结果，通过直接读取这些信息可以做任何想做的分析。 与本章介绍过的其他API不同，Web Timing API实际上已经成为了 W3C的建议标准，只不过目前支持 它的浏览器还+够多。
Web计时机制的核心是window.performance对象。对页面的所有度量信息，包括那些规范中已 经定义的和将来才能确定的，都包含在这个对象里面。Web Timing规范一开始就为performance对象 定义了两个属性。
其中，performance .navigation属性也是一个对象，包含着与页面导航有关的多个属性，如下所示。
redirectCount:页面加载前的電定向次数。
type:数值常请，表示刚刚发生的导航类沏。
performance.navigation.TYPE_NAVIGATE 0):页面第一次加载。
performance.navigation.TYPE_RELOAD 1:页面重载过。
performance.navigation.TYPE_BACK_FORWARD (2):页面是通过“后退”或“前进”按 钮打开的。
另外，performance .timing属性也是一个对象，但这个对象的属性都是时间截（从软件纪元开 始经过的毫秒数），不同的事件会产生不同的时间值。这些属性如下所示。
navigat_ionStart:开始导航到S前页面的时间。
unloadEventStart:前-个页面的unload事件开始的时间。但只有在前一个页面与当前页 面来自同一个域时这个属性才会有值;否则，值为0。
unloadKventEnd:前一个页面的unload亊件结束的时间。但只有在前一个页面与当前页面 来自同一个域时这个属性才会有值;否则，值为0。
reciirectstart:到当前页面的重定向开始的时间。但只有在重定向的觅面来g同一个域时这 个厲性才会有值;否则，值为0。
口 redirectEnd:到当前页面的重定向结束的时间。但只有在重定向的页面来自同一个域时这个 属性才会有值;否则，值为0。
?61;1：1131&1"1：:开始通过1171?0£1'取得页面的时间。
domainLookupStart:开始査询当前页面DNS的时间。
口 domainLookupEnd:査询当前页面DNS结束的时间。
comiectStart:浏览器尝试连接服务器的时间。
connectEnd:浏览器成功连接到服务器的时间。
secureConnectionStart:浏览器尝试以SSL方式连接服务器的时间„不使用SSL方式连接 时，这个属性的值为0。

25.6 Web Workers 697
requestStart:浏览器开始请求K面的时间。
responseStart:浏览器接收到页面第一字节的时间。
responseEnd:浏览器接收到贞面所有内容的时间。
domLoading: document.readyState 变为11 loading"的日^间〇
 domlnteractive： document.readyState	inceractive"fllJII*tfy〇
domContentLoadedEventStart：发生：)OMCor-tentLoaded 事件的时间〇
domContentLoadedEventEnd: DOMContentLoaded事件LI经发生且执行完所有事件处理程 序的时间。
口 domComplete: docuxnent.readyState 变为"complete"的时间〇
loadEventStart:发生load事件的时间。
loadEventEnd: load亊件已经发牛fl执行完所有車件处理程序的时间。
通过这些时间值，就可以全面了解页面在被加载到浏览器的过程中都经历了哪些阶段，而哪些阶段 口丁能是影晌性能的瓶颈。给大家推荐一个使用Web Timing API的绝好示例，地址是 

http://webtimingdemo.appspot.com/。
支持Web Timing API的浏览器冇丨E10+和Chrome。
Web Workers
随着Web应用复杂性的与日俱增，越来越复杂的汁算在所难免。长时间运行的JavaScript进程会导 致浏览器冻结用户界面，让人感觉屏幕“冻结” 了。Web Workers规范通过让JavaScript在后台运行解决 了这个问题。浏览器实现Web Workers规范的方式存很多种，可以使用线程、后台进程或者运行在其他 处理器核心h的进程，等等。具体的实现细节其实没有那么承要，重要的是开发人员现在可以放心地运 行JavaScript,而不必担心会影响用户体验J*。
S 前支持 Web Workers 的浏览器有 IE10+、Firefox3.5+、Safari 4+、Opera 10.6+、Chrome 和 iOS 版 的 Safari。
使用 Worker
实例化Worker对象并传人要执行的JavaScript文件名就可以创建一个新的Web Worker。例如：
var worker = new Worker("stufftodo.jsM);
这行代码会导致浏览器下载stuffcodo. js,但只有Workei•接收到消息才会实际执行文件中的代 码。要给Worker传递消息，可以使用postMessage()方法（与XDM中的postMessage()方法类似）: worker.postMossage( “start!");
消息内容可以是任何能够被序列化的值，不过与XDM不间的是，在所有支持的浏览器中， postMessage (都能接收对象参数（Safari 4是支持Web Workers的浏览器中最后' •个只支持字符串参 数的)。因此，可以随便传递任何形式的对象数据，如下面的例子所示：
worker.postMessage({ type： "command"# message： "start.! ■
);
25

698 第25章新兴的API
一般来说，可以序列化为JSON结构的任何值都可以作为参数传递给postMessage (。换句话说, 这就意味着传人的值是被复制到Worker中，而非直接传过去的（liXDM类似)。
Worker是通过message和error事件与页面通信的。这里的message +事件与XDM中的message 亊件行为相N,来A Worker的数据保存在event.data中。Worker返回的数据也可以是任何能够被序 列化的值：
worker.omnessage = function(ovent) { var data = event.data;
//对数描进行处理
}
Worker不能完成给定的任务时会触发error事件。具体来说，Worker内部的JavaScript在执行过 程中只要遇到错误，就会触发error事件。发生error事件时，事件对象中包含三个属性：filename、 lineno和message,分别表示发生错误的文件名、代码行号和完整的错误消息。
worker.onerror = function(event){
console.log("ERROR： n + event.filename + ■ (_ + event.lineno 十"}: " + event.message);
);
建议大家在使用Web Workers时，始终都要使用onerror事件处理程序，即使这个函数（像上面 例子所示的）除Y把错误记录到日志中什么也不做都可以。丙则，Worker就会在发生错误时，悄无声息 地失败了。
任何时候，只要调用terminate (方法就柯以停止Worker的工作。而且，Worker中的代码会立即 停止执行，后续的所有过程都不会再发生（包括error和message事件也不会再触发)。
worker. terminate () ?	//立即停止 Worker 的■!■作
Worker全局作用域
关于Web Worker，最重要的是要知道它所执行的JavaScript代码完全在另一个作用域中，与当前网 页中的代码不共亨作用域。在Web Worker中，N样有一个全局对象和其他对象以及方法。但是，Web Worker中的代码不能访问DOM,也无法通过任何方式影响页面的外观。
Web Worker中的全M对象是worker对象本身。也就是说，在这个特殊的全局作用域中，this和 self引用的都是worker对象。为便于处理数据，Web Worker本身也是一个最小化的运行环境。
□最小化的 navigator 对象，包括 onLine、appNaine、appVersion、userAgent 和 platform
属性;
□只读的location对象;
□ setTimeout()、setlnterval()、clearTimeout()和 clear Interval。方法;
口 XMLHttpRequest 构造闲数。
显然，Web Worker的运行环境与贞面环境相比，功能是相当有限的。
3页面在worker对象上调用postMessage ()时，数据会以异步方式被传递给worker,进而触发 worker中的message事件。为了处理来灼页面的数据，同样也需要创建一个orxmessage事件处理 程序。

25.6 Web Workers 699
//Web Worker内部的代码 self.onmessage = function(event){ var data = event-data;
//处理数槐
};
大家肴清楚，这里的self引用的是Worker全局作用域中的worker对象（与觅面中的Worker对 象不同一个对象)。Worker完成T.作后，通过调用postMessageO可以把数据冉发冋页面。例如，下 面的例子假设需要Worker 传人的数组进行排序，而Worker在排序之后又将数组发回了页面：



//Web Worker■内部的代码
self.onmessage = function(event){
var data = event.data;
//别忘了，默认的sort ()方法只比较字符串 data.sort(function(a, b){ return a - b;
;
self.postMessage(dat&;
Web WorkerExampleOl.js
传递消息就是贞面与Worker相互之间通信的方式。在Worker中调用postMessage()会以异步方 式触发页面中Worker实例的message亊件。如果页面想要使用这个Worker,可以这样：
//在页面中
var data = [23,4,7,9,2,14,6,651,87,41,7798,24], worker « new Worker("WebWorkerExampleOl.js*);
worker.onmessage ， function(event){ var data = event.data;
//对排序后的数组进行搮作
//将数组发送给worker排年 worker.postMessage(data);
Web WorkerExampleOI.htm
排序的确是比较消耗时间的操作，闪此转交给Worker做就不会阻塞用户界面了。另外，把彩色图 像转换成灰阶阁像以及加密解密之类的操作也是相当费时的。
在Worker内部，调用close ()方法也+nj以停止丁作。就像在页面中调用terminate 〇方法--样， Worker停止工作后就不会再有事件发生了。
//Web Worker内部的代码 self .closeO ;
25.6.3包含其他脚本
25
既然无法在Worker中动态创建新的£^1^口1:元素，那是不是就不能向Worker中添加其他脚本了

700 第25章新兴的API
呢？不是，Worker的全局作用域提供这个功能，即我们可以调用iroportScriptsO方法。这个方法接
收-个或多个指向JavaScript文件的URL。毎个加栽过程都是异步进行的，W此所有脚本加载并执行之
后，importScripts(才会执行。例如：
//Web Worker内部的代码
i.mportScripts (" f ilel. js" # Bfilo2 . js");
即使file2.js先于filel.js下载完，执彳？的时候仍然会按照先后顺序执行。（fif且，这些脚本是 在Worker的令局作用域中执行，如果脚本中包含与页时冇关的JavaScript代码，那么脚本可能无法正确 运行。请记住，Worker中的脚木-般都具冇特殊的用途，不会像页面中的脚本那么功能宽泛。
Web Workers 的未来
Web Workers规范还在继续制定和改进之屮。本节所讨论的Worker U前被称为“专用Worker” (dedicatedworker), W为它们是专门为某个特定的页面服务的，+能在页面间共享。该规范的另外•个 概念是“共亨Worker”（shared worker),这种Worker可以在浏览器的多个标签中打开的同一个页面间 共享。虽然Safari 5、Chrome和Opera丨0.6都实现了共亨Worker,但由丁•该规范尚未完稿，因此很可能 还会打变动。
另外，关于在Worker内部能访问什么不能访问什么，到如今仍然争论不休。冇人认为Worker应该 像页面一样能够访问任意数据，不光是XHR,还有localStroage、sessionStorage、Indexed DB、 Web Sockets、Server-Send Events等。好像支持这个观点的人史'多一fi,因此未来的Worker全局作用域 很吋能会存更大的空间。
###  25.7 小结
与HTML5问时兴起的是另外批JavaScript API。从技术规范角度讲，这批API不®于HTML5, 但从整体〖:吋以称它们为HTML5 JavaScript API。这些API的标准有不少虽然还在制定当中，但已经得 到了浏览器的广泛支持，因此本章觅点时论了它们。
re?uestAnimationFrame():是个若眼•优化JavaScript动画的API,能够在动画运行期间 发出信号。通过这种机制，浏览器就能够G动优化屏幕重绘操作。
- [ ]  Page Visibility API:让开发人员知道用户什么时候正在肴着页面，而什么时候页面是隐藏的。
- [ ]  Gedocation API:在得到许可的情况下，可以确定用户所在的位置^在移动Web应用中，这个 API非常重要而且常用。  

File API:坷以读取文件内容，用于显示、处理和上传。与HTML5的拖放功能结合，很容易就 能创造出拖放I:传功能。
Web Timing:给出了贞面加载和渲染过程的很多信息，对性能优化非常有价值。
Web Workers:可以运行异步JavaScript代码，避免阻塞州户界面。在执行复杂计笄和数据处理 的时候，这个API非常有用;要不然，这些任务轻则会占用很长时间，重则会导致用户无法与 页面交互。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter24.md)&emsp;&emsp;[下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter26.md)