---
title: "MSMQ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d9fca29f-fa44-4ec4-bb48-b10800694500
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# MSMQ
MSMQ アプリケーションでは、キューに置かれたチャネルから MSMQに、および MSMQ からキューに置かれたチャネルに追加アクティビティは転送されません。  
  
 さらに、MSMQ メッセージ ID と SOAP メッセージ ID \(および、存在する場合はアクティビティ ID\) は、キューに置かれたチャネルのトレースの一部として送信操作を追跡されます。  
  
 MSMQ メッセージ ID と SOAP メッセージ ID \(および、存在する場合はアクティビティ ID\) は、キューに置かれたチャネルのトレースの一部として受信操作を追跡されます。  
  
 受信操作で必要な転送については、他のトランスポートと同様に実行されます \(受信バイト\-\>プロセス メッセージ\-\> 操作\)。