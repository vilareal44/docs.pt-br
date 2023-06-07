---
title: Como configurar um emissor local
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 15263371-514e-4ea6-90fb-14b4939154cd
ms.openlocfilehash: 7da3cd34d0840eea48c9ef0bb89fb6580b87623b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601238"
---
# <a name="how-to-configure-a-local-issuer"></a>Como configurar um emissor local

Este tópico descreve como configurar um cliente para usar um emissor local para tokens emitidos.

Geralmente, quando um cliente se comunica com um serviço federado, o serviço especifica o endereço do serviço de token de segurança que deve emitir o token que o cliente usará para se autenticar no serviço federado. Em determinadas situações, o cliente pode ser configurado para usar um *emissor local*.

Windows Communication Foundation (WCF) usa um emissor local em casos em que o endereço do emissor de uma associação federada é `http://schemas.microsoft.com/2005/12/ServiceModel/Addressing/Anonymous` ou `null` . Nesses casos, você deve configurar o <xref:System.ServiceModel.Description.ClientCredentials> com o endereço do emissor local e a associação a ser usada para se comunicar com esse emissor.

> [!NOTE]
> Se a <xref:System.ServiceModel.Description.ClientCredentials.SupportInteractive%2A> propriedade da `ClientCredentials` classe for definida como `true` , um endereço de emissor local não será especificado e o endereço do emissor especificado pelo [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) ou outra associação federada será, `http://schemas.xmlsoap.org/ws/2005/05/identity/issuer/self` `http://schemas.microsoft.com/2005/12/ServiceModel/Addressing/Anonymous` ou será `null` , o emissor do Windows CardSpace será usado.

## <a name="to-configure-the-local-issuer-in-code"></a>Para configurar o emissor local no código

1. Criar uma variável do tipo<xref:System.ServiceModel.Security.IssuedTokenClientCredential>

2. Defina a variável para a instância retornada da <xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A> propriedade da `ClientCredentials` classe. Essa instância é retornada pela <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> Propriedade do cliente (herdada de <xref:System.ServiceModel.ClientBase%601> ) ou a <xref:System.ServiceModel.ChannelFactory.Credentials%2A> propriedade de <xref:System.ServiceModel.ChannelFactory> :

     [!code-csharp[c_CreateSTS#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#9)]
     [!code-vb[c_CreateSTS#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#9)]

3. Defina a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerAddress%2A> propriedade para uma nova instância do <xref:System.ServiceModel.EndpointAddress> , com o endereço do emissor local como um argumento para o construtor.

     [!code-csharp[c_CreateSTS#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#10)]
     [!code-vb[c_CreateSTS#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#10)]

     Como alternativa, crie uma nova <xref:System.Uri> instância como um argumento para o construtor.

     [!code-csharp[c_CreateSTS#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#11)]
     [!code-vb[c_CreateSTS#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#11)]

     O `addressHeaders` parâmetro é uma matriz de <xref:System.ServiceModel.Channels.AddressHeader> instâncias, como mostrado.

     [!code-csharp[c_CreateSTS#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#12)]
     [!code-vb[c_CreateSTS#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#12)]

4. Defina a associação para o emissor local usando a <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerBinding%2A> propriedade.

     [!code-csharp[c_CreateSTS#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#13)]
     [!code-vb[c_CreateSTS#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#13)]

5. Opcional. Adicione comportamentos de ponto de extremidade configurados para o emissor local adicionando esses comportamentos à coleção retornada pela <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerChannelBehaviors%2A> propriedade.

     [!code-csharp[c_CreateSTS#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#14)]
     [!code-vb[c_CreateSTS#14](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#14)]

## <a name="to-configure-the-local-issuer-in-configuration"></a>Para configurar o emissor local na configuração

1. Crie um [\<localIssuer>](../../configure-apps/file-schema/wcf/localissuer.md) elemento como um filho do [\<issuedToken>](../../configure-apps/file-schema/wcf/issuedtoken.md) elemento que é, em si, um filho do [\<clientCredentials>](../../configure-apps/file-schema/wcf/clientcredentials.md) elemento em um comportamento de ponto de extremidade.

2. Defina o `address` atributo como o endereço do emissor local que aceitará solicitações de token.

3. Defina os `binding` `bindingConfiguration` atributos e como valores que fazem referência à associação apropriada a ser usada na comunicação com o ponto de extremidade do emissor local.

4. Opcional. Defina o [\<identity>](../../configure-apps/file-schema/wcf/identity.md) elemento como um filho do elemento <`localIssuer`> e especifique informações de identidade para o emissor local.

5. Opcional. Defina o [\<headers>](../../configure-apps/file-schema/wcf/headers.md) elemento como um filho do elemento <`localIssuer`> e especifique cabeçalhos adicionais que são necessários para resolver corretamente o emissor local.

## <a name="net-framework-security"></a>Segurança do .NET Framework

Observe que, se um endereço de emissor e uma associação forem especificados para uma determinada associação, o emissor local não será usado para pontos de extremidade que usam essa associação. Os clientes que esperam sempre usar o emissor local devem garantir que eles não usem essa associação ou que modifiquem a associação para que o endereço do emissor seja `null` .

## <a name="see-also"></a>Consulte também

- [Como configurar credenciais em um serviço de federação](how-to-configure-credentials-on-a-federation-service.md)
- [Como criar um cliente federado](how-to-create-a-federated-client.md)
- [Como criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
