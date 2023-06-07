---
title: Como ouvir várias solicitações de cancelamento
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation tokens, joining
- LinkedTokenSource, how to
ms.assetid: 6f4f3804-2ed7-41b4-a97a-6e32b93f6e05
ms.openlocfilehash: 3f92d1d9e8fec91475886e8bd7bffbc97bb632a0
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279389"
---
# <a name="how-to-listen-for-multiple-cancellation-requests"></a>Como ouvir várias solicitações de cancelamento
Este exemplo mostra como detectar dois tokens de cancelamento simultaneamente para que você possa cancelar uma operação se um dos tokens assim o solicitar.  
  
> [!NOTE]
> Se a opção "Apenas Meu Código" estiver habilitada, o Visual Studio em alguns casos interromperá na linha que lança a exceção e exibirá uma mensagem de erro que diz "exceção não tratada pelo código do usuário". Esse erro é benigno. Você pode pressionar F5 para continuar a partir daí e ver o comportamento de tratamento de exceção, demonstrado nos exemplos a seguir. Para impedir que o Visual Studio seja interrompido no primeiro erro, basta desmarcar a caixa de seleção "Apenas Meu Código" em **Ferramentas, Opções, Depuração, Geral**.  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, o método <xref:System.Threading.CancellationTokenSource.CreateLinkedTokenSource%2A> é usado para juntar dois tokens em um token. Isso permite que o token seja passado para métodos que levam apenas um token de cancelamento como argumento. O exemplo demonstra um cenário comum em que um método deve observar um token passado de fora da classe e um token gerado dentro da classe.  
  
 [!code-csharp[Cancellation#13](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex13.cs#13)]
 [!code-vb[Cancellation#13](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex13.vb#13)]  
  
 Quando o token vinculado lança um <xref:System.OperationCanceledException>, o token que é passado para a exceção é o token vinculado, não qualquer um dos tokens anteriores. Para determinar qual dos tokens foi cancelado, verifique o status dos tokens anteriores diretamente.  
  
 Neste exemplo, <xref:System.AggregateException> nunca deverá ser lançado, mas é capturado aqui porque em cenários do mundo real todas as outras exceções além de <xref:System.OperationCanceledException> que são lançadas pelo delegado da tarefa são encapsuladas em um <xref:System.AggregateException>.  
  
## <a name="see-also"></a>Veja também

- [Cancelamento em threads gerenciados](cancellation-in-managed-threads.md)
