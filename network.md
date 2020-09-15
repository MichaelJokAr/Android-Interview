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


[https://www.jianshu.com/p/25b762d58e66](https://www.jianshu.com/p/25b762d58e66)

## **Q: https 原理**

[链接](https://www.jianshu.com/p/d85e2267ecb8)