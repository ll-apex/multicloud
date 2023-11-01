# 创建 Helidon 3 应用程序，然后将其迁移到 Helidon 4

## 简介

在本练习中，您将从基于 Netty 在原始反应性 Web 服务器上运行的 Helidon 3 应用程序开始。然后，您将使用虚拟线程将应用程序迁移到在新 Helidon Nima WebServer 上运行的 Helidon 4。

[Lab4 演练](videohub:1_zr1m00ba)

估计时间：20 分钟

### 目标

*   使用 Helidon Starter 生成、构建和运行 Helidon MicroProfile 应用程序。
*   将 Helidon 3 Microprofile 应用程序迁移到 Helidon 4

### 先备条件

*   Oracle Cloud 账户

## 任务 1：创建 Helidon 3 应用程序并构建应用程序

1.  复制下面的 URL 并将其粘贴到浏览器中以打开“Helidon 项目”页。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  在“生成项目”下，选择 _Helidon MP_ 作为 Helidon 风格，然后单击_下一步_。
    
3.  对于“应用程序类型”，选择_快速启动_，然后单击_下一步_。
    
4.  对于 "Media Support"，选择 _Jackson_ ，然后单击 _Next_ 。
    
5.  对于“定制项目”，选择默认值并单击_下载_。这将弹出窗口，将此 _myproject.zip_ 保存到您选择的位置。在此研讨会的其余部分中，将使用 _myproject_ 名称。如果您选择其他名称，请分别更改。
    
6.  返回到代码编辑器，在 HELIDON-LEVELUP-2023-MAIN 中，然后单击_文档_。 ![选择文档](images/select-docs.png)
    
7.  单击_文件_ -> _上载文件_，然后从先前保存此文件的位置选择 _myproject.zip_ ，然后单击_打开_。 ![加载文件](images/upload-files.png)
    
8.  您将在 _docs_ 文件夹下看到 _myproject.zip_ 文件。 ![Zip 文件](images/zip-file.png)
    
9.  复制并粘贴以下命令以解压缩文件。
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  在 myproject 文件夹中，运行以下命令来构建项目。请使用终端，在其中设置了 PATH 和 JAVA\_HOME 变量。
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > 执行此命令后应看到 _BUILD SUCCESS_ 。
    
11.  将以下命令复制并粘贴到终端，以运行此应用程序。您将看到类似于下方屏幕截图中所示的输出。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    将看到类似如下的输出：
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  返回终端，从其中运行 curl 命令并运行以下命令来检查应用程序：
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  通过在运行 "java -jar target/myproject.jar" 命令的终端中输入 `Ctrl + C` 来停止 _myproject_ 应用程序。
    
14.  编辑文件 _src/main/java/com/example/myproject/GreetResource.java_ ，查找方法 _createResponse(String Who)_ 并添加以下行，如屏幕截图中所示。
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![修改应用程序](images/modify-application.png)
    
15.  按步骤 10、11 和 12 中所述重建、运行和练习应用程序。
    
16.  查看启动服务器的终端中的服务器输出。请注意，线程名为 helidon-server-n。这是由 Helidon 创建的用于处理 JAX-RS 请求的线程池中的传统平台线程。服务器输出如下所示：
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  通过在运行 "java -jar target/myproject.jar" 命令的终端中输入 `Ctrl + C` 来停止 _myproject_ 应用程序。
    

## 任务 2：将 Helidom MicroProfile 应用程序迁移到 Helidon 4

1.  对于 myproject，打开 _pom.xml_ 文件并将父 pom 从 _3.1.1_ 更改为 _4.0.0-ALPHA5_ 。
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![Pom 版本](images/pom-version.png)
    
2.  编辑 src/main/resources/logging.properties 并将 _io.helidon.common.HelidonConsoleHandler_ 更改为 _io.helidon.logging.jul.HelidonConsoleHandler_ 。 ![编辑日志记录](images/edit-logging.png)
    
3.  您的应用程序现已迁移到 Helidon 4！复制并粘贴以下命令以构建应用程序。
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  复制并粘贴以下命令以运行应用程序。
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    输出结果如下。
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  查看服务器输出，请注意线程现在是 VirtualThread。
    
6.  通过在运行 "java --enable-preview -jar target/myproject.jar" 命令的终端中输入 `Ctrl + C` 来停止 _myproject_ 应用程序。
    

恭喜，您已完成 Helidon 虚拟线程研讨会。

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月