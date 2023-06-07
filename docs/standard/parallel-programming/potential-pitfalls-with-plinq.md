---
title: Possíveis armadilhas com PLINQ
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, pitfalls
ms.assetid: 75a38b55-4bc4-488a-87d5-89dbdbdc76a2
ms.openlocfilehash: b4d58734fba4b834d5f5819a6bf19da0b7b7e8db
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285307"
---
# <a name="potential-pitfalls-with-plinq"></a>Possíveis armadilhas com PLINQ

Em muitos casos, o PLINQ pode fornecer melhorias de desempenho significativas em relação a consultas sequenciais do LINQ to Objects. No entanto, o trabalho de paralelizar a execução da consulta apresenta complexidade que pode levar a problemas que, em código sequencial, não são tão comuns ou não são encontrados. Este tópico lista algumas práticas a evitar ao escrever consultas PLINQ.

## <a name="dont-assume-that-parallel-is-always-faster"></a>Não presuma que Parallel é sempre mais rápido

A paralelização, por vezes, faz com que uma consulta PLINQ seja executada de forma mais devagar do que seu LINQ to Object equivalente. A regra básica é que as consultas que têm poucos elementos fonte e delegados de usuários rápidos provavelmente não acelerarão muito. No entanto, como muitos fatores estão envolvidos no desempenho, recomendamos avaliar os resultados reais antes de decidir pelo uso do PLINQ. Para saber mais, veja [Noções básicas sobre agilização em PLINQ](understanding-speedup-in-plinq.md).

## <a name="avoid-writing-to-shared-memory-locations"></a>Evite gravar em locais de memória compartilhada

No código sequencial, não é incomum ler ou gravar em variáveis estáticas ou campos de classe. No entanto, sempre que vários threads estão acessando essas variáveis simultaneamente, existe um grande potencial para condições de corrida. Embora você possa usar bloqueios para sincronizar o acesso à variável, o custo da sincronização pode prejudicar o desempenho. Portanto, recomendamos que você evite, ou pelo menos limite, o acesso ao estado compartilhado em uma consulta PLINQ tanto quanto possível.

## <a name="avoid-over-parallelization"></a>Evitar a maior paralelização

Usando o `AsParallel` método, você incorre em custos de sobrecarga do particionamento da coleção de origem e na sincronização dos threads de trabalho. Os benefícios da paralelização ainda estão limitados pelo número de processadores no computador. Não há nenhum aumento de velocidade a ser obtido executando vários threads vinculados à computação em apenas um processador. Portanto, você deve ter cuidado para não paralelizar excessivamente uma consulta.

O cenário mais comum em que a paralelização excessiva pode ocorrer é em consultas aninhadas, como mostrado no seguinte snippet.

[!code-csharp[PLINQ#20](~/samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#20)]
[!code-vb[PLINQ#20](~/samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#20)]

Nesse caso, é melhor paralelizar apenas a fonte de dados externa (clientes), a menos que uma ou mais das seguintes condições se apliquem:

- A fonte interna de dados (cust.Orders) é muito longa.

- Você está realizando uma computação cara em cada ordem. (A operação mostrada no exemplo não é cara).

- O sistema de destino tem processadores suficientes para lidar com o número de threads que serão produzidos paralelizando a consulta em `cust.Orders`.

Em todos os casos, a melhor maneira de determinar a forma ideal da consulta é testar e medir. Para saber mais, confira [Como avaliar o desempenho de consulta PLINQ](how-to-measure-plinq-query-performance.md).

## <a name="avoid-calls-to-non-thread-safe-methods"></a>Evite chamadas para métodos sem thread-safe

Gravar para métodos de instância não thread-safe a partir de uma consulta PLINQ pode levar à corrupção de dados que pode ou não ser detectada no seu programa. Isso também poderá levar a exceções. No exemplo a seguir, vários segmentos estão tentando chamar simultaneamente o método `FileStream.Write`, que não é compatível com a classe.

```vb
Dim fs As FileStream = File.OpenWrite(…)
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(Sub(x) fs.Write(x))
```

```csharp
FileStream fs = File.OpenWrite(...);
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(x => fs.Write(x));
```

## <a name="limit-calls-to-thread-safe-methods"></a>Limitar chamadas para métodos de thread-safe

A maioria dos métodos estáticos no .NET Framework é thread-safe e pode ser chamada de vários threads simultaneamente. No entanto, mesmo nesses casos, a sincronização envolvida pode levar a uma desaceleração significativa na consulta.

> [!NOTE]
> Você pode testar isso sozinho inserindo algumas chamadas para <xref:System.Console.WriteLine%2A> nas suas consultas. Embora esse método seja usado nos exemplos de documentação para fins de demonstração, não o use em consultas PLINQ.

## <a name="avoid-unnecessary-ordering-operations"></a>Evite operações de ordenação desnecessárias

Quando o PLINQ executa uma consulta em paralelo, divide a sequência de origem em partições que podem ser operadas simultaneamente em múltiplos segmentos. Por padrão, a ordem em que as partições são processadas e os resultados fornecidos não é previsível (exceto para operadores como `OrderBy`). Você pode instruir o PLINQ a preservar a classificação de qualquer sequência de origem, mas isso tem um impacto negativo no desempenho. A prática recomendada, sempre que possível, é estruturar consultas para que elas não dependam da preservação da classificação. Para saber mais, veja [Preservação da ordem em PLINQ](order-preservation-in-plinq.md).

## <a name="prefer-forall-to-foreach-when-it-is-possible"></a>Prefira ForAll para ForEach quando possível

Embora o PLINQ execute uma consulta em múltiplos threads, se você consumir os resultados em um loop `foreach` (`For Each` no Visual Basic), os resultados da consulta devem ser mesclados novamente em um thread e acessados em série pelo enumerador. Em alguns casos, isso é inevitável. No entanto, sempre que possível, use o método `ForAll` para habilitar cada thread a gerar seus próprios resultados, por exemplo, gravando para uma coleção thread-safe como <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>.

O mesmo problema se aplica a <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> . Em outras palavras, `source.AsParallel().Where().ForAll(...)` deve ser altamente preferencial `Parallel.ForEach(source.AsParallel().Where(), ...)` .

## <a name="be-aware-of-thread-affinity-issues"></a>Esteja ciente dos problemas de afinidade de thread

Algumas tecnologias, por exemplo, interoperabilidade COM para componentes de um único segmento (STA), Windows Forms e Windows Presentation Foundation (WPF), impõem restrições de afinidade de thread que exigem que o código seja executado em um thread específico. Por exemplo, tanto no Windows Forms quanto no WPF, um controle só pode ser acessado no thread em que foi criado. Se você tenta acessar o estado compartilhado de um controle Windows Forms em uma consulta PLINQ, uma exceção é gerada se você estiver executando no depurador. (Essa configuração pode ser desativada.) No entanto, se a consulta for consumida no thread da interface do usuário, você poderá acessar o controle do `foreach` loop que enumera os resultados da consulta porque esse código é executado em apenas um thread.

## <a name="dont-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>Não presuma que iterações de ForEach, for e ForAll sempre são executadas em paralelo

É importante ter em mente que as iterações individuais em um <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> loop, <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> ou <xref:System.Linq.ParallelEnumerable.ForAll%2A> podem não ser executadas em paralelo. Portanto, você deve evitar gravar qualquer código que dependa da correção na execução paralela de iterações ou na execução de iterações em qualquer classificação específica.

Por exemplo, esse código é provavelmente um deadlock:

```vb
Dim mre = New ManualResetEventSlim()
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll(Sub(j)
   If j = Environment.ProcessorCount Then
       Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Set()
   Else
       Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Wait()
   End If
End Sub) ' deadlocks
```

```csharp
ManualResetEventSlim mre = new ManualResetEventSlim();
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll((j) =>
{
    if (j == Environment.ProcessorCount)
    {
        Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Set();
    }
    else
    {
        Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Wait();
    }
}); //deadlocks
```

Neste exemplo, uma iteração define um evento e todas as outras iterações aguardam o evento. Nenhuma das iterações em espera pode ser concluída até que a iteração de configuração do evento tenha sido concluída. No entanto, é possível que as iterações em espera bloqueiem todos os threads que são usados para executar o loop paralelo, antes que a iteração de configuração do evento tenha tido a chance de ser executada. Isso resulta em um deadlock – a iteração de configuração do evento nunca será executada e as iterações em espera nunca serão ativadas.

Em particular, uma iteração de um loop paralelo nunca deve aguardar outra iteração do loop para progredir. Se o loop paralelo decidir agendar as iterações sequencialmente, mas em ordem oposta, ocorrerá um deadlock.

## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
