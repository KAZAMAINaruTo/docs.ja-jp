---
title: "エラーを処理する方法 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: de566e39-9358-44ff-8244-780f6b799966
caps.latest.revision: 5
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 5
---
# エラーを処理する方法
ここでは、エラー処理を使用するルーティング構成の作成に必要な基本的な手順について説明します。  この例では、メッセージは送信先エンドポイントにルーティングされます。  ネットワークまたは通信関係の障害 \(<xref:System.ServiceModel.CommunicationException>\) のために、メッセージを配信できない場合、そのメッセージは代替エンドポイントに再送信されます。  
  
> [!NOTE]
>  ネットワーク障害をシミュレートするために、この例で使用される送信先エンドポイントには、誤ったアドレスが格納されています。  指定したアドレスをリッスンするサービスがないため、このエンドポイントにルーティングされるメッセージはエラーになります。  
  
> [!NOTE]
>  このトピックの例では、タイムアウト設定について明示的に説明していませんが、エラー処理を使用する際には、この点を考慮する必要があります。  エラーが発生すると、クライアントが応答を受け取る前に、追加の遅延が発生します。  これは、ルーティング サービスがエラーを受信し、そのメッセージをバックアップ エンドポイントに送信しようとするためです。  プライマリおよびバックアップの送信先エンドポイントに関連付けられたタイムアウト値が大きい場合は、クライアントでの遅延が大きくなることがあります。これは、メッセージが、バックアップ リスト内の複数のエンドポイントをフェールオーバーした後に、正常に送信されるためです。  
>   
>  ルーティング サービスが受ける遅延は、最大の場合、フィルターと関連付けられたすべてのエンドポイントのタイムアウト値の合計となる可能性があるため、これに応じて、クライアントで予測されるタイムアウト値を増やすことをお勧めします。  
  
### エラー処理の実装  
  
1.  サービスによって公開されるサービス エンドポイントを指定することによって、ルーティング サービスの基本的な構成を作成します。  次の例では、メッセージの受信に使用される、単一のサービス エンドポイントを定義します。  また、メッセージ送信に使用する deadDestination および realDestination クライアント エンドポイントも定義します。  deadDestination エンドポイントには、実行中のサービスを参照していないアドレスが格納されており、このエンドポイントへのメッセージ送信時のネットワーク障害をシミュレートするために使用されます。  
  
    ```xml  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/" />  
          </baseAddresses>  
        </host>  
        <!-- Create the Routing Service endpoint -->  
        <endpoint address="router"  
                  binding="basicHttpBinding"  
                  name="RoutingServiceEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
      </service>  
    </services>  
  
    <!-- Create the destination endpoints that we want to send to -->  
    <client>  
      <!-- Create a dummy endpoint that we know will be down -->  
      <endpoint name="deadDestination"   
                address="net.tcp://localhost:9090/servicemodelsamples/fakeDestination"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <!-- Now create the real service endpoint -->  
      <endpoint name="realDestination"   
                address="net.tcp://localhost:8080/servicemodelsamples/service"  
                binding="netTcpBinding"   
                contract="*" />  
    </client>  
    ```  
  
2.  送信先エンドポイントにメッセージをルーティングするのに使用するフィルターを定義します。  この例では、ルーティング サービスが受信するすべてのメッセージと照合するために MatchAll フィルターを使用します。  
  
    ```xml  
    <filters>  
      <!-- Create a MatchAll filter which will catch all messages -->  
      <filter name="MatchAllFilter1" filterType="MatchAll" />  
    </filters>  
  
    ```  
  
3.  バックアップ エンドポイント リストを定義します。このリストには、プライマリ送信先エンドポイントへの送信時にネットワークまたは通信の障害が発生した場合に、メッセージの送信先となるエンドポイントを格納します。  次の例では、1 つのエンドポイントを格納しているバックアップ リストを定義します。ただし、複数のエンドポイントを 1 つのバックアップ リストに指定できます。  
  
     バックアップ リストに複数のエンドポイントが格納されている場合、ネットワークまたは通信の障害が発生すると、ルーティング サービスでは、そのリスト内の最初のエンドポイントにメッセージを送信しようとします。  このエンドポイントへの送信時にネットワークまたは通信の障害が発生した場合、そのルーティング サービスでは、そのリスト内の次のエンドポイントにメッセージを送信しようとします。  このサービスは、メッセージが正常に送信されるか、すべてのバックアップ エンドポイントがネットワークまたは通信関連のエラーを返すか、メッセージが送信されてエンドポイントがネットワーク以外で通信関連以外のエラーを返すまで、このバックアップ エンドポイント リスト内の各エンドポイントにメッセージの送信を続けます。  
  
    ```  
    <backupLists>          
      <backupList name="backupEndpointList">  
          <add endpointName="realDestination" />  
      </backupList>  
    </backupLists>  
    ```  
  
4.  フィルターを deadDestination エンドポイントおよびバックアップ エンドポイント リストと関連付けるフィルター テーブルを定義します。  まず、ルーティング サービスは、このフィルターに関連付けられた送信先エンドポイントにメッセージを送信しようとします。  deadDestination エンドポイントには、実行中のサービスを参照していないアドレスが格納されているため、これはネットワーク エラーになります。  次に、このルーティング サービスは、backupEndpointlist に指定されたエンドポイントにメッセージを送信しようとします。  
  
    ```xml  
    <filterTables>  
            <!-- Set up the Routing Service's Message Filter Table -->  
            <filterTable name="filterTable1">  
                <!-- Add an entity that maps the MatchAllMessageFilter to the dead destination -->  
                <!-- If that endpoint is down, tell the Routing Service to try the endpoints -->  
                <!-- Listed in the backupEndpointList -->  
                <add filterName="MatchAllFilter1" endpointName="deadDestination" backupList="backupEndpointList"/>  
            </filterTable>  
          </filterTables>  
  
    ```  
  
5.  受信メッセージをフィルター テーブルに含まれているフィルターと照合して評価するには、ルーティング動作を使用して、フィルター テーブルをサービス エンドポイントと関連付ける必要があります。  次の例は、filterTable1 をサービス エンドポイントと関連付ける方法を示しています。  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
        <!-- Set up the Routing Behavior -->  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
## 使用例  
 構成ファイル全体の一覧を次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright (c) Microsoft Corporation.  All Rights Reserved. -->  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/" />  
          </baseAddresses>  
        </host>  
        <!-- Create the Routing Service endpoint -->  
        <endpoint address="router"  
                  binding="basicHttpBinding"  
                  name="RoutingServiceEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
      </service>  
    </services>  
    <behaviors>  
      <serviceBehaviors>  
        <!-- Set up the Routing Behavior -->  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <!-- Create the destination endpoints that we want to send to -->  
    <client>  
      <!-- Create a dummy endpoint that we know will be down -->  
      <endpoint name="deadDestination"   
                address="net.tcp://localhost:9090/servicemodelsamples/fakeDestination"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <!-- Now create the real service endpoint -->  
      <endpoint name="realDestination"   
                address="net.tcp://localhost:8080/servicemodelsamples/service"  
                binding="netTcpBinding"   
                contract="*" />  
    </client>  
    <routing>  
      <filters>  
        <!-- Create a MatchAll filter which will catch all messages -->  
        <filter name="MatchAllFilter1" filterType="MatchAll" />  
      </filters>  
      <filterTables>  
        <!-- Set up the Routing Service's Message Filter Table -->  
        <filterTable name="filterTable1">  
            <!-- Add an entrty that maps the MatchAllMessageFilter to the dead destination -->  
            <!-- If that endpoint is down, tell the Routing Service to try the endpoints -->  
            <!-- Listed in the backupEndpointList -->  
            <add filterName="MatchAllFilter1" endpointName="deadDestination" backupList="backupEndpointList"/>  
        </filterTable>  
      </filterTables>  
      <!-- Create the backup endpoint list -->  
      <backupLists>          
        <backupList name="backupEndpointList">  
            <add endpointName="realDestination" />  
        </backupList>  
      </backupLists>  
      </routing>  
  </system.serviceModel>  
</configuration>  
  
```