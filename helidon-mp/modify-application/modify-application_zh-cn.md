# 在代码编辑器中修改 Helidon MP 应用程序

## 简介

在此实验室中，您可以在代码编辑器中将定制端点添加到 JAVA 类。

估计时间：10 分钟

观看下面的视频，快速浏览实验室。[在代码编辑器中修改 Helidon MP 应用程序](videohub:1_sv1iug41)

### 目标

在本练习中，您将：

*   在 Java 类中添加定制端点
*   构建和运行修改的应用程序

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 任务 1：添加定制端点

1.  在代码编辑器中，单击 _GreetResource.java_ 文件将其打开。 ![打开文件](images/open-file.png)
    
2.  正如您在代码中看到的，它完全基于 MicroProfile。这意味着通过 POJO 和注释实现所有功能。这些注释是标准注释，因此可跨不同供应商移植。这意味着您可以轻松地在支持相同版本的 MicroProfile 的其他平台上运行代码。要了解更多信息，请访问[此处](https://microprofile.io/)。
    
3.  创建一个新端点，并在一组五个标题中随机提供标题。要创建此端点，请在第 80 行复制并粘贴以下代码，如下所示。
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![添加代码](images/add-code.png)
    
4.  要保存文件的内容，请在代码编辑器中，单击_文件_ -> _保存_。
    
    > 默认情况下，在代码编辑器中启用 AutoSave。
    

## 任务 2：运行应用程序

1.  将以下命令复制并粘贴到终端以运行应用程序。
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  打开新的终端/控制台，然后运行以下命令来检查应用程序：
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > 它随机提供了 5 个选项的标题，您可以多次运行此命令。
    
3.  _通过在运行 "java -jar target/myproject.jar" 命令的终端中输入 `Ctrl + C` 来停止 **myproject** 应用程序_。很重要，你将在实验室里面对问题。
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月