# ssh优化

##ssh自动断开时间

```
ClientAliveInterval 60//服务器端向客户端请求消息的时间间隔(s), 默认是0, 不发送
ClientAliveCountMax 3//服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开
```

