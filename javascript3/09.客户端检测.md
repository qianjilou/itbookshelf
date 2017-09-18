#  第9章 客户端检测([返回首页](https://github.com/qianjilou/javascript3))
**本章内容**
- [ ] 使用能力检测
- [ ] 用户代理检测的历史
- [ ] 选择检测方式  

  浏览器提供商虽然在实现公共接口方面投人了很多精力，但结果仍然是每一种浏览器都有各自 /XII的长处，也都有各自的缺点。即使是那些跨平台的浏览器，虽然从技术上看版本相丨吊，也照 样存在不一致性问题。面对普遍存在的不一致性问题，开发人员要么采取迁就各方的“最小公分母”策 略，要么（也是更常见的）就得利用各种客户端检测方法，来突破或者规避种种局限性。
  迄今为止，客户端检测仍然是Web开发领域中一个饱受争议的话题。一谈到这个话题，人们总会 不约而同地提到浏览器应该支持一组最常用的公共功能。在理想状态下，确实应该如此。但是，在现实 当中，浏览器之间的差异以及不同浏览器的“怪癖”（quirk),多得简直不胜枚举。因此，客户端检测除 了是一种补救措施之外，更是-种行之有效的开发策略。
检测Web客户端的手段很多，而且各有利弊。但最重要的还是要知道，不到万不得已，就不要使 用客户端检测。只要能找到吏通用的方法，就应该优先采用更通用的方法。一言以蔽之，先设计最通用 的方案，然后再使用特定于浏览器的技术增强该方案。  

##  9.1 能力检测
  最常用也最为人们广泛接受的客户端检测形式是能力检测（又称特性检渕)。能力检测的目标不是 识别特定的浏览器，而是识别浏览器的能力。采用这种方式不必顾及特定的浏览器如何如何，只要确定 浏览器支持特定的能力，就可以给出解决方案。能力检测的基本模式如下：
```

```
举例来说，IE5.0之前的版本不支持(1〇01111^111：.961£1611161115：8乂1(^()这个1>(说方法。尽管可以使 用非标准的document .all属性实现相同的口的，但IE的早期版本中确实不存在（3〇〇\〇11^11(:.961：- ElementsById()。于是，也就有了类似下面的能力检测代码：
```

```
这里的getElement ()函数的用途是返回具存给定ID的兀索。因为document .getElementById () 是实现这一u的的标准方式，所以一开始就测试了这个方法。如果该函数存在（不是未定义），则使用 该函数。否则，就要继续检测document.all是否存在，如果是，则使用它。如果上述两个特性都不 存在（很有可能)，则创建并抛出错误，表示这个函数尤法使用。
要理解能力检测，首先必须理解两个重要的概念。如前所述，第一个概念就是先检测达成目的的最常 用的特性。对前面的例子来说，就是要先检测document. getElementByldO , /n-检测document, all。 先检测玻常用的特性可以保证代码最优化，W为在多数情况下都可以避免测试多个条件。
第二个重要的概念就是必须测试实际要用到的特性。一个特性存在，不一定意味着另一个特性也存 在。来看一个例子：
```

```
这是一个错误使用能力检测的例子。getWindowWidthO函数首先检査document.all是否存在, 如果是则返回document:.documentElement.clientwidth。第8章铃经讨论过，IE8及之前版本确 实不支持window.innerwidth属性。但问题是document.all存在也不一定表示浏览器就是EE。实 际上，也可能是Opera; Opera支持document_.all，也支持window.innerWidth。  

###  9.1.1 更可靠的能力检测
能力检测对于想知道某个特性是否会按照适当方式行事（而不仅仅是某个特性存在）非常有用。上 一节中的例子利用类沏转换来确定某个对象成员是否存在，但这样你还是不知道该成员是不是你想要 的。来看下面的函数，它用来确定一个对象是否支持排序。
//不要这样做！这不是能力检測——只捡《了是否存在相应的方法
这个函数通过检测对象是否存在sort ()方法，来确定对象是否支持排序D问题是，任何包含sort 属性的对象也会返回true。
var result = isSortabJe{{ sort: true });
检测某个属性是赉存在并不能确定对象是杏支持排序。更好的方式是检测sort是不是一个函数。
//这样更好：检查sort是不是函数
这里的typeof操作符用于确定sort的确是一个函数，因此可以调用它对数据进行排序。
在可能的情况下，要尽量使用typeof进行能力检测。特别是，宿主对象没有义务让typeof返冋 合理的值。tt令人发指的亊儿就发生在IE中。大多数浏览器在检测到document.createElementO 存在时，都会返冋Crue。
/ /在【E8及之前版本中不行
在1E8及之前版本中，这个函数返M false，因为typeof document.createElement:返W的是 "object”，而不是"function"。如前所述，DOM对象是宿主对象，IE及更早版本中的宿主对象是通 过COM而非JScript实现的。因此，document.createElement()函数确实是一个COM对象，所以 typeof才会返冋"object**。IE9纠正了这个问题，对所有DOM方法都返回"function"。
关于typeof的行为不标准，IE中还可以举出例T来。ActiveX对象（只有IE支持）与其他对象的行 为差异很大。例如，不使川typeof测试某个属性会导致错误，如下所示。
//在IE中会导致嫌误
var xhr = new ActiveXObject("MicrosoCt.XMLHttp"); if (xhr.open){	//这里会发生械误
//执行操作
像这样直接把函数作为属性访问会导致JavaScript错误。使用typeof操作符会更靠谱一点，但IE 对typeof xhr.open会返回"unknown"。这就意味着，在浏览器环境下测试任何对象的某个特性是否 存在，要使用下面这个函数。
//作者：Peter Michaux
可以像下面这样使用这个函数：
目前使用isHostMethodO方法还是比较可靠的，闽为它考虑到了浏览器的怪异行为。不过也要注 意，宿主对象没冇义务保持目前的实现方式不变，也不一定会模仿已有宿主对象的行为。所以，这个函 数——以及其他类似函数，都不能百分之百地保证永远可靠。作为开发人M,必须对自己要使用某个功 能的风险作出理性的估计。
要想深入了解围绕JavaScript中能力检测的一些覌点，请参考PeterMichaux的文 幸 “Feature Detection: State of the Art Browser Scripting” ’ 网址为
http://pcter.michaux.ca/ articles/feature-detection-state-of-the-art-browser-scriptingo
###  9.1.2 能力检测,不是浏览器检测
检测某个或某几个特性并不能够确定浏览器。下面给出的这段代码（或与之差不多的代码）可以在 许多网站中肴到，这种“浏览器检测"代码就是错误地依赖能力检测的典型示例。
//儋误！还不够具体
var isFirefox - !!(navigator.vendor && navigator.vendorSub);
//铋误！假设过头了
var isIE = !!(document.all &S document.uniquelD);
这两行代码代表了对能力检测的典型误用。以前，确实可以通过检测navigator.vendor和 navigator .vendorSub来确定Firefox浏览器。但是，Safari也依胡芦_瓢地实现了相同的属性。于是， 这段代码就会导致人们作出错误的判断。为检测IE，代码测试了 documerxt.all和document. uniquelD。这就相当丁假设1E将来的版本中仍然会继续存在这两个属性，同时还假设其他浏览器都不 会实现这两个属性。烺后，这两个检测都使州了双逻辑非操作符来得到布尔值（比先存储后访问的效果 更好)。
实际上，根据浏览器不同将能力组合起來是更可取的方式。如果你知道All的应用程序需要使州某 些特定的浏览器特性，那么最好足一次性检测所有相关特性，而不要分别检测。看下面的例子。
//确定浏览器是否支持Netscape风格的祛件
以t例子展示了两个检测：一个检测浏览器是杏支持Netscapte风格的插件;另一个检测浏览器是 否具备DOM1级所规定的能力。得到的布尔值可以在以后继续使用，从而节锊重新检测能力的时间。
在实际开发中，应该将能力检测作为确定下一步解决方案的依据,而不是用它来 判断用户使用的是什么浏览器。  

##  9.2 怪癖检测
与能力检测类似，怪癖检测（quirks detection)的U标是识别浏览器的特殊行为。供与能力检测确 认浏览器支持什么能力不同，怪擗检测是想要知道浏览器存在什么缺陷（“任癖”也就是bug)。这通常 谣要运行一小段代码，以确定某一特性不能正常I：作。例如，IE8及更早版本屮存在一个bug,即如果 某个实例屈性与标记为[[DcmtEnum]]的某个原型属性同名，那么该实例属性将不会出现在fon-iri循 环当中。可以使用如下代码来检测这种“怪癖”。
```javascript
var hasDontEnumQuirk = function(){

	var o = { toString : function(){} };
	for (var prop in o){
		if (prop == "toString"){
			return false;
		}
	}

	return true;
}();
```
以上代码通过一个IS名函数来测试该“怪癖”，函数屮创建r—个带布toStringO方法的对象。 在正确的ECMAScript实现中，toString丨以该在for-in循环中作为W性返岡。
另一个经常需要检测的“怪癖”是Safari 3以前版本会枚举被隐藏的属性。Pi以用下面的函数来检 测该“搭癖”。
```javascript
var hasEnumShadowsQuirk = function(){
        
            var o = { toString : function(){} };
            var count = 0;
            for (var prop in o){
                if (prop == "toString"){
                    count++;
                }
            }
        
            return (count > 1);
        }();
```
如果浏览器存在这个bug,那么使用for-in循环枚举带有肖定义的toStringU方法的对象，就 会返冋两个toString的实例。
一般来说，“怪癖”都是个別浏览器所独存的，而且通常被归为bug。在相关浏览器的新版本中，这 些问题可能会也町能不会被修rtrf检测“怪癖”涉及运行代码，因此我们建议仅检测那*对你打ft 接影响的“怪癖”，而ft最好在脚本一开始就执行此类检测，以便尽早解决问题。  

##  9.3 用户代理检测
第=.种，也是争议最人的一种客户端检测技术叫做用户代理检测。用户代理检测通过检测用户代理 字符串来确定实际使JH的浏览器。在每一次HTTP请求过程中，用户代理字符串是作为响应ff部发送的， iftf且该字符串可以通过JavaScript的navigator.userAgcnt M性访问。在服务器端，通过检测用户代 理宇符串来确定用户使用的浏览器足一种常用而且广为接受的做法。时在客户端，用户代理检测-般被 当作一种万不得己才用的做法，并优先级排在能力检测和（或）怪癖检测之后。
提到与用户代理字符屮存关的争议，就不得不提到电子欺骗（spoofing)。所谓电子欺骗，就是指浏 览器通过在fl己的用户代理字符串加人一些错误或误导性信息，来达到欺骗服务器的口的。要弄淸楚这 个问题的来龙去脉，必须从Web问世初期用户代理字符串的发展讲起。  

###  9.3.1 用户代理字符串的历史  

HTTP规范（包括1.0和1.1版）明确规定，浏览器应该发送简短的用户代理字符串，指明浏览器的 名称和版本号。RFC 2616 (即HTTP 1.1协议规范）是这样描述用户代理字符串的：
“产品标识符常用于通信应用程序标识自身，由软件名和版本组成.使用产品标识符的大 多数领域也允许列出作为应用程序主要部分的子产品，由空格分隔.按照惯例，产品要按照相 应的重要程度依次列出，以便标识应用程序.”
h述规范进一步规定，用户代理字符串应该以一组产品的形式给出，字符串格式为：标识符/产品 版本号。但是，现实中的用户代理字符串则绝没有如此简单。  

**1.早期的浏览器**  

1993 年，美国 NCSA (National Center for Supercomputing Applications,国家超级计算机中心）发布 了世界上第一款Web浏览器Mosaic。这款浏览器的用户代理字符串非常简单，类似如下所示。
Mosaic/0.9
尽管这个字符串在不同操作系统和不同平台下会有所变化，但其基本格式还是简单明r的。正斜杠 前面的文本表示产品名称（有时候会出现NCSA Mosaic或其他类似字样），而斜杠后面的文本是产品的 版本5。
Netscape Communications公司介人浏览器开发领域后，遂将自己产品的代号定名为Mozilla( Mosaic Killer的简写，意即Mosaic杀手)。该公司第一个公开发行版，Netscape Navigator 2的用户代理字符串 具有如下格式。
Mozilla/版本号[语言].(平台;加*类变）
Netscape在坚持将产品名和版本号作为用户代理字符串开头的基础上，又在后面依次添加了下列
信息;>
口语言：即语言代码，表示应用程序针对哪种语W设计。
□平台：即操作系统和（或）平台，表示应用程序的运行环境。
□加密类型：即安全加密的类型。可能的值有U (128位加密）、丨（40位加密）和N (未加密)。 典型的Netscape Navigator 2的用户代理字符串如下所示。
Mozilla/2.02 [frj (WinNT; I)
这个字符串表示浏览器是Netscape Navigator 2.02,为法语国家编译，运行在Windows NT平台下， 加密类増为40位。那个时候，通过用户代理字符串中的产品名称，至少还能够轻易地确定用户使用的 是什么浏览器。  

**2. Netscape Navigator 3 和 Internet Explorer 3**  

1996年，Netscape Navigator3发布，随即超越Mosaic成为当时最流行的Web浏览器。而用户代理 字符串只作了一些小的改变，删除了语言标记，同时允许添加操作系统或系统使用的CPU等可选信息。 于是，格式变成如下所示。
Mozilla/技本号（平台;加密类塹丨;操作系仗或CPU说明
运行在Windows系统下的Netscape Navigator 3的用户代理字符中大致如下。
Mozilla/3.0 (Win95; U)
这个字符串表示Netscape Navigator 3运行在Windows 95中，采用了 128位加密技术。可见，在 Windows系统中，字符串中的操作系统或CPU说明被省略了。
Neiscape Navigator 3发布后不久，微软也发布了其第一款贏得用户广泛认可的Web浏览器，即 Internet Explorer 3。由于Netscape浏览器在当时占绝对市场份额，许多服务器在提供网页之前都要专门 检测该浏览器。如果用户通过ffi打不开相关网页，那么这个新生的浏览器很可能就会夭折。于是，微 软决定将IE的用户代理字符串修改成兼容Netscape的形式，结果如下：
Mozilla/2.0 (compatible; MSIE 板本号;操作系统}
例如，Windows95平ft下的IntemetExplorer3.02带有如下用户代理字符串：
Mozilla/2.0 (compatible; MSIE 3.02; Windows 95)
由于当时的大多数浏览器嗅探程序只检测用户代理字符串中的产品名称部分，结果IE就成功地将 自己标识为Mozilla,从而伪装成Netscape Navigatoi•。微软的这一做法招致了很多批评，因为它违反了 浏览器标识的惯例。更不规范的是，IE将真正的浏览器版本号插人到了字符串的中间。
字符串中另外一个有趣的地方是标识符Mozilla 2.0 (而不是3.0)。毕竟，当时的主流版本是3.0, 改成3.0应该对微软更有利才对。但真正的迷底到现在还没有揭开一但很可能只是人为疏忽所致。  

**3.Netscape Communicator 4 和 IE4〜IE8**  

1997年8月，Netscapte Communicator 4发布（这一版将浏览器名字中的Navigator换成了 Communicator )。 Netscape 继续遵循了第 3 版时的用户代理字符串格式 ：
Mozilla/版木号 <平台;加密类型t;操作系坑或CPU说明】>
因此，Windows 98平台中第4版的用户代理字符串如下所示：
Mozilla/4.0 (Win98; I)
Netscape在发布补丁时，子版本号也会相应提髙，用户代理字符串如下面的4.79版所示：
Mozilia/4.79 (Win98; I)
但是，微软在发布Internet Explorer 4时，顺便将用户代理字符串修改成了如下格式：
Mozilla/4.0 (compatible; MSIE 版本号;操作系统）
换句话说，对于Windows 98中运行的IE4而言，其用户代理字符屮为：
M〇2illa/4.0 (compatible; MSIE 4.0; Windows 98)
经过此番修改，MoziUa版本号就与实际的IE版本号一致了，为识别它们的第四代浏览器提供了方 便。但令人遗撼的是，两者的一致性仅限于这-个版本。在hitemetExplorer4.5发布时（只针对Macs )， 虽然Mozi丨丨a版本号还是4,怛正版本号则改成了如下所示：
M〇ziIla/4.0 (compatible; MSIE 4.5; Mac_PowerPC}
此后，IE的版本一直到7都沿袭了这个模式：
Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)
而IE8的用户代理字符串中添加了呈现引擎（Trident)的版本号：
Mozilla/4.0 (compatible; MSIE 版本号;操作系统•• Trident/Trident 版本号>
例如：
Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0)
这个新增的Trident记号是为了让开发人员知道ffi8是不是在兼容模式下运行。如果是，则MSffi的 版本号会变成7,但Trident及版本号还会留在用户代码字符串中：
Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0)
增加这个记号有助于分辨浏览器到底是IE7 (没有Trident记号），还是运行在兼容模式下的IE8。
IE9对字符串格式做了一点调整。Mozilla版本号增加到了 5.0,而Trident的版本号也升到了 5.0。IE9 默认的用户代理宇符串如下：
Mozxlla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/b.O)
如果IE9运行在兼容模式下，字符串中的Mozilla版本号和MSIE版本号会恢复旧的值，但Trident 的版本号仍然是5.0。例如，下面就是1E9运行在IE7兼容模式下的用户代理字符串：
Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; Trident/b.0)
所有这些变化都是为了确保过去的用户代理检测脚本能够继续发挥作用，同时还能给新脚本提供更 丰富的信息。  

**4.Gecko**  

Gecko是Firefox的呈现引擎。当初的Gecko是作为通用Mozilla浏览器的一部分开发的，而第一个 采用Gecko引擎的浏览器是Netscape 6。为Netscape 6编写的一份规范中规定了未来版本中用户代理字 符串的构成。这个新格式与4.x版本中相对简单的字符串相比，有着非常大的区别，如下所示：
Mozilla/Mozilla版本号（平台,•和密类®;操作系统或CPU;语言;预先发行版本）
Gecko/Gecko版本号应用程序或产品/应用•或产品版表号
这个明显复杂了很多的用户代理字符串中蕴含很多新想法。下表列出了字符屮中各项的用意。
为了帮助读者更好地理解Gecko的用户代理字符串，下面我们来看几个从基于Gecko的浏览器中取
得的字符串。
以上这些用户代理字符串都取自基于Gecko的浏览器（只是版本有所不同)。很多时候，检测特定的 浏览器还不如搞清楚它是否基于Gecko更重要。每个字符串中的Mozilla版本都是5.0,自从第一个基于 Gecko的浏览器发布时修改成这个样子，至今就没有改变过;而且，看起来以后似乎也不会有什么变化。
随着Firef〇X4发布，Mozilla简化了这个用户代理字符串。主要改变包括以下几方面。
□删除了 “语言”记号（例如，前面例子中的“en-US”)。
□在浏览器使用强加密（默认设S)时，不显示“加密类型”。也就是说，Mozilla用户代理字符串 中不会再出现“U”，而“I”和“N”还会照常出现。
“平台”记号从Windows用户代理字符串中删除了，“操作系统或CPU”中始终都包含 “Windows” 字符串。
“Gecko 版本号”固定为 “Qecko/201001〇r。
最后，Firefox4用户代理字符串变成了下面这个样子：
Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox 4.0.1  

**5.WebKit**  

2003年，Apple公司宜布要发布自己的Web浏览器，名字定为Safari。Safari的呈现引擎叫WebKit, 是Linux平台中Kxmqueror浏览器的呈现引擎KHTML的一个分支。几年后，WebKit独立出来成为了一 个开源项目，专注于呈现引擎的开发。
这款新浏览器和呈现引擎的开发人员也遇到了与Internet Explorer 3.0类似的问题：如何确保这款浏 览器不被流行的站点拒之门外？答案就是向用户代理字符中中放人足够多的信息，以便站点能够信任它 与其他流行的浏览器是兼容的。于是，WebKit的用户代理宁符串就具备了如下格式：
Mozilla/5.0 (平台;加密类赉;操作系统或CPU;诸言）AppleWebKit/AppleWebKit扳本号 (KHTML, like Gecko) Safari/Safari 版表号
以下就是一个示例：
M然，这乂是一个很长的用户代理卞符串。其中不仅包含了 Apple WebKit的版本兮，也包含了 Safari 的版本号。出于兼容性的考虑，有关人员很快就决定了将Safari标识为Mozilla。至今，基于WebKit的
所有浏览器都将自己标识为Mozilla 5,0,与基于Gecko的浏览器完全一样。但Safari的版本号则通常是 浏览器的编译版本号，不一定与发布时的版本号对应。换句话说，虽然Safari 1.25的用户代理字符串中 包含数字125.1,但两者却不一一对应。
Safari预发行1.0版用户代理字符串中最耐人寻味，也是最饱受诟病的部分就是字符串_(KHTML, like Gecko)"。Apple W此收到许多开发人员的反馈，他们认为这个字符串明显是在欺骗客户端和服 务器，实际上是想让它们把Safari当成Gecko (好像光添加Mozilla/5.0还嫌不够)。Apple的回应与 微软在IE的用户代理字符串遭到责难时如出一辙：Safari与Mozilla兼容，因此网站不应该将Safari用 户拒之n外，否则用户就会认为自己的浏览器不受支持。
到了 Safari3.0发布时，其用户代理字符串乂稍微变长f—点。下面这个新增的Version记号一直到 现在都被用来标识Safari实际的版本号：
需要注意的是，这个变化只在Safari中有，在WebKit中没有。换句话说，其他基于WebKit的浏 览器可能没有这个变化。一般来说，确定浏览器是否基于WebKit要比确定它是不是Safari更有价值， 就像针对Gecko—样。  

**6.Konqueror**  

与KDE Linux集成的Konqueror，是一款基于KHTML开源呈现引擎的浏览器。尽管Konqueror只 能在Linux中使用，但它也有数贵可观的用户。为确保最大限度的兼容性，Konqueroj■效仿IE选择了如 下用户代理字符串格式：
Mozilla/5.0 (compatible; Konqueror/ A本号;操作系统或 CPU >
不过，为了与WebKit的用户代理字符串的变化保持一致，Konqueror 3.2又有了变化，以如下格式 将自己标识为KHTML:
Mozilla/5.0 (compatible; Konqueror/ 版本号;搮作系统或 CPU) KHTML/ KHTML 版本号（like Gecko} 下面是一个例子：
Mozilla/5.0 (compatible; Konqueror/3.5; SunOS) KHTML/3.5.0 (like Gecko)
其中,Ko叫ueror与ICHTML的版本号比较一致，即使有差别也很小，例如K〇nqueror 3.5使用KHTML 3.5.1〇  

**7.Chrome**  

谷歌公司的Chrome浏览器以WebKit作为呈现引擎，但使用了不同的JavaScript引擎。在Chrome0.2 这个最初的beta版中，用户代理字符串完全取自WebKit,只添加了一段表示Chrome版本号的信息，格 式如下：
Mozilla/5.0 (平台;加密类型;操作系仗或CPU;语言} AppleWebKit/AppleWebKit 版未号（KHTML, like Gecko) Chrome/ Chrome A未号 Safari/ Safari 版表
Chrome 7的完整的用户代理字符串如下：
其中，WebKit版本与Safari版本看起来似乎始终会保持一致，尽管没有丨-分的把握。  

**8.Opera**  

仅就用户代理字符串而言，Opera应该是最有争议的一款浏览器f。Opera默认的用户代理字符串 是所有现代浏览器中最合理的——正确地标识身及其版本在Opera 8.0之前，用户代理字符 串采用如K格式：  
Opera/版本号（操作系统或CPU;加密类型}[语言]  
Windows XP中的Opera 7.54会显7TC下面的用户代理字符串：  
Opera/7.54 (Windows NT b.l; U) [er.)  
OperaS发布后，用户代理节符串的“语言”部分被移到圆括号内，以便更好地与其他浏览器匹配， 如下所示：  
Opera/版本号（操作系统或CPU;加密类型;语言）
Windows XP中的Opera 8会®示下面的用户代理字符印:
0pera/8.0 (Windows NT 5.1? U; en)  

默认情况下，Opera会以上面这种简单的格式返回一个用户代理字符串。H前来看，Opera也是主 要浏览器中唯一-个使用产品名和版本号来完全彻底地标识自身的浏览器。可是，与其他浏览器一样， Opera在使用f|己的用户代理字符串时也遇到了问题。即使技术上正确，但因特网h仍然冇不少浏览器 嗅探代码，只钟情于报告Mozilla产品名的那些用户代理字符串。另外还冇相当数的代码则只对IE或 Gecko感兴趣。Opera没有选抒通过修改自身的用户代理字符串来迷惑嗅探代码，而是干脆选择通过修 改自身的用户代理字符串将自身标识为一个完全不同的浏览器。  
Opera9以后，出现了两种修改用户代理字符串的方式。一种方式是将G身标识为另外一个浏览器, 如Firefox或者IE。在这种方式下，用户代理字符串就如同Firefox或丨E的用户代理字符串一样，只不 过末尾追加了字符串opera及Opera的版本兮。下面是一个例子：  
第一个字符串将Opera 9.5标识为Firefox 2,同时带有Opera版本信息。第二个字符串将Opera 9.5 标识为IE6,也包含了 Opera版本信息。这两个用户代理字符巾可以通过针对Firefox或IE的大多数测 试，不过还是为识Opera留下了余地。  
Opera标识自身的另一种方式，就是把白己装扮成Firefox或IE。在这种隐瞒真实身份的情况下，用 户代理字符串实际上与其他浏览器返间的相同——既没有Opera字样，也不包含Opera版本信息。换 句话说，在启用了身份隐蹒功能的情况下，无法将Opera和其他浏览器区别开来。另外，由于Opera莒 欢在不告知用户的情况下针对站点来设置用户代理字符串，因此问题就更复杂化了。例如，打开My Yahoo!站点（
http://my.yahoo.com )会自动导致Opera将自己装扮成Firefox。如此一来，要想识別Opera 就难上加难了。  
在Opera7以前的版本中，Opera会解析Windows操作系统字符串的含义。例如， Windows NT 5.1实际上就是Windows XP,因此Opera会在用户代理字符串中包含 Windows XP而非Windows NT 5.1。为了与其他浏览器更兼容，Opera 7开始包含正式 的操作系统版本，而非解析后的版本。
Opera 10对代理字符串进行了修改。现在的格式是：
Opera/9.80 (操作系统或CPU;加密类螌;语言> Presto/Presto版本号Version/版本亏
注意，初始的版本号Opera/9.80是固定不变的。实际并没有Opera9.8,怛工程师们担心写得不好的 浏览器嗅探脚木会将Opera/lO.O错识的解释为Opera〖，而不是Opera 10。因此，Opera 10 乂增加了 Presto 记号（Presto是Opera的S现引擊）和Version记号，后者用以保存实际的版本号。以下是Windows7中 Opera丨0.63的用户代理字符串：
Opera/5.80 (Windows NT 6.1; U; cn) ?resto/2.6.30 Versi〇n/10.63
iOS 和 Android
移动操作系统i〇S和Android默认的浏览器都基于WebKit,而且都像它们的桌面版一样，共享相同 的基木用户代理字符串格式。i〇S设备的基本格式如下：
Mozilla/S.O (平台;加密类型;操作系统或CPU like Mac OS X;语言）
AppleWebKit/AppleWebKit 版本号（KHTML, like Gecko) Version/浏览器版本号 Mobile/移动版本号Safari/Safari版本号
注意用于辅助确定Mac操作系统的"like Mac OS x■和额外的Mobile记号。一般来说，Mobile 记号的版本9 (移动版本号）没什么用，主要是用来确定WebKit是移动版，而非桌面版。而平台则可 能是"1卩110116"、"12131"或"1231"。例如：
Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us)
AppleWebKit/528.18 (KHTML, Uke Gecko > Vcrsion/4.0 M〇bUe/7A341 Safari/528.16
在iOS 3之前，用户代理卞符串中不会出现操作系统版本号。
Android浏览器中的默认格式与iOS的格式相似，没有移动版本号（fi有Mobile记号)。例如：
Mozilla/5.0 (Linux; U; Android 2.2; en-us; Nexus One Build/FRF91)
AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1
这是Google Nexus One手机的用户代理字符申。不过，其他Android设备的模式也一样。  

###  9.3.2 用户代理字符串检测技术  

考虑到历史原W以及现代浏览器中用户代理字符串的使用方式，通过用户代理字符串来检测特定的 浏览器并不是-件轻松的亊。因此，首先要确定的往往是你需要多么具体的浏览器信息。一般情况下， 知道呈现引擎和最低限度的版本就足以决定正确的操作方法了。例如，我们不推荐使用下列代码：
if (isIE6 II isIE7> { //不推荐！！！
//代码
}
这个例+是想要在浏览器为IE6或IE7时执行相应代码。这种代码其实是很脆弱的，因为它要依据 特定的版本来决定做什么。如果是怎么办呢？只要IE有新版本出来，就必须更新这些代码。不过， 像下面这样使用相对版木兮则可以避免此问题：
if (icVcr >=6){
//代码
>
这个例子首先检测IE的版本号是否至少等于6,如果是则执行相应操作。这样就可以确保相应的代 码将来照样能够起作用。我们F面的浏览器检测脚本就将本着这种思路来编写。  

**1.识别呈现引擎**  

如前所述，确切知道浏览器的名字和版本号不如确切知道它使用的是什么呈现引擎。如果Firefox、 Camino和Netscape都使用相间版本的Gecko,那它们一定支持相同的特性。类似地，不管是什么浏览 器，只要它跟Safari 3使用的是同一个版本的WebKit,那么该浏览器也就跟Safari 3具备同样的功能。 因此，我们要编¥的脚本将主要检测五大呈现引擎：IE、Gecko、WebKit、KHTML和Opera。
为了不在全局作用域中添加多余的变fi,我们将使用模块增强模式来封装检测脚本。检测脚本的基 本代码结构如下所示：
var client = function〇{ var engine c {
//X现引擎 ie： 0, gecko： 0, webkit： 0, khtml: 0, opera： 0,
//具体的版本号 ver: null
};
//在此检測呈现？丨擎、平台和设备
return {
engine ： engine
};
M);
这里卢明了 •个名为client的全局变M,用于保存相关信息。匿名函数内部定义了一个局部变* engine,它是一个包含默认设S的对象字面最。在这个对象字面量中，每个呈现引擎都对应着一个厲 性，属性的值默认为0。如果检测到了哪个呈现引擎，那么就以浮点数值形式将该引擎的版本号写入相 应的属性。而呈现引擎的完整版本（是一个字符串），则被写人ver属性。作这样的区分可以支持像下 面这样编写代码：
if (client.engine.ie) {//如果是 IE,client.ie 的值应该大于 0 //针对IE的代码
} else if {client.engine.gecko > 1.5){ if (client.engine.ver == ”1.8.1”）{
//针对这个版本执行某些操作
}
}
在检测到一个呈现引擎之后，其client.engine中对应的属性将被设置为个大于〇的值，该值 可以转换成布尔值true。这样，就可以在if语句中检测相应的屈性，以确定当前使用的呈现引擎，连 具体的版本号都不必考虑。鉴丁•每个属性都毡含•个浮点数值，因此有可能丢失某些版本信息。例如， 将字符串>1_8.1 ■传人parseFloatO后会得到数值丨.8。不过，在必要的时候可以检测ver属性，该 属性中会保存完整的版本信息。
要正确地识别呈现引擎，关键是检测顺序要正确。由于用户代理字符串存在诸多不一致的地方，如 果检测顺序不对，很可能会导致检测结果不正确。为此，第一步就是识别Opera,因为它的用户代理字	
符串有可能完全模仿其他浏览器。我们不相信Opera,是因为（任何情况下）其用户代理字符串（都） 不会将自己标识为Opera。
要识别Opera,必须得检测window.opera对象。Opera 5及更髙版本中都有这个对象，用以保存 与浏览器相关的标识信息以及与浏览器直接交互。在Opera 7.6及更高版本中，调用versionO方法可 以返回一个表示浏览器版本的字符串，而这也是确定Opera版本兮的最佳方式。要检测更早版本的Opera, 可以直接检査用户代理字符串，因为那些版本还不支持隐瞒身份。不过，2007底Opera的最高版本已经 是9.5 了，所以不太可能有人还在使用7.6之前的版本。那么，检测呈现引擎代码的第一步，就是编写 如下代码：
if (window.opera){
engine.ver = window.opera.version(); engine.opera = parseFloat{engine.ver);
}
这里，将版本的字符串表示保存在了 engine.ver中，将浮点数值表示的版本保存在了 engine.opera中。如果浏览器是Opera,测试window.opera就会返回true;否则，就要看看是其 他的什么浏览器了。
应该放在第二位检测的呈现引擎是WebKit。因为WebKit的用户代理字符串中包含"Gecko"和 "KHTML•这两个子字符串，所以如果首先检测它们，很可能会得出错误的结论。
不过，WebKit的用户代理字符串中的•AppleWebKit•是独一无二的，因此检测这个字符串最合适。 下面就是检测该字符串的示例代码：
var ua = navigator.userAgent;
if (window.opera){
engine.ver = window.opera.version{); engine.opera = parseFloat(engine.ver);
} else if (/AppleWebKit\/(\S-f )/.test<ua)) { engine.ver ■ RegrBxp["$l"】z engine.webkit « parsePloat(engine.ver);
)
代码首先将用户代理字符串保存在变量ua中。然后通过lE则表达式来测试其中是否包含字符串
•AppleWebKiC'并使用捕获组来取得版本号。由于实际的版本号中可能会包含数字、小数点和字母, 所以捕获组中使用了表示非空格的特殊字符（\S)。用户代理字符串中的版本号与下一部分的分隔符是 一个空格，因此这个模式可以保证捕获所有版本信息。test ()方法基于用户代理字符串运行正则表达 式。如果返回true,就将捕获的版本号保存在engine.ver中，而将版本号的浮点表示保存在 engine.webkit中。WebKit版本与Safari版本的详细对应情况如下表所示。
(续）
t
有时候，Safari版本并不会与WebKit版本严格地一一对应，也可能会存在某些小
版本上的差异。这个表中只是列出了最可能的WebKit版本，但不保证精确。
接下来要测试的呈现引擎是KHTML。N样，KHTML的用户代理字符串中也包含-Gecko",因此 在排除KHTML之前，我们无法准确检测基于Gecko的浏览器。KHTML的版本号与WebKit的版本号 在用户代理字符串中的格式差不多，因此可以使用类似的正则表达式。此外，由于Konqueror 3.1及更 早版本中不包含KHTML的版本，故而就要使用Konqueror的版本来代替。下面就是相应的检测代码。
与前面一样，由于KHTML的版本号与后继的鉍记之间冇-个空格，因此仍然要使用特殊的非空格 字符来取得与版本有关的所有宇符。然后，将字符串形式的版本信息保存在engine .ver中，将浮点数 值形式的版本保存在engin.khtml中。如果KHTML不在用户代理字符串中，那么就要匹配Konqueror 后跟一个斜杠，再后跟不包含分号的所有字符。
在排除了 WebKit和KHTML之后，就可以准确地检测Gecko 了。但是，在用户代理字符串中，Gecko 0 的版本号不会出现在字符串"Gecko"的后面，而是会出现在字符串"rv:"的后面。这样，我们就必须使 用一个比前面复杂-些的正则表达式，如下所示。
Gecko的版本弓-位T字符;U”rv:"与-个闭括5•之间，因此为了提取出这个版本号，正则表达式要 査找所有+是闭括号的字符，还要査找宁符串"Gecko/"后跟8个数字。如果上述模式闪配，就提取出 版本号并将M保存在相应的厲性中。Gecko版本号与Firefox版本号的对应关系如下表所示。
与Safari跟WebKit —样，Firefox与Gecko的版本号也不一定严格对应。
最后一个要检测的呈现引擎就是IE 了。IE的版本号位于字符串"MSIE"的后面、一个分号的前面， 因此相应的正则表达式非常简单，如下所示：
以上呈现引擎检测脚本的最后一部分，就是在正则表达式中使用取反的字符类来取得不是分兮的所 冇字符。IE通常会保证以标准浮点数值形式给出芄版本号，但有时候也不一定。因此，取反的字符类t A ,•] 可以确保取得多个小数点以及任何可能的字符。  

**2.识别浏览器**  

大多数情况下，识别r浏览器的m现引擎就足以为我们采取正确的操作提供依据了。可是，只有m 现引擎还不能说明存在所需的JavaScript功能。苹果公司的Safari浏览器和谷歌公司的Chrome浏览器都 使用WebKit作为呈现引擎，但它们的JavaScript引擎却不一样。在这两款浏览器中，client, webkit 都会返问非0值，但仅知道这一点恐怕还不够。对于它们，有必要像下面这样为client对象再添加一押
新的属性。
代码中又添加了私有变1 browser,用于保存每个主要浏览器的属性。与engine变量一样，除 了当前使用的浏览器，其他属性的值将保持为0;如果是当前使用的浏览器，则这个属性中保存的是浮 点数值形式的版本号。同样，ver属性中在必要时将会包含字符串形式的浏览器完整版本号。由于大 多数浏览器与其呈现引擎密切相关，所以下面示例中检测浏览器的代码与检测呈现引擎的代码是混合 在一起的。
//检测呈现幻学及浏JL忍
对Opera和IE而言，browser对象中的值等T engine对象中的值。对Konqueror而存，browser, konq 和 browser, ver 属性分别等于 engine, khtml 和 engine .ver 属性。
为了检测Chrome和Safari,我们在检测弓丨擎的代码中添加f if语句。提取Chrome的版本号时，需 要査找字符串"Chrome/"并取得该字符串后面的数值。而提取Safari的版本号时，则需要査找字符串 "Version/•并取得其后的数值。由于这种方式仅适用于Safari 3及更髙版本，因此需要一些备用的代 码，将WebKit的版本号近似地映射为Safari的版本号（参见上一小节中的表格)。
在检测FirefoxW版本时，首先要找到字符串"Firefox/",然后提取出该字符串后面的数值（即版 本号)。当然，只有呈现引擎被判别为Gecko时才会这样做。
有了上面这些代码之后，我们就可以编写下面的逻辑。
```
if (client.engine.webkit) { //if it's WebKit if (client.browser.chrome)(
//执行針对Chrome的代码 } else if (client.browser.safari){
//执行针对Safari的代码
}
 else if (client.engine.gecko){ if (client.browser.Cirefox){
//执行针对Firefox的代码 } else {
，/执行针对其他Gecko到览器的代码
}
)
 ```
**3.识别平台**  

很多时候，只要知道呈现引擎就足以编W出适当的代码了。但在某些条件下，平台可能是必须关注 的问题。那些具有各种平台版本的浏览器（如Safari、Firefox和Opera)在不同的平台下可能会有不同 的问题。目前的三大主流平台是Windows、Mac和Unix (包括各种Linux九为了检测这些平台，还需 要像下面这样再添加一个新对象。
显然，上面的代码中又添加/■一个包含3个属性的新变量system。其中，win属性表示是否为 Windows平台，mac表示Mac, Iftjxll表示Unix。与呈现引擎不同，在不能访问操作系统或版本的 情况下，平台信息通常是很有限的。对这三个平台而言，浏览器一般只报告Windows版本。为此，新 变童system的每个属性最初都保存着布尔值false，而不是像呈现引擎属性那样保存着数字值。
在确定T-台时，检测navigator .platform要比检测用户代理字符串更简单，后者在不同浏览器 中会给出不同的平台信息。而navigator.platform属性可能的值包括”Win32”、*Win64*、
uMacPPC"、"Maclntel"、•和-Linuxi686-,这些值在不同的浏览器中都是一致的。检测平台 的代码非常A观，如下所示：
以上代码使用indexOf ()方法来査找平台字符串的开始位置。虽然” Win32"是当前浏览器唯一支 持的Windows卞符串，但随若向64位Windows架构的迁移，将来很可能会出现"Win64*平台信息值。 为丫对此乜所准备，检测平台的代码中査找的只是字符串"Win•的开始位置。时检测Mac平台的方式也 类似，同样足考虎到了 MacPPC和Maclntel。在检测Unix时，则同时检査了字符串"XII”和-Linux” 在平台字符串中的开始位置，从Ifli确保了代码能够向前兼容其他变体。
	Gecko的早期版本在所有Windows平台中都返回字符串•Windows'1,在所有Mac平
台中則都返回字符串-Macintosh*。不过，这都是Firefox 1发布以前的事了，Firefox 1
确定了 navigator.platform 的值  

**4.识别Windows操作系统**  

在Windows平台下，还可以从用户代理字符串中进一步取得具体的操作系统信息。在Windows XP
之前，Windows有两种版本，分别针对家庭用户和商业用户。针对家庭用户的版本分别是Windows95、
98和Windows ME。而针对商业用户的版本则一直叫做Window NT,最后由于市场原因改名为Windows
2000。这两个产品线后来又合并成-个由Windows NT发展而来的公共的代码基,代表产品就是Windows
XP。随后，微软在Windows XP#础上又构建了 Windows Vista0
只有了解这些信息，才能搞清楚用户代理字符串中Windows操作系统的具体版本。下表列出了不同
浏览器在表不+同的Windows操作系统时给出的不同字符串。
由于用户代理字符串中的Windows操作系统版本表示方法各异，因此检测代码并不十分直观。好在, 从Windows 2000开始，表示操作系统的字符串人部分都还相同，只有版本号有变化。为了检测不同的 Windows操作系统，必须要使用正则表达式。由丁•使用Opera 7之前版本的用户已经不多了，因此我们 以忽略这部分浏览器。
第-步就是匹配Windows 95和Windows 98这两个字符串。对这两个字符出，只有Gecko与其他浏 览器不同，即没有_<3〇«£3•，而且-Win••与版本5之间没有空格。要K配这个模式，可以使用下面这个简 单的正则表达式。
/Win{V:dows )?(trtd〇l{2})/
这个正则表达式中的捕获组会返回操作系统的版本。ttr丁-版本可能是任何两个字符编码（例如95、 98、9x、NT、ME及XP),因此要使用两个非空格字符。
Gecko在表示Windows NT时会在末M添加《4.0",与其査找实际的字符串，不如像下面这样査找 小数值更合适。
/Win(?:dows )?([^do]{2})(\d+\.\d+)?/
这样，iH则表达式中就包含了第二个捕获组，用于取得NT的版本号。由于该版本号对于Windows 95和Windows 98而言是不存在的，所以必须设置为可选。这个模式与Opera表示Windows NT的字符 串之间唯一的区别，就是"NT”与"4.0•之间的空格，这在模式中很容易添加。
/Win(?:dows )?(tAdo]{2})\b?(\d+\.\d+)?/
经过一番修改之后，这个正则表达式也可以成功地K配Windows ME、Windows XP和Windows Vista 的字符串了。具体来说，第一个捕获组将会叫配95、98、9x、NT、ME或XP。第二个捕获组则只针对 Windows ME及所有Windows NT的变体。这个信息可以作为具体的操作系统信息保存在system.win 属性中，如下所示。
如果system.win的值为true,那么就使用这个正则表达式从用户代理字符串中提取具体的信息。 鉴于Windows将来的某个版本也许不能使用这个方法来检测，所以第一步应该先检测用户代理字符串是 否与这个模式四配。在模式匹配的情况下，第一个捕获组中可能会包含"95"、"98»、"9x"或”NT"。如 果这个值是"NT"，可以将system.win设置为相应操作系统的字符串;如果是"9x"，那么system.win 就要设置成"ME";如果是丼他值，则将所捕获的值直接赋给system.win。有了这《检测平台的代码后， 我们就可以编写如F代码。
if (client.system.win){
if (client.system.win "XPB) {
//说明是XP
} else if (client.system.win == "Visr.a") {
//说明是Vista
}
>
由J•非空字符申会转换为布尔值true, W此可以将client .system.win作为布尔值用在if语 句屮。而在谣要史多有关操作系统的信思时，贝何以使用其中保存的字符串  

**5.识别移动设备**  

2006年到2007年，移动设备中Web浏览器的应用呈爆炸性增长。四大主要浏览器都推出了手机 版和在其他设备中运行的版本。要检测相应的设备，第一步是为要检测的所冇移动设备添加属性，如 下所示。
var client = function(){
var engine = {
//呈现引擎 ie： 0, gecko： 0, webkit： 0, kfcfcml： 0, opera： 0,
//具体的版本号 vers null
/;
var browser = {
//浏定器 ie： 0, firefox： 0, safari: 0, konq： 0, opera： 0, chrome: 0,
//具体的版本号 ver： null
var system = { win: false# mac： false,
xll: false,
//移劝设备 iphone: false, ipod: false, ipad: false, ios： £alae, android: £alae# nokiaN: £alae# wlnMobile： false };
//在此捡測呈现引學、平台和设各
return {
engine： engine, browser: browser, system： system
);
}();
然后，通常简单地检测字符串"iPhone"、"iPod"和”iPad-,就可以分别设置相应属性的值了。
system.iphone = ua.indexOf("iPhone*) >	1;
system.ipod = ua.indexOf{"iPod") > -1?
system.ipod = ua.indexOf{"iPad") > -1;
除了知道iOS设备,最好还能知道〖OS的版本兮。在iOS 3之前，用户代理字符串中只包含-CPU like Mac OS•，后来 iPhone中乂改成-CPU iPhone OS 3_0 like Mac OS X", iPad中又改成"CPU OS 3_2 like Mac OS X•。也就是说，检测iOS需要正则表达式反映这些变化。
//拴测iOS版本
if (system.mac && ua.indexOf("Mobile") > -1){
if (/CPU (?:iPhone )70S (\d+_\d+)/.test(ua)){
system.ios = parsePloat(RegExp.$1.replace(,"."));
} else {
system.ios =2; //不轮真正裣测出来，所以只能猜测
>
>
检查系统是不是Mac 0S、字符串屮是否存在"Mobile",可以保证无论是什么版本，system, ios 中都不会是0。然后，再使用正则表达式确定是否存在i〇S的版本号。如果冇，将system, ios设置为 表示版本号的浮点值;否则，将版本设ft为2。（因为没有办法确定到底是什么版本，所以设置为更早的 版本比较稳妥。）
检测Android操作系统也很简单，也就是搜索字符串"Android"并取得紧随其后的版本号。
//检測Android板冬
i£ (/Android (\d+\.\d+)/.test(ua)){
system.android = parseFloat(RegExp.$1);
}
由于所有版本的Android都有版本值，因此这个正则表达式吋以精确地检测所有版本，并将 system .android设置为正确的值。
诺基亚N系列手机使用的也是WebKit,其用户代理字符串勺其他基于WebKit的手机很相似，例如：
Mozilla/5.0 (SymbianOS/9.2; Q; Series60/3.1 NokiaN95/ll.0.026; Profile MIDP-2.0 Configuration/CLDC-1.1) AppleWebKit/413 (KHTML, like Gecko) Safari/413
里然诺基亚N系列手机在用户代理字符中中声称使用的是"Safari",但实际上并不是Safari,尽 管确实娃基于WebKit引擎。只要像下面检测~ K用户代理字符串中M否存在"NoXiaN”，就足以确定是 不垃该系列的手机了。
system.nokiaN = ua.indexOf("NokiaN") > -1;
在了解这些设备信息的基础上，就吋以通过K列代码来确定用户使用的是什么设备中的WebKit来 访问网页：
if (client.engine.webkit){ if (client.system. iOS){
//i〇S手机的内容
} else if (client.system.android){
//Android手机的六容 } else if {client.system.r.okiaK) {
//诺基亚手机的内容
最后一种主要的移动设备平f?是Windows Mobile (也称为Windows CE),用于Pocket PC和 Smartphone中。由于从技术hi兑这些平台都属于Windows平台，因此Windows平台和操作系统都会返 回正确的值。对于WindowsMobile5.0及以前版本，这两种设备的用户代理字符串非常相似，如下所示:
Mozilla/4.0 (compatible; MSIE 4.01; Windows CE; PPC; 240x320)
Mozilla/4.0 (compatible; MSIE 4.01; Windows CE; Smartphone; 176x220)
第-个来ft Pocket PC中的移动Internet Explorer 4.01，第二个来白Smartphone中的同一个浏览器。 当Windows操作系统检测脚本检测这两个字符串时，system.win将被设置为”CE",因此在检测 Windows Mobile时可以使用这个值：
system.winMobile = {system.win =* "CE");
不建议测试字符串中的’_PPCni^" Smartphone1*，因为在Windows Mobile 5.0以后版本的浏览器中， 这些id号已经被移除/。不过，一般悄况下，只知道某个设备使用的是WindowsMobile也就足够了。 Windows Phone 7的用户代理字符串稍有改进，基本格式如下：
Mozilla/4.0 (compatible; MSIE 7.0; Windows Phone OS 7.0; Trident/3.1; IEMobile/7.0) Asus;Galaxy6
其中，Windows操作符的标识符与已往完全不同，因此在这个用户代理中client.system.v/in 等于”PH%从中可以取得宥关系统的更多倍息：
//windows mobile if (sysnerr,.win == "CE") {
system.winMobile = system.win;
} else if (system.win == *Php){
if(/Windows Phone OS {\d+.\d+)/.test <ua)){; system.win "Phone";
system. winMobile =： parseFloat (RegExpf" $1"]);
如果system.win的值是"CE",就说明是老版本的Windows Mobile,因此system.winMobile 会被设置为相同的值（只能知道这个信息）。如果system.win的值是"Ph_,那么这个设备就可能是
Windows Phone 7或更新版本。因此就用正则表达式来测试格式并提取版本号，将system.win的值重 置为*Phone%而将system.winMobile设置为版本号。  

**6.识别游戏系统**  

除了移动设备之外，视频游戏系统中的Web浏览器也开始日益普及。任天堂Wii和PlayStati〇n3或 者内置Web浏览器，或者提供了浏览器下载。Wii中的浏览器实际上是定制版的Opera,是专门为Wii Remote设计的。Playstation的浏览器是自己开发的，没有基于前面提到的任何呈现引擎。这两个浏览器 中的用户代理字符串如下所示：
Opera/9.10 (Nintendo Wii;U; ; 1621; en)
M〇zilla/5.Q (PLAYSTATION 3; 2.00)
第一个字符串来自运行在Wii中的Opera,它忠实地继承了 Opera最初的用户代理字符串格式（Wii 上的Opera不具备隐瞒身份的能力)。第二个字符串来自Playstation3,虽然它为了兼容性而将ft己标识 为Mozilla 5.0,但并没有给出太多信息。而且，设备名称居然全部使用了大写字母，让人觉得很奇怪; 强烈希望将来的版本能够改变这种情况。
在检测这些设备以前，我们必须先为client.system中添加适当的属性，如下所示：  

###  9.3.3 完整的代码  

以下是完整的用户代理字符串检测脚本，包括检测呈现引擎、平台、Windows操作系统、移动设备 和游戏系统。
```javascript
var client = function(){

	//呈现引擎
	var engine = {
		ie: 0,
		gecko: 0,
		webkit: 0,
		khtml: 0,
		opera: 0,
		
		//完整的版本号
		var: null;
	};
	
	//浏览器
	var browser = {
		
		//主要浏览器
		ie: 0,
		firefox: 0,
		safari: 0,
		konq: 0,
		opera: 0,
		chrome: 0,
		
		//完整的版本号
		var: null;
	};
	
	
	//
}
```
###  9.3.4 使用方法
我们在前面已经强调过了，用户代理检测是客户端检测的最后…个选择。只要可能，都应该优先采 用能力检测和怪癖检测。用户代理检测一般适用于下列情形。
- [ ] 不能直接准确地使用能力检测或怪癖检测。例如，某些浏览器实现了为将来功能预留的存根 (stub)函数。在这种情况下，仅测试相应的函数是否存在还得不到足够的信息。
- [ ] 同一款浏览器在不同平台K具备不同的能力。这时候，可能就有必要确定浏览器位于哪个平 台下。
- [ ] 为了跟踪分析等口的需要知道确切的浏览器。  

##  9.4 小结
客户端检测是JavaScript开发中最具争议的一个话题。由于浏览器间存在差别，通常需要根据不同 浏览器的能力分别编写不间的代码。有不少客户端检测方法，但下列是最经常使用的。
- [ ] **能力检测：**在编写代码之前先检测特定浏览器的能力。例如，脚本在调用某个函数之前，可能 要先检测该函数是否存在。这种检测方法将开发人员从考虑具体的浏览器类型和版本中解放出 来，让他们把注意力集屮到相应的能力是否存在上》能力检测尤法精确地检测特定的浏览器和 版本。
- [ ] **怪癖检测：**怪癖实际上是浏览器实现中存在的bug,例如早期的WebKit中就存在一个怪癖，即 它会在for-in循环屮返M被隐藏的属性。怪癖检测通常涉及到运行一小段代码，然后确定浏 览器是否存在某个怪癖。由于怪癖检测与能力检测相比效率更低，因此应该只在某个怪癖会干 扰脚本运行的情况下使用。怪癖检测无法精确地检测特定的浏览器和版本。
- [ ] **用户代理检测：**通过检测用户代理字符串来识别浏览器。用户代理字符串中包含大量与浏览器 有关的信息，包括浏览器、平台、操作系统及浏览器版本。用户代理字符串有过一段相当长的 发展历史，在此期间，浏览器提供商试图通过在用户代理字符串中添加一些欺骗性信息，欺骗 网站相信自Q的浏览器是另外一种浏览器。用户代理检测需要特殊的技巧，特别是要注意Opera 会隐瞒其用户代理宁符串的情况。即便如此，通过用户代理字符串仍然能够检测出浏览器所用 的M现引擎以及所在的平台，包括移动设备和游戏系统。
在决定使圯哪种客户端检测方法时，一般应优先考虑使用能力检测。怪癖检测是确定应该如何处理 代码的第二选择。而用户代理检测则是客户端检测的最后一种方案，因为这种方法对用户代理字符串具 有很强的依赖性。  
[上一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter8.md)  [下一章](https://github.com/qianjilou/javascript3/blob/master/chapter/chapter10.md)