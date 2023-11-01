# Modificar o aplicativo Helidon Nima e Reativo e analisar o rastreamento da pilha

## Introdução

Neste laboratório, você modificará o helidon nima e a aplicação reativa. Em seguida, você recriará o aplicativo e analisará o rastreamento de pilha em caso de exceções.

[Passo a passo de Lab3](videohub:1_gq37iysp)

Tempo Estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Modificar, criar e executar o aplicativo Helidon Nima
*   Faça uma exceção na solicitação para analisar o rastreamento de pilha para o aplicativo nima
*   Modificar, criar e executar o aplicativo Helidon Reactive
*   Faça uma exceção na solicitação para analisar o rastreamento da pilha para o aplicativo reativo

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarefa 1: Modificar o aplicativo Nima e criar o aplicativo

1.  Volte ao arquivo _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ e adicione a linha a seguir ao método _one_, conforme mostrado.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modificar Nima](images/modify-nima.png)
    
2.  Copie e cole o comando a seguir no terminal para criar o aplicativo.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > Certifique-se de usar o terminal, onde você definiu as variáveis PATH e JAVA\_HOME.
    
3.  Copie e cole o comando a seguir para executar o aplicativo nima.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  Volte ao terminal, que você abre para executar o comando curl. Copie e cole o comando curl a seguir no terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  Observe a saída no log do servidor. Você verá algo como o seguinte:
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## Tarefa 2: Analisar rastreamento de pilha para o aplicativo Nima

1.  Copie e cole o seguinte comando para forçar uma exceção (a contagem deve ser um número inteiro!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Você produzirá um resultado semelhante ao seguinte:
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  Verifique o log do servidor, você terá uma saída semelhante à seguinte:
    
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
        
        
3.  Pressione _Ctrl + C_ no terminal em que o comando \*java -jar \* está sendo executado para interromper o servidor.
    

## Tarefa 3: Modificar o aplicativo Reativo e criar o aplicativo

1.  Volte ao arquivo _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ e adicione a linha a seguir ao método _one_, conforme mostrado.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modificar Reativo](images/modify-reactive.png)
    
2.  Copie e cole o comando a seguir no terminal para criar o aplicativo.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > Certifique-se de usar o terminal, onde você definiu as variáveis PATH e JAVA\_HOME.
    
3.  Copie e cole o comando a seguir para executar o aplicativo nima.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  Volte ao terminal, que você abre para executar o comando curl. Copie e cole o comando curl a seguir no terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  Observe a saída no log do servidor. Você verá algo como o seguinte:
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > Este é um thread de loop de evento Netty; você pode bloquear um VirtualThread. Isso significa que os handlers de solicitação Nima podem usar código de bloqueio simples, mas os handlers reativos não devem.
    

## Tarefa 4: Analisar rastreamento de pilha para aplicativo Reativo

1.  Copie e cole o seguinte comando para forçar uma exceção (a contagem deve ser um número inteiro!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Você produzirá um resultado semelhante ao seguinte:
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  Verifique o log do servidor, você terá uma saída semelhante à seguinte:
    
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
        
    
    > Vamos comparar o rastreamento de pilha de Níma (bloqueio) e Helidon SE (reativo) para o mesmo tipo de exceção.
    
    *   Os rastreamentos reativos são longos e incluem chamadas de gerentes de loop de evento (por exemplo, Netty)
    *   O gerenciador de loops de eventos pode chamar qualquer um dos processadores registrados pelo aplicativo
        *   O fluxo de execução não é necessariamente propício à ordem do código
        *   Os handlers podem ser chamados em momentos diferentes durante a execução para tratar eventos
    *   Os rastreamentos de bloqueio são fáceis de entender devido à preservação da ordem
        *   Graças às operações de bloqueio de threads virtuais, elas são suspensas e continuarão mais tarde
        *   O fluxo de execução é simples e claro de seguir
3.  Pressione _Ctrl + C_ no terminal em que o comando \*java -jar \* está sendo executado para interromper o servidor.
    

Agora você pode _acessar o próximo laboratório_.

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023