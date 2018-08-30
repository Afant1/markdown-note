# SOAP注入和XPATH注入小结
>以下俩种注入理解其服务架构。soap就是一种接口，后端存储一般是数据库，所有sql注入该怎么注入就怎么注入，只是改一下数据包的格式就行，而XPATH有自己的语法，数据存储在xml文件中，注入需要xpath语法获得数据。
>
## 1.SOAP基本概念
>什么是SOAP？这就要从WebService说起了
WebService是一种跨平台，跨语言的规范，用于不同平台，不同语言开发的应用之间的交互。比如在Windows Server服务器上有个C#.Net开发的应用A，在Linux上有个Java语言开发的应用B，B应用要调用A应用，或者是互相调用。用于查看对方的业务数据。这个时候，如何解决呢？
 WebService就是出于以上类似需求而定义出来的规范：开发人员一般就是在具体平台开发webservice接口，以及调用webservice接口。每种开发语言都有自己的webservice实现框架。
而SOAP作为webService三要素（SOAP、WSDL(WebServicesDescriptionLanguage)、UDDI(UniversalDescriptionDiscovery andIntegration)）之一：WSDL 用来描述如何访问具体的接口， UDDI用来管理，分发，查询webService ，SOAP 可以和现存的许多因特网协议和格式结合使用，包括超文本传输协议（HTTP），简单邮件传输协议（SMTP），多用途网际邮件扩充协议（MIME）。
简单而言，SOAP（简单对象访问协议）是连接或Web服务或客户端和Web服务之间的接口。
其采用HTTP作为底层通讯协议，XML作为数据传送的格式
SOAP消息基本上是从发送端到接收端的单向传输，但它们常常结合起来执行类似于请求 / 应答的模式。
### 2.SOAP安全
>http://localhost/test/bWAPP/ws_soap.php?wsdl
>访问到如上wsdl结尾的连接。数据以xml格式呈现的。让AWVS扫描构造好数据包后，在手工检测。具体漏洞在具体分析

```
POST /test/bWAPP/ws_soap.php HTTP/1.1
Content-Type: text/xml
SOAPAction: "urn:tickets_stock#get_tickets_stock"
Content-Length: 545
X-Requested-With: XMLHttpRequest
Referer: http://localhost/test/bWAPP/ws_soap.php?wsdl
Host: localhost
Connection: Keep-alive
Accept-Encoding: gzip,deflate
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.21 (KHTML, like Gecko) Chrome/41.0.2228.0 Safari/537.21
Accept: */*

<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"  xmlns:xsd="http://www.w3.org/1999/XMLSchema"  xmlns:xsi="http://www.w3.org/1999/XMLSchema-instance"  xmlns:m0="http://tempuri.org/"  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:urn="urn:movie_service">
     <SOAP-ENV:Header/>
     <SOAP-ENV:Body>
        <get_tickets_stock>
         <title>-1' or 1=2-- -</title>
        </get_tickets_stock>
     </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```
## 2. XAPTH漏洞关键语句

    $login = $_REQUEST["login"];
    $password = $_REQUEST["password"];
    $xml = simplexml_load_file("passwords/heroes.xml");
    // var_dump($xml);
    // XPath search
    $result = $xml->xpath("/heroes/hero[login='" . $login . "' and password='" . $password . "']");
**heroes.xml中的数据**
```
<?xml version="1.0" encoding="UTF-8"?>
<heroes>
	<hero>
		<id>1</id>
		<login>neo</login>
		<password>trinity</password>
		<secret>Oh why didn't I took that BLACK pill?</secret>
		<movie>The Matrix</movie>
		<genre>action sci-fi</genre>
	</hero>
<heroes>
```
具体注入语句参考下面链接
### 参考连接
[soap注入利用](https://blog.csdn.net/niexinming/article/details/49491643)
[soap漏洞研究](http://skysec.top/2018/08/17/SOAP%E5%8F%8A%E7%9B%B8%E5%85%B3%E6%BC%8F%E6%B4%9E%E7%A0%94%E7%A9%B6/)
[xpath 注入](https://xz.aliyun.com/t/2500#toc-2)
[xpath 注入详解](https://www.cnblogs.com/bmjoker/p/8861927.html)