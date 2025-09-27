# Application Layer

传输层中UDP和TCP的区别

传输层和应用层是可以由OS参与的

## 通信架构
P2P，C/S，区别

## Socket编程，通信
Socket和API的概念和不同

## HTTP与网页

核心在于资源的整合，利用网页间的超链接（URL）将不同的资源整合到同一个网页中。
	HTML document就像一个**网页的说明书**。

内容分发网络CDN:**内容的预生成**
* 资源复用，节省反复对同一资源的重复获取
* 即时显示，（用户需要什么，再获取什么；少量资源的错误不影响网页的整体性）
* 通过链接协同资源。（小的视频网站，直接调用的是大网站的资源）

Content-conter，Name-Domain-Network
最大缺陷：网页式的组合式content带来的数据的爆炸式增长

HTTP1.0:Non-persistent（也有优点，长期占用连接的资源在某些特殊场景下会带来用户的不公平竞争）
HTTP1.1:Persistent



