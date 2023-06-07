---
title: Inscrever recursos como participantes em uma transação
description: Inscrever recursos como participantes em uma transação do .NET. Cada recurso em uma transação é gerenciado por um Gerenciador de recursos, coordenado por um Gerenciador de transações.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 786a12c2-d530-49f4-9c59-5c973e15a11d
ms.openlocfilehash: c3c777593750b3a4f05ae97ed6e26e19f58e4d72
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141869"
---
# <a name="enlisting-resources-as-participants-in-a-transaction"></a>Inscrever recursos como participantes em uma transação

Cada recurso que participa de uma transação é gerenciado por um Gerenciador de recursos, as ações são coordenadas por um Gerenciador de transações. A coordenação é feita por meio de notificações para assinantes que tem se inscrito em uma transação por meio do Gerenciador de transações.

Este tópico aborda como um recurso (ou vários recursos) podem ser inscrita em uma transação, bem como os diferentes tipos de inscrição. O tópico [confirmando uma transação em fase única e várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md) aborda como o compromisso de transação pode ser coordenado entre os recursos inscritos.

## <a name="enlisting-resources-in-a-transaction"></a>Recursos de inscrição em uma transação

Em ordem para um recurso para participar de uma transação, ele deve se inscrever na transação. A <xref:System.Transactions.Transaction> classe define um conjunto de métodos cujos nomes começam com a **inscrição** que fornecem essa funcionalidade. Os diferentes métodos de **inscrição** correspondem aos diferentes tipos de inscrição que um Gerenciador de recursos pode ter. Especificamente, você usa o <xref:System.Transactions.Transaction.EnlistVolatile%2A> métodos para recursos voláteis e o <xref:System.Transactions.Transaction.EnlistDurable%2A> método Recursos duráveis. A durabilidade (ou o inverso a volatilidade) de um recurso Gerenciador refere-se ao Gerenciador de recursos oferece suporte a recuperação de falhas. Se um Gerenciador de recursos oferece suporte à recuperação de falha, ele persiste os dados para o armazenamento durável durante da Fase1 (preparar), de modo que, se o Gerenciador de recursos de ficar inativo, ele pode novamente se inscrever na transação após a recuperação e executar as ações apropriadas com base em notificações recebidas de TM. Em geral, os gerenciadores de recursos voláteis gerenciar recursos voláteis, como uma estrutura de dados na memória (por exemplo, na memória transacionado-hashtable) e gerenciadores de recursos duráveis gerenciar recursos em um repositório de backup mais persistente (por exemplo, um banco de dados cujo armazenamento de backup em disco).

Para simplificar, depois de decidir se deseja usar o <xref:System.Transactions.Transaction.EnlistDurable%2A> ou <xref:System.Transactions.Transaction.EnlistVolatile%2A> método com base no suporte a durabilidade do recurso, você deve inscrever o recurso para participar de dois a fase de confirmação (2PC), Implementando o <xref:System.Transactions.IEnlistmentNotification> interface para seu Gerenciador de recursos. Para obter mais informações sobre 2PC, consulte [confirmando uma transação em fase única e várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md).

Um único participante pode se inscrever para mais de um desses protocolos chamando <xref:System.Transactions.Transaction.EnlistDurable%2A> e <xref:System.Transactions.Transaction.EnlistVolatile%2A> várias vezes.

### <a name="durable-enlistment"></a>Distribuição durável

O <xref:System.Transactions.Transaction.EnlistDurable%2A> métodos são usados para inscrever um Gerenciador de recursos de participação na transação como um recurso durável.  Espera-se que se um Gerenciador de recursos durável é interrompido no meio de uma transação, ele pode realizar recuperação depois que ele é colocado online pelo reenlisting (usando o <xref:System.Transactions.TransactionManager.Reenlist%2A> método) em todas as transações em que ele foi um participante e não concluiu a fase 2 e chamar <xref:System.Transactions.TransactionManager.RecoveryComplete%2A> depois que ele termine o processamento de recuperação. Para obter mais informações sobre recuperação, consulte [executando a recuperação](performing-recovery.md).

O <xref:System.Transactions.Transaction.EnlistDurable%2A> métodos usam um <xref:System.Guid> objeto como seu primeiro parâmetro. O <xref:System.Guid> é usado pelo Gerenciador de transações para associar uma distribuição durável com um Gerenciador de recursos específico. Como tal, é imperativo que um Gerenciador de recursos consistentemente usa o mesmo <xref:System.Guid> para identificar em si mesmo entre diferentes gerentes de recursos após a reinicialização, caso contrário, a recuperação poderá falhar.

O segundo parâmetro do <xref:System.Transactions.Transaction.EnlistDurable%2A> método é uma referência ao objeto que implementa o Gerenciador de recursos para receber notificações de transação. A sobrecarga que é usar informa o Gerenciador de transações se seu Gerenciador de recursos oferece suporte a otimização de confirmação de fase única (SPC). Na maioria das vezes você implementaria o <xref:System.Transactions.IEnlistmentNotification> interface para fazer parte de protocolo 2PC (2PC). No entanto, se você deseja otimizar o processo de confirmação, você pode considerar a implementação de <xref:System.Transactions.ISinglePhaseNotification> interface para SPC. Para obter mais informações sobre o SPC, consulte [confirmando uma transação em fase única, várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md) e [otimização usando a notificação de fase única de confirmação de fase única e a promovível](optimization-spc-and-promotable-spn.md).

O terceiro parâmetro é um <xref:System.Transactions.EnlistmentOptions> enumeração, cujo valor pode ser <xref:System.Transactions.EnlistmentOptions.None> ou <xref:System.Transactions.EnlistmentOptions.EnlistDuringPrepareRequired>. Se o valor for definido como <xref:System.Transactions.EnlistmentOptions.EnlistDuringPrepareRequired>, a inscrição pode inscrever gerenciadores de recursos adicionais depois de receber a notificação de preparação do Gerenciador de transações. No entanto, você deve estar ciente de que esse tipo de inscrição não está qualificado para a otimização de confirmação de fase única.

### <a name="volatile-enlistment"></a>Inscrição volátil

Participantes do gerenciamento de recursos voláteis, como um cache deve inscrever usando o <xref:System.Transactions.Transaction.EnlistVolatile%2A> métodos. Esses objetos não poderá obter o resultado de uma transação ou recuperar o estado de qualquer transação que participam após uma falha do sistema.

Conforme mencionado anteriormente, um Gerenciador de recursos se tornaria uma inscrição volátil se gerencia na memória e recursos voláteis. Um dos benefícios do uso de <xref:System.Transactions.Transaction.EnlistVolatile%2A> é que ele não forçar um escalonamento desnecessário da transação. Para obter mais informações sobre o escalonamento de transações, consulte o tópico [escalonamento de gerenciamento de transações](transaction-management-escalation.md) . A inscrição de volatilidade implica uma diferença na forma como as inscrições são manipuladas pelo Gerenciador de transações, bem como o que é esperado do Gerenciador de recursos pelo Gerenciador de transações. Isso ocorre porque um Gerenciador de recursos volátil não executar a recuperação. O <xref:System.Transactions.Transaction.EnlistVolatile%2A> métodos não têm um <xref:System.Guid> parâmetro, porque um Gerenciador de recursos volátil não realiza recuperação e não chama o <xref:System.Transactions.TransactionManager.Reenlist%2A> método que precisa de um <xref:System.Guid>.

Assim como acontece com inscrições duráveis, independentemente do método sobrecarga que você pode usar para se inscrever indica para o Gerenciador de transações se seu Gerenciador de recursos oferece suporte a otimização de confirmação de fase única. Como um Gerenciador de recursos volátil não pode realizar a recuperação, nenhuma informação de recuperação é criada para uma inscrição volátil durante a fase de preparação. Portanto, chamar o <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> método resulta em um <xref:System.InvalidOperationException>.

O exemplo a seguir mostra como se inscrever tal objeto como um participante em uma transação usando o <xref:System.Transactions.Transaction.EnlistVolatile%2A> método.

[!code-csharp[Tx_Enlist#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#1)]
[!code-vb[Tx_Enlist#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#1)]

### <a name="optimizing-performance"></a>Aperfeiçoando o desempenho

O <xref:System.Transactions.Transaction> classe também fornece o <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> método para inscrever um podem ser promovidas única fase de inscrição (PSPE). Isso permite que um recurso durável manager (RM) para hospedar e "proprietário" de uma transação que posteriormente pode ser escalonada para ser gerenciado pelo MSDTC se necessário. Para obter mais informações sobre isso, consulte [otimização usando confirmação de fase única e notificação de fase única de promoçãotable](optimization-spc-and-promotable-spn.md).

## <a name="see-also"></a>Veja também

- [Otimização usando commit de fase única e notificação de fase única promovível](optimization-spc-and-promotable-spn.md)
- [Confirmar uma transação de fase única e de várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md)
