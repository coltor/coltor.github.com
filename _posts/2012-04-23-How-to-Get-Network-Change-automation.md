---
layout: post
title: 如何自动获取网络变化通知
---

当我们把网线插到计算机上时，WINDOWS任务栏的托盘图标都会更改相应的网络图标，拔掉也会有相应的处理。一直都对这个机制感兴趣，却不知道如何做，而且公司的某个产品也需要这么一个功能。昨天在家测试某个程序的时候，发现了其中一个线程的栈中有一个叫wininet!CheckForNetworkChange的函数，IDA分析了WINNET.DLL后，有了本文。

微软在WINDOWS VISTA之后提供了一个叫NLA(Network List Manager API)的接口，用于获取网络状态变化通知的一个接口。以COM技术实现。
主要导出的COM接口如下：
<pre>
IEnumNetworkConnections
IEnumNetworks
INetwork
INetworkConnection
INetworkConnectionEvents
INetworkEvents
INetworkListManager
INetworkListManagerEvents
</pre>
其中INetworkListManager是一个根对象，可以获取计算机是否连接到因特网(INetworkListManager->get_IsConnectedToInternet)。还可以查询有哪些可用的网络和连接.更关键的是INetworkListManagerEvents和INetworkEvents两个类。这两个类在MSDN文档里的描述如下：
<pre>
is a message sink interface that a client implements to get overall machine state related events.
</pre>

也就是说我们要自己实现这两个类。而回调的方式是通过COM技术中特有的机制IConnectionPoint来搞定。实现方式如下：  
<script src="https://gist.github.com/3747800.js"> </script>
因为NLA API是WINDOWS VISTA之后才有的，对于WINDOWS XP是不兼容的，但是WINDOWS XP下有一个方法可以达到同样的效果，可以参考文章[Network Awareness in Windows XP](http://msdn.microsoft.com/en-us/library/ms700657(v=vs.85).aspx)，这个文章里的代码是C#的，其中关键的代码转换成C++:  
<script src="https://gist.github.com/3747803.js"> </script>