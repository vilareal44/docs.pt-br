---
title: Como criar uma WSFederationHttpBinding
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: e54897d7-aa6c-46ec-a278-b2430c8c2e10
ms.openlocfilehash: ccc28c46e8be0b835cf08d372ef85b8a66e989ef
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595434"
---
# <a name="how-to-create-a-wsfederationhttpbinding"></a>Como criar uma WSFederationHttpBinding

No Windows Communication Foundation (WCF), a <xref:System.ServiceModel.WSFederationHttpBinding> classe ( [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) na configuração) fornece um mecanismo para expor um serviço federado. Ou seja, um serviço que exige que os clientes se autentiquem usando um token de segurança emitido por um serviço de token de segurança. Este tópico mostra como configurar um <xref:System.ServiceModel.WSFederationHttpBinding> no código e na configuração. Depois que a associação é criada, você pode configurar um ponto de extremidade para usar essa associação.

 As etapas básicas são descritas a seguir:

1. Selecione um modo de segurança. O <xref:System.ServiceModel.WSFederationHttpBinding> oferece suporte `Message` , que fornece segurança de ponta a ponta no nível de mensagem, mesmo entre vários saltos e `TransportWithMessageCredential` , que fornece melhor desempenho em casos em que o cliente e o serviço podem fazer uma conexão direta via HTTPS.

    > [!NOTE]
    > O <xref:System.ServiceModel.WSFederationHttpBinding> também dá suporte `None` como modo de segurança. Esse modo não é seguro e é fornecido apenas para fins de depuração. Se um ponto de extremidade de serviço for implantado com um <xref:System.ServiceModel.WSFederationHttpBinding> com seu modo de segurança definido como `None` , a associação de cliente resultante (gerada pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)é um <xref:System.ServiceModel.WSHttpBinding> com um modo de segurança de `None` .

     Ao contrário de outras associações fornecidas pelo sistema, não é necessário selecionar um tipo de credencial de cliente ao usar o `WSFederationHttpBinding` . Isso ocorre porque o tipo de credencial do cliente é sempre um token emitido. O WCF adquire um token de um emissor especificado e apresenta esse token ao serviço para autenticar o cliente.

2. Em clientes federados, defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerAddress%2A> propriedade como a URL do serviço de token de segurança. Defina <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerBinding%2A> como a associação a ser usada para se comunicar com o serviço de token de segurança.

3. Opcional. Defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedTokenType%2A> propriedade para o Uniform Resource Identifier (URI) de um tipo de token. Em serviços federados, especifique o tipo de token que o serviço espera. Em clientes federados, especifique o tipo de token que o cliente solicita do serviço de token de segurança.

     Se nenhum tipo de token for especificado, os clientes gerarão tokens de segurança de solicitação WS-Trust (RSTs) sem um URI de tipo de token e os serviços esperam a autenticação de cliente usando um token 1,1 SAML (Security Asserts Markup Language) por padrão.

     O URI para um token SAML 1,1 é `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1` .

4. Opcional. Em serviços federados, defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerMetadataAddress%2A> propriedade como a URL de metadados de um serviço de token de segurança. O ponto de extremidade de metadados permite que os clientes do serviço selecionem um par de ligação/ponto de extremidade apropriado, se o serviço estiver configurado para publicar metadados. Para obter mais informações sobre como publicar metadados, consulte [publicando metadados](publishing-metadata.md).

 Você também pode definir outras propriedades, incluindo o tipo de chave usada como uma chave de prova no token emitido, o conjunto de algoritmos a ser usado entre o cliente e o serviço, se deseja negociar ou especificar explicitamente a credencial do serviço, quaisquer declarações específicas que o serviço espera que o token emitido contenha e quaisquer elementos XML adicionais que devem ser adicionados à solicitação que o cliente envia ao serviço de

> [!NOTE]
> A `NegotiateServiceCredential` propriedade só é relevante quando o `SecurityMode` é definido como `Message` . Se `SecurityMode` for definido como `TransportWithMessageCredential` , a `NegotiateServiceCredential` propriedade será ignorada.

## <a name="to-configure-a-wsfederationhttpbinding-in-code"></a>Para configurar um WSFederationHttpBinding no código

1. Crie uma instância de <xref:System.ServiceModel.WSFederationHttpBinding>.

2. Defina a <xref:System.ServiceModel.WSFederationHttpSecurity.Mode%2A> propriedade como <xref:System.ServiceModel.WSFederationHttpSecurityMode> ou <xref:System.ServiceModel.WSFederationHttpSecurityMode.Message> conforme necessário. Se um conjunto de algoritmos diferente de <xref:System.ServiceModel.Security.SecurityAlgorithmSuite.Basic256%2A> for necessário, defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.AlgorithmSuite%2A> propriedade como um valor obtido de <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> .

3. Defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.NegotiateServiceCredential%2A> propriedade conforme apropriado.

4. Defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedKeyType%2A> propriedade como <xref:System.IdentityModel.Tokens.SecurityKeyType> `SymmetricKey` ou.`AsymmetricKey` conforme necessário.

5. Defina a <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedTokenType%2A> propriedade para o valor apropriado. Se nenhum valor for definido, o WCF usa como padrão `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1` o, que indica tokens SAML 1,1.

6. Necessário no cliente se nenhum emissor local for especificado; opcional no serviço. Crie um <xref:System.ServiceModel.EndpointAddress> que contenha as informações de endereço e identidade do serviço de token de segurança e atribua a <xref:System.ServiceModel.EndpointAddress> instância à <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerAddress%2A> propriedade.

7. Necessário no cliente se nenhum emissor local for especificado; Não usado no serviço. Crie um <xref:System.ServiceModel.Channels.Binding> para o `SecurityTokenService` e atribua a <xref:System.ServiceModel.Channels.Binding> instância à <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerBinding%2A> propriedade.

8. Não usado no cliente; opcional no serviço. Crie uma <xref:System.ServiceModel.EndpointAddress> instância para os metadados do serviço de token de segurança e atribua-a à `IssuerMetadataAddress` propriedade.

9. Opcional no cliente e no serviço. Crie e adicione uma ou mais <xref:System.ServiceModel.Security.Tokens.ClaimTypeRequirement> instâncias à coleção retornada pela <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.ClaimTypeRequirements%2A> propriedade.

10. Opcional no cliente e no serviço. Crie e adicione uma ou mais <xref:System.Xml.XmlElement> instâncias à coleção retornada pela <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.TokenRequestParameters%2A> propriedade.

## <a name="to-create-a-federated-endpoint-in-configuration"></a>Para criar um ponto de extremidade federado na configuração

1. Crie um [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) como um filho do [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) elemento no arquivo de configuração do aplicativo.

2. Crie um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento como um filho de [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) e defina o `name` atributo como um valor apropriado.

3. Crie um `<security>` elemento como um filho do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento.

4. Defina o `mode` atributo no `<security>` elemento como um valor de `Message` ou `TransportWithMessageCredential` , conforme necessário.

5. Crie um `<message>` elemento como um filho do `<security>` elemento.

6. Opcional. Defina o `algorithmSuite` atributo no `<message>` elemento com um valor apropriado. O padrão é `Basic256`.

7. Opcional. Se uma chave de prova assimétrica for necessária, defina o `issuedKeyType` atributo do `<message>` elemento como `AsymmetricKey` . O padrão é `SymmetricKey`.

8. Opcional. Defina o `issuedTokenType` atributo no `<message>` elemento.

9. Necessário no cliente se nenhum emissor local for especificado; opcional no serviço. Crie um `<issuer>` elemento como um filho do `<message>` elemento.

10. Defina o `address` atributo para o `<issuer>` elemento e especifique o endereço no qual o serviço de token de segurança aceita solicitações de token.

11. Opcional. Adicionar um `<identity>` elemento filho e especificar a identidade do serviço de token de segurança

12. Para obter mais informações, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).

13. Necessário no cliente se nenhum emissor local for especificado; Não usado no serviço. Crie um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento na seção de associações que pode ser usado para se comunicar com o serviço de token de segurança. Para obter mais informações sobre como criar uma associação, consulte [como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md).

14. Especifique a associação criada na etapa anterior definindo os `binding` `bindingConfiguration` atributos e do `<issuer>` elemento.

15. Não usado no cliente; opcional no serviço. Crie um `<issuerMetadata>` elemento como um filho do elemento <`message`>. Em seguida, em um `address` atributo no `<issuerMetadata>` elemento, especifique o endereço no qual o serviço de token de segurança é para publicar seus metadados. Opcionalmente, adicione um `<identity>` elemento filho e especifique a identidade do serviço de token de segurança.

16. Opcional no cliente e no serviço. Adicione um `<claimTypeRequirements>` elemento como um filho do `<message>` elemento. Especifique as declarações obrigatórias e opcionais nas quais o serviço depende adicionando [\<add>](../../configure-apps/file-schema/wcf/add-of-claimtyperequirements.md) elementos ao `<claimTypeRequirements>` elemento e especificando o tipo de declaração com o `claimType` atributo. Especifique se uma determinada declaração é necessária ou opcional definindo o `isOptional` atributo.

## <a name="example"></a>Exemplo

O exemplo de código a seguir mostra o código para configurar um `WSFederationHttpBinding` imperativo.

[!code-csharp[c_FederationBinding#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_federationbinding/cs/source.cs#2)]
[!code-vb[c_FederationBinding#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_federationbinding/vb/source.vb#2)]

## <a name="see-also"></a>Consulte também

- [Federação](federation.md)
- [Exemplo de federação](../samples/federation-sample.md)
- [Como desabilitar sessões seguranças em uma WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
