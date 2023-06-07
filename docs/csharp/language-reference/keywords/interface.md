---
title: interface – Referência de C#
ms.date: 01/17/2020
f1_keywords:
- interface_CSharpKeyword
helpviewer_keywords:
- interface keyword [C#]
ms.assetid: 7da38e81-4f99-4bc5-b07d-c986b687eeba
ms.openlocfilehash: 869f1398ae0af3c7379655aa018a9f4aacb934d7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243965"
---
# <a name="no-loc-textinterface-c-reference"></a>:::no-loc text="interface":::(Referência C#)

Uma interface define um contrato. Qualquer um [`class`](class.md) ou [`struct`](../builtin-types/struct.md) que implemente esse contrato deve fornecer uma implementação dos membros definidos na interface. A partir do C# 8,0, uma interface pode definir uma implementação padrão para membros. Ele também pode definir [`static`](static.md) Membros para fornecer uma única implementação para a funcionalidade comum.

No exemplo a seguir, a classe `ImplementationClass` deve implementar um método chamado `SampleMethod` que não tem parâmetros e retorna `void`.

Para obter mais informações e exemplos, consulte [Interfaces](../../programming-guide/interfaces/index.md).

## <a name="example"></a>Exemplo

[!code-csharp[csrefKeywordsTypes#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#14)]

Uma interface pode ser membro de um namespace ou de uma classe. Uma declaração de interface pode conter declarações (assinaturas sem qualquer implementação) dos seguintes membros:

- [Métodos](../../programming-guide/classes-and-structs/methods.md)
- [Propriedades](../../programming-guide/classes-and-structs/using-properties.md)
- [Indexadores](../../programming-guide/indexers/using-indexers.md)
- [Eventos](event.md)

Essas declarações de membro anteriores normalmente não contêm um corpo. A partir do C# 8,0, um membro de interface pode declarar um corpo. Isso é chamado de *implementação padrão*. Os membros com corpos permitem que a interface forneça uma implementação "padrão" para classes e estruturas que não fornecem uma implementação de substituição. Além disso, a partir do C# 8,0, uma interface pode incluir:

- [Constantes](const.md)
- [Operadores](../operators/operator-overloading.md)
- [Construtor estático](../../programming-guide/classes-and-structs/constructors.md#static-constructors).
- [Tipos aninhados](../../programming-guide/classes-and-structs/nested-types.md)
- [Campos, métodos, propriedades, indexadores e eventos estáticos](static.md)
- Declarações de membro usando a sintaxe de implementação de interface explícita.
- Modificadores de acesso explícitos (o acesso padrão é [`public`](access-modifiers.md) ).

As interfaces não podem conter o estado da instância. Embora os campos estáticos agora sejam permitidos, os campos de instância não são permitidos em interfaces. Não há suporte para [Propriedades automáticas de instância](../../programming-guide/classes-and-structs/auto-implemented-properties.md) em interfaces, pois elas declarariam implicitamente um campo oculto. Essa regra tem um efeito sutil nas declarações de propriedade. Em uma declaração de interface, o código a seguir não declara uma propriedade implementada automaticamente como faz em um `class` ou `struct` . Em vez disso, ele declara uma propriedade que não tem uma implementação padrão, mas que deve ser implementada em qualquer tipo que implemente a interface:

```csharp
public interface INamed
{
  public string Name {get; set;}
}
```

Uma interface pode herdar de uma ou mais interfaces base. Quando uma interface [substitui um método](override.md) implementado em uma interface base, ela deve usar a sintaxe de [implementação de interface explícita](../../programming-guide/interfaces/explicit-interface-implementation.md) .

Quando uma lista de tipos base contém uma classe base e interfaces, a classe base deve vir em primeiro na lista.

Uma classe que implementa uma interface pode implementar membros dessa interface explicitamente. Um membro implementado explicitamente não pode ser acessado por meio de uma instância da classe, mas apenas por meio de uma instância da interface. Além disso, os membros de interface padrão só podem ser acessados por meio de uma instância da interface.

Para obter mais informações sobre implementação de interface explícita, consulte [implementação de interface explícita](../../programming-guide/interfaces/explicit-interface-implementation.md).

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra a implementação da interface. Neste exemplo, a interface contém a declaração de propriedade e a classe contém a implementação. Qualquer instância de uma classe que implementa `IPoint` tem propriedades de inteiro `x` e `y`.

[!code-csharp[csrefKeywordsTypes#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#15)]

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte a seção [interfaces](~/_csharplang/spec/interfaces.md) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md) e a especificação de recurso para [membros de interface padrão-C# 8,0](~/_csharplang/proposals/csharp-8.0/default-interface-methods.md)

## <a name="see-also"></a>Veja também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Tipos de referência](reference-types.md)
- [Interfaces](../../programming-guide/interfaces/index.md)
- [Usando propriedades](../../programming-guide/classes-and-structs/using-properties.md)
- [Usando indexadores](../../programming-guide/indexers/using-indexers.md)
