# Helidon NimaおよびReactiveアプリケーションを変更し、スタック・トレースを分析します

## 概要

この演習では、helidon nimaおよびリアクティブ・アプリケーションを変更します。次に、アプリケーションを再構築し、例外が発生した場合にスタック・トレースを分析します。

[Lab3ウォークスルー](videohub:1_gq37iysp)

見積時間: 10分

### 目的

この演習では、次のことを行います。

*   Helidon Nimaアプリケーションを変更、構築および実行します
*   nimaアプリケーションのスタック・トレースを分析するリクエストで例外を作成します
*   Helidon Reactiveアプリケーションを変更、構築、実行
*   リアクティブ・アプリケーションのスタック・トレースを分析するリクエストで例外を作成します

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## タスク1: Nimaアプリケーションを変更してアプリケーションを構築する

1.  _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ファイルに戻り、次に示すようにメソッド _one_に次の行を追加します。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Nimaの変更](images/modify-nima.png)
    
2.  次のコマンドをコピーして端末に貼り付け、アプリケーションをビルドします。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > PATH変数およびJAVA\_HOME変数を設定した端末を使用してください。
    
3.  次のコマンドをコピーして貼り付け、nimaアプリケーションを実行します。
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  curlコマンドを実行するために開いた端末に戻ります。次のcurlコマンドをコピーして端末に貼り付けます。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  サーバー・ログの出力に、次のような内容が表示されます。
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## タスク2: Nimaアプリケーションのスタック・トレースの分析

1.  次のコマンドをコピーして貼り付け、例外を強制します(カウントは整数である必要があります!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    次のような出力が表示されます。
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  サーバー・ログを確認すると、次のような出力が表示されます。
    
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
        
        
3.  \*java -jar \*コマンドが実行されている端末で_\[Ctrl\]を押しながら\[C\]_を押してサーバーを停止します。
    

## タスク3: リアクティブ・アプリケーションの変更およびアプリケーションのビルド

1.  _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ファイルに戻り、次に示すようにメソッド _one_に次の行を追加します。
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![リアクティブの変更](images/modify-reactive.png)
    
2.  次のコマンドをコピーして端末に貼り付け、アプリケーションをビルドします。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > PATH変数およびJAVA\_HOME変数を設定した端末を使用してください。
    
3.  次のコマンドをコピーして貼り付け、nimaアプリケーションを実行します。
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  curlコマンドを実行するために開いた端末に戻ります。次のcurlコマンドをコピーして端末に貼り付けます。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  サーバー・ログの出力に、次のような内容が表示されます。
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > これはNettyイベント・ループ・スレッドです。VirtualThreadをブロックできます。つまり、Nimaリクエスト・ハンドラは単純なブロック・コードを使用できますが、リアクティブ・ハンドラは使用できません。
    

## タスク4: リアクティブ・アプリケーションのスタック・トレースの分析

1.  次のコマンドをコピーして貼り付け、例外を強制します(カウントは整数である必要があります!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  サーバー・ログを確認すると、次のような出力が表示されます。
    
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
        
    
    > 同じタイプの例外について、Níma(ブロッキング)とHelidon SE(リアクティブ)のスタック・トレースを比較します。
    
    *   リアクティブ・トレースは長く、イベント・ループ・マネージャからのコールが含まれます(例: Netty)。
    *   イベント・ループ・マネージャは、アプリケーションによって登録されたハンドラをコールできます。
        *   実行フローが必ずしもコード順序を促すわけではありません。
        *   ハンドラは、実行中に異なるタイミングで呼び出してイベントを処理できます。
    *   ブロック・トレースは、オーダーの保存によりわかりやすくなります。
        *   仮想スレッド・ブロック操作が一時停止され、その後も継続されます。
        *   実行フローはシンプルで明確です
3.  \*java -jar \*コマンドが実行されている端末で_\[Ctrl\]を押しながら\[C\]_を押してサーバーを停止します。
    

_次の演習に進む_ことができます。

## 確認

*   **作成者** - Joe DiPol
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月