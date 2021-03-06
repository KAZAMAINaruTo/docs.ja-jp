---
title: "WCF Visual Studio テンプレート | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6a608575-3535-4190-89da-911e24c8374f
caps.latest.revision: 31
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 31
---
# WCF Visual Studio テンプレート
[!INCLUDE[indigo1](../../../includes/indigo1-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] テンプレートは、[!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] サービスや周辺アプリケーションをすばやく構築するために [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] で使用できる、定義済みのプロジェクト テンプレートと項目テンプレートです。  
  
## <a name="using-the-wcf-templates"></a>WCF テンプレートの使用  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] テンプレートには、サービス開発のための基本クラス構造が含まれています。 これには、サービス コントラクト、データ コントラクト、サービス実装、および構成の基本的な定義が含まれます。 これらのテンプレートを使用すると、最小限のコードを使用した単純なサービスや、より高度なサービスのビルド ブロックを作成できます。  
  
### <a name="wcf-service-library-project-template"></a>WCF サービス ライブラリ プロジェクト テンプレート  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)]サービス ライブラリ プロジェクト テンプレートは [新しいプロジェクト] ダイアログ ボックス **Visual C# \WCF**と**Visual Basic\WCF**します。  
  
 使用して新しいプロジェクトを作成する場合、 **WCF サービス**テンプレート、新しいプロジェクトには、次の&3; つのファイルに自動的に含まれています。  
  
-   サービス コントラクト ファイル (IService1.cs または IService1.vb)。 サービス コントラクト ファイルは、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスの属性が適用されたインターフェイスです。 このファイルには、サービスの定義方法を示す単純なサービスの定義が含まれています。その他に、パラメーター ベースの操作や、単純なデータ コントラクトのサンプルも含まれています。 このファイルは、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービス プロジェクトの作成後にコード エディターに表示される既定のファイルです。  
  
-   サービス実装ファイル (Service1.cs または Service1.vb)。 サービス実装ファイルは、サービス コントラクト ファイルに定義されているコントラクトを実装します。  
  
-   アプリケーション構成ファイル (App.config)。 この構成ファイルには、セキュリティで保護された HTTP バインディングを含む [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービス モデルの基本要素が含まれます。 サービスのエンドポイントも含まれており、メタデータの交換も可能です。  
  
> [!NOTE]
>  [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]使用して実行した場合に、App.config ファイルをプロジェクトの構成ファイルとして認識するように構成、 [WCF サービス ホスト (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md)、これは、既定の構成。 サービス ライブラリを実行可能ファイルでホストする場合は、DLL の構成ファイルは無効になるため、その実行可能ファイルの構成ファイルに構成コードを移動する必要があります。  
  
### <a name="wcf-service-application-template"></a>WCF サービス アプリケーション テンプレート  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)]サービス アプリケーション テンプレートは、[新しいプロジェクト] ダイアログ ボックスの下にある **Visual C# \WCF**と**Visual Basic\WCF**します。  
  
 使用して新しいプロジェクトを作成する場合、 **WCF Web アプリケーション サービス**テンプレート プロジェクトにはには、次の&4; つのファイルが含まれています。  
  
-   サービス ホスト ファイル (service1.svc)。  
  
-   サービス コントラクト ファイル (IService1.cs または IService1.vb)。  
  
-   サービス実装ファイル (Service1.svc.cs または Service1.svc.vb)。  
  
-   Web 構成ファイル (Web.config)。  
  
 このテンプレートでは、自動的に Web サイトが作成されて (仮想ディレクトリに配置されます)、サービスがホストされます。  
  
### <a name="wcf-web-site-template"></a>WCF Web サイト テンプレート  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] Web サイト テンプレートは、[新しいプロジェクト] ダイアログ ボックスの下にある **Visual C \Web Site\WCF サービス**と**Visual Basic\Web wcf サービス**します。 これによって、WCF サービス アプリケーションのテンプレートと同じファイルが作成されますが、ASP.NET Web サイトであるかのように編成されます。 App_Code フォルダーと App_Data フォルダーが作成されます。  
  
### <a name="wcf-service-item-template"></a>WCF サービス項目テンプレート  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービス項目テンプレートは、既存の [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] プロジェクトに [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] サービスをすばやく追加できるカスタム テンプレートです。  
  
 このテンプレートを使用するには、**ソリューション エクスプ ローラー** ウィンドウで、プロジェクト名を右クリックし、順にポイント**追加**、クリックして**[新しい項目の**を起動する、**新しい項目の追加**] ダイアログ ボックス。  
  
 サービス インターフェイス ファイルとサービス実装ファイルがルート プロジェクト フォルダーに配置されます。  
  
 このテンプレートでは、新しいサービスの構成セクションが既存の構成ファイルと互換性がある場合にそれらがマージされます。  
  
 既存のプロジェクトが Web プロジェクトの場合は、サービス ホスト ファイル (service1.svc) も作成されます。  
  
### <a name="wcf-wf-service-project-and-item-template"></a>WCF WF サービス プロジェクト/項目テンプレート  
 これらのテンプレートは、ワークフロー サービスをホストする [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスを作成します。ワークフロー サービスは、Web サービスのようにアクセスできるワークフローです。 この他に、XAML プログラミング モデルや命令型プログラミング モデルのテンプレートも用意されています。 これらのテンプレートを使用すると、シーケンシャル ワークフローやステート マシン ワークフローを作成できます。 これらのワークフローの種類の詳細については、次を参照してください。 [Windows Workflow Foundation チュートリアル](http://msdn.microsoft.com/ja-jp/e9705654-bd96-4b56-8d98-f1f118112d97)します。 [!INCLUDE[crabout](../../../includes/crabout-md.md)]ワークフロー プロジェクトを作成するを参照してください[従来のワークフロー プロジェクトを作成する](../Topic/Creating%20Legacy%20Workflow%20Projects.md)です。  
  
 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] デザイナーでは、コード ベースのワークフローではなく XOML 型のワークフローを使用すると応答性が向上します。 XOML ワークフローは、既定で作成されるワークフロー型です。  
  
### <a name="wcf-syndication-service-library-template"></a>WCF 配信サービス ライブラリ テンプレート  
 このテンプレートを使用すると、RSS フィードや ATOM フィードを [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスとして公開できます。 詳細については、次を参照してください。 [WCF 配信](../../../docs/framework/wcf/feature-details/wcf-syndication.md)します。  
  
#### <a name="changing-the-address-of-the-feed"></a>フィードのアドレスの変更  
 配信テンプレートは、実行中に Internet Explorer を使用します。 プロジェクトを右クリックしたとき**ソリューション エクスプ ローラー**で[!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)][**プロパティ**選択してから、**デバッグ**] タブをクリックして、テンプレートの既定のアドレスを確認できます。 Internet Explorer は、このアドレスにあるフィードを開きます。  
  
 内のアドレスを変更する必要がありますもフィードのアドレスを変更する場合、**デバッグ** タブをクリックします。 これを変更しないと、Internet Explorer が既定のアドレスにあるフィードを開こうとして、エラーになります。  
  
### <a name="ajax-enabled-wcf-service-item-template"></a>AJAX 対応 WCF サービス項目テンプレート  
 このテンプレートは、AJAX コントロールを [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスとして公開します。 AJAX コントロールの詳細については、次を参照してください。、 [AJAX コントロールに関するドキュメント](http://go.microsoft.com/fwlink/?LinkId=96717)します。  
  
### <a name="silverlight-enabled-wcf-service-item-template"></a>Silverlight 対応 WCF サービス項目テンプレート  
 このテンプレートは、Silverlight クライアントまたはフロントエンドにデータを提供する Web サービスを作成します。 このテンプレートを Web サイトまたは Web アプリケーションのプロジェクトに追加すると、Silverlight クライアントとの通信をサポートするサービス コードと構成が含まれた [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスを作成できます。 使用することができますし、**サービス参照の追加**クライアントにサービスのクライアント プロキシを追加し、Silverlight クライアントと Silverlight 対応 WCF サービスの間のデータを交換します。  
  
 このテンプレートにアクセスするで Web サイトまたは Web アプリケーション プロジェクトを右クリックし**ソリューション エクスプ ローラー**、クリックして**新しい項目の追加**、 をクリック**Silverlight 対応 WCF サービス**します。  
  
> [!NOTE]
>  Silverlight 対応 WCF サービスは、セキュリティ設定を一切有効にせずに `basicHttpBinding` エンドポイントを公開します。 したがって、サービスに接続しているすべてのクライアントが、このサービスに関する情報を取得できることになります。 また、サービスとクライアント間で交換されるメッセージの署名と暗号化も行われません。 エンドポイントを正しくセキュリティで保護するには、ASP.NET 認証や HTTPS などのメカニズムを使用する必要があります。  
  
## <a name="see-also"></a>関連項目  
 [WCF サービス ホスト (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md)   
 [WCF テスト クライアント (WcfTestClient.exe)](../../../docs/framework/wcf/wcf-test-client-wcftestclient-exe.md)