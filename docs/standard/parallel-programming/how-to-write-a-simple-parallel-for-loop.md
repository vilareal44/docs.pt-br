---
title: 'Como: Escrever um loop Parallel.For simples'
description: Aprenda a escrever loops Parallel. for no .NET no qual você não precisa cancelar o loop, interromper iterações de loop ou manter qualquer estado de thread local.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Parallel.For, How to Write
- for loop, parallel construction in .NET
- parallel for loops, how to use
ms.assetid: 9029ba7f-a9d1-4526-8c84-c88716dba5d4
ms.openlocfilehash: 8307f2205653fbd213d824acffc405ee97580166
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662687"
---
# <a name="how-to-write-a-simple-parallelfor-loop"></a>Como: Escrever um loop Parallel.For simples

Este tópico contém dois exemplos que ilustram o método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>. O primeiro usa a sobrecarga do método <xref:System.Threading.Tasks.Parallel.For%28System.Int64%2CSystem.Int64%2CSystem.Action%7BSystem.Int64%7D%29?displayProperty=nameWithType> e o segundo usa a sobrecarga <xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Action%7BSystem.Int32%7D%29?displayProperty=nameWithType>, as duas sobrecargas mais simples do método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>. Você pode usar essas duas sobrecargas do método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> quando não for preciso cancelar o loop, dividir as iterações do loop ou manter qualquer estado local do thread.

> [!NOTE]
> Esta documentação usa expressões lambda para definir delegados na TLP. Se você não estiver familiarizado com expressões lambda em C# ou Visual Basic, consulte [expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).

O primeiro exemplo calcula o tamanho dos arquivos em um único diretório. O segundo calcula o produto de duas matrizes.

## <a name="directory-size-example"></a>Exemplo de tamanho de diretório

Este exemplo é um utilitário de linha de comando simples que calcula o tamanho total dos arquivos em um diretório. Ele espera um caminho de diretório único como um argumento e informa o número e o tamanho total dos arquivos nesse diretório. Depois de verificar se o diretório existe, ele usa o método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> para enumerar os arquivos no diretório e determinar os tamanhos dos arquivos. Cada tamanho de arquivo é adicionado à variável `totalSize`. Observe que a adição é realizada chamando o <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> para que a adição seja realizada como uma operação atômica. Caso contrário, várias tarefas podem tentar atualizar a variável `totalSize` simultaneamente.

[!code-csharp[Conceptual.Parallel.For#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.parallel.for/cs/for1.cs#1)]
[!code-vb[Conceptual.Parallel.For#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.parallel.for/vb/for1.vb#1)]

## <a name="matrix-and-stopwatch-example"></a>Exemplo de matriz e cronômetro

Este exemplo usa o método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> para calcular o produto de duas matrizes. Ele também mostra como usar a classe <xref:System.Diagnostics.Stopwatch?displayProperty=nameWithType> para comparar o desempenho de um loop paralelo com um loop não paralelo. Observe que, como ele pode gerar um grande volume de saída, o exemplo permite que a saída seja redirecionada para um arquivo.

[!code-csharp[TPL_Parallel#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/simpleparallelfor.cs#01)]
[!code-vb[TPL_Parallel#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/simpleparallelfor.vb#01)]

Ao paralelizar qualquer código, incluindo loops, um objetivo importante é utilizar os processadores o máximo possível sem paralelizar tanto ao ponto da sobrecarga do processamento paralelo prejudicar os benefícios do desempenho. Nesse exemplo específico, somente o loop externo é paralelizado porque não há muito trabalho realizado no loop interno. A combinação de uma pequena quantidade de trabalho e efeitos de cache indesejáveis podem levar à degradação do desempenho em loops paralelos aninhados. Portanto, a paralelização do loop externo é apenas a melhor forma de maximizar os benefícios de simultaneidade na maioria dos sistemas.

## <a name="the-delegate"></a>O representante

O terceiro parâmetro dessa sobrecarga de <xref:System.Threading.Tasks.Parallel.For%2A> é um representante do tipo `Action<int>` em C# ou `Action(Of Integer)` no Visual Basic. Um representante de `Action`, se tiver zero, um ou dezesseis parâmetros de tipo, sempre retornará nulo. No Visual Basic, o comportamento de um `Action` é definido com um `Sub`. O exemplo usa uma expressão lambda para criar o representante, mas ele também pode ser criado de outras formas. Para saber mais, confira [Expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).

## <a name="the-iteration-value"></a>O valor da iteração

O representante usa um único parâmetro de entrada cujo valor é a iteração atual. Esse valor da iteração é fornecido pelo runtime e seu valor inicial é o índice do primeiro elemento no segmento (partição) da origem que está sendo processada no thread atual.

Se você precisar de mais controle sobre o nível de simultaneidade, use uma das sobrecargas que usa um parâmetro de entrada <xref:System.Threading.Tasks.ParallelOptions?displayProperty=nameWithType>, como: <xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Threading.Tasks.ParallelOptions%2CSystem.Action%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%7D%29?displayProperty=nameWithType>.

## <a name="return-value-and-exception-handling"></a>Valor de retorno e tratamento de exceções

<xref:System.Threading.Tasks.Parallel.For%2A> retorna um objeto <xref:System.Threading.Tasks.ParallelLoopResult?displayProperty=nameWithType> quando todos os threads forem concluídos. Esse valor de retorno é útil se você estiver parando ou interrompendo a iteração da loop manualmente, já que o <xref:System.Threading.Tasks.ParallelLoopResult> armazena informações como a última iteração executada antes da conclusão. Se uma ou mais exceções ocorrerem em um dos threads, um <xref:System.AggregateException?displayProperty=nameWithType> será lançado.

No código deste exemplo, o valor de retorno de <xref:System.Threading.Tasks.Parallel.For%2A> não é usado.

## <a name="analysis-and-performance"></a>Análise e desempenho

Você pode usar o Assistente de Desempenho para exibir o uso da CPU no computador. Para experimentar, aumente o número de colunas e linhas de matrizes. Quanto maior as matrizes, maior será a diferença de desempenho entre as versões paralelas e sequenciais da computação. Quando a matriz é pequena, a versão sequencial será executada com maior rapidez devido à sobrecarga da configuração do loop paralelo.

As chamadas síncronas para recursos compartilhados, como o Console ou o Sistema de arquivos, prejudicará significativamente o desempenho de um loop paralelo. Ao medir o desempenho, tente evitar chamadas como <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> dentro do loop.

## <a name="compile-the-code"></a>Compile o código

Copie e cole esse código em um projeto do Visual Studio.

## <a name="see-also"></a>Confira também

- <xref:System.Threading.Tasks.Parallel.For%2A>
- <xref:System.Threading.Tasks.Parallel.ForEach%2A>
- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Programação paralela](index.md)
