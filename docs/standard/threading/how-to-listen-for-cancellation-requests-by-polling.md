---
title: Como ouvir solicitações de cancelamento por meio de sondagem
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, how to poll for requests
ms.assetid: c7f2f022-d08e-4e00-b4eb-ae84844cb1bc
ms.openlocfilehash: 6f70ce75b1d6a3d4d7e8a38d739005a261b07241
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279550"
---
# <a name="how-to-listen-for-cancellation-requests-by-polling"></a>Como ouvir solicitações de cancelamento por meio de sondagem
O exemplo a seguir mostra uma maneira de o código do usuário sondar um token de cancelamento em intervalos regulares para verificar se o thread de chamada solicitou o cancelamento. Este exemplo usa o tipo <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>, mas o mesmo padrão se aplica a operações assíncronas criadas diretamente pelo tipo <xref:System.Threading.ThreadPool?displayProperty=nameWithType> ou <xref:System.Threading.Thread?displayProperty=nameWithType>.  
  
## <a name="example"></a>Exemplo  
 A sondagem requer um tipo de código recursivo ou de loop que possa fazer a leitura periódica do valor da propriedade booliana <xref:System.Threading.CancellationToken.IsCancellationRequested%2A>. Se estiver usando o tipo <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> e estiver aguardando a conclusão da tarefa no thread de chamada, use o método <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> para verificar a propriedade e lançar a exceção. Ao usar esse método, a exceção correta será gerada em resposta a uma solicitação. Se estiver usando um <xref:System.Threading.Tasks.Task>, chamar este método é melhor do que gerar manualmente uma <xref:System.OperationCanceledException>. Se não precisar lançar a exceção, bastará verificar a propriedade e obter um retorno do método se a propriedade for `true`.  
  
 [!code-csharp[Cancellation#11](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex11.cs#11)]
 [!code-vb[Cancellation#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex11.vb#11)]  
  
 Chamar <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> é extremamente rápido e não apresenta uma sobrecarga significativa em loops.  
  
 Se estiver chamando <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>, basta verificar explicitamente a propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> se você tiver outro trabalho a fazer em resposta ao cancelamento, além de lançar a exceção. Neste exemplo, vemos que o código realmente acessa a propriedade duas vezes: uma no acesso explícito e novamente no método <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>. Mas, como o ato de leitura da propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> envolve apenas uma instrução volátil de leitura por acesso, o acesso duplo não é significativo do ponto de vista do desempenho. Ainda assim é melhor chamar o método do que lançar manualmente a <xref:System.OperationCanceledException>.  
  
## <a name="see-also"></a>Veja também

- [Cancelamento em threads gerenciados](cancellation-in-managed-threads.md)
