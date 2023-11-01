# Helidon Nima 및 Reactive 애플리케이션 수정 및 스택 추적 분석

## 소개

이 연습에서는 helidon nima 및 reactive 응용 프로그램을 수정합니다. 그런 다음 응용 프로그램을 재구축하고 예외 발생 시 스택 추적을 분석합니다.

[Lab3 연습](videohub:1_gq37iysp)

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   Helidon Nima 애플리케이션 수정, 구축 및 실행
*   nima 애플리케이션에 대한 스택 추적 분석을 요청하는 중 예외사항 발생
*   Helidon Reactive 애플리케이션 수정, 빌드 및 실행
*   반응적 애플리케이션에 대한 스택 추적 분석을 요청하는 중 예외사항 발생

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 작업 1: Nima 애플리케이션 수정 및 애플리케이션 빌드

1.  _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ 파일로 돌아가서 그림과 같이 메소드 _one_에 다음 행을 추가합니다.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Nima 수정](images/modify-nima.png)
    
2.  다음 명령을 복사하여 터미널에 붙여넣어 응용 프로그램을 작성합니다.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > PATH 및 JAVA\_HOME 변수를 설정한 터미널을 사용해야 합니다.
    
3.  다음 명령을 복사하여 붙여넣어 nima 애플리케이션을 실행합니다.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  curl 명령을 실행하기 위해 연 터미널로 돌아갑니다. 다음 컬 명령을 복사하여 터미널에 붙여넣습니다.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  서버 로그에서 출력을 확인합니다. 다음과 같은 내용이 표시됩니다.
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## 작업 2: Nima 애플리케이션에 대한 스택 추적 분석

1.  다음 명령을 복사하여 붙여넣어 예외사항을 강제 적용합니다(개수는 정수여야 함).
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  서버 로그를 확인하십시오. 다음과 유사한 출력이 표시됩니다.
    
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
        
        
3.  \*java -jar \* 명령이 실행 중인 터미널에서 _Ctrl + C_를 눌러 서버를 정지합니다.
    

## 작업 3: 반응적 응용 프로그램 수정 및 응용 프로그램 빌드

1.  _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ 파일로 돌아가서 다음과 같이 메소드 _one_에 다음 행을 추가합니다.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![사후 수정](images/modify-reactive.png)
    
2.  다음 명령을 복사하여 터미널에 붙여넣어 응용 프로그램을 작성합니다.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > PATH 및 JAVA\_HOME 변수를 설정한 터미널을 사용해야 합니다.
    
3.  다음 명령을 복사하여 붙여넣어 nima 애플리케이션을 실행합니다.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  curl 명령을 실행하기 위해 연 터미널로 돌아갑니다. 다음 컬 명령을 복사하여 터미널에 붙여넣습니다.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  서버 로그에서 출력을 확인합니다. 다음과 같은 내용이 표시됩니다.
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > Netty 이벤트 루프 스레드입니다. VirtualThread를 차단할 수 있습니다. 즉, Nima 요청 처리기는 간단한 차단 코드를 사용할 수 있지만 반응적 처리기는 사용할 수 없습니다.
    

## 작업 4: 반응적 애플리케이션에 대한 스택 추적 분석

1.  다음 명령을 복사하여 붙여넣어 예외사항을 강제 적용합니다(개수는 정수여야 함).
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  서버 로그를 확인하십시오. 다음과 유사한 출력이 표시됩니다.
    
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
        
    
    > 동일한 유형의 예외에 대해 Níma(차단) 및 Helidon SE(반응적)의 스택 추적을 비교해 보겠습니다.
    
    *   반응적 추적은 길고 이벤트 루프 관리자(예: Netty)의 호출을 포함합니다.
    *   이벤트 루프 관리자는 응용 프로그램에서 등록된 처리기를 호출할 수 있습니다.
        *   실행 흐름이 반드시 코드 순서에 도움이 되는 것은 아닙니다.
        *   이벤트를 처리하기 위해 실행하는 동안 다른 시간에 처리기를 호출할 수 있습니다.
    *   추적 차단은 주문 보존으로 인해 이해하기 쉽습니다.
        *   가상 스레드 차단 작업으로 인해 작업이 일시 중지되고 나중에 계속됩니다.
        *   실행 흐름은 간단하고 명확합니다.
3.  \*java -jar \* 명령이 실행 중인 터미널에서 _Ctrl + C_를 눌러 서버를 정지합니다.
    

이제 _다음 실습으로 진행_할 수 있습니다.

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월