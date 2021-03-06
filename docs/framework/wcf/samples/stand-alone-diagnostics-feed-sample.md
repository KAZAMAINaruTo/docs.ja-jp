---
title: "スタンドアロン診断フィードのサンプル | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d31c6c1f-292c-4d95-8e23-ed8565970ea5
caps.latest.revision: 26
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 26
---
# スタンドアロン診断フィードのサンプル
このサンプルでは、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] を使用して配信用の RSS フィードおよび Atom フィードを作成する方法を示します。  このサンプルは基本の "Hello World" プログラムであり、オブジェクト モデルの基本とオブジェクト モデルを [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] サービスにセットアップする方法を示しています。  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、特殊なデータ型 \(<xref:System.ServiceModel.Syndication.SyndicationFeedFormatter>\) を返すサービス操作として配信フィードをモデル化します。  <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> のインスタンスは、フィードを RSS 2.0 形式および Atom 1.0 形式の両方にシリアル化できます。  使用するコントラクトを次のサンプル コードに示します。  
  
```  
[ServiceContract(Namespace = "")]  
    interface IDiagnosticsService  
    {  
        [OperationContract]  
        //The [WebGet] attribute controls how WCF dispatches  
        //HTTP requests to service operations based on a URI suffix  
        //(the part of the request URI after the endpoint address)  
        //using the HTTP GET method. The UriTemplate specifies a relative  
        //path of 'feed', and specifies that the format is  
        //supplied using a query string.   
        [WebGet(UriTemplate="feed?format={format}")]  
        [ServiceKnownType(typeof(Atom10FeedFormatter))]  
        [ServiceKnownType(typeof(Rss20FeedFormatter))]  
        SyndicationFeedFormatter GetProcesses(string format);  
    }  
```  
  
 `GetProcesses` 操作には、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] が HTTP GET 要求をサービス操作にディスパッチする方法を制御し、送信されるメッセージの形式を指定できるようにする <xref:System.ServiceModel.Web.WebGetAttribute> 属性で注釈が付けられています。  
  
 あらゆる [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] サービスと同様に、配信フィードは、任意のマネージ アプリケーション内での自己ホストが可能です。  配信サービスが適切に機能するには、特定のバインディング \(<xref:System.ServiceModel.WebHttpBinding>\) と、特定のエンドポイント動作 \(<xref:System.ServiceModel.Description.WebHttpBehavior>\) が必要です。  新しい <xref:System.ServiceModel.Web.WebServiceHost> クラスには、特定の構成を使用せずにこのようなエンドポイントを作成する際に便利な API が用意されています。  
  
```  
WebServiceHost host = new WebServiceHost(typeof(ProcessService), new Uri("http://localhost:8000/diagnostics"));  
  
            //The WebServiceHost will automatically provide a default endpoint at the base address  
            //using the proper binding (the WebHttpBinding) and endpoint behavior (the WebHttpBehavior)  
```  
  
 または、IIS でホストされる .svc ファイルから <xref:System.ServiceModel.Activation.WebServiceHostFactory> を使用して、同等の機能を用意することもできます \(この手法は、このサンプル コードでは示されません\)。  
  
```  
<%@ ServiceHost Language="C#|VB" Debug="true" Service="ProcessService" %>  
```  
  
 このサービスは標準の HTTP GET を使用して要求を受け取るので、サービスへのアクセスには、RSS または ATOM に対応している任意のクライアントを使用できます。  たとえば、Internet Explorer 7 などの RSS 対応のブラウザで、http:\/\/localhost:8000\/diagnostics\/feed\/?format\=atom または http:\/\/localhost:8000\/diagnostics\/feed\/?format\=rss に移動することで、このサービスの出力を表示できます。  
  
 命令型コードを使用して、配信データの読み取りと処理に[WCF 配信オブジェクト モデルを Atom や RSS に割り当てる方法](../../../../docs/framework/wcf/feature-details/how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md)を使用することもできます。  
  
```  
XmlReader reader = XmlReader.Create( "http://localhost:8000/diagnostics/feed/?format=rss",  
new XmlReaderSettings()  
{  
//MaxCharactersInDocument can be used to control the maximum amount of data   
//read from the reader and helps prevent OutOfMemoryException  
MaxCharactersInDocument = 1024 * 64  
} );  
  
SyndicationFeed feed = SyndicationFeed.Load( reader );  
  
foreach (SyndicationItem i in feed.Items)  
{  
        XmlSyndicationContent content = i.Content as XmlSyndicationContent;  
        ProcessData pd = content.ReadContent<ProcessData>();  
  
        Console.WriteLine(i.Title.Text);  
        Console.WriteLine(pd.ToString());  
}  
```  
  
### サンプルをセットアップ、ビルド、および実行するには  
  
1.  「[Windows Communication Foundation サンプルの 1 回限りのセットアップの手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)」のセットアップ手順の説明に従って、コンピュータ上で HTTP および HTTPS の正しいアドレス登録アクセス許可があることを確認します。  
  
2.  ソリューションをビルドします。  
  
3.  コンソール アプリケーションを実行します。  
  
4.  コンソール アプリケーションの実行中に、RSS 対応のブラウザーを使用して http:\/\/localhost:8000\/diagnostics\/feed\/?format\=atom または http:\/\/localhost:8000\/diagnostics\/feed\/?format\=rss に移動します。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。  続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。  このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\DiagnosticsFeed`  
  
## 参照  
 [WCF Web HTTP プログラミング モデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model.md)   
 [WCF 配信](../../../../docs/framework/wcf/feature-details/wcf-syndication.md)