---
title: 'Como: Tratar exceções em loops paralelos'
description: Saiba como lidar com exceções em loops paralelos no .NET. Veja um exemplo de como encapsular todas as exceções do loop em um System. AggregateException.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel loops, how to handle exceptions
ms.assetid: 512f0d5a-4636-4875-b766-88f20044f143
ms.openlocfilehash: 61c22d6e82282f8aeb54818c813d4489e3bc9641
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768970"
---
# <a name="how-to-handle-exceptions-in-parallel-loops"></a>Como: Tratar exceções em loops paralelos
As sobrecargas <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> não têm nenhum mecanismo especial para lidar com exceções que podem ocorrer. Nesse sentido, eles se assemelham a regulares `for` e `foreach` loops ( `For` e `For Each` em Visual Basic); uma exceção sem tratamento faz com que o loop seja encerrado assim que todas as iterações em execução no momento sejam concluídas.
  
 Quando você adiciona sua própria lógica de tratamento de exceção a loops paralelos, trate o caso em que exceções semelhantes podem ser geradas em vários threads ao mesmo tempo e o caso em que uma exceção lançada em um thread faz com que outra exceção seja lançada em outro thread. Você pode manipular ambos os casos encapsulando todas as exceções do loop em um <xref:System.AggregateException?displayProperty=nameWithType>. O exemplo a seguir mostra uma abordagem possível.  
  
> [!NOTE]
> Se a opção "Apenas Meu Código" estiver habilitada, o Visual Studio em alguns casos interromperá na linha que lança a exceção e exibirá uma mensagem de erro que diz "exceção não tratada pelo código do usuário". Esse erro é benigno. Você pode pressionar F5 para continuar a partir daí e ver o comportamento de tratamento de exceção, demonstrado no exemplo a seguir. Para impedir que o Visual Studio seja interrompido no primeiro erro, basta desmarcar a caixa de seleção "Apenas Meu Código" em **Ferramentas, Opções, Depuração, Geral**.  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, todas as exceções são detectadas e, em seguida, encapsuladas em uma <xref:System.AggregateException?displayProperty=nameWithType> gerada. O chamador pode decidir quais exceções tratar.  
  
 [!code-csharp[TPL_Exceptions#08](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptions.cs#08)]
 [!code-vb[TPL_Exceptions#08](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/exceptionsinloops.vb#08)]  
  
## <a name="see-also"></a>Veja também

- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md)
