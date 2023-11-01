# 修改 Helidon Nima 和 Reactive 应用程序并分析堆栈跟踪

## 简介

在本练习中，您将修改 Helidon nima 和反应性应用程序。然后，您将重建应用程序并在出现异常时分析堆栈跟踪。

[Lab3 演练](videohub:1_gq37iysp)

估计时间：10 分钟

### 目标

在本练习中，您将：

*   修改、构建和运行 Helidon Nima 应用程序
*   在请求中发出异常错误来分析 nima 应用程序的堆栈跟踪
*   修改、构建和运行 Helidon Reactive 应用程序
*   在请求中出现异常错误，分析反应性应用程序的堆栈跟踪

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 任务 1：修改 Nima 应用程序并构建应用程序

1.  返回到文件 _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ ，然后将以下行添加到方法 _one_ 中，如下所示。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![修改 Nima](images/modify-nima.png)
    
2.  将以下命令复制并粘贴到终端以构建应用程序。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > 确保使用已设置 PATH 和 JAVA\_HOME 变量的终端。
    
3.  复制并粘贴以下命令以运行 nima 应用程序。
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    您的输出如下所示：
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  返回到为运行 curl 命令而打开的终端。将以下 curl 命令复制并粘贴到终端。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  观察服务器日志中的输出，您将看到如下所示：
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## 任务 2：分析 Nima 应用程序的堆栈跟踪

1.  复制并粘贴以下命令以强制执行异常错误（计数必须为整数！）
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    输出结果如下：
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  检查服务器日志，您的输出如下所示：
    
        2023.02.28 03:19:49.910 Internal server error
        io.helidon.common.http.RequestException: For input string: "foo"
            at io.helidon.common.http.RequestException$Builder.build(RequestException.java:126)
            at io.helidon.nima.webserver.http.ErrorHandlers.unhandledError(ErrorHandlers.java:201)
            at io.helidon.nima.webserver.http.ErrorHandlers.lambda$handleError$1(ErrorHandlers.java:181)
            at java.base/java.util.Optional.ifPresentOrElse(Optional.java:198)
            at io.helidon.nima.webserver.http.ErrorHandlers.handleError(ErrorHandlers.java:180)
            at io.helidon.nima.webserver.http.ErrorHandlers.runWithErrorHandling(ErrorHandlers.java:118)
            at io.helidon.nima.webserver.http.Filters$FilterChainImpl.proceed(Filters.java:116)
            at io.examples.helidon.nima.NimaMain.lambda$routing$0(NimaMain.java:53)
            at io.helidon.nima.webserver.http.Filters$FilterChainImpl.runNextFilter(Filters.java:127)
            at io.helidon.nima.webserver.http.ErrorHandlers.runWithErrorHandling(ErrorHandlers.java:75)
            at io.helidon.nima.webserver.http.Filters$FilterChainImpl.proceed(Filters.java:114)
            at io.helidon.nima.webserver.http.Filters.filter(Filters.java:83)
            at io.helidon.nima.webserver.http.HttpRouting.route(HttpRouting.java:108)
            at io.helidon.nima.webserver.http1.Http1Connection.route(Http1Connection.java:259)
            at io.helidon.nima.webserver.http1.Http1Connection.handle(Http1Connection.java:145)
            at io.helidon.nima.webserver.ConnectionHandler.run(ConnectionHandler.java:107)
            at io.helidon.common.task.InterruptableTask.call(InterruptableTask.java:47)
            at io.helidon.nima.webserver.ThreadPerTaskExecutor$ThreadBoundFuture.run(ThreadPerTaskExecutor.java:234)
            at java.base/java.lang.VirtualThread.run(VirtualThread.java:287)
            at java.base/java.lang.VirtualThread$VThreadContinuation.lambda$new$0(VirtualThread.java:174)
            at java.base/jdk.internal.vm.Continuation.enter0(Continuation.java:327)
            at java.base/jdk.internal.vm.Continuation.enter(Continuation.java:320)
        Caused by: java.lang.NumberFormatException: For input string: "foo"
            at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
            at java.base/java.lang.Integer.parseInt(Integer.java:665)
            at java.base/java.lang.Integer.parseInt(Integer.java:781)
            at java.base/java.util.Optional.map(Optional.java:260)
            at io.examples.helidon.nima.BlockingService.count(BlockingService.java:96)
            at io.examples.helidon.nima.BlockingService.parallel(BlockingService.java:74)
            at io.helidon.nima.webserver.http.HttpRouting$RoutingExecutor.doRoute(HttpRouting.java:455)
            at io.helidon.nima.webserver.http.HttpRouting$RoutingExecutor.call(HttpRouting.java:414)
            at io.helidon.nima.webserver.http.HttpRouting$RoutingExecutor.call(HttpRouting.java:392)
            at io.helidon.nima.webserver.http.ErrorHandlers.runWithErrorHandling(ErrorHandlers.java:75)
            ... 16 more
            ```
        
        
3.  在运行 \*java -jar \* 命令的终端中按 _Ctrl + C_ 可停止服务器。
    

## 任务 3：修改反应性应用程序并构建应用程序

1.  返回到文件 _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ ，然后将以下行添加到方法 _one_ 中，如下所示。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![修改反应性](images/modify-reactive.png)
    
2.  在终端中复制并粘贴以下命令以构建应用程序。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > 确保使用已设置 PATH 和 JAVA\_HOME 变量的终端。
    
3.  复制并粘贴以下命令以运行 nima 应用程序。
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    您的输出如下所示：
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  返回到为运行 curl 命令而打开的终端。将以下 curl 命令复制并粘贴到终端。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  观察服务器日志中的输出，您将看到如下所示：
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > 这是 Netty 事件循环线程，您可以阻止 VirtualThread。这意味着尼玛请求处理程序可以使用简单的阻塞代码，但被动处理程序不能。
    

## 任务 4：分析反应性应用程序的堆栈跟踪

1.  复制并粘贴以下命令以强制执行异常错误（计数必须为整数！）
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    输出结果如下：
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  检查服务器日志，您的输出如下所示：
    
        2023.02.28 03:36:06.853 Default error handler: Unhandled exception encountered.
        java.util.concurrent.ExecutionException: Unhandled 'cause' of this exception encountered.
            at io.helidon.reactive.webserver.RequestRouting$RoutedRequest.defaultHandler(RequestRouting.java:401)
            at io.helidon.reactive.webserver.RequestRouting$RoutedRequest.nextNoCheck(RequestRouting.java:382)
            at io.helidon.reactive.webserver.RequestRouting$RoutedRequest.next(RequestRouting.java:331)
            at io.helidon.reactive.webserver.WebTracingConfig$RequestSpanHandler.accept(WebTracingConfig.java:278)
            at io.helidon.reactive.webserver.RequestRouting$RoutedRequest.next(RequestRouting.java:329)
            at io.helidon.reactive.webserver.RequestRouting.route(RequestRouting.java:91)
            at io.helidon.reactive.webserver.ForwardingHandler.lambda$channelReadHttpRequest$17(ForwardingHandler.java:459)
            at io.helidon.reactive.webserver.RequestContext.lambda$runInScope$4(RequestContext.java:77)
            at io.helidon.common.context.Contexts.runInContext(Contexts.java:117)
            at io.helidon.reactive.webserver.RequestContext.runInScope(RequestContext.java:75)
            at io.helidon.reactive.webserver.ForwardingHandler.channelReadHttpRequest(ForwardingHandler.java:459)
            at io.helidon.reactive.webserver.ForwardingHandler.lambda$channelRead0$3(ForwardingHandler.java:159)
            at io.helidon.common.context.Contexts.runInContext(Contexts.java:137)
            at io.helidon.reactive.webserver.ForwardingHandler.channelRead0(ForwardingHandler.java:158)
            at io.netty.channel.SimpleChannelInboundHandler.channelRead(SimpleChannelInboundHandler.java:99)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:444)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420)
            at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:412)
            at io.netty.channel.CombinedChannelDuplexHandler$DelegatingChannelHandlerContext.fireChannelRead(CombinedChannelDuplexHandler.java:436)
            at io.netty.handler.codec.ByteToMessageDecoder.fireChannelRead(ByteToMessageDecoder.java:346)
            at io.netty.handler.codec.ByteToMessageDecoder.channelRead(ByteToMessageDecoder.java:318)
            at io.netty.channel.CombinedChannelDuplexHandler.channelRead(CombinedChannelDuplexHandler.java:251)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:442)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420)
            at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:412)
            at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1410)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:440)
            at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420)
            at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:919)
            at io.netty.channel.nio.AbstractNioByteChannel$NioByteUnsafe.read(AbstractNioByteChannel.java:166)
            at io.netty.channel.nio.NioEventLoop.processSelectedKey(NioEventLoop.java:788)
            at io.netty.channel.nio.NioEventLoop.processSelectedKeysOptimized(NioEventLoop.java:724)
            at io.netty.channel.nio.NioEventLoop.processSelectedKeys(NioEventLoop.java:650)
            at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:562)
            at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:997)
            at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
            at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
            at java.base/java.lang.Thread.run(Thread.java:1589)
        Caused by: java.lang.NumberFormatException: For input string: "foo"
            at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
            at java.base/java.lang.Integer.parseInt(Integer.java:665)
            at java.base/java.lang.Integer.parseInt(Integer.java:781)
            at java.base/java.util.Optional.map(Optional.java:260)
            at io.examples.helidon.reactive.ReactiveService.count(ReactiveService.java:96)
            at io.examples.helidon.reactive.ReactiveService.parallel(ReactiveService.java:72)
            at io.helidon.reactive.webserver.RequestRouting$RoutedRequest.next(RequestRouting.java:329)
            ... 35 more
        
    
    > 我们将 Níma（阻塞）和 Helidon SE（反应性）的堆栈跟踪与同一类型的异常进行比较。
    
    *   反应性跟踪很长，包括事件循环经理的调用（例如 Netty）
    *   事件循环管理器可以调用应用程序注册的任何处理程序
        *   执行流不一定有助于代码顺序
        *   可以在执行期间不同时间调用处理程序来处理事件
    *   由于订单保留，拦截跟踪很容易理解
        *   由于虚拟线程阻塞操作已暂停，随后继续
        *   执行流简单明了，易于遵循
3.  在运行 \*java -jar \* 命令的终端中按 _Ctrl + C_ 可停止服务器。
    

您现在可以_进入下一个实验_。

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月