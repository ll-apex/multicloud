# 在運算執行處理中執行 Helidon 應用程式原生 docker 映像檔

## 簡介

這個實驗室會逐步引導您從 Oracle Container Image Registry 提取 docker 映像檔，並在 Oracle Cloud Infrastructure 內的虛擬機器中執行該映像檔。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[在運算執行處理中執行 Helidon 應用程式原生 docker 映像檔](videohub:1_dsfd22u5)

### 目標

在此實驗室中，您將：

*   建立運算執行處理
*   在運算執行處理中安裝 Docker
*   在運算執行處理內執行應用程式原生映像檔停駐程式容器。

### 先決條件

若要執行此實驗室，您必須具備：

*   Oracle Cloud 帳戶
*   擁有建立運算執行處理的資源
*   容器登錄中提供的容器封裝 Helidon _myproject-your\_first\_name_ 應用程式。

## 作業 1：建立運算執行處理

1.  在雲端主控台中，按一下_運算_ -> _執行處理_。 ![運算執行處理](images/compute-instance.png)
    
2.  選取正確的區間，然後按一下_建立執行處理_。 ![建立執行處理](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  執行處理建立後，請按一下_複製_以複製執行處理的公用 IP。 ![複製 ip](images/copy-ip.png)
    

## 作業 2：在運算執行處理上安裝 docker

1.  在「程式碼編輯器」內的終端機中，執行下列命令以建立私密金鑰。
    
        <copy>vi ~/opc.key</copy>
        
2.  按 _i_ 進入插入模式，然後貼上私密金鑰的內容，該私密金鑰已下載在此實驗室的任務 1 中。按 _escape_ 鍵，然後輸入 _：wq_ 以儲存檔案的內容。
    
3.  在終端機中執行下列指令以變更檔案的權限。
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  請使用您自己的 _PUBLIC IP_ 執行下列命令，連線至剛建立的運算執行處理。
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![ssh 作業系統](images/ssh-opc.png)
    
    > **在 Free Tier 帳戶中，我們預設不會有 _.ssh_ 資料夾，因此您將輸出不同的螢幕截圖。**
    
5.  執行下列命令，以 root 使用者身分在運算執行處理中安裝 docker，並提供 _opc_ 使用者執行 docker 的功能。
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  請等待 1-2 分鐘，直到機器重新啟動，並執行下列命令以再次連線至運算執行處理：
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## 作業 3：提取及執行運算執行處理中的問候語 App

1.  執行下列命令，從 Oracle Container Image Registry 擷取 docker 映像檔。
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    您的輸出結果如下所示。 ![拉式影像](images/docker-pull.png)
    
2.  執行下列命令以執行應用程式：
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  您可以開啟新的終端機與 ssh 來運算執行處理，然後執行下列命令以行使應用程式。
    
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
        
    
    恭喜您，您已在 Oracle Cloud Infrastructure 的運算執行處理上完成 Helidon 應用程式部署。
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月