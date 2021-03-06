# interview

## 哔哩哔哩

#### 1.网络七层协议

-   应用层
-   会话层
-   传输层
-   网络层
-   数据链路层
-   物理层
-   表示层

##### 应用层（HTTP，FTP，NFS，SMTP）

与其它计算机进行通讯的一个应用，它是对应应用程序的通信服务的。例如，一个没有通信功能的字处理程序就不能执行通信的代码，从事字处理工作的程序员也不关心 OSI 的第 7 层。但是，如果添加了一个传输文件的选项，那么字处理器的程序员就需要实现 OSI 的第 7 层。示例：TELNET，HTTP，FTP，NFS，SMTP 等。

##### 表示层

这一层的主要功能是定义数据格式及加密。例如，FTP 允许你选择以二进制或 ASCII 格式传输。如果选择二进制，那么发送方和接收方不改变文件的内容。如果选择 ASCII 格式，发送方将把文本从发送方的字符集转换成标准的 ASCII 后发送数据。在接收方将标准的 ASCII 转换成接收方计算机的字符集。示例：加密，ASCII 等。

##### 传输层（TCP，UDP，SPX）

这层的功能包括是否选择差错恢复协议还是无差错恢复协议，及在同一主机上对不同应用的数据流的输入进行复用，还包括对收到的顺序不对的数据包的重新排序功能。示例：TCP，UDP，SPX。

##### 会话层（RPC，SQL）

它定义了如何开始、控制和结束一个会话，包括对多个双向消息的控制和管理，以便在只完成连续消息的一部分时可以通知应用，从而使表示层看到的数据是连续的，在某些情况下，如果表示层收到了所有的数据，则用数据代表表示层。示例：RPC，SQL 等

##### 网络层（IP，IPX）

这层对端到端的包传输进行定义，它定义了能够标识所有结点的逻辑地址，还定义了路由实现的方式和学习的方式。为了适应最大传输单元长度小于包长度的传输介质，网络层还定义了如何将一个包分解成更小的包的分段方法。示例：IP，IPX 等。

##### 数据链路层（IP，IPX）

它定义了在单个链路上如何传输数据。这些协议与被讨论的各种介质有关。示例：ATM，FDDI 等。

##### 物理层（Rj45，802.3）

OSI 的物理层规范是有关传输介质的特这些规范通常也参考了其他组织制定的标准。连接头、帧、帧的使用、电流、编码及光调制等都属于各种物理层规范中的内容。物理层常用多个规范完成对所有细节的定义。示例：Rj45，802.3 等。

#### 2.什么是正向代理，什么是反向代理，说说解决了什么样的问题，应用在哪些场景？

![image](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=542139679,2956105114&fm=170&s=08285D32298F714B18D505DB000010B2&w=522&h=660&img.JPEG)

[传送门~咻](https://www.cnblogs.com/Anker/p/6056540.html)

#### 3.我们常见的设计模式，单例模式（什么是单例模式）和订阅者模式（说说订阅者模式如果不需要订阅，需要移除嘛）？

#### 4.网络层协议有哪些，socket 属于什么协议？

#### 5.https 中的 s 是什么，SSL，那么是如何实现的？

#### 6.数据库缓存了解吗？前端缓存策略有哪些，node 缓存技术有哪些？

#### 7.如何让我们的网站更安全，例如如何让别人看不到我们的代码（uglify 不能算做方案）？

#### 8.md5 是加密算法嘛？AES 加密我们前端的 key 该如何存储？

## 拼多多

#### 1.promise 串联执行问题，不使用 async await（希望可以有更好的解法）

##### 方法一

```js
var arr = [];
arr.push(function(a) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            res(1);
        }, 1000);
    });
});

arr.push(function(a) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            console.log(a);
            ++a;
            res(a);
        }, 1000);
    });
});

arr.push(function(a) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            console.log(a);
            ++a;
            res(a);
        }, 1000);
    });
});

//
function test(arr) {
    arr.reduce(function(a, b, c, d) {
        return a.then(b);
    }, arr[0]());
}
test(arr);
```

##### 方法二

```js
// 构建队列
function queue(arr) {
    var sequence = Promise.resolve();
    arr.forEach(function(item) {
        sequence = sequence.then(item);
    });
    return sequence;
}

// 执行队列
queue([a, b, c]).then(data => {
    console.log(data); // abc
});
```
