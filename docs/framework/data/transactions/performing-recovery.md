---
title: Executar a recuperação
description: Execute a recuperação no .NET. O Gerenciador de recursos ajuda a resolver inalistas de transações duráveis reinscrevendo o participante da transação após a falha do recurso.
ms.date: 03/30/2017
ms.assetid: 6dd17bf6-ba42-460a-a44b-8046f52b10d0
ms.openlocfilehash: bb00d2f574cc2651b733f3308cf2ffc6b430cc31
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141959"
---
# <a name="performing-recovery"></a>Executar a recuperação
Um Gerenciador de recursos facilita a resolução de inscrições duráveis em uma transação, reenlisting o participante de transação após falha de recurso.  
  
## <a name="the-recovery-process"></a>O processo de recuperação  
 Para inscrever permanentemente um recurso (descrito por uma implementação do <xref:System.Transactions.IEnlistmentNotification> interface) que podem ser mais tarde qualificadas para a recuperação, você deve chamar o <xref:System.Transactions.Transaction.EnlistDurable%2A> método. Além disso, você deve fornecer o <xref:System.Transactions.Transaction.EnlistDurable%2A> método com um identificador de Gerenciador de recursos (um <xref:System.Guid>) que é usado para rotular consistentemente o participante da transação em caso de falha de recurso. Por esse motivo, o <xref:System.Guid> que é fornecido para a chamada de inscrição inicial deve ser idêntico ao parâmetro *resourceManagerIdentifier* na <xref:System.Transactions.TransactionManager.Reenlist%2A> chamada durante a recuperação. Caso contrário, <xref:System.Transactions.TransactionException> é lançada. Para obter mais informações sobre inlistagens duráveis, consulte [Inscrevendo recursos como participantes em uma transação](enlisting-resources-as-participants-in-a-transaction.md) .  
  
 Na fase de preparação (fase 1) do protocolo 2PC, quando sua implementação de um Gerenciador de recursos duráveis recebe o <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> notificação, ele deve registrar seu registro de preparação durante essa fase. O registro deve conter todas as informações necessárias concluir a transação na confirmação. O registro de preparação pode ser acessado posteriormente durante a recuperação, recuperando a <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> Propriedade do retorno de chamada *PreparingEnlistment* . O log de registro não precisa ser executada dentro do <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> método como o Gerenciador de recursos pode fazer isso em um thread de trabalho.  
  
 O processo de recuperação consiste em duas etapas a seguir:  
  
### <a name="step-1---reenlist"></a>Etapa 1 - ReEnlist  
 O Gerenciador de recursos examina o registro de informações de preparação para cada inscrição está em dúvida. Isso é feito examinando o <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> propriedade do <xref:System.Transactions.PreparingEnlistment> retorno de chamada, que é passado para o Gerenciador de recursos do <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> notificação durante a fase 1.  
  
 Para cada essa inscrição examina, ele chama <xref:System.Transactions.TransactionManager.Reenlist%2A> no Gerenciador de transações. Esse método passa exclusivo <xref:System.Guid> que identifica o Gerenciador de recursos, bem como informações da inscrição em uma matriz de bytes. Um novo <xref:System.Transactions.Enlistment> objeto é retornado. Se na nova inscrição falhará com uma exceção, o Gerenciador de recursos será necessário tentar novamente mais tarde.  
  
 Você só deve chamar o <xref:System.Transactions.TransactionManager.Reenlist%2A> método quando reinicia um Gerenciador de recursos de falha. Além disso, você só deve reinscrevê registradas por um Gerenciador de recursos durante a fase de preparação inicial de protocolo 2PC de transações não resolvidas. Qualquer tentativa de chamar esse método em momentos inválidos pode produzir resultados incorretos.  
  
 Quando um participante é reenlisted usando esse método, os métodos de fase 2 da <xref:System.Transactions.IEnlistmentNotification> que correspondam ao resultado da transação (isto é, <xref:System.Transactions.IEnlistmentNotification.Commit%2A> , <xref:System.Transactions.IEnlistmentNotification.Rollback%2A> ou <xref:System.Transactions.IEnlistmentNotification.InDoubt%2A> ) são chamados conforme apropriado.  
  
### <a name="step-2---completing-the-recovery"></a>Etapa 2: concluir a recuperação  
 Quando todas as reenlistments forem concluídos, o Gerenciador de recursos chama o <xref:System.Transactions.TransactionManager.RecoveryComplete%2A> método. Esse método conclui a recuperação e informa o Gerenciador de transações que o Gerenciador de recursos tem não mais transações em dúvida. Fazendo isso, o Gerenciador de recursos garante que ele não chamará o <xref:System.Transactions.TransactionManager.Reenlist%2A> método novamente.  
  
 Um Gerenciador de recursos não é necessário para resolver todas as transações em dúvida antes de inscrever-se em novas transações. A primeira etapa pode ser executada a qualquer momento depois que o Gerenciador de recursos estabelece uma relação com o Gerenciador de transações, mas depois de foi <xref:System.Transactions.TransactionManager.RecoveryComplete%2A> invocado (etapa 2); a etapa 1 não pode ser executada novamente. Etapa 2 pode ser repetida várias vezes sem afetar o resultado das transações.
