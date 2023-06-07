---
title: Como configurar persistência com WorkflowServiceHost
ms.date: 03/30/2017
ms.assetid: e31cd4df-13a3-4a9a-9be8-5243e0055356
ms.openlocfilehash: 4ed9c76f091e75cf6ba7658f0314d2e21bbe962e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599107"
---
# <a name="how-to-configure-persistence-with-workflowservicehost"></a>Como configurar persistência com WorkflowServiceHost
Este tópico descreve como configurar o recurso de repositório de instância do fluxo de trabalho SQL para habilitar a persistência para fluxos de trabalho hospedados no <xref:System.ServiceModel.Activities.WorkflowServiceHost> usando um arquivo de configuração. Antes de usar o recurso de repositório de instância do fluxo de trabalho SQL, você deve criar um banco de dados SQL que é usado para manter instâncias de fluxo de trabalho. Para obter mais informações, consulte [como habilitar a persistência de SQL para fluxos de trabalho e serviços de fluxo de trabalho](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md).  
  
### <a name="to-configure-the-sql-workflow-instance-store-in-configuration"></a>Para configurar o repositório de instância do fluxo de trabalho SQL na configuração  
  
1. As propriedades do armazenamento da instância do fluxo de trabalho SQL podem ser configuradas por meio do <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> , um comportamento de serviço que permite alterar as configurações por meio da configuração de XML. O exemplo de configuração a seguir mostra como configurar o armazenamento de instância de fluxo de trabalho SQL usando o `sqlWorkflowInstanceStore` elemento de comportamento <> em um arquivo de configuração.  
  
    ```xml  
    <serviceBehaviors>  
        <behavior name="">  
            <sqlWorkflowInstanceStore
                 connectionString="provider=System.Data.SqlClient;Data Source=(local);Initial Catalog=DefaultPersistenceProviderDb;Integrated Security=True;Async=true"  
                 instanceEncodingOption="GZip | None"  
                 instanceCompletionAction="DeleteAll | DeleteNothing"  
                 instanceLockedExceptionAction="NoRetry | SimpleRetry | AggressiveRetry"  
                 hostLockRenewalPeriod="00:00:30"
                 runnableInstancesDetectionPeriod="00:00:05">  
            </sqlWorkflowInstanceStore>  
        </behavior>  
    </serviceBehaviors>  
    ```  
  
     Para obter mais informações sobre como configurar o repositório de instância do fluxo de trabalho SQL, consulte [como habilitar a persistência de SQL para fluxos de trabalho e serviços de fluxo de trabalho](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md). Para obter mais informações sobre as configurações individuais para o `sqlWorkflowInstanceStore` elemento de comportamento <>, consulte [repositório de instâncias de fluxo de trabalho SQL](../../windows-workflow-foundation/sql-workflow-instance-store.md). O Windows Server app Fabric fornece seu próprio armazenamento de persistência. Para obter mais informações, consulte [persistência do Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677272(v=azure.10)).  
  
    > [!NOTE]
    > O exemplo de configuração anterior usa configuração simplificada. Para obter mais informações, consulte [configuração simplificada](../simplified-configuration.md)  
  
### <a name="to-configure-the-sql-workflow-instance-store-in-code"></a>Para configurar o repositório de instância do fluxo de trabalho SQL no código  
  
1. As propriedades do armazenamento da instância do fluxo de trabalho SQL podem ser configuradas por meio do <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> , um comportamento de serviço que permite que você altere as configurações por meio de código. O exemplo a seguir mostra como configurar o armazenamento de instância do fluxo de trabalho SQL usando o <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> elemento Behavior em um código  
  
    ```csharp  
    host.Description.Behaviors.Add(new SqlWorkflowInstanceStoreBehavior  
    {  
       ConnectionString = "provider=System.Data.SqlClient;Data Source=(local);Initial Catalog=DefaultPersistenceProviderDb;Integrated Security=True;Async=true",  
       InstanceEncodingOption = "GZip | None",  
       InstanceCompletionAction = "DeleteAll | DeleteNothing",  
       InstanceLockedExceptionAction = "NoRetry | SimpleRetry | AggressiveRetry",  
       HostLockRenewalPeriod = new TimeSpan(00, 00, 30),  
       RunnableInstancesDetectionPeriod = new TimeSpan(00, 00, 05)  
    });  
    ```  
  
     Para obter mais informações sobre como configurar o repositório de instância do fluxo de trabalho SQL, consulte [como habilitar a persistência de SQL para fluxos de trabalho e serviços de fluxo de trabalho](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md). Para obter mais informações sobre as configurações individuais do <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> elemento de comportamento, consulte [repositório de instância de fluxo de trabalho SQL](../../windows-workflow-foundation/sql-workflow-instance-store.md). O Windows Server app Fabric fornece seu próprio armazenamento de persistência. Para obter mais informações, consulte [persistência do Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677272(v=azure.10)).  
  
    > [!NOTE]
    > O exemplo de configuração anterior usa configuração simplificada. Para obter mais informações, consulte [configuração simplificada](../simplified-configuration.md)  
  
     Para obter um exemplo de como configurar a persistência programaticamente [, consulte Como habilitar a persistência para fluxos de trabalho e serviços de fluxo de trabalho](../../windows-workflow-foundation/how-to-enable-persistence-for-workflows-and-workflow-services.md).  
  
## <a name="see-also"></a>Consulte também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Persistência de fluxo de trabalho](../../windows-workflow-foundation/workflow-persistence.md)
- [Persistência da tela de aplicativo Windows Server](https://docs.microsoft.com/previous-versions/appfabric/ee677272(v=azure.10))
