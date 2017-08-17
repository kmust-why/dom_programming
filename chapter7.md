# JavaScript Dom编程艺术

<h1>第7章 动态创建标记</h1>
<h2>7.1 一些传统方法</h2>
<h3>7.11 document.write</h3>
<h3>7.12 innerHTML属性</h3>
<h2>7.2 DOM方法</h2>
<h3>7.21 createElement方法</h3>
<h3>7.22 appendChild方法</h3>
<h3>7.23 createTextNode方法</h3>
<h3>7.24 一个更复杂的组合 4</h3>
<h2>7.3 重回图片库</h2>
<h3>7.31 在已有元素前插入一个新元素</h3>
<h3>7.32 在现有方法后插入一个新元素</h3>
<h3>7.33 图片库二次改进版</h3>
<h2>7.4 Ajax</h2>
<h3>7.41 XMLHttpRequest对象</h3>
<h3>7.42 渐进增强与Ajax</h3>
<h3>7.43 Hijax</h3>
<h2>7.5 小结</h2>



- [简介](README.md)

本章内容：&
〇传统技术:'document .write 和 tnnerHTML
、: '爾
.:—— ■，……-，，讀藝誠機pff…
(K-
^u： ^




本“	..	• •:.嚅起.，	分…•‘. ^*^；；.：•.• i•. mx- ：.. i：^：if,^^	广“ 乂鑛
_t:〇 深入剖析 D03VI 方法createE&ent“createfext咖de€ra叩en^
此前见过的绝大多数DOM方法只能用来査找元素。getElementByld和getElementsByTagName |
都可以方便快捷地找到文档中的某个或某些特定的元素节点，这些元素随后可以用诸如 setAttribute (改变某个属性的值）和nodeValue (改变某个元素节点所包含的文本）之类的方法 和属性来处理。我的图片库就是这样实现的。showP'ic函数先找出Id属性值是placeholder和 description的两个元素，然后刷新它们的内容。placeholder元素的six属性是用setAttribute修 改的，description元素所包含的文本是用nodeValue属性修改的。在这两种情况里，都是对已经存：
在的元素做出修改。
这是绝大多数JavaScript函数的工作原理。网页的结构由标记负责创建，JavaScript函数只用 来改变某些细节而不改变其底层结构。JavaScript也可以用来改变网页的结构和内容。本章中， 你将学习一些DOM方法，通过创建新元素和修改现有元素来改变网页结构。
—些传统方法
在学习利用DOM方法在Web浏览器中往文档添加标记时，先回顾两个过去使用的技术，即 document. w「1 te 和M nnerHTML。
document, write
document对象的write。方法可以方便快捷地把字符串插入到文档里。 请把以下标记代码保存为一个文件，文件名就用test.html好了。
<!D0CTYPE html>
<html lang="en’’>
<head>
<meta charset=lfutf-8n />
<title>Test</title>
</head>
<body>
<script>
document.write(1,<p>This is inserted•</p>11);

7.1 —些传统方法 97
</script>
</body>
</html>
如果把test.html文件加载到Web浏览器里，你将看到内容为“This is inserted.”的文本段落，
如图7-1所示。






document.write的最大缺点是它违背了“行为应该与表现分离”的原则。即使把document.write 语句挪到外部函数里，也还是需要在标记的必〇(^>^分使用<scr1pt>标签才能调用那个函数。
下面这个函数以一个字符串为参数，它将把一个<P>标签、字符串和一个</p>标签拼接在一起。 择接后的字符串被保存到变量str，然后用document.write()方法写出来：
干unction insertParagraph(text) { var str = __<p>"; str += text; str += "</p>”； document.write(str);
可以把这个函数保存在外部文件example.js里。为了调用这个函数，必须在标记里插入 scripts标签：
<!D0CTYPE html>
<html lang=nenn>
<head>
<meta charset,utf-8” />
<title>Test</title>
</script>
</head>
<body>
<script src=Mexample*jsH>
<script>
insertParagraph(flThis is inserted/1);
</script>
</body>
</html>
像上面这样把JavaScript和HTML代码混杂在一起是一种很不好的做法。这样的标记既不容 易阅读和编辑，也无法享受到把行为与结构分离开来的好处。

98 第7章动态创建标记
这样的文档还很容易导致验证错误。比如说，在第一个例子里，<sc「1pt>标签后面的“<p>” 很容易被误认为是<P>标签，而在巧(：「1卩1>标签的后面打开<p>标签是非法的。事实上，那个“<p>” 和“</p>”只不过是一个将被插入文档的字符串的组成部分而已。
还有，MIME类型appl 1 cat彳on/xhtml +xml与document • wrl te不兼容，浏览器在呈现这种XHTML
文档时根本不会执行document.write方法。
从某种意义上讲，使用document.write方法有点儿像使用<font>fe签去设定字体和颜色。虽 然这两种技术在HTML文档里的都工作得不错，但它们都不够优雅。
把结构、行为和样式分开永远都是一个好主意。只要有可能，就应该用外部CSS文件代替 <font>标签去设定和管理网页的样式信息，最好用外部JavaScript文件去控制网页的行为。应该 避免在<bocb^P分乱用<scr1pt>标签，避免使用document.write方法。
innerHTML 属性
现如今的浏览器几乎都支持属性1 nnerHTML，这个属性并不是W3C DOM标准的组成部分，但现 已经包含到HTML5规范中。它始见于微软公司的正4浏览器，并从那时起逐渐被其他的浏览器接受。
InnerHTML属性可以用来读、写某给定元素里的HTML内容。要了解它如何工作，请把下面 这段代码插入test. html文档的<body>部分：
<div id=ntestdivn>
<p>This is <em>my</em> contentx/p>
</div>
用DOM的眼睛看testdlv内的标记，是如图7-2所示的结构。
兀系T热
属性节点






3S
文本节点
图7-2

7.1 —些传统方法 99
dlv元素的Id是testdiv。它包含一个元素节点（p元素)。这个p元素又有一些子节点。其 中有两个文本节点，值分别是This is和content。还有一个元素节点（em元素），em元素本身包 含一个文本节点，这个文本节点的值是my。
DOM提供了关于这个标记的非常详细的一幅图画。使用DOM提供的方法和属性可以对任 何一个节点进行单独的访问。这个标记从InnerHTML属性的角度来看则简单得多，如图7-3所示， 就1 nnerHTML属性看来，1 d为testdi v的标记里面只有一个值为<p>Th1 s彳s <em>my</em> content. </p> 的HTML字符串。
元素节点



<p>This is <em»ny</em> content.</p> ||
HTML
图 7-3
用下面这个新函数更新example, js文件。
window.onload = function() { var testdiv = document•getElementById(lltestdiv,<); alert(testdiv^innerHTML);
>
然后在Web浏览器里刷新test • html页面，dl v元素(它的1 d属性值等于testdl v)的1 nnerHTML 翼性值将显示在一个alert对话框里，如图7-4所示。



图 74
很明显，InnerHTML属性无细节可言。要想获得细节，就必须使用DOM方法和属性。标准化 的DOM像手术刀一样精细，InnerHTML属性就像一把大锤那样粗放。
大锤有大锤的用武之地。在你需要把一大段HTML内容插入网页时，InnerHTML属性更适用。 它既支持读取，又支持写入，你不仅可以用它来读出元素的HTML内容，还可以用它把HTML

100 第7章动态创建标记
内容写入元素。
编辑test.html文件，让Id属性值等于testdlv的元素变成空白：
<div id="testdiv">
</div>
把下面这段JavaScript代码放入example.js文件，就可以把一段HTML内容插入这个<chv> 标签：
window,onload = function() { var testdiv = document^getElementById(,ltestdivt,); testdiv.innerHTML = "<p>I inserted <em>this</em> contentx/p>M;
}
在Web浏览器里刷新test.html文件，你就可以看到如图7-5所示的结果。
图7-5
利用这个技术无法区分“插入一段HTML内容”和“替换一段HTML内容”。testdiv元素 里有没有HTML内容无关紧要：一旦你使用了 InnerHTML属性，它的全部内容都将被替换。
在test.html文件里，把Id属性值等于testdiv的元素的内容修改回它原来的样子：
<div id="testdiv">
<p>This is <em>my</em> contentx/p>
</div>
example.js文件保持不变。如果你在Web浏览器里刷新test.html文件，结果将和刚才一样。 包含在testdiv元素里HTML内容被InnerUTML属性完全改变了，盧来的HTML内容未留下任何 痕迹。
在需要把一大段HTML内容插入一份文档时，1nne「Hm属性可以让你又快又简单地完成这 一任务。不过，1nne「HTML属性不会返回任何对刚插入的内容的引用。如果想对刚插入的内容进 行处理，则需要使用DOM提供的那些精确的方法和属性。
I’nnerHTML属性要比document.wrlteO方法更值得推荐。使用1nne「HTML属性，你就可以把 JavaScript代码从标记中分离出来。用不着再在标记的<b〇dy>部分插人<5〇^1:>标签。
类似于document.write方法，InnerHTML属性也是HTML专有属性，不能用于任何其他标记 语言文档。浏览器在呈现正宗的XHTML文档(即MIME类型是applIcatlon/xhtml+xml的XHTML 文档）时会直接忽略掉InnerHTML属性。
在任何时候，标准的DOM都可以用来替代InnerHTML。虽说这往往需要多编写一些代码才能 获得同样的效果，但DOM同时也提供了更高的精确性和更强大的功能。

7.2 DOM 方法 101
DOM方法
getElementByld和getElementsByTagName等方法可以把关于文档结构和内容的信息检索出来， 它们非常有用。
DOM是文档的表示。DOM所包含的信息与文档里的信息一一对应。你只要学会问正确的问 題（使用正确的方法），就可以获取DOM节点树上任何一个节点的细节。
DOM是一条双向车道。不仅可以获取文档的内容，还可以更新文档的内容。如果你改变了 DOM节点树，文档在浏览器里的呈现效果就会发生变化。你已经见识过setAttrlbute方法的神 奇之处了。用这个方法可以改变DOM节点树上的某个属性节点，相关文档在浏览器里的呈现就 会发生相应的变化。不过，setAtthbute方法并未改变文档的物理内容，如果用文本编辑器而不 是浏览器去打开这个文档，我们将看不到任何变化。只有在用浏览器打开那份文档时才能看到文 档呈现效果的变化。这是因为浏览器实际显示的是那棵DOM节点树。在浏览器看来，DOM节 点树才是文档。
一旦明白了这个道理，以动态方式实时创建标记就不那么难以理解了。你并不是在创建标记， 面是在改变DOM节点树。做到这一点的关键是一定要从DOM的角度去思考问题。
在DOM看来，一个文档就是一棵节点树。如果你想在节点树上添加内容，就必须插入新的 节点。如果你想添加一些标记到文档，就必须插入元素节点。
createEl ement 方法
编辑test.html文件，让Id等于testdlv的那个<d1v>标签的内容变成空白：
<div id=,!testdivM>
</div>
我想把一段文本插入testdlv元素。用DOM的语言来说，就是想添加一个p元素节点，并 街这个节点作为div元素节点的一个子节点。（dlv元素节点已经有了一个子节点，那是一个Id 属性节点，值是testdlv)。
这项任务需要分两个步骤完成：
创建一个新的元素；
把这个新元素插入节点树。
第一个步骤要用DOM方法createEl ement来完成。下面是使用这个方法的语法：
document • createElement (nodeNatne)
下面这条语句将创建一个p元素：
document •createElement(,lp,f);
这个方法本身并不能影响页面表现，还需要把这个新创建出来的元素插入到文档中去。为此， 你需要有个东西来引用这个新创建出来的节点。不论何时何地，只要你使用了 createElement方 铉，就应该把新创建出来的元素赋给一个变量就总是个好主意：
var para = document.createElement(MpH)j

102 第7章动态创建标记
变量para现在包含着一个指向刚创建出来的那个p元素的引用。现在，虽然这个新创建出 来的P元素已经存在了，但它还不是任何一棵DOM节点树的组成部分，它只是游荡在JavaScript 世界里的一个孤儿。它这种情况称为文档碎片(document fragment)，还无法显示在浏览器的窗口 画面里。不过，它已经像任何其他的节点那样有了自己的DOM属性。
这个无家可归的P元素现在已经有一个nodeType和一个nodeName值，如图7-6所示。这一事 实可以用下面这段代码来验证（把以下代码放入example, js文件并在浏览器里刷新test.html文 档)。
window^onload = furtction() { var para = document•createElement(npn); var info = trnodeName: ,r; inf〇+= para.nodeName; in千〇+= " nodeType: info+= para•nodeType; alert(info);



图 7-6
新节点确实已经存在，它有一个取值为P的nodeName属性。它还有一个取值为1的nodeType 属性，而这意味着它是一个元素节点。不过，这个节点现在还未被连接到test.html文档的节点 树上。
appendChild 方法
把新创建的节点插入某个文档的节点树的最简单的办法是，让它成为这个文档某个现有节点 的一个子节点。
具体到这个例子，是要把一段新文本插入到test.html文档中Id是testchv的元素节点。换 句话说，我想让新创建的P元素成为testdlv元素的一个子节点。你可以用appendChild方法来完 成这一任务。下面是叩pendChild方法的语法：
parents appendChild(c/)i!rf)
具体到test.html文档这个例子，上面这个语法中的child就是刚才用createElement方法创 建出来的，parent就是Id是testdiv的元素节点。我需要用一个DOM方法得到“testdiv”节点 最简单的办法是使用getElementByld方法。
像往常一样，你把这个元素赋给一个变量，这可以让你的代码简明易读：
var testdiv = document•getElementById(lltestdivM);

7.2 DOM 方法 103
变量testdiv现在包含着一个指向那个Id等于testdlv的元素的引用。
在上一小节我创建了一个para变量，它包含一个指向刚创建的那个p元素的引用：
var para = document.createElement("p");
有了这些，就可以像下面这样用appendChlld方法把变量para插入变量testdlv 了：
testdiv*appendChild(para);
新创建的P元素现在成为了 testdlv元素的一个子节点。它不再是JavaScript世界里的一个孤 X，它已经被插入到test.html文档的节点树里了。
在使用appendChlld方法时，不必非得使用一些变量来引用父节点和子节点。事实上，完全 可以把上面这条语句写成下面这样：
document•getElementById(t,te$tdivt,).appendChild( document• createElement(frpM));
可以看到，上面这样的代码很难阅读和理解。像下面这样多写几行，从长远来看是值得的：
var para = document ♦ createElement (ffptf);	1
var testdiv = document^getElementById(,rtestdiv,1); testdiv•appendChild(para);
createTextNode 方法
你现在已经创建出了一个元素节点并把它插入了文档的节点树，这个节点是一个空白的P元 素。你想把一些文本放入这个P元素，但createElement方法帮不上忙，它只能创建元素节点。 信需要创建一个文本节点，你可以用createTextNode方法来实现它。
注意千万不要被这些方法的名字弄糊涂。如果这些方法叫做createElentmentNode和 c「eateTextNode，或者叫做c「eateElentment和createText，都会非常清楚。遗憾的是，它 们的名字叫做 createElement 和 createTextNode。
createTextNode 的语法与 createElement 很相似：
documentscreateTextNode(text)
下面这条语句将创建出一个内容为“Hello world”的文本节点：
document•createTextNode(,lHello world11);
和刚才一样，把这个新创建的节点也赋给一个变量：
var txt = document•createTextNode(l,Hello world11);
变量txt现在包含指向新创建的那个文本节点的引用。这个节点现在也是JavaScript世界里 濟一个孤儿，因为它还未被插入任何一个文档的节点树。
可以用appendChlld方法把这个文本节点插入为某个现有元素的子节点。我将把这个文本节 点插入到我在上一小节创建的P元素。因为在上一小节里我已经把那个P元素存入了变量pa「a， 現在又把新创建的文本节点存入了变量txt，所以现在可以用下面这条语句来达到我的目的：

104 第7章动态创建标记
para.appendChild(txt);
内容为“Hello world”的文本节点就成为那个p元素的一个子节点了。
现在，试着把下面这段代码写入example, js文件：
window.onload = function() { var para = document•createElement(lfp,f); var testdiv = document•getElementById(utestdiv,r); testdiv*appendChild(para);
var txt = document•createTextNode(llHello world11); para.appendChild(txt);
}
然后在浏览器里重新加载test.html文件，你会从浏览器窗口里看到文本“Hdloworld”，如 图7-7所示。
图 7-7
这个例子是按照以下顺序来创建和插入节点的：
创建一个P元素节点。
把这个p元素节点追加到test.html文档中的一个元素节点上。
创建一个文本节点。
把这个文本节点追加到刚才创建的那个P元素节点上。
appendChlld方法还可以用来连接那些尚未成为文档树一部分的节点。也就是说，以下步骤顺 序同样可以达到目的。
创建一个P元素节点。
创建一个文本节点。
把这个文本节点追加到第1步创建的P元素节点上。
⑷把这个P元素节点追加到test.html文档中的一个元素节点上。
下面是按照新步骤编写出来的函数：

X2 DOM 方法 105
window.onload = function() { var para - document•createElement(,rptf); var txt = document•createTextNode卜Heiio world"); para^appendChild(txt);
var testdiv = document•getElementById(lftestdivM); testdiv^appendChild(para);
}
最终的结果是一样的。把上面这些代码写入、example.js文件，并在浏览器里重新加载 sst.html文件。你会看到文本“Hello world”，就像刚才一样。
724	—个更复杂的组合
刚才介绍InnerHTML属性时，我使用了如下所示的HTML内容：
<p>This is <em>iny</em> contentx/p>
与创建一个包含着一些文本的p元素相比，这个步骤要复杂不少。为了把这些标记插入 t.htnil文档，先把它转换为一棵节点树。
元素节点



图 7-8
如图7-8所示，这些HTML内容对应着一个p元素节点，它本身又包含着以下子节点。 □一个文本节点，其内容是“This is”
口 一个元素节点“em”，这个元素节点本身还包含着一个文本节点，其内容是“my” □一个文本节点，其内容是“content”
把需要创建哪些节点的问题弄清楚后，我们能制定出一个妥善的行动计划。
⑴创建一个P元素节点并把它赋给变量para。
创建一个文本节点并把它赋给变量txtl。
把txtl追加到para上。
创建一个em元素节点并把它赋给变量emphasis。
创建一个文本节点并把它赋值给变量txt2。

106 第7章动态创建标记
把 txt2 追加到 emphasis 上。
把 emphasis 追加到 para 上。
创建一个文本节点并把它赋值给变量txt3。
把txt3追加到para上。
把pa「a追加到test.html文档中的testdlv元素上。
下面是根据上述步骤编写出来的JavaScript代码：
window^onload =千unction() { var para = document•createElement(,fpu); var txtl = document•createTextNode(MThis is "); para^appendChild(txti); var emphasis = document•createElement(,'em11); var txt2 = document•createTextNode(llmyfr); emphasis.appendChild(txt2); para^appendChild(emphasis); var txt3 = document•createTextNode(11 content/1); para•appendChild(txt3);
var testdiv = docufnent.getElementByldC’testdiv"); testdiv#appendChild(para);
}
把上面这些代码写入example Js文件，然后在浏览器里重新加载test.html文档。
如果你愿意，可以采用一种不同的方案。可以先把所有的节点都创建出来，然后再把它们连 接在一起。下面是按照这一思路制定出来的行动计划。
创建一个P元素节点并把它赋值给变量para。
创建一个文本节点并把它赋值给变量txtl。
创建一个em元素节点并把它赋值给变量emphasis。
创建一个文本节点并把它赋值给变量txt2。
创建一个文本节点并把它赋值给变量txt3。
把txtl追加到para上。
把 txt2 追加到 emphasis 上。
把 emphasis 追加到 para 上。
把txt3追加到para上。
把para追加到test.html文档中的testdlv元素上。
下面是根据上述步骤编写出来的JavaScript代码：
window•onload =干unction() { var para = document^createElement(f,pn); var txtl = document^createTextNode(flThis is f,); var emphasis = document•createElement(flem11); var txt2 = document•createTextNode(11 my11); var txt3 = document•createTextNode(,t content/1); para.appendChild(txtl); emphasis•appendChild(txt2); para.appendChild(emphasis); para•appendChild(txt3)；
var testdiv = document.getElementById(lltestdivu); testdiv.appendChild(para);

7.3 重回图片库 107
如果把上面这些代码写入example, js文件，然后在浏览器里重新加载test.html文档，将看 到与前面一模一样的结果。
可以看到，把新节点插入某个文档的节点树的办法并非只有一种。即使决定永远也不使用 document.w「1te方法或InnerHTML属性，在使用DOM方法去创建和插人^T节点时你也可以灵活地
做出多种选择。
7.3重

片库
现在，我将向你展示一个动态创建HTML内容的实用案例。在上一章里，我们对图片库脚 本做了许多改进。我们做到了让JavaScript代码与HTML分离，并针对各种情况做到了平稳退化， 我们还做了一些访问性有关的改进。
不过，还有一些内容让我感到不太满意。看一下galler7.html文件中的标记：
<!D0CTYPE html>
<html lang=Henn>
<head>
<meta charset=,futf-8lf />
<title>Image Gallery</title>
<link rel=,fstylesheet11 href=tfstyles/layout.css,f media="screen" />
</head>
<body>
<hl>Snapshots</hl>
• <ul id=11 imagegalleryn>
<li>
<a href=JI images /fireworks • j pgH title=nA fireworks display’’〉
<img src=”images/thumbnail_fireworksdpg._ alt="Fireworks" />
</a>
</li>
<li>	V
<a href=Mimages/coffee.jpg11 title=nA cup of black coffee11 >
<img src=nimages/thumbnail_coffee. jpgtf a ItCo 干 fee" />
</a>
</li>
<li>
<a href="images/rose.jpg" title=MA red, red roseM>
<img src=nimages/thumKnail_rose，jpg” alt=!lRosen />
</a>
</li>
<li>
<a href=Mimages/bigben^jpgM title=,fThe famous clock">
<img src=,lifiiages/thumbnail-bigben.3pgn alt="Big Ben" />
</a>
</li>
</ul>
<img id=”placeholder” src="images/placeholder.gif" alt=f,my image gallery11 /> <p id=!ldescriptionfl>Choose an imagex/p>
〈script src=Hscripts/showPic^jsux/script>
</body>
</html>
这个XHTML文件中有一个图片和一段文字仅仅是为showPIc脚本服务的。若能把结构和行 为彻底分开那最好不过了。既然这些元素的存在只是为了让DOM方法处理它们，那么用DOM 方法来创建它们才是最合适的选择。
第一步非常简单：把这些元素从gallerxhtml文档里删掉。接下来，我们编写一些JavaScript

108 第7章动态创建标记
代码把它们动态地创建出来。
我们先编写一个函数P「epa「ePlaceholder并把它放进showPIc. js文件，然后在文档加载时调 用这个函数。下面是这个函数要完成的任务。
创建一个Img元素节点。
设置这个节点的Id属性。
设置这个节点的src属性。
.(4)设置这个节点的alt属性。
创建一个p元素节点。
设置这个节点的Id属性。
创建一个文本节点。
把这个文本节点追加到p元素上。
把p元素和Img元素插入到gallerxhtml文档。
创建这些元素和设置各有关属性的工作比较明确。我们在这里组合使用了 createElementO、 createTextNode()和 setAttnbuteO方法：
var placeholder = document•createElement(,_img"); placeholder. setAttribute( "id”，’’placeholder"); placeholder, set Attribute^11 srcH,11 images/placeholder*gifM); placeholder, set Attribute^11 alt	my image gallery");
var description = document^createElement(np?f); description.setAttribute(MidM/!descriptionn); var desctext = document♦createTextNode(nChoose an image,r);
接下来，我们用appendChlldO方法把新创建的文本节点插入p元素：
description•appendChild(desctext);
最后一步是把新创建的元素插入文档。很凑巧，因为图片清单（<ul> ... </ul>)刚好是文 档中的最后一个元素，所以如果把placeholder和description元素追加到body元素节点上，它们 就会出现在图片清单的后面。我们可以通过标签名“body”引用body标签（作为第一个也是唯 一一个body元素的引用）。
document •getElementsByTagName(11 body lf)[〇] •appendChild (placeholder); document •getElementsByTagName(11 body11 )[〇]• appendChild (descript ion);
当然，也可以使用HTML-DOM提供的属性body:
document.body•appendChild(placeholder); document•body•appendChild(description);
这两组语句都会把pi aceholde「和description元素插入到位于文档末尾的</body>标签之前。 以上代码工作得很好，但这一切都依赖于一个细节：图片清单刚好是必〇(1^分的最后一个 元素。如果在这个图片清单的后面还有一些其他的内容该怎么办？我们的真实想法是，让新创建 的元素紧跟在图片清单的后面，而不管这个清单出现在文档中的什么地方。
7.3.1在已有元素前插入一个新元素
DOM提供了名为InsertBeforeO方法，这个方法将把一个新元素插入到一个现有元素的前

7.3 重回图片库 109
面。在调用此方法时，你必须告诉它三件事。
新元素：你想插入的元素（neivnerent)。
 目标元素：你想把这个新元素插入到哪个元素酬〇之前。
父元素：目标元素的父元素（parentne/ert)。
下面是这个方法的调用语法：
parentElement • ir\sertBefore(newElentent, targetElement)
我们不必搞清楚父元素到底是哪个，因为targetElement元素的parentNode属性值就是它。 在DOM里，元素节点的父元素必须是另一个元素节点（属性节点和文本节点的子元素不允许是 元素节点)。
比如说，下面这条语句可以把placeholder和description元素插入到图片清单的前面（还记 得吗，图片清单的Id是Imagegallery):
var gallery = documents get ElementById(Miinagegallery11); galleryparentNode,insertBefore(placeholder,gallery);
此时，变量gallery的parentNode属性值是body元素，所以placeholder元素将被插人为body 素的新子元素，它被插入到它的兄弟元素gallery的前面。
还可以把description元素也插入到gallery元素之前，成为它的一个兄弟元素：
gallery^parentNode.insertBefore(description^gallery);
在gallery清单的前面插入placeholder图片和description文本段的效果如图7-9所示0



图 7-9
这种效果其实也不错，但我们刚才说的是把新创建的元素插入到图片清单的后面，而不是前面。
7.3.2在现有方法后插入一个新元素
你可能会想：既然有一个InsertBefore方法，是不是也有一个相应的1nse「tAfter()方法。很

110 第7章动态创建标记
可惜，DOM没有提供这个方法。
1 ■编写 1 nsertAfter 函数
虽然DOM本身没有提供1 nsertAfter方法，但它确实提供了把一个节点插入到另一个节点之 后所需的所有工具。我们完全可以利用已有的DOM方法和属性编写一个InsertAfter函数：
function insertA干ter(newElement,targetElement) { var parent = targetElement^parentNode; if (parent•lastChild == targetElement) { parent•appendChild(newElement);
} else {
parent.insertBefore(newElement>targetElement.nextSibling);
这个函数用到了以下DOM方法和属性：
parentNode
lastChild 属
appendChlld 方法	.
口 Insert Before 方法
nextS 化 ling 属性
下面，请看看这个函数是如何一步一步地完成工作的。
首先，这个函数有两个参数：一个是将被插入的新元素，另一个是目标元素。这两个参 数通过变量newElement和targetElement被传递到这个函数：
function insertAfter(newElement,targetElement)
把目标元素的parentNode属性值保存到变量parent里：
var parent = targetElement•parentNode
%
接下来，检査目标元素是不是parent的最后一个子元素，即比较parent元素的 lastChild属性值与目标元素是否存在“等于”关系：
if (parentslastChild == targetElement)
⑷如果是，就用appendChild方法把新元素追加到parent元素上，这样新元素就恰好被插 入到目标元素之后：
parent•appendChild(newElement)
(5)如果不是，就把新元素插入到目标元素和目标元素的下一个兄弟元素之间。目标元素的 下一个兄弟元素即目标元素的nextSIbllng属性。用InsertBefore方法把新元素插入到目标元素 的下一个兄弟元素之前：
parent•insertBefore(newElement*targetElement•nextSibling)
表面上看，这是一个相当复杂的函数，但只要把它分成几个小部分来理解，就很容易搞清 楚。即使你现在还不能完全明白也不要紧。等你对InsertAfte「函数所用到的DOM方法和属性更 加熟悉时，你自然就能完全理解它。
类似于第6章出现的addLoadEvent函数，InsertAfte「函数也非常实用，应该把它们都收录到

7.3 重回图片库 111
你的脚本里。、
2.使用 i nsertAfter函数
我们将InsertAfter函数用在我的preparePlaceholder函数中。首先，得到图片清单：
var gallery = document^getElementById(Mirnagegallery,r);
接下来，把placeholder (这个变量对应着新创建出来的*img元素）插入到gallery的后面：
insertA千ter(placeholder,gallery);
placeholde「图片现在成了 gallery.html文档的节点树的组成部分，而我们希望把description 文本段插入到它的后面。我们已经把这个节点保存在变量description里了。再次使用InsertAfteK) 方法，但这次是把description插入到placeholder的后面：
insertAfter(description, placeholder);
增加了上面这几条语句之后，preparePlaceholder函数变成了如下的样子：
function preparePlaceholder() { var placeholder = document•createElement(Mimg,!); placeholder •setAttribute(11 id11/1 placeholder11); placeholder •setAttribute(ttsrcl,/liinages/placeholder ♦gif'1); placeholder•setAttribute(,laltll,,tmy image gallery"); var description = document^createElement(Mpf,); description•setAttribute)"id","description"); var desctext = document•createTextNode(,'Choose an image"); description•appendChild(desctext); var gallery = documenttgetElementById(MimagegalleryM); insertAfter(placeholder,gallery); insertAfter(description,placeholder);
事情还未结束，这个函数还有最后一个问题没有解决：我在新增加的那几条语句里使用了一 差新的DOM方法，但没有测试浏览器是否支持它们。为了确保这个函数有足够的退路，还需要 再增加几条语句：
function preparePlaceholder() { if (!document•createElement) return false; if {idocumerrLcreateTextNode) return false; if (!document•getElementByld) return false; if (!document•getElementById(,limagegallery!,)) return false; var placeholder = document•createElement(nimgn); placeholder •setAttribute(,lidl,/lplaceholder,!); placeholder •setAttribute(,lsrcfl,,,images/placeholder •gif'1); placeholder.setAttribute卜alfVmy image gallery11); var description = document•createElement(rrpH); description •setAttribute(Midf,,?, description11); var desctext = document•createTextNode(lfChoose an image"); description•appendChild(desctext); var gailery = document，getElemen士Byld(f,imagegallery"); insertAfter(placeholder,gallery); insertAfter(description,placeholder);
7.3.3图片库二次改进版
现在showPIc.js文件包含5个不同的函数，它们是:

112 第7章动态创建标记
addLoadEvent 函数
InsertAfter 函数
preparePlaceholde「函数
p「epa「eGallery 函数
showPIc 函数
addLoadEvent和InsertAfter属于通用型函数，它们在许多场合都能派上用场。 preparePlaceholder函数负责创建一个Img元素和一个p元素。这个函数将把这些新创建的 元素插入到节点树里图片库清单的后面。P^P^eGalle「y函数负责处理事件。这个函数将遍历处 理图片库清单里的每个链接。当用户点击这些链接中的某一个时，就会调用showPIc函数。 showPIc函数负责把“占位符”图片切换为目标图片。	-
为了启用这些功能，用 addLoadEvent 函数调用 preparePlaceholder 和 prepareGallery 函数。
addLoadEvent(preparePlaceholder); addLoadEvent(prepareGallery);
下面是最终完成的showPIc. js文件：
function addLoadEvent(func) { var oldonload = window^onload;
if (typeof windoiALonload != 'function1) {	一
window^onload = func;
} else {
window.onload = ftmction() { oldonload(); func();
function insertAfter(newElement,targetElement) { var parent = targetElement^parentNode; if (parent•lastChild ~ targetElement) { parentsappendChild(newElement);
} else {
parentsinsertBe千ore(newElement,targetElement』extSibling);
function preparePlaceholder() {
if (!document•createElement) return false;
if (Idocument.createTextNode) return false;
if (!document.getElementByld) return 干alse^
if (!document.getElementByldf'irnagegallery1')) return false;
var placeholder = document•cieateElement(M imgM);
placeholder. setAttribute(MidM>'placeholder11);
placeholder^setAttribute(Msrc,,/,imagies/placeholder*gift,);
placeholder .set Attribute^ "alt 丨 V my image gallery’’)；
var description = documents createElement (11 pfr);
description • set Attribute(nid_’，"descriptiorT);
var desctext = document.createTextNode(,'Choose an image11);
description •appendChild(desctext);
var gallery = dociifnerrLgetElemen士Byld("imagegallery");
insertAfter(placeholder,gallery);

7.3重回图片库 113
insertAfter(description,placeholder);
function prepareGallery() {	^
if (!documentsgetElementsByTagName) return false; if (!document•getElementByld) return false; if (!document• getElementById(,!iniagegallery11)) return false; var gallery = document.getElementById(llimagegallery,1); var links = gallery.getElementsByTagName(”alf5; for ( var i=〇; i < links•length; i++) { links[i]*onclick = function() { return showPic(this);
}
links[i]^onkeypress = links[i]^onclick;
function showPic(whichpic) {
if (!docufnent^getElementById(t,plac€holder11)) return true; var source = whichpic^getAttribute(HhrefH)j var placeholder = document♦getElementById(l<placeholderM); placeholder •setAttribute(’’src”J source); if (!document^getElementById(1fdescription11)) return false; if (whichpic.getAttribute(,,titletr)) { var text = whichpic.getAttribute(lftitlefr);
} else { var text « 11
var description = document.getElementById(1,descriptionn); if (description•firstChild•nodeType == 3) { description♦firstChild•nodeValue = text;
}
return false;
}
addLoadEvent(preparePlaceholder); addLoadEvent(prepareGallery);
JavaScript代码增加了，但标记相应的减少了。galle17.html文件现在只包含一个由JavaScript _本和CSS样式表共用的“挂钩”。这个“挂钩”就是图片清单的id属性。
<!D0CTYPE html>	,
<html>	’
<head>
<meta http-equiv=ncontent-typeM content«f,text/html; charset=utf-8n />
<title>Image Gallery</title>
〈link rel=”stylesheet" href="styles/layou1^css" media="screen" />	、
</head>
<body>
<hl>Snapshots</hl>
<ul id=”ifnagegallery”>
<li>
<a href=lfimages/fireworks^jpgtf title=nA fireworks display,f>
<img src^^images/thumbnail^fireworks.jpg11 alt=!lFireworksn />
</a>
</li>
<li>
<a href=1limages/coffee^jpgff title=l!A cup of black coffee11 >
<img src=?limages/thuinbnail_coffee. jpgir a It=11 Coffeen />
</a>
</li>
<li>







114 第7章动态创建标记
<a href=!,images/rose^ jpgH title="A red, red rose">
<img src=rrimages/thumbnail_rose. jpgH al1>"Rose" />
</a>	~
</li>
<li>
<a href=Mimages/bigben.jpg11 title=!lThe famous clockrf>
<img src=f>images/thumbnail_bigben#jpg,r alt="Big Ben" />
</a>	—
</li>
</ul>
〈script src="scripts/showPic.]V></script>
</body>
</html>
现在，图片库的结构、样式和行为已经彻底分离了。
把gallery.html文件加载到Web浏览器里。如图7-10所示，你将看到placeholder图片和 description文本段，它们已被插入到Imagegallery清单后面。
我们用JavaScript动态地创建了标记并把它们添加到了文档里。JavaScript还对图片清单里的 所有链接进行了预处理。你可以点击任何一个缩略图去体验一下这个图片库，如图7-11所示。



图 7-10	图 7-11
到目前为止，我们创建的这些新内容对这个页面来说并不算是新的。比如，页面加载后，标 记中就已经存在title属性了。而通过createElement添加的新段落也是基于嵌入在脚本中的标记 添加的。实际上，我们创建的所有一切都包含在了初始的页面当中。只不过我们通过脚本对它们 进行了一番重排而已。怎么才能真正得到原来并不存在于初始页面中的内容呢？下面我们就给出 一种解决方案。
7.4 Ajax
2005年，Adaptive Path公司的Jesse James Garrett发明了 Ajax这个词，用于概括异步加载页 面内容的技术。以前，Web应用都要涉及大量的页面刷新：用户点击了某个链接，请求发送回服 务器，然后服务器根据用户的操作再返回新页面。即便用户看到的只是页面中的一小部分有变化，

7.4 Ajax 115
也要刷新和重新加载整个页面，包括公司标志、导航、头部区域、脚部区域等。
使用Ajax就可以做到只更新页面中的一小部分。其他内容一积^志、导航、头部、脚部， 都不用重新加载。用户仍然像往常一样点击链接，但这一次，已经加载的页面中只有一小部分区 域会更新，而不必再次加载整个页面了。
Ajax的主要优势就是对页面的请求以异步方式发送到服务器。而服务器不会用整个页面来响 应请求，它会在后台处理请求，与此同时用户还能继续浏览页面并与页面交互。你的脚本则可以 按需加载和创建页面内容，而不会打断用户的浏览体验。利用Ajax，Web应用可以呈现出功能 丰富、交互敏捷、类似桌面应用般的体验，就像你使用谷歌地图时的感觉一样。
和任何新技术一样，Ajax有它自己的适用范围。它依赖JavaScript，所以可能会有浏览器不 支持它，而捜索引擎的蜘蛛程序也不会抓取到有关内容。
XMLHttpRequest 对象
Ajax技术的核心就是XMLHttpRequest对象。这个对象充当着浏览器中的脚本（客户端）与服 务器之间的中间人的角色。以往的请求都由浏览器发出，而JavaScript通过这个对象可以自己发 送请求，同时也自己处理响应。
虽然有关XMLHttpRequest对象的标准还比较新（参见HTML5)，但这个对象的历史可谓久远， 因而得到了几乎所有现代浏览器的支持。但问题是，不同浏览器实现XMLHttpRequest对象的方式 不太一样。为了保证跨浏览器，你不得不为同样的事情写不同的代码分支。
下面我们举一个例子，把以下代码保存为ajax.html:
<!DOCTYPE html>	、
<html lang="en"> •	”
<head>
<meta charset="utf-8" />
<title>Ajax</title>	.
</head>
<body>
<div id=Hnewnx/div>
〈script src=?fscripts/addLoadEvent^jsnx/script>
〈script src=uscripts/getHTTPObject^jsHx/script>
〈script sro"scripts/getNewContent,js"></script>
</body>
</html>
这个HTML文件包含的addLoadEvent.js文件位于scripts文件夹中，该文件夹里还有另外两 个新脚本：getHTTPObject. js 和I getNewContent. js〇
为了模拟服务器的响应，在ajax.html文件的旁边创建一个example.txt文件，包含如下内容：
This was loaded asynchronously!
这个文件将充当服务器端脚本的输出。多数情况下，服务器端脚本在接到请求后，还会做一
香处理再输出结果。但这里我们只是为了演示说明，就不搞那么复杂了。接下来我们就编写 %etHTTPObject.js 和 getNewContent. js 这两个脚本。
微软最早在IE5中以ActiveX对象的形式实现了一个名叫XMLHTTP的对象。在ffi中创建新的

116 第7章动态创建标记
对象要使用下列代码：
var request = new ActiveX0bject("Msxml2.XMLHTTP.3•〇■’）；
其他浏览器则基于XMLHttpRequest来创建新对象：
var request = new XMLHttpRequest();
更麻烦的是，不同ffi版本中使用的XMLHTTP对象也不完全相同。为了兼容所有浏览器， getHTlTObject. js文件中的getHTTTObject函数要这样来写：
function getHTTPObject() { i干（typeof XMLHttpRequest == "undefined")
XMLHttpRequest = function () { try { return new ActiveX0bject(nMsxml2.XMLHTTP.6*0H); } catch (e) {}
try { return new ActiveX0bject(?tMsxml2^XMLHTTP.3*0rr)； } catch (e) {}
try { return new ActiveX0bject(nMsxml2^XMLHTTPH); }' catch (e) {} return false;
}
return new XMLHttpRequest();
getHTTPObject通过对象检测技术检测了 XMLHttpRequest。如果失败，则继续检测其他方法， 最终返回false或一个新的XMLHttpRequest (或XMLHTTP)对象。
这样，在你的脚本中要使用XMLHttpRequest对象时，可以将这个新对象直接赋值给一个变量
var request = getHTTPObject();
XMLHttpRequest对象有许多的方法。其中最有用的是open方法，它用来指定服务器上将要访 问的文件，指定请求类型：GET、POST或SEND。这个方法的第三个参数用于指定请求是否以异步 方式发送和处理。
在getNewContent. js文件中添加下列代码：
function getNewContent() { var request = getHTTPObject(); if (request) {
request^open( "GET1、 "example.txt", true );
4	request•onreadystatechange = function() {
if (request^readyState == 4) { var para = document-createEiementf’p"); var txt = document.createTextNode(request.responseText); para•appendChild(txt);
document•getElementById(fnew1)•appendChild(para);
}
request.send(null);
} else {
alert(fSorry, your browser doesn\ft support XMLHttpRequest1);
addLoadEvent(getNewContent);
当页面加载完成后，以上代码会发起一个GET请求，请求与ajax.html文件位于同一目录的

7.4 Ajax 117
sxample.txt 文件。
request.open( "GET% "example.txt", true );
代码中的onreaclystatechange是一个事件处理函数，它会在月艮务器给XMLHttpRequest对象送 回响应的时候被触发执行。在这个处理函数中，可以根据服务器的具体响应做相应的处理。 在此，我们给它指定了一个处理函数：	.
request^onreadystatechange = function() {
//处理响应
当然，也可以引用一个函数。下面的代码就会在〇n「eadystatechange被触发时执行名为 doSomethlng 的函数：
^ i-i-nTirfninfrrrti^r'^^^r^rrtrim      ,—     			— —   - — - — >»»»>	■••■ m c i a *〇%
注意在为onreadystatechange指定函数引用时，不要在函数名后面加括号。因为加括号表示立 即调用函数，而我们在此只想把函数自身的引用（而不是函数结果）赋值给 on「eadystate-change 属性。
						—
request.onreadystatechange = doSomething;
在指定了请求的目标，也明确了如何处理响应之后，就可以用send方法来发送请求了：
request.send(null);
如果浏览器不支持XMLHttpRequest对象，getH丌PObject函数会返回false,因此你还要处理 好这种情况。
服务器在向XMLHttpRequest对象发回响应时，该对象有许多属性可用，浏览器会在不同阶段 更新readyState属性的值，它有5个可能的值：	1
口 0表示未初始化
 1表示正在加载	.
 2表示加载完毕	-
3表示正在交互 Q 4表本完成
只要readyState属性的值变成了 4,就可以访问服务器发送回来的数据了。
访问服务器发送回来的数据要通过两个属性完成。一个是「esponseText属性，这个属性用于 保存文本字符串形式的数据。另一个属性是responseXML属性，用于保存Content-Type头部中指 定为"text/xmr的数据，其实是一个DocumentFragment对象。你可使用各种DOM方法来处理这个 对象。而这也正是XMLHttpRequest这个名称里有XML的原因。
在这个例子中，onreadystatechange事件处理函数在等到readyState值变成4之后，就会从 responseText属性里取得文本数据，然后把数据放到一个段落中，再将新段落添加到DOM里：
request•onreadystatechange = function() { if (request•readyState == 4) { var para = document.createElertient(,fpM); var txt = document.createTextNode(request.responseText);

118 第7章动态创建标记
para^appendChild(txt);
document^getElementById(Inew,)^appendChild(para);
此时，exa_e.txt文件中的文本内容就会出现在Id为new的dlv元素中，如图7-12所示。



图 7-12
注意在使用Ajax时，千万要注意同源策略。使用XMLHttpRequest对象发送的请求只能访问与 其所在的HTML处于同一个域中的数据，不能向其他域发送请求。此外，有些浏览器还 会限制Ajax请求使用的协议。比如在Chrome中，如果你使用file://协议从自己的硬盘里 加载 exampl e. txt 文件，就会看到 揅ross origin requests are only supported for HTTP”（跨 域请求只支持HTTP协议）的错误消息。
异步请求有一个容易被忽略的问题是异步性，就是脚本在发送XMLHttpRequest请求之后，仍 然会继续执行，不会等待响应返回。为了证明这一点，可以在request.onreadystatechange处理函 数中和getNewContent函数的最后各添加一个警告框：
function getNewContent() { var request = getHTTPObject(); if (request) {
request•〇pen( "GET% ''example.txt'1, true ); request.onreadystatechange =干unction() { if (request•readyState == 4) { alert("Response Received"); var para = document.createElement(tfpM); var txt = document♦createTextNode(request^responseText); para.appendChild(txt);
document*getElementById(,newf).appendChild(para);

7 A Ajax 119
};
request•send(null);
} else {
alert(fSorry, your browser doesn\ft support XMLHttpRequest1);
}
alert(,!Function Donen);
}
addLoadEvent(getNewContent);
现在加载一下页面试试，很可能显示“Function Done”的警告框会先于“Request Received” 翁警告框出现。这就证明了脚本不会等待send的响应，而是会继续执行。之所以说“很可能”， 是因为有时候服务器的响应也会非常快。如果你是从本地硬盘上加载文件，请求和响应几乎会同 喊发生。而如果是从手机浏览器中加载页面，那么在收到响应之前恐怕就要等很长时间。
为此，如果其他脚本依赖于服务器的响应，那么就得把相应的代码都转移到指定给 cr^eadystatechange属性的那个函数中。上面例子中添加DOM元素的代码就是一个例子。
XMLHttpRequest对象实际上是非常简单的，也没有什么值得大书特书的地方。不过，只要发 赛一点想象力，你就可以通过它达成令人炫目的效果。

議mm
w
总的耒说，Ajax技术还是给我们带了很多好处。利用它，可以增强站点的可用性，用户
无须刷新寅面，从而可以很快地得到响应。但与此同时，这:不新技术也给我们提出了^些挑
，•	复	• • *	〜餅、，	• ‘ . -々顯“	* *	， .
战。 S
Ajax应用的一个特色就是減少重复加载页面的次数。但这种缺少状态记录的技术会与刻
♦	>	*	* y 4
览器的一哉使用惯例产生冲突，导致用户无法使用后退按钮或者无法为特定状态下的页嘉添
釦书签。
♦	* ♦ ， ♦ ♦
♦ ♦	♦ '	♦ ♦ •
只更新部分页面区域的特性也会影响到用户的预期。理想情况，用户的每一次操作都应
该得到—个清晰明确的結果。为此，Web设计人员必须在向脤务器发出请求和服务器攀芦响
应时_用義明确的提示
要构建成功的Ajax应用、关键在无# Aja^c功能看做一#象的扭¥33姑|«增强功能，在^穩
退化的基础上求得渐进增强
7-4_2渐进增强与Ajax
由于Ajax应用能够让用户感觉到响应迅速而透明，很多人者卩认为它更像传统的桌面应用， 面不是网站。虽然这种说法在某种意义上是正确的，但却很容易误导人，很容易让人觉得可以毫 无颐忌地使用Ajax，而不必像在创建网站那样考虑可用性和可访问性。
很多站点使用了 Ajax技术并明确要求必须启用JavaScript才能正常访问网站的内容。有一种 漫点为此辩护，今天站点提供的功能是如此丰富，根本不可能做到平稳退化。
我不赞同这种观点。实际上，我认为能够通过Ajax实现的应用一定也可以通过其他非Ajax 技术来实现。归根结底，要看你怎么用Ajax。

120 第7章动态创建标记
如果你从一开始设计就以Ajax为起点，那么以后确实很难把它从成品站点中剥离出来，再 额外提供一个不使用Ajax的版本。但是，如果一开始你的应用就是基于老式的页面刷新机制构 建的，那么就可以在既有框架基础上，用Ajax拦住发送到服务器的请求，并将请求转交给 XMLHttpRequest对象处理。这种情况下，Ajax功能就扮演了一个位于常规站点之上的层。
这种说法听起来有点耳熟，是吗？这跟我们第5章讨论的渐进增强理念没有什么区别。从一 开始就依赖Ajax构建核心应用，无异于从一开始就使用javascript:伪协议去处理点击链接的操作 (同样也在第5章讨论过)。对于后者，更好的方式当然是只使用常规的链接，然后通过JavaScript 去拦截默认动作。同样的道理，构建Ajax网站的最好方法，也是先构建一个常规的网站，然后 岡狀它0
Hijax
如果说Ajax的成功要归功于它的这个简短好记的名字，让人提到它的时候不用再说 “XMLHttpRequest和DOM脚本编程、CSS，以及(X)HTML”，而只要说“Ajax”就可以了。那么， 你只要说“Hijax” '别人也就明白它指的是“渐进增强地使用Ajax”。
Ajax应用主要依赖后台服务器，实际上是服务器端的脚本语言完成了绝大部分工作。 XMLHttpRequest对象作为浏览器与服务器之间的“中间人”，它只是负责传递请求和响应。如果 把这个中间人请开，浏览器与服务器之间的请求和响应应该继续完成（而不是中断），只不过花 的时间可能会长一点点。
想一想登录表单，构建它最简单的办法就是按照老传统，让表单把整个页面都提交到服务器， 然后服务器再发回来一个包含反馈的新页面。所有处理操作都在服务器上完成，而用户在表单中 输入的数据则由服务器负责取得并与保存在数据库里的数据进行比较，看是不是真的存在这么个 用户。
然后，为了给这个登录表单添加Ajax功能，就需要拦截提交表单的请求（Hijax嘛），让 XMLHttpRequest请求来代为发送。提交表单触发的是submit事件，因此只要通过onsubmlt事件处 理函数捕获该事件，就可以取消它的默认操作（提交整个页面），然后代之以一个新的操作：通 过XMLHttpRequest对象向月艮务器发送数据。
拦截了登录表单的提交请求之后，登录过程就可以让用户感觉更方便。响应时间加快了，不 必刷新整个页面了。可是，万一用户的浏览器没有启动JavaScript呢？没关系，登录表单照样能 让用户正常登录。只不过所花时间要长一点，用户体验没有那么流畅罢了。毕竟，处理登录验证 的操作都在服务器上啊，有什么理由让没有JavaScript的用户不能登录呢！
请大家记住这个事实，Ajax应用主要依赖于服务器端处理，而非客户端处理。既然如此，它 就没有理由不能做到平稳退化。不可否认，有些应用如果没有了 Ajax而只依靠页面刷新，用户的 每一次操作可能都要等很长时间。但慢一点的退化的体验，是不是仍然要比完全没有体验更好呢？
第12章在构建一个完整的网站示例时，将详细介绍如何利用Hijax技术。
①Jeremy Keith借用了 hijack (劫持）一词的发音和含义，意思就是拦截用户操作触发的请求。——译者注

7.5 小结 121
.5小结
在本章里，我们介绍了几种不同的向浏览器里的文档动态添加标记的办法。我们还简要地回
^7两种“传统的”技术：
document, write 方法
1nne「HTML 属性
之后你看到了一些有一定深度的利用DOM方法来动态创建标记的例子。
createElement 方法
createTextNode 方法
appendChlld 方法
InsertBefore 方法
使用这些方法的关键是将Web文档视为节点树。请记住，你用createEl ement或createTextNode
法刚刚创建出来的节点只是JavaScript世界里的孤儿。利用appendChlld或InsertBefore方法，
以把这些DocumentFragnent对象插人某个文档的节点树，让它们呈现在浏览器窗口里。
在这一章里，你还看到了如何对图片库做进一步改进。你还看到了一个非常实用的
力sertAfter函数的构建过程。在需要把一些标记添加到文档时，这个函数往往能帮上大忙。
本章还简要讨论了 Ajax和异步请求，这些内容将在第12章更详细地介绍。
在下一章里，你将会看到更多向文档添加标记的例子，学会动态创建一些很有用的信息块来
増强你的文档。	^

充实文档的内容
*
^0^
W
§
%,
Mr




'	了c;
«« >	,、夕 W^/
,_麵
，一擔 如r …祕,- <吟 一	. :•: ••-，驟你“巧V- :••,.,，，，. ?	*"
心十为文档创建i缩略语g考;的雕'1-尨:，r^i:
^}4)
^ i为戈挡创建=文献来源链接'的函数
^r
印《:二,?:- ??*?
”	v>^5-



sii
、，y' -竽〜:w*t rtT卜以妒心?，,一
工’，乂，球筹蘇:
•您:簿、‘



千为文挡创建“快捷键清单”的函数1__

上一章你已经学会利用DOM方法和属性来动态创建标记。在这一章里你将继续在实践中g 用这些技术。你会通过DOM创建一些标记片段并随后把它们添加到网页。从Mends of ED网^ (

http://friendsofed.com/)本书的下载页面你可以找到这些函数的完整版本。
8.1不应该做什么	|
理论上，你可以用JavaScript把一些重要的内容添力卩到网页上。事实上这是一个坏主意，因. 为这样一来JavaScript就没有任何空间去平稳退化。那些缺乏必要的JavaScript支持的访问者就<
会永远也看不到你的重要内容。至少到现在为止，各大搜索引擎网站的搜索机器人（searchbot” 还几乎不支持JavaScript。
如果你觉察到自己正在使用DOM技术把一些重要的内容添加到网页上，则应该立刻停下来 去检讨你的计划和思路。你很可能会发现自己正在滥用DOM!
第5章我们讨论过，下面这两项原则要牢记在心。
□渐进增强（progressiveenhancement)。渐进增强原则基于这样一种思想：你应该总是从最
f
核心的部分，也就是从内容开始。应该根据内容使用标记实现良好的结构，然后再逐步 加强这些内容。这些增强工作既可以是通过CSS改进呈现效果，也可以是通过D0M添 加各种行为。如果你正在使用D0M添核心内容，那么你添加的时机未免太迟了，内容 应该在刚开始编写文档时就成为文档的组成部分。
□平稳退化。渐进增强的实现必然支持平稳退化。如果你按照渐进增强的原则去充实内容， 你为内容添加的样式和行为就自然支持平稳退化，那些缺乏必要的CSS和D0M支持的 访问者仍可以访问到你的核心内容。如果你用JavaScript去添加这些重要内容，它就没法

8.3 内容 123
支持平稳退化，不支持JavaScript，就看不到内容。这好像是一种限制，其实不是，利用 DOM去生成内容有着广泛的用途。
.2把“不可见”变成“可见”
现如今的Web设计人员能够从许多方面对网页的显示效果加以控制。在对包含在HTML标 記内的内容设置样式时，CSS提供了非常强大的功能。这种技术早已超越了对网页内容的字体和 色进行简单调整的初级阶段。利用CSS，我们可以把原本纵向排列的元素显示成一行。第6章 feva Script图片库页面上由缩略图构成的图片清单就是一个很好的例子。包含在<1彳>标签里的列 表项在通常情况下各占一行，但在我把每个列表项的display属性设置为Inline之后，那些列表 項在浏览器窗口里从纵向排列变成了横向排列。
反过来也是可以的。对于通常是横向排列的元素，只需把它的display属性设置为block，就 可以让这个元素独占一行。如果把某个元素的display属性设置为none，甚至可以让它根本不出 現在浏览器窗口里，这个元素仍是DOM节点树的组成部分，只是浏览器不显示它们而已。
除了标签之间的内容以外，标签内的属性中也包含语义信息。在对内容进行标记时，正确地 设置标记属性也是工作的重要组成部分。
绝大多数属性的内容（即属性值）在Web浏览器里都是不显示的，只有极少数属性例外， 但不同的浏览器在呈现这些例外的属性时却常常千姿百态。比如说，有些浏览器会把title属性 约内容显示为弹出式的提示框，另一些浏览器则会把它们显示在状态栏里。有些浏览器会把alt 翼性的内容显示为弹出式的提示框，这导致了对alt属性的广泛滥用。这个属性原本的用途是： 在图片不可用（无法显示）时用一段描述文字来解释这个位置的图片。
在显示属性这个问题上，你只能听任浏览器摆布。其实只需要一点点DOM编程，我们就能 够把这种控制权重新掌握在自己的手里。
本章我们着眼于使用DOM技术为网页添加一些实用的小部件。
□得到隐藏在属性里的信息。
□创建标记封装这些信息。
□把这些标记插入到文档。	〜
这与简单地利用DOM去新建一些内容有所区别。在本章的例子里，这些内容已经存在于标 记之中，你要利用JavaScript和DOM复制这些内容并以另外一种结构呈现它们。
8.3内容
和往常一样，任何网页都以内容为出发点。现在拿下面这段文字作为你的出发点:
What is the Document Object Model?
The W3C defines the DOM as:
A platform- and language-neutral interface that will allow programs and scripts to dynamically access and update the content, structure and style of documents.
It is an API that can be used to navigate HTML and XML documents•

124 第8章充实文档的内容
给这段文字加上适当的标记：
<hl>What is the Document Object Model?</hl>
<p>
The <abbr title=”World Wide Web .Consortium">W3C</abbr> defines •the <abbr title?Document Object Model">DOM</abbr> as:
</p>
<blockquote cite=Mhttp://www^w3.org/D0M/,!>
<P>
A platform- and language-neutral interface that will allow programs •and scripts to dynamically access and update the 一content, structure and style of documents•
</p>
</blockquote>
<p>
It is an <abbr title=f,Application Programming Interface1,>API</abbr>
•that can be used to navigate <abbr title=MHyperText Markup Language"〉
^HTML</abbr> and <abbr title=MeXtensible Markup Languagen>XML •</abbr> documents •	，
</p>
这段文本包含大量的缩略语，上面已经都用<31)1^>标签把它们都标识出来了。
M      ^梬hhimii t i inwiMKiWiW.ww iHilMU.. i liri^oui imi,									 ♦-?  ?w?iib粀h>immhwi籭
注意<abbr>标签与<acronym>这两个标签之间的区别一直纠缠不清。<abbr>标签的含义是“缩略 语”（源自英文单词abbreviation)，它是对单词或短语的简写形式的统称。<acronym>标签 的含义是被当成一个单词来读的“首字母缩写词”（源自英文单词acronym)。如果你把 DOM念成一个单词dom，它就是一个首字母缩写词；如果你把它念成三个字母D-0-M, 它就是一个缩略语。所有的首字母缩略词都是缩略语，但不是所有的缩略语都是首字母 缩略词。为避免混乱持续下去，在HTML5*<ac「onym>标签已被<3比「>标签代替。
^现在已经把那段文本改写成一个标记片段，你需要把它扩展为一个完整的网页。具体地说， 要先把这段内容放入<body>标签，再把这个body元素以及相应的head元素放入<html>标签。
选用 HTML、XHTML 还是 HTML5
对于标记而言，选用HTML还是XHTML是你的自由。重要的是不管选用的哪种文档类型， 你使用的标记必须与你选用的D0CTYPE声明保持一致。
就个人而言，我更喜欢使用XHTML规则，使用一个D0CTYPE让浏览器采用更严格的呈现方 案。它对允许使用的标记有着更严格的要求，而这可以督促我写出更严谨清晰的文档。比如说， 在写标签和属性时，HTML既允许使用大写字母（比如<P>)，也允许使用小写字母（比如<p>); XHTML却要求所有的标签名和属性名都必须使用小写字母。
HTML在某些情况下会允许省略结束标签，比如说，你可以省略</卩>和</11>标签。表面上看 它提供了一种弹性，但事实上一旦文档在浏览器里的呈现效果与你的预期不符，追査问题根源的 将变得十分困难。在XHTML的世界里，所有的标签都必须闭合——对诸如 <询>和吨「>之类的孤 立元素也不例外：在书写时它们必须有一个反斜线字符表示标签结束：即<彳呵/>和<^/>这样。注









8.3 内容 125
意，为了与早期的浏览器保持兼容，应该在反斜杠字符的前面保留一个空格。使用严格的DOCTYPE 对验证工具跟踪错误会有很大的帮助。
若要使用XHTML D0TYPE,应将下列内容写在文档开头：	(
<!D0CTYPE html
PUBLIC "-//W3C//DTD XHTML 1.0 Strict//ENK "

http://www.w3.org/TR/xhtmll/DTD/xhtmll-strict.dtd">
另一个方案你可能会更喜欢，那就是使用HTML5的文档类型声明，它非常简单：
\	<!DOCTYPE html>
i
总共才15个字符。简短好记，并且容易输入。而且这个文档声明同样也支持HTML和XHTML标记。 要详细了解HTML5,请看第11章。
注意某些浏览器要根据DOCTYPE来决定使用标准模式，还是使用兼容模式来呈现页面。兼容模 式意味着浏览器要模仿某些早期浏览器的“怪异行为”，并容许那些不规范的页面在新浏 览器也能正常工作。一般来说，我们都应该坚持使用标准模式，避免触发兼容模式。谢 天谢地，HTML5 DOCTYPE默认对应的就是标准模式。










mm..
被如真想较矣，可孩走XHTML5"的路线;让Web服务器以appl 1 catl ^
类型页&但必^预先

XHTML5本质土是便用严格的XML规则编写的HTML5。从技术角度说，Web浏览器应# 该将任萍XHTffi5文档都视为也文档f而术是Him文档f而在现实中，你还得在文裆翁
的头部发送正确的MME类型r即邵p扔c被ion/xhtml+观有些浏览器不认识这个MIME类: 型，因:而一g在霞端对浏览M进疗探查后@遂。则最坏的情况,页面4良可:能根本|
不会在浏览器中呈现。因此，绝大’多数XHTML页面仍然是以HTML类型发送的。> ^如果使用了f:mTML5,而龙MIME类型也正确，那么你的页面除J在某些浏览器中可能J
无法呈现之外，有些HTMLDOM属性，■洳doc咖e批.write也将无法使用。:当然，核政DOM
♦ ♦
方法总是能正常使用的^不仅在XHTML5中，在任何有效的XML文档中，核心DOM方法
都緣知无阻。

二
m
下面是按照HTML5规范完成的最终标记文件explanat1on.html:
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset=Mutf-8?t />
<title>Explaining the Document Object Model</title>
</head>
<body>
<hl>What is the Document Object Model?</hl>
<P>
The <abbr title=l!World Wide Web Consortiumfl>WBC</abbr> defines the <abbr title=nDocument



126 第8章充实文档的内容
Object Model11 >D0M</abbr> as:
</p>
〈blockquote cite="

http://www.w3.org/D0M/">
<P>
A platform- and language-neutral interface that will allow programs ^and scripts to dynamically access and update the
•content, structure and style of documents•
</p>
</blockquote>
<p>
It is an <abbr title=l!Application Programming Interface,f>API</abbr> •that can be used to navigate <abbr title=nHyperText Markup Language"〉 ^HTML</abbr> and <abbr title=MeXtensible Markup Languagen>XML ^</abbr> documents •
</p>
</body>
</html>
如果你在Web浏览器里加载这个页面，
图8-1所示。有些浏览器会把文档中的缩略语
另一些浏览器则会把缩略语显示为斜体字。
就可以看到浏览器是如何显示那些标记内容的，如
(<abbr>标签）显示为带有下划线或下划点的文本|
006







M % SiCZ™
” • ♦
• • v v 44 • _m，令•衫
]〇 t
What is the Document Object Model?
W3C defines ihe DOM as:
A platform^ amt languageH>c»ial	ihat wSI allow programs and scrips \x>
dynamkdly access and updbie 細 con始of, structure and 贫yk of document
ft is an API 細 can be used K) naWg迪！ HTML ami XML docuroenls.



8-1
CSS
虽然我还未给explanatlon.—l文档配上任何样式表，但样式显然已经在起作用了。这是因 为每种浏览器都有一些自己的默认样式。
我们可以用自己的样式表来取代浏览器的默认样式。请看下面这个例子：
body {
干orr^family: ’_Helvetica’VArial_、sans-serif; font-size: lOpt;
}
abbr {	、
text-decoration: none;









border: 0; font-style: normal;
8.4显示“缩略语列表
127
把这个样式表保存为typography .css文件，并将其放到子目录styles里去。
在explanation.html文档的<head>部分增加一条语句：	-
<link rel=MstylesheetM media=nscreen" href=Mstyles/typography.csst, />
现在你把explanatlon.httrTl文档加载到一个Web浏览器里，可以看到一些差别。这份文档的 字体变了，其中的缩略语已看不出有特别之处，如图8-2所示。

/A V « « «*> « «	\ 4	/严






鏐©免P
! © t
What is the Document Object Model?
7f» W3C defines thd DOM 9»<



A plaHoim- and langua^neutm!	that wiR |>
access and update ihs content, stiucUim and style of 6oc
It (s an API that can be used to navigate HTMt XML
m>d scH^s io dynamie^ly
图 8-2
JavaScript
缩略语（<abb「>标签）的title属性在浏览器里是隐藏的。有些浏览器会在你把鼠标指针悬 停在缩略语上时，将它的title属性显示为一个弹出式的提示消息。就像浏览器所使用的默认样 式一样，浏览器对缩略语的默认呈现行为也是各有各的做法。
就像我们可以用自己的CSS样式表去取代浏览器所使用的默认样式那样，你也可以用DOM 去改变浏览器的默认行为。
8.4显示“缩略语列表
要能把这些<abb「>fe签中的title属性集中起来显示在一个页面该多好！用一个定义列表元 素来显示这些<abbr>标签包含的文本和title属性最合适不过了。下面是我希望得到的定义列表:
<dl>
<dt>W3C</dt>
<dd>World Wide Web Consortium</dd>
<dt>D0M</dt>
<dd>Document Object Model</dd>
<dt>API</dt>
<dd>Application Programming Interface</dd>

128 第8章充实文档的内容
<dt>HTML</dt>
<dd>HyperText Markup Language</dd>
<dt>XML</dt>
<dd>eXtensible Markup Language</dd>
</dl>
你可以使用DOM来创建这个定义列表，具体步骤如下。
⑴遍历这份文档中的所有abbr元素。
保存每个abbr元素的title属性。
保存每个abbr元素包含的文本。
创建一个“定义列表”元素（即dl元素)。
遍历刚才保存的title属性和abb「元素的文本。
创建一个“定义标题”元素（即dt元素)。
把abbr元素的文本插入到这个dt元素。
创建一个“定义描述”元素（即dd元素)。
把title属性插入到这个dd元素。
把dt元素追加到第4步创建的dl元素上。
把dd元素追加到第4步创建的dl元素上。
把dl元素追加到explanation.html文档的body元素上。
我们编写一个函数来做上面这些事。
编写 displayAbbrevlations 函数
我们把这个函数命名为出splayAbbreviatlonsO。创建一个名为dlsplayAbbrevlatlons. js的文 件并将其存放到子目录scripts。
第一步是定义这个函数。因为它不需要任何参数，所以函数名后面的圆括号将是空的：
function displayAbbreviations() {
开始遍历这份文档里的所有abbr元素之前，我们必须先把它们找出来。这可以用 getElementsByTagName方法轻松完成：只需把abbr作为参数传递给这个方法，它就会返回一个包含 这个文档里的所有abbr元素的节点集合。（前面提到过，节点集合就是一个由节点构成的数组） 我把这个数组保存到变量abbreviations里：
var abbreviations = documentsgetElementsByTagName(MabbrM);
现在，我们可以开始遍历abbreviations数组了，但在遍历之前先进行一些测试。我们知道 在这份文档里有一些缩略语，但并非所有的文档都这样。如果想让这个函数还能适用于其他文档， 就应该先去检查一下当前文档是不是包含有缩略语，再决定要不要走下一步。
査询一下abbreviations数组的length属性，我们就能知道这个文档里有多少个缩略语。如 果abbreviations.length小于1，就说明这个文档里没有缩略语。如果真是这样，这个函数就应该 立刻停止执行并返回一个布尔值false:
if (abbreviations•length < i) return false;

8.4显示“缩略语列表
129
如果文档里没有abbr元素，这个函数将就此结束。
下一步是获取并保存每个abbr元素提供的信息。我们需要得到每个<abbr>标签包含的文本及 其title属性的值。当你需要把像这样的一系列数据保存起来时，数组是理想的存储媒介。定义
•个名为defs的新数组：
var defs = new Array()
现在开始遍历abbreviations数组：
for (var i=〇; iobbreviations.length; i++) {
为了得到当前缩略语的解释文字，用getAttrlbuteO方法得到title属性的值，并把值保存 到变量definition里：	•
var definition = abbreviations[i].getAttribute("title");
要得到<abb「>标签包含的缩略语文本需要nodeValue属性。实际上是需要拿到abbr元素里的 文本节点的值。在explanat1on.html文档中的每个abbr元素里，文本节点都是这个元素内部的第 一个（也是仅有的一个）节点。换句话说，这个文本节点是abb「元素节点的第一个子节点：
abbreviations!^]•firstChild
不过，这个文本也有可能会嵌套在其他的元素里。请看下面这个HTML片段：
<abbr title=HDocument Object Model,f><em>DOM</emx/abbr>
此时，abbr元素节点的第一个子节点将是em元素节点，文本节点是em元素的子节点。因此， 与其使用f 1「stChl 1 d属性，不如使用1 astChl 1 d属性更稳妥：
abbreviations[i].lastChild
下面这条语句得到这个文本节点的nodeValue属性并把它赋值给变量key:
var key = abbreviations[i]•lastChild•nodeValue;
现在有两个变量了： definition和key。
我们通过把其中之一用作数组元素的下标
两个值：
这两个变量的值就是我想保存到defs数组里的内容。 (键)，另一个用作数组元素的值的方式来同时保存这
de干s[key] = definition;
defs数组中的第一个元素的下标是W3C，值是World Wide Web Consortium; defs数组中的第二 个元素的下标是D〇M，值是Document Object Model,依次类推。
下面是这个for循环的完整代码：
for (var i=0; iobbreviations• length; i++) { var definition : abbreviations[i].getAttribute(!,titleH); var key = abbreviations[i].lastChild•nodeValue; defs[key] = definition;
}
为提髙这个循环的可读性，建议你把abbrev1at1ons[1]的值	你在本次循环里正在被遍历
的那个abbreviations数组元素	赋给一个名为current__abbr的变量：

130 第8章充实文档的内容
for (var i-0; i<abbreviations.length; i++) { var current_abbr = abbreviations[i]; var definition = current_abbr^getAttribute(t,titlen); var key = current一abbrJastChiicLnodeValue; defsfkey] = definition;
}
如果你觉得current_abb「变量可以帮助你更好地理解这段代码，那就把它留在那里好了。额 外增加一条这样的语句只是一个非常小的开销。
从理论上讲，你完全可以把整个循环体写成一条语句/但那会让代码非常难以阅读：
干or (var i=〇; iobbreviations • length; i++) { defs[abbreviations[i]•lastChild^nodeValue] = abbreviations[i]•getAttribute(ntitlef,);
> .
在编写JavaScript代码时，许多操作都有多种实现办法。就拿上面这个for循环来说，你已 经看到了三种不同的写法。选出一种最适合你的写法用在你的脚本里。如果在编写某些代码时你 就觉得它们不容易理解，等日后再去阅读它们的时候就会更加困难。
现在，我已经把那些缩略语及其解释保存到了 defs数组里。接下来我们要创建标记以便把 这些内容显示在页面上。
8A2创建标记
定义列表是表现缩略语及其解释的理想结构。定义列表（<dl>)由一系列“定义标题”（<dt>) 和相应的“定义描述”（<dd>)构成：
<dl>
<dt>Title l</dt>	•
<dd>Description K/dd>
<dt>Title 2</dt>
<dd>Description 2</dd>
</dl>
用createElement方法创建这个定义列表，并把这个新创建的元素赋值给变量dllst:
var dlist = document.createElement(,fdl!,);
由上面这条语句创建出来的dl元素只是JavaScript世界里的一个孤儿。稍后我们将通过它的 引用，也就是dlist变量，把它添加到explanati_on.html文档中。
现在需要再编写一个循环，对刚刚创建的defs数组进行遍历。这次我们还是使用for循环， 不过这次与前面编写的那个for循环有点儿不一样。你可以利用一个forVIn循环把某个数组的下 标（键）临时赋值给一个变量：
for (variable in array)
在进入第一次循环时，变量variable将被赋值为数组array的第一个元素的下标值；在进入 第二次循环时，变量variable将被赋值为数组array的第二个元素的下标值；依次类推，直到遍 历完数组array里的所有元素为止。这就是我们遍历关联数组defs的方式：

8.4显示“缩略语列表:
131
for (key in defs) {
上面这行代码的含义是“对于defs关联数组里的每个键，把它的值赋给变量key”。在接下 循环体部分，变量key可以像其他变量那样使用。具体到这个例子，因为变量key的值是当 在处理的数组元素的键，所以可以利用它得到相应的数组元素的值：
var definition = defs[key];
在这个f〇「/"in循环的第一次循环里，变量key的值是W3C，变量definition的值是World Wide Consortium;在第二次循环里，变量key的值是D0M，变量definition的值是Document Object
每次循环都需要创建一个dt元素和一个dd元素。我们还需要创建相应的文本节点并把它们 别添加到新创建的dt和dd元素。
先创建dt元素：
var dtitle = document•createElement(l,dtH);
然后用变量key的值去创建一个文本节点：
var dtitle_text = documentscreateTextNode(key);
我们已经创建了两个节点。新创建的元素节点被赋值给变量dtitle。把新创建的文本节点赋 变量dtitle_text。使用appendChlldO方法把dt1tle_text文本节点添加到dtitle元素节点:
dtitle.appendChild(dtitle_text);
重复这个过程创建dd元素：
var ddesc = document.createElement(MddM);
这次用变量definition的值创建一个文本节点：
var ddesc_text = document.createTextNode(definition);
再一次把文本节点添加到元素节点：
ddesc•appendChild(ddesc_text);
现在，我们有了两个元素节点:dtnie和ddesc。这两个元素节点分别包含文本节点dtniejext 和 ddesc_text。
在结束循环之前，接着把新创建的dt和dd元素追加到稍早创建的dl元素上。一这个dl 元素已经被赋给了变量dllst:
dlist^appendChild(dtitle);
dlist.appendChild(ddesc);
下面是这个forVIn循环的完整代码：
for (key in defs) { var definition = defs[key]; var dtitle = document.createElement(,ldtlt); var dtitle_text = document.createTextNode(key); dtitle•appendChild(dtitle_text);

132 第8章充实文档的内容
var ddesc = document•createElement(ffddM);
var ddesc_text = document•createTextNode(definition);
ddesc•appendChild(ddesc_text);
dlist.appendChild(dtitle);
dlist.appendChild(ddesc);
>
到了这个阶段，我们的定义列表就完成了。它作为一个DocumentFragment对象已经存在于 JavaScript上下文里。接下来的工作是把它插入到文档中去。
插入这个定义列表
与其把这个定义列表突兀地插入文档，不如给它加上一个描述性标题，这样应该会有更好的 效果。
先创建一个h2元素节点：
var header = document•createElement(,lh2M);
再创建一个内容为Abbreviations的文本节点：
var header一text 二 documentcreateTextNode^Abbreviations");
然后把文本节点添加到h2元素节点：	_
header.appendChild(header_text);
对于结构比较复杂的文档，或许还需要借助于特定的Id才能把新创建的元素插入到文档里 的特定位置。因为explanations.html文档的结构并不复杂，所以只要把新创建的元素追加到body! 标签上即可。
引用body标签的具体做法有两种。第一种是使用DOMCorc，目卩引用某给定文档的第一个: (也是仅有的一个）bocty标签：
document ♦getElementsByTagName(,1body11) [〇]
第二种做法是使用HTML-DOM，即引用某给定文档的body属性：
document♦body
首先，插入“缩略语表”的标题：
documents body♦appendChild(header);
然后，插入“缩略语表”本身：
documents body.appendChild(dlist);
dl spl ayAbbrevI atl ons ()函数终于全部完成：
function displayAbbreviations() { var abbreviations = document• getElementsByTagName(,fabbrfl); if (abbreviations.length < l) return false; var defs = new Array();
for (var i=0; iobbreviations • length; i++) { var current一abbr = abbreviations[i]; var definition = current_abbr,getAttribute(Mtitlelf); var key = current_abbr•lastChild•nodeValue; defs[key] = definition;
var dlist = document.createElement(,rdlft); for (key in defs) {

8.4显示“缩略语列表
133
var definition = de干s[key]; var dtitle = document• createElement(Hdt11); var dtitle_text = document.createTextNode(key); dtitle.appendChild(dtitle一text); var ddesc = document•createElement(nddn); var ddesc^text = document•createTextNode(definition); ddesc•appendChild(ddesc_text); dlist^appendChild(dtitle); dlist^appendChild(ddesc);
}
var header = document*createElement(l,h2t,); var header^text = document.createTextNode(MAbbreviationstf); header♦appendChild(header_text); documents body.appendChild^header); documents body•appendChild(dlist);
}
和往常一样，这个函数还有不少需要改进的余地。
检查兼容性
在这个函数的开头部分，应该安排一些检査以确保浏览器能够理解你这个函数里用到的那些 M 方法，这个函数用到了 getElementsByTagName、createElement 和 createTextNode。你可以分
匐检査这几个方法是否存在：
if (!document•getElementsByTagName) return false; if (!document•createElement) return false; if (!document•createTextNode) return false;
当然，也可以把这几项测试合并为一条语句：
if (!document♦getElementsByTagName || !document♦createElement •|| !document•createTextNodey return false;
这两种做法并无区别，你可以根据自己的个人习惯选择一种使用。 dlsplayAbbrevlatlons函数有点长，应该在它的代码里加上一些注释。
function displayAbbreviations() { if (!document•getElementsByTagName || !document•createElement 含|| !documentscreateTextNode) return false;
//取得所有缩略词
var abbreviations = document•getElementsByTagNanfie(llabbr!l); if (abbreviations•length < l) return false; var defs = new Array();
//遍历这些缩略词
for (var i=〇; i<abbreviations*length; i++) { var current_abbr = abbreviations[i]; var definition = current_abbr.getAttribute(fttitle,!); var key ^ current_abbr.lastChild*nodeValue; defs[key] = definition;
}
//创建定义列表
var dlist = documentscreateElement(ndln);
// loop through the definitions for (key in defs) { var definition = defs[key];
//创建定义标题
var dtitle = document♦ createElement(ndt,f)j var dtitlejtext = document.createTextNode(key); dtitle•appendChild(dtitle_text);
// create the definition description

134 第8章充实文档的内容
var ddesc = document.cxeateElement(nddn);
var ddesc_text = document.createTextNode(definition);
ddesc.appendChild(ddesc_text);
//把它们添加到定义列表	_
dlist*appendChild(dtitle);
dlist^appendChild(ddesc);
}
//创建标题
var header document•createElement(1,h2f,);
var header_text = document.createTextNode(HAbbreviationsH)
header•appendChild(header_text);
//把标题添加到页面主体	一
document•body.appendChild(header);
//把定义列表添加到页面主体 documents body•appendChild(dlist);
这个函数应该在页面加载时被调用。你可以通过window.onload事件来做到这一点：
window,onload = displayAbbreviations;
为了日后能够方便地把多个事件添加到wlndow.onload处理函数上，最好使用addLoadEvent1 函数来完成这一工作。首先，编写addLoadEvent函数并把它保存为一个新的JavaScript脚本文件j 将新文件命名为addLoadEvent.js并把它存入scripts文件夹：
function addLoadEvent(func) { var oldonload = window^onload; if (typeo干 window』nload != function1) { window.onload • func;
} else {
window.onload = function() { oldonload(); func();
然后，把下面这条语句添加到displayAbbrevlatlons.js文件里：
addLoadEvent(displayAbbreviations);
现在，JavaScript脚本文件都已经准备好了。接下来，为了调用这两个JavaScript脚本文件， 我们需要在explanation.ht/n]文件的<head>部分添加一些<saipt>标签，如下所示：
<$cript src=Mscripts/addLoadEvent.js,fx/script>
〈script $rc=uscripts/displayAbbreviations^jsnx/script>
注意请确保先包含3〇1乩03(^6111.」5，因为（115口_^八1^1^/131；1〇115.」$依赖于它。在真实项目中， 你通常还需要压缩脚本，并把它们合并成一个文件（如第5章所示）。对我们的例子来说， 保持多个JavaScript文件和较多的冗余空白有助于大家理解和试验。
最终的标记
下面是最终完成的explanat1on.html文件
<!D0CTYPE html>
<html lang=nenlf>
<head>

8,4显示“缩略语列表
135
<meta charset=’rutf-8n />
<title>Explaining the Document Object Model</title>
<link rel=ustylesheetH media="screen”
•href=’丨styles/typography.css” />
</head>
<body>
<hl>What is the Document Object Model?</hi>
<P>
The <abbr title=,,World Wide Web Consortiumn>W3C</abbr> defines •the <abbr title="Documerrt Object Model">D0M</abbr> as:
</p>
<blockquote cite=1fhttp://www^w3^org/D0M/,,>
<P>
A platform- and language-neutral interface that will allow programs •and scripts to dynamically access and update the •content, structure and style of documents♦
</p>
</blockquote>
<p>
It is an <abbr title=HApplication Programming Interfacefl>API</abbr>
•that can be used to navigate <abbr title=’THyperText Markup Language"〉
^HTML</abbr> and <abbr titles11 extensible Markup LanguageH>XML </abbr> documents•
</p>
<script srcs^scripts/addLoadEvent^jsl?x/script>
<script src=f,scripts/displayAbbreviations^jsn></script>
</body>
</html>
现在，把explanatlon.html文件加载到W?b浏览器里就可以看到displayAbbrevlatlons函数 釣效果了，如图8-3所示。	一
if _ in _ W V T rT rVrgTrT ▲rrrrrrrrrV^^^^     _ _   ^
• •
伞，皆,鐵 @ ♦ I0 一、	r}0 t
What is the Document Object Model?
TheV»C<Min9$ the
DOM
A pidttomv dnd iangua^osutral mterface that m sliow pr〇9mms and scripts to i^namicsay
«ee«M and	struetur» and styte ol 6ocmtm<
can used!
API thtt
Abbreviations
i lo nav)g8td HTML and 於dL dociKMntd，
Worttf \fMe Wab Oomcrtkm OoctmomObjKt Model AppUc^ion ProgmKrinQ Inteffaed H^rfwrTcftt Markup language eXteralbta Mukup lansuag^



图 8-3
—个浏览器“地雷”
在此以前，我一直避免提到任何特定的浏览器。只要使用的浏览器支持DOM，则此前见到 过的脚本就都可以正常工作。可是，这个d1splayAbb「ev1at1ons函数却是一个例外。
dlsplayAbbrevlations函数工作得确实不错，除非你使用的浏览器是IE6或更早的Windows

136 第8章充实文档的内容
版本。如果把explanat1on.html文件加载到IE浏览器里，不仅不会看到一个“缩略语列表”， 极有可能会看到一条JavaScript出错消息。
你肯定会对这种行为感到不解：我们已经在dlsplayAbbrevlatlons函数的开头部分加上了 象探测语句，以确保只有支持DOM的浏览器才会去执行DOM代码，IE浏览器 getElementsByTagName和getElementByld方法的支持也毋庸置疑，为什么还会出现这样的问题呢？
事情还要从本书第1章里提到的浏览器大战说起。在那场大战中，网景公司和微软公司曾 <abbr4i]<acronym>标签当做它们的武器之一。在竞争最激烈时，微软决定不在自己的浏览器里实 现abbr元素。
那场浏览器大战早已烟消云散，最终的结果是微软打败了网景，但微软的IE浏览器直到正 才支持abbr元素。chsplayAbbrevlatlons函数在早期版本中失败，是因为它试图从一些abb「元素 节点那里提取属性节点和文本节点，而ffi浏览器却拒绝承认那些abbr节点的“元素”地位。
我们意外地踏上了一颗在一场早已结束的战争中埋藏下来的“地雷”！
可供选择的解决方案有三种。
□把abbr元素统一替换为acronym元素。我对这种解决方案不感兴趣，因为我不想为了迁就 一种顽固不化的浏览器而“牺牲”一大批语义正确的标记。
口在元素中使用html命名空间（<html:abbr>abb「</html:abb「>)，这样IE就可以认出这些元 素。这个方案涉及修改标记，如果要在其他的文档中使用displayAbbrevlatlons函数，问 题仍得不到解决。
□保证dlsplayAbbrevlations函数在IE中能够平稳退化。这个方案实现起来最简单，也最容 易被人接受。只要多写几行代码，IE (或其他不能识别abbr元素的浏览器）就可以提前 退出。
所以，我们选用第三种。
首先，在负责从abb「元素提取title属性值和文本值的fo「循环里添加一条语句：
for (var i=0; i<abbreviations.length; i++) { var current_abbr = abbreviations[i]; if (current_abbr.childNodes•length < l) continue; var definition = current_abbr#getAttribute(!,title,f); var key = current_abbr*lastChild.nodeValue; de千s[key] = definition;
这条新增语句的含义是如果当前元素没有子节点，就立刻开始下一次循环”。因为IE浏 览器在统计abbr元素的子节点个数时总是会返回一个错误的值——零，所以这条新语句会让IE 浏览器不再继续执行这个循环中的后续代码。
当IE浏览器执行到dlsplayAbbrevlatlons函数中负责创建“缩略语列表”的那个for循环时，
因为defs数组是空的，所以它将不会创建出任何dt和dd元素。我们在那个for循环的后面添加
这样一条语句：如果对应于“缩略语列表”的那个dl元素没有任何子节点，则立刻退出 dlsplayAbbrevlatlons 函数：
//创建定义列表
var dlist = document•createElement(?fdln);

8.4显示“缩略语列表
137
//遍历所有定义 for (key in defs) { var definition = defs[key];
//创建定义标题
var dtitle = document.createElement(lldt!!); var dtitle一text = document^createTextNode(key); dtitle.appendChild(dtitle_text);
//创建定义描述	"
var ddesc = document •createElement(ffddf,); var ddesc^text = documenttCreateTextNode(definition); ddesc•appendChild(ddesc_text);
//把它们添加到定义列表	_
dlist^appendChild(dtitle); dlist.appendChild(ddesc);
}
if (dlist^childNodes^length < 1) return false;
请注意，新添加的这条if语句又一次违背了结构化程序设计原则（一个函数应该只有一个 人口和一个出口）——它等于是在函数的中间增加了一个出口点。但这应该是既可以解决IE浏 览器的问题，又不需要对现有的函数代码大动干戈的最简单的办法了。
下面是改进函数之后的代码清单：
function displayAbbreviations() { if (!document•getElementsByTagName || !document.createElement 叫丨！document.createTextNode) return false;
//取得所有缩略词
var abbreviations = document•getElementsByTagName(l,abbr,t); if (abbreviations.length < l) return false; var defs = new Array();
//遍历所有缩略词
for (var i=〇; iobbreviations^length; i++) { var current_abbr = abbreviations[i]; if (current_abbr^childNodes^length < 1) continue; var definition = current_abbr.getAttribute(utitleH); var key = current_abbr•lastChild•nodeValue; defs[key] = definition;
} >
//创建定义列表
var dlist = document•createElement(HdlH);
// loop through the definitions for (key in defs) { var definition = defs[key];
//创建定义标题
var dtitle = doajment.createElement(_’dt__); var dtitle_text = document.createTextNode(key); dtitle.appendChild(dtitle_text);
//创建定义描述	~
var ddesc = documents createElement (Mddfl); var dde$c_text = document•createTextNodefde干inition); ddesc•appendChild(ddesc_text);
//把它们添加到定义列表	^
dlist•appendChild(dtitle); dlist•appendChild(ddesc);
}
if (dlist*childNodes,length < 1) return false;
//创建标题
var header = document• createElement(t,h2n); var headerjtext = document•createTextNode卜Abbreviations"); header•appendChild(headerjtext);
//把标题添加到页面主命	^

138 第8章充实文档的内容
document.body.appendChild(header);
//把定义列表添加到页面主体 document.body.appendChild(dlist);
}
这两条新语句将确保explanatlon.—l文档就算遇到那些不理解abbr元素的浏览器也不会出 问题。它们就像是一条保险绳，其作用与脚本开头部分的对象探测语句很相似。
注意即使某种特定的浏览器会引起问题，也没有必要使用浏览器嗅探代码。对浏览器的名称 和版本号进行嗅探的办法很难做到面面俱到，而且往往会导致非常复杂难解的代码。
我们已经成功地排除了一颗在过去的浏览器大战中遗留下来的“地雷”。如果有什么教训的 话，那就是它可以让我们深刻地体会到标准的重要性。仅仅因为ffi浏览器不支持abb「元素，就 使得一大批用户没有机会看到一个自动生成的“缩略语列表”，这个事实让我感到很遗憾，但这 些用户仍能看到页面上的核心内容。缩略语列表是一种很好的增强补充，它还算不上是页面必不 可少的组成部分。如果它真的必不可少，从一开始就应该把它包括在标记里。
8-5显示“文献来源链接表”
dlsplayAbbrevlatlons函数是一个充实文档内容的好例子（至少对那些不是IE的浏览器来说 是如此)。它从文档结构提取出了一些内容并以一种清晰的方式显示出来。那些原本包含在abbS 标签的title属性里的信息现在直接呈现在了浏览器窗口里。现在，我们来看另一个增强文档 例子。请大家仔细看explanat1on.html文档中的这段标记：
〈blockquote cite="http://www*w3,org/D0M/">
<P>
A platform- and language-neutral interface that will allow programs •and scripts to dynamically access and update the •content, structure and style of documents♦
</py
</blockquote>
blockquote元素包含一个属性cite。这是一个可选属性，你可以给它一个URL地址，告诉火 们blockquote元素的内容引自哪里。从理论上讲，这是一个把文献资料与相关网页链接起来的好 办法；但在实践中，浏览器会完全忽视cite属性的存在。虽然信息就在那里，但用户却无法看 到它们。利用JavaScript语言和DOM，我们完全可以把那些信息收集起来，并以一种更有意义 的方式把它们显示在网页上。
我们计划按照以下步骤将文献以链接形式显示出来。
遍历这个文档里所有blockquote元素。
从blockquote元素提取出cite属性的值。
创建一个标识文本是source的链接。
把这个链接赋值为blockquote元素的cite属性值。
把这个链接插入到文献节选的末尾。

8.5显示“文献来源链接表
139
和显示缩略语列表一样，我们将根据上述步骤编写一个JavaScript函数。
编写 displayCitations 函数
我们将新函数命名为dlsplayCItatlons，将它保存在dlsplayCItations.js文件中。 首先，因为它不需要任何参数，所以函数名后面的圆括号将是空的：
function displayCitations() {
第一步是把文档里的所有blockquote元素找出来。使用getElementsByTagName方法完成这项 查找工作，并把找到的节点集合保存为变量quotes:	^
var quotes = document.getElementsByTagName(,'blockquote11);
接下来遍历这个集合：
for (var i=〇; i<quotes.length; i +十）{
在这个循环里，我们只对有cite属性的文献节选感兴趣。我们用一个简单的测试检査本次 循环中的当前文献节选有没有这个属性。
用getAttribute方法测试节点集合quotes中的当前元素（即quotes[1])，如果 getAttribute("cne")的结果为真，就说明这个节点有cite属性；如果!getAttribute("c1te")的结 果为真，就说明这个节点没有cite属性。如果是后一种情况，使用continue立刻跳到下一次循 环，不再继续执行本次循环中的后续语句：
i干（！quotes[i],getAttribute(Mcite")) { continue;
}
也可以把这条语句写成下面这样：
if (!quotes[i].getAttribute(lfciteH)) continue;
接下来的语句将只有当前blockquote元素有cite属性的情况下才会执行。
首先，得到当前blockquote元素的cite属性值并把它存入变量u「l:
var url quotes[i]^getAttribute(uciten);
下一步是确定应该把“文献来源链接”放到何处。这似乎是一项非常简单的任务。
1.查找你的元素
一个blockquote元素必定包含块级元素，如文本段落，以容纳被引用的大段文本。我们想 把“文献来源链接”放在blockquote元素所包含的最后一个子元素节点之后。显然我们应该先找 到当前blockquote元素的lastChlld属性：








quotes[i]^lastChild
可是，这样我们就会遇到一个问题。请大家再仔细看看这段标记:
〈blockquote cite="

http://www.w3.org/D0M广’>
<P>
A platform- and language-neutral interface that will allow programs ^and scripts to dynamically access and update the

140 第8章充实文档的内容
•content, structure and style of documents♦
</p>
</blockquote>
乍看起来，blockquote兀素的最后一个子节点应该是那个p兀素，而这意味着lastChlld属性 的返回值将是一个p元素节点。可是，事实却并不一定如此。
那个p节点的确是blockquote元素的最后一个元素节点。但在</p>标签和</blockquote>标签 之间还存在着一个换行符。有些浏览器会把这个换行符解释为一个文本节点。这样一来， blockquote元素节点的lastChlld属性就将是一个文本节点而不是那个p元素节点。
注意在编写DOM脚本时，你会想当然地认为某个节点肯定是一个元素节点，这是一种相当常 见的错误。如果没有百分之百的把握，就一定要去检查nodeType属性值。有很多D0M方 法只能用于元素节点，如果用在了文本节点身上，就会出错。
D0M已经提供了一个非常有用的lastChlld属性，如果它能再为我们提供一： lastChlldElement属性就更好了。但令人遗憾的是它没有。还好，你可以利用已有的D0M方法 属性编写一些语句，完成这项任务。
你可以把包含在当前blockquote元素里的所有元素节点找出来。如果把通配符作为参 数传递给getElementsByTagName方法，它就会把所有的元素，不管标签名是什么，——返回给我 们：
var quoteElements = quotes[i]•getElementsByTagName(1f*t,);
变量quoteElements是一个数组，它包含当前blockquote元素（即quotes[i])所包含的全体元素 节点D
现在，blockquote元素所包含的最后一个元素节点将对应着quoteElements数组中的最后一名 元素。数组中的最后一个元素的下标等于数组的长度减去1，因为数组的下标从零开始。记住， 数组中的最后一个元素的下标不等于数组的长度，而是数组的长度减去1:
var elem = quoteElements[quoteElements•length - l];
现在，变量elem对应blockquote元素所包含的最后一个元素节点。
回到我们正在dlsplayCItations函数里编写的那个循环，下面是已经写出来的代码：
for (var i=0; i<quotes^length; i++) { if (!quotes[i].getAttribute{’’cite’’)) continue; var url = quotes[i]<getAttribute(nciten); var quoteChildren = quotes[i]♦getElementsByTagName(r*f); var elem = quoteChildren[quoteChildren♦length - l];
与其假设quoteChildren变量肯定返回一个元素节点数组，不如增加一项测试来检查它的长 度是否小于1。如果是，就用关键字continue立刻退出本次循环：
for (var i=〇; i<quotes.length; i++) { if (!quotes[i].getAttribute(lfcitef,)) continue; var url = quotes[i].getAttribute(f,citeM); var quoteChildren = quotes[i].getElementsByTagName(1*1);

8.5 显示“文献来源链接表
141
if (quoteChildren.length < l) continue;
var elem = quoteChildren[quoteChildren^length - l];
我们已经把创建一个链接所需要的东西全准备好了。变量u「l包含着将成为那个链接的href 晨性值的字符串，elem变量包含着将成为那个链接在文档中的插入位置的节点。
创建链接
用createElement方法创建一个“链接”元素：
var link = document.createElement(Ma,e);
|	接下来，为那个新链接创建一条标识文本。用createTextNode方法创建一个内容为source的
文本节点：
var link_text = document•createTextNode(11 source1');
现在，变量link包含新创建的a元素，变量11 nk_text包含着新创建的文本节点。
用appendChlld方法把新的文本节点插人新链接：
link^appendChild(link_text);
把href属性添加给新链接。用setAttrlbute方法把它设置为变量url的值：
link,setAttribute("href",url);
新链接已经创建好了，可以插入文档中了。
插入链接
你可以就这样把它插入文档，也可以先用另一个元素，比如sup元素，包装它，使它在浏览 器里呈现出上标的效果。
创建一个sup元素节点并把它存入变量superscript:
var superscript = document乂reateElement("sup");
把新链接放入这个sup元素：
«
superscript•appendChild(link);
现在，有了一个存在于JavaScript上下文中的DocumentFragment对象，它此时尚未被插入 任何文档：
<supxa href=Mhttp://www,w3.〇rg/D0M/,f>source</ax/sup>
为了把这个标记插入文档，你要把变量superscript追加为变量elem的最后一个子节点。因 为变量elem对应着blockquote元素目前所包含的最后一个元素节点，这个上标形式的新链接将 出现在文献节选的后面：
elem^appendChild(superscript);
最后，先用一个右花括号结束这个fo「循环，再用一个右花括号结束整个函数。
下面是dlsplayCItations函数到目前为止的代码清单：
function displayCitations() { var quotes = document•getElefnentsByTagName(llblockquotet,); for (var i=〇; i<quotes*length; i十+) {
if (!quotes[i]•getAttribute(l,cite,t)) continue;

142 第8章充实文档的内容
var url = quotes[i]^getAttribute(,lcitet,);
var quoteChildren = quotes[i].getElefnentsByTagName(,*t);
if (quoteChildren•length < l) continue;
var elem = quoteChildren[quoteChildren^length • l];
var link = document.createElement(f,a?,);
var linkjtext = documentscreateTextNode(Msource11);
link* appendChild(linkjtext);
link^ set Attribute(!, hrefurl) j
var superscript = document•createElement("sup");
superscript.appendChild(link);
eleiappendChild (superscript);
改进脚本
依照惯例，总是会有需要改进的地方。在这个函数的开头部分增加一些测试，以确保浏览器 能够理解这个函数里用到的DOM方法。为了让代码更易于理解，你还应该加上一些注释：
function displayCitations() {
if (!document•getElementsByTagName || !document•createElement | !document♦createTextNode) return false;
//取得所有引用
var quotes = document.getElementsByTagName(ffblockquotef<);
//遍历引用
for (var i=〇; i<quotes^length; i++) {
//如果没有cite属性，继续循环
if (!quotes[i]tgetAttribute(,fcite11)) continue;
〃保存cite属性
var url = quotes[i].getAttribute(MciteM);
//取得引用中的所有元素节点
var quoteChildren = quotes[i]•getElementsByTagName(f*f);
//如果没有元素节点，继续_环
if (quoteChildren.length < l) continue;
//取得引用中的最后一个元素节点
var elem = quoteChildren[quoteChildren•length - l];
//创建标记
var link = document•createElement(,tatf); var linkjiext = document•createTextNode("source"); linktappendChild(link_text); link. setAttribute(,f href11, url); var superscript = document.createElement(11 sup11); superscript•appendChild(link);
//把标记添加到引用中的最后一个元素节点
elem.appendChild(superscript);
用 addLoadEvent 函数调用 dlsplayCItatlons 函数：
addLoadEvent(displayCitations);
最终的标记
为了调用dlsplayCItatlons. js文件，还需要在文档末尾添加一组<scr1 pt>标签:
<!D0CTYPE html>
<html lang=nen,!>
<head>
<meta charset=lfutf-8M />
<title>Explaining the Document Object Model</title>









8.6显示“快捷键清单
143
<link rel=MstylesheetM media="screen" styles/typography. css11 />
</head>
<body>
<hl>What is the Document Object Model?</hl>
<P>
The <abbr title=f,World Wide Web Consortiufnn>W3C</abbr> defines •the <abbr title="Document Object Model">DOM</abbr> as:
</p>
<blockquote cite=u

http://www.w3.org/D0M/11>
<p>
A platform- and language-neutral interface that will allow programs •and scripts to dynamically access and update the •content, structure and style of documents•
</p>
</blockquote>
<P>
It is an <abbr title=,rApplication Programming Interface11 >API</abbr>
•that can be used to navigate <abbr title=?fHyperText Markup Language1*〉
^HTML</abbr> and <abbr title=lfextensible Markup Language,f>XML _</abbr> documents •
</p>
<script src=flscripts/addLoadEvent^ jslrx/script>
<script src=!,scripts/displayAbbreviations.jsnx/script>
<script src=Mscripts/displayCitations^jslfx/script>
</body>
</html>	^	,
现在，把explanat1on.html文件加载到一个Web浏览器里就可以看到效果了，如图8-4所示。
♦，篇^ @ 療 「	l__—「n_「	「i nl ■「■	參 ^
What is the Document Object Mode)?
irttait游邮蝴 d»ow pf〇9r»n9 wd 极Dpis to
,«tru〇tur» Vid Of <hcixtwi»<
Document Object
Pr〇0nBnm)n9 Intsrfaoe M»kup Unguag# MMtup Langua^
om
♦,	、• ;
图 8^4
8.6显示“快捷键清单”
此前编写的dlsplayAbbrevlatlons和chsplayCItatlons函数有许多共同之处：从创建一个由 特定元素（abbr元素或blockquote元素）构成的节点集合开始，用一个循环去遍历这个节点集合 并在每次循环里创建出一些标记，最后把新创建的标记插入到文档里。
让我们沿着这一思路再看一个例子。我们编写一个函数、把文档里能用到的所有快捷键显示 在页面里。
accesskey属性可以把一个元素（如链接）与键盘上的某个特定按键关联在一起。这对那些不











144 第8章充实文档的内容
能或不喜欢使用鼠标来浏览网页的人们很有用。对于有视力障碍的人士，键盘快捷方式肯定会; 来许多方便。
一般来说，在适用于Windows系统的浏览器里，快捷键的用法是在键盘上同时按下Alt键; 特定按键；在适用于Mac系统的浏览器里，快捷键的用法是同时按下Ctrl键和特定按键。 下面是accesskey属性是一个例子：
<a hrefindex•htmln accesskey=l,il,>Home</a>
注意设置太多的快捷键往往会适得其反• 突。
它们或许会与浏览器内建的键盘快捷方式发生

支持accesskey属性的浏览器有很多，但是否以及如何把快捷键的分配情况显示在页面上:
需要由身为网页设计人员的你们来决定。有许多网站都会在一个快捷键清单（access^iblli
statement)页面上列明该网站都支持哪些快捷键。
一些基本的快捷键都有约定俗成的设置办法，对此感兴趣的读者可以浏


http://www.clagnut.com/blog/193/。
□ accesskey=T对应着一个“返回到本网站主页”的链接；
'2"对应着一个“后退到前一页面”的链接；
'4"对应着一个“打开本网站的搜索表单/页面”的链接；
'9”对应着一个“本网站联系办法”的链接；
V对应着一个“査看本网站的快捷键清¥”的链接。
accesskey:
accesskey:
accesskey=
accesskey:
下面是一个网站导航清单的例子，使用了快捷键：
<ul id=irnavigation,r>
<lixa href=Mindex^htmlff accesskey=l,i,,>Home</ax/li>
<lixa href=nsearchthtmlfl accesskey=lf4n>Search</ax/li>
<lixa href=T,contact.htmlf, accesskey=n9,f>Contact</ax/li>
</ul>
把这段标记添加到explanat1on.html文档的咕〇(^>开标签的后面。
现在，如果把explanat1on.html文档加载到一个浏览器里，你就会看到这份清单里的链接! 但看不到任何能表明这些链接都有accesskey属性的东西。
利用D0M技术，可以动态地创建一份快捷键清单。下面是具体的步骤。
把文档里的所有链接全部提取到一个节点集合里。
遍历这个节点集合里的所有链接。
如果某个链接带有acesskey属性，就把它的值保存起来。
把这个链接在浏览器窗口里的屏显标识文字也保存起来。
创建一个清单。
为拥有快捷键的各个链接分别创建一个列表项（11元素)。
把列表项添加到“快捷键清单”里。

8.6显示“快捷键清单
145
把“快捷键清单”添加到文档里。
和前面的例子一样，按照以上步骤编写函数。
把这个函数命名为chsplayAccessKeys并存入dlsplayAccessKeys.js文件。
这个函数的工作原理与dlsplayAbbrevlatlons函数很相似：先把accesskey属性值和相关链接 的屏显标识文本提取出来并存入一个关联数组，然后用一个f〇r/1n循环来遍历这个数组以创建各 个列表项。
不再逐行讲解，下面是最终完成的函数代码清单。代码中的注释语句可以把各个步骤解释 揖楚。
function displayAccesskeys() {
if (!document•getElementsByTagName || !document•createElement ||
^!document•createTextNode) return false;
//取得文档中的所有链接
var links = document.getElementsByTagName(,ratf);
//创建一个数组，保存访问键 var akeys = new Array();
//遍历链接
for (var i=0; i<links.length; i++) { var current^link • links[i];
//如果没有accesikey属性，继续循环
if (kurrervt一link•getAt1:ribute(naccesskey,,)) continue;
//取得accesskey葯值
var key = current_link*getAttribute(Maccesskeyfl);
//取得链接文本	一
var text = current_link.lastChilcLnodeValue;
//添加到数组	胃
akeys[key] = text;
}
//创建列表
var list = document • createElement (HulI!);
//遍历访问键 for (key in akeys) {
var text = akeys[key];
//创建放到列表项中的字符串 var str = key + f,: M+text;
//创建列表项
var item = documents createElement (l!liM); var item_text = document.createTextNode(str); iterrhappendChild(item_text);
//把列表项尜加到列表+
list.appendChild(item);
}
//创建标题
var header = docuinen1^createElemen1:("h3");
var headerjtext = document.createTextNode(nAccesskeysn);
header•appendChild(header_text);
//把标题添加到页面主体	"
document•body•appendChild(header);
//把列表添加到页面主体 documents body•appendChild(list);
addLoadEvent(displayAccesskeys);
为了调用displayAccessKeys Js文件，还需要在explanation.html文件的<head>部分添加一组 〈script〉标签：




146 第8章充实文档的内容
<script src=nscripts/displayAccesskeys^jsnx/script>
现在，如果把explanat1on.html文档加载到一个浏览器里，就可以看到动态创建的“快捷键 清单”，如图8-5所示。


♦	•灸	44 a
^0 X：



What is the Docmment Object Model?
TteWSCdeHnes th^OOMas:
A pldsiwir- va UngudQe-neutra interfac« Ihttwid ^〇k pro^mms dntf	to
ac?e»s tfd updxe the	structu货 tfMt style of documents.
R {s an API (hsl can be jsed to navi^ie HTML and XUL documents.
AMrevlaEltMs
ConsorMsn
(Model
min^MtB^ec9
XUl
eXIens由to Markup Un^u句9
AccMtkays
4；Sea^
0:Contact
图 8-5
8.7检索和添加信息
本章编写了几个很有用的脚本，你可以把这几个脚本用到任何一个网页里。虽然它们用途# 一，但基本思路是相同的：用JavaScript函数先把文档结构里的一些现有信息提取出来，再把^丨 些信息以一种清晰和有意义的方式重新插入到文档里去。
这些函数可以让网页变得更有条理、更容易浏览。
□把文档里的缩略语显示为一个“缩略语列表”。
□为文档里引用的每段文献节选生成一个“文献来源链接”。
□把文档所支持的快捷键显示为一份“快捷键清单”。
你可以根据具体情况对这些脚本做进一步改进。比如说，我们这里是把“文献来源链接” 接显示在每个blockquote元素的后面，而你可以把这些链接集中放在文档末尾的一个清单里一 像脚注那样。再比如说，我们这里生成了一份“快捷键清单”，而你可以把各个快捷键分别追： 在相关链接的末尾。
当然可以利用本章介绍的技术去编写一些全新的脚本。例如，可以为文档生成一份目录：
文档里的hi和h2元素提取出来放入一份清单，再将其插入到文档的开头。甚至可以彳巴这份清- 里的列表项增强为一些可以让用户快速到达各有关标题的内部链接。
只需要少量DOM方法和属性，就可以创建这些有用的脚本。如果你想通过DOM脚本去^ 实网页的内容，制作一份结构良好的标记文档将是最重要的前提条件。
在需要对文档里的现有信息进行检索时，以下DOM方法最有用：

