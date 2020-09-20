> **说明**：部分内容来自网络，若有侵权请联系我删除

# 网络 - [Java](./Java.md) - [Android](./README.md)

## **Q：http请求协议**


- 客户端发送一个HTTP请求到服务器的请求消息包括以下格式：

    请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。

    ![](http://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png)

- 一般情况下，服务器接收并处理客户端发过来的请求后会返回一个HTTP的响应消息。

    HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。

    ![](http://upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2)



[http://www.cnblogs.com/ranyonsue/p/5984001.html](http://www.cnblogs.com/ranyonsue/p/5984001.html)

## TCP 三次握手、四次握手
### 三次握手

 ![](https://img-blog.csdnimg.cn/20190424175133474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDcyMzQzNA==,size_16,color_FFFFFF,t_70)
     
- 第一次握手：
     Client端发送位码为SYN=1,随机产生seq number=J的数据包到服务器，Server端收到数据包后，由SYN=1判断出 Client端要求连接；此时Client端处于SYN_SENT的状态。
- 第二次握手：
     Server端收到请求后要向Client端发送确认连接的信息，于是，Server端向Client端发送一个ACK=1,SYN=1,ack number=J+1(即Client端的seq number +1),随机生成的seq number=K，此时服务端处于SYN_RCVD；
- 第三次握手：
    
    Client端收到后检查两点
    - ack number 是否正确（是否等于J+1）
    - 位码ACK是否等于1。

    若以上两点都正确，Client端会再次发送ack num=K+1(第二次机握手中 Server端发送的seq number + 1)，位码ACK=1，
    Server端收到后确认ack number值是否正确，ACK是否为1 ，若均正确则连接建立成功。
    此时双方处于ESTABLISHED的状态。

### 四次握手
 ![](https://img-blog.csdnimg.cn/20190429154058750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDcyMzQzNA==,size_16,color_FFFFFF,t_70)

- 第一次挥手

     Client端发送位码为FIN=1,随机产生seq number=J的数据包到服务器，Server端收到数据包后，由FIN=1判断出 Client端要求断开连接；此时Client端处于FIN-WAIT-1的状态。
- 第二次挥手

    Server端收到请求后要向Client端发送确认断开连接的信息，于是，Server端向Client端发送一个ACK=1,ack number=J+1(即Client端的seq number +1)，此时服务端处于CLOSE_WAIT的状态，Client端收到这个信号后，由FIN-WAIT-1变成FIN-WAIT-2的状态，此时Client端可以接受Server端的数据但是不能向Server端传输数据。
- 第三次挥手

     Server端主动向Client端发送一个位码为FIN=1,随机产生seq number=K 的数据包到服务器，Client端收到数据包后，由FIN=1判断出Server端要断开连接，此时Server端处于LAST-ACK的状态。
- 第四次挥手

     Client端接受到Server端的请求后，要向Server端发送确认端口连接的信息，于是，Client端向Server端发送了一个ACK=1，ack num=K+1(即Server端的seq number +1)，发送后Client端处于TIME-WAIT的状态，等待2MSL后变成CLOSED，而Server端收到Client端的最后一个ACK后便会变成CLOSED

https://blog.csdn.net/weixin_44723434/article/details/89500052




## **Q:HTTP1.0 HTTP 1.1 HTTP 2.0主要区别**


### HTTP1.0 HTTP 1.1主要区别

- 长连接

    HTTP 1.0需要使用keep-alive参数来告知服务器端要建立一个长连接，而HTTP1.1默认支持长连接。

    HTTP是基于TCP/IP协议的，创建一个TCP连接是需要经过三次握手的,有一定的开销，如果每次通讯都要重新建立连接的话，对性能有影响。因此最好能维持一个长连接，可以用个长连接来发多个请求。

- 节约带宽

    HTTP 1.1支持只发送header信息(不带任何body信息)，如果服务器认为客户端有权限请求服务器，则返回100，否则返回401。客户端如果接受到100，才开始把请求body发送到服务器。

    这样当服务器返回401的时候，客户端就可以不用发送请求body了，节约了带宽。

    另外HTTP还支持传送内容的一部分。这样当客户端已经有一部分的资源后，只需要跟服务器请求另外的部分资源即可。这是支持文件断点续传的基础。

- HOST域

    现在可以web server例如tomat，设置虚拟站点是非常常见的，也即是说，web server上的多个虚拟站点可以共享同一个ip和端口。

    HTTP1.0是没有host域的，HTTP1.1才支持这个参数。

### HTTP1.1 HTTP 2.0主要区别

- 多路复用

    HTTP2.0使用了(类似epoll)多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级。

    当然HTTP1.1也可以多建立几个TCP连接，来支持处理更多并发的请求，但是创建TCP连接本身也是有开销的。

    TCP连接有一个预热和保护的过程，先检查数据是否传送成功，一旦成功过，则慢慢加大传输速度。因此对应瞬时并发的连接，服务器的响应就会变慢。所以最好能使用一个建立好的连接，并且这个连接可以支持瞬时并发的请求。

- 数据压缩

    HTTP1.1不支持header数据的压缩，HTTP2.0使用HPACK算法对header的数据进行压缩，这样数据体积小了，在网络上传输就会更快。

- 服务器推送

    意思是说，当我们对支持HTTP2.0的web server请求数据的时候，服务器会顺便把一些客户端需要的资源一起推送到客户端，免得客户端再次创建连接发送请求到服务器端获取。这种方式非常合适加载静态资源。

    服务器端推送的这些资源其实存在客户端的某处地方，客户端直接从本地加载这些资源就可以了，不用走网络，速度自然是快很多的。


> [https://www.jianshu.com/p/25b762d58e66](https://www.jianshu.com/p/25b762d58e66)

## **Q: https 原理**

HTTPS并非是应用层的一种新协议。只是HTTP通信接口部分用SSL 和TLS协议替代而已。通常，HTTP直接和TCP通信。当使用SSL时，则演变成先和SSL通信，再由SSL和TCP通信。

[链接](https://www.jianshu.com/p/d85e2267ecb8)


### **https握手过程**
![](https://img-blog.csdn.net/20180425182355582?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyOTk4MTUz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

步骤 1 ： 客户端通过发送Client Hello报文开始SSL通信（这里是在TCP的三次握手已经完成的基础上进行的）。报文中包含客户端支持的SSL的指定版本、加密组件列表（所使用的加密算法及密钥长度等）。

步骤 2 ： 服务器可进行SSL通信时，会以Server Hello报文作为应答。和客户端一样，在报文中包含SSL版本以及加密组件。服务器的加密组件内容是从接收到的客户端加密组件内筛选出来的。

步骤 3 ： 之后服务器发送Certificate报文。报文中包含公开密钥证书。

步骤 4 ： 最后服务器发送Server Hello Done 报文通知客户端，最初阶段的SSL握手协商部分结束。

步骤 5 ： SSL第一次握手结束之后，客户端以Client Key Exchange报文作为回应。报文中包含通信加密中使用的一种被称为Pre-master secret的随机密码串。该报文已用步骤3中的公开密钥进行加密。

步骤 6 ： 接着客户端继续发送Change Cipher Spec报文。该报文会提示服务器，在此报文之后的通信会采用Pre-master secret密钥加密。

步骤 7 ： 客户端发送Finished报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。

步骤 8 ： 服务器同样发送Change Cipher Spec报文。

步骤 9 ： 服务器同样发送Finshed报文。

步骤 10： 服务器和客户端的Finished报文交换完毕之后，SSL连接就算建立完成。当然，通信会受到SSL的保护。从此处开始进行应用层协议的通信，即发送HTTP请求。

步骤 11 ： 应用层协议通信，即发送HTTP响应。

步骤 12 ： 最后由客户端断开连接。断开连接时，发送close_notify报文。上图做客一些省略，这步之后再发送TCP FIN报文来关闭与TCP的通信。

> [https://blog.csdn.net/qq_32998153/article/details/80022489](https://blog.csdn.net/qq_32998153/article/details/80022489)

## **socket握手过程**
- 服务器建立套接字配置
- 服务器监听套接字
- 客户端发送套接字
