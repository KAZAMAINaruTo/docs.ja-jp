---
title: "永続的な二重の相関関係 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8eb0e49a-6d3b-4f7e-a054-0d4febee2ffb
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# 永続的な二重の相関関係
永続的な二重の相関関係 \(コールバック相関関係\) は、ワークフロー サービスがコールバックを最初の呼び出し元に送信する必要がある場合に便利です。WCF の二重とは異なり、コールバックは、将来のどの時点でも発生する可能性があり、同じチャネルにも同じチャネルの有効期間にも関連付けられていません。唯一の要件は、呼び出し元にコールバック メッセージをリッスンするアクティブなエンドポイントを用意することです。このため、2 つのワークフロー サービスが長時間のメッセージ交換を使用して通信できます。このトピックでは、永続的な二重の相関関係について概説します。  
  
## 永続的な二重の相関関係の使用  
 永続的な二重の相関関係を使用するには、2 つのサービスが、<xref:System.ServiceModel.NetTcpContextBinding> や <xref:System.ServiceModel.WSHttpContextBinding> など、双方向の操作をサポートするコンテキスト対応バインドを使用する必要があります。呼び出し側サービスは、クライアントの <xref:System.ServiceModel.Endpoint> で使用するバインドを指定して <xref:System.ServiceModel.WSHttpContextBinding.ClientCallbackAddress%2A> を登録します。受信側サービスは、このデータを最初の呼び出しで受信し、呼び出し側サービスへのコールバックを行う <xref:System.ServiceModel.Activities.Send> アクティビティにおいて、受信側サービス自体の <xref:System.ServiceModel.Endpoint> でこのデータを使用します。次の例では、2 つのサービスが互いに通信しています。1 つ目のサービスは、2 つ目のサービスに対してメソッドを呼び出し、応答を待機します。2 つ目のサービスでは、コールバック メソッドの名前が認識されていますが、このメソッドを実装するサービスのエンドポイントは、設計時には認識されていません。  
  
> [!NOTE]
>  エンドポイントの <xref:System.ServiceModel.Channels.AddressingVersion> が <xref:System.ServiceModel.Channels.AddressingVersion.WSAddressing10%2A> で構成される場合は、永続的な二重のみが使用できます。そうでない場合は、<xref:System.InvalidOperation> 例外が次のメッセージと共にスローされます。"メッセージに、AddressingVersion 'Addressing200408 \(http:\/\/schemas.xmlsoap.org\/ws\/2004\/08\/addressing\)' のエンドポイント参照を含むコールバック コンテキスト ヘッダーが含まれています。コールバック コンテキストを送信できるのは、AddressingVersion で 'WSAddressing10' が構成されている場合のみです。"  
  
 次の例では、<xref:System.ServiceModel.WSHttpContextBinding> を使用してコールバック <xref:System.ServiceModel.Endpoint> を作成するワークフロー サービスがホストされます。  
  
```csharp  
// Host WF Service 1.  
string baseAddress1 = "http://localhost:8080/Service1";  
WorkflowServiceHost host1 = new WorkflowServiceHost(GetWF1(), new Uri(baseAddress1));  
  
// Add the callback endpoint.  
WSHttpContextBinding Binding1 = new WSHttpContextBinding();  
host1.AddServiceEndpoint("ICallbackItemsReady", Binding1, "ItemsReady");  
  
// Add the service endpoint.  
host1.AddServiceEndpoint("IService1", Binding1, baseAddress1);  
  
// Open the first workflow service.  
host1.Open();  
Console.WriteLine("Service1 waiting at: {0}", baseAddress1);  
```  
  
 このワークフロー サービスを実装するワークフローは、ワークフロー自体の <xref:System.ServiceModel.Activities.Send> アクティビティを使用してコールバック相関関係を初期化し、<xref:System.ServiceModel.Activities.Send> と相関関係にある <xref:System.ServiceModel.Activities.Receive> アクティビティからこのコールバック エンドポイントを参照します。次の例は、`GetWF1` メソッドから返されるワークフローです。  
  
```csharp  
Variable<CorrelationHandle> CallbackHandle = new Variable<CorrelationHandle>();  
  
Receive StartOrder = new Receive  
{  
    CanCreateInstance = true,  
    ServiceContractName = "IService1",  
    OperationName = "StartOrder"  
};  
  
Send GetItems = new Send  
{  
    CorrelationInitializers =   
    {  
        new CallbackCorrelationInitializer  
        {  
            CorrelationHandle = CallbackHandle  
        }  
    },  
    ServiceContractName = "IService2",  
    OperationName = "StartItems",  
    Endpoint = new Endpoint  
    {  
        AddressUri = new Uri("http://localhost:8081/Service2"),  
        Binding = new WSHttpContextBinding  
        {  
            ClientCallbackAddress = new Uri("http://localhost:8080/Service1/ItemsReady")                          
        }  
    }  
};  
  
Receive ItemsReady = new Receive  
{  
    ServiceContractName = "ICallbackItemsReady",  
    OperationName = "ItemsReady",  
    CorrelatesWith = CallbackHandle,  
};  
  
Activity wf = new Sequence  
{  
    Variables =  
    {  
        CallbackHandle  
    },  
    Activities =  
    {  
        StartOrder,  
        new WriteLine  
        {  
            Text = "WF1 - Started"  
        },  
        GetItems,  
        new WriteLine  
        {  
            Text = "WF1 - Request Submitted"  
        },  
        ItemsReady,  
        new WriteLine  
        {  
            Text = "WF1 - Items Received"  
        }  
     }  
};  
```  
  
 2 つ目のワークフロー サービスは、システムが提供するコンテキスト ベースのバインドを使用してホストされます。  
  
```csharp  
// Host WF Service 2.  
string baseAddress2 = "http://localhost:8081/Service2";  
WorkflowServiceHost host2 = new WorkflowServiceHost(GetWF2(), new Uri(baseAddress2));  
  
// Add the service endpoint.  
WSHttpContextBinding Binding2 = new WSHttpContextBinding();  
host2.AddServiceEndpoint("IService2", Binding2, baseAddress2);  
  
// Open the second workflow service.  
host2.Open();  
Console.WriteLine("Service2 waiting at: {0}", baseAddress2);  
```  
  
 このワークフロー サービスを実装するワークフローは、<xref:System.ServiceModel.Activities.Receive> アクティビティから開始されます。この受信アクティビティは、このサービスのコールバック相関関係を初期化し、一定の遅延時間を設けて長時間の作業をシミュレーションして、サービスへの最初の呼び出しで渡されたコールバック コンテキストを使用して、最初のサービスにコールバックします。次の例は、`GetWF2` への呼び出しから返されるワークフローです。<xref:System.ServiceModel.Activities.Send> アクティビティには、`http://www.contoso.com` のプレースホルダー アドレスが設定されていることに注意してください。実行時に使用される実際のアドレスは、指定されたコールバック アドレスです。  
  
```csharp  
Variable<CorrelationHandle> ItemsCallbackHandle = new Variable<CorrelationHandle>();  
  
Receive StartItems = new Receive  
{  
    CorrelationInitializers =   
    {  
        new CallbackCorrelationInitializer  
        {  
            CorrelationHandle = ItemsCallbackHandle  
        }  
    },  
    CanCreateInstance = true,  
    ServiceContractName = "IService2",  
    OperationName = "StartItems"  
};  
  
Send ItemsReady = new Send  
{  
    CorrelatesWith = ItemsCallbackHandle,  
    Endpoint = new Endpoint  
    {  
        // The callback address on the binding is used  
        // instead of this placeholder address.  
        AddressUri = new Uri("http://www.contoso.com"),  
  
        Binding = new WSHttpContextBinding()  
    },  
    OperationName = "ItemsReady",  
    ServiceContractName = "ICallbackItemsReady"  
};  
  
Activity wf = new Sequence  
{  
    Variables =  
    {  
        ItemsCallbackHandle  
    },  
    Activities =  
    {  
        StartItems,  
        new WriteLine  
        {  
            Text = "WF2 - Request Received"  
        },  
        new Delay  
        {  
            Duration = TimeSpan.FromMinutes(90)  
        },  
        new WriteLine  
        {  
            Text = "WF2 - Sending items"  
        },  
        ItemsReady,  
        new WriteLine  
        {  
            Text = "WF2 - Items sent"  
        }  
     }  
};  
```  
  
 `StartOrder` メソッドが最初のワークフローで呼び出されると、次の出力が表示されます。これは、2 つのワークフローの実行フローを示しています。  
  
```Output  
Service1 waiting at: http://localhost:8080/Service1  
Service2 waiting at: http://localhost:8081/Service2  
Enter キーを押して閉じます。  
WF1 - Started  
WF2 - Request Received  
WF1 - Request Submitted  
WF2 - Sending items  
WF2 - Items sent  
WF1 - Items Received  
  
```  
  
 この例では、どちらのワークフローも <xref:System.ServiceModel.Activities.CallbackCorrelationInitializer> を使用して相関関係を明示的に管理しています。サンプル ワークフローには相関関係が 1 つしかないため、既定の <xref:System.ServiceModel.Activities.CorrelationHandle> で十分に管理できます。  
  
## 参照  
 [永続的な二重 &#91;WF サンプル&#93;](../../../../docs/framework/windows-workflow-foundation/samples/durable-duplex.md)