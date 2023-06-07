---
title: Exceções geradas pelo compilador – Guia de Programação em C#
description: Saiba mais sobre as exceções geradas pelo compilador. Examine uma lista de exceções geradas automaticamente e suas condições de erro.
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions [C#], compiler-generated
ms.assetid: 53b52f97-b366-4ed7-b05b-9eb78096b7f9
ms.openlocfilehash: 1def83f72e83976ac672ec35169b4950a20ef54e
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302055"
---
# <a name="compiler-generated-exceptions-c-programming-guide"></a>Exceções geradas pelo compilador (Guia de Programação em C#)

Algumas exceções são geradas automaticamente pelo tempo de execução do .NET quando operações básicas falham. Essas exceções e suas condições de erro são listadas na tabela a seguir.  
  
|Exceção|Descrição|  
|---------------|-----------------|  
|<xref:System.ArithmeticException>|Uma classe base para exceções que ocorrem durante operações aritméticas, tais como <xref:System.DivideByZeroException> e <xref:System.OverflowException>.|  
|<xref:System.ArrayTypeMismatchException>|Gerada quando uma matriz não pode armazenar um determinado elemento porque o tipo real do elemento é incompatível com o tipo real da matriz.|  
|<xref:System.DivideByZeroException>|Lançada quando é feita uma tentativa de dividir um valor inteiro por zero.|  
|<xref:System.IndexOutOfRangeException>|Lançada quando é feita uma tentativa de indexar uma matriz quando o índice é menor que zero ou fora dos limites da matriz.|  
|<xref:System.InvalidCastException>|Gerada quando uma conversão explícita de um tipo base para uma interface ou um tipo derivado falha em runtime.|  
|<xref:System.NullReferenceException>|Gerado quando é feita uma tentativa de fazer referência a um objeto cujo valor é [nulo](../../language-reference/keywords/null.md).|  
|<xref:System.OutOfMemoryException>|Lançada quando uma tentativa de alocar memória usando o operador [new](../../language-reference/operators/new-operator.md) falha. Isso indica que a memória disponível para o Common Language Runtime está esgotada.|  
|<xref:System.OverflowException>|Lançada quando uma operação aritmética em um contexto `checked` estoura.|  
|<xref:System.StackOverflowException>|Lançada quando a pilha de execução acaba tendo muitas chamadas de método pendentes, normalmente indica uma recursão muito profunda ou infinita.|  
|<xref:System.TypeInitializationException>|Lançada quando um construtor estático lança uma exceção e não há nenhuma cláusula `catch` compatível para capturá-la.|  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Exceções e tratamento de exceções](./index.md)
- [Tratamento de exceção](./exception-handling.md)
- [try-catch](../../language-reference/keywords/try-catch.md)
- [try-finally](../../language-reference/keywords/try-finally.md)
- [try-catch-finally](../../language-reference/keywords/try-catch-finally.md)
