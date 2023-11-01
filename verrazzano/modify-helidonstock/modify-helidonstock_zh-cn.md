# 修改 Bobby 的工作簿应用程序并创建新的应用程序组件图像

## 简介

在本练习中，我们将通过 _Cloud Shell_ 修改 bobbys-helidon-stock-application。稍后，我们将为 bobbys-helidon-stock-application 创建新的 Docker 映像。此 bobbys-helidon-stock-application 映像是 Bobby 的 Books 应用程序的组件。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   修改 bobbys-helidon-stock-application。
*   为 bobbys-helidon-stock-application 创建新的 Docker 映像。

### 先备条件

您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：修改 bobbys-helidon-stock-application

1.  选择 Bobby 的 "Book" 选项卡，然后单击 _Books_ ，然后单击 _The Hobbit_ 书籍的图像，如下所示：
    
    ![单击工作簿](images/clickbooks.png " ")
    
    它以 _The Hobbit_ 格式显示工作簿名称，如图中所示。
    
    ![霍比特](images/thehobbit.png " ")
    
2.  我们希望将书籍名称转换为大写字母 (THE HOBBIT)。我们需要下载 Bobby 的 Books 应用程序的源代码。确保位于主文件夹中。复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![复制资源库](images/clonerepository.png " ")
    
3.  要查看 Bobby 的 Book 应用程序内的文件，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    输出应类似于以下内容：`bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  现在，我们将对相关的 JAVA\_FILE 进行更改。要打开文件，请复制以下命令并将其粘贴到_云 Shell_ 中。
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![打开文件](images/openfile.png " ")
    
5.  按 _i_ ，以便可以修改代码。要在行号 84 处添加新行，请复制以下行并将其粘贴在行号 84 处，如下所示：
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![插入行](images/insertline.png " ")
    
6.  按 _Esc_ ，然后按 _：wq_ 保存更改。
    
    ![保存更改](images/savechanges.png " ")
    

## 任务 2：为 bobbys-helidon-stock-application 创建新的 Docker 映像

1.  由于我们将为 _bobbys-helidon-stock-application_ 构建新的 Docker 映像，该映像依赖于 _bobbys-coherence-application_ ，因此我们运行 _Maven_ 命令来清除现有的 bobbys-coherence-application 归档文件，并在本地 Maven 系统信息库中编译、构建、打包和安装新的 bobby-coherence-application 归档文件。要更改为 _bobbys-coherence_ 目录文件夹，以及将 _bobbys-coherence_ 应用程序档案编译、构建和安装到本地 Maven 系统信息库中，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    生成的输出类似于以下内容（缩写以仅显示输出开始和结束）：`bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  由于我们修改了 _bobbys-helidon-stock-application_ ，因此我们需要编译、构建和打包此应用程序。要更改为 _bobbys-helidon-stock-application_ 目录并将 _bobbys-helidon-stock-application_ 打包到 JAR 文件中，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。在第二个映像中，可以在 `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target` 中看到 `bobbys-helidon-stock-application.jar` 文件的创建。
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  我们将创建用于 bobby-stock-helidon-application 的 Docker 映像，但此应用程序使用特定版本的 JDK，我们不想更改用于构建新映像的 Docker 文件。因此，我们下载所需的 JDK。要下载所需的 JDK 版本，请复制下面的命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    输出应类似于以下内容：\`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz 正在解析 download.java.net (download.java.net)... 2.23.220.73 正在连接到 download.java.net (download.java.net)|2.23.220.73|：443...。HTTP 请求已发送，正在等待响应 ... 200 OK 长度：198606200 (189M) \[application/x-gzip\] 保存到：'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  我们正在创建 Docker 映像，此映像将上载到实验室 6 中的 Oracle Cloud 容器注册表。要创建文件名，需要以下信息：
    
    *   租户名称空间
    *   区域端点
5.  要查找租户的名称空间，请单击_用户_图标 -> _租户_，如下所示。在**对象存储设置**中，您将找到名称空间。复制并将其保存在您的文本文件中的某个位置，因为我们也将在 Lab 6 中使用它。
    
    ![复制租户名称空间](images/copy-tenancynamespace.png " ")
    
6.  找到_区域的端点_。请参阅此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 中记录的表。在所示的示例中，区域的端点是 _UK South（伦敦）_（作为区域名称），其端点是 _lhr.ocir.io_ 。找到您自己的_区域名称_的端点并将其保存在文本文件中。下一个实验室还需要它。
    
    ![端点](images/end-point.png " ")
    
    > 现在，您的区域同时具有租户名称空间和端点。
    
7.  现在，您的区域同时具有租户名称空间和端点。复制以下命令并将其粘贴到文本编辑器中。然后，将 **`END_POINT_OF_YOUR_REGION`** 替换为区域名称的端点，将 **`NAMESPACE_OF_YOUR_TENANCY`** 替换为租户的名称空间，将 **`your_first_name`** 替换为您的名字（小写）。
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Docker 构建](images/docker-build.png " ")
    
    ![创建的图像](images/image-created.png " ")
    
    > 例如，在我的情况下，命令是 `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`。
    
    这将创建 Docker 映像，我们将此映像推送到实验室 6 中的 Oracle Cloud 容器注册表资料档案库。您需要在文本 editor.In Lab 6 中复制替换的完整映像名称 **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** ，在需要创建资料档案库时，需要为其指定名称 **`helidon-stock-application-your_first_name`** 。
    
    将 _Cloud Shell_ 保持打开状态；我们需要在下一个实验中使用它。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月