---
title: Como configurar comportamento ocioso com WorkflowServiceHost
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1bb93652-d687-46ff-bff6-69ecdcf97437
ms.openlocfilehash: 8b9fa36408d5f2bc5445ebeccba61f71417935e7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599120"
---
# <a name="how-to-configure-idle-behavior-with-workflowservicehost"></a>Como configurar comportamento ocioso com WorkflowServiceHost
Os fluxos de trabalho ficam ociosos quando encontram um indicador que deve ser retomado por algum estímulo externo, por exemplo, quando a instância do fluxo de trabalho está aguardando a entrega de uma mensagem usando uma <xref:System.ServiceModel.Activities.Receive> atividade. <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior>é um comportamento que permite que você especifique o tempo entre quando uma instância de serviço fica ociosa e quando a instância é persistente ou descarregada. Ele contém duas propriedades que permitem que você defina esses intervalos de tempo. <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A>Especifica o período de tempo entre o momento em que uma instância do serviço de fluxo de trabalho fica ociosa e quando a instância do serviço de fluxo de trabalho é persistida. <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A>Especifica o período de tempo entre quando uma instância do serviço de fluxo de trabalho fica ociosa e quando a instância do serviço de fluxo de trabalho é descarregada, em que Unload significa persistir a instância para o repositório de instância e removê-la da memória. Este tópico explica como configurar o <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior> em um arquivo de configuração.  
  
### <a name="to-configure-workflowidlebehavior"></a>Para configurar o WorkflowIdleBehavior  
  
1. Adicione um `workflowIdle` elemento <> ao `behavior` elemento> <dentro do elemento <> `serviceBehaviors` , conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <workflowIdle timeToUnload="0:05:0" timeToPersist="0:04:0"/>
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
     O `timeToUnload` atributo especifica o período de tempo entre o momento em que uma instância do serviço de fluxo de trabalho fica ociosa e quando o serviço de fluxo de trabalho é descarregado. O `timeToPersist` atributo especifica o período de tempo entre o momento em que uma instância do serviço de fluxo de trabalho fica ociosa e quando a instância do serviço de fluxo de trabalho é persistida. O valor padrão para `timeToUnload` é de 1 minuto. O valor padrão para `timeToPersist` é <xref:System.TimeSpan.MaxValue>. Se você quiser manter instâncias ociosas na memória, mas mantê-las para robustez, defina valores para que `timeToPersist`  <  `timeToUnload` . Se você quiser impedir que instâncias ociosas sejam descarregadas, defina `timeToUnload` como <xref:System.TimeSpan.MaxValue> . Para obter mais informações sobre o <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior> , consulte [extensibilidade do host do serviço de fluxo de trabalho](workflow-service-host-extensibility.md)  
  
    > [!NOTE]
    > O exemplo de configuração anterior está usando uma configuração simplificada. Para obter mais informações, consulte [configuração simplificada](../simplified-configuration.md).  
  
### <a name="to-change-idle-behavior-in-code"></a>Para alterar o comportamento ocioso no código  
  
- O exemplo a seguir altera o tempo de espera antes de persistir e descarregar programaticamente.  
  
     [!code-csharp[Wf_SvcHost_Idle_persist#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/wf_svchost_idle_persist/cs/source.cs#1)]
     [!code-vb[Wf_SvcHost_Idle_persist#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/wf_svchost_idle_persist/vb/source.vb#1)]  
  
## <a name="see-also"></a>Consulte também

- [Extensibilidade de host de serviço de fluxo de trabalho](workflow-service-host-extensibility.md)
- [Configuração simplificada](../simplified-configuration.md)
- [Serviços de fluxo de trabalho](workflow-services.md)
