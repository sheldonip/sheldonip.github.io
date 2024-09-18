---
title: "Netty 4 Optimisation"
date: 2024-04-07T18:31:00Z
draft: false
image: cover.jpg
---

## Introduction

When we chose netty for processing messages with low-latency, we often need to further improve the throughput after launch. There are plenty of ways to do but of course there must be some tradeoff. We will talk about some of them in details.

## Implementation
Though `writeAndFlush()` is easy to use, it combines `write()` and `flush()`, but `flush()` is an expensive system call. To futher improve the throughput, we may try to use `write()` and `flush()` separately, and limit the number of `flush()`. We can check no more data to read before calling `flush()`:

``` java
@Override
public void channelReadComplete(ChannelHandlerContext ctx) {
    ctx.flush(); 
}
```

This increases the complexity of the code but it would save us from consuming more resources.


To improve the efficiency, we may want to also check the channel is writable before writing. A slow receiver may not catch up the writing speed and cause an out of memory error. Futhermore, we should set reasonable values for the high and low water mark to control the return value of `Channel.writable()`.

Here is a sample code to check the channel is writable, `needsToWrite` represents the business logic:
``` java
private void writeHelper(Channel channel) {
    while(needsToWrite && channel.isWritable()) { 
        channel.writeAndFlush(createMessage());
    }
}
```
Again, though it increases the complexity, it further prevents error from occurring.


If we are certain about the operating system being used, we can switch to linux or macOS native transport for less GC and low latency to improve performance.

For linux, just search and replace:

- `NioEventLoopGroup` → `EpollEventLoopGroup`
- `NioEventLoop` → `EpollEventLoop`
- `NioServerSocketChannel` → `EpollServerSocketChannel`
- `NioSocketChannel` → `EpollSocketChannel`

Then add `netty-transport-native-epoll` dependency in your `pom.xml` or equivalent.

Similarly, for macOS, just search and replace:

- `NioEventLoopGroup` → `KQueueEventLoopGroup`
- `NioEventLoop` → `KQueueEventLoop`
- `NioServerSocketChannel` → `KQueueServerSocketChannel`
- `NioSocketChannel` → `KQueueSocketChannel`

Then add `netty-transport-native-kqueue` dependency in your `pom.xml` or equivalent.

Obviously after switching the transport, you cannot build when using other operation systems.

## Wrapping up
We have addressed how to optimise netty by limiting `flush()` calls, checking `writable()` and adpating the native transport. As always, there are some trade-offs and we should know before adapting them. For those who want to understand more, Norman has given great presentations on using netty and his slides are available [here](http://normanmaurer.me/presentations/2014-facebook-eng-netty/slides.html).

## References

- https://netty.io/wiki/user-guide-for-4.x.html
- https://netty.io/wiki/native-transports.html
- https://netty.io/4.1/api/io/netty/channel/Channel.html#isWritable--
- https://netty.io/4.1/api/io/netty/channel/WriteBufferWaterMark.html
- http://normanmaurer.me/presentations/2014-facebook-eng-netty/slides.html

## Credits

Cover photo by [Winston Chen](https://unsplash.com/@winstonchen?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-group-of-people-riding-on-top-of-a-boat-in-the-water-Vdi_bMAiC0Y?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 
  