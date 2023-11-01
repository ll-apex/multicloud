# 修改 Helidon Nima 和 Reactive 應用程式並分析堆疊追蹤

## 簡介

在此實驗室中，您將修改 helidon nima 與 reactive 應用程式。然後您將重建應用程式，並在發生異常狀況時分析堆疊追蹤。

[Lab3 逐步解說](videohub:1_gq37iysp)

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   修改、建置及執行 Helidon Nima 應用程式
*   要求分析 nima 應用程式的堆疊追蹤時發生異常狀況
*   修改、建置及執行 Helidon Reactive 應用程式
*   要求分析反應應用程式的堆疊追蹤時發生異常狀況

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 作業 1：修改 Nima 應用程式並建置應用程式

1.  回到 _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ 檔案，然後將下面一行新增至 _one_ 方法，如圖所示。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![修改 Nima](images/modify-nima.png)
    
2.  將下列指令複製並貼至終端機以建立應用程式。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > 確定使用終端機，在其中設定 PATH 與 JAVA\_HOME 變數。
    
3.  複製並貼上下列命令以執行 nima 應用程式。
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    您會有類似以下的執行結果：
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  回到終端機，開啟以執行 curl 命令。將下列 curl 指令複製並貼上至終端機。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  注意伺服器紀錄中的輸出內容，您會看到如下 ：
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## 作業 2：分析 Nima 應用程式的堆疊追蹤

1.  複製並貼上下列命令以強制發生異常狀況 (計數必須是整數！)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    您將輸出與下列類似：
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  檢查伺服器日誌，您的輸出結果是否與下方類似：
    
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
        
        
3.  在執行 \*java -jar \* 命令的終端機中按 _Ctrl + C_ 以停止伺服器。
    

## 作業 3：修改反應應用程式並建置應用程式

1.  回到 _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ 檔案，並將下列一行新增至 _one_ 方法，如圖所示。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![修改被動](images/modify-reactive.png)
    
2.  在終端機中複製並貼上下列命令以建立應用程式。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > 確定使用終端機，在其中設定 PATH 與 JAVA\_HOME 變數。
    
3.  複製並貼上下列命令以執行 nima 應用程式。
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    您會有類似以下的執行結果：
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  回到終端機，開啟以執行 curl 命令。將下列 curl 指令複製並貼上至終端機。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  注意伺服器紀錄中的輸出內容，您會看到如下 ：
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > 這是 Netty 事件迴圈執行緒，您可以封鎖 VirtualThread。這表示 Nima 要求處理常式可以使用簡單區塊代碼，但反應處理常式不能使用。
    

## 作業 4：分析反應應用程式的堆疊追蹤

1.  複製並貼上下列命令以強制發生異常狀況 (計數必須是整數！)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    您將輸出與下列類似：
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  檢查伺服器日誌，您的輸出結果是否與下方類似：
    
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
        
    
    > 讓我們比較相同類型之異常狀況的 Níma (阻隔) 與 Helidon SE (反應) 的堆疊追蹤。
    
    *   反應性追蹤很長，且包含來自事件迴圈管理員的呼叫 (例如 Netty)
    *   事件迴圈管理程式可呼叫任何由應用程式註冊的處理程式
        *   執行流程不一定符合程式碼順序
        *   在執行期間可以不同時間呼叫處理程式來處理事件
    *   由於訂單保留，封鎖追蹤是容易理解的
        *   由於虛擬執行緒阻隔作業已暫停，且之後繼續
        *   執行流程簡單明瞭，可遵循
3.  在執行 \*java -jar \* 命令的終端機中按 _Ctrl + C_ 以停止伺服器。
    

您現在可以_進入下一個實驗室_。

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月