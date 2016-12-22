---
title: "How to: Collapse and Hide Sections of Code (Visual Basic) | Microsoft Docs"
ms.custom: ""
ms.date: "12/15/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "Visual Basic, code collapsing"
  - "Visual Basic, code hiding"
  - "Visual Basic code, collapsing and hiding"
ms.assetid: b770e8f5-e07d-491a-ab4b-a977980f9ba2
caps.latest.revision: 11
caps.handback.revision: 11
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
---
# How to: Collapse and Hide Sections of Code (Visual Basic)
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

`#Region` ディレクティブを使用すると、[!INCLUDE[vbprvb](../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] ファイルのコードをセクションごとに折りたたんで非表示にできます。  `#Region` ディレクティブを使用すると、[!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] コード エディターを使用するときに展開したり折りたたんだりできるコードのブロックを指定できます。  コードを選択して非表示にすると、管理しやすく読みやすいファイルになります。  詳細については、「[アウトライン](/visual-studio/ide/outlining)」を参照してください。  
  
 `#Region` ディレクティブは、`#If...#End If` などのコード ブロック セマンティクスをサポートします。  これらは、あるブロックで始まって他のブロックで終わることはできません。始まりと終わりが同じブロック内にあることが必要です。  `#Region` ディレクティブは関数内ではサポートされません。  
  
### コードのセクションを折りたたんで非表示にするには  
  
-   次の例のように、コードのセクションを `#Region` ステートメントと `#End Region` ステートメントの間に配置します。  
  
     [!CODE [VbVbalrConditionalComp#6](../CodeSnippet/VS_Snippets_VBCSharp/VbVbalrConditionalComp#6)]  
  
     1 つのコード ファイルで複数の `#Region` ブロックを使用できます。したがって、プロシージャやクラスのブロックを各ユーザーが独自に定義でき、それらを折りたたむことができます。  `#Region` ブロックを他の `#Region` ブロック内に入れ子にすることもできます。  
  
    > [!NOTE]
    >  非表示にしたコードもコンパイルの対象となり、`#If...#End If` ステートメントにも影響しません。  
  
## 参照  
 [Conditional Compilation](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)   
 [\#Region Directive](../../../visual-basic/language-reference/directives/region-directive.md)   
 [\#If...Then...\#Else Directives](../../../visual-basic/language-reference/directives/if-then-else-directives.md)   
 [アウトライン](/visual-studio/ide/outlining)