---
title: "How to: Compile Conditionally with Trace and Debug | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "trace compiler options"
  - "trace statements"
  - "compiling source code, trace statements"
  - "tracing [.NET Framework], enabling or disabling"
  - "tracing [.NET Framework], compiling conditionally"
  - "TRACE directive"
  - "conditional compilation, tracing code"
ms.assetid: 56d051c3-012c-42c1-9a58-7270edc624aa
caps.latest.revision: 11
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 11
---
# How to: Compile Conditionally with Trace and Debug
開発時にアプリケーションをデバッグするときは、トレース出力とデバッグ出力の両方が Visual Studio の \[出力\] ウィンドウに表示されます。  しかし、配置されるアプリケーションにトレース機能を組み込むには、**TRACE** コンパイラ ディレクティブを有効にして、インストルメント化されたアプリケーションをコンパイルする必要があります。  これにより、コンパイルされたアプリケーションのリリース バージョンに、トレース コードが組み込まれます。  **TRACE** ディレクティブを有効にしないと、コンパイル時にすべてのトレース コードが無視され、配置する実行可能コードに含まれなくなります。  
  
 トレース用のメソッドとデバッグ用のメソッドにはどちらも、関連付けられた条件属性があります。  たとえば、トレースの条件属性が **true** の場合は、すべてのトレース ステートメントがアセンブリ \(コンパイル済みの .exe ファイルや .dll ファイル\) に組み込まれます。また、**Trace** の条件属性が **false** の場合、トレース ステートメントはアセンブリに組み込まれません。  
  
 ビルドでは、**Trace** 条件属性と **Debug** 条件属性のどちらか一方をオンにすること、両方ともオンにすること、あるいは両方ともオフにすることのいずれも可能です。  つまり、**Debug** のみオン、**Trace** のみオン、両方ともオン、両方ともオフという、4 種類のビルド方法が存在します。  実際の配置用のリリース ビルドでは両方ともオフにする場合もありますが、大半のデバッグ ビルドでは両方ともオンにします。  
  
 アプリケーションのコンパイラ設定は、次に示すいくつかの方法で指定できます。  
  
-   プロパティ ページ  
  
-   コマンド ライン  
  
-   **\#CONST** \(Visual Basic の場合\) および **\#define** \(C\# の場合\)  
  
### プロパティ ページのダイアログ ボックスでコンパイル設定を変更するには  
  
1.  **ソリューション エクスプローラー**で、プロジェクト ノードを右クリックします。  
  
2.  ショートカット メニューの **\[プロパティ\]** を選択します。  
  
    -   Visual Basic では、プロパティ ページの左ペインで **\[コンパイル\]** タブをクリックし、**\[詳細コンパイル オプション\]** ボタンをクリックして **\[コンパイラの詳細設定\]** ダイアログ ボックスを表示します。  有効にするコンパイラ設定のチェック ボックスをオンにします。  無効にする設定のチェック ボックスをオフにします。  
  
    -   C\# では、プロパティ ページの左ペインで **\[ビルド\]** タブをクリックし、有効にするコンパイラ設定のチェック ボックスをオンにします。  無効にする設定のチェック ボックスをオフにします。  
  
### インストルメント化されたコードをコマンド ラインを使用してコンパイルするには  
  
1.  コマンド ラインで、条件付きコンパイラ スイッチを設定します。  コンパイラにより、トレース コードまたはデバッグ コードが実行可能ファイルに組み込まれます。  
  
     たとえば、コマンド ラインで次のコンパイラ命令を入力すると、コンパイルされた実行可能ファイルにトレース コードが組み込まれます。  
  
     Visual Basic の場合: **vbc \/r:System.dll \/d:TRACE\=TRUE \/d:DEBUG\=FALSE MyApplication.vb**  
  
     C\# の場合: **csc \/r:System.dll \/d:TRACE \/d:DEBUG\=FALSE MyApplication.cs**  
  
    > [!TIP]
    >  複数のアプリケーション ファイルをコンパイルするには、各ファイル名の間にスペースを 1 つ挿入します。たとえば、「**MyApplication1.vb MyApplication2.vb MyApplication3.vb**」または「**MyApplication1.cs MyApplication2.cs MyApplication3.cs**」とします。  
  
     上記の例で使用した条件付きコンパイルのディレクティブの意味は次のとおりです。  
  
    |ディレクティブ|説明|  
    |-------------|--------|  
    |`vbc`|Visual Basic コンパイラ|  
    |`csc`|C\# コンパイラ|  
    |`/r:`|外部アセンブリ \(EXE または DLL\) を参照します。|  
    |`/d:`|条件付きコンパイル シンボルを定義します。|  
  
    > [!NOTE]
    >  TRACE または DEBUG は大文字で入力する必要があります。  条件付きコンパイル コマンドの詳細情報を参照するには、コマンド プロンプトに `vbc /?` \(Visual Basic の場合\) または `csc /?` \(c\# の場合\) と入力します。  詳細については、「[コマンド ラインからのビルド](../Topic/How%20to:%20Set%20Environment%20Variables%20for%20the%20Visual%20Studio%20Command%20Line.md)」\(C\# の場合\) または「[コマンド ライン コンパイラの起動](../Topic/How%20to:%20Invoke%20the%20Command-Line%20Compiler%20\(Visual%20Basic\).md)」\(Visual Basic の場合\) を参照してください。  
  
### \#CONST または \#define を使用して条件付きコンパイルを実行するには  
  
1.  ソース コード ファイルの先頭に、使用するプログラミング言語に該当するステートメントを入力します。  
  
    |言語|ステートメント|結果|  
    |--------|-------------|--------|  
    |**Visual Basic**|**\#CONST TRACE \= true**|トレースを有効にします。|  
    ||**\#CONST TRACE \= false**|トレースを無効にします。|  
    ||**\#CONST DEBUG \= true**|デバッグを有効にします。|  
    ||**\#CONST DEBUG \= false**|デバッグを無効にします。|  
    |**C\#**|**\#define TRACE**|トレースを有効にします。|  
    ||**\#undef TRACE**|トレースを無効にします。|  
    ||**\#define DEBUG**|デバッグを有効にします。|  
    ||**\#undef DEBUG**|デバッグを無効にします。|  
  
### トレースまたはデバッグを無効にするには  
  
1.  ソース コードからコンパイラ ディレクティブを削除します。  
  
     または  
  
2.  コンパイラ ディレクティブをコメント アウトします。  
  
    > [!NOTE]
    >  コンパイルの準備が整ったら、**\[ビルド\]** メニューの **\[ビルド\]** を選択します。または、条件付きコンパイル シンボルを定義するための「**d:**」を入力せずにコマンド ライン メソッドを使用することもできます。  
  
## 参照  
 [Tracing and Instrumenting Applications](../../../docs/framework/debug-trace-profile/tracing-and-instrumenting-applications.md)   
 [How to: Create, Initialize and Configure Trace Switches](../../../docs/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches.md)   
 [Trace Switches](../../../docs/framework/debug-trace-profile/trace-switches.md)   
 [Trace Listeners](../../../docs/framework/debug-trace-profile/trace-listeners.md)   
 [How to: Add Trace Statements to Application Code](../../../docs/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code.md)   
 [How to: Set Environment Variables for the Visual Studio Command Line](../Topic/How%20to:%20Set%20Environment%20Variables%20for%20the%20Visual%20Studio%20Command%20Line.md)   
 [How to: Invoke the Command\-Line Compiler](../Topic/How%20to:%20Invoke%20the%20Command-Line%20Compiler%20\(Visual%20Basic\).md)