---
title: 'Como: criar e executar uma consulta PLINQ simples'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to create
ms.assetid: 983b4213-bddd-4a44-9262-cbe59186df4c
ms.openlocfilehash: a9c044254423d0f9d266539c728a6604f562e97d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290000"
---
# <a name="how-to-create-and-execute-a-simple-plinq-query"></a>Como: criar e executar uma consulta PLINQ simples

O exemplo neste artigo mostra como criar uma consulta LINQ (consulta integrada de linguagem paralela) simples usando o método de <xref:System.Linq.ParallelEnumerable.AsParallel%2A?displayProperty=nameWithType> extensão na sequência de origem e executando a consulta usando o <xref:System.Linq.ParallelEnumerable.ForAll%2A?displayProperty=nameWithTyp> método.  
  
> [!NOTE]
> Esta documentação usa expressões lambda para definir representantes na PLINQ. Se você não estiver familiarizado com expressões lambda em C# ou Visual Basic, consulte [expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[PLINQ#11](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/create1.cs#11)]
 [!code-vb[PLINQ#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/create1.vb#11)]  
  
 Este exemplo demonstra o padrão básico para criar e executar qualquer consulta LINQ paralela quando a ordenação da sequência de resultados não é importante. As consultas não ordenadas são geralmente mais rápidas do que as consultas ordenadas. A consulta particiona a origem em tarefas executadas de maneira assíncrona em vários threads. A ordem em que cada tarefa é concluída depende não apenas da quantidade de trabalho envolvido para processar os elementos na partição, mas também de fatores externos como a maneira que o sistema operacional agenda cada thread. Este exemplo tem como objetivo demonstrar o uso e pode não executar tão rápido quanto a consulta LINQ to Objects sequencial equivalente. Para saber mais sobre agilização, confira [Noções básicas sobre agilização no PLINQ](understanding-speedup-in-plinq.md). Para saber mais sobre como preservar a ordenação de elementos em uma consulta, consulte [Como controlar a ordem em uma consulta PLINQ](how-to-control-ordering-in-a-plinq-query.md).  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
