---
title: TCP slow start
slug: Glossary/TCP_slow_start
---

{{GlossarySidebar}}

{{glossary('TCP')}} slow start helps buildup transmission speeds to the network's capabilities. It does this without initially knowing what those capabilities are and without creating congestion. {{glossary('TCP')}} slow start is an algorithm used to detect the available bandwidth for packet transmission, and balances the speed of a network connection. It prevents the appearance of network congestion whose capabilities are initially unknown, and slowly increases the volume of information diffused until the network's maximum capacity is found.

To implement TCP slow start, the congestion window (_cwnd_) sets an upper limit on the amount of data a source can transmit over the network before receiving an acknowledgment (ACK). The slow start threshold (_ssthresh_) determines the (de)activation of slow start. When a new connection is made, cwnd is initialized to one TCP data or acknowledgment packet, and waits for an acknowledgement, or ACK. When that ACK is received, the congestion window is incremented until the _cwnd_ is less than _ssthresh_. Slow start also terminates when congestion is experienced.

## 拥塞控制

拥塞本身是发生在网络层的一种状态，当消息流量太忙，它减慢了网络响应时间。服务器以 TCP 包发送数据，用户的客户端然后通过返回 acknoledgements 或 ack 来确认传输。根据硬件和网络条件，连接的容量是有限的。如果服务器发送太多的数据包太快，它们将被丢弃。意味着，不会有任何确认。服务器将其注册为丢失 ACKs。拥塞控制算法使用发送的数据包和 ack 流来确定发送速率。

## 参阅

- [Populating the page: how browsers work](/zh-CN/docs/Web/Performance/How_browsers_work)
- [http overview](/zh-CN/docs/Web/HTTP/Overview)
