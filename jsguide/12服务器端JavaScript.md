# 第12章 服务器端JavaScript  
前面的章节已经详细介绍了JavaScripi语言核心。我们即将开始本书的第一部分，该部分
会介绍JavaScript嵌人Web浏览器的原理，并袖盖庞杂的客户端JavaScript API。可以说
JavaScript是基于Web的编程语言，因为绝大部分JavaScript代码悬为Web浏览器而编写。
但是作为一]高效和通用的语言，JavaScript理所当然能用丁其他编程丁作。所以在过
渡到肢务端JavaScript之前，我们先快速了解一下另外两种JavaScript嵌入。Rhino是基丁
Java的JavaScripl解析器。实现了通过JavaScript程序访问整个Java AP],12.1节将会介绍
它。NodeFi'是Goog1e的V8 JavaScript解析器的一个特别版本，它在底层绑定了POSIX
(Unix) API,包括文件、进程、流和套接字等，并侧重十异步[!/O、网络和HTTP。12.2
节将会介绍它。  

本章标题表明本章是关于“服务器端”的JavaScript,Nodc和Rhion常币丁创建脚本服务
器。怛“服务器”这个词也意味着“Web浏览器之外的任何事情”。Rhino程序能使用
Java的Swing框架创建图形UI,ifNode 上:运行的JavaScrip(程序可以像shell脚本那样去操
作文件。  

本章非常简短,仅难备重点介绍在Web浏览器之外使用JavaScript的--些方式; 不会尝试
全面介绍Rhino和Node,第三部分也不会包涵这里讨论的API; 并且不会详细介绍Java平
台或POSIX API,接下来关于Rhion的章节假定读者有一定的Java经验，关于Node的章书
假定读者有一定的底层Unix API的经验。

---

译汪1: Node是其官方名字、Node.j是非官方名字,用于和其他的node区分，具体内容见hps://
www.github.com/joyent/node!wikilFAQ。
 
---

##  12.1 用Rhino脚本化Java
Rhina是一种用Java编写的JavaScript解释器，其设计目标是借助丁强大的Java平台API实
现轻松编写JavaScript程序。Rhino能白动完成JavaScript原生类型的Java原生类型之间的
稍互转换，因此JavaScript脚本可以设置、查询Java属性，并调用Java方祛。
<table style="border:2px solid #ccc">
<tr><td>
获得Rhino
Rhino是Mozilla开发的免赀软件，可以从http://www.mozilla.org/rhino/ 

下载。Rhino
的1.7r2版本实现了ECMAScript 3,以及在11章介绍的很多语言扩展。Rhino软件比
较成热，不会经常发布新版本。在写本章时，1.7r3的顶览版已出现在源码库中，
它实现了ECMAScript 5的部分内容毕。Rhino打包为JAR文件发布，可以从使用
下面这行命令开始探索之旅:
java-jar rhinol_7R2/js.ja progzam.js
如罘省略program.js,Rhino会开启一个交互的she]界面，它对尝试简单或单行的
程序比较有用。
</td></tr>
</table>

Rhino定义丁少量贡要的全局函数，不过它们都不悬JavaScript的核心组成邵分:
```
print(x) ;//特定于嵌人的全局函数: 输人he1p()获取更多的rhino提示:
//全局输出函数.将内容输出纠控制台

1告诉Rhin喘要使用JS 1.7的悟言特性
version(170) ;
加投并执行1个或宝个JavaScript代码文件
load(flename,...);
11读取义本文件，并以字符审的形式返回内容
readFile(fle);
readUr1(uI1);
11读取URL的原文内容，并以字符串的形式返回内容
'1运行f()或者在一个新线程巾加载执行文件f
spawn(f);_..
1使用0或多个命令行参数来运行系统命令
runcoanand(cnd,[args...]);
quit()
1退出Rhino
```
注意print()丽数: 任本节我们将用它取代console.log()。Rhino会将Java包和类表示成
JavaScript对象:
```
/! 全局变並Packages是Java包层次结构的根
Packages.any,package.name 

 /1任何来自Java CLASSPATH的包
iava,lang
!! 佥局空量java是Packages.java的短名
!! javax 起Packages.javax的短名
javax.Swing
! 类; 能像包的属性一样存取
var 5ystem = java.lang-System;
ar JFrame= javax.swing.JFrame;
```

---

1.7r3版本已经在2011.06.03正式发布.具体内容见htp://ww.aillaogrhinoidownload.
泽注2:
htmxl。
--.....-
一" " " ，

---


山丁Rhinb把包和类表示为JavaScript对象，因此可以将它们赋值给变景从而得到相应的
短名。如果感意。也可以用页正式的方式导入它们:
```
var ArrayList= java.util.ArrayList; 11为类创建短名
importClass(java.uti1,HashMap);
11其等问于: var HashiMap= java.util.HashMap
1使用importPackage()导人包(惰性池)
I 不要导人java.lang: 太多的名字和avaScript全局变母有冲突
impartPackage(java.util);
importPackage(java.net 

) ;
/1另--技术: 传人任态数盘的类和创给JavaImporter()
ll 并在HIth语句中使用它返回的对象
var guipkgs= ]avaImporter(java.awt,java.awt.event,Packges.javax.swing) ;
with (guipkg5) {、
/* 这里定义Font、ActionListener和JFrame等类*/
```
Java类能使用new进行实例化，就像JavaScript类一样:
```
1l 对象: 使用new实例化Java类
ver f= new java.io 

.File("/tmp/test"); 11我们随后将使用这些对象
vaI out= new java.i0.FileWriter(f);
Rhino让JavaScript的instanceof运算符能用TJava对象和类:
/l => true
f Instanceof fava.io 

.Flle
!/=> false: 它是Writer而非Reader
out instanceof java.io 

.Reader
out instanceof java.ia.closeable /l => true: Writer实观Closeable
```
如你所见，在之前的对象实例化示例中，Rhino允许把值传给Java构造困数，并将构造
承数的返回值赋给JavaSeript变量。(注意，在这个例了中Rhino执行了隐式类型转换;
JavaScript字?符串“/type!test”肖动转换成Java的java.lang.String值。) Java方法更像Java
构造函数，而Rhino允许JavaScript耀序调用Java方法;
```
/1静态Java方i 法工作类似JavaScript函数
java.lang-System-getProperty("java.version") 11返回Java版术
var 1sDlgit = java.lang.Character.isDigit; // 把静态方毖赋值给变饿
isDigit("2")
//=> true: 阿拉伯数字2
/1调用Java对象f的实例方法，out已经在前面创建
out.WIite("Hello World\n") ;
qut.close();
. 1ength() ;
var len= f.
```
Rhino也允许JavaScript代码查询、设置Java类的静态宇段和Java对象的实列字段。Java类
通常利用gcller和sectlr方祛避免定义公共字段。当gctlcr和sctter方法存在时，Rhinu将其
显示为JavaScript的属性:
```
11读取Java类的静态牢段
vaI stdout= java.lang.System.out;
叫;
// Rhino把getter和lsetter力法映射到单个JavaScript羼性
!/=>"/tmp/test": 调用f.getName()
f. name
f.directory 1I => false: 调用f.isDirectory()

般，Rhino能根捃传递的畚数类型判断
```
Java允许重载方法，它们名宁相同但签名不向。
出所要调用方法的版本。不过偶尔也需要通过名宁利签名来明确识别方法:
```
1假设]ava对象0有一个名为f()的方法，它接受int或float黍数
1! 在JavaScript中.必须明确指定签名
11调用int方祛
['f(int)'](3);
['f(fbat)' ](Math.PI);
11凋用f0毗方祛
使用for/in循环能遍历Java类和对象的方法，宇段和属性:
importClass(java.lang-System);
for(var m in System) print(m); 11翰Ejava.lang.5ystem的静态成员
for(m 1n f) print(m);
11输[ijava.io .FIle的实例成员


11注意不能用这种方法牧举包中的类
for (c in java.lang) print(c); 11无祛工作
```
Rhino允许JavaScript程序状取、设置Java数组的元素，就像它们是JavaScripl数组那样。
当然，Java数组和JavaScripl数组并不完全一致: Java数组长度固定、兀素类型统。，但
不具备像s1ice()这样的JavaScripl方法。由于没有现成的JavaScript诏法可供Rhino#' 展
JavaScript程序从i创建新的Java数组，因此必须使用java.lang.refect.Array类来实现:
```
'1分别创建一个长度为1D的牢符串教组和一.个长度为12B字节的数组
var words= java.lang.reflect.Array.newInstance(java.lang.String; 10);
var bytes = java.lang.Ieflect.AIIay.newInstance(java.lang.Byte.TYPE,12B);
11.且创建了数组，號能像JavaScript数组:样使用它们
for(var i= 0; i< bytes.1ength; i++) bytes[i]= i;
```
Java编程经常涉及实现接口，这在GCI编程巾很常见，每个事件处理程序都必须实现事
件监听接口，接下来的例子将演示如何实现Java事件监听接日:
```
/! 接口: 如下所示实现接1
var handler= neb java.awt.event.FocusListener{{
focusGained: function(e){ print("got focus"); },
focuslost: function(e){ print("lost focus"); :
});
11用伺样的方式扩展抽象类
var handler= new java.awt.event.WindowAdapter({
windowClosing: function(e){ java.lang-System.exit(o}; }
});
服务器端JavaScript
293

秦宇  15:13:01
1I 当接几只有一个方法，可以使用一个函数取而代之
button.addActionlistenex(function(e){ print("button clicked"); });
11幻果接口或抽象类的所有方祛都有相同的签名
11则可以使用个单独的函影作为接口的实现
1! 且Rhino将把方法名作为最后一个参数传人
frame.addHindowListener(function(e,name) ，
if (name==z "windowC1osing") java.lang.5ystem.exit(0);
});
/I 如果需要一个对象实现多重接门，则使用JavaAdapter
var 口= new Javadapter(java,awt.event.ActionListener; java.lang.Runnable,{
11实现Runnable
tun: functon() {},
function{e) {} 11实现ActionListener
actionPerformed:
}};
```
当Java方祛抛出异常,Rhino将其作为JavaScript异常传递。通过JavaScript错误对象的
javaException属性可以获取原始的Javajava.{ang.Fxcepfion对象:
```
try{
java.lang.5ystem.getProperty(null); 11null不足合法的参數
11e是JavaStript异常
catch(e) {、.
print(e.javaException);
11它包含一个java.lang.NullPointerException异常
```
最后,必须往意Rhino的类型转换。Rhin会按需要自动转换原始数字、布尔值利Inu11。
Java的char类犁被当做JavaScript数字对待,因为JavaScript设有字符类型。JavaScript字
符串能自动转换成Java字符串，但这叮能也是个绊脚石，因，为像java.lang String对象这
样的Java字符串不能转换回JavaScript字符串。注意前面出现过的这行代码;
```
vat version = fava.lang.System.getPropexty("java.vexsion");
```
调用这行代码后，变量version保存了个java.iang,String对象。这行代码的行为希起
来像JavaScript字符串，其实区别巨大。首先。Java宁符串有length()方祛而役有length
属性。其次，对Jaya字符申进行typeof运算得到的结果是“object”。无祛通过调用其
to5tring()方法把Java字符申转换成JavaScrip字符串，因为所有的Java对象都有自己的
toString()方法,石者返回java.iang.String。为了杷Java值转换成宁符串，请将它传递
给JavaSeript的String() 函数:
```
vax version = string(java.lang-System.getProperty("java.version"));
```
**Rhino示例**  

例12-1是一个简单的Rhino应用，它演示了前面介绍的很多特性和技术。本示例使用javax.
swing GUI包、java.net 

网络包.javu.io 

流的输入/输出{I/O) 包和Java的多线程功能实现

个简单的下载管瑾器应用; 它把对应URL的文件下载到本地，井在下载时显示下载进
度。图12-1展示「当两个下载挂起时应用的火致样子。


```
/*
 * 使用简单的Java GUI实现下载管理器应用
 */

// 导入Swing GUI组件和一些其他组件
importPackage(javax.swing); 
importClass(javax.swing.border.EmptyBorder);
importClass(java.awt.event.ActionListener);
importClass(java.net.URL);
importClass(java.io.FileOutputStream);
importClass(java.lang.Thread);

// Create some GUI widgets
var frame = new JFrame("Rhino URL Fetcher");     // 应用窗体
var urlfield = new JTextField(30);               // URL输入字段
var button = new JButton("Download");            // 开始下载的按钮
var filechooser = new JFileChooser();            // 文件选择对话框
var row = Box.createHorizontalBox();             // 用于放置字段和按钮的方案
var col = Box.createVerticalBox();               // 用于放置数据行和进度条
var padding = new EmptyBorder(3,3,3,3);          // 填充数据行的空白

// 把他们组装一起并显示这个GUI
row.add(urlfield);                               // 把输入字段放入行中
row.add(button);                                 // 把按钮放入行中
col.add(row);                                    // 把行放入列中
frame.add(col);                                  // 把列放入窗体中
row.setBorder(padding);                          // 为行增加一些空白
frame.pack();                                    // 设置为最小值
frame.visible = true;                            // 设置窗体可见

// 当窗体中发生任何事件都会调用这个函数
frame.addWindowListener(function(e, name) {
    // 如果用户关闭窗体，推出这个应用
    if (name === "windowClosing")                // Rhino 加入了name参数
        java.lang.System.exit(0);
});

// 当用户单击按钮时，调用这个函数
button.addActionListener(function() {
    try {
        // 创建java.net.URL表示源URL
        // (这会检查用户的输入是否是符合语法规则)
        var url = new URL(urlfield.text);
        // 告诉用户选择保存URL内容的文件
        var response = filechooser.showSaveDialog(frame);
        // 如果单击Cancel按钮，立即退出
        if (response != JFileChooser.APPROVE_OPTION) return;
        // 现在启动一个新线程下载URL
        var file = filechooser.getSelectedFile();
        // Now start a new thread to download the url
        new java.lang.Thread(function() { download(url,file); }).start();
    }    catch(e) {
        // 如果出现错误，显示一个对话框
        JOptionPane.showMessageDialog(frame, e.message, "Exception",
                                     JOptionPane.ERROR_MESSAGE);
    }
});

// 使用java.net.URL等下载URL的内容，使用java.io.File等把内容保存到一个文件中
// 在JProgressBar组件中显示下载进度
// 这将在一个新线程中调用
function download(url, file) {
    try {
        // 每次下载一个URL时，我们会添加一个新的数据行到窗体中
        // 数据行中会显示URL、文件名和下载进度
        var row = Box.createHorizontalBox();     // 创建数据行
        row.setBorder(padding);                  // 填充它的空白
        var label = url.toString() + ": ";       // 显示URL
        row.add(new JLabel(label));              // 在Jlabel中
        var bar = new JProgressBar(0, 100);      // 加入进度条
        bar.stringPainted = true;                // 显示文件名
        bar.string = file.toString();            // 在进度条中
        row.add(bar);                            // 把进度条加入新的行中
        col.add(row);                            // 把数据行加入列中
        frame.pack();                            // 调整窗体大小

        // 我们不知道URL的大小，所以进度条是动画
        bar.indeterminate = true; 

        // 如果可能，立即连接服务器并获取URL的长度
        var conn = url.openConnection();         // 得到java.net.URLConnection
        conn.connect();                          // 连接且等待连接头
        var len = conn.contentLength;            // 如果能得到URL长度就设置
        if (len) {                               // 如果长度已知，那么
            bar.maximum = len;                   // 设置进度条展示
            bar.indeterminate = false;           // 下载的百分比
        }

        // Get input and output streams
        var input = conn.inputStream;            // 从服务器读取字节
        var output = new FileOutputStream(file); // 把字节写入文件
        
        // Create an array of 4k bytes as an input buffer
        var buffer = java.lang.reflect.Array.newInstance(java.lang.Byte.TYPE,
                                                         4096);
        var num;
        while((num=input.read(buffer)) != -1) {  // 读取然后循环至EOF
            output.write(buffer, 0, num);        // 把字节写入文件
            bar.value += num;                    // 更新进度条
        }
        output.close();                          // 完成后关闭流
        input.close();
    }
    catch(e) { // If anything goes wrong, display error in progress bar
        if (bar) {
            bar.indeterminate = false;           // 停止动画
            bar.string = e.toString();           // 用错误取代文件名
        }
    }
}


```

##  12.2 用Node实现异步I/O  

Node是基丁C++的尚速JavaScript解释器，绑定了用丁进程、文仆和网络套接字等底层
Unix API,还绑定了HTTP客户端和服务器API。除了一些专门命名的同步方壮外，Node
的绑定都恳异步的，」Node程序默认绝不阻塞。这意味着它们通常具备强大的可伸缩能
力并能有效地处理高负荷。由于API是异步的，因此Node依赖事件处理程序，其通常使
用嵌套丽数和闭包米实现伊。
本节重点介绍Nole部分最重要的API和事件; 但这些文档并不完整。请到http://nodejs 

.
org!apil 有看Node的联机文档详在3。

<table style="border:2px solid #ccc">
<tr><td>
<h3>获得Node</h3>
Node是免费软件，可以从http://nodejs.org 
上下载。在写本章时，Node依旧处于活
跃开发期，不过尚无二进制版本、.你必须自己获取并编译源码。本节的创子是在
Node 0.4版本下编写和测试的掉江4。这些API尚未完全确定，但这里介绍的基本原
则在未来不会有太多改变。
Noule是在Google的V8 JavaScript引l 擎上构建而成。Node 0.4使用的是V8的3.1版
本，它实现了除严格模式之外的全部ECMAScript5。
下载、编译并安装Node后，可以使用如下命令运行Node程序:
node program.js
</td></tr>
</table>

---

客户端的JavaScript也能高度地并步和基于事件，如果你读过本书第二部分，且在客户端
注1;
中运行过JavaScript程序，就会很容易理解本章的例子。
译注3:
大家也可以交肴CNode社区组织颓译的Node中文文档，参见htp:/cnodejs.org/cman 

/.
在翻译本书时，Node发布了0.4.12稳定版和0.5.7不稳定版。Node的版本控制方崬是偶
详注4:
数版本稳定，奇数版本不稳定，狍定版本只会修复bug,不会改变JavaScript API和扩展
API,在稳定版本分支升级之后不需委重断生成模块。

---

我们之前从print()和1oad(}函数开始介绍Rhino。Node也有类似函数，只是名字不同:
```
1Node定义了console.log(},可以像在浏览器吼那祥调试代码输出
console.log("Hello Node"); 1l 渊试输出到控制台
11使用require()替代1oad()
1它加载井执行(只有一次) 命名模块。返回包含其导出标识符(exported symbo1) 的对豫
var fs= require("fs"); 11加裁"fs”模块、并返回其API对象
```
Nude在其全局对象中实现了所有标准的ECMAScript 5构造闲数、属性和函数。除此之
外。它也支持客户端的计时器凼数集setTimeput{).setInterval(),clearTmeout()和
clearInterval():
```
111秒钟后输出"Hello Wor1d"
setTimeout(function(){ console.1og("Hello Wrld"); ); 1000);
```
客户端的令局函数将在14.1节介绍。Node的灾现与Web浏览器的实现兼容。
Node在process名字空间中定义了其他重要的全局属性。这里有该对象的一些属性:
```
11Nade的版本字符申信息
pIacess.version
//“node"命令行的数组参數，argv[o]是"node"
process.aIgv
env
/l 环境变盘对象。例如:process.env.PATH
process.e
11进程id
process.pid
process.getuld() 1! 返区用户过
/! 返回当前的工作日录
process.cwd()
!! 改变目录
process.chdiz()
/1退出(运行shutdown命令之后)
process.exit()
```
由于Node的函数和方法都是异步的，因此当它们等待运算完成时并不产生阻塞。非阻塞
方法的返回值无法返回异步运算的结果给你。如果想获取结果，或想知道完成运算的时
间，当结果稚备好或完成运算{或发生错误) 时,就必须提供Node能调用的一个函数。
在某些悄况下(如在调用前面出现的setTimeout()时)，只须简单地把函数作为参数传
人,Node会适时调用它。在另外--些情况下，则可[以利用Node的事件机制。Nade对象
产生事件(弥为事件舷发器(event emiter)) 定义on()方祛来往册处理程序。当传人参
数时; 将事件类型(一个字符串) 作为第一参数，处理程序函数作为第二参数。不同的
事件类型传递给处理程序函数的参数不同，你可能湍要查阅AP1文档从而确切了解如何
编写处理程片:
```
emitter,on(name,f}
/! emitter往册f函教处理name$件
/! add Linsten e r() 和o n()是 同一个方祛
emitter.addListener(name; f)
11只执行一次; 然后f会自动删除
eitter.once(name: f)
/! 返回事件处理的数组成的数组
emitter.listeners(name)
enitter.removeListener(name,f) 11注销事件处理程序f
emitter.removeAllListeners(name) /l 移除name事件的所有处埋程序
前面介绍的process对象是一个事件触发器，这里是其部分事件的处理程序示例;
11"ex1t" 半件在Mode退出之前发送
process.on("exit",function(){ console.log("Goodbye"); }}
前空
品
1如果往册了任何事件处理程序。非捕获异常都会产“生事件，
11否则，异常仅会使Node输出错误然后退出
process.on("uncaughtException",function(e){ console.1og(Exception,e); }) ;
/1$0SIX申诸如SICINT.SIGHUP和5IGTERM等信号产生事件
process.on("SIGINT",function(){ console.1og("Ignored Ctr1-C"); });
```
Node的设计目标是高性能IO,因此其流API常被用到。当数据准备好时，可读流会触发
事作。任下面的代码中，假设s是在其他地方得到的可读流。下面我们将石到如何从文
件和网络套接宇中得到流对象;
```
/1输入流s
/1当数据可用时，把它作为参数仿给f(}
s.on("data",f);
11当不再有数据达到，在文件结束(EOF) 时会妯发"end"事件
s. c n ("end'
鸡地女司，
11如果发生错误，把异常传递给f()
s.on("exxox"; f};
11如果它是依旧打开的可读流，返回true
s.readable
11暂停“data"事件。例如，为了限制上传
5.pouse();
11再次恢复
s.resume();
11如果想把字符串传给"data"事件处理程序，嵴指定编码
5.setEncoding(enc); 11如何对宁兴编码:"utf8"、"ascii"或"base64"
```
可写流比可读流的核心事件少。使用write()方法发送数据，当所有数据写入完毕后使
用end()方祛结束流。write()方法决不会阻塞。若Nadc无祛立即写人数据而不得不在内
部缓存它，则write()方法返川false。如果你想知道Nodc何时刷新缓仲区并确保数据实
际上已写人，那么请往册“drain”事件的处理程序:
```
11钧出流s
11写人二进制数据
s.wI it e{ bu ffer} ;
;ot''
。urit
s.write(string,encoding) 11写人字符串数据,默认编码是"utf-8"
s.end()
// 结束流
11写人最后的二进制数据块井结束
s.end(buffer);
/ 写人最后的子符串并结束所有流
s. end(s tr ，encod ing)
1/ 如果流依旧打开且可写入。返回true
s.writeable;
11当内部缓冲区为空，调用f()
s.cn{"drain",f)
```
如之前代码所示，Node的流能处理二进制数据和文术数据。文本传输使用的是普通
JavaScript字符串，字布使用Nodc特定的缓冲区来处埋。Node的缓冲区是有固定长度的
类数组对象，其元素数量必须在0~255之间。Node程序通常把缓冲区作为不透明的数
据块来对待，将它们从一个流中读取然后写人另一个。但缓冲区中的字节能够像数组元
素一样存取，其对应的方祛有从一个缓冲区复制二进制数据到另一个、获取基础缓冲区
的切片(slice)、使用指定编码把字符串写人缓冲区和把缓冲区或部分缓冲区解码回字
符串:
```
11创建一个256宇节的新缓冲区
var bytes= neW BuffeI(256) ;
for(var i = 0; i< bytes.length; i++) // 通过索引值进行迹历
bytes[i]= I;
11设翌缓冲|区的每个元亲
/1为这个缓冲区创建一个新的祝图
var end= bytes.sl1ce(240; 256);
1=> 240: end[0] 就悬bytes[240]
end[0]
end[o]= a
/! 修改这个切片的一个元素
// => 0: 原始缀仲区也修改了
bytes[240]
11创建个新的独立级冲区
var more = new Buffer(8);
nd.copy(more,O; B; 16);
11把end[}的第8~15元索复制到more[]中
1=> 248
more[Q]
1缓冲|就，也叮以实现二进制<=> 文本的转换
11舍法编码必"utf8"、"ascii" 和"base64".默认编码是"utf8"
var buf= new Buffer("2tr","utf8" ); 11使用UTF-8把文本编码为字书
buf.length
!/=> 3个宇符占4个字书
的子下
buf.toString()
//=> "2rr": 返回文本
buf= new 8uffer(10];
!1开始一个新的用定长度的缓冲区
1l 从第4个宇节开始写人文本‘
var len= buf.write("tr2"; 4};
buf.toString("utf8",4,4tlen)
!/=> "zr2"; 解码段字书
```
Node的文件和文件系统API位于“fs" 模块中:
```
var fs r require("fs"); 11加载文件系统API
```
这个模块提供了共绝大部分方法的“同步版本”
任何名字以“Sync”结尾的方祛都是
一个阻塞方法，它返回一个值或抛出一个异常。不以“Sync”结尾的文件系统方祛都是
非阻塞的方法，它们会把结果或错误传给指定的回调函数。下面的代码展示了如何使用
阻塞方祛读取文本文件、如何使用非阻塞方祛读取二进制文件:
```
/1同步读取文件，通过传逊编码获得文本而非字节
var text= fs.readFi1e5ync("config.json","utf8");
11异步读取二进制文件; 通过传递丽数获得数摒
fs.readFile{"image.png",function{err,buffer) {
if (err) throw err; 11如果出现任何错误
'1文件内容在缓冲区中
pIocess(buffer);
});
```
类似地，存在用来写文件的writeFile()和writeFileSync()函数:
```
fs.niteFi1e(*comfg.json"; JSON.stringify(userprefs));
```
前面展示的函数将文件内容看待为单个字符串或缓冲区。Node也定义了读写文件的流
API; 下面这个两数实现了文件复制:
```
11用流API复制文件
11岩想知道何时完成，诮传递回调函敬
function f1eCopy(filename1; flename2; done) {
11输入流
input= fs.createReadStrean(filename1);
vaI output = fs.createWriteStrem{f1ename2);
11输出讹
input.on("data",function(d){ output.write(d}; }); 11把输人复制到输出
11提示错误
input.on("error" ，function(exr){ throw err; });
品
l! 当输人结束
input.on("end"，function() {
山元
11关闭输出
output.end(} ;
11并通知回调函数
if (done) done();
}),
```
“fs" 模块还包括大量的方法，用于列出文仆目录，查询文件属怔等。下面的Node程序
使用同步的方让列出一个目录的内容，并显示文件大小和修改口期;
```
#! /usr/loca1/bin/node
11加载需要的模块
ar fs 口zequire("fs"),path = Tequire("path");
/1当前目录
var dir = process.cwd();
if (process.argv.1ength > 2) dir = process.argv[2]; 1或米自命令行
11读取肖录内容
var fles 口fsireaddiISync(dlz);
process.stdout.WIite("Name\tSize\tDate\n");
输出头
/1获取每个文件名
fles.forEach(function(flename) {
11拼接目录和文件名
fullname = path.join(diz,flename);
var.
// 获取文件属性
stats = fs.statSync(fu11name};..
var,
今日票生
f (stats.isDirectory()) filename +="/";
11瓠记子日录
'1输出文件名+
process.stdout.write(filename
/! 文件大小+
stats.size + "\t" +
stats.mtime + "\n");
11修改时间
});
```
注意上面第一行的往释“#!”
这是Unix中的“shebang”注释，常用于使脚本文件被捐
定的某种语言解释器自动执行年注5。当像这样的代码出现在文件的第一行时，Node会忽
略它们。
“net”模块是用于基于TCP网络的API。
(用于基于数据包网络的模块诮看
"dgram”。) 下面是Node中一个非常简单的TCP服务器:
```
1l Node中简单的TCP回晁服务器: 它监听200端口」的连掖，
/1并把客户端的数据回显给它
var net= require('net' );
var server 口net.createServer();
server.Iisten(2000; function(){ consale.log("Listening on part 200"); });
server.on("connection",function(stream)
console.log("Accepting connection fram"; stream.remoteAddress);
stream,on("data",function(data) {stream,write(data); }) ;
stream.on("end",function(data){ console.1og("Connection closed"); }};
]);
```
除了基础的“net" 模块，Node使用“http" 模块内置支持HTTP协议。接下来的示例可
以说明更多细节。
译注5: * 于shebang的详细鲜释谚查看http:/h.wkipedia org/wilShebang.
服务器端JavaScript| 301

###  12.2.1 Node示例:HTTP服务器  

例12-2是一个基于Nodc的简单HTTP服务。它能处理当前目录的文件，并能实现两种特
殊的URL。它使用了Node的“h(tp”模块，也会使用到前面提到的文件和流API。第18
章的示例18-17是一个与之类似的HTTP服务器示例。
例 12-2：基于Node的HTTP服务器

```javascript
// 这是一个简单的Node HTTP服务器，能处理当前目录的文件
// 并能实现两种特殊的URL用于测试
// 用http://localhost:8000 或者 http://127.0.0.1:8000 连接这个服务器

// 首先，加载所有要用的模块
var http = require('http');      // HTTP服务器API
var fs = require('fs');          // 用于处理本地文件

var server = new http.Server();  // 创建新的HTTP服务器
server.listen(8000);             // 在端口8000上运行它

// Node使用"on()"方法注册事件处理程序,
// 当服务器得到新请求，则运行函数处理它
server.on("request", function (request, response) {
    // 解析请求的URL
    var url = require('url').parse(request.url);
    // 特殊URL会让服务器在发送响应前先等待
    // 此处用于模拟缓慢的网络连接
    if (url.pathname === "/test/delay") {
        // 使用查询字符串来获取延迟时长，或者2000毫秒
        var delay = parseInt(url.query) || 2000;
        // 设置响应状态码和头
        response.writeHead(200, {"Content-Type": "text/plain; charset=UTF-8"});
        // 立即开始编写相应主体
        response.write("Sleeping for " + delay + " milliseconds...");
        // 在之后调用的另一个函数中完成响应
        setTimeout(function() { 
            response.write("done.");
            response.end();
        }, delay);
    }
    // 若请求是"test/mirror"，则原文返回它
    // 当需要看到这个请求头和主体时，会很有用
    else if (url.pathname === "/test/mirror") {
        // 响应状态和头
        response.writeHead(200, {"Content-Type": "text/plain; charset=UTF-8"});
        // 用请求的内容开始编写响应主体
        response.write(request.method + " " + request.url + 
                       " HTTP/" + request.httpVersion + "\r\n");
        // 所有的请求头
        for(var h in request.headers) {
            response.write(h + ": " + request.headers[h] + "\r\n");
        }
        response.write("\r\n");  // 使用额外的空白行来结束头
        // 在这些事件处理程序函数中完成响应：
        // 当请求主体的数据块完成时，把其写入相应中
        request.on("data", function(chunk) { response.write(chunk); });
        // 当请求结束时，响应也完成
        request.on("end", function(chunk) { response.end(); });
    }
    // 获取本地文件名，基于其扩展名推测内容类型
    else {
        // Get local filename and guess its content type based on its extension.
        var filename = url.pathname.substring(1); // 去掉前导 /
        var type;  
        switch(filename.substring(filename.lastIndexOf(".")+1))  { // 扩展名
        case "html":
        case "htm":      type = "text/html; charset=UTF-8"; break;
        case "js":       type = "application/javascript; charset=UTF-8"; break;
        case "css":      type = "text/css; charset=UTF-8"; break;
        case "txt" :     type = "text/plain; charset=UTF-8"; break;
        case "manifest": type = "text/cache-manifest; charset=UTF-8"; break;
        default:         type = "application/octet-stream"; break;
        }
                
        // 异步读取文件，并将内容作为单独的数据块传给回调函数
        // 对于确实很大的文件，使用流API fs.createReadStream()更好
        fs.readFile(filename, function(err, content) {
            if (err) {  // 如果由于某些原因无法读取该文件
                response.writeHead(404, {    // 发送404未找到状态码
                    "Content-Type": "text/plain; charset=UTF-8"});
                response.write(err.message); // 简单的错误消息主体
                response.end();              // 完成
            }
            else {      // 否则，若读取文件成功
                response.writeHead(200,  // 设置状态码和MIME类型
                                   {"Content-Type": type});
                response.write(content); // 把文件内容作为相应主体发送
                response.end();          // 完成
            }
        });
    }
});
```

###  12.2.2 Node示例: HTTP客户端工具模块  

例12-3使用“http" 榄块定义了用于发送HTTP GET和POST请求的工具函数。本例则是
基于“ttputils" 榄块，在代码中应该这样使用;
```javascript
var httputils = require("./httputils
; 11让意没有".js"后蹂
httputils.get (ur1,function(status,headers,body){ console.log(body); }) ;
```
require()函数并非用咨通的eval()函数来执行模块代码。模块足在一个特殊的环境中
执行，以便它们不能定义任何全局变量或更改其他全局命名空问。这个特殊的模块执行
```javascript
//
// 基于Node的"httputils"模块
//

// 为制定的URL实现一个异步HTTP GET请求，
// 并将HTTP状态、头和响应主体传递给指定的回调函数
// 注意这里是如何通过exports对象导出这个方法的
exports.get = function(url, callback) {  
    //  解析URL，获取所需的信息
    url = require('url').parse(url);
    var hostname = url.hostname, port = url.port || 80;
    var path = url.pathname, query = url.query;
    if (query) path += "?" + query;

    // 实现一个简单的GET请求
    var client = require("http").createClient(port, hostname);
    var request = client.request("GET", path, { 
        "Host": hostname    // Request headers
    }); 
    request.end();

    // 该函数用于处理到达的相应
    request.on("response", function(response) {
        // 设置编码，使返回的主体成文文本而非字节
        response.setEncoding("utf8");
        // 一旦响应主体达到，保护它
        var body = ""
        response.on("data", function(chunk) { body += chunk; });
        // 相应完成时，调用这个函数
        response.on("end", function() {
            if (callback) callback(response.statusCode, response.headers, body);
        });
    });
};

// 以数据作为请求主体的简单HTTP POST请求
exports.post = function(url, data, callback) {
    // 解析URL，获取所需的信息
    url = require('url').parse(url);
    var hostname = url.hostname, port = url.port || 80;
    var path = url.pathname, query = url.query;
    if (query) path += "?" + query;

    // 判断将要作为请求主体发送的数据类型
    var type;
    if (data == null) data = "";
    if (data instanceof Buffer)             // 二进制数据
        type = "application/octet-stream";
    else if (typeof data === "string")      // 字符串数据
        type = "text/plain; charset=UTF-8";
    else if (typeof data === "object") {    // 名/值对
        data = require("querystring").stringify(data);
        type = "application/x-www-form-urlencoded";
    }

    // 生成POST请求，其中包括请求主体
    var client = require("http").createClient(port, hostname);
    var request = client.request("POST", path, {
        "Host": hostname,       
        "Content-Type": type
    });
    request.write(data);                        // 发送请求主体
    request.end();       
    request.on("response", function(response) { // 处理响应
        response.setEncoding("utf8");           // 假设它是文本
        var body = ""                           // 用于保存相应主体
        response.on("data", function(chunk) { body += chunk; });
        response.on("end", function() {         // 完成后，调用回调函数
            if (callback) callback(response.statusCode, response.headers, body);
        });
    });
};
```