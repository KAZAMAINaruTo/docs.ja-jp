---
title: "303 - UserDefinedInformationEventOccured | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 5ed5acaf-3755-4417-92c4-4ebc8e854ca1
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# 303 - UserDefinedInformationEventOccured
## プロパティ  
  
|||  
|-|-|  
|ID|303|  
|キーワード|Troubleshooting、HealthMonitoring、UserEvents、ServiceModel、EndToEndMonitoring|  
|レベル|Information \(情報\)|  
|チャネル|Microsoft\-Windows\-Application Server\-Applications\/Analytic|  
  
## 説明  
 このイベントは、ユーザー コードから生成されます。開発者は、カスタム定義の情報イベントがサービスで発生したときに、このイベントを生成できます。これは、<xref:System.Diagnostics.Eventing> API を使用して実行できます。また、その API をラップし、このイベントを適切に生成する方法を示す、WCF サンプルもあります。  
  
## メッセージ  
 名前:'%1'、参照:'%2'、ペイロード:%3  
  
## 詳細  
  
|データ項目名|データ項目の型|説明|  
|------------|-------------|--------|  
|Name|`xs:string`|イベントのユーザー定義名。|  
|HostReference|`xs:string`|Web ホスト サービスの場合は、このフィールドにより、サービスが Web 階層内で一意に識別されます。その形式は、'Web サイト名アプリケーション仮想パス&#124;サービス仮想パス&#124;サービス名' と定義されます。例: 'Default Web Site\/CalculatorApplication&#124;\/CalculatorService.svc&#124;CalculatorService'。|  
|Payload|`xs:string`|イベントのユーザー定義ペイロード。|