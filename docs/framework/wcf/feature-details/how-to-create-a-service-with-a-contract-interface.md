---
title: Como criar um serviço com interface de contrato
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7b6803f6-d6f9-4cc2-9f1b-6f4c920475d5
ms.openlocfilehash: c7d4bce174790b97db6b95aa5d15af455f261f82
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597157"
---
# <a name="how-to-create-a-service-with-a-contract-interface"></a>Como criar um serviço com interface de contrato
A maneira preferida de criar um contrato de Windows Communication Foundation (WCF) é usando uma interface. Este contrato especifica a coleção e a estrutura de mensagens necessárias para acessar as operações que o serviço oferece. Essa interface define os tipos de entrada e saída aplicando a <xref:System.ServiceModel.ServiceContractAttribute> classe à interface e a <xref:System.ServiceModel.OperationContractAttribute> classe aos métodos que você deseja expor.  
  
 Para obter mais informações sobre contratos de serviço, consulte [Designing Service Contracts](../designing-service-contracts.md).  
  
### <a name="creating-a-wcf-contract-with-an-interface"></a>Criando um contrato do WCF com uma interface  
  
1. Crie uma nova interface usando Visual Basic, C# ou qualquer outra linguagem de Common Language Runtime.  
  
2. Aplique a classe <xref:System.ServiceModel.ServiceContractAttribute> à interface.  
  
3. Defina os métodos na interface.  
  
4. Aplique a <xref:System.ServiceModel.OperationContractAttribute> classe a cada método que deve ser exposto como parte do contrato do WCF público.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra uma interface que define um contrato de serviço.  
  
 [!code-csharp[c_HowTo_CreateContractWithInterface#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createcontractwithinterface/cs/source.cs#1)]
 [!code-vb[c_HowTo_CreateContractWithInterface#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_createcontractwithinterface/vb/source.vb#1)]  
  
 Por padrão, os métodos que têm a <xref:System.ServiceModel.OperationContractAttribute> classe aplicada usam um padrão de mensagem de solicitação-resposta. Para obter mais informações sobre esse padrão de mensagem, consulte [como: criar um contrato de solicitação-resposta](how-to-create-a-request-reply-contract.md). Você também pode criar e usar outros padrões de mensagem definindo as propriedades do atributo. Para obter mais exemplos, consulte [como: criar um contrato unidirecional](how-to-create-a-one-way-contract.md) e [como criar um contrato duplex](how-to-create-a-duplex-contract.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
