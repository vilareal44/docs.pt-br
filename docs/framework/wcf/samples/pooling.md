---
title: Agrupamento
ms.date: 03/30/2017
ms.assetid: 688dfb30-b79a-4cad-a687-8302f8a9ad6a
ms.openlocfilehash: 82b81637deb0715d19109794348d2a2bcda7f0d9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594615"
---
# <a name="pooling"></a>Agrupamento
Este exemplo demonstra como estender Windows Communication Foundation (WCF) para dar suporte ao pooling de objetos. O exemplo demonstra como criar um atributo que seja sintaticamente e semanticamente semelhante à `ObjectPoolingAttribute` funcionalidade de atributo dos serviços corporativos. O pooling de objetos pode fornecer um aumento considerável para o desempenho de um aplicativo. No entanto, ele pode ter o efeito oposto se não for usado corretamente. O pooling de objetos ajuda a reduzir a sobrecarga de recriação de objetos usados com frequência que exigem inicialização extensiva. No entanto, se uma chamada para um método em um objeto em pool levar um tempo considerável para ser concluída, o pooling de objetos enfileirará solicitações adicionais assim que o tamanho máximo do pool for atingido. Portanto, ele pode falhar ao fornecer algumas solicitações de criação de objeto lançando uma exceção de tempo limite.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 A primeira etapa na criação de uma extensão WCF é decidir o ponto de extensibilidade a ser usado.  
  
 No WCF, o termo *Dispatcher* refere-se a um componente de tempo de execução responsável pela conversão de mensagens de entrada em invocações de método no serviço do usuário e pela conversão de valores de retorno desse método em uma mensagem de saída. Um serviço WCF cria um Dispatcher para cada ponto de extremidade. Um cliente WCF deve usar um Dispatcher se o contrato associado a esse cliente for um contrato duplex.  
  
 Os despachantes de canal e ponto de extremidade oferecem extensibilidade de canal e de todo o contrato expondo várias propriedades que controlam o comportamento do Dispatcher. A <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.DispatchRuntime%2A> propriedade também permite inspecionar, modificar ou personalizar o processo de expedição. Este exemplo se concentra na <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> propriedade que aponta para o objeto que fornece as instâncias da classe de serviço.  
  
## <a name="the-iinstanceprovider"></a>O IInstanceProvider  
 No WCF, o Dispatcher cria instâncias da classe de serviço usando um <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> , que implementa a <xref:System.ServiceModel.Dispatcher.IInstanceProvider> interface. Essa interface tem três métodos:  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29>: Quando uma mensagem chega ao dispatcher chama o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> método para criar uma instância da classe de serviço para processar a mensagem. A frequência das chamadas para esse método é determinada pela <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade. Por exemplo, se a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade for definida como <xref:System.ServiceModel.InstanceContextMode.PerCall> uma nova instância da classe de serviço for criada para processar cada mensagem que chega, então <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> é chamado sempre que uma mensagem chega.  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%29>: Isso é idêntico ao método anterior, exceto que isso é invocado quando não há nenhum argumento de mensagem.  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29>: Quando o tempo de vida de uma instância de serviço tiver decorrido, o Dispatcher chamará o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29> método. Assim como para o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> método, a frequência das chamadas para esse método é determinada pela <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> propriedade.  
  
## <a name="the-object-pool"></a>O pool de objetos  
 Uma <xref:System.ServiceModel.Dispatcher.IInstanceProvider> implementação personalizada fornece a semântica de pool de objetos necessária para um serviço. Portanto, esse exemplo tem um `ObjectPoolingInstanceProvider` tipo que fornece implementação personalizada do <xref:System.ServiceModel.Dispatcher.IInstanceProvider> para pooling. Quando o `Dispatcher` chama o <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> método, em vez de criar uma nova instância, a implementação personalizada procura um objeto existente em um pool na memória. Se houver um disponível, ele será retornado. Caso contrário, um novo objeto será criado. A implementação do `GetInstance` é mostrada no código de exemplo a seguir.  
  
```csharp  
object IInstanceProvider.GetInstance(InstanceContext instanceContext, Message message)  
{  
    object obj = null;  
  
    lock (poolLock)  
    {  
        if (pool.Count > 0)  
        {  
            obj = pool.Pop();  
        }  
        else  
        {  
            obj = CreateNewPoolObject();  
        }  
        activeObjectsCount++;  
    }  
  
    WritePoolMessage(ResourceHelper.GetString("MsgNewObject"));  
  
    idleTimer.Stop();  
  
    return obj;
}  
```  
  
 A `ReleaseInstance` implementação personalizada adiciona a instância liberada de volta ao pool e decrementa o `ActiveObjectsCount` valor. O `Dispatcher` pode chamar esses métodos de threads diferentes e, portanto, o acesso sincronizado aos membros de nível de classe na `ObjectPoolingInstanceProvider` classe é necessário.  
  
```csharp  
void IInstanceProvider.ReleaseInstance(InstanceContext instanceContext, object instance)  
{  
    lock (poolLock)  
    {  
        pool.Push(instance);  
        activeObjectsCount--;  
  
        WritePoolMessage(  
        ResourceHelper.GetString("MsgObjectPooled"));  
  
        // When the service goes completely idle (no requests
        // are being processed), the idle timer is started  
        if (activeObjectsCount == 0)  
            idleTimer.Start();
    }  
}  
```  
  
 O `ReleaseInstance` método fornece um recurso de "inicialização de limpeza". Normalmente, o pool mantém um número mínimo de objetos durante o tempo de vida do pool. No entanto, pode haver períodos de uso excessivo que exigem a criação de objetos adicionais no pool para alcançar o limite máximo especificado na configuração. Eventualmente, quando o pool se torna menos ativo, esses objetos excedentes podem se tornar uma sobrecarga extra. Portanto, quando o `activeObjectsCount` chega a zero, um temporizador ocioso é iniciado que dispara e executa um ciclo de limpeza.  
  
## <a name="adding-the-behavior"></a>Adicionando o comportamento  
 As extensões de camada de Dispatcher são conectadas usando os seguintes comportamentos:  
  
- Comportamentos de serviço. Elas permitem a personalização de todo o tempo de execução do serviço.  
  
- Comportamentos de ponto de extremidade. Eles permitem a personalização de pontos de extremidade de serviço, especificamente um Dispatcher de canal e de ponto final.  
  
- Comportamentos de contrato. Eles permitem a personalização de ambas as <xref:System.ServiceModel.Dispatcher.ClientRuntime> classes e do <xref:System.ServiceModel.Dispatcher.DispatchRuntime> cliente e do serviço, respectivamente.  
  
 Para a finalidade de uma extensão de pool de objetos, um comportamento de serviço deve ser criado. Os comportamentos de serviço são criados pela implementação da <xref:System.ServiceModel.Description.IServiceBehavior> interface. Há várias maneiras de fazer com que o modelo de serviço reconheça os comportamentos personalizados:  
  
- Usando um atributo personalizado.  
  
- Adicionando imperativamente à coleção de comportamentos da descrição do serviço.  
  
- Estendendo o arquivo de configuração.  
  
 Este exemplo usa um atributo personalizado. Quando o <xref:System.ServiceModel.ServiceHost> é construído, ele examina os atributos usados na definição de tipo do serviço e adiciona os comportamentos disponíveis à coleção de comportamentos da descrição do serviço.  
  
 A interface <xref:System.ServiceModel.Description.IServiceBehavior> tem três métodos <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> ,, <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> e <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> . O <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> método é usado para garantir que o comportamento possa ser aplicado ao serviço. Neste exemplo, a implementação garante que o serviço não esteja configurado com <xref:System.ServiceModel.InstanceContextMode.Single> . O <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> método é usado para configurar as associações do serviço. Isso não é necessário neste cenário. O <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> é usado para configurar os expedidores do serviço. Esse método é chamado pelo WCF quando o <xref:System.ServiceModel.ServiceHost> está sendo inicializado. Os parâmetros a seguir são passados para este método:  
  
- `Description`: Esse argumento fornece a descrição do serviço para todo o serviço. Isso pode ser usado para inspecionar dados de descrição sobre os pontos de extremidade, contratos, associações e outros dados do serviço.  
  
- `ServiceHostBase`: Esse argumento fornece o <xref:System.ServiceModel.ServiceHostBase> que está sendo inicializado no momento.  
  
 Na implementação personalizada <xref:System.ServiceModel.Description.IServiceBehavior> , uma nova instância do `ObjectPoolingInstanceProvider` é instanciada e atribuída à <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> propriedade em cada uma <xref:System.ServiceModel.Dispatcher.DispatchRuntime> das ServiceHostBase.  
  
```csharp  
void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
{  
    // Create an instance of the ObjectPoolInstanceProvider.  
    ObjectPoolingInstanceProvider instanceProvider = new  
           ObjectPoolingInstanceProvider(description.ServiceType,
                                                    minPoolSize);  
  
    // Forward the call if we created a ServiceThrottlingBehavior.  
    if (this.throttlingBehavior != null)  
    {  
        ((IServiceBehavior)this.throttlingBehavior).ApplyDispatchBehavior(description, serviceHostBase);  
    }  
  
    // In case there was already a ServiceThrottlingBehavior
    // (this.throttlingBehavior==null), it should have initialized
    // a single ServiceThrottle on all ChannelDispatchers.
    // As we loop through the ChannelDispatchers, we verify that
    // and modify the ServiceThrottle to guard MaxPoolSize.  
    ServiceThrottle throttle = null;  
  
    foreach (ChannelDispatcherBase cdb in
            serviceHostBase.ChannelDispatchers)  
    {  
        ChannelDispatcher cd = cdb as ChannelDispatcher;  
        if (cd != null)  
        {  
            // Make sure there is exactly one throttle used by all
            // endpoints. If there were others, we could not enforce
            // MaxPoolSize.  
            if ((this.throttlingBehavior == null) &&
                        (this.maxPoolSize != Int32.MaxValue))  
            {  
                throttle ??= cd.ServiceThrottle;
                if (cd.ServiceThrottle == null)  
                {  
                    throw new
InvalidOperationException(ResourceHelper.GetString("ExNullThrottle"));  
                }  
                if (throttle != cd.ServiceThrottle)  
                {  
                    throw new InvalidOperationException(ResourceHelper.GetString("ExDifferentThrottle"));  
                }  
             }  
  
             foreach (EndpointDispatcher ed in cd.Endpoints)  
             {  
                 // Assign it to DispatchBehavior in each endpoint.  
                 ed.DispatchRuntime.InstanceProvider =
                                      instanceProvider;  
             }  
         }  
     }  
  
     // Set the MaxConcurrentInstances to limit the number of items
     // that will ever be requested from the pool.  
     if ((throttle != null) && (throttle.MaxConcurrentInstances >
                                      this.maxPoolSize))  
     {  
         throttle.MaxConcurrentInstances = this.maxPoolSize;  
     }  
}  
```  
  
 Além de uma <xref:System.ServiceModel.Description.IServiceBehavior> implementação, a <xref:System.EnterpriseServices.ObjectPoolingAttribute> classe tem vários membros para personalizar o pool de objetos usando os argumentos de atributo. Esses membros incluem <xref:System.EnterpriseServices.ObjectPoolingAttribute.MaxPoolSize%2A> , <xref:System.EnterpriseServices.ObjectPoolingAttribute.MinPoolSize%2A> e <xref:System.EnterpriseServices.ObjectPoolingAttribute.CreationTimeout%2A> , para corresponder ao conjunto de recursos do pool de objetos fornecido pelo .net Enterprise Services.  
  
 O comportamento do pool de objetos agora pode ser adicionado a um serviço do WCF anotando a implementação do serviço com o atributo personalizado recém-criado `ObjectPooling` .  
  
```csharp  
[ObjectPooling(MaxPoolSize=1024, MinPoolSize=10, CreationTimeout=30000)]
public class PoolService : IPoolService  
{  
  // …  
}  
```  
  
## <a name="running-the-sample"></a>Executando o exemplo  
 O exemplo demonstra os benefícios de desempenho que podem ser obtidos usando o pool de objetos em determinados cenários.  
  
 O aplicativo de serviço implementa dois serviços – `WorkService` e `ObjectPooledWorkService` . Ambos os serviços compartilham a mesma implementação, ambos exigem uma inicialização cara e, em seguida, expõem um `DoWork()` método relativamente barato. A única diferença é que o `ObjectPooledWorkService` tem o pool de objetos configurado:  
  
```csharp  
[ObjectPooling(MinPoolSize = 0, MaxPoolSize = 5)]  
public class ObjectPooledWorkService : IDoWork  
{  
    public ObjectPooledWorkService()  
    {  
        Thread.Sleep(5000);  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService instance created.");  
    }  
  
    public void DoWork()  
    {  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService.GetData() completed.");  
    }
}  
```  
  
 Quando você executa o cliente, ele se chama de `WorkService` 5 vezes. Em seguida, eles chamam o tempo de `ObjectPooledWorkService` 5 vezes. A diferença no tempo é exibida:  
  
```console
Press <ENTER> to start the client.  
  
Calling WorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling WorkService took: 26722 ms.  
Calling ObjectPooledWorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling ObjectPooledWorkService took: 5323 ms.  
Press <ENTER> to exit.  
```  
  
> [!NOTE]
> Na primeira vez em que o cliente é executado, ambos os serviços parecem levar aproximadamente na mesma quantidade de tempo. Se você executar novamente o exemplo, poderá ver que o `ObjectPooledWorkService` retorna muito mais rápido porque já existe uma instância desse objeto no pool.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!NOTE]
> Se você usar svcutil. exe para regenerar a configuração para este exemplo, certifique-se de modificar o nome do ponto de extremidade na configuração do cliente para corresponder ao código do cliente.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Pooling`  
