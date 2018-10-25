# WebService

## 什么是WebService 

webservice 可以叫xml webservice ,轻量级的独立的通讯技术

1. 基于Web的服务：服务端提供的服务接口让客户端访问
2. 跨平台，跨语言的整合方案

## 为什么要用webservice

1. 跨语言调用的解决方案

##什么时候要去使用webservice

电商平台，查看订单物流状态等

##webservice中的一些概念

WSDL(web service definition language webservice 定义语言)

webservice 服务需要通过wsdl 文件来说明自己有什么服务可以对外调用，并且有那些方法，参数的定义使用

1. 一个webservice对应一个.wsdl的文件类型
2. 定义了webservice的服务器和客户端应用进行交换的传递数据和相应数据交换的格式和方式

SOAP(简单对象访问协议)

http+xml

webservice 通过HTTP协议发送和接收请求时，发送的内容（请求报文）和接收的内容（相应报文）都是采用xml 格式来进行封装

这些特定地点http消息头和xml内容格式就是SOAP协议

1. 一种简单的，基于HTTP和xml的协议
2. soap消息：请求和响应消息
3. http+xml报文

SEI（webservice endpoint interface webservice的终端接口）

webservice服务端用来处理请求的接口，也就是发布出去的接口

