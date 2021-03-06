---
title: "方法 : フェデレーション サービスで資格情報を設定する | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "フェデレーション"
  - "WCF, フェデレーション"
ms.assetid: 149ab165-0ef3-490a-83a9-4322a07bd98a
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# 方法 : フェデレーション サービスで資格情報を設定する
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] でフェデレーション サービスを作成するには、大きく分けると次のような手順があります。  
  
1.  <xref:System.ServiceModel.WSFederationHttpBinding> または同様のカスタム バインディングの構成。適切なバインディングの作成[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[方法 : WSFederationHttpBinding を作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)」を参照してください。  
  
2.  サービスに提示される発行済みトークンの認証方法を制御する <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> の構成。  
  
 このトピックでは、2 番目の手順について詳しく説明します。フェデレーション サービスのしくみ[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[フェデレーション](../../../../docs/framework/wcf/feature-details/federation.md)」を参照してください。  
  
### コードで IssuedTokenServiceCredential のプロパティを設定するには  
  
1.  <xref:System.ServiceModel.Description.ServiceCredentials> クラスの <xref:System.ServiceModel.Description.ServiceCredentials.IssuedTokenAuthentication%2A> プロパティを使用して、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential> インスタンスへの参照を返します。このプロパティは、<xref:System.ServiceModel.ServiceHostBase> クラスの <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> プロパティからアクセスされます。  
  
2.  [!INCLUDE[infocard](../../../../includes/infocard-md.md)] カードのように自己発行されるトークンを認証する場合は、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> プロパティを `true` に設定します。既定値は `false` です。  
  
3.  <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> プロパティによって返されるコレクションに <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> クラスのインスタンスを設定します。各インスタンスは、サービスが認証を行うトークンの発行者を表します。  
  
    > [!NOTE]
    >  <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> プロパティによって返されるクライアント側のコレクションとは異なり、既知の証明書コレクションはキー付きのコレクションではありません。指定した証明書が発行するトークンは、発行済みトークンを含むメッセージを送信したクライアントのアドレスとは無関係に、サービスによって受け入れられます \(その他の制約については以下で説明します\)。  
  
4.  <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> プロパティを <xref:System.ServiceModel.Security.X509CertificateValidationMode> 列挙値のいずれかに設定します。これは、コードでのみ設定することができます。既定値は <xref:System.IdentityModel.Selectors.X509CertificateValidator.ChainTrust%2A> です。  
  
5.  <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> プロパティが <xref:System.ServiceModel.Security.X509CertificateValidationMode> に設定されている場合、カスタム <xref:System.IdentityModel.Selectors.X509CertificateValidator> クラスのインスタンスを <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CustomCertificateValidator%2A> プロパティに割り当てます。  
  
6.  <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> が `ChainTrust` または `PeerOrChainTrust` に設定されている場合、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.RevocationMode%2A> プロパティを <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> 列挙値にある適切な値に設定します。失効モードは `PeerTrust` または `Custom` 検証モードでは使用されないことに注意してください。  
  
7.  必要に応じて、カスタム <xref:System.IdentityModel.Tokens.SamlSerializer> クラスのインスタンスを <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.SamlSerializer%2A> プロパティに割り当てます。カスタム SAML \(Security Assertions Markup Language\) アサーションの解析などには、カスタム SAML シリアライザーが必要です。  
  
### 構成で IssuedTokenServiceCredential のプロパティを設定するには  
  
1.  \<`serviceCredentials`\> 要素の子として `<issuedTokenAuthentication>` 要素を作成します。  
  
2.  [!INCLUDE[infocard](../../../../includes/infocard-md.md)] カードのように自己発行されるトークンを認証する場合は、`<issuedTokenAuthentication>` 要素の `allowUntrustedRsaIssuers` 属性を `true` に設定します。  
  
3.  `<issuedTokenAuthentication>` 要素の子要素として `<knownCertificates>` 要素を作成します。  
  
4.  `<knownCertificates>` 要素の子要素として 0 個以上の `<add>` 要素を作成し、`storeLocation`、`storeName`、`x509FindType`、および `findValue` 属性を使用して証明書の検索方法を指定します。  
  
5.  必要に応じて、\<`issuedTokenAuthentication`\> 要素の `samlSerializer` 属性をカスタム <xref:System.IdentityModel.Tokens.SamlSerializer> クラスの型名に設定します。  
  
## 使用例  
 次の例では、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential> のプロパティをコードで設定しています。  
  
 [!code-csharp[C_FederatedService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federatedservice/cs/source.cs#2)]
 [!code-vb[C_FederatedService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federatedservice/vb/source.vb#2)]  
  
 クライアントがフェデレーション サービスによって認証されるためには、発行済みトークンが次のことを満たす必要があります。  
  
-   発行済みトークンのデジタル署名が RSA セキュリティ キー識別子を使用している場合、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> プロパティを `true` にする必要があります。  
  
-   発行済みトークンの署名に X.509 発行者シリアル番号、X.509 サブジェクト キー識別子、または X.509 拇印セキュリティ識別子が使用されている場合、発行済みトークンは <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> クラスの <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> プロパティによって返されたコレクションにある証明書で署名されている必要があります。  
  
-   発行済みトークンが X.509 証明書を使用して署名されている場合、証明書が <xref:System.IdentityModel.Tokens.X509RawDataKeyIdentifierClause> として証明書利用者に送信されたか、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> プロパティから取得されたかに関係なく、<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> プロパティの値で指定されるセマンティックスごとに証明書を検証する必要があります。X.509 証明書の検証[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
 たとえば、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> を <xref:System.ServiceModel.Security.X509CertificateValidationMode> に設定すると、その署名の証明書が `TrustedPeople` 証明書ストアに格納されている任意の発行済みトークンが認証されます。この場合、<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.TrustedStoreLocation%2A> プロパティを <xref:System.Security.Cryptography.X509Certificates.StoreLocation> または <xref:System.Security.Cryptography.X509Certificates.StoreLocation> に設定します。<xref:System.ServiceModel.Security.X509CertificateValidationMode> を含めて、他のモードを選択できます。`Custom` を選択した場合、<xref:System.IdentityModel.Selectors.X509CertificateValidator> クラスのインスタンスを <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CustomCertificateValidator%2A> プロパティに割り当てる必要があります。カスタム検証では、任意の基準を使用して証明書を検証できます。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][方法 : カスタム証明書検証を使用するサービスを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md).  
  
## 参照  
 [フェデレーション](../../../../docs/framework/wcf/feature-details/federation.md)   
 [フェデレーションと信頼](../../../../docs/framework/wcf/feature-details/federation-and-trust.md)   
 [フェデレーション サンプル](../../../../docs/framework/wcf/samples/federation-sample.md)   
 [方法 : WSFederationHttpBinding のセキュリティで保護されたセッションを無効にする](../../../../docs/framework/wcf/feature-details/how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)   
 [方法 : WSFederationHttpBinding を作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)   
 [方法 : フェデレーション クライアントを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-federated-client.md)   
 [証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)   
 [SecurityBindingElement 認証モード](../../../../docs/framework/wcf/feature-details/securitybindingelement-authentication-modes.md)