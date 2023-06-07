---
title: Internos do host de serviço de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: af44596f-bf6a-4149-9f04-08d8e8f45250
ms.openlocfilehash: 7b47293211ee8143b1ce713c64ff1d5b22161b45
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594875"
---
# <a name="workflow-service-host-internals"></a>Internos do host de serviço de fluxo de trabalho
<xref:System.ServiceModel.WorkflowServiceHost>fornece um host para os serviços de fluxo de trabalho. Ele é responsável por escutar mensagens de entrada e roteá-las para a instância apropriada do serviço de fluxo de trabalho, controla o descarregamento e a persistência de fluxos de trabalho ociosos e muito mais. Este tópico descreve como o WorkflowServiceHost processa as mensagens de entrada.  
  
## <a name="workflowservicehost-overview"></a>Visão geral de WorkflowServiceHost  

A <xref:System.ServiceModel.WorkflowServiceHost> classe é usada para hospedar serviços de fluxo de trabalho. Ele escuta mensagens de entrada e as roteia para a instância de serviço apropriada, criando novas instâncias ou carregando instâncias existentes do armazenamento durável, conforme necessário. O diagrama a seguir ilustra em um nível alto como <xref:System.ServiceModel.WorkflowServiceHost> funciona:
  
 ![Diagrama que mostra uma visão geral do host do serviço de fluxo de trabalho.](./media/workflow-service-host-internals/workflow-service-host-high-level-overview.gif)  
  
 Este diagrama mostra que o <xref:System.ServiceModel.WorkflowServiceHost> carrega definições de serviço de fluxo de trabalho de arquivos. xamlx e carrega informações de configuração de um arquivo de configuração. Ele também carrega a configuração de rastreamento do perfil de controle. <xref:System.ServiceModel.WorkflowServiceHost>expõe um ponto de extremidade de controle de fluxo de trabalho que permite enviar operações de controle para instâncias de fluxo de trabalho.  Para obter mais informações, consulte [exemplo de ponto de extremidade de controle de fluxo](workflow-control-endpoint.md)  
  
 <xref:System.ServiceModel.WorkflowServiceHost>também expõe os pontos de extremidade do aplicativo que escutam as mensagens de entrada do aplicativo. Quando uma mensagem de entrada chega, ela é enviada para a instância apropriada do serviço de fluxo de trabalho (se estiver carregada no momento). Se necessário, uma nova instância de fluxo de trabalho será criada. Ou se uma instância existente tiver sido persistida, ela será carregada do armazenamento de persistência.  
  
## <a name="workflowservicehost-details"></a>Detalhes do WorkflowServiceHost  
 O diagrama a seguir mostra como o <xref:System.ServiceModel.WorkflowServiceHost> lida com as mensagens em um pouco mais de detalhes:  
  
 ![Diagrama que mostra o fluxo de mensagens do host do serviço de fluxo de trabalho.](./media/workflow-service-host-internals/workflow-service-host-message-flow.gif)  
  
 Este diagrama mostra três pontos de extremidade diferentes, um ponto final do aplicativo, um ponto de extremidade de controle do fluxo de trabalho e um ponto de extremidade de hospedagem do fluxo de trabalho O ponto de extremidade do aplicativo recebe mensagens que estão associadas a uma instância de fluxo de trabalho específica. O ponto de extremidade de controle de fluxo de trabalho escuta operações de controle. O ponto de extremidade de hospedagem do fluxo de trabalho escuta mensagens que causam o <xref:System.ServiceModel.WorkflowServiceHost> carregamento e a execução de fluxos de trabalho que não são de serviço. Conforme mostrado no diagrama, todas as mensagens são processadas por meio do tempo de execução do WCF.  A limitação da instância do serviço de fluxo de trabalho é obtida usando a <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentInstances%2A> propriedade. Essa propriedade limitará o número de instâncias de serviço de fluxo de trabalho simultâneas. Quando essa restrição for excedida, quaisquer solicitações adicionais para novas instâncias de serviço de fluxo de trabalho ou solicitações para ativar instâncias de fluxo de trabalho persistentes serão enfileiradas. As solicitações enfileiradas são processadas na ordem FIFO, independentemente de serem solicitações de uma nova instância ou de uma instância persistente em execução. As informações de política de host são carregadas que determinam como as exceções sem tratamento são tratadas e como os serviços de fluxo de trabalho ociosos são descarregados e persistidos. Para obter mais informações sobre esses tópicos, consulte [como: configurar o comportamento de exceção sem tratamento do fluxo de trabalho com o WorkflowServiceHost](config-workflow-unhandled-exception-workflowservicehost.md) e [como: configurar o comportamento ocioso com o WorkflowServiceHost](how-to-configure-idle-behavior-with-workflowservicehost.md). As instâncias de fluxo de trabalho são persistidas de acordo com as políticas de host e são recarregadas quando necessário. Para obter mais informações sobre a persistência do fluxo de trabalho, consulte: [como configurar a persistência com o WorkflowServiceHost](how-to-configure-persistence-with-workflowservicehost.md), [criar um serviço de fluxo de trabalho de execução longa](creating-a-long-running-workflow-service.md)e [persistência do fluxo de trabalho](../../windows-workflow-foundation/workflow-persistence.md).  
  
 A ilustração a seguir mostra o fluxo quando WorkflowServiceHost. Open é chamado:  
  
 ![O diagrama que mostra o fluxo quando WorkflowServiceHost. Open é chamado.](./media/workflow-service-host-internals/workflow-service-host-open.gif)  
  
 O fluxo de trabalho é carregado do XAML e a árvore de atividades é criada. <xref:System.ServiceModel.WorkflowServiceHost>percorre a árvore de atividades e cria a descrição do serviço. A configuração é aplicada ao host. Por fim, o host começa a escutar mensagens de entrada.  
  
 A ilustração a seguir mostra o que o <xref:System.ServiceModel.WorkflowServiceHost> faz quando recebe uma mensagem associada a uma atividade Receive que tem CanCreateInstance definido como `true` :  
  
 ![Árvore de decisão usada pelo host WFS quando recebe uma mensagem e CanCreateInstance é verdadeiro.](./media/workflow-service-host-internals/workflow-service-host-receive-message-cancreateinstance.gif)  
  
 A mensagem chega e é processada pela pilha de canais do WCF. As restrições são verificadas e as consultas de correlação são executadas. Se a mensagem estiver associada a uma instância existente, a mensagem será entregue. Se uma nova instância precisar ser criada, a propriedade CanCreateInstance da atividade Receive será marcada. Se estiver definido como true, uma nova instância será criada e a mensagem será entregue.  
  
 A ilustração a seguir mostra o que o <xref:System.ServiceModel.WorkflowServiceHost> faz quando recebe uma mensagem associada a uma atividade Receive que tem CanCreateInstance definido como false.  
  
 ![A árvore de decisão usada pelo host WFS quando recebe uma mensagem e CanCreateInstance é false.](./media/workflow-service-host-internals/workflow-service-host-receive-message.gif)  
  
 A mensagem chega e é processada pela pilha de canais do WCF. As restrições são verificadas e as consultas de correlação são executadas. A mensagem é associada a uma instância existente (porque CanCreateInstance é false), portanto, a instância é carregada do repositório de persistência, o indicador é retomado e o fluxo de trabalho é executado.  
  
> [!WARNING]
> O host do serviço de fluxo de trabalho não será aberto se SQL Server estiver configurado para escutar somente no protocolo NamedPipe.  
  
## <a name="see-also"></a>Confira também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Serviços de fluxo de trabalho de hospedagem](hosting-workflow-services.md)
- [Ponto de extremidade de controle de fluxo de trabalho](workflow-control-endpoint.md)
- [Como configurar um comportamento de exceção não tratado de fluxo de trabalho com o WorkflowServiceHost](config-workflow-unhandled-exception-workflowservicehost.md)
- [Criando um serviço de fluxo de trabalho de execução longa](creating-a-long-running-workflow-service.md)
- [Persistência de fluxo de trabalho](../../windows-workflow-foundation/workflow-persistence.md)
