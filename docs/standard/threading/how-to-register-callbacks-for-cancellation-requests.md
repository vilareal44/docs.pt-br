---
title: Registrar retornos de chamada para solicitações de cancelamento
ms.date: 08/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, how to register callbacks
ms.assetid: 8838dd75-18ed-4b8b-b322-cd4531faac64
ms.openlocfilehash: faa8dada5779f6eaee7a60e6210ec604f5fb4680
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608449"
---
# <a name="register-callbacks-for-cancellation-requests"></a>Registrar retornos de chamada para solicitações de cancelamento

Saiba como registrar um delegado que será invocado quando uma <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> propriedade se tornar verdadeira. O valor muda de false para true quando uma chamada para <xref:System.Threading.CancellationTokenSource.Cancel%2A> no objeto que criou o token é feita. Use essa técnica para cancelar operações assíncronas que não dão suporte nativo à estrutura de cancelamento unificada e para métodos de desbloqueio que podem estar aguardando a conclusão de uma operação assíncrona.

> [!NOTE]
> Se a opção "Apenas Meu Código" estiver habilitada, o Visual Studio em alguns casos interromperá na linha que lança a exceção e exibirá uma mensagem de erro que diz "exceção não tratada pelo código do usuário". Esse erro é benigno. Você pode pressionar <kbd>F5</kbd> para continuar a partir dele e ver o comportamento de tratamento de exceção que é demonstrado nos exemplos abaixo. Para impedir que o Visual Studio seja interrompido no primeiro erro, basta desmarcar a caixa de seleção "Apenas Meu Código" em **Ferramentas, Opções, Depuração, Geral**.

## <a name="example"></a>Exemplo

No exemplo a seguir, o método <xref:System.Net.WebClient.CancelAsync%2A> é registrado como o método a ser invocado quando o cancelamento é solicitado por meio do token de cancelamento.

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.cancellation.callback/cs/howtoexample1.cs":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.cancellation.callback/vb/howtoexample1.vb":::

Se o cancelamento já tiver sido solicitado quando o retorno de chamada for registrado, ainda haverá garantia de chamada do retorno de chamada. Nesse caso específico, o método <xref:System.Net.WebClient.CancelAsync%2A> não fará nada se nenhuma operação assíncrona estiver em andamento, portanto, é seguro sempre chamar o método.

## <a name="see-also"></a>Confira também

- [Cancelamento em threads gerenciados](cancellation-in-managed-threads.md)
- <xref:System.Net.WebClient.DownloadStringTaskAsync(System.String)?displayProperty=nameWithType>
