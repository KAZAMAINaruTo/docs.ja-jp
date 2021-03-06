---
title: "ポータブル サブセット プロジェクトでサービス参照を追加する | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 61ccfe0f-a34b-40ca-8f5e-725fa1b8095e
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# ポータブル サブセット プロジェクトでサービス参照を追加する
ポータブル サブセット プロジェクトにより、.NET アセンブリ プログラマは 1 つのソース ツリーを保持しつつ、システムを構築できるようになります。また、ポータブル サブセット プロジェクトは、複数の .NET プラットフォーム \(デスクトップ、Silverlight、Windows Phone、および XBOX\) をサポートしています。ポータブル サブセット プロジェクトは、コア .NET プラットフォームで使用できる .NET Framework アセンブリである .NET ポータブル ライブラリのみを参照します。  
  
## サービス参照の追加の詳細  
 ポータブル サブセット プロジェクトでサービス参照を追加する場合は、次の制限が適用されます。  
  
1.  <xref:System.Xml.Serialization.XmlSerializer> では、文字エンコーディングのみを使用できます。SOAP エンコーディングを使用すると、インポート中にエラーが発生します。  
  
2.  <xref:System.Runtime.Serialization.DataContractSerializer> シナリオを使用するサービスの場合、再利用された型をポータブル サブセットからのみ受け取ることを確認するために、データ コントラクト サロゲートが提供されます。  
  
3.  ポータブル ライブラリでサポートされていないバインド \(<xref:System.ServiceModel.BasicHttpBinding>、トランザクション フロー、信頼できるセッション、または MTOM エンコーディングがない <xref:System.ServiceModel.WsHttpBinding>、および等価のカスタム バインドを除くすべてのバインド\) に依存するエンドポイントは無視されます。  
  
4.  メッセージ ヘッダーは、インポート前にすべての操作におけるすべてのメッセージの説明から削除されます。  
  
5.  非ポータブル属性 \(<xref:System.ComponentModel.DesignerCategoryAttribute>、<xref:System.Serializable>、および <xref:System.ServiceModel.TransactionFlow>\) は、生成されたクライアント プロキシ コードから削除されます。  
  
6.  非ポータブル プロパティ \(ProtectionLevel、SessionMode、IsInitiating、IsTerminating\) は、<xref:System.ServiceModel.ServiceContractAttribute>、<xref:System.ServiceModel.OperationContract>、および <xref:System.ServiceModel.FaultContract> から削除されます。  
  
7.  すべてのサービス操作は、クライアント プロキシ上で非同期操作として生成されます。  
  
8.  非ポータブル型を使用する生成済みのクライアント コンストラクターは削除されます。  
  
9. <xref:System.Net.CookieContainer> インスタンスは、生成されたクライアントで公開されます。  
  
10. コード ジェネレーターのアセンブリとバージョンを示すコメント "`// This code was auto-generated by Microsoft.VisualStudio.Portable.AddServiceReference, version 1.0.0.0`" がファイルの先頭に挿入されます。  
  
11. <xref:System.Runtime.Serialization.ISerializable> インターフェイスはサポートされません。  
  
12. Net.Tcp および PollingDuplex バインドはサポートされません。  
  
13. <xref:System.Runtime.Serialization.DataContractSerializer> は常にエラーに対して使用されます。  
  
14. ポータブル サブセット プロジェクトでは <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A> はサポートされません。  
  
## 参照  
 [WCF クライアントを使用したサービスへのアクセス](../../../docs/framework/wcf/accessing-services-using-a-wcf-client.md)   
 [汎用性のあるクラス ライブラリ](http://msdn.microsoft.com/library/gg597391\(v=vs.110\))