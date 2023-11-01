# Modificar la aplicación Helidon Nima y Reactive y analizar el rastreo de pila

## Introducción

En este laboratorio, modificará el helidón nima y la aplicación reactiva. A continuación, volverá a crear la aplicación y analizará el rastreo de pila en caso de excepciones.

[Recorrido virtual por Lab3](videohub:1_gq37iysp)

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Modificar, crear y ejecutar la aplicación Helidon Nima
*   Hacer una excepción en la solicitud para analizar el rastreo de pila para la aplicación nima
*   Modificar, crear y ejecutar la aplicación Helidon Reactive
*   Hacer una excepción en la solicitud para analizar el rastreo de pila para una aplicación reactiva

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarea 1: Modificar la aplicación Nima y crear la aplicación

1.  Vuelva al archivo _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ y agregue la siguiente línea al método _one_, como se muestra.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modificar Nima](images/modify-nima.png)
    
2.  Copie y pegue el siguiente comando en el terminal para crear la aplicación.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > Asegúrese de utilizar el terminal, donde ha definido las variables PATH y JAVA\_HOME.
    
3.  Copie y pegue el siguiente comando para ejecutar la aplicación nima.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  Vuelva al terminal, que abre para ejecutar el comando curl. Copie y pegue el siguiente comando curl en el terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  Observe la salida en el log del servidor, verá algo como lo siguiente:
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## Tarea 2: Analizar el rastreo de pila para la aplicación Nima

1.  Copie y pegue el siguiente comando para forzar una excepción (¡el recuento debe ser un entero!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Aparecerá un resultado similar al siguiente:
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  Compruebe el log del servidor; tendrá una salida similar a la siguiente:
    
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
        
        
3.  Pulse _Ctrl + C_ en el terminal donde se ejecuta el comando \*java -jar \* para detener el servidor.
    

## Tarea 3: Modificación de la aplicación Reactiva y creación de la aplicación

1.  Vuelva al archivo _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ y agregue la siguiente línea al método _one_, como se muestra.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modificar reactivo](images/modify-reactive.png)
    
2.  Copie y pegue el siguiente comando en el terminal para crear la aplicación.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > Asegúrese de utilizar el terminal, donde ha definido las variables PATH y JAVA\_HOME.
    
3.  Copie y pegue el siguiente comando para ejecutar la aplicación nima.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  Vuelva al terminal, que abre para ejecutar el comando curl. Copie y pegue el siguiente comando curl en el terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  Observe la salida en el log del servidor, verá algo como lo siguiente:
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > Este es un thread de bucle de evento de red; puede bloquear VirtualThread. Esto significa que los manejadores de solicitudes Nima pueden utilizar un código de bloqueo simple, pero los manejadores reactivos no deben hacerlo.
    

## Tarea 4: Analizar el rastreo de pila para una aplicación reactiva

1.  Copie y pegue el siguiente comando para forzar una excepción (¡el recuento debe ser un entero!)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    Aparecerá un resultado similar al siguiente:
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  Compruebe el log del servidor; tendrá una salida similar a la siguiente:
    
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
        
    
    > Comparemos el rastreo de pila de Níma (bloqueo) y Helidon SE (reactivo) para el mismo tipo de excepción.
    
    *   Los rastreos reactivos son largos e incluyen llamadas de gestores de bucle de eventos (por ejemplo, Netty)
    *   El gestor de bucle de eventos puede llamar a cualquiera de los gestores registrados por la aplicación
        *   El flujo de ejecución no es necesariamente conducente al orden de código
        *   Los manejadores se pueden llamar en diferentes momentos durante la ejecución para manejar eventos
    *   Los rastreos de bloqueo son fáciles de entender debido a la conservación del pedido
        *   Gracias a los threads virtuales se suspenden las operaciones de bloqueo y posteriormente se continúan
        *   El flujo de ejecución es sencillo y claro de seguir
3.  Pulse _Ctrl + C_ en el terminal donde se ejecuta el comando \*java -jar \* para detener el servidor.
    

Ahora puede _proceder al siguiente laboratorio_.

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023