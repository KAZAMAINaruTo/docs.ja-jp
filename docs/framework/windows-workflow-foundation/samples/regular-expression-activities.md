---
title: "正規表現アクティビティ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b8f24694-49db-4339-92ec-014e3d4ae63b
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# 正規表現アクティビティ
このサンプルでは、<xref:System.Text.RegularExpressions> 名前空間の正規表現機能を公開する一連のアクティビティを作成する方法を示します。このカスタム アクティビティはワークフロー アプリケーション内で使用できます。正規表現[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[N: System.Text.RegularExpressions](http://go.microsoft.com/fwlink/?LinkId=150434)」の名前空間参照してください。  
  
 次の表に、このサンプルのカスタム アクティビティの詳細を示します。  
  
|アクティビティ|説明|  
|-------------|--------|  
|IsMatch|正規表現で入力文字列内に一致が見つかったかどうかを示します。|  
|Matches|入力文字列で、正規表現に一致するすべての文字列を検索し、一致した文字列をすべて返します。|  
|Replace|指定された入力文字列内で、正規表現パターンに一致する文字列を、指定された置換文字列で置き換えます。|  
  
## IsMatch  
 `IsMatch` カスタム アクティビティは、`Input` 文字列プロパティが `Pattern` プロパティで指定された正規表現で一致を見つけると、`true` を返します。このアクティビティは <xref:System.Activities.CodeActivity%601> から派生し、<xref:System.Activities.CodeActivity%601.Execute%2A> メソッド内で、<xref:System.Text.RegularExpressions.Regex.IsMatch%2A> メソッドを呼び出します。  
  
 次の表で、`IsMatch` カスタム アクティビティのプロパティと戻り値について説明します。  
  
|プロパティ値または戻り値|説明|  
|------------------|--------|  
|Pattern \(必須\)|検索に使用する正規表現。|  
|Input \(必須\)|検索対象の入力文字列。|  
|RegexOptions|[RegexOptions](http://go.microsoft.com/fwlink/?LinkId=150446) 列挙値のビット単位の OR の組み合わせ。|  
|戻り値|指定されたパターンで一致が見つかった場合は `true`、それ以外の場合は `false`。|  
  
 次のコード例は、`IsMatch` カスタム アクティビティの使用方法を示します。  
  
```  
new IsMatch  
{  
    Pattern = new InArgument<string>( @"^-?\d+(\.\d{2})?$"),  
    Input = "20.00",  
};  
  
```  
  
## Matches  
 `Matches` カスタム アクティビティは、入力文字列で、正規表現に一致するすべての文字列を検索し、一致した文字列をすべて返します。このアクティビティは <xref:System.Activities.CodeActivity%601> から派生し、<xref:System.Activities.CodeActivity%601.Execute%2A> メソッド内で、<xref:System.Text.RegularExpressions.Regex.Matches%2A> メソッドを呼び出します。  
  
 次の表で、`IsMatch` カスタム アクティビティのプロパティと戻り値について説明します。  
  
|プロパティ値または戻り値|説明|  
|------------------|--------|  
|Pattern \(必須\)|検索に使用する正規表現。|  
|Input \(必須\)|検索対象の入力文字列。|  
|RegexOptions|[RegexOptions](http://go.microsoft.com/fwlink/?LinkId=150446) 列挙値のビット単位の OR の組み合わせ。|  
|戻り値|一致する文字列のコレクションが格納された <xref:System.Text.RegularExpressions.MatchCollection>。|  
  
 次のコード例は、`Matches` カスタム アクティビティの使用方法を示します。  
  
```  
new Matches  
{  
    Pattern = @"\b(?<word>\w+)\s+(\k<word>)\b",  
    Input = "The quick brown fox  fox jumped over over the lazy dog dog.",  
};  
  
```  
  
## Replace  
 `Replace` カスタム アクティビティは入力文字列を検索し、指定された正規表現と一致するすべての文字列を 1 つの文字列で置き換えます。このアクティビティは <xref:System.Activities.CodeActivity%601> から派生し、<xref:System.Activities.CodeActivity%601.Execute%2A> メソッド内で、<xref:System.Text.RegularExpressions.Regex.Replace%2A> メソッドを呼び出します。  
  
 次の表で、`Replace` カスタム アクティビティのプロパティと戻り値について説明します。  
  
|プロパティ値または戻り値|説明|  
|------------------|--------|  
|Pattern \(必須\)|検索に使用する正規表現。|  
|Input \(必須\)|検索対象の入力文字列。|  
|Replacement|置換文字列。<br /><br /> `Replacement` が指定されると、`MatchEvaluator` プロパティは無視されます。`Replacement` または `MatchEvaluator` のどちらかのプロパティを設定する必要があります。|  
|MatchEvaluator|各一致文字列を調べ、元の一致文字列または置換文字列のどちらかを返すカスタム メソッド。<br /><br /> `Replacement` が指定されると、`MatchEvaluator` プロパティは無視されます。`Replacement` または `MatchEvaluator` のどちらかのプロパティを設定する必要があります。|  
|RegexOptions|[RegexOptions](http://go.microsoft.com/fwlink/?LinkId=150446) 列挙値のビット単位の OR の組み合わせ。|  
|戻り値|一致する文字列のコレクションが格納された <xref:System.Text.RegularExpressions.MatchCollection>。|  
  
 次のコード例は、`Replace` カスタム アクティビティの使用方法を示します。  
  
```  
// Using the replacement string.  
new Replace  
{  
    Pattern = @"\bWorld\b",  
    Input = "Hello World! This is a wonderful World",  
    Replacement = "Universe"  
};  
  
// Using a match evaluator.  
new Replace  
{  
    Pattern = new InArgument<string>(pattern),  
    Input = new InArgument<string>(input),  
    MatchEvaluator = new MatchEvaluator(CapText)                  
};  
  
```  
  
#### このサンプルを使用するには  
  
1.  [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] を使用して、RegexActivities.sln ソリューション ファイルを開きます。  
  
2.  ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。  
  
3.  ソリューションを実行するには、Ctrl キーを押しながら F5 キーを押します。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\Regex`  
  
## 参照