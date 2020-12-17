# select模块

## 标准库文档
[文档连接](https://docs.python.org/3.6/library/select.html)

## 语法

>  高效地等待 I/O
,select 模块提供了一组访问不同平台下 I/O 监控函数的功能。
```python

import select

readable, writeable, exceptional = select.select(inputs,
                                                outputs,
                                                exc,
                                                timeout)

# 参数说明：
    ①：前三个参数都是列表，第四个参数是超时时间，单位为秒
    ②：inputs:列表中的对象用于检测即将读取的数据
    ③：outputs:列表包含的对象会在缓存区有空间的时候接收传出的数据
    ④：接收它们执行期间发生的错误（通常是输入和输出对象的结合）

# 返回值说明：
    ①：返回三个新的列表，列表的内容是传入的列表的内容，
    ②：readable列表里的套接字都有数据缓存并且都可读
    ③：writeable列表的套接字的缓冲区都有空闲空间并都可写。
    ④：exceptional中的套接字都发生了错误(j具体异常的定义去取决于平台)
    ⑤：当到了超时时间以后，select会返回三个空列表。
```

## 关于可读套接字的说明（三种情况）
- 如果这个套接字是主服务器的套接字，一方面继续来监听连接，另外一个方面，可读这个条件要让他准备好接收其他连接，注意：除了要添加新的连接到inputs中监控，还要设置个客户端套接字为非阻塞状态（conn.setblocking(0) ）
- 另一种情况是与已经发送过数据的客户端建立连接。用recv()读取，然后将它放到队列中，这样就可以通过该套接字发送出去然后返回到客户端。
- 如果是一个可读的套接字但没有数据，客户端也断开了连接，就要准备断开流。

## 关于可写套接字的说明
- 如果此连接的队列中存在数据，那我们的下一个消息就是要把它发送出去。否则要把该连接从输出列表中移除，这样下一次循环 select() 就不会标记这个套接字为要发送数据了。


## 项目应用demo
```python

def __write_data(self, client_socket, client_addr):
    while True:
        try:
            if SocketServerHelper.__exist_in_avail_client_address_list(client_addr):
                infds_c, outfds_c, eerrfds_c = select.select([], [client_socket, ], [], 1)
                if len(outfds_c) != 0 and self.get_write_data_list_by_addr is not None:
                    data_models = self.get_write_data_list_by_addr(client_addr)
                    if data_models is not None and len(data_models) > 0:
                        for data_model in data_models:
                            # data = data_model.frame.pack_data()
                            data = data_model.frame
                            print(data)
                            if data_model is not None and data is not None and len(data) > 0:
                                try:
                                    sent = client_socket.send(data)
                                    print(sent)
                                    if sent > 0:
                                        self.event_dispatcher.dispatch_event(SocketDataEvent(
                                            SocketDataEvent.SentSuccessful, {'srcAddress': client_addr, 'data': data_model}))
                                        continue
                                    self.event_dispatcher.dispatch_event(SocketDataEvent(
                                            SocketDataEvent.SentFailed,{'srcAddress': client_addr, 'data': data_model}))
                                except Exception as e :
                                    print(e)
                                    self.event_dispatcher.dispatch_event(SocketDataEvent(
                                            SocketDataEvent.SentFailed,{'srcAddress': client_addr, 'data': data_model}))
                                    raise Exception('Socket Data Sent Error')
            else:
                break
            try:
                time.sleep(1)
            except:
                pass
        except Exception as e :
            print(e)
            SocketServerHelper.__remove_from_avail_client_address_list(client_socket, client_addr)
            self.event_dispatcher.dispatch_event(SocketDataEvent(SocketDataEvent.Disconnected, {'srcAddress': client_addr}))
            break

def __read_data(self, client_socket, client_addr):
    lasttime = datetime.datetime.now()
    leftbytes = []
    while True:
        try:
            if SocketServerHelper.__exist_in_avail_client_address_list(client_addr):
                infds_c, outfds_c, eerrfds_c = select.select([client_socket, ], [], [], 1)
                if len(infds_c) != 0:
                    buf = client_socket.recv(4096)
                    print_and_log("接收到的数据包【长度】为:%d；【类型】为：%s；【内容】为: %s" % (len(buf), type(buf), buf))
                    try:
                        if buf is None or len(buf) == 0:
                            continue
                        else:
                            structs, payload, dataleft, lt = self.__read_complete(buf, leftbytes, lasttime)
                            lasttime = lt
                            leftbytes = dataleft
                            if len(structs) > 0:
                                self.event_dispatcher.dispatch_event(SocketDataEvent(SocketDataEvent.Received, {'srcAddress':client_addr, 'data':payload}))
                    except Exception as e:
                        leftbytes = []
                        lasttime = datetime.datetime.now()
            else:
                break
        except:
            SocketServerHelper.__remove_from_avail_client_address_list(client_socket, client_addr)
            self.event_dispatcher.dispatch_event(SocketDataEvent(SocketDataEvent.Disconnected, {'srcAddress':client_addr}))
            break
```
