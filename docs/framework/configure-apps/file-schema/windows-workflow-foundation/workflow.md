---
title: "&lt;workflow&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: 560aa9b6-9cf3-460e-b798-f87d14b1d2de
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# &lt;workflow&gt;
**activityDefinitionId** プロパティによって識別される特定のワークフローのすべてのクエリを格納する構成要素。  
  
 ワークフロー追跡とその構成の詳細については、「[ワークフロー追跡とトレース](../../../../../docs/framework/windows-workflow-foundation//workflow-tracking-and-tracing.md)」と「[追跡プロファイル](../../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)」を参照してください。  
  
## 構文  
  
```vb  
  
<system.serviceModel>  
  <tracking>    
    <trackingProfile name="String">  
      <workflow activityDefinitionId="String">  
          <activityScheduledQueries>  
             <activityScheduledQuery activityName="String"  
                 childActivityName="String"/>  
          </activityScheduledQueries>  
             <activityStateQuery activityName="String" />  
                <arguments>  
                   <argument name="String"/>  
                </arguments>  
                <states>  
                   <state name="String"/>  
                </states>  
                <variables>  
                   <variable name="String"/>  
                </variables>  
          </activityStateQueries>  
          <bookmarkResumptionQueries>  
             <bookmarkResumptionQuery name="String" />  
          </bookmarkResumptionQueries>  
          <cancelRequestQueries>  
             <cancelRequestQuery activityName="String"  
                 childActivityName="String"/>  
          </cancelRequestQueries>  
          <customTrackingQueries>  
             <customTrackingQuery activityName="String"  
                 name="String"/>  
          </customTrackingQueries>  
          <faultPropagationQueries>  
             <faultPropagationQuery activityName="String"  
                 faultHandlerActivityName="String"/>  
          </faultPropagationQueries>  
         <workflowInstanceQueries>  
            <workflowInstanceQuery>  
              <states>  
                 <state name="String"/>  
              </states>  
          </workflowInstanceQuery>  
        </workflowInstanceQueries>  
      </workflow>  
    </trackingProfile>          
   </profiles>  
  </tracking>  
</system.serviceModel>  
  
```  
  
## 属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### 属性  
  
|属性|説明|  
|--------|--------|  
|activityDefinitionId|追跡対象のワークフローのアクティビティ定義 ID を指定する文字列。|  
  
### 子要素  
  
|要素|説明|  
|--------|--------|  
|[\<activityScheduledQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/activityscheduledqueries.md)|親アクティビティによる実行がスケジュールされているアクティビティを追跡するために使用する、クエリのコレクションを表します。  アクティビティがスケジュールされたレコードを追跡参加要素が定期受信するには、このクエリが必要です。|  
|[\<activityStateQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/activitystatequeries.md)|ワークフロー インスタンスを構成するアクティビティのライフサイクルの変化を追跡するために使用する、クエリのコレクションを表します。  たとえば、ワークフロー インスタンスの "電子メール送信" アクティビティの完了を毎回追跡することが必要な場合があります。  追跡参加要素がアクティビティ状態レコード オブジェクトを定期受信するには、このクエリが必要です。  定期受信可能な状態は ActivityStates で指定します。|  
|[\<bookmarkResumptionQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/bookmarkresumptionqueries.md)|ワークフロー インスタンス内のブックマークの再開を追跡するために使用する、クエリのコレクションを表します。  追跡参加要素がブックマーク再開レコードを定期受信するには、このクエリが必要です。|  
|[\<cancelRequestedQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/cancelrequestedqueries.md)|親アクティビティが子アクティビティを取り消すための要求を追跡するのに使用する、クエリのコレクションを表します。  追跡参加要素がキャンセル要求レコード オブジェクトを定期受信するには、このクエリが必要です。|  
|[\<customTrackingQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/customtrackingqueries.md)|コード アクティビティで定義するイベントを追跡するために使用する、クエリのコレクションを表します。  追跡参加要素がカスタム追跡レコードを定期受信するには、このクエリが必要です。|  
|[\<faultPropagationQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/faultpropagationqueries.md)|1 つのアクティビティ内で発生するエラーの処理を追跡するために使用する、クエリのコレクションを表します。 このイベントは、FaultHandler がエラーを処理するたびに発生します。  1 つのアクティビティ内で発生したエラーの処理は、このようなクエリを使用して追跡する必要があります。  追跡参加要素がエラー伝達レコードを定期受信するには、このクエリが必要です。|  
|[\<workflowInstanceQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/workflowinstancequeries.md)|開始したイベントや完了したイベントなど、ワークフロー インスタンスのライフサイクルの変化を追跡する構成要素のコレクションを表します。|  
  
### 親要素  
  
|要素|説明|  
|--------|--------|  
|[\<trackingProfile\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/trackingprofile.md)|追跡参加要素内でワークフロー追跡レコードのサブスクリプションを作成するための、構成セクションを表します。  追跡プロファイルには、実行時にワークフロー インスタンスの状態が変化したときに生成されるワークフロー イベントを追跡参加要素が定期受信できるようにする、追跡クエリが含まれています。  追跡プロファイル セクション内で定義されたクエリでは、サブスクリプションによって返されるイベントの種類が定義されます。|  
  
## 解説  
 追跡プロファイルには、実行時に特定のワークフロー インスタンスの状態が変化したときに生成されるワークフロー イベントを追跡参加要素が定期受信できるようにする、追跡クエリが含まれています。  この構成要素を使用して、追跡対象のワークフロー インスタンスが識別されます。  
  
 監視の要件に応じて、ワークフローの主な状態変化の少数のセットを定期受信する、大まかなプロファイルを作成できます。  それとは反対に、結果として得られるイベントが、後で詳細な実行フローを十分に再構築できるほど豊富な、詳細なプロファイルを作成することもできます。  
  
 追跡プロファイルは、特定の追跡レコードを対象としてワークフロー ランタイムを照会できる、追跡レコード用の宣言型のサブスクリプションとして構築されます。  クエリには複数の型があり、これらを使用して、追跡レコードのさまざまなクラスを定期受信できます。  クエリの完全な一覧については、このトピックの子要素の一覧と「[追跡プロファイル](../../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)」を参照してください。  
  
 次の例は、追跡参加要素が `Started`ワークフロー イベントおよび `Completed`ワークフロー イベントを定期受信できるようにする、構成ファイル内の追跡プロファイルを示します。  
  
```  
  
<system.serviceModel>  
  <tracking>    
    <trackingProfile name="Sample Tracking Profile">  
      <workflow activityDefinitionId="*">  
         <workflowInstanceQueries>  
            <workflowInstanceQuery>  
            <states>  
              <state name="Started"/>  
              <state name="Completed"/>  
            </states>  
          </workflowInstanceQuery>  
        </workflowInstanceQueries>  
      </workflow>  
    </trackingProfile>          
   </profiles>  
  </tracking>  
</system.serviceModel>  
  
```  
  
## 参照  
 <xref:System.ServiceModel.Activities.Tracking.Configuration.ProfileWorkflowElement>   
 <xref:System.Activities.Tracking.TrackingProfile>   
 [ワークフロー追跡とトレース](../../../../../docs/framework/windows-workflow-foundation//workflow-tracking-and-tracing.md)   
 [追跡プロファイル](../../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)