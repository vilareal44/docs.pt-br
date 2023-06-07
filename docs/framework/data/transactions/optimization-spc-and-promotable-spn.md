---
title: Otimização usando commit de fase única e notificação de fase única promovível
description: Otimize o desempenho usando a confirmação de fase única e a notificação de fase única promocionaltable. Saiba mais sobre a infraestrutura System. Transactions no .NET.
ms.date: 03/30/2017
ms.assetid: 57beaf1a-fb4d-441a-ab1d-bc0c14ce7899
ms.openlocfilehash: 89ce82e673340c93254983c078f78a2501129383
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141972"
---
# <a name="optimization-using-single-phase-commit-and-promotable-single-phase-notification"></a>Otimização usando commit de fase única e notificação de fase única promovível

Este tópico descreve os mecanismos fornecidos pelo <xref:System.Transactions> infra-estrutura para otimizar o desempenho.

## <a name="promotable-single-phase-enlistment"></a>Inscrição de fase única passíveis de promoção

O <xref:System.Transactions> infra-estrutura administrates uma transação dentro de um único domínio de aplicativo que envolve a no máximo um único recurso durável ou vários recursos voláteis. Como o <xref:System.Transactions> infra-estrutura usa chamadas de domínio apenas intra-aplicativo, ele produz a melhor taxa de transferência e o desempenho.

No entanto, se a transação é fornecida a outro objeto em outro domínio de aplicativo (incluindo entre limites de processo e da máquinas) no mesmo computador, ou se você se inscrever outro gerenciador de recursos duráveis, o <xref:System.Transactions> infra-estrutura aumenta automaticamente a transação a ser gerenciado pelo MSDTC. Uma transação gerenciada por MSDTC não é tão em termos de desempenho como um gerenciado pelo <xref:System.Transactions> infra-estrutura.

Para otimizar o desempenho, o <xref:System.Transactions> infra-estrutura fornece o podem ser promovidas única fase de inscrição (PSPE) que permite que um único durável recurso remoto, localizado em um domínio de aplicativo diferente, o processo ou a máquina, participe de um <xref:System.Transactions> transação sem fazer com que ele ser escalonado para uma transação MSDTC. Esse Gerenciador de recursos (RM) pode hospedar e "proprietário" de uma transação que posteriormente pode ser escalonada para uma transação distribuída (ou transação MSDTC) se necessário. Isso reduz a possibilidade de usar o MSDTC.

O Gerenciador de recursos específico normalmente tem suas próprias transações distribuídas não internas e precisa oferecer suporte a converter essas transações em transações distribuídas em runtime. Por exemplo, o SQL Server 2005 é tal um Gerenciador de recursos. Nesse caso, o <xref:System.Transactions> infra-estrutura usa uma função de gerenciamento de passivo monitorando apenas a transação para uma necessidade de escalonamento. Para oferecer suporte a interação entre o <xref:System.Transactions> infra-estrutura e Gerenciador de recursos, o último precisa implementar a interface <xref:System.Transactions.IPromotableSinglePhaseNotification>.

O <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> método é usado para inscrever um único recurso durável que pode ser escalonado posteriormente. Esse método garante que a inscrição pode ser escalonada conforme necessário. Se a inscrição for bem-sucedida, o Gerenciador de recursos cria sua transação interna e o associa a <xref:System.Transactions> transação. Se a inscrição PSPE falhar, o Gerenciador de recursos em vez disso, deve inscrever usando o <xref:System.Transactions.Transaction.EnlistDurable%2A> método. Falhas para se inscrever no PSPE podem acontecer quando a transação já for uma transação distribuída ou outro RM já executou uma inscrição de PSPE

Depois de inscrito, chamadas por clientes para confirmar ou anular a <xref:System.Transactions> transação são convertidos em chamadas no Gerenciador de recursos, invocando o <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> método, ou o <xref:System.Transactions.IPromotableSinglePhaseNotification.Rollback%2A> respectivamente.

Se o <xref:System.Transactions> transação nunca requer escalonamento, quando a transação é confirmada, o RM recebe um <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> notificação. Em seguida, ele poderá confirmar a transação interna que foi inicialmente criada.

Se o <xref:System.Transactions> transação deverá ser escalonado (por exemplo, para oferecer suporte a vários RMs), <xref:System.Transactions> informa ao Gerenciador de recursos, chamando o <xref:System.Transactions.ITransactionPromoter.Promote%2A> método no <xref:System.Transactions.ITransactionPromoter> interface, da qual o <xref:System.Transactions.IPromotableSinglePhaseNotification> interface deriva. O Gerenciador de recursos converte a transação interna de uma transação local (que não exigem registro em log) para um objeto de transação que é capaz de participar de uma transação do DTC e associa o trabalho já realizado. Quando a transação é solicitada a confirmar, o Gerenciador de transações ainda envia o <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> notificação para o Gerenciador de recursos, que confirma a transação distribuída que criou durante a expansão.

> [!NOTE]
> Os rastreamentos de **TransactionCommitted** (gerados quando uma confirmação é invocada na transação escalonada) contêm a ID de atividade da transação do DTC.

Para saber mais sobre escalonamento de gerenciamento, confira [escalonamento de gerenciamento de transações](transaction-management-escalation.md).

## <a name="transaction-management-escalation-scenario"></a>Cenário de escalonamento de gerenciamento de transações

O cenário a seguir demonstra um escalonamento para uma transação distribuída usando o <xref:System.Data> namespace como o 'proxy' para o Gerenciador de recursos. Este cenário pressupõe que já existe um <xref:System.Data> conexão com o banco de dados, CN1, envolvidos na transação e o aplicativo deseja envolver outro <xref:System.Data> conexão, CN2. A transação deve ser escalada para o DTC, como uma transação de confirmação de duas fases distribuída completa.

Nesse cenário,

1. Chamadas de CN1 a <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> método para se inscrever na transação. Em seguida, a transação é ainda local e não há nenhum inscrições podem ser promovidas na transação, para que o <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> chamada for bem-sucedida.

2. Quando a segunda conexão, CN2 chama <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A>, a chamada falhará porque não há outra inscrição podem ser promovida envolvidas. Por isso, CN2 deve obter uma transação do DTC para passá-lo para SQL. Para fazer isso, ele usa um dos métodos fornecidos pelo <xref:System.Transactions.TransactionInterop> classe para produzir um formato da transação que pode ser determinado para SQL.

3. <xref:System.Transactions>chama o <xref:System.Transactions.ITransactionPromoter.Promote%2A> método na <xref:System.Transactions.ITransactionPromoter> interface implementada por CN1.

4. Neste ponto, CN1 aumenta a transação, usando um mecanismo específico para o SQL 2005 e <xref:System.Data>.

5. O valor de retorno de <xref:System.Transactions.ITransactionPromoter.Promote%2A> método é uma matriz de bytes que contém um token de propagação da transação. <xref:System.Transactions>usa esse token de propagação para criar uma transação do DTC que pode ser incorporada à transação local.

6. Neste ponto, CN2 pode usar os dados recebidos de chamar um dos métodos por <xref:System.Transactions.TransactionInterop> para passar a transação para SQL.

7. Agora, ambos estão inscritos em uma transação distribuída do DTC.

## <a name="single-phase-commit-optimization"></a>Otimização de confirmação de fase única

O protocolo de confirmação de fase única é mais eficiente em runtime, como todas as atualizações são realizadas sem qualquer coordenação explícita. Para tirar proveito dessa otimização, você deve implementar um Gerenciador de recursos usando <xref:System.Transactions.ISinglePhaseNotification> para o recurso de interface e se inscrever em uma transação usando o <xref:System.Transactions.Transaction.EnlistDurable%2A> ou <xref:System.Transactions.Transaction.EnlistVolatile%2A> método. Especificamente, o parâmetro *Enlistoptions* deve ser igual a <xref:System.Transactions.EnlistmentOptions.None> para garantir que uma única confirmação de fase seja executada.

Como o <xref:System.Transactions.ISinglePhaseNotification> interface deriva de <xref:System.Transactions.IEnlistmentNotification> a interface, se o RM não está qualificado para confirmação de fase única, ele pode ainda receber a fase de duas notificações de confirmação. Se receber o RM um <xref:System.Transactions.ISinglePhaseNotification.SinglePhaseCommit%2A> notificação do TM, ele deve tentar fazer o trabalho necessário para confirmar e proporcionalmente informam o Gerenciador de transações se a transação deve ser confirmada ou revertida chamando o <xref:System.Transactions.SinglePhaseEnlistment.Committed%2A>, <xref:System.Transactions.SinglePhaseEnlistment.Aborted%2A>, ou <xref:System.Transactions.SinglePhaseEnlistment.InDoubt%2A> método o <xref:System.Transactions.SinglePhaseEnlistment> parâmetro. Uma resposta de <xref:System.Transactions.Enlistment.Done%2A> sobre a inscrição neste estágio implica a semântica de somente leitura. Portanto, você não deve responder <xref:System.Transactions.Enlistment.Done%2A> além de qualquer um dos outros métodos.

Se houver apenas uma inscrição volátil e nenhuma inscrição durável, a inscrição volátil receberá uma notificação do SPC. Se houver qualquer inscrições voláteis e apenas uma inscrição durável, as inscrições voláteis receberão 2PC. Quando ele for concluído, a distribuição durável recebe SPC.

## <a name="see-also"></a>Veja também

- [Inscrever recursos como participantes em uma transação](enlisting-resources-as-participants-in-a-transaction.md)
- [Confirmar uma transação de fase única e de várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md)
