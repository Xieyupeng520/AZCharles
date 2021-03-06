# AZCharles
#【黑科技】学习抓包之如何用Charles实现“刷楼”

CSDN：http://blog.csdn.net/XieYupeng520/article/details/50251449
简书：http://www.jianshu.com/p/644ccb582ba6

---

本来这篇文章发表在CSDN的，我实在是怕被CSDN的管理员删帖，所以再存一份2333

![这里写图片描述](http://img.blog.csdn.net/20151210152419387)

0.前言
===
为了获取一些网络中的数据，我们需要掌握抓包技术。

Charles是一个 HTTP 代理服务器， HTTP 监视器,反转代理服务器.它允许一个开发者查看所有连接互联网的 HTTP 通信.这些包括`Request`，`Response`现`HTTP Headers` (包含`Cookies`与`Caching`信息).

Charles是一个简单的基于HTTP协议传输的调试工具，在开发和测试工作扮演着重要的角色。   


1.下载配置Charles
===

参见->[《Mac上的抓包工具Charles》](http://blog.csdn.net/jiangwei0910410003/article/details/41620363)

下载地址：

[Mac版 Charles3.11.2 下载](http://download.csdn.net/detail/xieyupeng520/9342943)

[Charles3.11.2 破解包下载](http://download.csdn.net/detail/xieyupeng520/9342987)


2.界面功能
===

![这里写图片描述](http://img.blog.csdn.net/20151210163706481)

`红色`：新建/打开/关闭/保存一个会话。

`黄色`：会话选择区。

`绿色`：清除掉`青色`区域里面的**所有**已经请求到的数据

`紫色`：按钮打开时右下角会提示`Recording`，处于`Recording`状态时会截获网络请求的数据。再按一次关闭。

`海蓝`：编辑，能够编辑当前选中的请求信息，可用于测试不同参数的请求。（图中因为没有选中请求信息，所以为不可点击状态。）。

`紫红`：设置。在这里可以设置允许接收的ip地址的范围（如果全部范围都接收的话，那么就直接设置成0.0.0.0/0），需要和其他设备在同一局域网。

`青色`：抓取到的请求和返回信息，按照域名划分。如果是选择Sequence模式，则是按照顺序全部列出请求。

`灰色`：点击`青色`部分的出现的详情界面。

在请求信息上点击右键也有一些功能：

![这里写图片描述](http://img.blog.csdn.net/20151210165633961)

3.开始刷楼
===
这里以给CSDN刷楼为例，首先为了看得更清楚一点，把当前会话先清除咯。然后到达回复界面，在回复框写好评论，提交之前，把`紫色 - Record`按钮打开。

![这里写图片描述](http://img.blog.csdn.net/20151210170947015)

点击提交，然后看Charles里抓取的数据：

![这里写图片描述](http://img.blog.csdn.net/20151210171203658)

左边有两个红色框，第一个是可以看到名称是`submit...`，说明是提交评论的请求，第二个其实是刷新显示所有评论的请求。

看到`submit...`请求的`Request`，可以看到内容就是我刚刚写的。

然后再看到显示所有评论的请求返回的数据：

![这里写图片描述](http://img.blog.csdn.net/20151210172031208)

可以看到返回的数据已经有刚刚的评论了。

我们直接在`submit...`上点击右键，选择`Repeat`，就是自动发送一个评论请求，内容没有更改，和刚刚一样。

还可以选择`Advanced Repeat`，就能够自动重复刷评论了。

![这里写图片描述](http://img.blog.csdn.net/20151210172654994)

（慎用！小心被封号~~嘤嘤嘤~我只是为了学习下抓包技术，不要封小的哈！~）

4.楼中楼
===

观察看看请求显示的Json返回数据，有一个`CommentId`，有一个`ParentId`，而在`submit...`请求中有一个`replyId`，它们有关系吗？

其实这就是楼中楼的数据格式啦，`replayId`就是请求数据时发送的所评论的评论`id`，在显示的返回数据中，用`ParentId`来记录这个数据。

而`CommentId`就是当前评论的唯一`id`啦。

基于此，我们来修改`submit...`请求造成楼中楼回复的效果。

![这里写图片描述](http://img.blog.csdn.net/20151210173452926)

刚刚的评论 `id` 是 `5711145`，我们修改请求数据内容，加上`replayId = 5711145`

![这里写图片描述](http://img.blog.csdn.net/20151210173702090)

然后点击右下角的`Execute`执行。

![这里写图片描述](http://img.blog.csdn.net/20151210174025145)

看，左边多了一条请求（第三条），而我重新请求刷新评论列表时，刷出了我刚刚更改的内容，并且`ParentId`不为0了！~

去界面看看效果！

![这里写图片描述](http://img.blog.csdn.net/20151210174250958)

哈哈，有了，好好玩。


联想到N年前风靡一时的QQ空间刷评论，应该也是类似的原理。

刚还看到有的人利用这个方法刷QQ农场的经验。各位不要做坏事哈！~

---

Reference:

[《Charles抓包工具详细教程》](http://jingyan.baidu.com/album/5bbb5a1b4cb92513eaa1797a.html?picindex=2)
[《charles使用教程指南》](http://drops.wooyun.org/tips/2423)
[《Charles 从入门到精通(中国5折特惠) - 唐巧的技术博客》](http://blog.devtang.com/blog/2015/11/14/charles-introduction/)
---

刚刚发现CSDN管理员把我之前刷楼的数据都删除了，本来我准备自己删的，看来这个“黑科技”还是不要随便乱用哈！~

