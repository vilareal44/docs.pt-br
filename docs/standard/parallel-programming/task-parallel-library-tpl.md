---
title: Biblioteca de tarefas paralelas (TPL)
description: Explore a TPL (biblioteca paralela de tarefas), um conjunto de tipos públicos e APIs para simplificar o processo de adição de paralelismo & simultaneidade a aplicativos no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- .NET, concurrency in
- .NET, parallel programming in
- Parallel Programming
ms.assetid: b8f99f43-9104-45fd-9bff-385a20488a23
ms.openlocfilehash: 42768d99e7f3a15751ccf4c980edb9373666d49f
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768595"
---
# <a name="task-parallel-library-tpl"></a>Biblioteca de tarefas paralelas (TPL)
A Biblioteca Paralela de Tarefas (TPL) é um conjunto de tipos e APIs públicos em <xref:System.Threading?displayProperty=nameWithType> e namespaces do <xref:System.Threading.Tasks?displayProperty=nameWithType> em. O objetivo da TPL é tornar os desenvolvedores mais produtivos ao simplificar o processo de adicionar paralelismo e concorrência em aplicativos. A TPL dimensiona o grau de simultaneidade dinamicamente para usar todos os processadores disponíveis de forma mais eficiente. Além disso, a TPL lida com o particionamento do trabalho, a programação de threads em <xref:System.Threading.ThreadPool>, o suporte ao cancelamento, o gerenciamento de estados e outros detalhes de baixo nível. Ao usar a TPL, você pode maximizar o desempenho do seu código enquanto se concentra no trabalho para o qual seu programa foi criado para realizar.  
  
 Do .NET Framework 4 em diante, a TPL é a maneira preferencial para escrever códigos multi-threaded e paralelos. No entanto, nem todos os códigos são apropriados para paralelização; por exemplo, se um loop executa somente uma pequena quantidade de trabalho em cada iteração, ou se ele não é executado para muitas iterações, a sobrecarga da paralelização pode fazer com que o código seja executado mais lentamente. Além disso, a paralelização, assim como qualquer código multithread, acrescenta complexidade à execução do seu programa. Embora a TPL simplifique cenários multithread, recomendamos que você tenha uma compreensão básica dos conceitos de threads, por exemplo, bloqueios, deadlocks e condições de corrida para que possa usar a TPL efetivamente.  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-|-|  
|[Paralelismo de dados](data-parallelism-task-parallel-library.md)|Descreve como criar `for` paralelo e loops `foreach` (`For` e `For Each` no Visual Basic).|  
|[Programação assíncrona baseada em tarefas](task-based-asynchronous-programming.md)|Descreve como criar e executar tarefas implicitamente usando <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> ou explicitamente ao usar objetos <xref:System.Threading.Tasks.Task> diretamente.|  
|[Fluxo de dados](dataflow-task-parallel-library.md)|Descreve como usar os componentes do fluxo de dados na biblioteca de fluxo de dados de TPL para manipular várias operações que devem se comunicar umas com as outras ou processar dados à medida que são disponibilizados.|  
|[Usando TPL com outros padrões assíncronos](using-tpl-with-other-asynchronous-patterns.md)|Descreve como usar a TPL com outros padrões assíncronos em .NET|  
|[Armadilhas em potencial em dados e paralelismo da tarefa](potential-pitfalls-in-data-and-task-parallelism.md)|Descreve algumas armadilhas comuns e como evitá-las.|  
|[LINQ paralelo (PLINQ)](introduction-to-plinq.md)|Descreve como obter o paralelismo de dados com consultas LINQ.|  
|[Programação paralela](index.md)|Nó de nível superior para a programação paralela do .NET.|  
  
## <a name="see-also"></a>Veja também

- [Exemplos de programação paralela com o & do .NET Core .NET Standard](/samples/browse/?products=dotnet-core%2Cdotnet-standard&term=parallel)
