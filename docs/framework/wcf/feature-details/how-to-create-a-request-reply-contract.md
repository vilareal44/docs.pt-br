---
title: Como criar um contrato de resposta/solicitação
ms.date: 03/30/2017
ms.assetid: 801d90da-3d45-4284-9c9f-56c8aadb4060
ms.openlocfilehash: 8a09c265c77edc584b591477e64314f1e76e332b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593432"
---
# <a name="how-to-create-a-request-reply-contract"></a>Como criar um contrato de resposta/solicitação
Um contrato de solicitação-resposta especifica um método que retorna uma resposta. A resposta deve ser enviada e correlacionada à solicitação sob os termos deste contrato. Mesmo que o método não retorne nenhuma resposta ( `void` em C# ou um `Sub` no Visual Basic), a infraestrutura cria e envia uma mensagem vazia para o chamador. Para evitar o envio de uma mensagem de resposta vazia, use um contrato unidirecional para a operação.  
  
### <a name="to-create-a-request-reply-contract"></a>Para criar um contrato de solicitação-resposta  
  
1. Crie uma interface na linguagem de programação de sua escolha.  
  
2. Aplique o <xref:System.ServiceModel.ServiceContractAttribute> atributo à interface.  
  
3. Aplique o <xref:System.ServiceModel.OperationContractAttribute> atributo a cada método que os clientes podem invocar.  
  
4. Opcional. Defina o valor da <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> propriedade como `true` para impedir o envio de uma mensagem de resposta vazia. Por padrão, todas as operações são contratos de solicitação-resposta.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define um contrato para um serviço de calculadora que fornece `Add` `Subtract` métodos e. O `Multiply` método não faz parte do contrato porque não está marcado pela <xref:System.ServiceModel.OperationContractAttribute> classe e, portanto, não pode ser acessado pelos clientes.  
  
```csharp
using System.ServiceModel;

[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    // It would be equivalent to write explicitly:
    // [OperationContract(IsOneWay=false)]
    int Add(int a, int b);

    [OperationContract]
    int Subtract(int a, int b);

    int Multiply(int a, int b)
}
```
  
- Para obter mais informações sobre como especificar contratos de operação, consulte a <xref:System.ServiceModel.OperationContractAttribute> classe e a <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> propriedade.  
  
- Aplicar os <xref:System.ServiceModel.ServiceContractAttribute> <xref:System.ServiceModel.OperationContractAttribute> atributos e causa a geração automática de definições de contrato de serviço em um documento WSDL (linguagem de descrição de serviços Web) assim que o serviço é implantado. O documento é baixado acrescentando- `?wsdl` se ao endereço base http do serviço. Por exemplo, `http://microsoft/CalculatorService?wsdl`  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.OperationContractAttribute>
- [Criando contratos de serviço](../designing-service-contracts.md)
- [Como criar um contrato duplex](how-to-create-a-duplex-contract.md)
