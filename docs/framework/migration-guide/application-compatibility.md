---
title: ".NET Framework のアプリケーションの互換性 | Microsoft Docs"
ms.custom: 
ms.date: 05/19/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
- app-compat
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- application compatibility
- .NET Framework application compatibility
- .NET Framework changes
caps.latest.revision: 19
ms.assetid: c4ba3ff2-fe59-4c5d-9e0b-86bba3cd865c
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: Human Translation
ms.sourcegitcommit: 3d0d39f1d6d15dc2757387ea83d3a0f868f6ec17
ms.openlocfilehash: 9169b8ec118ed0d9ab3f05eec47317cf68551754
ms.contentlocale: ja-jp
ms.lasthandoff: 05/31/2017

---

# .NET Framework のアプリケーションの互換性
<a id="application-compatibility-in-the-net-framework" class="xliff"></a>

## はじめに
<a id="introduction" class="xliff"></a>
.NET の各リリースにおいて互換性は非常に重要な目標です。 各バージョンが付加的な場合は互換性が確保され、以前のバージョンも引き続き動作します。 一方、以前の機能に変更が生じた場合 (パフォーマンスの向上、セキュリティに関する問題への対処、またはバグの修正を目的として)、既存のコードまたは既存のアプリケーションを以降のバージョンで実行すると互換性に問題が発生する可能性があります。 .NET Framework では、変更の再ターゲットとランタイムの変更点を認識します。 変更の再ターゲットは、.NET Framework の特定のバージョンをターゲットとしているもののそれ以降のバージョンで実行されるアプリケーションに影響します。 ランタイムの変更点は、特定のバージョンで実行されるすべてのアプリケーションに影響します。

各アプリは .NET Framework の特定のバージョンをターゲットとします。バージョンは次の方法で指定することができます。

* Visual Studio でターゲット フレームワークを定義する。
* プロジェクト ファイルでターゲット フレームワークを指定する。
* <xref:System.Runtime.Versioning.TargetFrameworkAttribute> をソース コードに適用する。

ターゲットに指定されたバージョンより新しいバージョンでアプリが実行されると、.NET Framework は後方互換動作によって、ターゲットに指定されている古いバージョンを模倣します。 つまり、アプリは、Framework の新しいバージョンで実行されていても、古いバージョンで実行されているように機能します。 .NET Framework のバージョン間の互換性の問題の多くは、この後方互換モデルを通して対応されます。

## ランタイムの変更
<a id="runtime-changes" class="xliff"></a>

ランタイムの問題とは、コンピューターに新しいランタイムを配置し前と同じバイナリを実行したが動作が異なっている場合に発生する問題です。 バイナリが .NET Framework 4.0 向けにコンパイルされている場合、そのバイナリは .NET Framework 4.5 以降のバージョンでは .NET Framework 4.0 互換モードで実行されます。 4.5 に影響する変更の多くは、4.0 向けにコンパイルされたバイナリには影響しません。 これは、AppDomain に固有のものであり、入力アセンブリの設定内容によって異なります。

## 変更の再ターゲット
<a id="retargeting-changes" class="xliff"></a>

再ターゲットの問題とは、4.0 をターゲットにしていたアセンブリが今度は 4.5 をターゲットにするように設定されたときに発生するものです。 この場合、アセンブリは新しい機能を選択するようになるので、古い機能との互換性の問題が発生する可能性があります。 繰り返しますが、これは入力アセンブリによって異なります。したがって、アセンブリを使用するコンソール アプリ、またはアセンブリを参照する Web サイトが該当します。

## .NET 互換性診断
<a id="net-compatibility-diagnostics" class="xliff"></a>

.NET 互換性診断は、.NET Framework のバージョン間のアプリケーション互換性問題の識別に役立つ Roslyn を使用したアナライザーです。 この一覧には、使用可能なすべてのアナライザーが含まれますが、特定の移行に適用されるのは、その一部分のみです。 アナライザーは、計画的な移行に該当する問題と表面上の問題にすぎないものを判断します。

各問題には、次の情報が含まれます。

-   以前のバージョンからの変更点の説明。

-   変更が顧客に与える影響と、バージョン間で互換性を保つための回避策があるかどうかの説明。

-   変更の重要性の評価。 アプリケーション互換性問題は、次のように分類されます。

    |   |   |
    |---|---|
    |Major|多数のアプリに影響するか、コードの大幅な変更を必要とする重大な変更。|
    |マイナー|少数のアプリに影響するか、コードにわずかな変更を加える必要がある変更。|
    |エッジ ケース|非常に限られた一般的でないシナリオでアプリに影響を与える変更。|
    |透明|アプリケーションの開発者やユーザーには大きな影響を及ぼさない変更。|

-   バージョンは、フレームワークで変更が最初に適用されたバージョンを示します。 変更によっては、特定のバージョンで導入され、それ以降のバージョンで元に戻されるものがあります。そのバージョンも同様に示されます。

-   変更の種類：

    |   |   |
    |---|---|
    |再ターゲット中|変更は、新しいバージョンの .NET Framework 向けに再コンパイルされるアプリに影響します。|
    |ランタイム|変更は、以前のバージョンの .NET Framework 向けだが、その後のバージョンでも実行する既存のアプリに影響します。|

-   影響を受ける API (ある場合)。

-   使用可能な診断の ID

## 使用方法
<a id="usage" class="xliff"></a>
開始するには、以下の中から互換性の変更の種類を選択します。

* [変更の再ターゲット](./retargeting/index.md)
* [ランタイムの変更点](./runtime/index.md)


## 関連項目
<a id="see-also" class="xliff"></a>

* [バージョンおよび依存関係](../../../docs/framework/migration-guide/versions-and-dependencies.md)
* [新機能](../../../docs/framework/whats-new/index.md)
* [クラス ライブラリの互換性のために残されている機能](../../../docs/framework/whats-new/whats-obsolete.md)

