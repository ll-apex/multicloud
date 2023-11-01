# 生成 Helidon MP 应用程序

## 简介

此实验将指导您完成使用 **Helidon CLI** 创建 **Helidon MP** 应用程序的步骤。此外，您还将修改 Helidon 应用程序以显示其与 **OCI 日志记录和监视**服务的集成。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[生成 Helidon MP 应用程序](videohub:1_i4j33ed4)

### 目标

在本练习中，您将：

*   下载 Helidon CLI
*   使用 Helidon CLI 生成 Helidon MP 应用程序
*   与 OCI 日志记录和度量服务集成
*   将应用程序代码推送到 OCI 代码资料档案库
*   触发 DevOps 管道

### 先备条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户
*   熟悉 git 命令

## 任务 1：下载 Helidon CLI

1.  在 **OCI 代码编辑器**中打开新终端，然后粘贴以下命令以导航到主文件夹。
    
        <copy>cd ~</copy>
        
2.  复制并粘贴以下命令以下载 **Helidon CLI** 并更改**权限**以使其可执行。
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## 任务 2：使用 Helidon CLI 生成 Helidon Microprofile 应用程序

1.  执行 CLI 以**生成 Helidon Microprofile 应用程序**项目。
    
        <copy>./helidon init</copy>
        
2.  当提示输入 _Helidon 版本_时，输入 _4_ 以查看所有版本，然后输入 _35_ 以选择 _4.0.0-ALPHA6_ 版本，如下所示。
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  当提示_选择风格_时，将以下值复制并粘贴到终端。
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  当提示_选择应用程序类型_时，将以下值复制并粘贴到终端。
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  当提示输入 _Project groupId_ 、 _Project artifactId_ 和 _Project version_ 时，仅**接受默认值**。
    
6.  提示输入 _Java 程序包名称_时，将以下值复制并粘贴到终端。
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  承诺执行_启动开发循环？（默认值：n）：_时，按 _Enter_ 键选择默认值。
    
    > 完成后，将生成 **oci-mp** 项目。
    

## 任务 3：修改用于日志记录和度量浏览器的 Helidon 应用程序

1.  要在_代码编辑器_中打开 **oci-mp** 项目，请单击_文件_ -> _打开_。 ![打开项目](images/open-project.png)
    
2.  单击_上箭头_以导航到父文件夹，然后选择 _oci-mp_ 文件夹并单击_打开_。 ![打开 oci mp](images/open-ocimp.png)
    
    > 这将打开 Explorer 中的 _oci-mp_ 应用程序。
    
3.  打开新终端，将以下命令复制并粘贴到 **`devops_helidon_to_instance_ocw_hol`** 文件夹中的_复制构建和部署管道规范_。
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  添加 **.gitignore** ，以便 git 将忽略不需要作为系统信息库一部分的文件和目录。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  复制并粘贴以下命令以从主文件夹 _`devops_helidon_to_instance_ocw_hol`_ 运行实用程序脚本以更新 **config** 参数。当它要求**输入 Helidon MP 项目的根目录**时，按 Enter 键选择 **default** 值。
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    输出如下所示： ![更新配置](images/update-config.png)
    
    > **请读取：-**
    
    *   调用此脚本将执行以下操作：
    *   _~/oci-mp/server/src/main/resources/application.yaml_ 配置文件中的更新可设置 Helidon 功能，该功能将 Helidon 生成的度量发送到 OCI 监视服务。
        *   **compartmentId** - 用于此演示的区间 OCID
        *   **namespace** - 此字符串可以是任何字符串，但对于此演示，此字符串将设置为 helidon\_metrics。
    *   在 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 配置文件中进行更新，以设置 Helidon 应用程序代码使用的配置参数，以执行与 OCI 日志记录和度量服务的集成。
        *   **oci.monitoring.compartmentId** - 用于此演示的区间 OCID
        *   **oci.monitoring.namespace** - 这可以是任何字符串，但对于此演示，这将设置为 helidon\_application。
        *   **oci.logging.id** - 由 Terraform 脚本预配的应用程序日志 ID。
    *   在 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 配置文件中进行更新，以设置 _oci.bucket.name_ 属性，以包含由 terraform 脚本预配的对象存储桶名称。之后的练习将使用该名称来演示 Helidon 应用程序的对象存储支持。
6.  在代码编辑器中打开文件 _~/oci-mp/server/src/main/resources/application.yaml_ 以验证是否更新了 **compartmentId** 和 **namespace** 的值。 ![应用产品系列](images/application-yaml.png)
    
7.  在代码编辑器中打开文件 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ ，以验证是否更新了 **oci.monitoring.compartmentId** 、**oci.monitoring.namespace** 、**oci.logging.id** 和 **oci.bucket.name** 的值。_oci.bucket.name_ 将在实验室 5 中稍后使用。
    

## 任务 4：生成验证令牌以将代码推送到 OCI 代码资料档案库

在此步骤中，我们将生成一个_验证令牌_，我们将使用该令牌将 Helidon 应用程序代码推送到 _OCI 代码资料档案库_。

1.  选择右上角的_用户图标_，然后选择_我的概要信息_。 ![用户图标](images/user-icon.png)
    
2.  向下滚动并选择 _Auth Tokens_ 。 ![验证令牌](images/auth-tokens.png)
    
3.  单击_生成标记_。 ![生成标记](images/generate-token.png)
    
4.  复制 _oci-mp_ 并将其粘贴到“说明”框中，然后单击_生成令牌_。 ![标记说明](images/token-description.png)
    
5.  在“生成的令牌”下选择**复制**，然后使用所选的编辑器将它粘贴/保存到文本文件中。请记住，以后无法检索令牌，因此现在保留此令牌的副本很重要。完成后，单击**关闭**。 ![复制标记](images/copy-token.png)
    

## 任务 5：将 Helidon 应用程序项目同步到 DevOps 项目中的 OCI 代码资料档案库

1.  打开新终端，复制并粘贴以下命令以导航到 _oci-mp_ 目录。
    
        <copy>cd ~/oci-mp</copy>
        
2.  初始化 _oci-mp_ 项目目录以成为 **git 系统信息库**。
    
        <copy>git init</copy>
        
3.  将分支设置为 **main** 以匹配对应的远程分支。
    
        <copy>git checkout -b main</copy>
        
4.  设置**远程系统信息库**。使用上次 terraform 输出中显示的 OCI 代码资料档案库的 https URL，或使用 `devops_helidon_to_instance_ocw_hol` 中的 get.sh 工具检索该值。
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v** 用于验证源是否已设置。
    
5.  配置 git 以使用 **credential helper** 存储，以便 OCI 资料档案库的用户名和密码仅在需要它们的 git 命令上输入一次。此外，设置 git 提交所需的 **user.name** 和 **user.email** 。
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  使用 **git 提取**将 oci 系统信息库的 git 日志与本地系统信息库同步。
    
        <copy>git pull origin main</copy>
        
7.  这将提示输入用户名和口令。对于密码，请使用 **tenancy name**/**username** 作为用户名，使用在**任务 4** 生成的 OCI 用户 **auth token** 。![git 用户名](images/git-username.png) ![git 密码](images/git-password.png)
    
8.  输出类似于以下内容。如果没有，请检查是否正确运行了每个 git 命令。 ![git 拉动同步](images/git-pull-sync.png)
    

## 任务 6：推送 Helidon 应用程序代码并触发 DevOps 管道

1.  **暂存** Git 提交的所有文件。
    
        <copy>git add .
        git status</copy>
        
    
    > git 状态将输出系统信息库中的所有文件。
    
2.  执行第一个 **commit** 。
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  **推送**对远程系统信息库所做的更改。
    
        <copy>git push -u origin main</copy>
        
    
    > 这将触发 DevOps 启动构建管道。
    
4.  在新选项卡中打开[云控制台](https://cloud.oracle.com/)，单击 _Hamburger（汉堡）菜单_ -> _Developer Services（开发人员服务）_ -> _Projects（项目）_，位于 **DevOps** 下。 ![devops 项目](images/devops-project.png)
    
5.  选择在**练习 1** 中创建的区间，然后单击 _devops-project-helidon-ocw-hol-string_ 打开 **DevOps 项目**。 ![选择区间](images/select-compartment.png)
    
    > 刷新浏览器，如果看不到新区间。
    
6.  在_最新构建历史记录_下，您将看到_运行_和状态为_已接受/正在进行_。单击最新的运行，如下所示。 ![构建历史记录](images/build-history.png)
    
7.  构建管道完成全部三个阶段后，您将看到如下所示的输出。您可以在各个阶段之前单击箭头，以查看正在执行的操作。此操作在 _oci-mp_ 文件夹中的 _`build_spec.yaml`_ 文件中进行了定义。 ![首先构建](images/build-run-first.png)
    
8.  在构建运行过程中，在第三阶段，单击**三个点**，然后单击**查看部署**，如下所示。这将打开**部署管道**。 ![查看部署](images/view-deployment.png)
    
9.  在此处可以查看_部署进度_。完成部署管道后，您将看到如下所示的输出。您可以在阶段之前单击箭头，以查看正在执行的操作。此操作在 _oci-mp_ 文件夹中的 _`deployment_spec.yaml`_ 文件中进行了定义。 ![部署运行](images/deployment-run.png)
    
    > 这已成功将 Helidon 应用程序部署到 OCI 中的**计算实例**。
    
10.  要查看部署管道的日志，请单击靠近部署阶段的**三个点**，然后单击**查看详细信息**，如下所示。 ![查看日志](images/view-logs.png)
    
11.  向下滚动日志并验证 JDK 风格是否为 **Open JDK** 。另请注意，在日志中，Helidon 应用程序使用 Java 中的新**虚拟线程**功能，如下所示。 ![打开 jdk](images/open-jdk.png)
    
    > 作为**实验 4** 的一部分，我们将 **Open JDK** 替换为 **Oracle JDK** 。
    

您现在可以**继续下一个实验。**

## 了解详细信息

*   [Helidon CLI](https://helidon.io/docs/v3/#/about/cli)
*   [Helidon MP 快速启动指南](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Helidon MP 配置源](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Helidon MP 配置源](https://helidon.io/docs/v3/#/mp/guides/config)

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月