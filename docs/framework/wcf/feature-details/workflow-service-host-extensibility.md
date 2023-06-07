---
title: Extensibilidade de host de serviço do fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: c0e8f7bb-cb13-49ec-852f-b85d7c23972f
ms.openlocfilehash: 4c2edec8c23e27f019b95223c998c72069384c75
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594888"
---
# <a name="workflow-service-host-extensibility"></a>Extensibilidade de host de serviço do fluxo de trabalho
[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]fornece a <xref:System.ServiceModel.Activities.WorkflowServiceHost> classe para hospedar serviços de fluxo de trabalho. Essa classe é usada quando você está hospedando internamente um serviço de fluxo de trabalho em um aplicativo gerenciado ou em um serviço do Windows. Essa classe também é usada ao hospedar um serviço de fluxo de trabalho com Serviços de Informações da Internet (IIS) ou o WAS (serviço de ativação de processos do Windows). A <xref:System.ServiceModel.Activities.WorkflowServiceHost> classe fornece pontos de extensão que permitem adicionar extensões personalizadas, alterar o comportamento ocioso e hospedar fluxos de trabalho que não são de serviço (fluxos de trabalho que não usam atividades de mensagens).  
  
## <a name="workflow-service-host-extensions"></a>Extensões de host do serviço de fluxo de trabalho  
 O <xref:System.ServiceModel.Activities.WorkflowServiceHost> contém uma <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> Propriedade do tipo <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager> que fornece métodos para adicionar extensões ao <xref:System.ServiceModel.Activities.WorkflowServiceHost> . Use o <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager.Add%2A> método para adicionar uma extensão para cada instância de serviço de fluxo de trabalho. O delegado especificado é chamado para criar uma nova extensão quando uma instância do serviço de fluxo de trabalho é criada ou carregada de um repositório de persistência. Use o <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager.Add%2A> método para adicionar uma extensão para cada host de serviço de fluxo de trabalho, uma instância da extensão é compartilhada para todas as instâncias de serviço de fluxo de trabalho.  
  
## <a name="react-to-unhandled-exceptions"></a>Reagir a exceções sem tratamento  
 O <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> permite que você especifique a ação a ser tomada se uma exceção sem tratamento ocorrer em um serviço de fluxo de trabalho. A <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior.Action%2A> propriedade especifica um dos <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction> valores:  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Abandon>– Anula a instância do serviço de fluxo de trabalho.  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.AbandonAndSuspend>– Reverte para o último estado persistido e suspende a instância do serviço de fluxo de trabalho. Isso só ocorrerá se o fluxo de trabalho já tiver sido persistido pelo menos uma vez. Se não for, a instância do fluxo de trabalho será anulada.  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Cancel>– Cancela a instância.  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Terminate>– Encerra a instância.  
  
 Esse comportamento pode ser configurado no código, conforme mostrado no exemplo a seguir.  
  
```csharp  
host.Description.Behaviors.Add(new WorkflowUnhandledExceptionBehavior { Action = WorkflowUnhandledExceptionAction.Abandon });  
```  
  
 Ele também pode ser configurado em um arquivo de configuração, conforme mostrado no exemplo a seguir.  
  
```xml
<behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <workflowUnhandledExceptionBehavior action="Abandon" />
        </behavior>  
      </serviceBehaviors>  
</behaviors>
```  
  
## <a name="hosting-non-service-workflows"></a>Hospedando fluxos de trabalho que não são de serviço  
 <xref:System.ServiceModel.Activities.WorkflowServiceHost>pode ser usado para hospedar fluxos de trabalho que não são de serviço ou fluxos de trabalho que não começam com uma <xref:System.ServiceModel.Activities.Receive> atividade ou fluxos de trabalho que não usam as atividades de mensagens. Os serviços de fluxo de trabalho normalmente começam com uma <xref:System.ServiceModel.Activities.Receive> atividade. Quando o <xref:System.ServiceModel.Activities.WorkflowServiceHost> recebe uma mensagem para um serviço de fluxo de trabalho, se ele ainda não estiver em execução (ou persistiu), uma nova instância de serviço de fluxo de trabalho será criada. Se um fluxo de trabalho não começar com uma atividade Receive, ele não poderá ser iniciado enviando uma mensagem porque não há nenhuma atividade para receber a mensagem. Para hospedar um fluxo de trabalho que não seja de serviço, derive uma classe de <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> e substitua <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetInstanceId%2A> , <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetCreationContext%2A> e <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnResolveBookmark%2A> . Substitua <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetInstanceId%2A> se você quiser fornecer uma ID de instância preferencial. Substitua <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetCreationContext%2A> para criar um contexto de criação de fluxo de trabalho personalizado ou preencha uma instância do existente <xref:System.ServiceModel.Activities.WorkflowCreationContext> . Substitua <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnResolveBookmark%2A> para extrair manualmente o indicador da mensagem de entrada. Se você substituir esse método, deverá invocar <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A> em seu corpo para responder à mensagem enviada para o WorkflowHostingEndpoint. Se você não fizer isso, um <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> limite pode ser eventualmente excedido. Em contratos bidirecionais, você poderá detectar sua falha para invocar devido à <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A> falha do cliente em receber uma resposta. Em contratos unidirecionais, você pode não reconhecer o erro de falha ao chamar <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A> até que seja tarde demais, depois que o <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> limite de limitação for excedido. Para obter mais informações, consulte o [indicador de retomada do WorkflowHostingEndpoint](../../windows-workflow-foundation/samples/workflowhostingendpoint-resume-bookmark.md)para criar uma nova instância de um fluxo de trabalho que não seja de serviço, declare um contrato de serviço que define uma operação que cria uma nova instância. A operação de criação deve levar um IDictionary \<string, object> para passar qualquer parâmetro de fluxo de trabalho necessário. Esse contrato é implementado implicitamente pela <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> classe derivada. Ao hospedar o fluxo de trabalho, adicione uma instância da <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> classe derivada ao host chamando <xref:System.ServiceModel.Activities.WorkflowServiceHost.AddServiceEndpoint%2A> e chamar <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> . Para criar uma instância do fluxo de trabalho, crie um <xref:System.ServiceModel.ChannelFactory%601> do seu tipo de contrato de serviço e chame <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> . Em seguida, você pode chamar a operação de criação definida em seu contrato de serviço.  
  
## <a name="see-also"></a>Confira também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Atividades de mensagem](messaging-activities.md)
