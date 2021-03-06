---
title: "C# アプリケーションの配置 | Microsoft Docs"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
- CSharp
helpviewer_keywords:
- deploying applications [C#]
- Visual C#, deployment
- C# language, deployment
- application deployment [C#]
ms.assetid: 5c561a21-cc5b-4180-8042-391062af0015
caps.latest.revision: 9
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
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: cc3aa401ae0f44fe7904f1d426ce0051c452078b
ms.lasthandoff: 03/13/2017

---
# <a name="deployment-of-c-applications"></a>C# アプリケーションの配置
C# アプリケーションのビルドが完了したら、それを配布するのが次のステップになります。 C# は .NET 言語であるため、C# の実行可能ファイルを他のコンピューターに配布するには、配布先の各コンピューターに .NET Framework が (場合によっては、アプリケーションに固有の他の依存関係も含めて) インストールされている必要があります。 .NET Framework を配布する方法としては、さまざまな選択肢が用意されています。 概要については、「[配置ガイド (開発者向け)](https://msdn.microsoft.com/library/ee942965)」を参照してください。  
  
 一般的に、完成したアプリケーションを他のコンピューターに移行することを "配置" と呼びます。 Microsoft の開発環境では、配置のメカニズムが用意されています。詳細については、「[アプリケーションとコンポーネントの配置](https://docs.microsoft.com/visualstudio/deployment/deploying-applications-services-and-components)」を参照してください。  
  
 主にコマンドラインからビルドおよび配布作業を行う場合は、アプリケーションの配置と依存関係の再配布について、方法を別途検討する必要があります。  
  
## <a name="see-also"></a>関連項目  
 [csc.exe を使用したコマンド ラインからのビルド](command-line-building-with-csc-exe.md)
