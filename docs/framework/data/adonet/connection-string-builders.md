---
title: "接続文字列ビルダー | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# 接続文字列ビルダー
以前のバージョンの [!INCLUDE[vstecado](../../../../includes/vstecado-md.md)] では、文字列値の連結によって構築された接続文字列がコンパイル時にはチェックされません。そのため、不適切なキーワードが使用された場合、実行時に <xref:System.ArgumentException> が発生します。  接続文字列のキーワードの構文は [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] のデータ プロバイダーごとに異なるため、有効な接続文字列を手動で作成するのが難しいという問題がありました。  この問題に対処するため、[!INCLUDE[vstecado](../../../../includes/vstecado-md.md)] 2.0 では、各 [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] データ プロバイダー用の新しい接続文字列ビルダーが導入されました。  各データ プロバイダーは、<xref:System.Data.Common.DbConnectionStringBuilder> を継承した、厳密に型指定された接続文字列ビルダー クラスを提供しています。  次の表は、各 [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] データ プロバイダーおよび対応する接続文字列ビルダー クラスの一覧です。  
  
|プロバイダー|ConnectionStringBuilder クラス|  
|------------|---------------------------------|  
|<xref:System.Data.SqlClient>|<xref:System.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=fullName>|  
|<xref:System.Data.OleDb>|<xref:System.Data.OleDb.OleDbConnectionStringBuilder?displayProperty=fullName>|  
|<xref:System.Data.Odbc>|<xref:System.Data.Odbc.OdbcConnectionStringBuilder?displayProperty=fullName>|  
|<xref:System.Data.OracleClient>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=fullName>|  
  
## 接続文字列のインジェクション攻撃  
 ユーザー入力から文字列を動的に連結することによって接続文字列を構築している場合、接続文字列のインジェクション攻撃を受ける可能性があります。  文字列の検証や悪意のある文字のエスケープを怠ると、機密データなど、サーバー上のリソースへのアクセスを攻撃者に許してしまうことも考えられます。  たとえば、セミコロンに続けて値を追加するだけでも攻撃が成立します。  接続文字列は "後勝ち" のアルゴリズムで解析されるため、悪質な入力データによって本来の値が置き換えられます。  
  
 接続文字列ビルダー クラスは推測に頼った作業を排除し、構文エラーやセキュリティ上の脆弱性を防ぐことを目的に設計されています。  このクラスには、各データ プロバイダーによってサポートされた既知のキーと値のペアに対応するプロパティおよびメソッドが存在します。  それぞれのクラスは、あらかじめ決められた一連のシノニムを管理しており、特定のシノニムを対応する既知のキー名に変換することができます。  有効なキーと値のペアに対してチェックが実行され、無効なペアが見つかると例外がスローされます。  また、挿入された値は安全な方法で処理されます。  
  
 次の例を実行すると、`Initial Catalog` の設定に対して挿入された余分な値が、<xref:System.Data.SqlClient.SqlConnectionStringBuilder> によってどのように処理されるかを確認できます。  
  
```vb  
Dim builder As New System.Data.SqlClient.SqlConnectionStringBuilder  
builder("Data Source") = "(local)"  
builder("Integrated Security") = True  
builder("Initial Catalog") = "AdventureWorks;NewValue=Bad"  
Console.WriteLine(builder.ConnectionString)  
```  
  
```csharp  
System.Data.SqlClient.SqlConnectionStringBuilder builder =  
  new System.Data.SqlClient.SqlConnectionStringBuilder();  
builder["Data Source"] = "(local)";  
builder["integrated Security"] = true;  
builder["Initial Catalog"] = "AdventureWorks;NewValue=Bad";  
Console.WriteLine(builder.ConnectionString);  
```  
  
 出力結果を見ると、挿入された値が <xref:System.Data.SqlClient.SqlConnectionStringBuilder> によって適切に処理されていることがわかります。二重引用符内の余分な値は、新しいキーと値のペアとして接続文字列に追加されるのではなくエスケープされています。  
  
```  
data source=(local);Integrated Security=True;  
initial catalog="AdventureWorks;NewValue=Bad"  
```  
  
## 構成ファイルからの接続文字列の作成  
 接続文字列の特定の要素があらかじめわかっている場合、接続文字列を構成ファイルに格納しておき、それを実行時に取得することによって完全な接続文字列を作成できます。  たとえば、サーバー名は不明でも、データベースの名前はあらかじめ把握できる場合があります。  または、ユーザーに名前とパスワードだけを実行時に指定してもらい、それ以外の値を接続文字列に挿入できないようにしたい場合もあります。  
  
 接続文字列ビルダーには、<xref:System.String> を引数として受け取るオーバーロード コンストラクターがあります。この引数に対して接続文字列を部分的に指定しておき、それ以外の部分をユーザー入力で補完することも可能です。  部分的な接続文字列は構成ファイルに保存し、実行時に取得できます。  
  
> [!NOTE]
>  構成ファイルへのプログラム アクセスは <xref:System.Configuration> 名前空間によって実現できます。Web アプリケーションの場合は <xref:System.Web.Configuration.WebConfigurationManager> を、Windows アプリケーションの場合は <xref:System.Configuration.ConfigurationManager> を使用します。  接続文字列および構成ファイルを使った作業の詳細については、「[接続文字列と構成ファイル](../../../../docs/framework/data/adonet/connection-strings-and-configuration-files.md)」を参照してください。  
  
### 例  
 この例では、接続文字列の一部を構成ファイルから取得し、<xref:System.Data.SqlClient.SqlConnectionStringBuilder> の <xref:System.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> プロパティ、<xref:System.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> プロパティ、および <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> プロパティを設定することによって接続文字列全体を作成します。  構成ファイルは次のように定義されています。  
  
```  
<connectionStrings>  
  <clear/>  
  <add name="partialConnectString"   
    connectionString="Initial Catalog=Northwind;"  
    providerName="System.Data.SqlClient" />  
</connectionStrings>  
```  
  
> [!NOTE]
>  このコードを実行するには、プロジェクトで `System.Configuration.dll` を参照設定する必要があります。  
  
 [!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/CS/source.cs#1)]
 [!code-vb[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/VB/source.vb#1)]  
  
## 参照  
 [接続文字列](../../../../docs/framework/data/adonet/connection-strings.md)   
 [プライバシーとデータ セキュリティ](../../../../docs/framework/data/adonet/privacy-and-data-security.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)