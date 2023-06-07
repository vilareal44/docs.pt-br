---
title: Utilizando contratos no fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: 939c64e9-e7cc-4abc-b41e-27cfce1d7e50
ms.openlocfilehash: def100f9483ea9ac8bf1aa3285d76edccffb030a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595005"
---
# <a name="using-contracts-in-workflow"></a>Utilizando contratos no fluxo de trabalho
Ao implementar um serviço, você define um número de contratos que descrevem o serviço e os dados que ele envia e recebe. Os dados são representados como contratos de dados e contratos de mensagem; o WCF e os serviços de fluxo de trabalho usam as definições de contrato de dados e de contrato de mensagem como parte das descrições de serviço. O próprio serviço expõe metadados (na forma de WSDL) para descrever as operações do serviço. No WCF, os contratos de serviço e os contratos de operação definem o serviço e as operações que ele suporta. No entanto, em um serviço de fluxo de trabalho, esses contratos fazem parte do próprio processo de negócios; Eles são expostos em metadados por um processo chamado inferência de contrato.  
  
## <a name="contract-inference"></a>Inferência de contrato  
 Quando um serviço de fluxo de trabalho é hospedado usando <xref:System.ServiceModel.Activities.WorkflowServiceHost> , a definição de fluxo de trabalho é examinada e um contrato é gerado com base no conjunto de atividades de mensagens encontradas no fluxo de trabalho. Em particular, as seguintes atividades e propriedades são usadas para gerar o contrato:  
  
 <xref:System.ServiceModel.Activities.Receive> Atividade  
  
- <xref:System.ServiceModel.Activities.Receive.ServiceContractName%2A>  
  
- <xref:System.ServiceModel.Activities.Receive.OperationName%2A>
  
- <xref:System.ServiceModel.Activities.Receive.Action%2A>

 <xref:System.ServiceModel.Activities.SendReply> Atividade  
  
- <xref:System.ServiceModel.Activities.SendReply.Action%2A>  
  
 <xref:System.ServiceModel.Activities.TransactedReceiveScope> Atividade  
  
 O resultado final da inferência de contrato é uma descrição do serviço usando as mesmas estruturas de dados que o serviço WCF e os contratos de operação. Essas informações são usadas para expor WSDL para o serviço de fluxo de trabalho.  
  
## <a name="see-also"></a>Consulte também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Atividades de mensagem](messaging-activities.md)
- [Como criar um serviço de fluxo de trabalho com atividades de mensagens](how-to-create-a-workflow-service-with-messaging-activities.md)
- [Como criar um serviço de fluxo de trabalho que consome um contrato de serviço existente](../../windows-workflow-foundation/how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md)
