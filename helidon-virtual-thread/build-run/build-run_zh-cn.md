# 构建和运行 Helidon Nima 和 Reactive 应用

## 简介

此实验将指导您完成在 Oracle Cloud Infrastructure 中的 Oracle Code Editor 中构建和运行 Nima 和 Reactive Helidon 应用程序的过程。

[Lab2 演练](videohub:1_e88ydqwt)

估计时间：15 分钟

### 目标

在本练习中，您将：

*   构建、运行和测试 Helidon Nima 应用
*   构建、运行和测试 Helidon Reactive 应用
*   分析 Nima 应用程序与 Reactive 应用程序比较的简单性

### 先备条件

要运行此实验，您必须：

*   Oracle Cloud 账户
*   完成的实验 1，用于安装所需的 JDK 和 maven。

## 任务 1：构建和运行 Nima 应用程序

1.  在代码编辑器中单击 _File_ -> _Open_ 。 ![打开项目](images/open-project.png)
    
2.  选择 _helidon-levelup-2023-main_ 文件夹，然后单击 _Open（打开）_。 ![Helidon 项目](images/helidon-project.png)
    
    > 请忽略警告/问题，打开此文件夹时您会在代码编辑器中看到。
    
3.  打开新的终端，然后复制并粘贴以下命令以设置 PATH 和 JAVA\_HOME 变量。
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  复制以下命令并在终端中运行以验证是否正确配置了所需的 JDK 和 Maven 版本。
    
        <copy>mvn -v</copy>
        
    
    输出结果如下所示：
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _您将仅使用此终端构建和运行应用程序，因为它具有所需的 JDK 和 Maven 版本。_
    
5.  复制并粘贴以下命令以构建 nima 应用程序。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    您的输出如下所示：
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  在终端中复制并粘贴以下命令以运行应用程序的阻止 nima 版本：
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    您的输出如下所示：
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  写下运行服务器的端口号（请参见 @default 的日志条目）。例如，在输出中，端口号为 33193。同样，请找出服务器端口号。
    
8.  要打开新终端，请单击_终端_ -> _新建终端_。我们将使用此终端运行 _curl_ 命令。 ![新终端](images/new-terminal.png)
    
9.  将以下命令复制并粘贴到新终端。不要忘记将 _`<port>`_ 替换为服务器端口。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您的输出如下所示：
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  将以下命令复制并粘贴到新终端。不要忘记使用服务器端口替换为 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 注意结果的顺序。按照名称的建议，此请求按顺序多次调用远程资源。
    
11.  将以下命令复制并粘贴到新终端。不要忘记使用服务器端口替换为 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > 注意结果的顺序。按照名称的建议，此请求并行调用远程资源多次。
    
12.  在运行 \*java -jar \* 命令的终端中按 _Ctrl + C_ 可停止服务器。
    

## 任务 2：构建和运行反应性应用程序

1.  复制并粘贴以下命令以构建 nima 应用程序。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    您的输出如下所示：
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  在终端中复制并粘贴以下命令以运行应用程序的反应性版本：
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    您的输出如下所示：
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  写下运行服务器的端口号（请参见 @default 的日志条目）。例如，在输出中，端口号为 45765。同样，请找出服务器端口号。
    
4.  返回到在任务 1 中打开的用于运行 curl 命令的终端。
    
5.  将以下命令复制并粘贴到新终端。不要忘记使用服务器端口替换为 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  将以下命令复制并粘贴到新终端。不要忘记使用服务器端口替换为 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 注意结果的顺序。按照名称的建议，此请求按顺序多次调用远程资源。
    
7.  将以下命令复制并粘贴到新终端。不要忘记使用服务器端口替换为 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    您的输出如下所示：
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > 注意结果的顺序。按照名称的建议，此请求并行调用远程资源多次。
    
8.  在运行 \*java -jar \* 命令的终端中按 _Ctrl + C_ 可停止服务器。
    

## 任务 3：分析 Nima 应用程序的简单性

**拦截与被动**

我们来比较 Níma（阻塞）和 Helidon SE（反应性）之间的实现。

*   两个实施都使用 Helidon WebClient 执行 REST 调用
*   块实现简单易懂：
    *   执行流由语句在代码中的顺序反映
    *   没有调用复杂或语义丰富的库调用
    *   调试很简单
*   反应性版本需要深入了解反应性库
    *   包括多重、flatMap、错误处理等
    *   由于使用反应性处理程序，控制流不再明显
    *   需要了解平面映射的启用/禁止并发功能
    *   调试更加困难

1.  打开 _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ 文件以查看在应用程序的 nima 版本中如何实施端点。 ![Nima Block](images/nima-block.png)
    
2.  打开 _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ 文件，查看在应用程序的反应性版本中如何实施端点。 ![反应性服务](images/reactive-service.png)
    
3.  在 _OPEN EDITORS_ 部分中，右键单击 _BlockingService.java_ 文件并选择_选择进行比较_。 ![选择比较](images/select-compare.png)
    
4.  右键单击 _ReactiveService.java_ 文件，然后选择_与选定项比较_。 ![比较所选项](images/compare-selected.png)
    
5.  发现被动代码比阻塞（虚拟线程）更复杂 ![比较代码](images/compare-code.png)
    
    > 分别在 BlockingService 和 ReactiveService 中检查方法一、序列和并行。看看您是否了解他们的工作方式！
    

您现在可以_进入下一个实验_。

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月