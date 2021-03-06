---
title: "トークン プロバイダー | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 947986cf-9946-4987-84e5-a14678d96edb
caps.latest.revision: 22
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 22
---
# トークン プロバイダー
このサンプルでは、カスタム トークン プロバイダーを実装する方法を示します。[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] のトークン プロバイダーは、資格情報をセキュリティ インフラストラクチャに提供するために使用します。一般的に、トークン プロバイダーは、ターゲットをチェックし、適切な証明書を発行して、セキュリティ インフラストラクチャがメッセージのセキュリティを保護できるようにします。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] には、既定の Credential Manager Token Provider が付属しています。また、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] には、[!INCLUDE[infocard](../../../../includes/infocard-md.md)] トークン プロバイダーも付属しています。カスタム トークン プロバイダーは、次の場合に便利です。  
  
-   トークン プロバイダーが連携動作できない資格情報ストアがある場合。  
  
-   資格情報をユーザーが詳細を提供した時点から [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] クライアント フレームワークが資格情報を使用した時点に変換するための、独自のカスタム メカニズムを提供する場合。  
  
-   カスタム トークンを構築している場合。  
  
 このサンプルでは、ユーザーの入力を別の形式に変換するカスタム トークン プロバイダーを構築する方法を示します。  
  
 このサンプルに示されている手順の概要は次のとおりです。  
  
-   クライアントがユーザー名\/パスワードの組み合わせを使用して認証する方法。  
  
-   クライアントをカスタム トークン プロバイダーを使用して構成する手順。  
  
-   サーバーが、ユーザー名とパスワードが一致していることを検証するカスタム <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> とパスワードを使用して、クライアント資格情報を検証する手順。  
  
-   サーバーがクライアントによってサーバーの X.509 証明書を使用して認証される手順。  
  
 さらにこのサンプルでは、カスタム トークン認証システムの処理後に、呼び出し元の ID にアクセスするための方法も示します。  
  
 サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは App.config 構成ファイルで定義します。エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。バインディングの構成には、標準の `wsHttpBinding` が使用されます。これは既定でメッセージ セキュリティを使用します。このサンプルは、標準の `wsHttpBinding` を設定してクライアントのユーザー名認証を使用します。また、サービスは serviceCredentials 動作を使用してサービス証明書の構成も行います。serviceCredentials 動作を使用すると、サービス証明書を構成できます。クライアントはサービス証明書を使用して、サービスを認証し、メッセージを保護します。次の構成では、次のセットアップ手順で説明しているサンプル セットアップでインストールされる localhost 証明書を参照しています。  
  
```  
<system.serviceModel>  
    <services>  
      <service   
          name="Microsoft.ServiceModel.Samples.CalculatorService"  
          behaviorConfiguration="CalculatorServiceBehavior">  
        <host>  
          <baseAddresses>  
            <!-- configure base address provided by host -->  
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service"/>  
          </baseAddresses>  
        </host>  
        <!-- use base address provided by host -->  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  bindingConfiguration="Binding1"   
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
      </service>  
    </services>  
  
    <bindings>  
      <wsHttpBinding>  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by a client to authenticate the service and provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
  
```  
  
 クライアント エンドポイント構成は、構成名、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。クライアント バインディングは、適切な `Mode` およびメッセージ `clientCredentialType` で構成されます。  
  
```  
<system.serviceModel>  
  <client>  
    <endpoint name=""  
              address="http://localhost:8000/servicemodelsamples/service"   
              binding="wsHttpBinding"   
              bindingConfiguration="Binding1"   
              contract="Microsoft.ServiceModel.Samples.ICalculator">  
    </endpoint>  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="UserName" />  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
</system.serviceModel>  
```  
  
 カスタム トークン プロバイダーを開発して [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ フレームワークに統合する方法を、次の手順に示します。  
  
1.  カスタム トークン プロバイダーを作成します。  
  
     サンプルでは、ユーザー名とパスワードを取得するカスタム トークン プロバイダーを実装します。パスワードは、このユーザー名と一致する必要があります。このカスタム トークン プロバイダーは、デモンストレーション用にのみ作成されたものです。実環境に展開することは推奨されません。  
  
     このタスクを実行するため、カスタム トークン プロバイダーは <xref:System.IdentityModel.Selectors.SecurityTokenProvider> クラスを派生し、<xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%28System.TimeSpan%29> メソッドをオーバーライドします。このメソッドは、新しい `UserNameSecurityToken` を作成して返します。  
  
    ```  
    protected override SecurityToken GetTokenCore(TimeSpan timeout)  
    {  
        // obtain username and password from the user using console window  
        string username = GetUserName();  
        string password = GetPassword();  
        Console.WriteLine("username: {0}", username);  
  
        // return new UserNameSecurityToken containing information obtained from user  
        return new UserNameSecurityToken(username, password);  
    }  
  
    ```  
  
2.  カスタム セキュリティ トークン マネージャーを作成します。  
  
     <xref:System.IdentityModel.Selectors.SecurityTokenManager> は、特定の <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> \(`CreateSecurityTokenProvider` メソッドで渡されます\) に対する <xref:System.IdentityModel.Selectors.SecurityTokenProvider> を作成するために使用されます。セキュリティ トークン マネージャーは、トークン認証システムとトークン シリアライザーの作成にも使用されますが、このサンプルでは扱っていません。このサンプルでは、カスタム セキュリティ トークン マネージャーは <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> クラスを継承し、渡されたトークンの要件でユーザー名プロバイダーが必要であることが示されている場合に、`CreateSecurityTokenProvider` メソッドをオーバーライドして、カスタムのユーザー名トークン プロバイダーを返します。  
  
    ```  
    public class MyUserNameSecurityTokenManager : ClientCredentialsSecurityTokenManager  
    {  
        MyUserNameClientCredentials myUserNameClientCredentials;  
  
        public MyUserNameSecurityTokenManager(MyUserNameClientCredentials myUserNameClientCredentials)  
            : base(myUserNameClientCredentials)  
        {  
            this.myUserNameClientCredentials = myUserNameClientCredentials;  
        }  
  
        public override SecurityTokenProvider CreateSecurityTokenProvider(SecurityTokenRequirement tokenRequirement)  
        {  
            // if token requirement matches username token return custom username token provider  
            // otherwise use base implementation  
            if (tokenRequirement.TokenType == SecurityTokenTypes.UserName)  
            {  
                return new MyUserNameTokenProvider();  
            }  
            else  
            {  
                return base.CreateSecurityTokenProvider(tokenRequirement);  
            }  
        }  
    }  
    ```  
  
3.  カスタム クライアント資格情報を作成します。  
  
     クライアント資格情報クラスは、クライアント プロキシ用に構成された資格情報を表すために使用され、トークン認証システム、トークン プロバイダー、およびトークン シリアライザーの取得に使用されるセキュリティ トークン マネージャーを作成します。  
  
    ```  
    public class MyUserNameClientCredentials : ClientCredentials  
    {  
        public MyUserNameClientCredentials()  
            : base()  
        {  
        }  
  
        protected override ClientCredentials CloneCore()  
        {  
            return new MyUserNameClientCredentials();  
        }  
  
        public override SecurityTokenManager CreateSecurityTokenManager()  
        {  
            // return custom security token manager  
            return new MyUserNameSecurityTokenManager(this);  
        }  
    }  
    ```  
  
4.  カスタム クライアント資格情報を使用するようクライアントを構成します。  
  
     クライアントがカスタム クライアント資格情報を使用するため、サンプルでは既定のクライアント資格情報を削除し、新しいクライアント資格情報クラスを提供しています。  
  
    ```  
    static void Main()  
    {  
        // ...  
           // Create a client with given client endpoint configuration  
          CalculatorClient client = new CalculatorClient();  
  
          // set new credentials  
           client.ChannelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));  
         client.ChannelFactory.Endpoint.Behaviors.Add(new MyUserNameClientCredentials());  
       // ...  
    }  
  
    ```  
  
 サービス側で呼び出し元の情報を表示するには、次のコード例に示すように <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> を使用します。<xref:System.ServiceModel.ServiceSecurityContext.Current%2A> には、現在のユーザーに関する情報が保持されています。  
  
```  
static void DisplayIdentityInformation()  
{  
    Console.WriteLine("\t\tSecurity context identity  :  {0}",   
        ServiceSecurityContext.Current.PrimaryIdentity.Name);  
}  
  
```  
  
 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。  
  
## セットアップ バッチ ファイル  
 このサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、サーバー証明書ベースのセキュリティを必要とする自己ホスト型アプリケーションを実行できるようになります。このバッチ ファイルは、複数のコンピューターを使用する場合またはホストなしの場合に応じて変更する必要があります。  
  
 次に、バッチ ファイルのセクションのうち、該当する構成で実行するために変更が必要となる部分を示します。  
  
-   サーバー証明書の作成。  
  
     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。`%SERVER_NAME%` 変数はユーザー名を指定します。この変数を変更して、使用するサーバー名を指定します。このバッチ ファイルでの既定値は localhost です。  
  
    ```  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
-   クライアントの信頼された証明書ストアへのサーバー証明書のインストール。  
  
     Setup.bat バッチ ファイルの次の行は、サーバー証明書をクライアントの信頼されたユーザーのストアにコピーします。この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないためです。マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
  
    ```  
  
> [!NOTE]
>  Setup.bat バッチ ファイルは、Windows SDK コマンド プロンプトから実行します。MSSDK 環境変数が SDK のインストール ディレクトリを指している必要があります。この環境変数は、Windows SDK コマンド プロンプトで自動設定されます。  
  
#### サンプルをセットアップしてビルドするには  
  
1.  「[Windows Communication Foundation サンプルの 1 回限りのセットアップの手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)」が実行済みであることを確認します。  
  
2.  ソリューションをビルドするには、「[Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
#### サンプルを同じコンピューターで実行するには  
  
1.  管理特権で [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプトを開き、サンプルのインストール フォルダーから Setup.bat を実行します。これにより、サンプルの実行に必要なすべての証明書がインストールされます。  
  
    > [!NOTE]
    >  Setup.bat バッチ ファイルは、[!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプトから実行します。[!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプト内で設定された PATH 環境変数は、Setup.bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指しています。  
  
2.  service.exe を \\service\\bin で起動します。  
  
3.  Client.exe を \\client\\bin で起動します。クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
4.  username プロンプトに対してユーザー名を入力します。  
  
5.  password プロンプトに対して、username プロンプトで入力したものと同じ文字列を入力します。  
  
6.  クライアントとサービス間で通信できない場合は、「[Troubleshooting Tips](http://msdn.microsoft.com/ja-jp/8787c877-5e96-42da-8214-fa737a38f10b)」を参照してください。  
  
#### サンプルを複数のコンピューターで実行するには  
  
1.  サービス コンピューターにサービス バイナリ用のディレクトリを作成します。  
  
2.  サービス プログラム ファイルを、サービス コンピューターのサービス ディレクトリにコピーします。Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
3.  コンピューターの完全修飾ドメイン名を含むサブジェクト名を持つサーバー証明書が必要です。新しい証明書名を反映するには、Service.exe.config ファイルを更新する必要があります。サーバー証明書を作成するには、Setup.bat バッチ ファイルを変更します。setup.bat ファイルは、管理特権を使用して開いた Visual Studio コマンド プロンプトから実行する必要があります。`%SERVER_NAME%` 変数には、サービスをホストするコンピューターの完全修飾ホスト名を設定する必要があります。  
  
4.  サーバー証明書をクライアントの CurrentUser\-TrustedPeople ストアにコピーします。この操作は、サーバー証明書の発行元をクライアントが信頼できる場合は不要です。  
  
5.  サービス コンピューターの Service.exe.config ファイルで、ベース アドレスの値を localhost から完全修飾コンピューター名に変更します。  
  
6.  サービス コンピューターで、コマンド プロンプトから service.exe を実行します。  
  
7.  クライアント プログラム ファイルを、言語固有のフォルダーにある \\client\\bin\\ フォルダーからクライアント コンピューターにコピーします。  
  
8.  クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。  
  
9. クライアント コンピューターで、コマンド プロンプト ウィンドウから `Client.exe` を起動します。  
  
10. クライアントとサービス間で通信できない場合は、「[Troubleshooting Tips](http://msdn.microsoft.com/ja-jp/8787c877-5e96-42da-8214-fa737a38f10b)」を参照してください。  
  
#### サンプルの実行後にクリーンアップするには  
  
1.  サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
  
## 参照