---
title: Ponto de extremidade de controle de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: 1b883334-1590-4fbb-b0d6-65197efe0700
ms.openlocfilehash: 91923129235a596e4fa19a8e5982845a25db9712
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594914"
---
# <a name="workflow-control-endpoint"></a>Ponto de extremidade de controle de fluxo de trabalho
O ponto de extremidade de controle de fluxo de trabalho permite aos desenvolvedores chamar operações de controle para controlar remotamente as instâncias de fluxo de trabalho hospedadas <xref:System.ServiceModel.Activities.WorkflowServiceHost> Esse recurso pode ser usado para realizar programaticamente operações de controle como suspender, retomar e encerrar.  
  
> [!WARNING]
> Se você estiver usando o ponto de extremidade de controle do fluxo de trabalho dentro de uma transação e o fluxo de trabalho que está sendo controlado contiver uma <xref:System.Activities.Statements.Persist> atividade, a instância do fluxo de trabalho será bloqueada até que  
  
## <a name="workflow-instance-management"></a>Gerenciamento de instância de fluxo de trabalho  
 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]define um novo contrato chamado <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> . Este contrato define uma série de operações de controle que permitem controlar remotamente as instâncias de fluxo de trabalho hospedadas pelo <xref:System.ServiceModel.Activities.WorkflowServiceHost> . <xref:System.ServiceModel.Activities.WorkflowControlEndpoint>é um ponto de extremidade padrão que fornece uma implementação do <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> contrato. <xref:System.ServiceModel.Activities.WorkflowControlClient>é uma classe que é usada para enviar as operações de controle para o <xref:System.ServiceModel.Activities.WorkflowControlEndpoint> .  
  
 As instâncias de fluxo de trabalho podem estar em um dos seguintes Estados:  
  
 Ativo  
 O estado de uma instância de fluxo de trabalho antes de atingir o estado concluído e quando não está no estado suspenso. Nesse estado, a instância de fluxo de trabalho executa e processa as mensagens de aplicativo.  
  
 Suspenso  
 Nesse estado, a instância de fluxo de trabalho não é executada mesmo que haja atividades que não tenham iniciado a execução ou tenham sido parcialmente executadas.  
  
 Concluído  
 O estado final de uma instância de fluxo de trabalho. A instância de fluxo de trabalho não pode ser executada Depois de atingir o estado concluído.  
  
## <a name="iworkflowinstancemanagement"></a>IWorkflowInstanceManagement  
 A <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> interface define um conjunto de operações de controle com as versões síncronas e assíncronas. As versões transacionadas exigem o uso de uma associação com reconhecimento de transação. A tabela a seguir lista as operações de controle com suporte.  
  
|Operação de controle|Descrição|  
|-----------------------|-----------------|  
|Anular|Interrompe a execução da instância do fluxo de trabalho de modo forçado.|  
|Cancelar|Faz a transição de uma instância de fluxo de trabalho do estado ativo ou suspenso para o estado concluído.|  
|Executar|Fornece uma instância de fluxo de trabalho a oportunidade de executar.|  
|Suspend|Faz a transição de uma instância de fluxo de trabalho do estado ativo para o estado suspenso.|  
|Terminate|Faz a transição de uma instância de fluxo de trabalho do estado ativo ou suspenso para o estado concluído.|  
|Cancelar suspensão|Faz a transição de uma instância de fluxo de trabalho do estado suspenso para o estado ativo.|  
|TransactedCancel|Executa a operação de cancelamento em uma transação (transmitida do cliente ou criada localmente). Se o sistema mantiver o estado durável da instância do fluxo de trabalho, a instância do fluxo de trabalho deverá ser persistida durante a execução dessa operação.|  
|TransactedRun|Executa a operação de execução em uma transação (transmitida do cliente ou criada localmente). Se o sistema mantiver o estado durável da instância do fluxo de trabalho, a instância do fluxo de trabalho deverá ser persistida durante a execução dessa operação.|  
|TransactedSuspend|Executa a operação de suspensão em uma transação (transmitida do cliente ou criada localmente). Se o sistema mantiver o estado durável da instância do fluxo de trabalho, a instância do fluxo de trabalho deverá ser persistida durante a execução dessa operação.|  
|TransactedTerminate|Executa a operação de encerramento em uma transação (transmitida do cliente ou criada localmente). Se o sistema mantiver o estado durável da instância do fluxo de trabalho, a instância do fluxo de trabalho deverá ser persistida durante a execução dessa operação.|  
|TransactedUnsuspend|Executa a operação de dessuspensão em uma transação (transmitida do cliente ou criada localmente). Se o sistema mantiver o estado durável da instância do fluxo de trabalho, a instância do fluxo de trabalho deverá ser persistida durante a execução dessa operação.|  
  
 O <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> contrato não fornece um meio para criar uma nova instância de fluxo de trabalho, somente para gerenciar instâncias de fluxo de trabalho existentes. Para obter mais informações sobre como criar uma nova instância de fluxo de trabalho remotamente, consulte [extensibilidade do host do serviço de fluxo de trabalho](workflow-service-host-extensibility.md).  
  
## <a name="workflowcontrolendpoint"></a>WorkflowControlEndpoint  
 <xref:System.ServiceModel.Activities.WorkflowControlEndpoint>é um ponto de extremidade padrão com um contrato fixo, <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> . Quando adicionado a uma <xref:System.ServiceModel.Activities.WorkflowServiceHost> instância, esse ponto de extremidade pode ser usado para enviar operações de comando para qualquer instância de fluxo de trabalho hospedada pela instância do host. Para obter mais informações sobre pontos de extremidade padrão, consulte [pontos de extremidade padrão](standard-endpoints.md).  
  
## <a name="workflowcontrolclient"></a>WorkflowControlClient  
 <xref:System.ServiceModel.Activities.WorkflowControlClient>é uma classe que permite enviar mensagens de controle para um <xref:System.ServiceModel.Activities.WorkflowControlEndpoint> em um <xref:System.ServiceModel.Activities.WorkflowServiceHost> . Ele contém um método para cada uma das operações com suporte do <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> contrato, exceto para as operações transacionadas. <xref:System.ServiceModel.Activities.WorkflowControlClient>usa a transação de ambiente para determinar se uma operação transacionada deve ser usada.
