---
title: Como chamar operações assíncronas usando uma fábrica de canais
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: cc17dd47-b9ad-451c-a362-e36e0aac7ba0
ms.openlocfilehash: 25d88b00e0bb1105be57989ff5d090a308177329
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601264"
---
# <a name="how-to-call-operations-asynchronously-using-a-channel-factory"></a>Como chamar operações assíncronas usando uma fábrica de canais
Este tópico aborda como um cliente pode acessar uma operação de serviço de forma assíncrona ao usar um <xref:System.ServiceModel.ChannelFactory%601> aplicativo cliente baseado em um. (Ao usar um <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType> objeto para invocar um serviço, você pode usar o modelo de chamada assíncrona orientado por evento. Para obter mais informações, consulte [como: chamar operações de serviço de forma assíncrona](how-to-call-wcf-service-operations-asynchronously.md). Para obter mais informações sobre o modelo de chamada assíncrona baseado em evento, consulte [padrão assíncrono baseado em evento (EAP)](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md).)  
  
 O serviço neste tópico implementa a `ICalculator` interface. O cliente pode chamar as operações nessa interface de forma assíncrona, o que significa que operações como `Add` são divididas em dois métodos, `BeginAdd` e `EndAdd` o primeiro deles inicia a chamada e a última que recupera o resultado quando a operação é concluída. Para obter um exemplo que mostra como implementar uma operação de forma assíncrona em um serviço, consulte [como implementar uma operação de serviço assíncrono](../how-to-implement-an-asynchronous-service-operation.md). Para obter detalhes sobre as operações síncronas e assíncronas, consulte operações síncronas [e](../synchronous-and-asynchronous-operations.md)assíncronas.  
  
## <a name="procedure"></a>Procedimento  
  
#### <a name="to-call-wcf-service-operations-asynchronously"></a>Para chamar operações do serviço WCF de forma assíncrona  
  
1. Execute a ferramenta [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) com a `/async` opção conforme mostrado no comando a seguir.  
  
    ```console
    svcutil /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost:8000/servicemodelsamples/service/mex /a  
    ```  
  
     Isso gera uma versão de cliente assíncrona do contrato de serviço para a operação.  
  
2. Crie uma função de retorno de chamada a ser chamada quando a operação assíncrona for concluída, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[C_How_To_CF_Async#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_how_to_cf_async/cs/client.cs#2)]
     [!code-vb[C_How_To_CF_Async#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_how_to_cf_async/vb/client.vb#2)]  
  
3. Para acessar uma operação de serviço de forma assíncrona, crie o cliente e chame o `Begin[Operation]` (por exemplo, `BeginAdd` ) e especifique uma função de retorno de chamada, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[C_How_To_CF_Async#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_how_to_cf_async/cs/client.cs#3)]
     [!code-vb[C_How_To_CF_Async#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_how_to_cf_async/vb/client.vb#3)]  
  
     Quando a função de retorno de chamada é executada, o cliente chama `End<operation>` (por exemplo, `EndAdd` ) para recuperar o resultado.  
  
## <a name="example"></a>Exemplo  
 O serviço usado com o código do cliente que é usado no procedimento anterior implementa a `ICalculator` interface, conforme mostrado no código a seguir. No lado do serviço, as `Add` `Subtract` operações e do contrato são invocadas de forma síncrona pelo tempo de execução do Windows Communication Foundation (WCF), mesmo que as etapas do cliente precedentes sejam invocadas de modo assíncrono no cliente. As `Multiply` `Divide` operações e são usadas para invocar o serviço de forma assíncrona no lado do serviço, mesmo que o cliente os invoque de maneira síncrona. Este exemplo define a <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> propriedade como `true` . Essa configuração de propriedade, em combinação com a implementação do padrão .NET Framework assíncrono, informa o tempo de execução para invocar a operação de forma assíncrona.  
  
 [!code-csharp[C_How_To_CF_Async#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_how_to_cf_async/cs/service.cs#4)]
 [!code-vb[C_How_To_CF_Async#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_how_to_cf_async/vb/service.vb#4)]  
