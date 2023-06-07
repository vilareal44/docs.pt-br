---
title: Implementar um Gerenciador de Recursos
description: Implemente um Gerenciador de recursos no .NET. Um Gerenciador de recursos gerencia os recursos usados em transações. Um Gerenciador de transações coordena as ações do Gerenciador de recursos.
ms.date: 03/30/2017
ms.assetid: d5c153f6-4419-49e3-a5f1-a50ae4c81bf3
ms.openlocfilehash: bf40c6eaee35a5a548c6de4a286e46c4d4a66aca
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141843"
---
# <a name="implementing-a-resource-manager"></a>Implementar um Gerenciador de Recursos
Cada recurso usado em uma transação é gerenciado por um Gerenciador de recursos, as ações são coordenadas por um Gerenciador de transações. Gerenciadores de recursos trabalham em cooperação com o Gerenciador de transações para fornecer o aplicativo com uma garantia de atomicidade e isolamento. Microsoft SQL Server, filas de mensagens duráveis, tabelas de hash em memória são exemplos de gerenciadores de recursos.  
  
 Um Gerenciador de recursos gerencia dados duráveis ou voláteis. A durabilidade (ou o inverso a volatilidade) de um recurso Gerenciador refere-se ao Gerenciador de recursos oferece suporte a recuperação de falhas. Se um Gerenciador de recursos oferece suporte à recuperação de falha, ele persiste os dados para o armazenamento durável durante da Fase1 (preparar), de modo que, se o Gerenciador de recursos de ficar inativo, ele pode novamente se inscrever na transação após a recuperação e executar as ações apropriadas com base em notificações recebidas do Gerenciador de transações. Em geral, os gerenciadores de recursos voláteis gerenciar recursos voláteis, como uma estrutura de dados na memória (por exemplo, na memória transacionado-hashtable) e gerenciadores de recursos duráveis gerenciar recursos em um repositório de backup mais persistente (por exemplo, um banco de dados cujo armazenamento de backup em disco).  
  
 Em ordem para um recurso para participar de uma transação, ele deve se inscrever na transação. A <xref:System.Transactions.Transaction> classe define um conjunto de métodos cujos nomes começam com a **inscrição** que fornecem essa funcionalidade. Os diferentes métodos de **inscrição** correspondem aos diferentes tipos de inscrição que um Gerenciador de recursos pode ter. Especificamente, você usa o <xref:System.Transactions.Transaction.EnlistVolatile%2A> métodos para recursos voláteis e o <xref:System.Transactions.Transaction.EnlistDurable%2A> método Recursos duráveis. Para simplificar, depois de decidir se deseja usar o <xref:System.Transactions.Transaction.EnlistDurable%2A> ou <xref:System.Transactions.Transaction.EnlistVolatile%2A> método com base no suporte a durabilidade do recurso, você deve inscrever o recurso para participar de dois a fase de confirmação (2PC), Implementando o <xref:System.Transactions.IEnlistmentNotification> interface para seu Gerenciador de recursos. Para obter mais informações sobre 2PC, consulte [confirmando uma transação em fase única e várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md).  
  
 Inscrevendo, o Gerenciador de recursos garante que ele obtém retornos de chamada do Gerenciador de transações quando a transação é confirmada ou anulada. Há uma instância de <xref:System.Transactions.IEnlistmentNotification> por inscrição. Normalmente, há uma inscrição por transação, mas um Gerenciador de recursos pode optar por se inscrever várias vezes na mesma transação.  
  
 Após a inscrição, o Gerenciador de recursos responde às solicitações da transação. Um Gerenciador de recursos duráveis armazena informações suficientes para permitir que ele seja desfeita ou refeita trabalho da transação em recursos que ele gerencia. Há várias maneiras de fazer isso. Manter versões dos dados ou manter um log das alterações é duas técnicas comuns.  
  
 Quando o aplicativo confirma a transação, o Gerenciador de transações inicia o protocolo de confirmação de duas fases. Primeiro, o Gerenciador de transações solicita cada Gerenciador de recursos listados se ele está preparado para confirmar a transação. O Gerenciador de recursos deve preparar para confirmar — ele se prepara para confirmar ou anular a transação.  
  
 Durante a fase de preparação, o Gerenciador de recursos duráveis registra os dados antigos e novos em armazenamento estável para que o Gerenciador de recursos pode recuperá-lo mesmo que o sistema falhe. Se o Gerenciador de recursos pode preparar, ele informa o Gerenciador de transações seu voto confirmar ou anular a transação. Se o Gerenciador de recursos relatar uma falha na preparação, o Gerenciador de transações envia um comando de reversão para cada Gerenciador de recursos e indica a falha da confirmação ao aplicativo.  
  
 Quando preparado, um Gerenciador de recursos deve esperar até obter uma confirmação ou anulação de retorno de chamada do Gerenciador de transações na fase 2. Normalmente, o protocolo de preparação e confirmação inteiro é concluída em uma fração de segundo. Se houver falhas no sistema ou a comunicação, a notificação de confirmação ou anulação pode não chegar para minutos ou horas. Durante esse período, o Gerenciador de recursos está em dúvida sobre o resultado da transação. Ele não sabe se a transação foi confirmada ou anulada. Embora o Gerenciador de recursos está em dúvida sobre uma transação, ele mantém os dados modificados, mantendo a transação bloqueada, isolando essas alterações de quaisquer outras transações.  
  
 Quando um Gerenciador de recursos falha, todas as suas transações inscrita são anuladas, exceto aquelas que são preparadas ou confirmadas antes da falha. Quando um Gerenciador de recursos durável é reiniciado, reconstrói o estado confirmado os recursos que ele gerencia recuperando as informações de preparação gravadas na fase de preparação, e confirma ou anula essas transações de forma adequada.  
  
 Em resumo, o protocolo de confirmação de duas fases e os gerenciadores de recursos se combinam para realizar transações atômicas e duráveis.  
  
 O <xref:System.Transactions.Transaction> classe também fornece o <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> método para inscrever um podem ser promovidas única fase de inscrição (PSPE). Isso permite que um recurso durável manager (RM) para hospedar e "proprietário" de uma transação que posteriormente pode ser escalonada para ser gerenciado pelo MSDTC se necessário. Para obter mais informações sobre isso, consulte [otimização usando confirmação de fase única e notificação de fase única de promoçãotable](optimization-spc-and-promotable-spn.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 As etapas que geralmente seguidas por um Gerenciador de recursos são descritas nos tópicos a seguir.  
  
 [Inscrever recursos como participantes em uma transação](enlisting-resources-as-participants-in-a-transaction.md)  
  
 Descreve como um recurso durável ou voláteis pode se inscrever em uma transação.  
  
 [Confirmar uma transação de fase única e de várias fases](committing-a-transaction-in-single-phase-and-multi-phase.md)  
  
 Descreve como um Gerenciador de recursos responde para confirmar a notificação e preparar a confirmação.  
  
 [Executar a recuperação](performing-recovery.md)  
  
 Descreve como um Gerenciador de recursos durável recupera de falha.  
  
 [Níveis de confiança de segurança ao acessar recursos](security-trust-levels-in-accessing-resources.md)  
  
 Descreve como os três níveis de confiança para System. Transactions restringem o acesso os tipos de recursos que <xref:System.Transactions> expõe.  
  
 [Otimização usando commit de fase única e notificação de fase única promovível](optimization-spc-and-promotable-spn.md)  
  
 Descreve as práticas de otimização disponíveis para as implementações de gerenciadores de recursos.
