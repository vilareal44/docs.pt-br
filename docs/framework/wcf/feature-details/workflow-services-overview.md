---
title: Visão geral dos serviços de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: e536dda3-e286-441e-99a7-49ddc004b646
ms.openlocfilehash: f752eca621f9d30f38d85d7e71228fdfe1343c32
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594862"
---
# <a name="workflow-services-overview"></a>Visão geral dos serviços de fluxo de trabalho

Os serviços de fluxo de trabalho são serviços baseados no WCF que são implementados usando fluxos de trabalho. Os serviços de fluxo de trabalho são fluxos de trabalho que usam as atividades de mensagens para enviar e receber mensagens de Windows Communication Foundation (WCF). .NET Framework 4,5 apresenta várias atividades de mensagens que permitem que você envie e receba mensagens de dentro de um fluxo de trabalho. Para obter mais informações sobre as atividades de mensagens e como elas podem ser usadas para implementar diferentes padrões de troca de mensagens, consulte [atividades de mensagens](messaging-activities.md).

## <a name="benefits-of-using-workflow-services"></a>Benefícios do uso dos serviços de fluxo de trabalho

À medida que os aplicativos se tornam cada vez mais distribuídos, os serviços individuais se tornam responsáveis por chamar outros serviços para descarregar parte do trabalho. Implementar essas chamadas como operações assíncronas introduz alguma complexidade no código. O tratamento de erros adiciona mais complexidade na forma de tratamento de exceções e fornecimento de informações de acompanhamento detalhadas. Alguns serviços são geralmente de longa execução e podem ocupar recursos valiosos do sistema enquanto aguardam a entrada. Devido a esses problemas, os aplicativos distribuídos costumam ser muito complexos e difíceis de escrever e manter. Os fluxos de trabalho são uma maneira natural de expressar a coordenação de trabalhos assíncronos, especialmente chamadas para serviços externos. Os fluxos de trabalho também são eficazes na representação de processos de negócios de execução longa. São essas qualidades que tornam o fluxo de trabalho um grande ativo para a criação de serviços em um ambiente distribuído.

## <a name="implementing-a-workflow-service"></a>Implementando um serviço de fluxo de trabalho

Ao implementar um serviço WCF, você define um número de contratos que descrevem o serviço e os dados que ele envia e recebe. Os dados são representados como contratos de dados e contratos de mensagem. O WCF e os serviços de fluxo de trabalho usam as definições de contrato de dados e de contrato de mensagem como parte das descrições de serviço. O próprio serviço expõe metadados (na forma de WSDL) para descrever as operações do serviço. No WCF, os contratos de serviço e os contratos de operação definem o serviço e as operações que ele suporta. No entanto, em um serviço de fluxo de trabalho, esses contratos fazem parte do próprio processo de negócios. Eles são expostos em metadados por um processo chamado inferência de contrato. Quando um serviço de fluxo de trabalho é hospedado usando <xref:System.ServiceModel.Activities.WorkflowServiceHost> , a definição de fluxo de trabalho é examinada e um contrato é gerado com base no conjunto de atividades de mensagens encontradas no fluxo de trabalho. Em particular, as seguintes atividades e propriedades são usadas para gerar o contrato:

<xref:System.ServiceModel.Activities.Receive> Atividade

- <xref:System.ServiceModel.Activities.Receive.ServiceContractName%2A>

- <xref:System.ServiceModel.Activities.Receive.OperationName%2A>

- <xref:System.ServiceModel.Activities.Receive.Action%2A>

<xref:System.ServiceModel.Activities.SendReply> Atividade

- <xref:System.ServiceModel.Activities.SendReply.Action%2A>

<xref:System.ServiceModel.Activities.TransactedReceiveScope> Atividade

O resultado final da inferência de contrato é uma descrição do serviço usando as mesmas estruturas de dados que os serviços WCF e os contratos de operação. Essas informações são usadas para expor WSDL para o serviço de fluxo de trabalho.

> [!NOTE]
> [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)]Não permite que você grave serviços de fluxo de trabalho usando uma definição de contrato existente sem algum suporte de ferramentas adicional. Os contratos de serviço de fluxo de trabalho são criados pelo processo de inferência de contrato discutido anteriormente. No entanto, os contratos de mensagem e os contratos de dados têm suporte total.

## <a name="workflow-services-and-msmq-based-bindings"></a>Serviços de fluxo de trabalho e associações baseadas no MSMQ

O WCF define duas associações baseadas no MSMQ <xref:System.ServiceModel.NetMsmqBinding> e o <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> .  As associações baseadas no MSMQ geralmente são usadas com os serviços de fluxo de trabalho devido à natureza de longa execução desses serviços. As associações baseadas em MSMQ têm uma `ValidityDuration` propriedade que especifica por quanto tempo as mensagens do MSMQ podem assumir que são válidas. Devido à natureza de longa execução dos serviços de fluxo de trabalho, é possível que a duração da validade de uma mensagem MSMQ seja decorrida antes que o serviço de fluxo de trabalho possa processá-lo. Portanto, é muito importante definir a duração da validade de uma associação MSMQ com um valor apropriado. Esse valor deve ser escolhido com base no fluxo de trabalho e como ele processa as mensagens. Por exemplo, se você tiver um fluxo de trabalho com uma <xref:System.ServiceModel.Activities.Receive> atividade seguida por uma atividade personalizada que leva 10 minutos para ser executada, seguida por outra <xref:System.ServiceModel.Activities.Receive> atividade, o valor correto para `ValidityDuration` seria maior que 10 minutos.

## <a name="hosting-a-workflow-service"></a>Hospedando um serviço de fluxo de trabalho

Como os serviços WCF, os serviços de fluxo de trabalho devem ser hospedados. Os serviços WCF usam a <xref:System.ServiceModel.ServiceHost> classe para hospedar serviços e serviços de fluxo de trabalho usados <xref:System.ServiceModel.Activities.WorkflowServiceHost> para hospedar serviços. Como os serviços WCF, os serviços de fluxo de trabalho podem ser hospedados de várias maneiras, por exemplo:

- Em um aplicativo .NET Framework gerenciado.

- No Serviços de Informações da Internet (IIS).

- No WAS (serviço de ativação de processos do Windows).

- Em um serviço do Windows gerenciado.

Os serviços de fluxo de trabalho hospedados em um aplicativo .NET Framework gerenciado ou um serviço gerenciado do Windows criam uma instância da <xref:System.ServiceModel.Activities.WorkflowServiceHost> classe e passam a ela uma instância do <xref:System.ServiceModel.Activities.WorkflowService> que contém a definição de fluxo de trabalho dentro da <xref:System.ServiceModel.Activities.WorkflowService.Body%2A> propriedade. Uma definição de fluxo de trabalho que contém atividades de mensagens é exposta como um serviço de fluxo de trabalho.

Para hospedar um serviço de fluxo de trabalho no IIS ou WAS, coloque o arquivo. xamlx que contém a definição do serviço de fluxo de trabalho em um diretório virtual. Um ponto de extremidade padrão (usando <xref:System.ServiceModel.BasicHttpBinding> ) é criado automaticamente para obter mais informações, consulte [configuração simplificada](../simplified-configuration.md). Você também pode inserir um arquivo Web. config no diretório virtual para especificar seus próprios pontos de extremidade. Se sua definição de fluxo de trabalho estiver em um assembly, você poderá inserir um arquivo. svc no diretório virtual e no assembly do fluxo de trabalho no diretório App_Code. O arquivo. svc deve especificar a fábrica de hosts de serviço e a classe que implementa o serviço de fluxo de trabalho. O exemplo a seguir mostra como especificar a fábrica de hosts de serviço e especificar a classe que implementa o serviço de fluxo de trabalho.

```
<%@ServiceHost Factory=" System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory
Service="EchoService"%>
```
