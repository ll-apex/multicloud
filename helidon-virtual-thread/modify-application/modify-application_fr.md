# Modifier l'application Helidon Nima et Reactive et analyser la trace de pile

## Présentation

Dans cet exercice, vous allez modifier l'application helidon nima et l'application réactive. Vous allez ensuite reconstruire l'application et analyser la trace de pile en cas d'exceptions.

[Revue de processus Lab3](videohub:1_gq37iysp)

Temps estimé : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Modifier, créer et exécuter l'application Helidon Nima
*   Faire une exception dans la demande d'analyse de la trace de pile pour l'application nima
*   Modifier, créer et exécuter l'application Helidon Reactive
*   Faire une exception dans la demande pour analyser la trace de pile pour une application réactive

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## Tâche 1 : modifier l'application Nima et la paramétrer

1.  Revenez au fichier _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ et ajoutez la ligne suivante à la méthode _one_ comme indiqué.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modifier Nima](images/modify-nima.png)
    
2.  Copiez et collez la commande suivante dans le terminal pour créer l'application.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    > Veillez à utiliser le terminal, où vous avez défini les variables PATH et JAVA\_HOME.
    
3.  Copiez et collez la commande suivante pour exécuter l'application nima.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.28 03:13:54.951 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:13:55.295 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:13:56.094 [0x314d2373] http://127.0.0.1:35037 bound for socket '@default'
        2023.02.28 03:13:56.097 [0x314d2373] direct writes
        2023.02.28 03:13:56.124 Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:13:56.139 Started all channels in 54 milliseconds. 1396 milliseconds since JVM startup. Java 19.0.2+7-44
        
4.  Revenez au terminal que vous ouvrez pour exécuter la commande curl. Copiez et collez la commande curl suivante dans le terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:35037/one
        remote_1
        $
        
5.  Observez la sortie dans le journal du serveur, vous verrez quelque chose comme ci-dessous :
    
        ## one VirtualThread[#27,[0x75c91e4b 0x3b495792] Nima socket]/runnable@ForkJoinPool-1-worker-2
        

## Tâche 2 : Analyse de la trace de pile pour l'application Nima

1.  Copiez et collez la commande suivante pour forcer une exception (le nombre doit être un entier !)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    La sortie se présente comme suit :
    
        $ curl "http://localhost:35037/parallel?count=foo"
        For input string: &quot;foo&quot;$
        
2.  Consultez le journal du serveur, vous obtiendrez une sortie similaire à la suivante :
    
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
        
        
3.  Appuyez sur _Ctrl + C_ dans le terminal où la commande \*java -jar \* est en cours d'exécution pour arrêter le serveur.
    

## Tâche 3 : modifier l'application réactive et créer l'application

1.  Revenez au fichier _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ et ajoutez la ligne suivante à la méthode _one_ comme indiqué.
    
        <copy>System.out.println("## one " + Thread.currentThread());</copy>
        
    
    ![Modifier la réactivité](images/modify-reactive.png)
    
2.  Copiez et collez la commande suivante dans le terminal pour créer l'application.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    > Veillez à utiliser le terminal, où vous avez défini les variables PATH et JAVA\_HOME.
    
3.  Copiez et collez la commande suivante pour exécuter l'application nima.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.28 03:33:03.624 Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:33:04.193 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.28 03:33:04.737 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.28 03:33:05.113 Channel '@default' started: [id: 0x75db5394, L:/0.0.0.0:45153]
        
4.  Revenez au terminal que vous ouvrez pour exécuter la commande curl. Copiez et collez la commande curl suivante dans le terminal.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:45153/one
        remote_1
        $
        
5.  Observez la sortie dans le journal du serveur, vous verrez quelque chose comme ci-dessous :
    
        ## one Thread[#25,nioEventLoopGroup-3-1,10,main]
        
    
    > Il s'agit d'un thread de boucle d'événement Netty. Vous pouvez bloquer un élément VirtualThread. Cela signifie que les gestionnaires de requêtes Nima peuvent utiliser un code de blocage simple, mais pas les gestionnaires réactifs.
    

## Tâche 4 : analyser la trace de pile pour une application réactive

1.  Copiez et collez la commande suivante pour forcer une exception (le nombre doit être un entier !)
    
        <copy>curl "http://localhost:<port>/parallel?count=foo"</copy>
        
    
    La sortie se présente comme suit :
    
        $ curl http://localhost:45153/parallel?count=foo
        For input string: "foo"
        $
        
2.  Consultez le journal du serveur, vous obtiendrez une sortie similaire à la suivante :
    
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
        
    
    > Comparons la trace de pile de Níma (blocage) et Helidon SE (réactif) pour le même type d'exception.
    
    *   Les traces réactives sont longues et incluent des appels provenant de gestionnaires de boucles d'événements (par exemple, Netty)
    *   Le gestionnaire de boucles d'événements peut appeler n'importe lequel des gestionnaires enregistrés par l'application.
        *   Le flux d'exécution n'est pas nécessairement propice à l'ordre du code
        *   Les gestionnaires peuvent être appelés à différents moments de l'exécution pour gérer les événements.
    *   Les traces de blocage sont faciles à comprendre en raison de la préservation de la commande
        *   Grâce au blocage des threads virtuels, les opérations sont suspendues et poursuivies ultérieurement
        *   Le flux d'exécution est simple et clair à suivre
3.  Appuyez sur _Ctrl + C_ dans le terminal où la commande \*java -jar \* est en cours d'exécution pour arrêter le serveur.
    

Vous pouvez maintenant _passer à l'exercice suivant_.

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023