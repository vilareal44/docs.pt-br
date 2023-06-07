---
title: 'Como: usar Parallel.Invoke para executar operações paralelas'
description: Consulte como usar o método Parallel. Invoke na TPL (biblioteca paralela de tarefas), que faz operações paralelas em uma fonte de dados compartilhada no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- task parallelism in .NET
- parallel programming, task parallelism
ms.assetid: 6b3ecd79-dec9-4ce1-abf4-62e5392a59c6
ms.openlocfilehash: 084ade48b1406d23a11eb311739525f35ac973df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596339"
---
# <a name="how-to-use-parallelinvoke-to-execute-parallel-operations"></a>Como: usar Parallel.Invoke para executar operações paralelas

Este exemplo mostra como paralelizar operações usando <xref:System.Threading.Tasks.Parallel.Invoke%2A> na Biblioteca de paralelismo de tarefas. Três operações são executadas em uma fonte de dados compartilhada. As operações podem ser executadas em paralelo de maneira simples, porque nenhuma delas modifica a origem.

> [!NOTE]
> Esta documentação usa expressões lambda para definir delegados na TLP. Se você não estiver familiarizado com expressões lambda em C# ou Visual Basic, consulte [expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).

## <a name="example"></a>Exemplo

[!code-csharp[TPL_Parallel#06](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallelinvoke.cs#06)]
[!code-vb[TPL_Parallel#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/parallelinvoke.vb#06)]

Com o <xref:System.Threading.Tasks.Parallel.Invoke%2A> , basta expressar quais ações você deseja executar simultaneamente, e o tempo de execução manipula todos os detalhes de agendamento de thread, incluindo o dimensionamento automático para o número de núcleos no computador host.

Este exemplo paraleliza as operações, não os dados. Como uma abordagem alternativa, você pode paralelizar as consultas do LINQ usando PLINQ e executar as consultas em sequência. Como alternativa, você poderia paralelizar os dados usando o PLINQ. Outra opção é paralelizar as consultas e as tarefas. Embora a sobrecarga resultante possa prejudicar o desempenho em computadores host com relativamente poucos processadores, ela é dimensionada melhor em computadores com muitos processadores.

## <a name="compile-the-code"></a>Compile o código

Copie e cole o exemplo inteiro em um projeto do Microsoft Visual Studio e pressione **F5**.

## <a name="see-also"></a>Consulte também

- [Programação paralela](index.md)
- [Como: Cancelar uma tarefa e seus filhos](how-to-cancel-a-task-and-its-children.md)
- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
