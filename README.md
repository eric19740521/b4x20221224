# b4x20221224
B4X實驗: 使用B4J的HTML網頁產生器工具庫 BANano,產生Web網站
B4X實驗: 使用B4J的HTML網頁產生器工具庫 BANano,產生Web網站
參考資料:
https://www.b4x.com/android/forum/threads/banano-website-app-pwa-library-with-abstract-designer-support.99740/#post-627764
https://www.b4x.com/android/forum/threads/banano-b4x-vuejs-vuetify-web-sockets-rest-pwa.141659/ 


BANano 是一個新的 B4J 庫，用於支持（離線）漸進式 Web 應用程序的網站/網絡應用程序。
與其大哥ABMaterial不同，BANano 不依賴任何特定框架，如 Materialize CSS。
您必須自己編寫該部分，但另一方面，您可以選擇選擇哪一個。

為什麼要有第二個框架？好吧，從一開始 ABMaterial 就是在考慮後台服務器的情況下構建的。
B4J 非常支持設置一個非常強大的 jServer 來處理 web 請求。這確實意味著所有智能都在服務器端，
您擁有 B4J 的全部功能來做任何您想做的事情（安全數據庫訪問、串行通信、緩存控制等）。
使用 B4JS，一些智能可以轉移到瀏覽器端，但該應用程序仍然需要互聯網/內部網訪問，所以這是它所能做到的。


利用 BANano 應用程序需要一些 HTML、CSS 和某種擴展 Javascript 的基本知識。
它充分利用 B4X 的 SmartStrings 來創建應用程序的 HTML 部分。BANano 為您提供了一套工具來圍繞任何框架
（MiniCSS、Skeleton、Spectre、Bootstrap 等）編寫您自己的包裝器，然後可以輕鬆地使用它們來快速構建
 web 應用程序/網站。
 BANano 使用 Umbrella JS（一種更輕量級的 jQuery，大約小 50 倍）和 Mustache 來構建 HTML。

1.下載最新版 HTML網頁產生器庫BANano
https://www.b4x.com/android/forum/threads/banano-website-app-pwa-library-with-abstract-designer-support.99740/#post-627764


BANano7.37
底下兩個目錄的檔案 拷貝到 B4J的額外庫 目錄
C:\Users\User\Downloads\BANano7.37\Libraries
C:\Users\User\Downloads\BANano7.37\Templates

B4J IDE 重啟

2.chrome 設定
https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en


3.ServiceWorker的解釋
https://ithelp.ithome.com.tw/articles/10187529 Day 10 - 30 天 Progressive Web App 學習筆記 - Service Work 簡介
https://ithelp.ithome.com.tw/articles/10187529

Service Worker 解決了 WEB 用戶無法離線瀏覽的問題，
當網站處於離線狀態，沒有辦法利用網路得到更多資料之前，
仍然可以提供基本體驗，讓網站更像 Native App、可以提供像是定期更新、推播通知等功能。



https://www.b4x.com/android/forum/threads/banano-clear-pwa-cache.140054/#post-887018
https://www.b4x.com/android/forum/threads/banano-working-with-jserver-files-got-cached.103642/#post-649688
使用 ServiceWorker 有它自己的一種“緩存”。
它旨在緩存所有內容，因此即使網站處於離線狀態，網站也能正常工作。
它將改進 ServiceWorker 更新系統。
BANano.Initialize("BANano", "ChatNoir", DateTime.Now) '<--- also update the version here, so that the service-worker.js file actually changes.
BANano.JAVASCRIPT_NAME = "app" & DateTime.Now & ".js"
瀏覽器 按 F5 看狀況


5.建立一個新的banano專案
https://www.b4x.com/android/forum/threads/banano-for-dummies-by-example.108722/#content  參考@Mashiane...
Private fx As JFX

	'do not use service workers for PWA
	BANano.TranspilerOptions.UseServiceWorker = False


	' from an URL
	BANano.Header.AddCSSFile("https://www.w3schools.com/w3css/4/w3.css")


	'open the compiled index file
	Dim URL As String = File.GetUri(File.Combine(File.dirapp,"MyApp"),"index.html")
	fx.ShowExternalDocument(URL)
	ExitApplication


	'****banano****
	'Dim body As BANanoElement = BANano.GetElement("#body")
	'body.Append("Hello Alain Bailleul!")


Visual Studio Code對文件進行了美化
https://visualstudio.microsoft.com/zh-hant/



6.常用語法
https://www.w3schools.com/html/html_css.asp

a.body預設的
    '****javascript****
    'var body = document.getElementById("body");
    'body.innerHTML = 'Hello Alain Bailleul!';
    'body.style.backgroundColor = "lightblue";

    '****jquery****
    'var body = $("#body");
    'body.append("Hello Alain Bailleul!");
    'body.css("background-color","yellow");

    '****banano****
    Dim body As BANanoElement = BANano.GetElement("#body")

    body.Empty
    body.Append("Hello Alain Bailleul!")
    body.SetStyle(BANano.ToJson(CreateMap("background-color": "lightblue")))

 

B.語法2
    '****javascript****
    'var body = document.getElementById("body");
    'body.innerHTML = 'Hello Alain Bailleul!';
    'body.style.backgroundColor = "lightblue";

    '//****jquery****
    'var body = $("#body");
    'body.append("Hello Alain Bailleul!");
    'body.css("background-color","yellow");

  
    '***banano 1.get the document
    Dim doc As BANanoObject = BANano.Window.GetField("document")
    '2. run the method
    Dim body As BANanoObject = doc.RunMethod("getElementById", Array("body"))

    body.RunMethod("append", Array("Hello Alain Bailleul!"))
    'body.RunMethod("backgroundColor", Array("lightblue"))










b.語法1

	<div id="div1" class="row" style="border-style:dotted" height="100" width="100">

	   <p id="p1" style="color:red">This content is added via code!</p>
	   <p id="p2" style="border-style:dotted">{B}This is a paragraph{/B}</p>

	</div>	


	'****BANano****
	Dim body As BANanoElement = BANano.GetElement("#body")
	body.Empty
	
	'create an empty div and get it
	Dim div1 As BANanoElement = body.Append($"<div id="div1"></div>"$).Get("#div1")
	'add a class
	div1.AddClass("row")
	'set an attribute
	div1.SetAttr("height", "100")
	div1.SetAttr("width", "100")
	'set border
	div1.SetStyle(BANano.ToJson(CreateMap("border-style": "dotted")))
 

	Dim p1 As BANanoElement = div1.Append($"<p id="p1"></p>"$).Get("#p1")
	p1.SetStyle(BANano.ToJson(CreateMap("color":"red")))
	p1.SetText("This content is added via code!")

	Dim p2 As BANanoElement = div1.Append($"<p id="p2"></p>"$).Get("#p2")
	p2.SetHTML(BANano.SF($"{B}This is a paragraph{/B}"$))


html字體變粗, <b>想輸入的字</b>, 可讓字體變成粗體

C.語法2
    '****javascript****
    'var body = document.getElementById("body");
    'body.innerHTML = 'Hello Alain Bailleul!';
    'body.style.backgroundColor = "lightblue";

    '//****jquery****
    'var body = $("#body");
    'body.append("Hello Alain Bailleul!");
    'body.css("background-color","yellow");

  
    '***banano 1.get the document
    Dim doc As BANanoObject = BANano.Window.GetField("document")
    '2. run the method
    Dim body As BANanoObject = doc.RunMethod("getElementById", Array("body"))

    body.RunMethod("innerHTML", Array("Hello Alain Bailleul!"))
    body.RunMethod("backgroundColor", Array("lightblue"))



'get the drawing context
    '****javascript
    'var ctx = document.getElementById('mycanvas').getContext('2d');
    '***banano 1.get the document
    Dim doc As BANanoObject = BANano.Window.GetField("document")
    '2. run the method
    Dim mycanvas As BANanoObject = doc.RunMethod("getElementById", Array("mycanvas"))
    'get the document context
    Dim ctx As BANanoObject = mycanvas.RunMethod("getContext", Array("2d"))


slide.AddEventListener("change", BANano.CallBack(Me, "changescale", Null), True)





6.動手寫..w3css..
範例:t21
https://www.w3schools.com/w3css/tryit.asp?filename=tryw3css_examples_mobile_blue

範例t22
https://www.w3schools.com/w3css/tryit.asp?filename=tryw3css_examples_login

範例t23
https://www.w3schools.com/w3css/tryit.asp?filename=tryw3css_modal

7.動手寫..Bootstrap..範例t24
https://www.tutlane.com/example/bootstrap/bootstrap-show-hide-toast-example

JQuery
https://www.w3school.com.cn/jquery/index.asp
https://www.runoob.com/jquery/jquery-tutorial.html










