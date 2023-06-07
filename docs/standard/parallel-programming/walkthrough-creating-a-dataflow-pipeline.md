---
title: 'Passo a passo: criar um pipeline de fluxo de dados'
description: Crie um pipeline de Dataflow, que é uma série de componentes ou blocos de fluxo de os. Um bloco de fluxo de tipos faz uma determinada tarefa para contribuir com uma meta maior.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dataflow pipelines, creating with TPL
- Task Parallel Library, dataflows
- TPL dataflow library, creating dataflow pipeline
ms.assetid: 69308f82-aa22-4ac5-833d-e748533b58e8
ms.openlocfilehash: 7fe12b63b04d403334e4b64a421b105550467ca4
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84767865"
---
# <a name="walkthrough-creating-a-dataflow-pipeline"></a>Passo a passo: criar um pipeline de fluxo de dados
Embora possa usar os métodos <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Dataflow.DataflowBlock.TryReceive%2A?displayProperty=nameWithType> para receber mensagens dos blocos de origem, você também pode conectar blocos de mensagens para formar um *pipeline de fluxo de dados*. Um pipeline de fluxo de dados é uma série de componentes, ou *blocos de fluxo de dados*, e cada uma série executa uma tarefa específica que contribui para um objetivo maior. Todos os blocos de fluxo de dados em um pipeline de fluxo de dados realizam trabalhos ao receber uma mensagem de outro bloco de fluxo de dados. Como analogia, podemos usar uma linha de montagem de automóveis. À medida que os veículos passam por ela, uma estação monta a carroceria, a seguinte instala o motor e assim por diante. Como a linha de montagem permite que vários veículos sejam montados ao mesmo tempo, seu desempenho é superior se comparado com a montagem de um veículo completo por vez.

 Este documento demonstra um pipeline de fluxo de dados que baixa o catálogo *A Ilíada de Homero* de um site e pesquisa o texto para fazer a correspondência de palavras individuais com palavras que invertem os primeiros caracteres da palavra. A formação do pipeline de fluxo de dados neste documento conta com as seguintes etapas:  
  
1. Crie os blocos de fluxo de dados que participam do pipeline.  
  
2. Conecte cada bloco de fluxo de dados ao próximo bloco do pipeline. Cada bloco recebe como entrada a saída do bloco anterior no pipeline.  
  
3. Para cada bloco de fluxo de dados, crie uma tarefa de continuação que definirá o estado do próximo bloco como concluído após o término do bloco anterior.  
  
4. Publique dados no cabeçalho do pipeline.  
  
5. Marque o cabeçalho do pipeline como concluído.  
  
6. Aguarde até que o pipeline conclua todo o trabalho.  
  
## <a name="prerequisites"></a>Pré-requisitos  
 Leia sobre o [Fluxo de dados](dataflow-task-parallel-library.md) antes de iniciar essa explicação passo a passo.  
  
## <a name="creating-a-console-application"></a>Criar um Aplicativo de console  
 No Visual Studio, crie um projeto de aplicativo de console do Visual Basic ou do Visual C#. Instale o pacote NuGet de System.Threading.Tasks.Dataflow.

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

 Adicione o seguinte código ao projeto para criar o aplicativo básico.  
  
 [!code-csharp[TPLDataflow_Palindromes#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#2)]
 [!code-vb[TPLDataflow_Palindromes#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromesemptymain.vb#2)]  
  
## <a name="creating-the-dataflow-blocks"></a>Criar os blocos de fluxo de dados  
 Adicione o seguinte código ao método `Main` para criar os blocos de fluxo de dados que participam do pipeline. A tabela a seguir resume a função de cada membro do pipeline.  
  
 [!code-csharp[TPLDataflow_Palindromes#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#3)]
 [!code-vb[TPLDataflow_Palindromes#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#3)]  
  
|Membro|Tipo|Description|  
|------------|----------|-----------------|  
|`downloadString`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Baixa o texto do catálogo na Web.|  
|`createWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Separa o texto do catálogo em uma matriz de palavras.|  
|`filterWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Remove palavras curtas e duplicadas da matriz de palavras.|  
|`findReversedWords`|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|Localiza todas as palavras na coleção de matrizes de palavras filtradas cujo inverso também ocorre na matriz de palavras.|  
|`printReversedWords`|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|Exibe palavras e o inverso correspondente para o console.|  
  
 Embora você possa combinar várias etapas no pipeline de fluxo de dados deste exemplo em uma única etapa, o exemplo ilustra o conceito de composição de várias tarefas independentes do fluxo de dados para executar uma tarefa maior. O exemplo usa <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> para permitir que cada membro do pipeline execute uma operação em seus dados de entrada e envie os resultados para a próxima etapa do pipeline. O membro `findReversedWords` do pipeline é um objeto <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> porque produz várias saídas independentes para cada entrada. A parte final do pipeline, `printReversedWords`, é um objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> porque executa uma ação na entrada, mas não produz um resultado.  
  
## <a name="forming-the-pipeline"></a>Formação do pipeline  
 Adicione o código a seguir para conectar cada bloco ao próximo bloco do pipeline.  
  
 Quando você chama o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> para conectar um bloco de fluxo de dados de origem a um bloco de fluxo de dados de destino, o bloco de fluxo de dados de origem propaga dados para o bloco de destino assim que os dados ficam disponíveis. Se você também fornecer <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions> com <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions.PropagateCompletion> definido como "true", a conclusão bem-sucedida ou não de um bloco do pipeline levará à conclusão do próximo bloco do pipeline.
  
 [!code-csharp[TPLDataflow_Palindromes#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#4)]
 [!code-vb[TPLDataflow_Palindromes#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#4)]  
  
## <a name="posting-data-to-the-pipeline"></a>Publicar dados no pipeline  
 Adicione o seguinte código para publicar a URL do catálogo *A Ilíada de Homero* no cabeçalho do pipeline de fluxo de dados.  
  
 [!code-csharp[TPLDataflow_Palindromes#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#6)]
 [!code-vb[TPLDataflow_Palindromes#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#6)]  
  
 Este exemplo usa <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A?displayProperty=nameWithType> para enviar dados de forma síncrona para o cabeçalho do pipeline. Use o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A?displayProperty=nameWithType> quando precisar enviar dados de forma assíncrona para um nó do fluxo de dados.  
  
## <a name="completing-pipeline-activity"></a>Concluir a atividade do pipeline  
 Adicione o seguinte código para marcar o cabeçalho do pipeline como concluído. O cabeçalho do pipeline propaga sua conclusão após processar todas as mensagens em buffer.
  
 [!code-csharp[TPLDataflow_Palindromes#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#7)]
 [!code-vb[TPLDataflow_Palindromes#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#7)]  
  
 Este exemplo envia uma URL pelo pipeline de fluxo de dados que será processado. Se você enviar mais de uma entrada por um pipeline, chame o método <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A?displayProperty=nameWithType> após enviar todas as entradas. Você pode omitir essa etapa se seu aplicativo não tiver pontos bem definidos nos quais os dados já não estarão disponíveis ou se o aplicativo não precisar aguardar a conclusão do pipeline.  
  
## <a name="waiting-for-the-pipeline-to-finish"></a>Aguardar a conclusão do pipeline  
 Adicione o seguinte código para aguardar a conclusão do pipeline. A operação geral termina com a conclusão da parte final do pipeline.  
  
 [!code-csharp[TPLDataflow_Palindromes#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#8)]
 [!code-vb[TPLDataflow_Palindromes#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#8)]  
  
 Você pode aguardar a conclusão do fluxo de dados de qualquer thread ou de vários threads ao mesmo tempo.  
  
## <a name="the-complete-example"></a>O Exemplo Completo  
 O exemplo a seguir mostra o código completo dessa explicação passo a passo.  
  
 [!code-csharp[TPLDataflow_Palindromes#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#1)]
 [!code-vb[TPLDataflow_Palindromes#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#1)]  
  
## <a name="next-steps"></a>Próximas etapas  
 Este exemplo envia uma URL para ser processada pelo fluxo de dados do pipeline. Se você enviar mais de um valor de entrada pelo pipeline, introduza um formulário de paralelismo no aplicativo, semelhante à forma como as peças seriam movimentadas em uma fábrica de automóveis. Após o primeiro membro do pipeline enviar o resultado para o segundo membro, o primeiro membro poderá processar outro item em paralelo enquanto o segundo membro processa o primeiro resultado.  
  
 O paralelismo obtido usando pipelines de fluxo de dados é conhecido como *paralelismo de alta granularidade* porque, geralmente, é formado por menos tarefas, porém maiores. Você também pode usar um *paralelismo mais refinado* com tarefas menores e de curta execução em um pipeline de fluxo de dados. Neste exemplo, o membro `findReversedWords` do pipeline usa o [PLINQ](introduction-to-plinq.md) para processar em paralelo vários itens da lista de trabalho. O uso do paralelismo refinado em um pipeline de alta granularidade pode melhorar o desempenho geral.  
  
 Você também pode conectar um bloco de fluxo de dados de origem a vários blocos de destino para criar uma *rede de fluxo de dados*. A versão sobrecarregada do método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> usa um objeto <xref:System.Predicate%601> que define se o bloco de destino aceita cada mensagem com base em seu valor. A maioria dos tipos de blocos de fluxo de dados que agem como fontes fornecem mensagens para todos os blocos de destino conectados, na ordem em que estavam conectados, até que um dos blocos aceite essa mensagem. Com esse mecanismo de filtragem, é possível criar sistemas de blocos de fluxo de dados conectados que direcionam determinados dados por um caminho e outros dados por meio de outro caminho. Para ver um exemplo que usa a filtragem para criar uma rede de fluxo de dados, confira [Explicação passo a passo: usar o fluxo de dados em um aplicativo do Windows Forms](walkthrough-using-dataflow-in-a-windows-forms-application.md).  
  
## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
