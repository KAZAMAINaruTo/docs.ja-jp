---
title: "119 - WorkflowInstanceUpdatedRecord | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 32485d0a-dcdb-45bc-b1e3-79fa9ad9439b
caps.latest.revision: 4
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 4
---
# 119 - WorkflowInstanceUpdatedRecord
## プロパティ  
  
|||  
|-|-|  
|ID|119|  
|キーワード|HealthMonitoring、WFTracking|  
|レベル|情報|  
|チャネル|Microsoft\-Windows\-Application Server\-Applications\/Analytic|  
  
## 説明  
 このイベントは、ワークフロー インスタンスが更新されたときに、ETW 追跡参加要素によって生成されます。  
  
## メッセージ  
 TrackRecord\= WorkflowInstanceUpdatedRecord, InstanceID \= %1、RecordNumber \= %2、EventTime \= %3、ActivityDefinitionId \= %4、State \= %5、OriginalDefinitionIdentity \= %6、UpdatedDefinitionIdentity \= %7、Annotations \= %8、ProfileName \= %9  
  
## 詳細  
  
|データ項目名|データ項目の型|説明|  
|------------|-------------|--------|  
|InstanceId|xs:GUID|ワークフローのインスタンス ID|  
|RecordNumber|xs:long|生成されたレコードのシーケンス番号|  
|EventTime|xs:dateTime|イベントの生成時刻 \(UTC\)|  
|ActivityDefinitionId|xs:string|ワークフローのルート アクティビティの名前|  
|状態|xs:string|ワークフローの現在の状態。|  
|OriginalDefinitionIdentity|xs:string|元のワークフロー定義 ID|  
|UpdatedDefinitionIdentity|xs:string|更新されたワークフロー定義 ID|  
|コメント|xs:string|このイベントに追加された注釈。  値は、\<items\>\< item name \= "annotationName" type\="System.String"\>annotationValue\<\/item\>\<\/items\> という形式で XML 要素に格納されます。  注釈が指定されていない場合は、文字列に \<items\/\> が含まれます。  ETW イベントのサイズは、ETW バッファーのサイズまたは ETW イベントの最大ペイロードに制限されます。  イベントのサイズが ETW の制限を超えると、注釈が破棄され、注釈値が \<items\>...\<\/items\> に置き換えられて、イベントが切り捨てられます。|  
|ProfileName|xs:string|このイベントを生成した追跡プロファイルの名前|  
|WorkflowDefinitionIdentity|xs:string|ワークフロー定義 ID|  
|AppDomain|xs:string|AppDomain.CurrentDomain.FriendlyName で返される文字列。|