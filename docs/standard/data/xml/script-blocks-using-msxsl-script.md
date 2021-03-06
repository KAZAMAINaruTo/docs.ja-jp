---
title: "msxsl:script を使用したスクリプト ブロック | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: fde6f43f-c594-486f-abcb-2211197fae20
caps.latest.revision: 4
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 4
---
# msxsl:script を使用したスクリプト ブロック
<xref:System.Xml.Xsl.XslCompiledTransform> クラスは、`msxsl:script` 要素を使用した埋め込みスクリプトをサポートしています。  スタイル シートが読み込まれると、定義されているすべての関数はコード ドキュメント オブジェクト モデル \(CodeDOM\) によって Microsoft intermediate language \(MSIL\) にコンパイルされ、実行時に実行されます。  埋め込みのスクリプト ブロックから生成されたアセンブリは、スタイル シートに対して生成されるアセンブリとは区別されます。  
  
## XSLT スクリプトの有効化  
 埋め込みスクリプトのサポートは、<xref:System.Xml.Xsl.XslCompiledTransform> クラスではオプションの XSLT 設定です。  スクリプトのサポートは既定で無効になっています。  スクリプトのサポートを有効にするには、<xref:System.Xml.Xsl.XsltSettings.EnableScript%2A> プロパティを `true` に設定して <xref:System.Xml.Xsl.XsltSettings> オブジェクトを作成し、そのオブジェクトを <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> メソッドに渡します。  
  
> [!NOTE]
>  XSLT スクリプトは、スクリプトのサポートが必要であり、完全に信頼された環境で作業している場合のみ有効にします。  
  
## msxsl:script 要素の定義  
 `msxsl:script` 要素は XSLT 1.0 勧告に対するマイクロソフトの拡張機能であり、次のように定義されます。  
  
```  
<msxsl:script language = "language-name" implements-prefix = "prefix of user namespace"> </msxsl:script>  
```  
  
 `msxsl` プレフィックスは `urn:schemas-microsoft-com:xslt` 名前空間 URI に関連付けられています。  スタイル シートには `xmlns:msxsl=urn:schemas-microsoft-com:xslt` という名前空間の宣言が必要です。  
  
 `language` 属性は省略できます。  その値は、埋め込みコード ブロックのコード言語です。  言語は <xref:System.CodeDom.Compiler.CodeDomProvider.CreateProvider%2A?displayProperty=fullName> メソッドを使用して、適切な CodeDOM コンパイラに対応付けられます。  <xref:System.Xml.Xsl.XslCompiledTransform> クラスは、適切なプロバイダーがコンピューターにインストールされ、machine.config ファイルの system.codedom セクションに登録されている限り、任意の Microsoft .NET 言語をサポートできます。  `language` 属性を指定しない場合、既定の言語は JScript です。  言語名では大文字小文字が区別されないため、"JavaScript" と "javascript" は同じと見なされます。  
  
 `implements-prefix` 属性は必須です。  この属性は、名前空間を宣言し、それをスクリプト ブロックに関連付けるために使用されます。  この属性の値は、名前空間を表すプレフィックスです。  このプレフィックスは、スタイル シート内の任意の場所で定義できます。  
  
> [!NOTE]
>  `msxsl:script` 要素を使用するときは、どの言語でも、スクリプトを CDATA セクション内に配置することをお勧めします。  スクリプトは、その言語で使用する演算子、識別子、または区切り記号を含むことがあり、CDATA セクション内に配置しないと、誤って XML として解釈される可能性があります。  次の XML は、コードを配置できる CDATA セクションのテンプレートです。  
  
```  
<msxsl:script implements-prefix='your-prefix' language='CSharp'>  
<![CDATA[  
// Code block.  
]]>  
</msxsl:script>  
```  
  
## スクリプト関数  
 関数は、`msxsl:script` 要素内で宣言できます。  関数が宣言されると、その関数はスクリプト ブロックに含まれます。  スタイル シートには、相互に独立して機能する複数のスクリプト ブロックを含めることができます。  このため、1 つのスクリプト ブロック内で実行しているときに、別のスクリプト ブロックで定義した関数を呼び出すことはできません。ただし、そのブロックが同じ名前空間とスクリプト言語を持つように宣言されている場合は例外です。  各スクリプト ブロックは独自の言語で記述でき、ブロックはその言語に対応するパーサーの文法規則に従って解析されるため、使われている言語の正しい構文を使用することを推奨します。  たとえば、Microsoft C\# スクリプト内では C\# のコメント構文を使用します。  
  
 関数に渡される引数と戻り値の型は任意です。  W3C XPath 型は共通言語ランタイム \(CLR\) 型のサブセットなので、XPath 型とは考えられない型については型変換が行われます。  W3C 型と等価な CLR 型を次の表に示します。  
  
|W3C 型|CLR 型|  
|-----------|-----------|  
|`String`|<xref:System.String>|  
|`Boolean`|<xref:System.Boolean>|  
|`Number`|<xref:System.Double>|  
|`Result Tree Fragment`|<xref:System.Xml.XPath.XPathNavigator>|  
|`Node Set`|<xref:System.Xml.XPath.XPathNodeIterator>|  
  
 CLR 数値型は <xref:System.Double> に変換されます。  <xref:System.DateTime> 型は <xref:System.String> に変換されます。  <xref:System.Xml.XPath.IXPathNavigable> 型は <xref:System.Xml.XPath.XPathNavigator> に変換されます。  **XPathNavigator\[\]** は <xref:System.Xml.XPath.XPathNodeIterator> に変換されます。  
  
 その他の型はエラーになります。  
  
### 名前空間とアセンブリのインポート  
 <xref:System.Xml.Xsl.XslCompiledTransform> クラスには、既定で `msxsl:script` 要素がサポートする一連のアセンブリと名前空間が事前定義されています。  しかし、アセンブリと名前空間を `msxsl:script` ブロックにインポートすることにより、事前定義の一覧にない 1 つの名前空間に属するクラスとメンバーを使用することができます。  
  
#### アセンブリ  
 次の 2 つのアセンブリは既定で参照されます。  
  
-   System.dll  
  
-   System.Xml.dll  
  
-   Microsoft.VisualBasic.dll \(スクリプト言語が VB の場合\)  
  
 `msxsl:assembly` 要素を使用して、追加のアセンブリをインポートすることができます。  これには、スタイル シートがコンパイルされたときのアセンブリも含まれます。  `msxsl:assembly` 要素は、次のように定義されます。  
  
```  
<msxsl:script>  
  <msxsl:assembly name="system.assemblyName" />  
  <msxsl:assembly href="path-name" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
 `name` 属性にはアセンブリの名前が含まれ、`href` 属性にはアセンブリへのパスが含まれます。  アセンブリの名前は "System.Data, Version\=2.0.3600.0, Culture\=neutral, PublicKeyToken\=b77a5c561934e089" などの完全な名前でも、"System.Web" などの短い名前でもかまいません。  
  
#### 名前空間  
 次の名前空間は既定で含まれます。  
  
-   システム  
  
-   System.Collection  
  
-   System.Text  
  
-   System.Text.RegularExpressions  
  
-   System.Xml  
  
-   System.Xml.Xsl  
  
-   System.Xml.XPath  
  
-   Microsoft.VisualBasic \(スクリプト言語が VB の場合\)  
  
 `namespace` 属性を使用して、追加の名前空間のサポートを追加することができます。  属性値は名前空間の名前です。  
  
```  
<msxsl:script>  
  <msxsl:using namespace="system.namespaceName" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
## 例  
 埋め込みスクリプトを使用して、半径が指定された円の円周を算出する例を次に示します。  
  
 [!code-csharp[XSLT_Script#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_Script/CS/xslt_script.cs#1)]
 [!code-vb[XSLT_Script#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_Script/VB/xslt_script.vb#1)]  
  
#### number.xml  
 [!code-xml[XSLT_Script#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/number.xml#2)]  
  
#### calc.xsl  
 [!code-xml[XSLT_Script#3](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/calc.xsl#3)]  
  
### 出力  
  
```  
<circles xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">  
  <circle>  
    <radius>12</radius>  
    <circumference>75.36</circumference>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
    <circumference>235.5</circumference>  
  </circle>  
</circles>  
```  
  
## 参照  
 [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)   
 [動的なソース コードの生成とコンパイル](../../../../docs/framework/reflection-and-codedom/dynamic-source-code-generation-and-compilation.md)