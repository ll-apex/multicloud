# 开发 Helidon 应用

## 简介

此实验将指导您完成创建 Helidon MP 应用程序的步骤。

估计时间：15 分钟

### 关于产品/技术

Helidon 设计简单易用，可提供工具和示例来帮助您快速上手。由于 Helidon 只是在快速 Netty 核心上运行的库集合，因此没有额外的开销或 bloat。Helidon 支持 MicroProfile，并提供 JAX-RS、CDI 和 JSON-P/B 等熟悉的 API。我们的 MicroProfile 实施基于我们的快速 Helidon Reactive WebServer 运行。Reactive WebServer 提供了一个现代、功能强大的编程模型，并在 Netty 上运行。轻量级、灵活性和反应性强，Helidon WebServer 为您的微服务提供了简单易用的快速基础。

借助对运行状况检查、指标、跟踪和容错能力的支持，Helidon 可以满足您需要编写与 Prometheus、Jaeger/Zipkin 和 Kubernetes 集成的云就绪应用。

### 关于 Helidon CLI

通过 Helidon CLI，您可以从一组 Archetypes 中进行选取，从而轻松创建 Helidon 项目。它还支持执行连续编译和应用程序重新启动的开发人员循环，因此您可以轻松迭代源代码更改。

CLI 分发为独立可执行文件（使用 GraalVM 编译），以便于安装。它目前作为 Linux、Mac 和 Windows 的下载版本提供。只需下载二进制文件，即可在可从 PATH 访问的位置安装二进制文件，然后即可开始使用。

### 目标

*   安装 Helidon CLI
*   创建一个 MicroProfile 支持的微服务，称为 Helidon Greeting
*   运行并练习 Helidon 问候应用程序
*   查看运行状况和度量数据
*   向应用程序添加新功能

### 先备条件

*   Helidon 需要 Java 11+
*   Maven 3.6.x

> **注意：由于应用程序构建存在已知问题，请勿使用 3.8.x 版本！**

*   Java 和 `mvn` 位于您的路径中。
*   Windows 用户还需要可视 C++ 可再分发运行时。  
    有关更多信息，请参见 [Helidon on Windows](https://helidon.io/docs/v2/#/about/04_windows) 。

## 任务 1：安装 Helidon CLI

1.  安装 Helidon CLI
    
    对于 macOS：
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    对于 Linux：
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    对于 Windows：
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## 任务 2：创建 Helidon 问候应用程序

1.  在控制台中输入：
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > 为避免任何潜在问题，请定义针对此实验环境测试的特定 Helidon 版本。
    
2.  在本演示中，我们将创建 MicroProfile 支持的微服务，因此请为 **Helidon MP Flavor** 选择选项 **(2)** ：
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  对于大多数功能，请依次选择选项 **(2) quickstart** 和 **Enter** 以获得默认答案。请注意，您可能具有不同的默认程序包和项目组名称，因为它使用操作系统用户名。请注意程序包名称，创建新的 Java 类时需要使用它。
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > 对于 **development loop** ，现在接受缺省值 ( **n** )。您将在本练习的后面部分启动开发循环。
    
    您现在拥有一个功能完备的微服务 Maven 项目：
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## 任务 3：运行 Helidon 问候应用程序

1.  在同一控制台/终端中，导航到 quickstart-mp 目录并运行以下命令：
    
        <copy> cd quickstart-mp
        </copy>
        
2.  使用 JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### 练习应用程序

1.  打开新的终端/控制台，然后运行以下命令来检查应用程序：
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        

### 复核健康和指标数据

1.  在同一终端/控制台中，运行以下命令来检查运行状况和度量：
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  通过在运行 "java -jar target/quickstart-mp.jar" 命令的终端中输入 `Ctrl + C` 来停止 _quickstart-mp_ 应用程序。
    

## 任务 4：修改应用程序

1.  打开您最喜欢的 IDE 并导航到 **microprofile-config.properties** 文件。
    
    ![配置文件](images/config.jpg)
    
2.  在控制台/终端中，导航到项目文件夹并输入：
    
        <copy>helidon dev</copy>
        
    
    > 这将启动上一个任务中提到的**开发循环**。
    
3.  将属性 _app.greeting_ 更改为 "Hello Oracle" 并保存文件。
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > 您会看到，每当您更改文件时，**Helidon CLI** 都会识别发生更改、重新编译应用程序并重新运行。由于 Helidon 很小，一切都会很快发生。
    
4.  在控制台/终端中，输入以下内容：
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    结果应为：
    
        {"message":"Hello Oracle World!"}
        
    
    > 确保使用 `CTRL+C` 停止开发循环
    
5.  返回到项目文件夹并打开 **GreetResource.java** 文件。
    
    > 您可以看到，它是纯 MicroProfile 兼容代码：
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  创建一个新端点，为不同语言的不同问候提供帮助。要创建此新功能，请使用以下代码创建一个名为 **GreetHelpResource** 的新类：
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > 该类只有一个方法 _getAllGreetings_ ，它返回不同语言的问候列表。复制代码时，请确保在类顶部添加必需的程序包名称。
    
7.  构建和运行应用程序：
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  执行以下命令并注意结果：
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    预期的结果：
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  查看指标，您将看到出现一个新的计数器。只要调用此端点，值就会递增：
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  在控制台中，您现在将看到有关此调用的 INFO 日志行：
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    并且已添加了新端点。
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > 使用 Helidon 及其工具简单快捷！
    
11.  将您的终端/控制台保持打开状态，然后继续使用 Verrazzano 安装实验室。
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Maciej Gruszka、Ankit Pandey
*   **上次更新者/日期** - Ankit Pandey，2023 年 3 月