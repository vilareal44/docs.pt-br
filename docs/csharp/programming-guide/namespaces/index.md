---
title: Namespaces – Guia de Programação em C#
description: Saiba mais sobre namespaces em programação em C#. Consulte uma visão geral das propriedades do namespace e exiba recursos adicionais.
ms.date: 08/21/2018
helpviewer_keywords:
- C# language, namespaces
- namespaces [C#]
ms.assetid: b1c4ab46-3fad-4ffa-9deb-dd50a2d8c65a
ms.openlocfilehash: fca2c641520bd9cd19a48bff2119a6f09c3713ea
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382094"
---
# <a name="namespaces-c-programming-guide"></a>Namespaces (Guia de Programação em C#)

Os namespaces são usados intensamente em programações de C# de duas maneiras. Primeiro, o .NET usa namespaces para organizar suas várias classes, da seguinte maneira:  

[!code-csharp[csProgGuide#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#22)]

<xref:System> é um namespace e <xref:System.Console> é uma classe nesse namespace. A palavra-chave `using` pode ser usada para que o nome completo não seja necessário, como no exemplo a seguir:

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#25)]

Para saber mais, confira [Diretiva using](../../language-reference/keywords/using-directive.md).

Em segundo lugar, declarar seus próprios namespaces pode ajudar a controlar o escopo dos nomes de classe e de método em projetos de programação maiores. Use a palavra-chave [namespace](../../language-reference/keywords/namespace.md) para declarar um namespace, como no exemplo a seguir:

[!code-csharp[csProgGuideNamespaces#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#6)]

O nome do namespace deve ser um [nome do identificador](../inside-a-program/identifier-names.md) válido em C#.

## <a name="namespaces-overview"></a>Visão geral dos namespaces

Os namespaces têm as seguintes propriedades:

- Eles organizam projetos de códigos grandes.
- Eles são delimitados usando o operador `.`.
- A diretiva `using` elimina a necessidade de especificar o nome do namespace para cada classe.
- O namespace `global` é o namespace "raiz": `global::System` sempre fará referência ao namespace do .NET <xref:System>.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [Namespaces](~/_csharplang/spec/namespaces.md) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Usando namespaces](using-namespaces.md)
- [Como usar o My Namespace](how-to-use-the-my-namespace.md)
- [Nomes de identificadores](../inside-a-program/identifier-names.md)
- [Diretiva de uso](../../language-reference/keywords/using-directive.md)
- [Operador::](../../language-reference/operators/namespace-alias-qualifier.md)
