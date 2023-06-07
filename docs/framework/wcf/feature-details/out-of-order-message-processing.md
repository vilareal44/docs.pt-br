---
title: Processamento de mensagem com problema
ms.date: 03/30/2017
ms.assetid: 33fc62a5-5d59-461c-a37a-0e1b51ac763d
ms.openlocfilehash: 7930f26cf5957158a16d65085267cf1bab2e4504
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598717"
---
# <a name="out-of-order-message-processing"></a>Processamento de mensagem com problema
Os serviços de fluxo de trabalho podem depender de mensagens sendo enviadas em uma ordem específica. Um serviço de fluxo de trabalho contém uma ou mais <xref:System.ServiceModel.Activities.Receive> atividades e cada <xref:System.ServiceModel.Activities.Receive> atividade está esperando uma mensagem específica. Sem garantias de entrega de transporte específicas, as mensagens enviadas pelos clientes podem ser atrasadas e, portanto, entregues em uma ordem o serviço de fluxo de trabalho pode não esperar. A implementação de um serviço de fluxo de trabalho que não requer mensagens enviadas em uma ordem específica normalmente é feita usando uma atividade paralela. Para um protocolo de aplicativo mais complicado, o fluxo de trabalho se tornaria muito complexo muito rapidamente.  O recurso de processamento de mensagens fora de ordem no Windows Communication Foundation (WCF) permite que você crie um fluxo de trabalho sem toda a complexidade de atividades paralelas aninhadas. O processamento de mensagens fora de ordem tem suporte apenas em canais que dão suporte como <xref:System.ServiceModel.Channels.ReceiveContext> associações MSMQ do WCF.  
  
## <a name="enabling-out-of-order-message-processing"></a>Habilitando o processamento de mensagens fora de ordem  
 O processamento de mensagens fora de ordem é habilitado pela definição da <xref:System.ServiceModel.Activities.WorkflowService.AllowBufferedReceive%2A> propriedade como `true` no WorkflowService. O exemplo a seguir mostra como definir a <xref:System.ServiceModel.Activities.WorkflowService.AllowBufferedReceive%2A> propriedade no código.  
  
```csharp  
// Code: Opt-in to Buffered Receive processing...  
WorkflowService service = new WorkflowService  
{  
    Name="MyService",  
    Body = workflow,  
    AllowBufferedReceive = true  
};  
```  
  
 Você também pode aplicar o `AllowBufferedReceive` atributo a um serviço de fluxo de trabalho em XAML, conforme mostrado no exemplo a seguir.  
  
```xaml  
// Xaml: Opt-in to Buffered Receive processing...  
<WorkflowService AllowBufferedReceive="True">  
   <!--the actual children activities -->  
</Sequence>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Channels.ReceiveContext>
- [Serviços de fluxo de trabalho](workflow-services.md)
- [Sessões confiáveis e filas](queues-and-reliable-sessions.md)
