---
title: Confirmar uma transação de fase única e de várias fases
description: Leia sobre como confirmar transações em uma ou duas fases no .NET. Uma confirmação de duas fases (2PC) deve ser executada se a transação envolver mais de um recurso.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 694ea153-e4db-41ae-96ac-9ac66dcb69a9
ms.openlocfilehash: 2f4486998f347bf1db6d22433e6e48b553609c18
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141818"
---
# <a name="committing-a-transaction-in-single-phase-and-multi-phase"></a>Confirmar uma transação de fase única e de várias fases
Cada recurso usado em uma transação é gerenciado por um Gerenciador de recursos (RM), as ações são coordenadas por um Gerenciador de transações (TM). A lista de [recursos como participantes em um](enlisting-resources-as-participants-in-a-transaction.md) tópico de transação discute como um recurso (ou vários recursos) pode ser inscrito em uma transação. Este tópico discute como confirmação de transação pode ser coordenada entre recursos.  
  
 No final da transação, o aplicativo solicita que a transação seja confirmada ou revertida. O Gerenciador de transações deve eliminar riscos, como alguns gerenciadores de recursos de votação para confirmar quando outro votação para reverter a transação.  
  
 Se a transação envolve mais de um recurso, você deve realizar uma confirmação de duas fases (2PC). O protocolo de confirmação de duas fases (a fase de preparação e a fase de confirmação) garante que quando a transação termina, todas as alterações a todos os recursos são totalmente confirmadas ou revertidas completamente. Todos os participantes, em seguida, são informados do resultado final. Para obter uma discussão detalhada sobre o protocolo de confirmação de duas fases, consulte o livro "*processamento de transações: conceitos e técnicas (Morgan Kaufmann Series em sistemas gerenciamento de dados) ISBN: 1558601902*", de Jim Gray.  
  
 Você também pode otimizar o desempenho da transação que fazem parte do protocolo de confirmação de fase única. Para obter mais informações, consulte [otimização usando confirmação de fase única e notificação de fase única de promoçãotable](optimization-spc-and-promotable-spn.md).  
  
 Se você apenas deseja ser informado do resultado da transação e não quiser participar de votação, você deve se registrar para o <xref:System.Transactions.Transaction.TransactionCompleted> eventos.  
  
## <a name="two-phase-commit-2pc"></a>Confirmação de duas fases (2PC)  
 Na primeira fase da transação, o Gerenciador de transações consulta cada recurso para determinar se uma transação deve ser confirmada ou revertida. A segunda fase da transação, o Gerenciador de transações informa cada recurso do resultado de suas consultas, permitindo que ele execute qualquer limpeza necessária.  
  
 Para participar desse tipo de transação, um Gerenciador de recursos deve implementar o <xref:System.Transactions.IEnlistmentNotification> interface, que fornece métodos que são chamados pelo TM como notificações durante um 2PC.  O exemplo a seguir mostra um exemplo dessa implementação.  
  
 [!code-csharp[Tx_Enlist#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#2)]
 [!code-vb[Tx_Enlist#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#2)]  
  
### <a name="prepare-phase-phase-1"></a>Preparar fase (fase 1)  
 Ao receber um <xref:System.Transactions.CommittableTransaction.Commit%2A> solicitação do aplicativo, o Gerenciador de transações começa a fase de preparação de todos os participantes inscrita chamando o <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> método em cada inscrito recurso, para obter um voto de cada recurso na transação.  
  
 O Gerenciador de recursos que implementa o <xref:System.Transactions.IEnlistmentNotification> primeiro deve implementar a interface a <xref:System.Transactions.IEnlistmentNotification.Prepare%28System.Transactions.PreparingEnlistment%29> método como mostra o seguinte exemplo simples.  
  
```csharp
public void Prepare(PreparingEnlistment preparingEnlistment)  
{  
     Console.WriteLine("Prepare notification received");  
     //Perform work  
  
     Console.Write("reply with prepared? [Y|N] ");  
     c = Console.ReadKey();  
     Console.WriteLine();  
  
     //If work finished correctly, reply with prepared  
     if ((c.KeyChar == 'Y') || (c.KeyChar == 'y'))  
     {  
          preparingEnlistment.Prepared();  
          break;  
     }  
  
     // otherwise, do a ForceRollback  
     else if ((c.KeyChar == 'N') || (c.KeyChar == 'n'))  
     {  
          preparingEnlistment.ForceRollback();  
          break;  
     }  
}  
```  
  
 Quando o Gerenciador de recursos duráveis recebe essa chamada, ele deverá registrar informações de recuperação da transação (disponível ao recuperar o <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> propriedade) e qualquer informação é necessária para concluir a transação de confirmação. Isso não precisa ser executada dentro do <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> método porque o Gerenciador de recursos pode fazer isso em um thread de trabalho.  
  
 Quando o RM terminou seu trabalho de preparação, devem votar para confirmar ou reverter chamando o <xref:System.Transactions.PreparingEnlistment.Prepared%2A> ou <xref:System.Transactions.PreparingEnlistment.ForceRollback%2A> método. Observe que o <xref:System.Transactions.PreparingEnlistment> classe herda um <xref:System.Transactions.Enlistment.Done%2A> método a partir de <xref:System.Transactions.Enlistment> classe. Se você chamar esse método no <xref:System.Transactions.PreparingEnlistment> o retorno de chamada durante a fase de preparação, ele informa o TM que é uma inscrição de somente leitura (ou seja, gerenciadores de recursos que pode ler, mas não é possível atualizar dados protegidos por transação) e o RM não recebe nenhuma notificação adicional do Gerenciador de transações sobre o resultado da transação na fase 2.  
  
 O aplicativo é informado do compromisso bem-sucedida da transação depois que todos os gerenciadores de recursos votam <xref:System.Transactions.PreparingEnlistment.Prepared%2A>.  
  
### <a name="commit-phase-phase-2"></a>Confirmar fase (fase 2)  
 Na segunda fase da transação, se o Gerenciador de transações recebe bem-sucedida prepara de todos os gerenciadores de recursos (todos os gerenciadores de recursos invocado <xref:System.Transactions.PreparingEnlistment.Prepared%2A> no final da fase 1), ele invoca o <xref:System.Transactions.IEnlistmentNotification.Commit%2A> método para cada Gerenciador de recursos. Os gerenciadores de recursos, faça as alterações durável e concluir a confirmação.  
  
 Se o Gerenciador de recursos relatou uma falha ao preparar na fase 1, o Gerenciador de transações invoca o <xref:System.Transactions.IEnlistmentNotification.Rollback%2A> método para cada Gerenciador de recursos e indica a falha da confirmação ao aplicativo.  
  
 Assim, o Gerenciador de recursos deve implementar os métodos a seguir.  
  
```csharp
public void Commit (Enlistment enlistment)  
{  
     // Do any work necessary when commit notification is received  
  
     // Declare done on the enlistment  
     enlistment.Done();  
}  
  
public void Rollback (Enlistment enlistment)  
{  
     // Do any work necessary when rollback notification is received  
  
     // Declare done on the enlistment
     enlistment.Done();
}  
```  
  
 O Gerenciador de recursos deve executar qualquer trabalho necessário para concluir a transação com base no tipo de notificação e informar o TM concluiu chamando <xref:System.Transactions.Enlistment.Done%2A> método o <xref:System.Transactions.Enlistment> parâmetro. Esse trabalho pode ser feito em um thread de trabalho. Observe que as notificações de fase 2 podem acontecer embutido no mesmo thread que chamou o <xref:System.Transactions.PreparingEnlistment.Prepared%2A> método na fase 1. Assim, você não deve fazer qualquer trabalho após o <xref:System.Transactions.PreparingEnlistment.Prepared%2A> chamada (por exemplo, liberar bloqueios) que você esperaria ter concluído antes de receber as notificações de fase 2.  
  
### <a name="implementing-indoubt"></a>Implementando incertas  
 Por fim, você deve implementar o <xref:System.Transactions.IEnlistmentNotification.InDoubt%2A> método para o Gerenciador de recursos volátil. Esse método é chamado se o Gerenciador de transações perder o contato com um ou mais participantes, portanto, seu status é desconhecido. Se isso ocorrer, você deve registrar esse fato para que você possa investigar mais tarde se qualquer um dos participantes da transação foi deixado em um estado inconsistente.  
  
```csharp
public void InDoubt (Enlistment enlistment)  
{  
     // log this  
     enlistment.Done();  
}  
```  
  
## <a name="single-phase-commit-optimization"></a>Otimização de confirmação de fase única  
 O protocolo de confirmação de fase única é mais eficiente em tempo de execução porque todas as atualizações são feitas sem qualquer coordenação explícita. Para obter mais informações sobre esse protocolo, consulte [otimização usando confirmação de fase única e notificação de fase única de promoçãotable](optimization-spc-and-promotable-spn.md).  
  
## <a name="see-also"></a>Veja também

- [Otimização usando commit de fase única e notificação de fase única promovível](optimization-spc-and-promotable-spn.md)
- [Inscrever recursos como participantes em uma transação](enlisting-resources-as-participants-in-a-transaction.md)
