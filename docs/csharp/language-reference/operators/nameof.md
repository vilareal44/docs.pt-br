---
title: expressão nameof-referência C#
ms.date: 04/23/2020
f1_keywords:
- nameof_CSharpKeyword
- nameof
helpviewer_keywords:
- nameof expression [C#]
ms.assetid: 33601bf3-cc2c-4496-846d-f9679bccf2a7
ms.openlocfilehash: b00c5f6f97d27290fb3773dcbb422bf9fb4c425b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916771"
---
# <a name="nameof-expression-c-reference"></a>expressão nameof (referência C#)

Uma `nameof` expressão produz o nome de uma variável, tipo ou membro como a constante de cadeia de caracteres:

[!code-csharp-interactive[nameof expression](snippets/shared/NameOfOperator.cs#Examples)]

Como mostra o exemplo anterior, no caso de um tipo e um namespace, o nome produzido geralmente não é [totalmente qualificado](~/_csharplang/spec/basic-concepts.md#fully-qualified-names).

No caso de [identificadores textuais](../tokens/verbatim.md), o `@` caractere não é a parte de um nome, como mostra o exemplo a seguir:

[!code-csharp-interactive[nameof verbatim](snippets/shared/NameOfOperator.cs#Verbatim)]

Uma `nameof` expressão é avaliada em tempo de compilação e não tem nenhum efeito no tempo de execução.

Você pode usar uma `nameof` expressão para tornar o código de verificação de argumento mais passível de manutenção:

[!code-csharp[nameof and argument check](snippets/shared/NameOfOperator.cs#ExceptionMessage)]

Uma `nameof` expressão está disponível no C# 6 e posterior.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [Expressões nameof](~/_csharplang/spec/expressions.md#nameof-expressions) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
