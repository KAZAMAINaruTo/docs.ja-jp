---
title: "$ (C# リファレンス) | Microsoft Docs"
ms.date: 2017-02-09
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- $_CSharpKeyword
- $
dev_langs:
- CSharp
helpviewer_keywords:
- $ special character [C#]
- $ language element [C#]
ms.assetid: 7d9e21b5-eac3-4878-9530-50e4da578acd
author: rpetrusha
ms.author: ronpet
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 8b6afd7d9839f6d86f7fcfa3f097053ba4f418ff
ms.lasthandoff: 03/13/2017

---
# <a name="-c-reference"></a>$ (C# リファレンス)

リテラル文字列を[挿入文字列](../keywords/interpolated-strings.md)として識別します。 挿入文字列はテンプレートのような文字列で、"*挿入式*" とリテラル テキストを含みます。 挿入文字列が解決されると、たとえば代入ステートメントまたはメソッド呼び出し内で、結果文字列の文字列表現によって挿入式が置き換えられます。 挿入文字列は、.NET Framework でサポートされている[複合書式指定文字列](../../../standard/base-types/composite-format.md)の代わりになるものです。

次の例では、`$` 文字を使用して、挿入文字列を定義します。

[!CODE-cs[interpolated-string-symbol](../../../../samples/snippets/csharp/language-reference/keywords/dollar-sign1.cs#1)]

挿入文字列の詳細については、トピック「[挿入文字列](../keywords/interpolated-strings.md)」を参照してください。

## <a name="see-also"></a>関連項目  
 [C# リファレンス](../../../csharp/language-reference/index.md)   
 [C# プログラミング ガイド](../../../csharp/programming-guide/index.md)   
 [C# の特殊文字](../../../csharp/language-reference/tokens/index.md)

