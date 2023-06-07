---
title: operador delegate – referência do C#
ms.date: 07/18/2019
helpviewer_keywords:
- delegate [C#]
- anonymous method [C#]
ms.openlocfilehash: 33c438b88f1e993f4648786da9b20b91961b7ee1
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062984"
---
# <a name="delegate-operator-c-reference"></a>operador delegate (referência do C#)

O operador `delegate` cria um método anônimo que pode ser convertido em um tipo delegado:

[!code-csharp-interactive[anonymous method](snippets/shared/DelegateOperator.cs#AnonymousMethod)]

> [!NOTE]
> Começando com o C# 3, as expressões lambda fornecem uma maneira mais concisa e expressiva de criar uma função anônima. Use o [operador =>](lambda-operator.md) para construir uma expressão lambda:
>
> [!code-csharp-interactive[lambda expression](snippets/shared/DelegateOperator.cs#Lambda)]
>
> Para saber mais sobre recursos de expressões lambda, por exemplo, que capturam variáveis externas, confira [Expressões lambda](lambda-expressions.md).

Você pode omitir a lista de parâmetros quando usa o operador `delegate`. Se você fizer isso, o método anônimo criado poderá ser convertido em um tipo delegado com qualquer lista de parâmetros, como mostra o exemplo a seguir:

[!code-csharp-interactive[no parameter list](snippets/shared/DelegateOperator.cs#WithoutParameterList)]

Essa é a única funcionalidade de métodos anônimos que não tem suporte por expressões lambda. Em todos os outros casos, uma expressão lambda é a forma preferida de gravar código embutido.

Você também usa a palavra-chave `delegate` para declarar um [tipo delegado](../builtin-types/reference-types.md#the-delegate-type).

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, confira a seção [Expressões de função anônima](~/_csharplang/spec/expressions.md#anonymous-function-expressions) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [= Operador de>](lambda-operator.md)
