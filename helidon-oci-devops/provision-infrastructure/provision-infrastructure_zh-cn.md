# 提供基础设施

## 简介

在本练习中，您将创建**区间**、**动态组**、**用户组和策略**。然后，您将使用 **OCI 代码编辑器**中的 Terraform 服务创建 **DevOps 项目**及其相关资源。

估计时间：10 分钟

观看下面的视频，快速浏览实验室。[预配基础设施](videohub:1_tnhjvsxr)

### 目标

在本练习中，您将：

*   打开代码编辑器以下载 Terraform 脚本。
*   预配区间、动态组、用户组和策略。
*   预配 DevOps 项目所需的资源

### 先备条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户
*   熟悉区间、动态组、用户组和策略

## 任务 1：打开代码编辑器并下载源代码

1.  在云控制台中，单击所示的_开发人员工具_图标，然后单击_代码编辑器_。 ![打开代码编辑器](images/open-codeeditor.png)
    
2.  单击 _Terminal（终端）_ \-> _New Terminal（新建终端）_以打开终端。在研讨会期间，系统将要求您打开一个新终端。通过这种方式，您可以在代码编辑器中打开一个新终端。 ![打开终端](images/open-terminal.png)
    
3.  将以下命令复制并粘贴到终端以下载源代码。此源代码包含 terraform 脚本，这些脚本创建此研讨会所需的 OCI 资源。
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![下载源代码](images/download-sourcecode.png)

4.  要在_代码编辑器_中打开源代码 **`devops_helidon_to_instance_ocw_hol`** ，请单击_文件_ \-> _打开_。 ![开源代码](images/open-sourcecode.png)
    
5.  在主目录中选择 _`devops_helidon_to_instance_ocw_hol`_ ，然后单击_打开_。 ![开放的开发者](images/open-devops.png)
    
6.  单击 _`devops_helidon_to_instance_ocw_hol`_ 文件夹中的文件名 _terraform.tfvars_ ，如下所示。您可以看到，我们有变量 **`tenancy_ocid`** 、 **region** 、**`compartment_ocid`** 、**`user_ocid`** ，我们将为您的试用云账户定制这些变量。 ![打开的 tfvars](images/open-tfvars.png)
    

> 如果看不到该项目。您可能需要单击代码编辑器中的_浏览器_图标。 ![怪物](images/explorer.png)

7.  在浏览器中，打开**云控制台**的[新选项卡](https://cloud.oracle.com/)。我们将使用此选项卡获取上述变量的值。
    
8.  要获取 **`tenancy_ocid`** ，请单击_用户图标_，然后单击_租户_，如下所示。 ![获取租户 OCID](images/get-tenancyocid.png)
    
9.  单击_复制_可复制租户的 **OCID** 并将其粘贴到 _terraform.tfvars_ 文件中作为 _`tenancy_ocid`_ 的值。 ![复制租户 ocid](images/copy-tenancyocid.png)
    
10.  要获取 **`user_ocid`** ，请单击_用户图标_，然后单击_我的概要信息_，如下所示。 ![我的概要信息](images/my-profile.png)
    
11.  单击_复制_以复制用户的 **OCID** 并将其粘贴到 _terraform.tfvars_ 文件中作为 _`user_ocid`_ 的值。 ![复制用户 OCID](images/copy-userocid.png)
    
12.  要查找区域名称，请单击**管理区域**，如下所示。然后复制主区域的**区域标识符**，并将其粘贴到 _terraform.tfvars_ 文件中作为 _region_ 的值。![管理区域](images/manage-region.png) ![区域名称](images/region-name.png)
    
13.  最后，_terraform.tfvars_ 应如下所示。保留 _`compartment_ocid`_ 的值不变。将在作为任务 2 的一部分创建区间后，我们将替换该值。 ![初始化 tfvars](images/init-tfvars.png)
    

## 任务 2：创建区间、动态组、用户组和策略

此任务的目标是通过创建区间、动态组、用户组和策略为 DevOps 设置准备环境。此部分需要具有管理员权限的用户。如果您没有此权限，请确保请求其他具有此权限的用户为您运行此权限。

1.  打开终端，复制并粘贴以下命令以导航到 _init_ 文件夹。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  在代码编辑器中，可以查看 _init_ 文件夹中的各种文件。这些是创建区间、动态组、用户组和策略的 terraform 脚本。 ![初始化文件](images/init-files.png)
    
3.  复制并粘贴以下命令来预配区间、动态组、用户组和策略。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    您将看到类似于下文的输出。请**观察输出**以了解 terraform 脚本创建的内容。此外，还可以参考代码来查看其实现。 ![已创建初始化](images/init-created.png)
    
    > 如果存在任何错误，请检查以确保在 **terraform.tfvars** 文件中正确设置了值。
    

## 任务 3：创建 DevOps 项目及其资源

1.  在终端中，复制并粘贴以下命令以导航到 _main_ 文件夹。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  复制并粘贴以下命令以更新 **`devops_helidon_to_instance_ocw_hol`** 文件夹内 _terraform.tfvars_ 中新创建的区间的 _compartment\_ocid_ 。
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  在终端中复制并粘贴以下命令以预配所有 DevOps 资源。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **请阅读：**这将提供 DevOps 所需的以下资源：
    
    *   **OCI DevOps 服务**
        *   **OCI DevOps Project** ，其中包含此项目所需的所有 DevOps 组件。
        *   将托管应用程序源代码项目的 **OCI 代码资料档案库**。
        *   具有以下阶段的 **DevOps 构建管道**：
            *   **管理构建** - 执行下载 JDK20、maven 和构建 Helidon 应用程序的步骤
            *   **传送构件** - 将构建的 Helidon 应用程序和部署上载到构件资料档案库
            *   **触发部署** - 触发部署管道
        *   **DevOps 部署管道**，将在目标环境中执行以下操作：
            *   下载 JDK20
            *   安装 OCI CLI 并使用它下载应用程序可交付项
            *   运行应用程序
        *   **DevOps 实例组环境**，部署管道将使用该环境将创建的 OCI 计算实例标识为部署目标。
        *   **DevOps 触发器**，将在 OCI 代码资料档案库上发生推送事件时从开始到结束调用管道生命周期。
    *   **OCI 构件注册表**
        *   **OCI Artifact 资料档案库**，它将托管已构建的 Helidon 应用程序二进制文件和部署清单作为版本化构件。
    *   **OCI 平台**
        *   从防火墙打开端口 8080 的 **OCI 计算实例**。最终将在此处部署应用程序。
    *   包含打开端口 8080 的入站的 **OCI 虚拟云网络 (VCN) 及安全列表**。端口 8080 是从中访问 Helidon 应用程序的地方。OCI 计算实例将使用 OCI VCN 来满足其网络需求。
4.  下图描述了 DevOps 设置的工作原理： ![开发运维图](images/devops-diagram.png)
    
5.  您将获得如下所示的类似输出。 ![tf 输出](images/tf-output.png)
    

您现在可以**继续下一个实验。**

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月