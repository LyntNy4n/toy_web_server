# toy_web_server

一个简易web服务器,使用Rust实现,并且自带了一个简易的线程池以供多线程运行

## 服务器处理流程

1. 使用标准库`net::{TcpListener, TcpStream}`监听127.0.0.1:7878
2. 如果接收到连接请求,把请求发送到线程池的消息队列里,让其中一个线程处理
3. 读取http请求行(目前只支持GET方法),根据GET路径返回不同的html或函数
4. 把html包装在http响应报文中返回给客户端

### 线程池实现简介

线程池使用多生产者单消费者(mpsc)的消息队列来传递函数给线程

它需要一个地方保存所有线程,并且自己拿着消息队列的发送端

线程在线程池初始化时就会全部创建,线程函数体拿着消息队列的接收端(需要用Arc和Mutex),循环阻塞地获取接收端的数据(函数),一旦收到就执行



更多细节可查看 https://lyntny4n.github.io/post/toy_web_server/