---
title: "LINQ to XML クラスの概要 (C#) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: bf666100-5392-4968-97f4-f6b9d3287d7b
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: f75a7a2aa3f9fca867562807595c6387b0be23d4
ms.lasthandoff: 03/13/2017

---
# <a name="linq-to-xml-classes-overview-c"></a>LINQ to XML クラスの概要 (C#)
このトピックでは、<xref:System.Xml.Linq> 名前空間内の [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] クラスの一覧を示し、各クラスについて簡単に説明します。  
  
## <a name="linq-to-xml-classes"></a>LINQ to XML クラス  
  
### <a name="xattribute-class"></a>XAttribute クラス  
 <xref:System.Xml.Linq.XAttribute> は、XML 属性を表します。 詳細と例については、「[XAttribute クラスの概要 (C#)](../../../../csharp/programming-guide/concepts/linq/xattribute-class-overview.md)」を参照してください。  
  
### <a name="xcdata-class"></a>XCData クラス  
 <xref:System.Xml.Linq.XCData> は、CDATA テキスト ノードを表します。  
  
### <a name="xcomment-class"></a>XComment クラス  
 <xref:System.Xml.Linq.XComment> は、XML コメントを表します。  
  
### <a name="xcontainer-class"></a>XContainer クラス  
 <xref:System.Xml.Linq.XContainer> は、子ノードを持つ可能性があるすべてのノードの抽象基本クラスです。 <xref:System.Xml.Linq.XContainer> クラスから、次のクラスが派生します。  
  
-   <xref:System.Xml.Linq.XElement>  
  
-   <xref:System.Xml.Linq.XDocument>  
  
### <a name="xdeclaration-class"></a>XDeclaration クラス  
 <xref:System.Xml.Linq.XDeclaration> は、XML 宣言を表します。 XML 宣言は、ドキュメントの XML バージョンとエンコーディングを宣言するために使用されます。 また、XML 宣言では、XML ドキュメントがスタンドアロンかどうかを指定します。 ドキュメントがスタンドアロンである場合、外部 DTD、または内部サブセットから参照されている外部パラメーター エンティティのいずれかに外部マークアップ宣言がありません。  
  
### <a name="xdocument-class"></a>XDocument クラス  
 <xref:System.Xml.Linq.XDocument> は、XML ドキュメントを表します。 詳細と例については、「[XDocument クラスの概要 (C#)](../../../../csharp/programming-guide/concepts/linq/xdocument-class-overview.md)」を参照してください。  
  
### <a name="xdocumenttype-class"></a>XDocumentType クラス  
 <xref:System.Xml.Linq.XDocumentType> は、XML ドキュメント型定義 (DTD) を表します。  
  
### <a name="xelement-class"></a>XElement クラス  
 <xref:System.Xml.Linq.XElement> は、XML 要素を表します。 詳細と例については、「[XElement クラスの概要 (C#)](../../../../csharp/programming-guide/concepts/linq/xelement-class-overview.md)」を参照してください。  
  
### <a name="xname-class"></a>XName クラス  
 <xref:System.Xml.Linq.XName> は、要素の名前 (<xref:System.Xml.Linq.XElement>) と属性 (<xref:System.Xml.Linq.XAttribute>) を表します。 詳細と例については、「[XDocument クラスの概要 (C#)](../../../../csharp/programming-guide/concepts/linq/xdocument-class-overview.md)」を参照してください。  
  
 [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] は、XML 名ができる限り単純になるように設計されています。 XML 名は、その複雑さのために XML の高度な領域として扱われることがよくあります。 この複雑さの原因として考えられるのは、開発者がプログラミングで通常使用する名前空間ではなく、名前空間プレフィックスです。 名前空間プレフィックスは、XML を入力するときに必要なキー ストロークを減らしたり、XML を読みやすくしたりするために役立ちます。 しかし、プレフィックスは、完全な XML 名前空間を使用するためのショートカットに過ぎないことが多く、必要ない場合がほとんどです。 [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] では、すべてのプレフィックスを対応する XML 名前空間に解決することで、XML 名を簡素化しています。 プレフィックスが必要な場合は、<xref:System.Xml.Linq.XElement.GetPrefixOfNamespace%2A> メソッドを介して使用できます。  
  
 必要に応じて名前空間プレフィックスを制御することができます。 XSLT や XAML などの他の XML システムを使用する場合は、状況によって名前空間プレフィックスを制御する必要があります。 たとえば、名前空間プレフィックスを使用し、XSLT スタイルシートに組み込まれている XPath 式がある場合は、XPath 式内で使用しているものと一致する名前空間プレフィックスで XML ドキュメントがシリアル化されるようにする必要があります。  
  
### <a name="xnamespace-class"></a>XNamespace クラス  
 <xref:System.Xml.Linq.XNamespace> は <xref:System.Xml.Linq.XElement> または<xref:System.Xml.Linq.XAttribute> の名前空間を表します。 名前空間は、<xref:System.Xml.Linq.XName> のコンポーネントです。  
  
### <a name="xnode-class"></a>XNode クラス  
 <xref:System.Xml.Linq.XNode> は、XML ツリーのノードを表す抽象クラスです。 <xref:System.Xml.Linq.XNode> クラスから、次のクラスが派生します。  
  
-   <xref:System.Xml.Linq.XText>  
  
-   <xref:System.Xml.Linq.XContainer>  
  
-   <xref:System.Xml.Linq.XComment>  
  
-   <xref:System.Xml.Linq.XProcessingInstruction>  
  
-   <xref:System.Xml.Linq.XDocumentType>  
  
### <a name="xnodedocumentordercomparer-class"></a>XNodeDocumentOrderComparer クラス  
 <xref:System.Xml.Linq.XNodeDocumentOrderComparer> には、ノードをドキュメント順で比較する機能が用意されています。  
  
### <a name="xnodeequalitycomparer-class"></a>XNodeEqualityComparer クラス  
 <xref:System.Xml.Linq.XNodeEqualityComparer> には、ノードを値の等価性で比較する機能が用意されています。  
  
### <a name="xobject-class"></a>XObject クラス  
 <xref:System.Xml.Linq.XObject> は、<xref:System.Xml.Linq.XNode> および <xref:System.Xml.Linq.XAttribute> の抽象基本クラスです。 このクラスには、注釈およびイベント機能が用意されています。  
  
### <a name="xobjectchange-class"></a>XObjectChange クラス  
 <xref:System.Xml.Linq.XObjectChange> は、<xref:System.Xml.Linq.XObject> に対してイベントが生成されるときのイベントの種類を指定します。  
  
### <a name="xobjectchangeeventargs-class"></a>XObjectChangeEventArgs クラス  
 <xref:System.Xml.Linq.XObjectChangeEventArgs> には、<xref:System.Xml.Linq.XObject.Changing> および <xref:System.Xml.Linq.XObject.Changed> イベントのデータが用意されています。  
  
### <a name="xprocessinginstruction-class"></a>XProcessingInstruction クラス  
 <xref:System.Xml.Linq.XProcessingInstruction> は、XML 処理命令を表します。 処理命令は、XML を処理するアプリケーションに情報を伝達します。  
  
### <a name="xtext-class"></a>XText クラス  
 <xref:System.Xml.Linq.XText> は、テキスト ノードを表します。 このクラスを使用する必要はほとんどありません。 このクラスは、主に混合コンテンツに使用されます。  
  
## <a name="see-also"></a>関連項目  
 [LINQ to XML プログラミングの概要 (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-programming-overview.md)