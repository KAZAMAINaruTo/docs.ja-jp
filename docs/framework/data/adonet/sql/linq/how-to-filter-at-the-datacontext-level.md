---
title: "How to: Filter at the DataContext Level | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 15505cd7-0df2-427a-9f86-e0f96f60ee2e
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# How to: Filter at the DataContext Level
`EntitySets` を `DataContext` レベルでフィルター処理できます。  このフィルター処理は、対象の <xref:System.Data.Linq.DataContext> インスタンスを使って実行されたすべてのクエリに適用されます。  
  
## 使用例  
 次の例では、あらかじめ読み込まれた顧客からの注文を `ShippedDate` に基づいてフィルター処理するために <xref:System.Data.Linq.DataLoadOptions.AssociateWith%28System.Linq.Expressions.LambdaExpression%29?displayProperty=fullName> が使用されます。  
  
 [!code-csharp[DLinqQueryConcepts#10](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#10)]
 [!code-vb[DLinqQueryConcepts#10](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#10)]  
  
## 参照  
 [Query Concepts](../../../../../../docs/framework/data/adonet/sql/linq/query-concepts.md)