---
title: Particionadores personalizados para PLINQ e TPL
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, partitioners
ms.assetid: 96153688-9a01-47c4-8430-909cee9a2887
ms.openlocfilehash: 50553aab30d5a1bc5880ae0fe39c34508e57d0e5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276719"
---
# <a name="custom-partitioners-for-plinq-and-tpl"></a>Particionadores personalizados para PLINQ e TPL

Para paralelizar a uma operação em uma fonte de dados, uma das etapas essenciais é *particionar* a fonte em várias seções que possam ser acessadas simultaneamente por vários threads. O PLINQ e a TPL (Biblioteca de Paralelismo de Tarefas) fornecem particionadores padrão que funcionam de forma transparente quando você escreve uma consulta paralela ou um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A>. Para cenários mais avançados, você pode conectar seu próprio particionador.

## <a name="kinds-of-partitioning"></a>Tipos de particionamento

Há muitas maneiras de particionar uma fonte de dados. Nas abordagens mais eficientes, vários threads cooperam para processar a sequência de origem original, em vez de separar fisicamente a origem em várias subsequências. Para matrizes e outras fontes indexadas como coleções <xref:System.Collections.IList> em que o tamanho é conhecido com antecedência, o *particionamento por intervalos* é o tipo mais simples de particionamento. Cada thread recebe índices exclusivos de abertura e fechamento, para que possa processar seu intervalo da origem sem substituir ou ser substituído por qualquer outro thread. A única sobrecarga envolvida no particionamento por intervalos é o trabalho inicial de criação de intervalos. Nenhuma sincronização adicional é necessária depois disso. Portanto, pode fornecer bom desempenho, desde que a carga de trabalho seja dividida igualmente. Uma desvantagem do particionamento por intervalos é que se um thread termina cedo, não pode ajudar os outros threads a concluírem seu trabalho.

Para listas vinculadas ou outras coleções cujo comprimento não é conhecido, você pode usar o *particionamento por partes*. Na parte de particionamento, cada thread ou tarefa em uma consulta ou um loop paralelo consome alguns elementos de origem em um bloco, processa-os e volta para recuperar elementos adicionais. O particionador garante que todos os elementos sejam distribuídos e que não haja duplicatas. Uma parte pode ser de qualquer tamanho. Por exemplo, o particionador que é demonstrado em [Como implementar partições dinâmicas](how-to-implement-dynamic-partitions.md) cria blocos que contêm apenas um elemento. Como as partes não são muito grandes, esse tipo de particionamento é, inerentemente, um balanceamento de carga, pois a atribuição de elementos a threads não é predeterminada. No entanto, o particionador causa sobrecarga de sincronização sempre que o thread precisa obter outro bloco. A quantidade de sincronização que ocorre nesses casos é inversamente proporcional ao tamanho das partes.

Em geral, a partição de intervalo só é mais rápida quando o tempo de execução do representante é pequeno a médio e a origem tem um grande número de elementos e o trabalho total de cada partição é aproximadamente equivalente. Portanto, o particionamento de bloco geralmente é mais rápido na maioria dos casos. Em origens com um pequeno número de elementos ou tempos de execução maiores para o representante, o desempenho de bloco e particionamento por intervalos é mais ou menos igual.

Os particionadores de TPL também dão suporte a um número dinâmico de partições. Isso significa que podem criar partições imediatamente, por exemplo, quando o loop <xref:System.Threading.Tasks.Parallel.ForEach%2A> gera uma nova tarefa. Esse recurso permite que o particionador seja dimensionado junto com o próprio loop. Os particionadores dinâmicos também realizam, inerentemente, o balanceamento de carga. Ao criar um particionador personalizado, você deve dar suporte ao particionamento dinâmico para ser consumível de um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A>.

### <a name="configuring-load-balancing-partitioners-for-plinq"></a>Configuração de particionadores de balanceamento de carga para PLINQ

Algumas sobrecargas do método <xref:System.Collections.Concurrent.Partitioner.Create%2A?displayProperty=nameWithType> permitem que você crie um particionador para uma matriz ou origem <xref:System.Collections.IList> e especifique se deve tentar equilibrar a carga de trabalho entre os threads. Quando o particionador está configurado para balanceamento de carga, o particionamento por partes é usado, e os elementos são enviados para cada partição em pequenas partes conforme são solicitados. Essa abordagem ajuda a garantir que todas as partições tenham elementos para processar até que a consulta ou o loop inteiro seja concluído. Uma sobrecarga adicional pode ser usada para fornecer particionamento de balanceamento de carga de qualquer origem de <xref:System.Collections.IEnumerable>.

Em geral, o balanceamento de carga requer que as partições solicitem elementos relativamente com frequência do particionador. Por outro lado, um particionador que faz o particionamento estático pode atribuir os elementos a cada particionador de uma só vez usando o particionamento por intervalos ou bloco. Isso requer menos sobrecarga de balanceamento de carga, mas pode levar mais tempo para ser executado se um thread acaba com significativamente mais trabalho do que os outros. Por padrão, quando recebe uma IList ou uma matriz, o PLINQ sempre usa o particionamento por intervalos sem balanceamento de carga. Para habilitar o balanceamento de carga para o PLINQ, use o método `Partitioner.Create`, conforme mostrado no exemplo a seguir.

[!code-csharp[TPL_Partitioners#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioners.cs#02)]
[!code-vb[TPL_Partitioners#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionsnippets_vb.vb#02)]

A melhor maneira de determinar se é preciso usar o balanceamento de carga em qualquer cenário específico é testar e medir o tempo necessário para concluir operações em cargas representativas e configurações de computador. Por exemplo, o particionamento estático pode fornecer um aumento de velocidade significativo em um computador com vários núcleos que tenha apenas alguns núcleos, mas pode resultar em lentidão em computadores que têm relativamente muitos núcleos.

A tabela a seguir lista as sobrecargas disponíveis do método <xref:System.Collections.Concurrent.Partitioner.Create%2A>. Essas particionadores não estão limitados ao uso somente com PLINQ ou <xref:System.Threading.Tasks.Task>. Também podem ser usados com qualquer constructo paralelo personalizado.

|Sobrecarga|Usa o balanceamento de carga|
|--------------|-------------------------|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>|Sempre|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28%60%600%5B%5D%2CSystem.Boolean%29>|Quando o argumento Boolean é especificado como true|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IList%7B%60%600%7D%2CSystem.Boolean%29>|Quando o argumento Boolean é especificado como true|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%29>|Nunca|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%2CSystem.Int32%29>|Nunca|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%29>|Nunca|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%2CSystem.Int64%29>|Nunca|

### <a name="configuring-static-range-partitioners-for-parallelforeach"></a>Configurando particionadores de intervalo estático para Parallel.ForEach

Em um loop <xref:System.Threading.Tasks.Parallel.For%2A>, o corpo do loop é fornecido para o método como um representante. O custo de invocar esse representante é quase o mesmo que o de uma chamada de método virtual. Em alguns cenários, o corpo de um loop paralelo pode ser pequeno o suficiente para que o custo da invocação do representante em cada iteração do loop se torne significativo. Nessas situações, você pode usar uma das sobrecargas <xref:System.Collections.Concurrent.Partitioner.Create%2A> para criar um <xref:System.Collections.Generic.IEnumerable%601> de partições de intervalo sobre os elementos de origem. Em seguida, você pode passar essa coleção de intervalos para um método <xref:System.Threading.Tasks.Parallel.ForEach%2A> cujo corpo consiste em um loop `for` regular. A vantagem dessa abordagem é que o custo de invocação do representante é incorrido apenas uma vez por intervalo, em vez de uma vez por elemento. O exemplo a seguir demonstra o padrão básico.

[!code-csharp[TPL_Partitioners#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioner01.cs#01)]
[!code-vb[TPL_Partitioners#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionercreate01.vb#01)]

Cada thread no loop recebe seu próprio <xref:System.Tuple%602>, que contém os valores de índice inicial e final no subintervalo especificado. O loop interno `for` usa os valores `fromInclusive` e `toExclusive` para executar um loop sobre a matriz ou o <xref:System.Collections.IList> diretamente.

Uma das sobrecargas <xref:System.Collections.Concurrent.Partitioner.Create%2A> permite especificar o tamanho de partições e o número delas. Essa sobrecarga pode ser usada em cenários em que o trabalho por elemento é tão baixo que até mesmo uma chamada de método virtual por elemento tem impacto significativo sobre o desempenho.

## <a name="custom-partitioners"></a>Particionadores Personalizados

Em alguns cenários, talvez seja útil ou até mesmo necessário implementar seu próprio particionador. Por exemplo, você pode ter uma classe de coleção personalizada que pode particionar de forma mais eficiente do que os particionadores padrão, com base em seu conhecimento sobre a estrutura interna da classe. Ou, talvez, você queira criar partições de intervalo de tamanhos variados com base em seu conhecimento de quanto tempo levará para processar elementos em locais diferentes na coleção de origem.

Para criar um particionador personalizado básico, derive uma classe de <xref:System.Collections.Concurrent.Partitioner%601?displayProperty=nameWithType> e substitua os métodos virtuais, conforme descrito na tabela a seguir.

|||
|-|-|
|<xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>|Esse método é chamado uma vez pelo thread principal e retorna um IList(IEnumerator(TSource)). Cada thread de trabalho na consulta ou no loop pode chamar `GetEnumerator` na lista para recuperar um <xref:System.Collections.Generic.IEnumerator%601> por uma partição distinta.|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|Retorne `true` se você implementar <xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>; caso contrário, `false`.|
|<xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>|Se <xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> for `true`, esse método poderá ser opcionalmente chamado, em vez de <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>.|

Se os resultados precisarem ser classificáveis ou exigirem acesso indexado aos elementos, derive de <xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> e substitua seus métodos virtuais, conforme descrito na tabela a seguir.

|||
|-|-|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetPartitions%2A>|Esse método é chamado uma vez pelo thread principal e retorna um `IList(IEnumerator(TSource))`. Cada thread de trabalho na consulta ou no loop pode chamar `GetEnumerator` na lista para recuperar um <xref:System.Collections.Generic.IEnumerator%601> por uma partição distinta.|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|Retorne `true` se você implementar <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A>; caso contrário, false.|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A>|Normalmente, isso apenas chama <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A>.|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A>|Se <xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> for `true`, esse método poderá ser opcionalmente chamado, em vez de <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>.|

A tabela a seguir fornece detalhes adicionais sobre como os três tipos de particionadores de balanceamento de carga implementam a classe <xref:System.Collections.Concurrent.OrderablePartitioner%601>.

|Método/propriedade|IList/matriz sem balanceamento de carga|IList/matriz com balanceamento de carga|IEnumerable|
|----------------------|-------------------------------------------|----------------------------------------|-----------------|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>|Usa o particionamento por intervalos|Usa o particionamento por partes otimizado para Listas para o partitionCount especificado|Usa o particionamento por partes por meio da criação de um número estático de partições.|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A?displayProperty=nameWithType>|Gera uma exceção sem suporte|Usa o particionamento por partes otimizado para Listas e partições dinâmicas|Usa o particionamento por partes por meio da criação de um número dinâmico de partições.|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedInEachPartition%2A>|Retorna `true`|Retorna `true`|Retorna `true`|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedAcrossPartitions%2A>|Retorna `true`|Retorna `false`|Retorna `false`|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysNormalized%2A>|Retorna `true`|Retorna `true`|Retorna `true`|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|Retorna `false`|Retorna `true`|Retorna `true`|

### <a name="dynamic-partitions"></a>Partições dinâmicas

Se desejar que o particionador seja usado em um método <xref:System.Threading.Tasks.Parallel.ForEach%2A>, você deverá ser capaz de retornar um número dinâmico de partições. Isso significa que o particionador pode fornecer um enumerador para uma nova partição sob demanda a qualquer momento durante a execução do loop. Basicamente, sempre que o loop adicionar uma nova tarefa paralela, solicitará uma nova partição para a tarefa. Se for preciso que os dados sejam ordenáveis, derive <xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> para que seja atribuído um índice exclusivo a cada item em cada partição.

Para saber mais e obter um exemplo, confira [Como implementar partições dinâmicas](how-to-implement-dynamic-partitions.md).

### <a name="contract-for-partitioners"></a>Contrato para particionadores

Ao implementar um particionador personalizado, siga estas diretrizes para garantir a interação correta com PLINQ e <xref:System.Threading.Tasks.Parallel.ForEach%2A> na TPL:

- Se <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> for chamado com um argumento de zero ou menos para `partitionsCount`, gere <xref:System.ArgumentOutOfRangeException>. Embora o PLINQ e o TPL nunca passem um `partitionCount` igual a 0, mesmo assim, recomendamos que você proteja contra a possibilidade.

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> e <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A> devem retornar sempre `partitionsCount` partições. Se o particionador ficar sem dados e não puder criar tantas partições quantas foram solicitadas, o método deverá retornar um enumerador vazio para cada uma das partições restantes. Caso contrário, o PLINQ e o TPL gerarão um <xref:System.InvalidOperationException>.

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>, <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>, <xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A> e <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A> nunca devem retornar `null` (`Nothing` no Visual Basic). Se isso ocorrer, PLINQ/TPL gerará um <xref:System.InvalidOperationException>.

- Os métodos que retornam partições devem retornar sempre partições que podem enumerar total e exclusivamente a fonte de dados. Não deve haver nenhuma duplicação na fonte de dados nem itens ignorados, a menos que isso seja especificamente necessário para o design do particionador. Se essa regra não for seguida, a ordem de saída poderá ser embaralhada.

- Os seguintes getters boolianos devem sempre retornar com precisão os seguintes valores para que a ordem de saída não seja embaralhada:

  - `KeysOrderedInEachPartition`: cada partição retorna elementos com índices de chave crescentes.

  - `KeysOrderedAcrossPartitions`: para todas as partições que são retornadas, os índices de chave na partição *i* são maiores do que os índices de chave na partição *i*-1.

  - `KeysNormalized`: todos os índices de chave aumentam de forma monotônica sem lacunas, a partir de zero.

- Todos os índices devem ser exclusivos. Não pode haver índices duplicados. Se essa regra não for seguida, a ordem de saída poderá ser embaralhada.

- Todos os índices devem ser não negativos. Se essa regra não for seguida, PLINQ/TPL poderão gerar exceções.

## <a name="see-also"></a>Veja também

- [Programação paralela](index.md)
- [Como: Implementar partições dinâmicas](how-to-implement-dynamic-partitions.md)
- [Como: implementar um particionador para particionamento estático](how-to-implement-a-partitioner-for-static-partitioning.md)
