---
title: Como criar um cliente federado
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 56ece47e-98bf-4346-b92b-fda1fc3b4d9c
ms.openlocfilehash: 47e59452edfff74daf17d94a058ce8b12af7867c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593536"
---
# <a name="how-to-create-a-federated-client"></a>Como criar um cliente federado
No Windows Communication Foundation (WCF), a criação de um cliente para um *Serviço Federado* consiste em três etapas principais:  
  
1. Configure uma [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) associação personalizada ou semelhante. Para obter mais informações sobre como criar uma associação apropriada, consulte [como: criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md). Como alternativa, execute a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) em relação ao ponto de extremidade de metadados do serviço federado para gerar um arquivo de configuração para comunicação com o serviço federado e um ou mais serviços de token de segurança.  
  
2. Defina as propriedades do <xref:System.ServiceModel.Security.IssuedTokenClientCredential> que controlam vários aspectos da interação de um cliente com um serviço de token de segurança.  
  
3. Defina as propriedades do <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> , que permite que os certificados sejam necessários para se comunicar com segurança com determinados pontos de extremidade, como serviços de token de segurança.  
  
> [!NOTE]
> Um <xref:System.Security.Cryptography.CryptographicException> pode ser gerado quando um cliente usa credenciais representadas, a <xref:System.ServiceModel.WSFederationHttpBinding> associação ou um token emitido por padrão e chaves assimétricas. As chaves assimétricas são usadas com os <xref:System.ServiceModel.WSFederationHttpBinding> tokens de associação e emitidos personalizados quando as <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedKeyType%2A> <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.KeyType%2A> Propriedades e, respectivamente, são definidas como <xref:System.IdentityModel.Tokens.SecurityKeyType.AsymmetricKey> . O <xref:System.Security.Cryptography.CryptographicException> é gerado quando o cliente tenta enviar uma mensagem e um perfil de usuário não existe para a identidade que o cliente está representando. Para atenuar esse problema, faça logon no computador cliente ou ligue `LoadUserProfile` antes de enviar a mensagem.  
  
 Este tópico fornece informações detalhadas sobre esses procedimentos. Para obter mais informações sobre como criar uma associação apropriada, consulte [como: criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md). Para obter mais informações sobre como um serviço federado funciona, consulte [Federação](federation.md).  
  
### <a name="to-generate-and-examine-the-configuration-for-a-federated-service"></a>Para gerar e examinar a configuração de um serviço federado  
  
1. Execute a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) com o endereço da URL de metadados do serviço como um parâmetro de linha de comando.  
  
2. Abra o arquivo de configuração gerado em um editor apropriado.  
  
3. Examine os atributos e o conteúdo de quaisquer [\<issuer>](../../configure-apps/file-schema/wcf/issuer.md) elementos e gerados [\<issuerMetadata>](../../configure-apps/file-schema/wcf/issuermetadata.md) . Eles estão localizados dentro dos [\<security>](../../configure-apps/file-schema/wcf/security-of-wsfederationhttpbinding.md) elementos para os [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) elementos de associações personalizados ou. Verifique se os endereços contêm os nomes de domínio esperados ou outras informações de endereço. É importante verificar essas informações porque o cliente autentica esses endereços e pode divulgar informações como pares de nome de usuário/senha. Se o endereço não for o endereço esperado, isso poderá resultar na divulgação de informações para um destinatário indesejado.  
  
4. Examine os [\<issuedTokenParameters>](../../configure-apps/file-schema/wcf/issuedtokenparameters.md) elementos adicionais dentro do elemento comentado <`alternativeIssuedTokenParameters`>. Ao usar a ferramenta svcutil. exe para gerar a configuração de um serviço federado, se o serviço federado ou quaisquer serviços de token de segurança intermediários não especificarem um endereço de emissor, mas, em vez disso, especificar um endereço de metadados para um serviço de token de segurança que exponha vários pontos de extremidade, o arquivo de configuração resultante se referirá ao primeiro. Os pontos de extremidade adicionais estão no arquivo de configuração como comentado <`alternativeIssuedTokenParameters` elementos de>.  
  
     Determine se um desses <`issuedTokenParameters`> é preferível para aquele já presente na configuração. Por exemplo, o cliente pode preferir autenticar para um serviço de token de segurança usando um token do Windows CardSpace em vez de um par de nome de usuário/senha.  
  
    > [!NOTE]
    > Onde vários serviços de token de segurança devem ser percorridos antes de se comunicar com o serviço, é possível que um serviço de token de segurança intermediário direcione o cliente para um serviço de token de segurança incorreto. Portanto, verifique se o ponto de extremidade do serviço de token de segurança no [\<issuedTokenParameters>](../../configure-apps/file-schema/wcf/issuedtokenparameters.md) é o serviço de token de segurança esperado e não um serviço de token de segurança desconhecido.  
  
### <a name="to-configure-an-issuedtokenclientcredential-in-code"></a>Para configurar um IssuedTokenClientCredential no código  
  
1. Acesse o <xref:System.ServiceModel.Security.IssuedTokenClientCredential> por meio da <xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A> propriedade da <xref:System.ServiceModel.Description.ClientCredentials> classe (retornada pela <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade da <xref:System.ServiceModel.ClientBase%601> classe ou por meio da <xref:System.ServiceModel.ChannelFactory> classe), conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[c_CreateSTS#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#9)]
     [!code-vb[c_CreateSTS#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#9)]  
  
2. Se o cache de token não for necessário, defina a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.CacheIssuedTokens%2A> propriedade como `false` . A <xref:System.ServiceModel.Security.IssuedTokenClientCredential.CacheIssuedTokens%2A> propriedade controla se esses tokens de um serviço de token de segurança estão armazenados em cache. Se essa propriedade for definida como `false` , o cliente solicitará um novo token do serviço de token de segurança sempre que ele precisar se autenticar novamente no serviço federado, independentemente de um token anterior ainda ser válido. Se essa propriedade for definida como `true` , o cliente reutiliza um token existente sempre que ele precisar se autenticar novamente no serviço federado (desde que o token não tenha expirado). O padrão é `true`.  
  
3. Se um limite de tempo for necessário em tokens em cache, defina a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.MaxIssuedTokenCachingTime%2A> propriedade como um <xref:System.TimeSpan> valor. A propriedade especifica por quanto tempo um token pode ser armazenado em cache. Depois que o período de tempo especificado tiver decorrido, o token será removido do cache do cliente. Por padrão, os tokens são armazenados em cache indefinidamente. O exemplo a seguir define o período de tempo como 10 minutos.  
  
     [!code-csharp[c_CreateSTS#15](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#15)]
     [!code-vb[c_CreateSTS#15](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#15)]  
  
4. Opcional. Defina <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuedTokenRenewalThresholdPercentage%2A> como um percentual. O padrão é 60 (porcentagem). A propriedade especifica uma porcentagem do período de validade do token. Por exemplo, se o token emitido for válido por 10 horas e <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuedTokenRenewalThresholdPercentage%2A> for definido como 80, o token será renovado após oito horas. O exemplo a seguir define o valor como 80 por cento.  
  
     [!code-csharp[c_CreateSTS#16](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#16)]
     [!code-vb[c_CreateSTS#16](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#16)]  
  
     O intervalo de renovação determinado pelo período de validade do token e o `IssuedTokenRenewalThresholdPercentage` valor é substituído pelo `MaxIssuedTokenCachingTime` valor nos casos em que o tempo de cache é menor do que o tempo limite de renovação. Por exemplo, se o produto de `IssuedTokenRenewalThresholdPercentage` e a duração do token forem de oito horas e o `MaxIssuedTokenCachingTime` valor for de 10 minutos, o cliente entrará em contato com o serviço de token de segurança para obter um token atualizado a cada 10 minutos.  
  
5. Se um modo de entropia de chave diferente de <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.CombinedEntropy> for necessário em uma associação que não usa segurança de mensagem ou segurança de transporte com credenciais de mensagem (por exemplo. a associação não tem um <xref:System.ServiceModel.Channels.SecurityBindingElement> ), defina a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> propriedade como um valor apropriado. O modo de *entropia* determina se as chaves simétricas podem ser controladas usando a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> propriedade. Esse padrão é <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.CombinedEntropy> , em que o cliente e o emissor do token fornecem dados que são combinados para produzir a chave real. Outros valores são <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.ClientEntropy> e <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.ServerEntropy> , o que significa que a chave inteira é especificada pelo cliente ou pelo servidor, respectivamente. O exemplo a seguir define a propriedade para usar apenas os dados do servidor para a chave.  
  
     [!code-csharp[c_CreateSTS#17](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#17)]
     [!code-vb[c_CreateSTS#17](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#17)]  
  
    > [!NOTE]
    > Se um <xref:System.ServiceModel.Channels.SecurityBindingElement> estiver presente em um serviço de token de segurança ou associação de serviço, o <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> conjunto ativado <xref:System.ServiceModel.Security.IssuedTokenClientCredential> será substituído pela <xref:System.ServiceModel.Channels.SecurityBindingElement.KeyEntropyMode%2A> Propriedade do `SecurityBindingElement` .  
  
6. Configure qualquer comportamento de ponto de extremidade específico do emissor adicionando-os à coleção retornada pela <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuerChannelBehaviors%2A> propriedade.  
  
     [!code-csharp[c_CreateSTS#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#14)]
     [!code-vb[c_CreateSTS#14](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#14)]  
  
### <a name="to-configure-the-issuedtokenclientcredential-in-configuration"></a>Para configurar o IssuedTokenClientCredential na configuração  
  
1. Crie um [\<issuedToken>](../../configure-apps/file-schema/wcf/issuedtoken.md) elemento como um filho do [\<issuedToken>](../../configure-apps/file-schema/wcf/issuedtoken.md) elemento em um comportamento de ponto de extremidade.  
  
2. Se o cache de token não for necessário, defina o `cacheIssuedTokens` atributo (do `issuedToken` elemento <>) como `false` .  
  
3. Se um limite de tempo for necessário em tokens em cache, defina o `maxIssuedTokenCachingTime` atributo no `issuedToken` elemento <> como um valor apropriado. Por exemplo:  
    `<issuedToken maxIssuedTokenCachingTime='00:10:00' />`  
  
4. Se um valor diferente do padrão for preferencial, defina o `issuedTokenRenewalThresholdPercentage` atributo no `issuedToken` elemento <> como um valor apropriado, por exemplo:  
  
    ```xml  
    <issuedToken issuedTokenRenewalThresholdPercentage = "80" />  
    ```  
  
5. Se um modo de entropia de chave diferente de `CombinedEntropy` estiver em uma associação que não usa segurança de mensagem ou segurança de transporte com credenciais de mensagem (por exemplo, a associação não tem um `SecurityBindingElement` ), defina o `defaultKeyEntropyMode` atributo no `<issuedToken>` elemento como um `ServerEntropy` ou `ClientEntropy` como obrigatório.  
  
    ```xml  
    <issuedToken defaultKeyEntropyMode = "ServerEntropy" />  
    ```  
  
6. Opcional. Configure qualquer comportamento de ponto de extremidade personalizado específico do emissor criando um `issuerChannelBehaviors` elemento <> como um filho do `issuedToken` elemento <>. Para cada comportamento, crie um `add` elemento <> como um filho do elemento <`issuerChannelBehaviors`>. Especifique o endereço do emissor do comportamento definindo o `issuerAddress` atributo no `add` elemento <>. Especifique o próprio comportamento definindo o `behaviorConfiguration` atributo no `add` elemento <>.  
  
    ```xml  
    <issuerChannelBehaviors>  
    <add issuerAddress="http://fabrikam.org/sts" behaviorConfiguration="FabrikamSTS" />  
    </issuerChannelBehaviors>  
    ```  
  
### <a name="to-configure-an-x509certificaterecipientclientcredential-in-code"></a>Para configurar um X509CertificateRecipientClientCredential no código  
  
1. Acesse o <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> por meio da <xref:System.ServiceModel.Description.ClientCredentials.ServiceCertificate%2A> propriedade da <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade da <xref:System.ServiceModel.ClientBase%601> classe ou da <xref:System.ServiceModel.ChannelFactory> propriedade.  
  
     [!code-csharp[c_CreateSTS#18](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#18)]
     [!code-vb[c_CreateSTS#18](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#18)]  
  
2. Se uma <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> instância estiver disponível para o certificado para um determinado ponto de extremidade, use o <xref:System.Collections.Generic.ICollection%601.Add%2A> método da coleção retornada pela <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> propriedade.  
  
     [!code-csharp[c_CreateSTS#19](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#19)]
     [!code-vb[c_CreateSTS#19](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#19)]  
  
3. Se uma <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> instância não estiver disponível, use o <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetScopedCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> classe, conforme mostrado no exemplo a seguir.  
  
     [!code-csharp[c_CreateSTS#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#20)]
     [!code-vb[c_CreateSTS#20](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#20)]  
  
### <a name="to-configure-an-x509certificaterecipientclientcredential-in-configuration"></a>Para configurar um X509CertificateRecipientClientCredential na configuração  
  
1. Crie um [\<scopedCertificates>](../../configure-apps/file-schema/wcf/scopedcertificates-element.md) elemento como um filho do [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) elemento que é, em si, um filho do [\<clientCredentials>](../../configure-apps/file-schema/wcf/clientcredentials.md) elemento em um comportamento de ponto de extremidade.  
  
2. Crie um `<add>` elemento como um filho do `<scopedCertificates>` elemento. Especifique valores para os `storeLocation` `storeName` atributos,, `x509FindType` e `findValue` para fazer referência ao certificado apropriado. Defina o `targetUri` atributo para um valor que forneça o endereço do ponto de extremidade para o qual o certificado deve ser usado, conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <scopedCertificates>  
     <add targetUri="http://fabrikam.com/sts"
          storeLocation="CurrentUser"  
          storeName="TrustedPeople"  
          x509FindType="FindBySubjectName"  
          findValue="FabrikamSTS" />  
    </scopedCertificates>  
    ```  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir configura uma instância da <xref:System.ServiceModel.Security.IssuedTokenClientCredential> classe no código.  
  
 [!code-csharp[c_FederatedClient#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federatedclient/cs/source.cs#2)]
 [!code-vb[c_FederatedClient#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federatedclient/vb/source.vb#2)]  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Para evitar a possível divulgação de informações, os clientes que executam a ferramenta svcutil. exe para processar metadados de pontos de extremidade federados devem garantir que os endereços resultantes do serviço de token de segurança sejam os esperados. Isso é especialmente importante quando um serviço de token de segurança expõe vários pontos de extremidade, uma vez que a ferramenta svcutil. exe gera o arquivo de configuração resultante para usar o primeiro, que pode não ser aquele que o cliente deve usar.  
  
## <a name="localissuer-required"></a>LocalIssuer necessário  
 Se espera-se que os clientes sempre usem um emissor local, observe o seguinte: a saída padrão de svcutil. exe resulta no emissor local não ser usado se o serviço de token de segurança de segundo a último na cadeia especifica um endereço de emissor ou endereço de metadados do emissor.  
  
 Para obter mais informações sobre como definir as <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerAddress%2A> <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerBinding%2A> Propriedades, e <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerChannelBehaviors%2A> da <xref:System.ServiceModel.Security.IssuedTokenClientCredential> classe, consulte [como: configurar um emissor local](how-to-configure-a-local-issuer.md).  
  
## <a name="scoped-certificates"></a>Certificados com escopo  
 Se os certificados de serviço devem ser especificados para comunicação com qualquer um dos serviços de token de segurança, normalmente porque a negociação de certificado não está sendo usada, eles podem ser especificados usando a <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> propriedade da <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> classe. O <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetDefaultCertificate%2A> método usa um <xref:System.Uri> e um <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> como parâmetros. O certificado especificado é usado ao se comunicar com pontos de extremidade no URI especificado. Como alternativa, você pode usar o <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetScopedCertificate%2A> método para adicionar um certificado à coleção retornada pela <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> propriedade.  
  
> [!NOTE]
> A ideia do cliente de certificados que têm o escopo de um determinado URI aplica-se somente a aplicativos que fazem chamadas de saída para serviços que expõem pontos de extremidade nesses URIs. Ele não se aplica a certificados que são usados para assinar tokens emitidos, como aqueles configurados no servidor na coleção retornada pelo <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> da <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> classe. Para obter mais informações, consulte [How to: Configure Credentials on a serviço de Federação](how-to-configure-credentials-on-a-federation-service.md).  
  
## <a name="see-also"></a>Consulte também

- [Exemplo de federação](../samples/federation-sample.md)
- [Como desabilitar sessões seguranças em uma WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [Como criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
- [Como configurar credenciais em um serviço de federação](how-to-configure-credentials-on-a-federation-service.md)
- [Como configurar um emissor local](how-to-configure-a-local-issuer.md)
- [Considerações de segurança com metadados](security-considerations-with-metadata.md)
- [Como proteger pontos de extremidade de metadados](how-to-secure-metadata-endpoints.md)
