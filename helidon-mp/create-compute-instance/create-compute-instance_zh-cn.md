# 在计算实例中运行 Helidon 应用原生 docker 映像

## 简介

此实验将指导您完成从 Oracle 容器映像注册表中提取 docker 映像并将其运行在 Oracle Cloud Infrastructure 中的虚拟机中的过程。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[在计算实例中运行 Helidon 应用原生 docker 映像](videohub:1_dsfd22u5)

### 目标

在本练习中，您将：

*   创建计算实例
*   在计算实例中安装 Docker
*   在计算实例中运行应用原生映像 docker 容器。

### 先备条件

要运行此实验，您必须：

*   Oracle Cloud 账户
*   具有用于创建计算实例的资源
*   容器打包的 Helidon _myproject-your\_first\_name_ 应用程序在容器注册表中可用。

## 任务 1：创建计算实例

1.  在云控制台中，单击_计算_ -> _实例_。 ![计算实例](images/compute-instance.png)
    
2.  选择正确的区间，然后单击_创建实例_。 ![创建实例](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  创建实例后，单击_复制_以复制实例的公共 IP。 ![副本 ip](images/copy-ip.png)
    

## 任务 2：在计算实例上安装 docker

1.  在代码编辑器内的终端中，运行以下命令以创建私钥。
    
        <copy>vi ~/opc.key</copy>
        
2.  按 _i_ 进入插入模式并粘贴在此实验任务 1 中下载的私钥的内容。按 _escape_ 键，然后输入 _：wq_ 以保存文件的内容。
    
3.  通过在终端中运行以下命令来更改文件的权限。
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  使用您自己的 _PUBLIC IP_ 运行以下命令以连接到刚刚创建的计算实例。
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![SSH 操作员](images/ssh-opc.png)
    
    > **在免费套餐账户中，默认情况下没有 _.ssh_ 文件夹，因此输出与屏幕截图不同。**
    
5.  运行以下命令以 root 用户身份在计算实例中安装 docker，并提供运行 docker 的 _opc_ 用户功能。
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  等待 1-2 分钟，直到计算机重新引导并通过运行以下命令重新连接到计算实例：
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## 任务 3：提取并运行计算实例中的问候应用程序

1.  运行以下命令从 Oracle 容器映像注册表提取 docker 映像。
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    您将具有如下所示的类似输出。 ![提取映像](images/docker-pull.png)
    
2.  运行以下命令以运行应用程序：
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  您可以打开新的终端和 ssh 来计算实例并运行以下命令来运行应用程序。
    
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
        
    
    祝贺您已完成 Oracle Cloud Infrastructure 中计算实例上的 Helidon 应用程序部署。
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月