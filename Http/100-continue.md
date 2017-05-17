在使用curl做POST的时候, 当要POST的数据大于1024字节的时候, curl并不会直接就发起POST请求, 而是会分为俩步

```
1. 发送一个请求, 包含一个Expect:100-continue, 询问Server使用愿意接受数据
2. 接收到Server返回的100-continue应答以后, 才把数据POST给Server
 
```


于是,这样就有了一个问题, 并不是所有的Server都会正确应答100-continue, 比如lighttpd, 就会返回417 “Expectation Failed”, 则会造成逻辑出错,,

要解决的办法也挺容易:

```
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Expect:'));
// Disable Expect: header (lighttpd does not support it)
```


1、http 100-continue用于客户端在发送POST数据给服务器前，征询服务器情况，看服务器是否处理POST的数据，如果不处理，客户端则不上传POST数据，如果处理，则POST上传数据。在现实应用中，通过在POST大数据时，才会使用100-continue协议。

　　2、客户端策略。

　　　　1）如果客户端有POST数据要上传，可以考虑使用100-continue协议。加入头{"Expect":"100-continue"}

　　　　2）如果没有POST数据，不能使用100-continue协议，因为这会让服务端造成误解。

　　　　3）并不是所有的Server都会正确实现100-continue协议，如果Client发送Expect:100-continue消息后，在timeout时间内无响应，Client需要立马上传POST数据。

　　　　4）有些Server会错误实现100-continue协议，在不需要此协议时返回100，此时客户端应该忽略。

　　3、服务端策略。

　　　　1）正确情况下，收到请求后，返回100或错误码。

　　　　2）如果在发送100-continue前收到了POST数据（客户端提前发送POST数据），则不发送100响应码(略去)。