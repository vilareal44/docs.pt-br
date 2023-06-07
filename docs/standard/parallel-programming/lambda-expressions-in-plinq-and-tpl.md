---
title: Expressões lambda em PLINQ e TPL
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Func delegate, creating with lambda expression
- Action delegate, creating with lambda expression
- lambda expressions, with Action and Func
ms.assetid: 645b2c17-29d0-4ffa-8684-430743cc2f2d
ms.openlocfilehash: 469c164630e1dab84b3d54c16c43d031ebf560ed
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063764"
---
# <a name="lambda-expressions-in-plinq-and-tpl"></a>Expressões lambda em PLINQ e TPL

A TPL (Biblioteca de Paralelismo de Tarefas) contém vários métodos que usam uma das família de delegados <xref:System.Func%601?displayProperty=nameWithType> ou <xref:System.Action?displayProperty=nameWithType> como parâmetros de entrada. Você pode usar esses delegados para transmitir sua lógica de programa personalizada para o loop paralelo, tarefa ou consulta. Os exemplos de código para TPL, bem como o PLINQ, usam expressões lambda para criar instâncias desses delegados como blocos de código embutido. Este tópico fornece uma breve introdução a Func e Action, e mostra como usar expressões lambda no PLINQ e na Biblioteca de paralelismo de tarefas.

> [!NOTE]
> Para obter mais informações sobre delegados em geral, consulte [delegados](../../csharp/programming-guide/delegates/index.md) e [delegados](../../visual-basic/programming-guide/language-features/delegates/index.md). Para saber mais sobre expressões lambda em C# e em Visual Basic, confira [Expressões lambda](../../csharp/language-reference/operators/lambda-expressions.md) e [Expressões lambda](../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md).

## <a name="func-delegate"></a>Delegado Func

Um delegado `Func` encapsula um método que retorna um valor. Em uma `Func` assinatura, o parâmetro de tipo Last ou mais à direita sempre especifica o tipo de retorno. Uma causa comum de erros do compilador é tentar passar dois parâmetros de entrada para um <xref:System.Func%602?displayProperty=nameWithType>; na verdade, esse tipo usa somente um parâmetro de entrada. O .NET define 17 versões de `Func` : <xref:System.Func%601?displayProperty=nameWithType> ,, <xref:System.Func%602?displayProperty=nameWithType> <xref:System.Func%603?displayProperty=nameWithType> e assim por diante <xref:System.Func%6017?displayProperty=nameWithType> .

## <a name="action-delegate"></a>Delegado Action

Um <xref:System.Action?displayProperty=nameWithType> delegado encapsula um método (sub no Visual Basic) que não retorna um valor. Em uma `Action` assinatura de tipo, os parâmetros de tipo representam apenas os parâmetros de entrada. Como `Func` o, o .NET define 17 versões do `Action` , de uma versão que não tem parâmetros de tipo por meio de uma versão que tem 16 parâmetros de tipo.

## <a name="example"></a>Exemplo

O exemplo a seguir para o método <xref:System.Threading.Tasks.Parallel.ForEach%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%601%7D%2CSystem.Func%7B%60%600%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%601%2C%60%601%7D%2CSystem.Action%7B%60%601%7D%29?displayProperty=nameWithType> mostra como expressar delegados Func e Action usando expressões lambda.

[!code-csharp[System.Threading.Tasks.Parallel#02](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.threading.tasks.parallel/cs/parallelforeach.cs#02)]
[!code-vb[System.Threading.Tasks.Parallel#02](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.threading.tasks.parallel/vb/parallelforeach.vb#02)]

## <a name="see-also"></a>Consulte também

- [Programação paralela](index.md)
