---
title: "方法: XML ファイルにオブジェクト データを書き込む (C#) | Microsoft Docs"
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
ms.assetid: 7681eb98-703d-4005-a369-26a7bca0f894
caps.latest.revision: 4
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 197e91be6d3785e437cb33541b2b4c9b4a2cbb84
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-write-object-data-to-an-xml-file-c"></a>方法: XML ファイルにオブジェクト データを書き込む (C#)
<xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、クラスから XML ファイルにオブジェクトを書き込む例を次に示します。  
  
## <a name="example"></a>例  
  
```csharp  
public class XMLWrite  
{  
  
   static void Main(string[] args)  
    {  
        WriteXML();  
    }  
  
    public class Book  
    {  
        public String title;   
    }  
  
    public static void WriteXML()  
    {  
        Book overview = new Book();  
        overview.title = "Serialization Overview";  
        System.Xml.Serialization.XmlSerializer writer =   
            new System.Xml.Serialization.XmlSerializer(typeof(Book));  
  
        var path = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) + "//SerializationOverview.xml";  
        System.IO.FileStream file = System.IO.File.Create(path);  
  
        writer.Serialize(file, overview);  
        file.Close();  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 クラスには、パラメーターのないパブリック コンストラクターが必要です。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
-   シリアル化されるクラスにパブリックなパラメーターなしのコンストラクターがない場合  
  
-   ファイルが存在し、読み取り専用である場合 (<xref:System.IO.IOException>)  
  
-   パスが長すぎる場合 (<xref:System.IO.PathTooLongException>)  
  
-   ディスクの空き領域がない場合 (<xref:System.IO.IOException>)  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 次のコード例では、ファイルが存在しない場合は新規にファイルを作成します。 アプリケーションでファイルを作成する必要がある場合、そのアプリケーションにはフォルダーに対する `Create` アクセスが必要です。 ファイルが既に存在する場合、アプリケーションに必要なのは、より低い権限である `Write` アクセスだけです。 フォルダーに対して `Read` アクセスを許可するのではなく、可能な限りアプリケーションの配置時にファイルを作成しておき、1 つのファイルに対してのみ `Create` アクセスを許可する方が安全です。  
  
## <a name="see-also"></a>関連項目  
 <xref:System.IO.StreamWriter>   
 [方法: XML ファイルからオブジェクト データを読み込む (C#)](../../../../csharp/programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md)   
 [シリアル化 (C#)](../../../../csharp/programming-guide/concepts/serialization/index.md)
