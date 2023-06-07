---
title: Como criar um serviço que requer sessões
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8a7613ef-0df9-47c3-b8dc-47f42cb1fd8b
ms.openlocfilehash: 29c2a87daaf763a50aa657c9badc002ff2fa27e1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593328"
---
# <a name="how-to-create-a-service-that-requires-sessions"></a>Como criar um serviço que requer sessões
As sessões criam um estado compartilhado entre dois ou mais pontos de extremidade que habilitam recursos úteis, como retornos de chamada, segurança de saltos múltiplos e associações entre clientes e instâncias de serviço. Para obter mais informações sobre sessões em aplicativos Windows Communication Foundation (WCF), consulte [usando sessões](../using-sessions.md).  
  
### <a name="to-specify-that-a-contract-require-its-binding-to-support-sessions"></a>Para especificar que um contrato exige sua ligação para dar suporte a sessões  
  
1. Crie um contrato de serviço com pelo menos uma operação. Para obter um exemplo de como criar um contrato de serviço, consulte [como definir um contrato de serviço](../how-to-define-a-wcf-service-contract.md).  
  
2. Modifique o <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType> que declara o contrato definindo a <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> propriedade como:  
  
    - <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType>Se este contrato deve ser executado dentro de uma sessão.  
  
    - <xref:System.ServiceModel.SessionMode.Allowed?displayProperty=nameWithType>Se esse contrato puder ser executado dentro de uma sessão.  
  
    - <xref:System.ServiceModel.SessionMode.NotAllowed?displayProperty=nameWithType>Se este contrato não deve ser executado dentro de uma sessão.  
  
3. Configure seu ponto de extremidade de serviço para usar uma associação que dá suporte a sessões. O exemplo de configuração a seguir mostra o uso do <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> , que dá suporte a uma `-` sessão WS ReliableMessaging.  
  
     [!code-xml[SCA.Session#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/hostapplication.exe.config#2)]
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir mostra como especificar um requisito de sessão de nível de contrato e usar um arquivo de configuração para dar suporte a esse requisito com a <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> associação.  
  
 [!code-csharp[SCA.Session#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/services.cs#1)]
 [!code-vb[SCA.Session#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/sca.session/vb/services.vb#1)]
 [!code-xml[SCA.Session#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/hostapplication.exe.config#2)]
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType>
