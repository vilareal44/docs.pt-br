---
title: Gerenciar simultaneidade com DependentTransaction
description: Gerenciar a simultaneidade de transação, incluindo tarefas assíncronas, usando a classe DependentTransaction no .NET.
ms.date: 03/30/2017
ms.assetid: b85a97d8-8e02-4555-95df-34c8af095148
ms.openlocfilehash: 9c825c977419718c8b9870b40699c4d3516e8533
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141882"
---
# <a name="managing-concurrency-with-dependenttransaction"></a>Gerenciar simultaneidade com DependentTransaction
O <xref:System.Transactions.Transaction> objeto é criado usando o <xref:System.Transactions.Transaction.DependentClone%2A> método. Sua única finalidade é garantir que a transação não confirmada enquanto alguns outros trechos de código (por exemplo, um thread de trabalho) ainda estão executando o trabalho na transação. Quando o trabalho realizado dentro da transação clonada é concluído e pronto para ser confirmada, ele pode notificar o criador da transação usando o <xref:System.Transactions.DependentTransaction.Complete%2A> método. Portanto, você pode preservar a consistência e correção dos dados.  
  
 O <xref:System.Transactions.DependentTransaction> classe também pode ser usada para gerenciar a simultaneidade entre tarefas assíncronas. Nesse cenário, o pai pode continuar a executar qualquer código enquanto o clone dependente funciona em suas tarefas. Em outras palavras, a execução do pai não é bloqueada até que o dependente é concluída.  
  
## <a name="creating-a-dependent-clone"></a>Criando um Clone dependente  
 Para criar uma transação dependente, chame o <xref:System.Transactions.Transaction.DependentClone%2A> método e passar o <xref:System.Transactions.DependentCloneOption> enumeração como um parâmetro. Esse parâmetro define o comportamento da transação se `Commit` é chamado na transação pai antes que o clone dependente indica que ele está pronto para a transação seja confirmada (chamando o <xref:System.Transactions.DependentTransaction.Complete%2A> método). Os seguintes valores são válidos para esse parâmetro:  
  
- <xref:System.Transactions.DependentCloneOption.BlockCommitUntilComplete>Cria uma transação dependente que bloqueia o processo de confirmação da transação pai até que a transação pai expire ou até que <xref:System.Transactions.DependentTransaction.Complete%2A> seja chamada em todos os dependentes indicando sua conclusão. Isso é útil quando o cliente não deseja que a transação do pai seja confirmada até que as transações dependentes sejam concluídas. Se o pai concluir seu trabalho antes que a transação dependente e chamadas <xref:System.Transactions.CommittableTransaction.Commit%2A> na transação, o processo de confirmação será bloqueado em um estado onde trabalho adicional pode ser feito na transação e novas inscrições podem ser criadas, até que todos os da chamada dependentes <xref:System.Transactions.DependentTransaction.Complete%2A>. Assim que todos eles concluiu seu trabalho e chame <xref:System.Transactions.DependentTransaction.Complete%2A>, inicia o processo de confirmação da transação.  
  
- <xref:System.Transactions.DependentCloneOption.RollbackIfNotComplete>, por outro lado, o cria uma transação dependente que aborta automaticamente se <xref:System.Transactions.CommittableTransaction.Commit%2A> for chamado na transação pai antes de <xref:System.Transactions.DependentTransaction.Complete%2A> ser chamado. Nesse caso, todo o trabalho feito na transação dependente está intacto no tempo de vida de uma transação e não tem a oportunidade de confirmar apenas uma parte dele.  
  
 O <xref:System.Transactions.DependentTransaction.Complete%2A> método deve ser chamado apenas uma vez quando seu aplicativo termina seu trabalho na transação dependente; Caso contrário, um <xref:System.InvalidOperationException> é lançada. Depois que essa chamada é invocada, você não deve tentar qualquer trabalho adicional na transação ou uma exceção é lançada.  
  
 O exemplo de código a seguir mostra como criar uma transação dependente para gerenciar duas tarefas simultâneas por uma transação dependente de clonagem e passá-la para um thread de trabalho.  
  
```csharp  
public class WorkerThread  
{  
    public void DoWork(DependentTransaction dependentTransaction)  
    {  
        Thread thread = new Thread(ThreadMethod);  
        thread.Start(dependentTransaction);
    }  
  
    public void ThreadMethod(object transaction)
    {
        DependentTransaction dependentTransaction = transaction as DependentTransaction;  
        Debug.Assert(dependentTransaction != null);
        try  
        {  
            using(TransactionScope ts = new TransactionScope(dependentTransaction))  
            {  
                /* Perform transactional work here */
                ts.Complete();  
            }  
        }  
        finally  
        {  
            dependentTransaction.Complete();
             dependentTransaction.Dispose();
        }  
    }  
  
//Client code
using(TransactionScope scope = new TransactionScope())  
{  
    Transaction currentTransaction = Transaction.Current;  
    DependentTransaction dependentTransaction;
    dependentTransaction = currentTransaction.DependentClone(DependentCloneOption.BlockCommitUntilComplete);  
    WorkerThread workerThread = new WorkerThread();  
    workerThread.DoWork(dependentTransaction);  
    /* Do some transactional work here, then: */  
    scope.Complete();  
}  
```  
  
 O código do cliente cria um escopo transacional que também define a transação de ambiente. Você não deve passar a transação de ambiente para o thread de trabalho. Em vez disso, você deve clonar a transação (ambiente) atual chamando o <xref:System.Transactions.Transaction.DependentClone%2A> método na transação atual e passe o dependente para o thread de trabalho.  
  
 O `ThreadMethod` método é executado no novo segmento. O cliente inicia um novo thread, passando a transação dependente como o `ThreadMethod` parâmetro.  
  
 Como a transação dependente é criada com <xref:System.Transactions.DependentCloneOption.BlockCommitUntilComplete>, terá a garantia de que a transação não pode ser confirmada até que todos os do trabalho transacional feito no segundo thread é concluído e <xref:System.Transactions.DependentTransaction.Complete%2A> é chamado na transação dependente. Isso significa que, se o escopo do cliente terminar (quando tentar descartar o objeto de transação no final da `using` instrução) antes que o novo thread chame <xref:System.Transactions.DependentTransaction.Complete%2A> na transação dependente, o código do cliente será bloqueado até que <xref:System.Transactions.DependentTransaction.Complete%2A> seja chamado no dependente. Em seguida, a transação pode concluir a confirmação ou anulação.  
  
## <a name="concurrency-issues"></a>Problemas de simultaneidade  
 Existem alguns problemas de simultaneidade adicionais que precisam ser consideradas ao usar o <xref:System.Transactions.DependentTransaction> classe:  
  
- Se o thread de trabalho reverte a transação, mas o pai tenta confirmar, um <xref:System.Transactions.TransactionAbortedException> é lançada.  
  
- Você deve criar um novo clone dependente para cada thread de trabalho na transação. Não passam o mesmo clone dependente em vários threads, porque apenas um deles pode chamar <xref:System.Transactions.DependentTransaction.Complete%2A> nele.  
  
- Se o thread de trabalho gera um novo thread de trabalho, certifique-se de criar um clone dependente do clone dependente e passá-lo para o novo thread.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Transactions.DependentTransaction>
