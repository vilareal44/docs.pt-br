---
title: Inicialização de instancialização
ms.date: 03/30/2017
ms.assetid: 154d049f-2140-4696-b494-c7e53f6775ef
ms.openlocfilehash: 06a8dfe571b652ded236df3097b37861c03a858d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596650"
---
# <a name="instancing-initialization"></a>Inicialização de instancialização
Este exemplo estende o exemplo de [pooling](pooling.md) definindo uma interface, `IObjectControl` , que personaliza a inicialização de um objeto ativando-o e desativando-o. O cliente invoca métodos que retornam o objeto ao pool e que não retornam o objeto para o pool.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
## <a name="extensibility-points"></a>Pontos de extensibilidade  
 A primeira etapa na criação de uma extensão Windows Communication Foundation (WCF) é decidir o ponto de extensibilidade a ser usado. No WCF, o termo *EndpointDispatcher* refere-se a um componente de tempo de execução responsável pela conversão de mensagens de entrada em invocações de método no serviço do usuário e pela conversão de valores de retorno desse método em uma mensagem de saída. Um serviço WCF cria um EndpointDispatcher para cada ponto de extremidade.  
  
 O EndpointDispatcher oferece a extensibilidade do escopo do ponto de extremidade (para todas as mensagens recebidas ou enviadas pela serviço) usando a <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> classe. Essa classe permite que você personalize várias propriedades que controlam o comportamento do EndpointDispatcher. Este exemplo se concentra na <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> propriedade que aponta para o objeto que fornece as instâncias da classe de serviço.  
  
## <a name="iinstanceprovider"></a>IInstanceProvider  
 No WCF, o EndpointDispatcher cria instâncias de uma classe de serviço usando um provedor de instância que implementa a <xref:System.ServiceModel.Dispatcher.IInstanceProvider> interface. Esta interface tem apenas dois métodos:  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A>: Quando uma mensagem chega, o Dispatcher chama o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> método para criar uma instância da classe de serviço para processar a mensagem. A frequência das chamadas para esse método é determinada pela <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade. Por exemplo, se a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade for definida como <xref:System.ServiceModel.InstanceContextMode.PerCall?displayProperty=nameWithType> , uma nova instância da classe de serviço será criada para processar cada mensagem que chega, portanto, <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> é chamado sempre que uma mensagem chega.  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%2A>: Quando a instância do serviço termina de processar a mensagem, o EndpointDispatcher chama o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%2A> método. Como no <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> método, a frequência das chamadas para esse método é determinada pela <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade.  
  
## <a name="the-object-pool"></a>O pool de objetos  
 A `ObjectPoolInstanceProvider` classe contém a implementação para o pool de objetos. Essa classe implementa a <xref:System.ServiceModel.Dispatcher.IInstanceProvider> interface para interagir com a camada de modelo de serviço. Quando o EndpointDispatcher chama o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> método, em vez de criar uma nova instância, a implementação personalizada procura um objeto existente em um pool na memória. Se houver um disponível, ele será retornado. Caso contrário, `ObjectPoolInstanceProvider` o verifica se a `ActiveObjectsCount` Propriedade (número de objetos retornados do pool) atingiu o tamanho máximo do pool. Caso contrário, uma nova instância será criada e retornada ao chamador e `ActiveObjectsCount` será subsequentemente incrementada. Caso contrário, uma solicitação de criação de objeto será enfileirada por um período de tempo configurado. A implementação do `GetObjectFromThePool` é mostrada no código de exemplo a seguir.  
  
```csharp
private object GetObjectFromThePool()  
{  
    bool didNotTimeout =
       availableCount.WaitOne(creationTimeout, true);  
    if(didNotTimeout)  
    {  
         object obj = null;  
         lock (poolLock)  
        {  
             if (pool.Count != 0)  
             {  
                   obj = pool.Pop();  
                   activeObjectsCount++;  
             }  
             else if (pool.Count == 0)  
             {  
                   if (activeObjectsCount < maxPoolSize)  
                   {  
                        obj = CreateNewPoolObject();  
                        activeObjectsCount++;  
  
                        #if (DEBUG)  
                        WritePoolMessage(  
                             ResourceHelper.GetString("MsgNewObject"));  
                       #endif  
                   }
            }  
           idleTimer.Stop();  
      }  
     // Call the Activate method if possible.  
    if (obj is IObjectControl)  
   {  
         ((IObjectControl)obj).Activate();  
   }  
   return obj;  
}  
throw new TimeoutException(  
ResourceHelper.GetString("ExObjectCreationTimeout"));  
}  
```  
  
 A `ReleaseInstance` implementação personalizada adiciona a instância liberada de volta ao pool e decrementa o `ActiveObjectsCount` valor. O EndpointDispatcher pode chamar esses métodos de threads diferentes e, portanto, o acesso sincronizado aos membros de nível de classe na `ObjectPoolInstanceProvider` classe é necessário.  
  
```csharp
public void ReleaseInstance(InstanceContext instanceContext, object instance)  
{  
    lock (poolLock)  
    {  
        // Check whether the object can be pooled.
        // Call the Deactivate method if possible.  
        if (instance is IObjectControl)  
        {  
            IObjectControl objectControl = (IObjectControl)instance;  
            objectControl.Deactivate();  
  
            if (objectControl.CanBePooled)  
            {  
                pool.Push(instance);  
  
                #if(DEBUG)  
                WritePoolMessage(  
                    ResourceHelper.GetString("MsgObjectPooled"));  
                #endif
            }  
            else  
            {  
                #if(DEBUG)  
                WritePoolMessage(  
                    ResourceHelper.GetString("MsgObjectWasNotPooled"));  
                #endif  
            }  
        }  
        else  
        {  
            pool.Push(instance);  
  
            #if(DEBUG)  
            WritePoolMessage(  
                ResourceHelper.GetString("MsgObjectPooled"));  
            #endif
        }  
  
        activeObjectsCount--;  
  
        if (activeObjectsCount == 0)  
        {  
            idleTimer.Start();
        }  
    }  
  
    availableCount.Release(1);  
}  
```  
  
 O `ReleaseInstance` método fornece um recurso de *inicialização de limpeza* . Normalmente, o pool mantém um número mínimo de objetos durante o tempo de vida do pool. No entanto, pode haver períodos de uso excessivo que exigem a criação de objetos adicionais no pool para alcançar o limite máximo especificado na configuração. Eventualmente, quando o pool se torna menos ativo, os objetos excedentes podem se tornar uma sobrecarga extra. Portanto, quando o `activeObjectsCount` chega a zero, um temporizador ocioso é iniciado e executa um ciclo de limpeza.  
  
```csharp  
if (activeObjectsCount == 0)  
{  
    idleTimer.Start();
}  
```  
  
 As extensões de camada de ServiceModel são conectadas usando os seguintes comportamentos:  
  
- Comportamentos de serviço: permitem a personalização de todo o tempo de execução do serviço.  
  
- Comportamentos de ponto de extremidade: permitem a personalização de um ponto de extremidade de serviço específico, incluindo o EndpointDispatcher.  
  
- Comportamentos de contrato: permitem a personalização de qualquer uma <xref:System.ServiceModel.Dispatcher.ClientRuntime> das <xref:System.ServiceModel.Dispatcher.DispatchRuntime> classes ou do cliente ou do serviço, respectivamente.  
  
- Comportamentos de operação: permitem a personalização de qualquer uma <xref:System.ServiceModel.Dispatcher.ClientOperation> das <xref:System.ServiceModel.Dispatcher.DispatchOperation> classes ou do cliente ou do serviço, respectivamente.  
  
 Para fins de uma extensão de pool de objetos, um comportamento de ponto de extremidade ou um comportamento de serviço pode ser criado. Neste exemplo, usamos um comportamento de serviço, que aplica a capacidade de pooling de objetos a todos os pontos de extremidade do serviço. Os comportamentos de serviço são criados pela implementação da <xref:System.ServiceModel.Description.IServiceBehavior> interface. Há várias maneiras de tornar o ServiceModel ciente dos comportamentos personalizados:  
  
- Usando um atributo personalizado.  
  
- Adicionando imperativamente à coleção de comportamentos da descrição do serviço.  
  
- Estendendo o arquivo de configuração.  
  
 Este exemplo usa um atributo personalizado. Quando o <xref:System.ServiceModel.ServiceHost> é construído, ele examina os atributos usados na definição de tipo do serviço e adiciona os comportamentos disponíveis à coleção de comportamentos da descrição do serviço.  
  
 A <xref:System.ServiceModel.Description.IServiceBehavior> interface tem três métodos: <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> `,` <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> `,` e <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> . Esses métodos são chamados pelo WCF quando o <xref:System.ServiceModel.ServiceHost> está sendo inicializado. <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A?displayProperty=nameWithType>é chamado primeiro; Ele permite que o serviço seja inspecionado quanto a inconsistências. <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A?displayProperty=nameWithType>é chamado em avançar; Esse método só é necessário em cenários muito avançados. <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A?displayProperty=nameWithType>é chamado por último e é responsável por configurar o tempo de execução. Os parâmetros a seguir são passados em <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A?displayProperty=nameWithType> :  
  
- `Description`: Esse parâmetro fornece a descrição do serviço para todo o serviço. Isso pode ser usado para inspecionar dados de descrição sobre os pontos de extremidade do serviço, os contratos, as associações e outros dados associados ao serviço.  
  
- `ServiceHostBase`: Esse parâmetro fornece o <xref:System.ServiceModel.ServiceHostBase> que está sendo inicializado no momento.  
  
 Na implementação personalizada <xref:System.ServiceModel.Description.IServiceBehavior> , uma nova instância do `ObjectPoolInstanceProvider` é instanciada e atribuída à <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> propriedade em cada uma <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> que está anexada ao <xref:System.ServiceModel.ServiceHostBase> .  
  
```csharp
public void ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
{  
    if (enabled)  
    {  
        // Create an instance of the ObjectPoolInstanceProvider.  
        instanceProvider = new ObjectPoolInstanceProvider(description.ServiceType,  
        maxPoolSize, minPoolSize, creationTimeout);  
  
        // Assign our instance provider to Dispatch behavior in each
        // endpoint.  
        foreach (ChannelDispatcherBase cdb in serviceHostBase.ChannelDispatchers)  
        {  
             ChannelDispatcher cd = cdb as ChannelDispatcher;  
             if (cd != null)  
             {  
                 foreach (EndpointDispatcher ed in cd.Endpoints)  
                 {  
                        ed.DispatchRuntime.InstanceProvider = instanceProvider;  
                 }  
             }  
         }  
     }  
}
```  
  
 Além de uma <xref:System.ServiceModel.Description.IServiceBehavior> implementação, a `ObjectPoolingAttribute` classe tem vários membros para personalizar o pool de objetos usando os argumentos de atributo. Esses membros incluem `MaxSize` , `MinSize` `Enabled` e `CreationTimeout` , para corresponder ao conjunto de recursos do pool de objetos fornecido pelo .net Enterprise Services.  
  
 O comportamento do pool de objetos agora pode ser adicionado a um serviço do WCF anotando a implementação do serviço com o atributo personalizado recém-criado `ObjectPooling` .  
  
```csharp  
[ObjectPooling(MaxSize=1024, MinSize=10, CreationTimeout=30000]
public class PoolService : IPoolService  
{  
  // …  
}  
```  
  
## <a name="hooking-activation-and-deactivation"></a>Conectando ativação e desativação  
 O objetivo principal do pool de objetos é otimizar objetos de curta duração com criação e inicialização relativamente caras. Portanto, ele pode dar um aumento considerável no desempenho para um aplicativo, se usado corretamente. Como o objeto é retornado do pool, o construtor é chamado apenas uma vez. No entanto, alguns aplicativos exigem algum nível de controle para que possam inicializar e limpar os recursos usados durante um único contexto. Por exemplo, um objeto que está sendo usado para um conjunto de cálculos pode redefinir seus campos particulares antes de processar o próximo cálculo. Os serviços corporativos habilitaram esse tipo de inicialização específica de contexto, permitindo que o desenvolvedor do objeto substitua `Activate` e `Deactivate` métodos da <xref:System.EnterpriseServices.ServicedComponent> classe base.  
  
 O pool de objetos chama o `Activate` método logo antes de retornar o objeto do pool. `Deactivate`é chamado quando o objeto retorna de volta para o pool. A <xref:System.EnterpriseServices.ServicedComponent> classe base também tem uma `boolean` propriedade chamada `CanBePooled` , que pode ser usada para notificar o pool se o objeto pode ser mais agrupado.  
  
 Para imitar essa funcionalidade, o exemplo declara uma interface pública ( `IObjectControl` ) que tem os membros mencionados anteriormente. Essa interface é então implementada por classes de serviço destinadas a fornecer inicialização específica de contexto. A <xref:System.ServiceModel.Dispatcher.IInstanceProvider> implementação deve ser modificada para atender a esses requisitos. Agora, sempre que você receber um objeto chamando o `GetInstance` método, deverá verificar se o objeto implementa `IObjectControl.` se ele faz isso, você deve chamar o `Activate` método adequadamente.  
  
```csharp  
if (obj is IObjectControl)  
{  
    ((IObjectControl)obj).Activate();  
}  
```  
  
 Ao retornar um objeto para o pool, uma verificação é necessária para a `CanBePooled` propriedade antes de adicionar o objeto de volta ao pool.  
  
```csharp  
if (instance is IObjectControl)  
{  
    IObjectControl objectControl = (IObjectControl)instance;  
    objectControl.Deactivate();  
    if (objectControl.CanBePooled)  
    {  
       pool.Push(instance);  
    }  
}  
```  
  
 Como o desenvolvedor do serviço pode decidir se um objeto pode ser agrupado, a contagem de objetos no pool em um determinado momento pode ficar abaixo do tamanho mínimo. Portanto, você deve verificar se a contagem de objetos esteve abaixo do nível mínimo e executar a inicialização necessária no procedimento de limpeza.  
  
```csharp  
// Remove the surplus objects.  
if (pool.Count > minPoolSize)  
{  
  // Clean the surplus objects.  
}
else if (pool.Count < minPoolSize)  
{  
  // Reinitialize the missing objects.  
  while(pool.Count != minPoolSize)  
  {  
    pool.Push(CreateNewPoolObject());  
  }  
}  
```  
  
 Quando você executa o exemplo, as solicitações e respostas da operação são exibidas nas janelas do console do cliente e do serviço. Pressione Enter em cada janela do console para desligar o serviço e o cliente.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Initialization`  
