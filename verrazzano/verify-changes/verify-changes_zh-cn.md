# 通过应用程序访问和精选堆栈验证更改

## 简介

在实验室 7 中，我们在 bobbys-helidon-stock-application 中应用了更改，并且其云池处于_正在运行_状态。在本练习中，我们将验证应用程序、Verrazzano 控制台和 Grafana 控制台中的更改。

估计时间：05 分钟

### 目标

在本练习中，您将：

*   验证 Bobby 的 Books 应用程序中的更改。
*   验证 Grafana 控制台中的更改。

### 先备条件

*   您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：验证 Bobby 工作簿应用程序中的更改

1.  打开 Bobby 的“Books（工作簿）”选项卡，然后选择“Refresh（刷新）”。如果已关闭该选项卡，则在文本编辑器中复制并粘贴以下命令，并将该应用程序的 XX.XX.XX.XX 替换为 EXTERNAL\_IP。您会注意到，帐簿名称全是大写字母。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![鲍比的书](images/bobbysbooks.png " ")
    

## 任务 2：验证 Grafana 控制台中的更改

1.  返回 Verrazzano 主页。
    
    ![Verrazzano 主目录](images/verrazzao-home.png " ")
    
2.  选择 Grafana 的链接以打开 _Grafana 控制台_。
    
    ![Grafana 链接](images/grafana-link.png " ")
    
3.  选择_主页_，键入 _Helidon_ ，然后选择 _Helidon 监视仪表盘_。
    
    ![搜索 Helidon](images/search-helidon.png " ")
    
4.  在 ServiceID 中，选择 _bobs-books\_default\_bobs-books\_bobby-helidon_ ，然后在实例中选择新创建的实例。在这种情况下，您将获得修改后的 _bobby-helidon-stock-application_ 的信息。
    
    ![新建组件](images/new-component.png " ")
    
    祝贺您！您已成功完成练习。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月