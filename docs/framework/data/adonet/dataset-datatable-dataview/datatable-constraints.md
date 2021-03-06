---
title: "DataTable の制約 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 27c9f2fd-f64d-4b4e-bbf6-1d24f47067cb
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# DataTable の制約
制約を使用すると、データの整合性を維持するために <xref:System.Data.DataTable> のデータを強制的に制限できます。  制約は、1 つの列または関連付けられた複数の列に対して自動的に適用される規則であり、行の値がなんらかの方法で変更されたときに実行されるアクションを決定します。  制約は、<xref:System.Data.DataSet> の  `System.Data.DataSet.EnforceConstraints` プロパティを **true** に設定したときに適用されます。  `EnforceConstraints` プロパティの設定方法のコード例については、<xref:System.Data.DataSet.EnforceConstraints%2A> のリファレンス トピックを参照してください。  
  
 ADO.NET には、<xref:System.Data.ForeignKeyConstraint> と <xref:System.Data.UniqueConstraint> の 2 種類の制約があります。  既定では、<xref:System.Data.DataRelation> を **DataSet** に追加して複数のテーブル間のリレーションシップを作成すると、この 2 種類の制約が両方とも自動的に作成されます。  ただし、リレーションの作成時に **createConstraints** \= **false** と指定することにより、この動作を無効にできます。  
  
## ForeignKeyConstraint  
 **ForeignKeyConstraint** は、関連付けられているテーブルに更新や削除を反映させる方法についての規則を適用します。  たとえば、あるテーブルの行の値が更新または削除され、その同じ値が、関連付けられている別のテーブルでも使用されている場合、関連付けられているテーブル内で実行されるアクションは **ForeignKeyConstraint** によって決定されます。  
  
 **ForeignKeyConstraint** の <xref:System.Data.ForeignKeyConstraint.DeleteRule%2A> プロパティおよび <xref:System.Data.ForeignKeyConstraint.UpdateRule%2A> プロパティは、ユーザーが関連付けられているテーブルの行を削除または更新しようとしたときに実行されるアクションを定義します。  **ForeignKeyConstraint** の **DeleteRule** プロパティおよび **UpdateRule** プロパティに使用できるさまざまな設定の説明を次の表に示します。  
  
|規則の設定|説明|  
|-----------|--------|  
|**Cascade**|関連付けられている行を削除または更新します。|  
|**SetNull**|関連付けられている行の値を **DBNull** に設定します。|  
|**SetDefault**|関連付けられている行の値を既定値に設定します。|  
|**なし**|関連付けられている行に対してアクションは実行しません。  既定値です。|  
  
 **ForeignKeyConstraint** は、関連付けられている行への変更を制限したり、反映させたりできます。  列の **ForeignKeyConstraint** に対して設定されたプロパティによっては、**DataSet** の **EnforceConstraints** プロパティが **true** である場合に親の行に対して操作を実行すると、例外が発生することがあります。  たとえば、**ForeignKeyConstraint** の **DeleteRule** プロパティが **None** の場合、子の行を持っている親の行は削除できません。  
  
 **ForeignKeyConstraint** コンストラクターを使用すると、単一列間または列配列間の外部キー制約を作成できます。  **ConstraintCollection** の 1 つである、テーブルの **Constraints** プロパティの **Add** メソッドに結果の **ForeignKeyConstraint** オブジェクトを渡します。  コンストラクター引数を **ConstraintCollection** の **Add** メソッドのいくつかのオーバーロードに渡すことにより、**ForeignKeyConstraint** を作成することもできます。  
  
 **ForeignKeyConstraint** を作成するときに、**DeleteRule** の値と **UpdateRule** の値を引数としてコンストラクターに渡したり、それらの値を次の例に示すようにプロパティとして設定したりできます。この例では、**DeleteRule** の値が **None** に設定されています。  
  
```vb  
Dim custOrderFK As ForeignKeyConstraint = New ForeignKeyConstraint("CustOrderFK", _  
  custDS.Tables("CustTable").Columns("CustomerID"), _  
  custDS.Tables("OrdersTable").Columns("CustomerID"))  
custOrderFK.DeleteRule = Rule.None    
' Cannot delete a customer value that has associated existing orders.  
custDS.Tables("OrdersTable").Constraints.Add(custOrderFK)  
  
```  
  
```csharp  
ForeignKeyConstraint custOrderFK = new ForeignKeyConstraint("CustOrderFK",  
  custDS.Tables["CustTable"].Columns["CustomerID"],   
  custDS.Tables["OrdersTable"].Columns["CustomerID"]);  
custOrderFK.DeleteRule = Rule.None;    
// Cannot delete a customer value that has associated existing orders.  
custDS.Tables["OrdersTable"].Constraints.Add(custOrderFK);  
```  
  
### AcceptRejectRule  
 行への変更は、**AcceptChanges** メソッドを使用して受け入れることができ、**DataSet**、**DataTable**、または **DataRow** の **RejectChanges** メソッドを使用してキャンセルできます。  **DataSet** に **ForeignKeyConstraints** が含まれている場合は、**AcceptChanges** メソッドまたは **RejectChanges** メソッドを呼び出すと、**AcceptRejectRule** が強制適用されます。  **ForeignKeyConstraint** の **AcceptRejectRule** プロパティは、親の行に対して **AcceptChanges** または **RejectChanges** が呼び出されたときに、子の行に対して実行されるアクションを決定します。  
  
 **AcceptRejectRule** に使用できる設定の一覧を次の表に示します。  
  
|規則の設定|説明|  
|-----------|--------|  
|**Cascade**|子の行への変更を受け入れるかまたは拒否します。|  
|**なし**|子の行に対してアクションは実行しません。  既定値です。|  
  
### 例  
 次の例では、<xref:System.Data.ForeignKeyConstraint> を作成し、<xref:System.Data.ForeignKeyConstraint.AcceptRejectRule%2A> を含む複数のプロパティを設定して、<xref:System.Data.DataTable> オブジェクトの <xref:System.Data.ConstraintCollection> に追加します。  
  
 [!code-csharp[DataWorks Data.AcceptRejectRule#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.AcceptRejectRule/CS/source.cs#1)]
 [!code-vb[DataWorks Data.AcceptRejectRule#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.AcceptRejectRule/VB/source.vb#1)]  
  
## UniqueConstraint  
 **UniqueConstraint** オブジェクトは、**DataTable** 内の 1 つの列または列の配列に対して割り当てることができ、指定された列内のすべてのデータが行ごとに一意になるようにします。  **UniqueConstraint** コンストラクターを使用して、1 つの列または列の配列に対する UNIQUE 制約を作成できます。  **ConstraintCollection** の 1 つである、テーブルの **Constraints** プロパティの **Add** メソッドに結果の **UniqueConstraint** オブジェクトを渡します。  コンストラクター引数を **ConstraintCollection** の **Add** メソッドのいくつかのオーバーロードに渡すことにより、**UniqueConstraint** を作成することもできます。  1 つの列または複数の列に対して **UniqueConstraint** を作成するときは、オプションで、その列または複数の列を主キーにするかどうかを指定できます。  
  
 列の **Unique** プロパティを **true** に設定することにより、1 つの列に対する UNIQUE 制約を作成することもできます。  また、1 つの列の **Unique** プロパティを **false** に設定することにより、既存の UNIQUE 制約を削除できます。  1 つの列 \(または複数の列\) をテーブルの主キーとして定義すると、指定した列 \(または複数の列\) の UNIQUE 制約が自動的に作成されます。  **DataTable** の **PrimaryKey** プロパティから列を削除すると、**UniqueConstraint** が削除されます。  
  
 **DataTable** の 2 つの列の **UniqueConstraint** を作成する例を次に示します。  
  
```vb  
Dim custTable As DataTable = custDS.Tables("Customers")  
Dim custUnique As UniqueConstraint = _  
    New UniqueConstraint(New DataColumn()   {custTable.Columns("CustomerID"), _  
    custTable.Columns("CompanyName")})  
custDS.Tables("Customers").Constraints.Add(custUnique)  
  
```  
  
```csharp  
DataTable custTable = custDS.Tables["Customers"];  
UniqueConstraint custUnique = new UniqueConstraint(new DataColumn[]   
    {custTable.Columns["CustomerID"],   
    custTable.Columns["CompanyName"]});  
custDS.Tables["Customers"].Constraints.Add(custUnique);  
```  
  
## 参照  
 <xref:System.Data.DataRelation>   
 <xref:System.Data.DataTable>   
 <xref:System.Data.ForeignKeyConstraint>   
 <xref:System.Data.UniqueConstraint>   
 [DataTable スキーマの定義](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/datatable-schema-definition.md)   
 [DataSets、DataTables、および DataViews](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/index.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)