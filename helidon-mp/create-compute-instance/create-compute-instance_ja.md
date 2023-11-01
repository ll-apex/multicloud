# コンピュート・インスタンスでのHelidonアプリケーション・ネイティブdockerイメージの実行

## 概要

このラボでは、Oracle Container Image Registryからdockerイメージをプルし、Oracle Cloud Infrastructure内の仮想マシンで実行するプロセスについて説明します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[コンピュート・インスタンスでのHelidonアプリケーション・ネイティブdockerイメージの実行](videohub:1_dsfd22u5)

### 目的

この演習では、次のことを行います。

*   コンピュート・インスタンスの作成
*   コンピュート・インスタンスへのDockerのインストール
*   コンピュート・インスタンス内でアプリケーション・ネイティブ・イメージdockerコンテナを実行します。

### 事前設定

この演習を実行するには、次のことが必要です。

*   Oracle Cloudアカウント
*   コンピュート・インスタンスを作成するためのリソースがあること
*   コンテナ・レジストリで使用可能なコンテナ・パッケージHelidon _myproject-your\_first\_name_アプリケーション。

## タスク1: コンピュート・インスタンスの作成

1.  クラウド・コンソールで、_「コンピュート」_→_「インスタンス」_をクリックします。 ![コンピュート・インスタンス](images/compute-instance.png)
    
2.  正しいコンパートメントを選択し、_「インスタンスの作成」_をクリックします。 ![インスタンスの作成](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  インスタンスが作成されたら、_「コピー」_をクリックしてインスタンスのパブリックIPをコピーします。 ![ipのコピー](images/copy-ip.png)
    

## タスク2: コンピュート・インスタンスへのdockerのインストール

1.  コード・エディタ内のターミナルで、次のコマンドを実行して秘密キーを作成します。
    
        <copy>vi ~/opc.key</copy>
        
2.  _i_を押して挿入モードに入り、この演習のタスク1でダウンロードした秘密キーの内容を貼り付けます。_escape_キーを押してから、_:wq_と入力してファイルの内容を保存します。
    
3.  端末で次のコマンドを実行して、ファイルの権限を変更します。
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  作成したコンピュート・インスタンスに接続するには、独自の_PUBLIC IP_を指定して次のコマンドを実行します。
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![ssh opc](images/ssh-opc.png)
    
    > **Free層アカウントでは、デフォルトでは_.ssh_フォルダがないため、画面ショットとは異なる出力が表示されます。**
    
5.  次のコマンドを実行して、dockerをrootユーザーとしてコンピュート・インスタンスにインストールし、dockerを実行する_opc_ユーザー機能を提供します。
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  次のコマンドを実行して、マシンが再起動してコンピュート・インスタンスに再度接続するまで、1から2分待ちます。
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## タスク3: コンピュート・インスタンスでのGreetingアプリケーションのプルおよび実行

1.  次のコマンドを実行して、Oracle Container Image Registryからdockerイメージをプルします。
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    次のように出力されます。 ![イメージのプル](images/docker-pull.png)
    
2.  次のコマンドを実行して、アプリケーションを実行します:
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  新しいターミナルおよびsshを開いてインスタンスを計算し、次のコマンドを実行してアプリケーションを実行できます。
    
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
        
    
    Oracle Cloud Infrastructureのコンピュート・インスタンスでのHelidonアプリケーション・デプロイメントが完了しました。
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月