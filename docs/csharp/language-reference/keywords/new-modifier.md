---
title: Modificador new – Referência em C#
ms.date: 07/20/2015
helpviewer_keywords:
- new modifier keyword [C#]
ms.assetid: a2e20856-33b9-4620-b535-a60dbce8349b
ms.openlocfilehash: 529d77b0a66a8a3cedfe7a400af0fb34dd653322
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82795164"
---
# <a name="new-modifier-c-reference"></a>Modificador new (referência em C#)

Quando usada como um modificador de declaração, a palavra-chave `new` oculta explicitamente um membro herdado de uma classe base. Quando você oculta um membro herdado, a versão derivada do membro substitui a versão da classe base. Embora possa ocultar membros sem usar o modificador `new`, você obtém um aviso do compilador. Se você usar `new` para ocultar explicitamente um membro, ele suprime o aviso.

Você também pode usar a palavra-chave `new` para [criar uma instância de um tipo](../operators/new-operator.md) ou como uma [restrição de tipo genérico](./new-constraint.md).

Para ocultar um membro herdado, declare-o na classe derivada usando o mesmo nome de membro e modifique-o com a palavra-chave `new`. Por exemplo:

[!code-csharp[csrefKeywordsOperator#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#8)]

Neste exemplo, `BaseC.Invoke` é ocultado por `DerivedC.Invoke`. O campo `x` não é afetado porque não é ocultado por um nome semelhante.

A ocultação de nome por meio da herança assume uma das seguintes formas:

- Normalmente, uma constante, campo, propriedade ou tipo que é apresentado em uma classe ou struct oculta todos os membros da classe base que compartilham seu nome. Há casos especiais. Por exemplo, se você declarar que um novo campo com o nome `N` tem um tipo que não pode ser invocado e um tipo base declarar que `N` é um método, o novo campo não ocultará a declaração base na sintaxe de invocação. Para obter mais informações, consulte a seção [Pesquisa de membro](~/_csharplang/spec/expressions.md#member-lookup) na [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

- Um método introduzido em uma classe ou struct oculta propriedades, campos e tipos que compartilham esse nome na classe base. Ele também oculta todos os métodos da classe base que têm a mesma assinatura.

- Um indexador introduzido em uma classe ou struct oculta todos os indexadores de classe base que têm a mesma assinatura.

É um erro usar `new` e [override](override.md) no mesmo membro, porque os dois modificadores têm significados mutuamente exclusivos. O modificador `new` cria um novo membro com o mesmo nome e faz com que o membro original seja ocultado. O modificador `override` estende a implementação de um membro herdado.

Usar o modificador `new` em uma declaração que não oculta um membro herdado gera um aviso.

## <a name="example"></a>Exemplo

Neste exemplo, uma classe base, `BaseC` e uma classe derivada, `DerivedC`, usam o mesmo nome do campo `x`, que oculta o valor do campo herdado. O exemplo demonstra o uso do modificador `new`. Ele também demonstra como acessar os membros ocultos da classe base usando seus nomes totalmente qualificados.

[!code-csharp[csrefKeywordsOperator#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#9)]

## <a name="example"></a>Exemplo

Neste exemplo, uma classe aninhada oculta uma classe que tem o mesmo nome na classe base. O exemplo demonstra como usar o modificador `new` para eliminar a mensagem de aviso e como acessar os membros da classe oculta usando seus nomes totalmente qualificados.

[!code-csharp[csrefKeywordsOperator#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#10)]

Se você remover o modificador `new`, o programa ainda será compilado e executado, mas você receberá o seguinte aviso:

```text
The keyword new is required on 'MyDerivedC.x' because it hides inherited member 'MyBaseC.x'.
```

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte a seção [O modificador new](~/_csharplang/spec/classes.md#the-new-modifier) na [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Veja também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Modificadores](index.md)
- [Controle de versão com as palavras-chave override e new](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)
- [Quando usar as palavras-chave override e new](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)
