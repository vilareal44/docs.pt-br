---
title: Serviços de duplex
description: Saiba como criar um contrato de serviço duplex no WCF, que permite que os dois pontos de extremidade enviem mensagens entre si por meio de um canal criado pelo cliente.
ms.date: 05/09/2018
dev_langs:
- csharp
- vb
ms.assetid: 396b875a-d203-4ebe-a3a1-6a330d962e95
ms.openlocfilehash: a43bb63a0ccf1a34b79dce755c19f7ed4cb6c16c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247345"
---
# <a name="duplex-services"></a>Serviços de duplex

Um contrato de serviço duplex é um padrão de troca de mensagens no qual ambos os pontos de extremidade podem enviar mensagens para o outro de forma independente. Um serviço duplex, portanto, pode enviar mensagens de volta ao ponto de extremidade do cliente, fornecendo comportamento semelhante a evento. A comunicação duplex ocorre quando um cliente se conecta a um serviço e fornece o serviço com um canal no qual o serviço pode enviar mensagens de volta ao cliente. Observe que o comportamento do tipo evento dos serviços duplex só funciona em uma sessão.

Para criar um contrato duplex, crie um par de interfaces. A primeira é a interface de contrato de serviço que descreve as operações que um cliente pode invocar. Esse contrato de serviço deve especificar um *contrato de retorno de chamada* na <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType> propriedade. O contrato de retorno de chamada é a interface que define as operações que o serviço pode chamar no ponto de extremidade do cliente. Um contrato duplex não requer uma sessão, embora as associações duplex fornecidas pelo sistema façam uso delas.

Veja a seguir um exemplo de um contrato duplex.

[!code-csharp[c_DuplexServices#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/service.cs#0)]
[!code-vb[c_DuplexServices#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/service.vb#0)]

A `CalculatorService` classe implementa a `ICalculatorDuplex` interface primária. O serviço usa o <xref:System.ServiceModel.InstanceContextMode.PerSession> modo de instância para manter o resultado de cada sessão. Uma propriedade privada chamada `Callback` acessa o canal de retorno de chamada para o cliente. O serviço usa o retorno de chamada para enviar mensagens de volta para o cliente por meio da interface de retorno de chamada, conforme mostrado no código de exemplo a seguir.

[!code-csharp[c_DuplexServices#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/service.cs#1)]
[!code-vb[c_DuplexServices#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/service.vb#1)]

O cliente deve fornecer uma classe que implemente a interface de retorno de chamada do contrato duplex, para receber mensagens do serviço. O código de exemplo a seguir mostra uma `CallbackHandler` classe que implementa a `ICalculatorDuplexCallback` interface.

[!code-csharp[c_DuplexServices#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/client.cs#2)]
[!code-vb[c_DuplexServices#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/client.vb#2)]

O cliente WCF que é gerado para um contrato duplex requer <xref:System.ServiceModel.InstanceContext> que uma classe seja fornecida na construção. Essa <xref:System.ServiceModel.InstanceContext> classe é usada como o site de um objeto que implementa a interface de retorno de chamada e manipula as mensagens que são enviadas de volta do serviço. Uma <xref:System.ServiceModel.InstanceContext> classe é construída com uma instância da `CallbackHandler` classe. Esse objeto manipula as mensagens enviadas do serviço para o cliente na interface de retorno de chamada.

[!code-csharp[c_DuplexServices#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/client.cs#3)]
[!code-vb[c_DuplexServices#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/client.vb#3)]

A configuração do serviço deve ser configurada para fornecer uma associação que dê suporte à comunicação de sessão e à comunicação duplex. O `wsDualHttpBinding` elemento dá suporte à comunicação de sessão e permite a comunicação duplex fornecendo conexões http duplas, uma para cada direção.

No cliente, você deve configurar um endereço que o servidor pode usar para se conectar ao cliente, conforme mostrado na seguinte configuração de exemplo.

> [!NOTE]
> Clientes não duplex que não se autenticam usando uma conversa segura normalmente lançam um <xref:System.ServiceModel.Security.MessageSecurityException> . No entanto, se um cliente duplex que usa uma conversa segura não for autenticado, o cliente receberá um <xref:System.TimeoutException> em vez disso.

Se você criar um cliente/serviço usando o `WSHttpBinding` elemento e não incluir o ponto de extremidade de retorno de chamada do cliente, você receberá o seguinte erro.

```console
HTTP could not register URL
htp://+:80/Temporary_Listen_Addresses/<guid> because TCP port 80 is being used by another application.
```

O código de exemplo a seguir mostra como especificar o endereço do ponto de extremidade do cliente programaticamente.

```csharp
WSDualHttpBinding binding = new WSDualHttpBinding();
EndpointAddress endptadr = new EndpointAddress("http://localhost:12000/DuplexTestUsingCode/Server");
binding.ClientBaseAddress = new Uri("http://localhost:8000/DuplexTestUsingCode/Client/");
```

```vb
Dim binding As New WSDualHttpBinding()
Dim endptadr As New EndpointAddress("http://localhost:12000/DuplexTestUsingCode/Server")
binding.ClientBaseAddress = New Uri("http://localhost:8000/DuplexTestUsingCode/Client/")
```

O código de exemplo a seguir mostra como especificar o endereço do ponto de extremidade do cliente na configuração.

```xml
<client>
    <endpoint name ="ServerEndpoint"
          address="http://localhost:12000/DuplexTestUsingConfig/Server"
          bindingConfiguration="WSDualHttpBinding_IDuplexTest"
            binding="wsDualHttpBinding"
           contract="IDuplexTest" />
</client>
<bindings>
    <wsDualHttpBinding>
        <binding name="WSDualHttpBinding_IDuplexTest"
          clientBaseAddress="http://localhost:8000/myClient/" >
            <security mode="None"/>
         </binding>
    </wsDualHttpBinding>
</bindings>
```

> [!WARNING]
> O modelo duplex não detecta automaticamente quando um serviço ou cliente fecha seu canal. Portanto, se um cliente for encerrado inesperadamente, por padrão, o serviço não será notificado, ou se um serviço for encerrado inesperadamente, o cliente não será notificado. Se você usar um serviço que está desconectado, a <xref:System.ServiceModel.CommunicationException> exceção será gerada. Os clientes e serviços podem implementar seu próprio protocolo para notificar uns aos outros se escolherem. Para obter mais informações sobre o tratamento de erros, consulte [controle de erros do WCF](../wcf-error-handling.md).

## <a name="see-also"></a>Veja também

- [Duplex](../samples/duplex.md)
- [Especificando o comportamento em tempo de execução do cliente](../specifying-client-run-time-behavior.md)
- [Como criar uma fábrica de canais e utilizá-la para criar e gerenciar canais](how-to-create-a-channel-factory-and-use-it-to-create-and-manage-channels.md)
