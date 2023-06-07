---
title: Como criar um ponto de extremidade de serviço em código
description: Saiba como implementar um serviço em uma classe e definir seu ponto de extremidade programaticamente. No WCF, os pontos de extremidade geralmente são definidos em um arquivo de configuração.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3fbb22fa-2930-48b8-b437-def1de87c6a0
ms.openlocfilehash: 3b2ff2a17975bc381db61edc2c0f85f67edd3325
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247046"
---
# <a name="how-to-create-a-service-endpoint-in-code"></a>Como criar um ponto de extremidade de serviço em código
Neste exemplo, um `ICalculator` contrato é definido para um serviço de calculadora, o serviço é implementado na `CalculatorService` classe e, em seguida, seu ponto de extremidade é definido no código, onde é especificado que o serviço deve usar a <xref:System.ServiceModel.BasicHttpBinding> classe.  
  
 Geralmente, é a prática recomendada especificar a associação e as informações de endereço de forma declarativa na configuração, em vez de imperativa no código. A definição de pontos de extremidade no código geralmente não é prática porque as associações e os endereços para um serviço implantado são normalmente diferentes daqueles usados enquanto o serviço está sendo desenvolvido. Em geral, manter as informações de vinculação e endereçamento do código permite que elas sejam alteradas sem a necessidade de recompilar ou reimplantar o aplicativo.  
  
#### <a name="to-create-a-service-endpoint-in-code"></a>Para criar um ponto de extremidade de serviço no código  
  
1. Crie a interface que define o contrato de serviço.  
  
     [!code-csharp[c_HowTo_CodeServiceBinding#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeservicebinding/cs/source.cs#1)]
     [!code-vb[c_HowTo_CodeServiceBinding#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeservicebinding/vb/source.vb#1)]  
  
2. Implemente o contrato de serviço definido na etapa 1.  
  
     [!code-csharp[c_HowTo_CodeServiceBinding#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeservicebinding/cs/source.cs#2)]
     [!code-vb[c_HowTo_CodeServiceBinding#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeservicebinding/vb/source.vb#2)]  
  
3. No aplicativo de hospedagem, crie o endereço base para o serviço e a associação a ser usada com o serviço.  
  
     [!code-csharp[c_HowTo_CodeServiceBinding#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeservicebinding/cs/source.cs#3)]
     [!code-vb[c_HowTo_CodeServiceBinding#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeservicebinding/vb/source.vb#3)]  
  
4. Crie o host e a chamada <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%28System.Type%2CSystem.ServiceModel.Channels.Binding%2CSystem.String%29?displayProperty=nameWithType> ou uma das outras sobrecargas para adicionar o ponto de extremidade de serviço para o host.  
  
     [!code-csharp[c_HowTo_CodeServiceBinding#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeservicebinding/cs/source.cs#6)]
     [!code-vb[c_HowTo_CodeServiceBinding#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeservicebinding/vb/source.vb#6)]  
  
     Para especificar a associação no código, mas usar os pontos de extremidade padrão fornecidos pelo tempo de execução, passe o endereço base para o construtor ao criar o <xref:System.ServiceModel.ServiceHost> , e não chame <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A?displayProperty=nameWithType> .  
  
     [!code-csharp[c_HowTo_CodeServiceBinding#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeservicebinding/cs/source.cs#7)]
     [!code-vb[c_HowTo_CodeServiceBinding#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeservicebinding/vb/source.vb#7)]  
  
     Para obter mais informações sobre pontos de extremidade padrão, consulte [configuração simplificada](../simplified-configuration.md) e [configuração simplificada para serviços WCF](../samples/simplified-configuration-for-wcf-services.md).  
  
## <a name="see-also"></a>Veja também

- [Como especificar uma associação de serviço no código](../how-to-specify-a-service-binding-in-code.md)
