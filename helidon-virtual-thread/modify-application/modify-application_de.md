# Ändern Sie die Anwendung Helidon Nima und Reactive, und analysieren Sie das Stacktrace

## Einführung

In dieser Übung ändern Sie das Helidon nima und die reaktive Anwendung. Anschließend erstellen Sie die Anwendung neu und analysieren das Stacktrace im Falle von Ausnahmen.

[Durchlauf durch Lab3](videohub:1_gq37iysp)

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Helidon Nima-Anwendung ändern, erstellen und ausführen
*   Ausnahme in Anforderung zur Analyse des Stacktrace für die nima-Anwendung erstellen
*   Helidon Reactive-Anwendung ändern, erstellen und ausführen
*   Ausnahme in Anforderung zur Analyse des Stacktrace für reaktive Anwendung erstellen

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Aufgabe 1: Nima-Anwendung ändern und Anwendung erstellen

1.  Gehen Sie zurück zur Datei _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_, und fügen Sie die folgende Zeile zur Methode _one_ hinzu (siehe Abbildung).
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Nima ändern](images/modify-nima.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um die Anwendung zu erstellen.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > Stellen Sie sicher, dass Sie das Terminal verwenden, in dem Sie die Variablen PATH und JAVA\_HOME festgelegt haben.
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die nima-Anwendung auszuführen.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  Kehren Sie zum Terminal zurück, das Sie zum Ausführen des curl-Befehls öffnen. Kopieren Sie den folgenden curl-Befehl, und fügen Sie ihn in das Terminal ein.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  Beachten Sie die Ausgabe im Serverlog. Folgendes wird angezeigt:
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## Aufgabe 2: Stack Trace für die Nima-Anwendung analysieren

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um eine Ausnahme zu erzwingen (count muss eine Ganzzahl sein!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Sie erhalten folgende Ausgabe:
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  Prüfen Sie das Serverlog. Sie erhalten eine Ausgabe ähnlich der folgenden:
    
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
        
        
3.  Drücken Sie _Strg + C_ im Terminal, in dem der Befehl \*java -jar \* ausgeführt wird, um den Server zu stoppen.
    

## Aufgabe 3: Reaktive Anwendung ändern und Anwendung erstellen

1.  Gehen Sie zurück zur Datei _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_, und fügen Sie der Methode _one_ wie gezeigt die folgende Zeile hinzu.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Reaktiv ändern](images/modify-reactive.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um die Anwendung zu erstellen.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > Stellen Sie sicher, dass Sie das Terminal verwenden, in dem Sie die Variablen PATH und JAVA\_HOME festgelegt haben.
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die nima-Anwendung auszuführen.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  Kehren Sie zum Terminal zurück, das Sie zum Ausführen des curl-Befehls öffnen. Kopieren Sie den folgenden curl-Befehl, und fügen Sie ihn in das Terminal ein.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  Beachten Sie die Ausgabe im Serverlog. Folgendes wird angezeigt:
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > Dies ist ein Netty-Ereignisschleifen-Thread. Sie können einen VirtualThread blockieren. Dies bedeutet, dass die Nima-Request-Handler einfachen Blockierungscode verwenden können, die reaktiven Handler jedoch nicht.
    

## Aufgabe 4: Stack Trace für reaktive Anwendung analysieren

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um eine Ausnahme zu erzwingen (count muss eine Ganzzahl sein!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Sie erhalten folgende Ausgabe:
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  Prüfen Sie das Serverlog. Sie erhalten eine Ausgabe ähnlich der folgenden:
    
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
        
    
    > Vergleichen wir das Stacktrace von Níma (Blockieren) und Helidon SE (Reaktiv) für denselben Typ von Exception.
    
    *   Reaktive Traces sind lang und umfassen Aufrufe von Event Loop Managern (z.B. Netty)
    *   Der Event Loop Manager kann jeden der von der Anwendung registrierten Handler aufrufen
        *   Der Ausführungsablauf ist nicht unbedingt förderlich für die Codereihenfolge
        *   Handler können zu verschiedenen Zeiten während der Ausführung aufgerufen werden, um Ereignisse zu verarbeiten
    *   Blockierspuren sind aufgrund der Bestellungserhaltung einfach zu verstehen
        *   Dank virtueller Threads werden Blockierungsvorgänge unterbrochen und später fortgesetzt
        *   Der Ausführungsablauf ist einfach und leicht zu befolgen
3.  Drücken Sie _Strg + C_ im Terminal, in dem der Befehl \*java -jar \* ausgeführt wird, um den Server zu stoppen.
    

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023