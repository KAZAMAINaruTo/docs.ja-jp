---
title: "カルチャを認識しない文字列比較の実行 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "culture パラメーター"
  - "カルチャを認識しない文字列操作, 比較"
  - "文字列比較 [.NET Framework], カルチャを認識しない"
  - "String.Compare メソッド"
  - "String.CompareTo メソッド"
  - "文字列 [.NET Framework], 比較"
ms.assetid: abae50ef-32f7-4a50-a540-fd256fd1aed0
caps.latest.revision: 23
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 23
---
# カルチャを認識しない文字列比較の実行
既定では、<xref:System.String.Compare%2A?displayProperty=fullName> メソッドはカルチャを認識し、大文字と小文字を区別する比較を実行します。  また、このメソッドには、使用するカルチャを `culture` パラメーターで指定し、使用する比較規則を `comparisonType` パラメーターで指定できる、複数のオーバーロードが含まれています。  既定のオーバーロードの代わりにこれらのメソッドを呼び出すと、特定のメソッド呼び出しで使用する規則に関するあいまいさが解消され、特定の比較がカルチャに依存するかどうかが明確になります。  
  
> [!NOTE]
>  <xref:System.String.CompareTo%2A?displayProperty=fullName> メソッドの両方のオーバーロードでは、カルチャに依存した比較と大文字と小文字を区別する比較を実行します。このメソッドを使用してカルチャに依存しない比較を実行することはできません。  コードをわかりやすくするために、<xref:System.String.Compare%2A?displayProperty=fullName> メソッドを代わりに使用することをお勧めします。  
  
 カルチャに依存する操作の場合は、`comparisonType` パラメーターとして <xref:System.StringComparison?displayProperty=fullName> 列挙値または <xref:System.StringComparison?displayProperty=fullName> 列挙値を指定します。  現在のカルチャ以外の指定されたカルチャを使用して、カルチャに依存した比較を実行する場合は、 パラメーターとして、そのカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトを指定します。  
  
 <xref:System.String.Compare%2A?displayProperty=fullName> メソッドでサポートされている、カルチャに依存しない文字列比較は、言語的な比較 \(インバリアント カルチャの並べ替え規則に基づきます\) または非言語的な比較 \(文字列内の文字の序数値に基づきます\) です。  カルチャに依存しないほとんどの文字列比較は非言語的な比較です。  このような比較の場合は、`comparisonType` パラメーターとして <xref:System.StringComparison?displayProperty=fullName> 列挙値または <xref:System.StringComparison?displayProperty=fullName> 列挙値を指定します。  たとえば、セキュリティに関する決定 \(ユーザー名やパスワードの比較など\) が文字列比較の結果に基づいて行われる場合は、この操作をカルチャに依存しない非言語的な比較に指定して、操作の結果が特定のカルチャまたは言語の規則に影響されないようにする必要があります  
  
 複数のカルチャからの対応する文字列を一貫した方法で言語的に処理する場合は、カルチャに依存しない言語的な文字列比較を使用します。  たとえば、アプリケーションのリスト ボックスに表示する単語の一覧で複数の文字セットが使用されている場合などに、現在のカルチャに関係なく同じ順序で単語を表示することが必要になります。  カルチャに依存しない言語的な比較用に、英語の言語規則に基づくインバリアント カルチャが .NET Framework によって定義されています。  カルチャに依存しない言語的な比較を実行するには、`comparisonType` パラメーターとして <xref:System.StringComparison?displayProperty=fullName> または <xref:System.StringComparison?displayProperty=fullName> を指定します。  
  
 次の例では、カルチャに依存しない 2 つの非言語的な文字列比較を実行します。  最初の例では大文字小文字が区別されますが、2 番目の例では区別されません。  
  
 [!code-csharp[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/cs/cultureinsensitive1.cs#1)]
 [!code-vb[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/vb/cultureinsensitive1.vb#1)]  
  
## 参照  
 <xref:System.String.Compare%2A?displayProperty=fullName>   
 <xref:System.String.CompareTo%2A?displayProperty=fullName>   
 [カルチャを認識しない文字列操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)   
 [文字列を使用するためのベスト プラクティス](../../../docs/standard/base-types/best-practices-strings.md)