---
title: "HTTP、TCP、または名前付きパイプを使用した非同期シナリオ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a4d62402-43a4-48a4-9ced-220633ebc4ce
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# HTTP、TCP、または名前付きパイプを使用した非同期シナリオ
ここでは、マルチスレッド要求で HTTP、TCP、または名前付きパイプを使用したときの、さまざまな非同期要求\/応答シナリオでのアクティビティおよび転送について説明します。  
  
## エラーを伴わない非同期要求\/応答  
 ここでは、マルチスレッド クライアントを使用したときの、非同期要求\/応答シナリオでのアクティビティおよび転送について説明します。  
  
 呼び出し元アクティビティは、`beginCall` が返され、`endCall` が返されると終了します。コールバックが呼び出されたときは、そのコールバックが返されます。  
  
 呼び出されたアクティビティは、`beginCall` が返され、`endCall` が返されると終了します。または、そのアクティビティからコールバックが呼び出された場合は、コールバックが返されると終了します。  
  
### コールバックを伴わない非同期クライアント  
  
#### HTTP を使用して、両方の側で伝達が有効になっている場合  
 ![非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/asyn1.gif "Asyn1")  
  
 図 1. 非同期クライアント、コールバックなし、両方の側で `propagateActivity`\=`true`、HTTP  
  
 `propagateActivity`\=`true` の場合、"メッセージを処理" は転送先の "アクションを処理" アクティビティを示します。  
  
 HTTP ベースのシナリオでは、最初に送信するメッセージで "バイトを受信" が呼び出され、要求の有効期間だけ存在します。  
  
#### HTTP を使用して、両方の側で伝達が無効になっている場合  
 どちらかの側で `propagateActivity`\=`false` の場合、"メッセージを処理" は転送先の "アクションを処理" アクティビティを示しません。したがって、新しい ID を使用して、新しい一時的な "アクションを処理" アクティビティが呼び出されます。非同期応答と ServiceModel コード内の要求が一致する場合は、アクティビティ ID をローカル コンテキストから取得できます。その ID を使用して、実際の "アクションを処理" アクティビティに転送できます。  
  
 ![HTTP、TCP、および名前付きパイプを使用した非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/async2.gif "Async2")  
  
 図 2. 非同期クライアント、コールバックなし、どちらかの側で `propagateActivity`\=`false`、HTTP  
  
 HTTP ベースのシナリオでは、最初に送信するメッセージで "バイトを受信" が呼び出され、要求の有効期間だけ存在します。  
  
 "アクションを処理" アクティビティは、呼び出し元または呼び出し先で `propagateActivity`\=`false` の場合、および応答メッセージに Action ヘッダーが含まれない場合に、非同期クライアントで作成されます。  
  
#### TCP または名前付きパイプを使用して、両方の側で伝達が有効になっている場合  
 ![HTTP、TCP、および名前付きパイプを使用した非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/async3.gif "Async3")  
  
 図 3. 非同期クライアント、コールバックなし、両方の側で `propagateActivity`\=`true`、名前付きパイプ\/TCP  
  
 名前付きパイプまたは TCP ベースのシナリオでは、クライアントが開かれるときに "バイトを受信" が呼び出され、接続の有効期間だけ存在します。  
  
 図 1. と同様、`propagateActivity`\=`true` の場合、"メッセージを処理" は転送先の "アクションを処理" アクティビティを示します。  
  
#### TCP または名前付きパイプを使用して、両方の側で伝達が無効になっている場合  
 名前付きパイプまたは TCP ベースのシナリオでは、クライアントが開かれるときに "バイトを受信" が呼び出され、接続の有効期間だけ存在します。  
  
 図2. と同様、どちらかの側で `propagateActivity`\=`false` の場合、"メッセージを処理" は転送先の "アクションを処理" アクティビティを示しません。したがって、新しい ID を使用して、新しい一時的な "アクションを処理" アクティビティが呼び出されます。非同期応答と ServiceModel コード内の要求が一致する場合は、アクティビティ ID をローカル コンテキストから取得できます。その ID を使用して、実際の "アクションを処理" アクティビティに転送できます。  
  
 ![HTTP、TCP、および名前付きパイプを使用した非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/async4.gif "Async4")  
  
 図 4. 非同期クライアント、コールバックなし、どちらかの側で `propagateActivity`\=`false`、名前付きパイプ\/TCP  
  
### コールバックを伴う非同期クライアント  
 このシナリオでは、コールバックと `endCall` に対するアクティビティ G と A’、およびその転送 \(送受信\) が追加されています。  
  
 ここでは、`propragateActivity`\=`true` で HTTP を使用する例だけを示します。ただし、追加のアクティビティや転送は、その他の場合 \(つまり、`propagateActivity`\=`false` で TCP または名前付きパイプを使用\) にも適用されます。  
  
 クライアントがユーザー コードを呼び出して結果の準備が完了したことを通知すると、コールバックは新しいアクティビティ \(G\) を作成します。ユーザー コードは、次に、コールバック内 \(図 5 を参照\) またはコールバック外 \(図 6 を参照\) で `endCall` を呼び出します。`endCall` がどのユーザー アクティビティから呼び出されているかわからないため、このアクティビティには "`A’`" というラベルが付けられています。A’ と A が同じである場合もありますし、異なる場合もあります。  
  
 ![非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/asynccallback1.gif "AsyncCallback1")  
  
 図 5. 非同期クライアント、コールバックあり、コールバック内での `endCall`  
  
 ![非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/asynccallback2.gif "AsyncCallback2")  
  
 図 6. 非同期クライアント、コールバックあり、コールバック外での `endCall`  
  
### コールバックを伴う非同期サーバー  
 ![HTTP、TCP、および名前付きパイプを使用した非同期シナリオ](../../../../../docs/framework/wcf/diagnostics/tracing/media/aynchserver.gif "AynchServer")  
  
 図 7. 非同期サーバー、コールバックあり  
  
 チャネル スタックは、"メッセージを受信" でクライアントをコールバックします。この処理のトレースは、"要求を処理" アクティビティ自体で出力されます。  
  
## エラーを伴う非同期要求\/応答  
 エラー メッセージは、`endCall` 中に受信されます。この点を除き、アクティビティおよび転送は前のシナリオと同様です。  
  
## エラーを伴う\/伴わない非同期一方向要求\/応答  
 クライアントに返される応答やエラーはありません。