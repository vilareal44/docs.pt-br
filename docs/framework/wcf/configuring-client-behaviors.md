---
title: Configurando comportamentos do cliente
description: 'Saiba mais sobre as duas maneiras pelas quais o WCF configura comportamentos: no arquivo de configuração do aplicativo ou programaticamente do aplicativo de chamada.'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: df5b32fa-e73b-4e8e-b66f-357c748e0173
ms.openlocfilehash: 4b83862221cf249455478c3ade159a3101062f3e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245434"
---
# <a name="configuring-client-behaviors"></a>Configurando comportamentos do cliente
O Windows Communication Foundation (WCF) configura comportamentos de duas maneiras: seja referindo-se a configurações de comportamento, que são definidas na `<behavior>` seção de um arquivo de configuração de aplicativo cliente – ou programaticamente no aplicativo de chamada. Este tópico descreve as duas abordagens.  
  
 Ao usar um arquivo de configuração, a configuração de comportamento é uma coleção nomeada de definições de configuração. O nome de cada configuração de comportamento deve ser exclusivo. Essa cadeia de caracteres é usada no `behaviorConfiguration` atributo de uma configuração de ponto de extremidade para vincular o ponto de extremidade ao comportamento.  
  
## <a name="example"></a>Exemplo  
 O código de configuração a seguir define um comportamento chamado `myBehavior` . O ponto de extremidade do cliente faz referência a esse comportamento no `behaviorConfiguration` atributo.  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="myBehavior">  
                    <clientVia />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
        <bindings>  
            <basicHttpBinding>  
                <binding name="myBinding" maxReceivedMessageSize="10000" />  
            </basicHttpBinding>  
        </bindings>  
        <client>  
            <endpoint address="myAddress" binding="basicHttpBinding" bindingConfiguration="myBinding" behaviorConfiguration="myBehavior" contract="myContract" />  
        </client>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="using-behaviors-programmatically"></a>Usando comportamentos programaticamente  
 Você também pode configurar ou inserir comportamentos programaticamente, localizando a `Behaviors` propriedade apropriada no objeto de cliente Windows Communication Foundation (WCF) ou no objeto de fábrica de canais do cliente antes de abrir o cliente.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra como inserir programaticamente um comportamento acessando a <xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> propriedade no <xref:System.ServiceModel.Description.ServiceEndpoint> retornado da <xref:System.ServiceModel.ChannelFactory.Endpoint%2A> propriedade antes da criação do objeto de canal.  
  
 [!code-csharp[ChannelFactoryBehaviors#10](../../../samples/snippets/csharp/VS_Snippets_CFX/channelfactorybehaviors/cs/client.cs#10)]
 [!code-vb[ChannelFactoryBehaviors#10](../../../samples/snippets/visualbasic/VS_Snippets_CFX/channelfactorybehaviors/vb/client.vb#10)]  
  
## <a name="see-also"></a>Confira também

- [\<behaviors>](../configure-apps/file-schema/wcf/behaviors.md)
