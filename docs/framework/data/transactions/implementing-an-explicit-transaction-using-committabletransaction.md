---
title: Implementar uma transação explícita usando CommittableTransaction
description: Implemente uma transação explícita usando a classe CommittableTransaction no .NET. Essa classe forneceu uma maneira explícita para que os aplicativos usem uma transação.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 29efe5e5-897b-46c2-a35f-e599a273acc8
ms.openlocfilehash: 40001422e665a7dda3fb938c8d57860909525404
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141985"
---
# <a name="implementing-an-explicit-transaction-using-committabletransaction"></a>Implementar uma transação explícita usando CommittableTransaction
O <xref:System.Transactions.CommittableTransaction> classe fornece um modo explícito para os aplicativos que usam uma transação, em vez de usar o <xref:System.Transactions.TransactionScope> classe implicitamente. É útil para aplicativos que deseja usar a mesma transação em várias chamadas de função ou várias chamadas de threads. Ao contrário do <xref:System.Transactions.TransactionScope> classe, o criador do aplicativo precisa chamar especificamente o <xref:System.Transactions.CommittableTransaction.Commit%2A> e <xref:System.Transactions.Transaction.Rollback%2A> métodos para confirmar ou anular a transação.  
  
## <a name="overview-of-the-committabletransaction-class"></a>Visão geral da classe CommittableTransaction  
 O <xref:System.Transactions.CommittableTransaction> classe deriva de <xref:System.Transactions.Transaction> classe, fornecendo, portanto, toda a funcionalidade do último. É especificamente útil a <xref:System.Transactions.Transaction.Rollback%2A> método o <xref:System.Transactions.Transaction> classe também pode ser usado para reverter uma <xref:System.Transactions.CommittableTransaction> objeto.  
  
 O <xref:System.Transactions.Transaction> classe é semelhante de <xref:System.Transactions.CommittableTransaction> de classe, mas não oferece uma `Commit` método. Isso permite que você passe o objeto de transação (ou clones dele) para outros métodos (potencialmente em outros threads) enquanto ainda controla quando a transação é confirmada. O código de chamada é capaz de se inscrever e votar na transação, mas somente o criador do <xref:System.Transactions.CommittableTransaction> objeto tem a capacidade de confirmar a transação.  
  
 Observe o seguinte ao trabalhar com o <xref:System.Transactions.CommittableTransaction> classe,  
  
- Criando um <xref:System.Transactions.CommittableTransaction> transação não define a transação de ambiente. Você precisa definir especificamente e redefinir a transação ambiente, para garantir que os gerenciadores de recursos operam sob o contexto de transação direita quando apropriado. A maneira de definir a transação de ambiente atual está definindo estático <xref:System.Transactions.Transaction.Current%2A> propriedade global <xref:System.Transactions.Transaction> objeto.  
  
- Um <xref:System.Transactions.CommittableTransaction> objeto não pode ser reutilizado. Uma vez um <xref:System.Transactions.CommittableTransaction> objeto foi confirmado ou revertido, ele não pode ser usado novamente em uma transação. Ou seja, ele não pode ser definido como o contexto de transação de ambiente atual.  
  
## <a name="creating-a-committabletransaction"></a>Criando um CommittableTransaction  
 O exemplo a seguir cria um novo <xref:System.Transactions.CommittableTransaction> e confirma a ele.  
  
 [!code-csharp[Tx_CommittableTx#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_committabletx/cs/committabletxwithsql.cs#1)]
 [!code-vb[Tx_CommittableTx#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_committabletx/vb/committabletxwithsql.vb#1)]  
  
 Criando uma instância de <xref:System.Transactions.CommittableTransaction> não define automaticamente o contexto de transação de ambiente. Portanto, qualquer operação em um Gerenciador de recursos não é parte da transação. Estático <xref:System.Transactions.Transaction.Current%2A> propriedade global <xref:System.Transactions.Transaction> objeto é usado para definir ou recuperar a transação de ambiente e o aplicativo deve defini-lo para garantir que os gerenciadores de recursos podem participar da transação manualmente. Também é uma boa prática para salvar a transação ambiente antiga e restaurá-lo quando terminar de usar o <xref:System.Transactions.CommittableTransaction> objeto.  
  
 Para confirmar a transação, você precisa chamar explicitamente o <xref:System.Transactions.CommittableTransaction.Commit%2A> método. Para reverter uma transação, você deve chamar o <xref:System.Transactions.Transaction.Rollback%2A> método. É importante observar que até um <xref:System.Transactions.CommittableTransaction> foi confirmada ou revertida, todos os recursos envolvidos nessa transação ainda estão bloqueados.  
  
 Um <xref:System.Transactions.CommittableTransaction> objeto pode ser usado em chamadas de função e threads. No entanto, é o desenvolvedor de aplicativo para manipular exceções e chamar especificamente o <xref:System.Transactions.Transaction.Rollback%28System.Exception%29> método em caso de falhas.  
  
## <a name="asynchronous-commit"></a>Confirmação assíncrona  
 O <xref:System.Transactions.CommittableTransaction> classe também fornece um mecanismo para confirmar uma transação de forma assíncrona. Uma confirmação de transação pode levar algum tempo, como ele pode envolver vários acesso de banco de dados e possível latência de rede. Quando você deseja evitar deadlocks em aplicativos de alta taxa de transferência, use confirmação assíncrona para concluir o trabalho transacional assim que possível e executar a operação de confirmação como uma tarefa em segundo plano. O <xref:System.Transactions.CommittableTransaction.BeginCommit%2A> e <xref:System.Transactions.CommittableTransaction.EndCommit%2A> métodos de <xref:System.Transactions.CommittableTransaction> classe permitem que você faça isso.  
  
 Você pode chamar <xref:System.Transactions.CommittableTransaction.BeginCommit%2A> para expedir a demora de confirmação a um thread do pool de threads. Você também pode chamar <xref:System.Transactions.CommittableTransaction.EndCommit%2A> para determinar se a transação, na verdade, foi confirmada. Se a transação não foi confirmada por algum motivo, <xref:System.Transactions.CommittableTransaction.EndCommit%2A> gera uma exceção de transação. Se a transação é ainda não foram confirmada no momento <xref:System.Transactions.CommittableTransaction.EndCommit%2A> é chamado, o chamador é bloqueado até que a transação é confirmada ou anulada.  
  
 É a maneira mais fácil de fazer uma confirmação assíncrona, fornecendo um método de retorno de chamada a ser chamada quando a confirmação for concluída. No entanto, você deve chamar o <xref:System.Transactions.CommittableTransaction.EndCommit%2A> método no original <xref:System.Transactions.CommittableTransaction> objeto usado para invocar a chamada. Para obter esse objeto, você pode downcast o parâmetro *IAsyncResult* do método de retorno de chamada, já que a <xref:System.Transactions.CommittableTransaction> classe implementa a <xref:System.IAsyncResult> classe.  
  
 O exemplo a seguir mostra como uma confirmação assíncrona pode ser feita.  
  
```csharp  
public void DoTransactionalWork()  
{  
     Transaction oldAmbient = Transaction.Current;  
     CommittableTransaction committableTransaction = new CommittableTransaction();  
     Transaction.Current = committableTransaction;  
  
     try  
     {  
          /* Perform transactional work here */  
          // No errors - commit transaction asynchronously  
          committableTransaction.BeginCommit(OnCommitted,null);  
     }  
     finally  
     {  
          //Restore the ambient transaction
          Transaction.Current = oldAmbient;  
     }  
}  
void OnCommitted(IAsyncResult asyncResult)  
{  
     CommittableTransaction committableTransaction;  
     committableTransaction = asyncResult as CommittableTransaction;
     Debug.Assert(committableTransaction != null);  
     try  
     {  
          using(committableTransaction)  
          {  
               committableTransaction.EndCommit(asyncResult);  
          }  
     }  
     catch(TransactionException e)  
     {  
          //Handle the failure to commit  
     }  
}  
```  
  
## <a name="see-also"></a>Veja também

- [Implementar uma transação implícita usando o escopo da transação](implementing-an-implicit-transaction-using-transaction-scope.md)
- [Processamento de transações](index.md)
