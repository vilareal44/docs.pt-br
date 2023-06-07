---
title: Visão geral de transações do Windows Communication Foundation
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF]
- WCF, transactions
- Windows Communication Foundation, transactions
ms.assetid: c7757854-1207-4019-8b31-552578b7d570
ms.openlocfilehash: a8e3306612e016568ad7cfd5138ab538af771a17
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585825"
---
# <a name="windows-communication-foundation-transactions-overview"></a>Visão geral de transações do Windows Communication Foundation
As transações fornecem uma maneira de agrupar um conjunto de ações ou operações em uma única unidade indivisível de execução. Uma transação é uma coleção de operações com as seguintes propriedades:  
  
- Atomicidade. Isso garante que todas as atualizações concluídas em uma transação específica sejam confirmadas e tornadas duráveis ou todas todas sejam anuladas e revertidas para o estado anterior.  
  
- Consistência. Isso garante que as alterações feitas em uma transação representem uma transformação de um estado consistente para outro. Por exemplo, uma transação que transfere dinheiro de uma conta de verificação para uma conta de poupança não altera a quantidade de dinheiro na conta do banco geral.  
  
- Isolamento. Isso impede que uma transação Observe alterações não confirmadas que pertencem a outras transações simultâneas. O isolamento fornece uma abstração de simultaneidade, garantindo que uma transação não possa ter um impacto inesperado na execução de outra transação.  
  
- Durabilidade. Isso significa que, uma vez confirmadas, as atualizações em recursos gerenciados (como um registro de banco de dados) serão persistentes diante de falhas.  
  
 O Windows Communication Foundation (WCF) fornece um conjunto avançado de recursos que permitem que você crie transações distribuídas em seu aplicativo de serviço Web.  
  
 O WCF implementa o suporte para o protocolo WS-AtomicTransaction (WS-AT) que permite que os aplicativos WCF fluam transações para aplicativos interoperáveis, como serviços Web interoperáveis criados com tecnologia de terceiros. O WCF também implementa o suporte para o protocolo transações OLE, que pode ser usado em cenários em que você não precisa da funcionalidade de interoperabilidade para habilitar o fluxo de transações.  
  
 Você pode usar um arquivo de configuração de aplicativo para configurar associações para habilitar ou desabilitar o fluxo de transações, bem como para definir o protocolo de transação desejado em uma associação. Além disso, você pode definir o tempo limite da transação no nível de serviço usando o arquivo de configuração. Para obter mais informações, consulte [habilitando o fluxo de transações](enabling-transaction-flow.md).  
  
 Os atributos de transação no <xref:System.ServiceModel> namespace permitem que você faça o seguinte:  
  
- Configure tempos limite de transação e filtragem de nível de isolamento usando o <xref:System.ServiceModel.ServiceBehaviorAttribute> atributo.  
  
- Habilite a funcionalidade de transações e configure o comportamento de conclusão da transação usando o <xref:System.ServiceModel.OperationBehaviorAttribute> atributo.  
  
- Use os <xref:System.ServiceModel.ServiceContractAttribute> <xref:System.ServiceModel.OperationContractAttribute> atributos e em um método de contrato para exigir, permitir ou negar o fluxo de transações.  
  
 Para obter mais informações, consulte [ServiceModel Transaction Attributes](servicemodel-transaction-attributes.md).  
  
## <a name="see-also"></a>Consulte também

- [Atributos de transação de ServiceModel](servicemodel-transaction-attributes.md)
- [Ativando o fluxo de transações](enabling-transaction-flow.md)
