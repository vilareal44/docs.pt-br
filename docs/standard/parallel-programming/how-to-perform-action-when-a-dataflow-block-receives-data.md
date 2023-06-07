---
title: 'Como: executar ações quando um bloco de fluxo de dados recebe dados'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, receiving data
ms.assetid: fc2585dc-965e-4632-ace7-73dd02684ed3
ms.openlocfilehash: 647e77f0c5e182cea90f6e90063826b705de354b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288167"
---
# <a name="how-to-perform-action-when-a-dataflow-block-receives-data"></a>Como: executar ações quando um bloco de fluxo de dados recebe dados
Os tipos de *Blocos de fluxo de dados de execução* chamam um representante fornecido pelo usuário ao receber dados. As classes <xref:System.Threading.Tasks.Dataflow.ActionBlock%601?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Dataflow.TransformBlock%602?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602?displayProperty=nameWithType> são tipos de blocos de fluxo de dados de execução. Você pode usar a palavra-chave `delegate` (`Sub` em Visual Basic), <xref:System.Action%601>, <xref:System.Func%602> ou uma expressão lambda ao fornecer uma função de trabalho a um bloco de fluxo de dados de execução. Este documento descreve como usar <xref:System.Func%602> e as expressões lambda para executar a ação em blocos de execução.  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o fluxo de dados para ler um arquivo do disco e calcula o número de bytes no arquivo que são iguais a zero. Ele usa <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> para ler o arquivo e calcula o número de zero bytes e <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> para imprimir o número de zero bytes no console. O objeto <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> especifica um objeto <xref:System.Func%602> para executar o trabalho quando os blocos recebem dados. O objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> usa uma expressão lambda para imprimir no console o número de zero bytes que são lidos.  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#1)]
 [!code-vb[TPLDataflow_ExecutionBlocks#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#1)]  
  
 Embora você possa fornecer uma expressão lambda para um objeto <xref:System.Threading.Tasks.Dataflow.TransformBlock%602>, este exemplo usa <xref:System.Func%602> para habilitar outro código para usar o método `CountBytes`. O objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> usa uma expressão lambda porque o trabalho a ser executado é específico dessa tarefa e provavelmente não será útil para outro código. Para obter mais informações sobre como as expressões lambda funcionam na Biblioteca de paralelismo de tarefas, confira [Expressões lambda no PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).  
  
 O resumo da seção de tipos delegados no documento de [fluxo](dataflow-task-parallel-library.md) de documentos resume os tipos delegados que você pode fornecer aos <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> objetos, e <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> . A tabela também especifica se o tipo de representante opera de forma síncrona ou assíncrona.  
  
## <a name="robust-programming"></a>Programação robusta  
 Este exemplo fornece um representante do tipo <xref:System.Func%602> ao objeto <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> para executar a tarefa do bloco de fluxo de dados de forma síncrona. Para habilitar o bloco de fluxo de dados para se comportar de forma assíncrona, forneça um representante do tipo <xref:System.Func%601> ao bloco de fluxo de dados. Quando um bloco de fluxo de dados se comporta de forma assíncrona, a tarefa do bloco de fluxo de dados só será concluída quando o objeto <xref:System.Threading.Tasks.Task%601>retornado for concluído. O exemplo a seguir modifica o método `CountBytes` e usa os operadores [async](../../csharp/language-reference/keywords/async.md) e [await](../../csharp/language-reference/operators/await.md) ([Async](../../visual-basic/language-reference/modifiers/async.md) e [Await](../../visual-basic/language-reference/operators/await-operator.md) em Visual Basic) para calcular de forma assíncrona o número total de bytes que tem o valor zero no arquivo fornecido. O método <xref:System.IO.FileStream.ReadAsync%2A> executa operações de leitura de arquivo de forma assíncrona.  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#2)]
 [!code-vb[TPLDataflow_ExecutionBlocks#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#2)]  
  
 Você também pode usar expressões lambda assíncronas para executar a ação em um bloco de fluxo de dados de execução. O exemplo a seguir modifica o objeto <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> usado no exemplo anterior para que ele use uma expressão lambda para executar o trabalho de forma assíncrona.  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#3)]
 [!code-vb[TPLDataflow_ExecutionBlocks#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#3)]  
  
## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
