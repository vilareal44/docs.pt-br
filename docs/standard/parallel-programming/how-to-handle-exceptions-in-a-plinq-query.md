---
title: 'Como: Tratar exceções em uma consulta PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to handle exceptions
ms.assetid: 8d56ff9b-a571-4d31-b41f-80c0b51b70a5
ms.openlocfilehash: 4097d222b5fa51cc638a2d07d3fd2eddf5d9859c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84278643"
---
# <a name="how-to-handle-exceptions-in-a-plinq-query"></a>Como: Tratar exceções em uma consulta PLINQ

O primeiro exemplo neste tópico mostra como tratar o <xref:System.AggregateException?displayProperty=nameWithType> que pode ser gerado de uma consulta PLINQ ao ser executado. O segundo exemplo mostra como colocar blocos try-catch em representantes, o mais próximo possível de onde a exceção será gerada. Dessa forma, você pode capturá-los assim que eles ocorrerem e, possivelmente, continuar a execução da consulta. Quando as exceções tiverem permissão de emergirem novamente para o thread de associação, então será possível que uma consulta continue a processar alguns itens após a geração da exceção.

Em alguns casos em que PLINQ volta à execução sequencial e ocorre uma exceção, a exceção pode ser propagada diretamente e não é encapsulada em um <xref:System.AggregateException>. Além disso, <xref:System.Threading.ThreadAbortException>s sempre são propagados diretamente.

> [!NOTE]
> Se a opção "Apenas Meu Código" estiver habilitada, o Visual Studio interromperá na linha que gerou a exceção e exibirá a mensagem de erro "exceção não tratada pelo código do usuário". Esse erro é benigno. Você pode pressionar F5 para continuar a partir daí e ver o comportamento de tratamento de exceção, demonstrado nos exemplos a seguir. Para impedir que o Visual Studio seja interrompido no primeiro erro, basta desmarcar a caixa de seleção "Apenas Meu Código" em **Ferramentas, Opções, Depuração, Geral**.
>
> Este exemplo tem como objetivo demonstrar o uso e pode não executar tão rápido quanto a consulta LINQ to Objects sequencial equivalente. Para saber mais sobre agilização, confira [Noções básicas sobre agilização no PLINQ](understanding-speedup-in-plinq.md).

## <a name="example"></a>Exemplo

Este exemplo mostra como colocar os blocos try-catch no código que executa a consulta para capturar quaisquer <xref:System.AggregateException?displayProperty=nameWithType>s que são gerados.

[!code-csharp[PLINQ#41](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#41)]
[!code-vb[PLINQ#41](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#41)]

Neste exemplo, a consulta não pode continuar depois que a exceção é gerada. Quando o código do aplicativo captura a exceção, PLINQ já interrompeu a consulta em todos os threads.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como inserir um bloco try-catch em um representante para que seja possível capturar uma exceção e continuar com a execução da consulta.

[!code-csharp[PLINQ#42](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#42)]
[!code-vb[PLINQ#42](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#42)]

## <a name="compiling-the-code"></a>Compilando o código

- Para compilar e executar esses exemplos, copie-os para o Exemplo de Dados do PLINQ e chame o método do Principal.

## <a name="robust-programming"></a>Programação robusta

Não captura uma exceção, a menos que você saiba como tratá-la para que o estado do programa não sejam corrompido.

## <a name="see-also"></a>Veja também

- <xref:System.Linq.ParallelEnumerable>
- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
