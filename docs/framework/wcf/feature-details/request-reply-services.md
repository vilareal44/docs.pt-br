---
title: Serviços de resposta por solicitação
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], request-reply services
- contracts [WCF], request-reply services
- WCF [WCF], request-reply services
- request-reply contracts [WCF]
ms.assetid: 2fa710f1-47f4-4598-b063-3ab3bd22ebba
ms.openlocfilehash: df42f3fa8f5a15572987b0d4859856c7f838e632
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586228"
---
# <a name="request-reply-services"></a>Serviços de resposta por solicitação
Os serviços de solicitação-resposta são o tipo padrão de contrato de operação no Windows Communication Foundation (WCF). Os clientes fazem chamadas para operações de serviço e aguardam uma resposta do serviço. Você pode executar chamadas para uma operação de serviço de forma síncrona, em que o cliente é bloqueado até receber uma resposta do serviço ou dos tempos de chamada, ou de forma assíncrona, em que o cliente faz uma chamada para a operação de serviço, continua trabalhando e recebe a resposta do serviço em outro thread.  
  
 Para criar um contrato de serviço de solicitação-resposta, defina seu contrato de serviço e aplique a <xref:System.ServiceModel.OperationContractAttribute> classe a cada operação, conforme mostrado no código de exemplo a seguir.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IRequestReplyCalculator  
{  
    [OperationContract]  
    double Add(double n1, double n2);  
}  
```  
  
 Você não precisa definir a <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> propriedade como, `false` pois esse é o comportamento padrão.  
  
## <a name="see-also"></a>Consulte também

- [Serviços unidirecionais](one-way-services.md)
- [Serviços duplex](duplex-services.md)
