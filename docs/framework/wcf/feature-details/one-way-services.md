---
title: Serviços unidirecionais
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], one-way service contracts
- WCF [WCF], one-way service contracts
- service contracts [WCF], defining one-way
ms.assetid: 19053a36-4492-45a3-bfe6-0365ee0205a3
ms.openlocfilehash: 0d69af40e4b9a0133e44b64b45466f9aac84ffe2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598743"
---
# <a name="one-way-services"></a>Serviços unidirecionais
O comportamento padrão de uma operação de serviço é o padrão de solicitação-resposta. Em um padrão de solicitação-resposta, o cliente aguarda a mensagem de resposta, mesmo que a operação de serviço seja representada no código como um `void` método. Com uma operação unidirecional, apenas uma mensagem é transmitida. O receptor não envia uma mensagem de resposta, nem o remetente espera uma.  
  
 Use o padrão de design unidirecional:  
  
- Quando o cliente deve chamar operações e não é afetado pelo resultado da operação no nível da operação.  
  
- Ao usar a <xref:System.ServiceModel.NetMsmqBinding> classe ou <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> . (Para obter mais informações sobre esse cenário, consulte [filas no WCF](queues-in-wcf.md).)  
  
 Quando uma operação é unidirecional, não há uma mensagem de resposta para transportar informações de erro ao cliente. Você pode detectar condições de erro usando os recursos da Associação subjacente, como sessões confiáveis, ou criando um contrato de serviço duplex que usa operações de 2 1, um contrato unidirecional do cliente para o serviço para chamar a operação de serviço e outro contrato unidirecional entre o serviço e o cliente para que o serviço possa enviar falhas de volta ao cliente usando um retorno de chamada que o cliente implementa.  
  
 Para criar um contrato de serviço unidirecional, defina seu contrato de serviço, aplique a <xref:System.ServiceModel.OperationContractAttribute> classe a cada operação e defina a <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> propriedade como `true` , conforme mostrado no código de exemplo a seguir.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOneWayCalculator  
{  
    [OperationContract(IsOneWay=true)]  
    void Add(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Subtract(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Multiply(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Divide(double n1, double n2);  
}  
```  
  
 Para obter um exemplo completo, consulte o exemplo [unidirecional](../samples/one-way.md) .  
  
## <a name="clients-blocking-with-one-way-operations"></a>Clientes bloqueando com operações unidirecionais  
 É importante perceber que, enquanto alguns aplicativos unidirecionais retornam assim que os dados de saída são gravados na conexão de rede, em vários cenários a implementação de uma associação ou de um serviço pode fazer com que um cliente WCF bloqueie usando operações unidirecionais. Em aplicativos cliente WCF, o objeto cliente WCF não retorna até que os dados de saída tenham sido gravados na conexão de rede. Isso é verdadeiro para todos os padrões de troca de mensagens, incluindo operações unidirecionais; Isso significa que qualquer problema ao gravar os dados no transporte impede que o cliente retorne. Dependendo do problema, o resultado pode ser uma exceção ou um atraso no envio de mensagens para o serviço.  
  
 Por exemplo, se o transporte não puder localizar o ponto de extremidade, uma <xref:System.ServiceModel.EndpointNotFoundException?displayProperty=nameWithType> exceção será lançada sem muito atraso. No entanto, também é possível que o serviço não possa ler os dados fora do fio por algum motivo, o que impede que a operação de envio de transporte do cliente seja retornada. Nesses casos, se o <xref:System.ServiceModel.Channels.Binding.SendTimeout%2A?displayProperty=nameWithType> período na associação de transporte do cliente for excedido, um <xref:System.TimeoutException?displayProperty=nameWithType> será lançado — mas não até que o período de tempo limite seja excedido. Também é possível acionar tantas mensagens em um serviço que o serviço não pode processá-las após um determinado ponto. Nesse caso também, o cliente unidirecional é bloqueado até que o serviço possa processar as mensagens ou até que uma exceção seja gerada.  
  
 Outra variação é a situação na qual a propriedade de serviço <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> é definida <xref:System.ServiceModel.ConcurrencyMode.Single> e a associação usa sessões. Nesse caso, o Dispatcher impõe a ordenação das mensagens de entrada (um requisito de sessões), o que impede que as mensagens subsequentes sejam lidas da rede até que o serviço tenha processado a mensagem anterior para essa sessão. Novamente, o cliente bloqueia, mas se ocorre uma exceção depende se o serviço é capaz de processar os dados em espera antes das configurações de tempo limite no cliente.  
  
 Você pode atenuar um pouco desse problema inserindo um buffer entre o objeto de cliente e a operação de envio do transporte de cliente. Por exemplo, o uso de chamadas assíncronas ou o uso de uma fila de mensagens na memória pode permitir que o objeto de cliente seja retornado rapidamente. Ambas as abordagens podem aumentar a funcionalidade, mas o tamanho do pool de threads e a fila de mensagens ainda impõem limites.  
  
 Em vez disso, é recomendável que você examine os vários controles no serviço, bem como no cliente, e teste seus cenários de aplicativo para determinar a melhor configuração em ambos os lados. Por exemplo, se o uso de sessões estiver bloqueando o processamento de mensagens em seu serviço, você poderá definir a <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> propriedade como para <xref:System.ServiceModel.InstanceContextMode.PerCall> que cada mensagem possa ser processada por uma instância de serviço diferente e definir a <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> para para permitir que <xref:System.ServiceModel.ConcurrencyMode.Multiple> mais de um thread envie mensagens por vez. Outra abordagem é aumentar as cotas de leitura das associações de serviço e de cliente.  
  
## <a name="see-also"></a>Consulte também

- [Unidirecional](../samples/one-way.md)
