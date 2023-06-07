---
title: Como implementar uma operação de serviço assíncrona
description: Saiba mais sobre a estrutura de uma operação de serviço assíncrona em WFC. Uma operação de serviço pode ser implementada de forma assíncrona ou síncrona.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 4e5d2ea5-d8f8-4712-bd18-ea3c5461702c
ms.openlocfilehash: 5f890bd5124e2353cecee37d163b7f2c65b87fde
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244615"
---
# <a name="how-to-implement-an-asynchronous-service-operation"></a>Como implementar uma operação de serviço assíncrona
Em aplicativos Windows Communication Foundation (WCF), uma operação de serviço pode ser implementada de forma assíncrona ou síncrona, sem ditar ao cliente como chamá-lo. Por exemplo, as operações de serviço assíncronas podem ser chamadas de forma síncrona e as operações de serviço síncronas podem ser chamadas assincronamente. Para obter um exemplo que mostra como chamar uma operação de forma assíncrona em um aplicativo cliente, consulte [como: chamar operações de serviço de forma assíncrona](./feature-details/how-to-call-wcf-service-operations-asynchronously.md). Para obter mais informações sobre operações síncronas e assíncronas, consulte [Criando contratos de serviço](designing-service-contracts.md) e operações síncronas [e assíncronas](synchronous-and-asynchronous-operations.md). Este tópico descreve a estrutura básica de uma operação de serviço assíncrona, o código não está completo. Para obter um exemplo completo dos lados do serviço e do cliente, consulte [assíncrono](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms751505(v=vs.100)).  
  
### <a name="implement-a-service-operation-asynchronously"></a>Implementar uma operação de serviço de forma assíncrona  
  
1. Em seu contrato de serviço, declare um par de métodos assíncronos de acordo com as diretrizes de design assíncrono do .NET. O `Begin` método usa um parâmetro, um objeto de retorno de chamada e um objeto de estado e retorna um <xref:System.IAsyncResult?displayProperty=nameWithType> e um `End` método correspondente que usa um <xref:System.IAsyncResult?displayProperty=nameWithType> e retorna o valor de retorno. Para obter mais informações sobre chamadas assíncronas, consulte [padrões de design de programação assíncrona](../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md).  
  
2. Marque o `Begin` método do par de métodos assíncronos com o <xref:System.ServiceModel.OperationContractAttribute?displayProperty=nameWithType> atributo e defina a <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A?displayProperty=nameWithType> propriedade como `true` . Por exemplo, o código a seguir executa as etapas 1 e 2.  
  
     [!code-csharp[C_SyncAsyncClient#6](../../../samples/snippets/csharp/VS_Snippets_CFX/c_syncasyncclient/cs/services.cs#6)]
     [!code-vb[C_SyncAsyncClient#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_syncasyncclient/vb/services.vb#6)]  
  
3. Implemente o `Begin/End` par de métodos em sua classe de serviço de acordo com as diretrizes de design assíncronas. Por exemplo, o exemplo de código a seguir mostra uma implementação na qual uma cadeia de caracteres é gravada no console tanto no `Begin` quanto em `End` partes da operação de serviço assíncrono, e o valor de retorno da `End` operação é retornado para o cliente. Para obter o exemplo de código completo, consulte a seção de exemplo.  
  
     [!code-csharp[C_SyncAsyncClient#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_syncasyncclient/cs/services.cs#3)]
     [!code-vb[C_SyncAsyncClient#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_syncasyncclient/vb/services.vb#3)]  
  
## <a name="example"></a>Exemplo  
 Os exemplos de código a seguir mostram:  
  
1. Uma interface de contrato de serviço com:  
  
    1. Uma `SampleMethod` operação síncrona.  
  
    2. Uma operação assíncrona `BeginSampleMethod` .  
  
    3. Um par de operações assíncronas `BeginServiceAsyncMethod` / `EndServiceAsyncMethod` .  
  
2. Uma implementação de serviço usando um <xref:System.IAsyncResult?displayProperty=nameWithType> objeto.  
  
 [!code-csharp[C_SyncAsyncClient#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_syncasyncclient/cs/services.cs#1)]
 [!code-vb[C_SyncAsyncClient#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_syncasyncclient/vb/services.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Criando contratos de serviço](designing-service-contracts.md)
- [Operações síncronas e assíncronas](synchronous-and-asynchronous-operations.md)
