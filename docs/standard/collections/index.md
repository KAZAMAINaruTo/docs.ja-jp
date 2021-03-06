---
title: "コレクションとデータ構造体 | Microsoft Docs"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- grouping data in collections
- objects [.NET Framework], grouping in collections
- Array class, grouping data in collections
- threading [.NET Framework], safety
- Collections classes
- collections [.NET Framework]
ms.assetid: 60cc581f-1db5-445b-ba04-a173396bf872
caps.latest.revision: 36
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: Human Translation
ms.sourcegitcommit: c50b3e328998b65ec47efe6d7457b36116813c77
ms.openlocfilehash: 27c475b8d29eb295bb3d6be24aa9ee9188f5c114
ms.contentlocale: ja-jp
ms.lasthandoff: 04/08/2017

---
# <a name="collections-and-data-structures"></a>コレクションとデータ構造体
多くの場合、類似するデータはコレクションとして格納および操作すると、より効率的に処理できます。 <xref:System.Array?displayProperty=fullName> クラス、または <xref:System.Collections>、<xref:System.Collections.Generic>、<xref:System.Collections.Concurrent>、System.Collections.Immutable の各名前空間のクラスを使って、コレクションの個々の要素または一定の範囲の要素を追加、削除、または変更することができます。  
  
 主要なコレクションの型として、ジェネリック コレクションと非ジェネリック コレクションの 2 つがあります。 ジェネリック コレクションは .NET Framework 2.0 で追加されたもので、コンパイル時にタイプ セーフなコレクションを提供します。 このため、通常、ジェネリック コレクションの方がパフォーマンスが高くなります。 ジェネリック コレクションは構築時に型パラメーターを受け取りますが、項目をコレクションに追加またはコレクションから削除するときに <xref:System.Object> 型との間でキャストする必要はありません。  また、ほとんどのジェネリック コレクションが [!INCLUDE[win8_appstore_long](../../../includes/win8-appstore-long-md.md)] アプリでサポートされています。 非ジェネリック コレクションは、項目を <xref:System.Object> として格納し、キャストが必要であり、ほとんどが [!INCLUDE[win8_appstore_long](../../../includes/win8-appstore-long-md.md)] アプリの開発でサポートされていません。 ただし、以前のコードに非ジェネリック コレクションが含まれている場合があります。  
  
 [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)] 以降では、<xref:System.Collections.Concurrent> 名前空間のコレクションによって、複数のスレッドからコレクション項目にアクセスするための効率的なスレッド セーフ操作が可能になります。 System.Collections.Immutable 名前空間の変更できないコレクション クラス ([NuGet パッケージ](https://www.nuget.org/packages/System.Collections.Immutable)) は、操作が元のコレクションのコピーで実行され、元のコレクションは変更不可能なため、本質的にスレッドセーフです。  
  
  
<a name="BKMK_Commoncollectionfeatures"></a>   
## <a name="common-collection-features"></a>一般的なコレクションの機能  
 すべてのコレクションに、コレクション内の項目を追加、削除、または検索するためのメソッドが用意されています。 また、<xref:System.Collections.ICollection> インターフェイスまたは <xref:System.Collections.Generic.ICollection%601> インターフェイスを直接または間接的に実装するすべてのコレクションで、次の機能を共有しています。  
  
-   **コレクションを列挙する機能**  
  
     .NET Framework コレクションは <xref:System.Collections.IEnumerable?displayProperty=fullName> または <xref:System.Collections.Generic.IEnumerable%601?displayProperty=fullName> のどちらかを実装して、コレクションの反復を可能にします。 列挙子は、コレクション内の任意の要素への移動可能なポインターと考えることができます。 [foreach, in](~/docs/csharp/language-reference/keywords/foreach-in.md) ステートメントと [For Each...Next Statement](~/docs/visual-basic/language-reference/statements/for-each-next-statement.md) では、<xref:System.Collections.IEnumerable.GetEnumerator%2A> メソッドによって公開される列挙子を使って、列挙子の操作の複雑さを隠しています。 また、<xref:System.Collections.Generic.IEnumerable%601?displayProperty=fullName> を実装するコレクションはいずれも "*クエリ可能型*" と見なされ、LINQ で照会できます。 LINQ クエリでは、データにアクセスするための共通パターンが提供されます。 通常、これらは、標準の `foreach` ループよりも簡潔で読みやすく、フィルター処理、並べ替え、およびグループ化の機能を利用できます。 さらに、LINQ クエリによってパフォーマンスを向上させることができます。 詳細については、「[LINQ to Objects](http://msdn.microsoft.com/library/73cafe73-37cf-46e7-bfa7-97c7eea7ced9)」、「[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)」、および「[LINQ クエリの概要 (C#)](~/docs/csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)」をご覧ください。  
  
-   **コレクションの内容を配列にコピーする機能**  
  
     **CopyTo** メソッドを使ってすべてのコレクションを配列にコピーできます。ただし、新しい配列の要素の順序は、列挙子が返す順序に基づきます。 結果の配列は、常に下限がゼロの 1 次元です。  
  
 また、多くのコレクション クラスに次の機能が含まれています。  
  
-   **容量とカウントのプロパティ**  
  
     コレクションの容量とは、そこに含むことのできる要素の数です。 コレクションのカウントとは、実際に含まれている要素の数です。 コレクションによっては、容量またはカウント、あるいはその両方を内部で処理する場合があります。  
  
     ほとんどのコレクションで、現在の容量に達すると自動的に容量が拡張されます。 メモリが再割り当てされ、要素は古いコレクションから新しいコレクションにコピーされます。 これにより、コレクションを使用するために必要なコードが削減されます。ただし、コレクションのパフォーマンスに悪影響を及ぼす可能性があります。 たとえば、<xref:System.Collections.Generic.List%601> では、<xref:System.Collections.Generic.List%601.Count%2A> が <xref:System.Collections.Generic.List%601.Capacity%2A> 未満の場合、項目の追加は O(1) 操作になります。 容量を増やして新しい要素を格納する必要がある場合、項目の追加は O(n) 操作になります。ここで、n は <xref:System.Collections.Generic.List%601.Count%2A> です。 複数の再割り当てによるパフォーマンスの低下を回避するには、初期容量をコレクションの見積もりサイズに設定しておくことが最善の方法です。  
  
     <xref:System.Collections.BitArray> は特殊なケースで、その容量はその長さと同じで、長さはそのカウントと同じです。  
  
-   **一貫した下限**  
  
     コレクションの下限とは、最初の要素のインデックスです。 <xref:System.Collections> 名前空間のすべてのインデックス付きコレクションの下限がゼロです。つまり、インデックスがゼロから始まります。 <xref:System.Array> の下限は既定でゼロですが、<xref:System.Array.CreateInstance%2A?displayProperty=fullName> を使って **Array** クラスのインスタンスを作成する場合には、別の下限を定義できます。  
  
-   **複数のスレッドからのアクセスの同期** (<xref:System.Collections> クラスのみ)。  
  
     <xref:System.Collections> 名前空間の非ジェネリック コレクション型では、同期によるスレッド セーフが提供され、通常、<xref:System.Collections.ICollection.SyncRoot%2A> メンバーと <xref:System.Collections.ICollection.IsSynchronized%2A> メンバーを介して公開されます。 既定では、これらのコレクションはスレッド セーフではありません。 拡張性が高く効率的な、コレクションへのマルチスレッド アクセスが必要な場合は、<xref:System.Collections.Concurrent> 名前空間のいずれかのクラスを使うか、変更できないコレクションを使うことを検討します。 詳しくは、「[スレッド セーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)」を参照してください。  
  
<a name="BKMK_Choosingacollection"></a>   
## <a name="choosing-a-collection"></a>コレクションの選択  
 通常は、ジェネリック コレクションを使用します。 次の表では、一般的なコレクションのシナリオとこれらのシナリオに使用できるコレクション クラスについて説明します。 ジェネリック コレクションに対する知識がない場合、このテーブルは、タスクに最適なジェネリック コレクションを選択するのに役立ちます。  
<!-- todo: All code-formatted API refs in the table need to be changed into links -->  
|やりたいこと|ジェネリック コレクションのオプション|非ジェネリック コレクションのオプション|スレッド セーフまたは変更できないコレクションのオプション|  
|-|-|-|-|  
|キーによるクイック検索用のキー/値のペアとして項目を保存する|<xref:System.Collections.Generic.Dictionary%602?displayProperty=fullName>|<xref:System.Collections.Hashtable><br /><br /> (キーのハッシュ コードに基づいて編成された、キーと値のペアのコレクション。)|<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName><br /><br /> <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602?displayProperty=fullName><br /><br /> `ImmutableDictionary(TKey, TValue) Class`|  
|インデックスで項目にアクセスする|<xref:System.Collections.Generic.List%601?displayProperty=fullName>|<xref:System.Array?displayProperty=fullName><br /><br /> <xref:System.Collections.ArrayList?displayProperty=fullName>|`ImmutableList(T) Class`<br /><br /> `ImmutableArray Class`|  
|先入れ先出し (FIFO) で項目を使用する|<xref:System.Collections.Generic.Queue%601?displayProperty=fullName>|<xref:System.Collections.Queue?displayProperty=fullName>|<xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=fullName><br /><br /> `ImmutableQueue(T) Class`|  
|後入れ先出し (LIFO) でデータを使用する|<xref:System.Collections.Generic.Stack%601?displayProperty=fullName>|<xref:System.Collections.Stack?displayProperty=fullName>|<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=fullName><br /><br /> `ImmutableStack(T) Class`|  
|項目に順次アクセスする|<xref:System.Collections.Generic.LinkedList%601?displayProperty=fullName>|推奨しません|推奨しません|  
|項目がコレクションから削除またはコレクションに追加されるときに通知を受け取る。 (<xref:System.ComponentModel.INotifyPropertyChanged> および <xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=fullName> を実装)|<xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=fullName>|推奨しません|推奨しません|  
|並べ替えられたコレクション|<xref:System.Collections.Generic.SortedList%602?displayProperty=fullName>|<xref:System.Collections.SortedList?displayProperty=fullName>|`ImmutableSortedDictionary(TKey, TValue) Class`<br /><br /> `ImmutableSortedSet(T) Class`|  
|数学関数のセット|<xref:System.Collections.Generic.HashSet%601?displayProperty=fullName><br /><br /> <xref:System.Collections.Generic.SortedSet%601?displayProperty=fullName>|推奨しません|`ImmutableHashSet(T) Class`<br /><br /> `ImmutableSortedSet(T) Class`|  
  
<a name="BKMK_RelatedTopics"></a>   
## <a name="related-topics"></a>関連トピック  
  
|タイトル|説明|  
|-----------|-----------------|  
|[コレクション クラスの選択](../../../docs/standard/collections/selecting-a-collection-class.md)|さまざまなコレクションについて説明し、いずれかのシナリオを選択できるよう支援します。|  
|[ 一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)|<xref:System.Array?displayProperty=fullName>、<xref:System.Collections.Generic.List%601?displayProperty=fullName>、<xref:System.Collections.Generic.Dictionary%602?displayProperty=fullName> などの一般的に使われるジェネリックと非ジェネリック コレクション型について説明します。|  
|[ジェネリック コレクションを使用する状況](../../../docs/standard/collections/when-to-use-generic-collections.md)|ジェネリック コレクション型の使用について説明します。|  
|[コレクション内での比較と並べ替え](../../../docs/standard/collections/comparisons-and-sorts-within-collections.md)|コレクションでの等価比較と並べ替え比較の使用について説明します。|  
|[Sorted コレクション型](../../../docs/standard/collections/sorted-collection-types.md)|並べ替えられたコレクションのパフォーマンスと特性について説明します|  
|[Hashtable コレクション型と Dictionary コレクション型](../../../docs/standard/collections/hashtable-and-dictionary-collection-types.md)|ジェネリックと非ジェネリックのハッシュをベースにしたディクショナリ型の機能について説明します。|  
|[スレッドセーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)|複数のスレッドからの安全で効率的な同時アクセスをサポートする <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=fullName> や <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=fullName> などのコレクション型について説明します。|  
|System.Collections.Immutable|変更できないコレクションを導入し、コレクション型へのリンクを提供します。|  
  
<a name="BKMK_Reference"></a>   
## <a name="reference"></a>参照  
 <xref:System.Array?displayProperty=fullName>  
  
 <xref:System.Collections?displayProperty=fullName>  
  
 <xref:System.Collections.Concurrent?displayProperty=fullName>  
  
 <xref:System.Collections.Generic?displayProperty=fullName>  
  
 <xref:System.Collections.Specialized?displayProperty=fullName>  
  
 <xref:System.Linq?displayProperty=fullName>  
  
 System.Collections.Immutable
