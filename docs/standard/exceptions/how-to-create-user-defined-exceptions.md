---
title: Como criar exceções definidas pelo usuário
description: Saiba como criar exceções definidas pelo usuário, que são uma alternativa à hierarquia de classes de exceção derivadas da classe base de exceção no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- user-defined exceptions
- exceptions, examples
- exceptions, user-defined
ms.assetid: 25819a5a-f915-4fc8-b924-a76915674e04
ms.openlocfilehash: 14eb6246ba4347f33766f7dff36463f2bf996330
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662791"
---
# <a name="how-to-create-user-defined-exceptions"></a>Como criar exceções definidas pelo usuário

O .NET fornece uma hierarquia de classes de exceção derivada, em última análise, da classe base <xref:System.Exception>. No entanto, se nenhuma das exceções predefinidas atender às suas necessidades, será possível criar suas próprias classes de exceção derivando da classe <xref:System.Exception>.

Ao criar suas próprias exceções, encerre o nome de classe de exceção definida pelo usuário com a palavra "Exception" e implemente os três construtores comuns, como mostrado no exemplo a seguir. O exemplo define uma nova classe de exceção chamada `EmployeeListNotFoundException`. A classe é derivada de <xref:System.Exception> e inclui três construtores.

[!code-cpp[dg_exceptionDesign#14](../../../samples/snippets/cpp/VS_Snippets_CLR/dg_exceptionDesign/cpp/example2.cpp#14)]
[!code-csharp[dg_exceptionDesign#14](../../../samples/snippets/csharp/VS_Snippets_CLR/dg_exceptionDesign/cs/example2.cs#14)]
[!code-vb[dg_exceptionDesign#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/dg_exceptionDesign/vb/example2.vb#14)]  

> [!NOTE]
> Em situações nas quais você está usando a comunicação remota, você deve garantir que os metadados para todas as exceções definidas pelo usuário estejam disponíveis no servidor (computador chamado) e para o cliente (o objeto de proxy ou chamador). Para obter mais informações, consulte [Práticas recomendadas para exceções](best-practices-for-exceptions.md).

## <a name="see-also"></a>Confira também

- [Exceções](index.md)
