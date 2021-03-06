---
title: "dynamic 型の使用 (C# プログラミング ガイド) | Microsoft Docs"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- dynamic [C#], about dynamic type
- dynamic type [C#]
ms.assetid: 3828989d-c967-4a51-b948-857ebc8fdf26
caps.latest.revision: 30
author: BillWagner
ms.author: wiwagn
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
ms.translationtype: Human Translation
ms.sourcegitcommit: cf8206e856a31882f5f30ee61d965ad5672f518e
ms.openlocfilehash: 4bc2cd0e4fc165ef68338c2cda3b8c57c1bf18b2
ms.contentlocale: ja-jp
ms.lasthandoff: 07/03/2017

---
# <a name="using-type-dynamic-c-programming-guide"></a>dynamic 型の使用 (C# プログラミング ガイド)
[!INCLUDE[csharp_dev10_long](~/includes/csharp-dev10-long-md.md)] では、`dynamic` という新しい型が導入されています。 この型は静的な型ですが、`dynamic` 型のオブジェクトは静的な型チェックをバイパスします。 ほとんどの場合、`object` 型を使用する場合と同様に機能します。 コンパイル時には、`dynamic` として型指定された要素はあらゆる操作をサポートすると見なされます。 したがって、オブジェクトが COM API、IronPython などの動的言語、HTML ドキュメント オブジェクト モデル (DOM)、リフレクション、プログラムの他の場所のいずれから値を取得するのかを考慮する必要はありません。 ただし、コードが無効な場合には、実行時にエラーが検出されます。  
  
 たとえば、次のコードの `exampleMethod1` インスタンス メソッドにパラメーターが 1 つしかない場合、`ec.exampleMethod1(10, 4)` メソッドへの最初の呼び出しは引数を 2 つ含むため、コンパイラはこの呼び出しを無効と認識します。 この呼び出しではコンパイラ エラーが発生します。 `dynamic_ec.exampleMethod1(10, 4)` メソッドの 2 番目の呼び出しは、`dynamic_ec` の型が `dynamic` であるため、コンパイラによってチェックされません。 そのため、コンパイラ エラーは報告されません。 ただし、このエラーがいつまでも検出されないということではありません。 実行時に検出されて実行時例外が発生します。  
  
 [!code-cs[CsProgGuideTypes#50](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_1.cs)]  
  
 [!code-cs[CsProgGuideTypes#56](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_2.cs)]  
  
 これらの例におけるコンパイラの役割は、`dynamic` として型指定されたオブジェクトまたは式に対して各ステートメントが何を実行しようとしているかについて、情報をまとめてパッケージ化することです。 格納された情報が実行時に調べられ、無効なステートメントがある場合は実行時例外が発生します。  
  
 ほとんどの動的操作は、結果自体が `dynamic` です。 たとえば、次の例では `testSum` を使用している箇所にマウス ポインターを置くと、IntelliSense によって型が **(ローカル変数) dynamic testSum** と表示されます。  
  
 [!code-cs[CsProgGuideTypes#51](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_3.cs)]  
  
 結果が `dynamic` でない操作として、`dynamic` から他の型への変換や、`dynamic` 型の引数を含むコンストラクター呼び出しがあります。 たとえば、次の宣言の `testInstance` の型は、`dynamic` ではなく、`ExampleClass` です。  
  
 [!code-cs[CsProgGuideTypes#52](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_4.cs)]  
  
 次の「変換」のセクションに変換例を示します。  
  
## <a name="conversions"></a>変換  
 動的オブジェクトとその他の型との変換は簡単です。 そのため、開発者は動的な動作と動的でない動作を切り替えることができます。  
  
 次の例に示すように、任意のオブジェクトを動的な型に暗黙的に変換できます。  
  
 [!code-cs[CsProgGuideTypes#53](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_5.cs)]  
  
 逆に、`dynamic` 型の任意の式に暗黙的な変換を動的に適用できます。  
  
 [!code-cs[CsProgGuideTypes#54](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_6.cs)]  
  
## <a name="overload-resolution-with-arguments-of-type-dynamic"></a>dynamic 型の引数を使用したオーバーロードの解決  
 メソッド呼び出しの 1 つ以上の引数が `dynamic` 型を指定している場合、またはメソッド呼び出しの受信側が `dynamic` 型である場合は、コンパイル時ではなく実行時にオーバーロードの解決が実行されます。 次の例では、唯一のアクセス可能な `exampleMethod2` メソッドが文字列引数を受け取るように定義されている場合、`d1` を引数として送信すると、コンパイラ エラーは発生しませんが、実行時例外が発生します。 `d1` の実行時の型は `int` であり、`exampleMethod2` には文字列が必要であるため、オーバーロードの解決は実行時に失敗します。  
  
 [!code-cs[CsProgGuideTypes#55](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_7.cs)]  
  
## <a name="dynamic-language-runtime"></a>動的言語ランタイム  
 動的言語ランタイム (DLR) は、[!INCLUDE[net_v40_short](~/includes/net-v40-short-md.md)] の新しい API です。 DLR は、C# の `dynamic` 型だけでなく、IronPython や IronRuby などの動的プログラミング言語の実装もサポートするインフラストラクチャを提供します。 DLR の詳細については、「[動的言語ランタイムの概要](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)」を参照してください。  
  
## <a name="com-interop"></a>COM 相互運用  
 [!INCLUDE[csharp_dev10_long](~/includes/csharp-dev10-long-md.md)] には、Office オートメーション API などの COM API との相互運用エクスペリエンスを強化する複数の機能があります。 この機能強化には、`dynamic` 型の使用、および[名前付き引数と省略可能な引数](../../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md)の使用が含まれます。  
  
 多くの COM メソッドでは、型を `object` と指定することによって、引数の型と戻り値の型にバリエーションを持たせることができます。 このためには、C# で厳密に型指定された変数と連携できるように値の明示的なキャストが必要でした。 [/link (C# コンパイラ オプション)](../../../csharp/language-reference/compiler-options/link-compiler-option.md) オプションを使用してコンパイルする場合、`dynamic` 型が導入されて COM シグネチャの `object` のオカレンスを `dynamic` 型と同様に処理できるようになったため、ほとんどのキャストを回避できます。 たとえば、Microsoft Office Excel スプレッドシートのセルに `dynamic` 型を使用してアクセスする方法と、`dynamic` 型を使用しないでアクセスする方法の対比を次のステートメントに示します。  
  
 [!code-cs[csOfficeWalkthrough#12](../../../csharp/programming-guide/interop/codesnippet/CSharp/using-type-dynamic_8.cs)]  
  
 [!code-cs[csOfficeWalkthrough#13](../../../csharp/programming-guide/interop/codesnippet/CSharp/using-type-dynamic_9.cs)]  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|説明|  
|-----------|-----------------|  
|[dynamic](../../../csharp/language-reference/keywords/dynamic.md)|`dynamic` キーワードの使用法について説明します。|  
|[動的言語ランタイムの概要](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)|DLR の概要について説明します。DLR は動的言語の一連のサービスを共通言語ランタイム (CLR) に追加するランタイム環境です。|  
|[チュートリアル: 動的オブジェクトの作成と使用](../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)|動的なカスタム オブジェクト、および `IronPython` ライブラリにアクセスするプロジェクトを作成するための詳細な手順について説明します。|  

|[方法: Visual C# の機能を使用して Office 相互運用オブジェクトにアクセスする](../../../csharp/programming-guide/interop/how-to-access-office-onterop-objects.md)|名前付き引数と省略可能な引数、`dynamic` 型、および Office API オブジェクトへのアクセスを簡単にするその他の強化機能を使用するプロジェクトを作成する方法について説明します。|
