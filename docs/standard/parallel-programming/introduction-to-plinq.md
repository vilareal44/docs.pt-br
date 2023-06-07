---
title: Introdução ao PLINQ
description: Saiba como fazer consultas em paralelo usando PLINQ no .NET. PLINQ significa consulta integrada de linguagem paralela (LINQ).
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, introduction to
ms.assetid: eaa720d8-8999-4eb7-8df5-3c19ca61cad0
ms.openlocfilehash: 9dbc4fde3f72d01aee91978ed5cb0baf0895de26
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662453"
---
# <a name="introduction-to-plinq"></a>Introdução ao PLINQ

O Parallel LINQ (PLINQ) é uma implementação paralela do padrão [LINQ (consulta integrada à linguagem)](../../csharp/programming-guide/concepts/linq/index.md) . O PLINQ implementa o conjunto completo de operadores de consulta padrão LINQ como métodos de extensão para o namespace <xref:System.Linq> e tem operadores adicionais para operações paralelas. O PLINQ combina a simplicidade e a legibilidade da sintaxe LINQ com o poder da programação paralela.

> [!TIP]
> Se você não estiver familiarizado com o LINQ, ele apresenta um modelo unificado para consultar qualquer fonte de dados enumerável de forma segura de tipo. LINQ to Objects é o nome para consultas LINQ executadas em coleções na memória, como <xref:System.Collections.Generic.List%601> e matrizes. Este artigo pressupõe que você tenha uma compreensão básica de LINQ. Para obter mais informações, consulte [LINQ (consulta integrada à linguagem)](../../csharp/programming-guide/concepts/linq/index.md).

## <a name="what-is-a-parallel-query"></a>O que é uma consulta paralela?

Uma consulta PLINQ, em muitas formas, é semelhante a uma consulta não paralela de LINQ to Objects. Consultas PLINQ, assim como consultas sequenciais de LINQ, operam em qualquer <xref:System.Collections.IEnumerable> fonte de dados ou na memória <xref:System.Collections.Generic.IEnumerable%601> e têm a execução adiada, o que significa que elas não começam a ser executadas até que a consulta seja enumerada. A principal diferença é que a PLINQ tenta fazer uso integral de todos os processadores no sistema. Ela faz isso particionando a fonte de dados em segmentos e então executando a consulta em cada segmento em threads de trabalho separados em paralelo em vários processadores. Em muitos casos, a execução paralela significa que a consulta é executada de forma significativamente mais rápida.

Por meio da execução paralela, a PLINQ pode obter melhorias significativas de desempenho sobre o código herdado para determinados tipos de consultas, com frequência simplesmente adicionando a operação de consulta <xref:System.Linq.ParallelEnumerable.AsParallel%2A> à fonte de dados da operação. No entanto, o paralelismo pode apresentar suas próprias complexidades e nem todas as operações de consulta são executadas mais rapidamente na PLINQ. Na verdade, a paralelização realmente atrasa determinadas consultas. Portanto, você deve compreender como problemas como a classificação afetam consultas paralelas. Para saber mais, veja [Noções básicas sobre agilização em PLINQ](understanding-speedup-in-plinq.md).

> [!NOTE]
> Esta documentação usa expressões lambda para definir representantes na PLINQ. Se você não estiver familiarizado com expressões lambda em C# ou Visual Basic, consulte [expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).

O restante deste artigo fornece uma visão geral das principais classes PLINQ e discute como criar consultas PLINQ. Cada seção contém links para mais informações e exemplos de código.

## <a name="the-parallelenumerable-class"></a>A Classe ParallelEnumerable

A classe <xref:System.Linq.ParallelEnumerable?displayProperty=nameWithType> expõe quase todas as funcionalidades de PLINQ. Ela e o restante dos tipos <xref:System.Linq?displayProperty=nameWithType> de namespace são compilados no assembly System.Core.dll. Os projetos padrão de C# e do Visual Basic no Visual Studio fazem referência ao assembly e importam o namespace.

<xref:System.Linq.ParallelEnumerable> inclui implementações de todos os operadores de consulta padrão com suporte do LINQ to Objects, embora ele não tente paralelizar cada uma. Se você não estiver familiarizado com o LINQ, consulte [introdução ao LINQ (C#)](../../csharp/programming-guide/concepts/linq/index.md) e [introdução ao LINQ (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/introduction-to-linq.md).

Além dos operadores de consulta padrão, a classe <xref:System.Linq.ParallelEnumerable> contém um conjunto de métodos que permitem comportamentos específicos para execução paralela. Esses métodos específicos de PLINQ são listados na tabela a seguir.

|Operador ParallelEnumerable|Descrição|
|---------------------------------|-----------------|
|<xref:System.Linq.ParallelEnumerable.AsParallel%2A>|O ponto de entrada para PLINQ. Especifica que o restante da consulta deverá ser paralelizado, se possível.|
|<xref:System.Linq.ParallelEnumerable.AsSequential%2A>|Especifica que o restante da consulta deve ser executado em sequência, como uma consulta LINQ não paralela.|
|<xref:System.Linq.ParallelEnumerable.AsOrdered%2A>|Especifica que a PLINQ deve preservar a ordenação da sequência de origem para o restante da consulta, ou até que a ordenação seja alterada, por exemplo, pelo uso de uma cláusula orderby (Order By no Visual Basic).|
|<xref:System.Linq.ParallelEnumerable.AsUnordered%2A>|Especifica que a PLINQ para o restante da consulta não precisa preservar a ordenação da sequência de origem.|
|<xref:System.Linq.ParallelEnumerable.WithCancellation%2A>|Especifica que a PLINQ deve monitorar periodicamente o estado do token de cancelamento fornecido e cancelar a execução se isso for solicitado.|
|<xref:System.Linq.ParallelEnumerable.WithDegreeOfParallelism%2A>|Especifica o número máximo de processadores que a PLINQ deve usar para paralelizar a consulta.|
|<xref:System.Linq.ParallelEnumerable.WithMergeOptions%2A>|Fornece uma dica sobre como a PLINQ deve, se possível, mesclar resultados paralelos novamente em apenas uma sequência no segmento de consumo.|
|<xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A>|Especifica se a PLINQ deve paralelizar a consulta mesmo quando o comportamento padrão for executá-la em sequência.|
|<xref:System.Linq.ParallelEnumerable.ForAll%2A>|Um método de enumeração multithread que, ao contrário de iterar sobre os resultados da consulta, permite que os resultados sejam processados em paralelo sem primeiro mesclar de volta para o thread de consumidor.|
|sobrecarga de <xref:System.Linq.ParallelEnumerable.Aggregate%2A>|Uma sobrecarga que é exclusiva da PLINQ e permite agregação intermediária em partições de thread local, além de uma função de agregação final para combinar os resultados de todas as partições.|

## <a name="the-opt-in-model"></a>O Modelo de Aceite

Ao escrever uma consulta, escolha o PLINQ invocando o método de extensão <xref:System.Linq.ParallelEnumerable.AsParallel%2A?displayProperty=nameWithType> na fonte de dados, conforme mostrado no exemplo a seguir.

[!code-csharp[PLINQ#1](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinq2_cs.cs#1)]
[!code-vb[PLINQ#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#1)]

O método de extensão <xref:System.Linq.ParallelEnumerable.AsParallel%2A> associa os operadores de consulta subsequentes, nesse caso, `where` e `select`, às implementações <xref:System.Linq.ParallelEnumerable?displayProperty=nameWithType>.

## <a name="execution-modes"></a>Modos de Execução

Por padrão, a PLINQ é conservadora. Em tempo de execução, a infraestrutura da PLINQ analisa a estrutura geral da consulta. Se for provável que a consulta produza aumentos de velocidade por paralelização, a PLINQ particionará a sequência de origem em tarefas que podem ser executadas simultaneamente. Se não for seguro a paralelização de uma consulta, a PLINQ apenas executa a consulta em sequência. Se a PLINQ tiver a opção de escolher entre um algoritmo paralelo potencialmente caro ou um algoritmo sequencial barato, ela escolherá o algoritmo sequencial, por padrão. Você pode usar o método <xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A> e a enumeração <xref:System.Linq.ParallelExecutionMode?displayProperty=nameWithType> para instruir o PLINQ a selecionar o algoritmo em paralelo. Isso é útil quando você sabe por meio de testes e medidas que uma determinada consulta é executada mais rapidamente em paralelo. Para saber mais, veja [Como especificar o modo de execução em PLINQ](how-to-specify-the-execution-mode-in-plinq.md).

## <a name="degree-of-parallelism"></a>Grau de paralelismo

Por padrão, o PLINQ utiliza a todos os processadores no computador host. Você pode instruir o PLINQ a não usar mais do que um número especificado de processadores usando o método <xref:System.Linq.ParallelEnumerable.WithDegreeOfParallelism%2A>. Isso é útil quando você deseja certificar-se de que outros processos em execução no computador receberão um determinado período de tempo de CPU. O snippet a seguir limita a consulta ao utilizar no máximo dois processadores.

[!code-csharp[PLINQ#5](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#5)]
[!code-vb[PLINQ#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#5)]

Em casos onde uma consulta esteja executando uma quantidade significativa de trabalho associado a computação, como E/S de Arquivo, pode ser útil especificar um grau de paralelismo maior do que o número de núcleos no computador.

## <a name="ordered-versus-unordered-parallel-queries"></a>Consultas Paralelas Ordenadas Contra Não Ordenadas

Em algumas consultas, um operador de consulta deve produzir resultados que preservem a ordenação da sequência de origem. O PLINQ fornece o operador <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> para essa finalidade. <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> é diferente de <xref:System.Linq.ParallelEnumerable.AsSequential%2A>. Uma sequência <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> ainda é processada em paralelo, mas os resultados são armazenados em buffer e classificados. Como a preservação da ordem normalmente envolve trabalho extra, uma sequência <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> pode ser processada mais lentamente do que a sequência padrão <xref:System.Linq.ParallelEnumerable.AsUnordered%2A>. Se uma determinada operação paralela ordenada será mais rápida do que uma versão sequencial da operação, isso dependerá de vários fatores.

O exemplo de código a seguir mostra como aceitar a preservação da ordem.

[!code-csharp[PLINQ#3](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinq2_cs.cs#3)]
[!code-vb[PLINQ#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#3)]

Para saber mais, veja [Preservação da ordem em PLINQ](order-preservation-in-plinq.md).

## <a name="parallel-vs-sequential-queries"></a>Consultas paralelas versus sequenciais

Algumas operações exigem que os dados de origem sejam entregue de forma sequencial. Os operadores de consulta <xref:System.Linq.ParallelEnumerable> voltam para o modo sequencial automaticamente, quando necessário. Para os operadores de consulta definidos pelo usuário e os representantes do usuário que exigem execução sequencial, o PLINQ fornece o método <xref:System.Linq.ParallelEnumerable.AsSequential%2A>. Quando você usa <xref:System.Linq.ParallelEnumerable.AsSequential%2A>, todos os operadores subsequentes na consulta são executados sequencialmente até <xref:System.Linq.ParallelEnumerable.AsParallel%2A> ser chamado novamente. Para saber mais, veja [Como combinar consultas LINQ paralelas e sequenciais](how-to-combine-parallel-and-sequential-linq-queries.md).

## <a name="options-for-merging-query-results"></a>Opções para Mesclar Resultados da Consulta

Quando uma consulta PLINQ é executada em paralelo, seus resultados de cada thread de trabalho precisam ser mesclados de volta com o thread principal para serem consumidos por um loop `foreach` (`For Each` em Visual Basic) ou inseridos em uma lista ou matriz. Em alguns casos, ela pode ser útil para especificar um determinado tipo de operação de mesclagem, por exemplo, para começar a produzir resultados mais rapidamente. Para essa finalidade, o PLINQ dá suporte ao método <xref:System.Linq.ParallelEnumerable.WithMergeOptions%2A> e à enumeração <xref:System.Linq.ParallelMergeOptions>. Para saber mais, veja [Opções de mesclagem em PLINQ](merge-options-in-plinq.md).

## <a name="the-forall-operator"></a>O Operador ForAll

Em consultas de LINQ sequenciais, a execução é adiada até que a consulta seja enumerada em um `foreach` `For Each` loop (no Visual Basic) ou invocando um método como <xref:System.Linq.ParallelEnumerable.ToList%2A> , <xref:System.Linq.ParallelEnumerable.ToArray%2A> ou <xref:System.Linq.ParallelEnumerable.ToDictionary%2A> . Na PLINQ, você também pode usar `foreach` para executar a consulta e percorrer os resultados. No entanto, `foreach` em si não é executada em paralelo e, portanto, requer que a saída de todas as tarefas paralelas seja mesclada de volta ao thread que está executando o loop. Na PLINQ, você pode usar `foreach` quando tiver de preservar a ordenação final dos resultados da consulta e também sempre que estiver processando os resultados de forma serial, por exemplo, quando estiver chamando `Console.WriteLine` para cada elemento. Para uma execução mais rápida quando a preservação da ordem não for necessário e quando o processamento dos resultados possa ele próprio ser paralelizado, use o método <xref:System.Linq.ParallelEnumerable.ForAll%2A> para executar uma consulta PLINQ. <xref:System.Linq.ParallelEnumerable.ForAll%2A> não executa essa etapa de mesclagem final. O exemplo de código a seguir mostra como usar o método <xref:System.Linq.ParallelEnumerable.ForAll%2A>. <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> é usado aqui porque é otimizado para vários threads, adicionando simultaneamente sem tentar remover itens.

[!code-csharp[PLINQ#4](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinq2_cs.cs#4)]
[!code-vb[PLINQ#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#4)]

A ilustração a seguir mostra a diferença entre `foreach` e <xref:System.Linq.ParallelEnumerable.ForAll%2A> em relação à execução da consulta.

![ForAll vs. ForEach](media/vs-isvnt-allvseach.png "VS_ISVNT_ALLvsEACH")

## <a name="cancellation"></a>Cancelamento

A PLINQ é integrada aos tipos de cancelamento no .NET Framework 4. (Para obter mais informações, consulte [cancelamento em threads gerenciados](../threading/cancellation-in-managed-threads.md).) Portanto, diferentemente das consultas de LINQ to Objects sequenciais, as consultas PLINQ podem ser canceladas. Para criar uma consulta PLINQ anulável, use o operador <xref:System.Linq.ParallelEnumerable.WithCancellation%2A> na consulta e forneça uma instância <xref:System.Threading.CancellationToken> como argumento. Quando a propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> no token é definida como true, o PLINQ a observa, para o processamento de todos os threads e lança um <xref:System.OperationCanceledException>.

É possível que uma consulta PLINQ possa continuar a processar alguns elementos depois que o token de cancelamento é definido.

Para maior capacidade de resposta, você também pode responder a solicitações de cancelamento em representantes do usuário de longa duração. Para saber mais, veja [Como cancelar uma consulta PLINQ](how-to-cancel-a-plinq-query.md).

## <a name="exceptions"></a>Exceções

Quando uma consulta PLINQ é executada, várias exceções podem ser geradas de diversos threads simultaneamente. Além disso, o código para tratar a exceção pode estar em um thread diferente do código que gerou a exceção. O PLINQ utiliza o tipo <xref:System.AggregateException> para encapsular todas as exceções que foram geradas por uma consulta e para realizar marshaling nessas exceções no thread de chamada. No thread de chamada, apenas um bloco try-catch é necessário. No entanto, você pode iterar por todas as exceções encapsuladas na <xref:System.AggregateException> e capturar qualquer uma que você possa recuperar com segurança. Em casos raros, podem ser geradas algumas exceções que não são encapsuladas em um <xref:System.AggregateException>, e <xref:System.Threading.ThreadAbortException>s também não são encapsulados.

Quando as exceções tiverem permissão de emergirem novamente para o thread de associação, então será possível que uma consulta continue a processar alguns itens após a geração da exceção.

Para saber mais, veja [Como tratar exceções em uma consulta PLINQ](how-to-handle-exceptions-in-a-plinq-query.md).

## <a name="custom-partitioners"></a>Particionadores Personalizados

Em alguns casos, você pode melhorar o desempenho da consulta escrevendo um particionador personalizado que aproveite algumas características da fonte de dados. Na consulta, o particionador personalizado em si é o objeto enumerável que será consultado.

[!code-csharp[PLINQ#2](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinq2_cs.cs#2)]
[!code-vb[PLINQ#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq3.vb#2)]

A PLINQ oferece suporte a um número fixo de partições (embora os dados possam ser dinamicamente reatribuídos a essas partições durante o tempo de execução para balanceamento de carga.). <xref:System.Threading.Tasks.Parallel.For%2A> e <xref:System.Threading.Tasks.Parallel.ForEach%2A> dão suporte somente ao particionamento dinâmico, o que significa que o número de partições é alterado em tempo de execução. Para saber mais, veja [Particionadores personalizados para PLINQ e TPL](custom-partitioners-for-plinq-and-tpl.md).

## <a name="measuring-plinq-performance"></a>Medindo Desempenho PLINQ

Em muitos casos, uma consulta pode ser paralelizada, mas a sobrecarga de configuração de consulta paralela supera o benefício de desempenho obtido. Se uma consulta não gerar muita computação ou se a fonte de dados for pequena, uma consulta PLINQ poderá ser mais lenta do que uma consulta sequencial LINQ to Objects. Você pode usar o Analisador de Desempenho Paralelo no Visual Studio Team Server para comparar o desempenho de várias consultas, para localizar gargalos de processamento e para determinar se a consulta está em execução em paralelo ou sequencialmente. Para saber mais, veja [Visualizador de Simultaneidade](/visualstudio/profiling/concurrency-visualizer) e [Como medir o Desempenho da Consulta PLINQ](how-to-measure-plinq-query-performance.md).

## <a name="see-also"></a>Confira também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
- [Noções básicas sobre agilização em PLINQ](understanding-speedup-in-plinq.md)
