---
title: Processamento ordenado de mensagens em modo de concorrência única
ms.date: 03/30/2017
ms.assetid: a90f5662-a796-46cd-ae33-30a4072838af
ms.openlocfilehash: baba75fe398d974f989acfda7ef7366986f6813b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598730"
---
# <a name="ordered-processing-of-messages-in-single-concurrency-mode"></a>Processamento ordenado de mensagens em modo de concorrência única
O WCF não faz nenhuma garantia sobre a ordem na qual as mensagens são processadas, a menos que o canal subjacente seja sessão.  Por exemplo, um serviço WCF que usa MsmqInputChannel, que não é um canal de sessão, falhará ao processar mensagens na ordem. Há algumas circunstâncias em que um desenvolvedor pode querer o comportamento de processamento em ordem, mas não quer usar sessões. Este tópico descreve como configurar esse comportamento quando um serviço está sendo executado em modo de simultaneidade única.  
  
## <a name="in-order-message-processing"></a>Processamento de mensagens em ordem  
 Um novo atributo chamado <xref:System.ServiceModel.ServiceBehaviorAttribute.EnsureOrderedDispatch%2A> foi adicionado ao <xref:System.ServiceModel.ServiceBehaviorAttribute> . Quando <xref:System.ServiceModel.ServiceBehaviorAttribute.EnsureOrderedDispatch%2A> é definido como true e <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> é definido para <xref:System.ServiceModel.ConcurrencyMode.Single> as mensagens enviadas ao serviço serão processadas na ordem. O trecho de código a seguir ilustra como definir esses atributos.  
  
```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Single, EnsureOrderedDispatch = true )]  
    class Service : IService  
    {  
         // ...  
    }  
```  
  
 Se <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> é definido como qualquer outro valor, um <xref:System.InvalidOperationException> é gerado.  
  
## <a name="see-also"></a>Consulte também

- [Sessões, instanciação e simultaneidade](sessions-instancing-and-concurrency.md)
- [Simultaneidade](../samples/concurrency.md)
