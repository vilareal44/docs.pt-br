---
title: Como configurar um cliente do WCF para interoperar com serviços WSE3.0
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3dadd7f1-d207-4ea5-a73b-3e8aa44407f8
ms.openlocfilehash: 7dd50fcc07c6c090042cf87acb4aa5d2b5321a68
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579573"
---
# <a name="how-to-configure-a-wcf-client-to-interoperate-with-wse30-services"></a>Como configurar um cliente do WCF para interoperar com serviços WSE3.0
Os clientes do Windows Communication Foundation (WCF) são compatíveis com o nível de conexão com os serviços do WSE (Web Services Enhancements 3,0 for Microsoft .NET) quando os clientes WCF são configurados para usar a versão de agosto de 2004 da especificação WS-Addressing.  
  
### <a name="to-configure-a-wcf-client-to-interoperate-with-a-wse-30-web-service"></a>Para configurar um cliente WCF para interoperar com um serviço Web WSE 3,0  
  
1. Execute a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para criar um cliente WCF para o serviço Web WSE 3,0.  
  
     Para um serviço Web do WSE, uma classe de cliente WCF é criada.  
  
     Para obter detalhes sobre como criar um cliente WCF, consulte o [How to: criar um cliente](../how-to-create-a-wcf-client.md).  
  
2. Crie uma classe que represente uma associação que possa se comunicar com os serviços Web do WSE 3,0.  
  
     A classe a seguir faz parte do exemplo de [interoperação com o WSE](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752257%28v=vs.90%29) .  
  
    1. Crie uma classe que derive da classe <xref:System.ServiceModel.Channels.Binding>.  
  
         O exemplo de código a seguir cria uma classe chamada `WseHttpBinding` derivada da <xref:System.ServiceModel.Channels.Binding> classe.  
  
         [!code-csharp[c_WCFClientToWSEService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#1)]
         [!code-vb[c_WCFClientToWSEService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#1)]  
  
    2. Adicione Propriedades à classe que especifica a declaração ativada do WSE, se as chaves derivadas são necessárias, se as sessões seguras são usadas, se as confirmações de assinatura são necessárias e as configurações de proteção de mensagem.  
  
         O exemplo de código a seguir define as `SecurityAssertion` `RequireDerivedKeys` Propriedades,, e `EstablishSecurityContext` `MessageProtectionOrder` . Eles especificam a declaração do WSE pronto, independentemente de as chaves derivadas serem necessárias, se as sessões seguras são usadas, se as confirmações de assinatura são necessárias e as configurações de proteção de mensagem, respectivamente.  
  
         [!code-csharp[c_WCFClientToWSEService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#3)]
         [!code-vb[c_WCFClientToWSEService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#3)]  
  
    3. Substitua o <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> método para definir as propriedades de associação.  
  
         O exemplo de código a seguir especifica as configurações de transporte, codificação de mensagem e proteção de mensagem obtendo os valores das `SecurityAssertion` `MessageProtectionOrder` Propriedades e.  
  
         [!code-csharp[c_WCFClientToWSEService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#2)]
         [!code-vb[c_WCFClientToWSEService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#2)]  
  
3. No código do aplicativo cliente, adicione o código para definir as propriedades de associação.  
  
     O exemplo de código a seguir especifica que o cliente WCF deve usar a proteção e a autenticação de mensagens conforme definido pela declaração de segurança do WSE 3,0 pronto para uso `AnonymousForCertificate` . Além disso, as sessões seguras e as chaves derivadas são necessárias.  
  
     [!code-csharp[c_WCFClientToWSEService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#4)]
     [!code-vb[c_WCFClientToWSEService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#4)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir define uma associação personalizada que expõe propriedades que correspondem às propriedades de uma declaração de segurança do WSE 3,0 pronto para uso. A associação personalizada, que é nomeada `WseHttpBinding` , é usada para especificar as propriedades de associação para um cliente WCF.  

[!code-csharp[c_WCFClientToWSEService#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#0)]
[!code-vb[c_WCFClientToWSEService#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#0)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Channels.Binding>
- [Interoperação com o WSE](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752257%28v=vs.90%29)
