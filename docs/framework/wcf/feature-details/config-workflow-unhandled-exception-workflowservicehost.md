---
title: Como configurar um comportamento de exceção não tratado de fluxo de trabalho com o WorkflowServiceHost
ms.date: 03/30/2017
ms.assetid: 51b25c86-292c-43e4-8d13-273d2badc8ad
ms.openlocfilehash: 3881d1af21dcc0c211c6738162360e522648d949
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593588"
---
# <a name="how-to-configure-workflow-unhandled-exception-behavior-with-workflowservicehost"></a>Como configurar um comportamento de exceção não tratado de fluxo de trabalho com o WorkflowServiceHost
O <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> é um comportamento que permite que você especifique a ação a ser tomada se uma exceção sem tratamento ocorrer em um fluxo de trabalho hospedado no <xref:System.ServiceModel.Activities.WorkflowServiceHost> . Este tópico mostra como configurar esse comportamento em um arquivo de configuração.  
  
### <a name="to-configure-workflowunhandledexceptionbehavior"></a>Para configurar o WorkflowUnhandledExceptionBehavior  
  
1. Adicione um `workflowUnhandledException` elemento <> em um `behavior` elemento <> dentro de um elemento <> `serviceBehaviors` , usando o `action` atributo para especificar a ação a ser tomada quando uma exceção sem tratamento ocorrer, conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <workflowUnhandledException action="abandonAndSuspend"/>
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
    > [!NOTE]
    > O exemplo de configuração anterior está usando uma configuração simplificada. Para obter mais informações, consulte [configuração simplificada](../simplified-configuration.md).  
  
     Esse comportamento pode ser configurado no código, conforme mostrado no exemplo a seguir.  
  
    ```csharp  
    host.Description.Behaviors.Add(new WorkflowUnhandledExceptionBehavior { Action = WorkflowUnhandledExceptionAction.AbandonAndSuspend });  
    ```  
  
     O `action` atributo do elemento <`workflowUnhandledException`> pode ser definido como um dos seguintes valores:  
  
     **Abandon**  
     Anula a instância na memória sem tocar no estado de instância persistente (ou seja, reverta para o último ponto de persistência).  
  
     **abandonAndSuspend**  
     Anula a instância na memória e atualiza a instância persistente a ser suspensa.  
  
     **cancel**  
     Chama manipuladores de cancelamento para a instância e, em seguida, conclui a instância na memória, o que também pode removê-la do repositório de instância  
  
     **encerrar**  
     Conclui a instância na memória e a remove do repositório de instância.  
  
     Para obter mais informações sobre o <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> , consulte [extensibilidade do host do serviço de fluxo de trabalho](workflow-service-host-extensibility.md).  
  
## <a name="see-also"></a>Consulte também

- [Extensibilidade de host de serviço de fluxo de trabalho](workflow-service-host-extensibility.md)
- [Serviços de fluxo de trabalho](workflow-services.md)
