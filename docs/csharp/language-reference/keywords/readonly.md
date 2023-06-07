---
title: Palavra-chave readonly – Referência de C#
ms.date: 04/14/2020
f1_keywords:
- readonly_CSharpKeyword
- readonly
helpviewer_keywords:
- readonly keyword [C#]
ms.assetid: 2f8081f6-0de2-4903-898d-99696c48d2f4
ms.openlocfilehash: 66a096e8831f72a2216e8ba5dd9866046504624f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84368615"
---
# <a name="readonly-c-reference"></a>readonly (Referência de C#)

A `readonly` palavra-chave é um modificador que pode ser usado em quatro contextos:

- Em uma [declaração de campo](#readonly-field-example), `readonly` indica que a atribuição para o campo só pode ocorrer como parte da declaração ou em um construtor na mesma classe. Um campo readonly pode ser atribuído e reatribuído várias vezes na declaração de campo e no construtor.
  
  Um `readonly` campo não pode ser atribuído depois que o construtor é encerrado. Essa regra tem implicações diferentes para tipos de valor e tipos de referência:
  
  - Como os tipos de valor contêm diretamente seus dados, um campo que é um tipo de valor `readonly` é imutável.
  - Como os tipos de referência contêm uma referência a seus dados, um campo que é um tipo de referência `readonly` deve sempre se referir ao mesmo objeto. Esse objeto não é imutável. O modificador `readonly` impede que o campo seja substituído por uma instância diferente do tipo de referência. No entanto, o modificador não impede que os dados da instância do campo sejam modificados por meio do campo somente leitura.

  > [!WARNING]
  > Um tipo visível externamente que contém um campo somente leitura visível externamente, que é um tipo de referência mutável, pode ser uma vulnerabilidade de segurança e pode disparar o aviso [CA2104](/visualstudio/code-quality/ca2104) : "Não declare tipos de referência mutáveis somente leitura".

- Em uma `readonly struct` definição de tipo, `readonly` indica que o tipo de estrutura é imutável. Para obter mais informações, consulte a seção [ `readonly` struct](../builtin-types/struct.md#readonly-struct) do artigo [tipos de estrutura](../builtin-types/struct.md) .
- Em uma declaração de membro de instância dentro de um tipo de estrutura, `readonly` indica que um membro de instância não modifica o estado da estrutura. Para obter mais informações, consulte a seção [ `readonly` membros da instância](../builtin-types/struct.md#readonly-instance-members) do artigo [tipos de estrutura](../builtin-types/struct.md) .
- Em um [ `ref readonly` retorno de método](#ref-readonly-return-example), o `readonly` modificador indica que o método retorna uma referência e as gravações não são permitidas para essa referência.

Os `readonly struct` `ref readonly` contextos e foram adicionados em C# 7,2. `readonly`Membros de struct foram adicionados em C# 8,0

## <a name="readonly-field-example"></a>Exemplo de campo readonly

Neste exemplo, o valor do campo `year` não pode ser alterado no método `ChangeYear` , mesmo que ele tenha sido atribuído a um valor no construtor da classe:

[!code-csharp[Readonly Field example](snippets/ReadonlyKeywordExamples.cs#ReadonlyField)]

Você pode atribuir um valor a um campo `readonly` apenas nos seguintes contextos:

- Quando a variável é inicializada na declaração, por exemplo:

  ```csharp
  public readonly int y = 5;
  ```

- Em um construtor de instância da classe que contém a declaração de campo de instância.
- No construtor estático da classe que contém a declaração do campo estático.

Esses contextos de Construtor também são os únicos contextos em que é válido passar um `readonly` campo como um parâmetro [out](out-parameter-modifier.md) ou [ref](ref.md) .

> [!NOTE]
> A palavra-chave `readonly` é diferente da palavra-chave [const](const.md). O campo `const` pode ser inicializado apenas na declaração do campo. Um campo `readonly` pode ser atribuído várias vezes na declaração do campo e em qualquer construtor. Portanto, campos `readonly` podem ter valores diferentes dependendo do construtor usado. Além disso, embora um `const` campo seja uma constante de tempo de compilação, o `readonly` campo pode ser usado para constantes de tempo de execução como no exemplo a seguir:
>
> ```csharp
> public static readonly uint timeStamp = (uint)DateTime.Now.Ticks;
> ```

[!code-csharp[Initialize readonly Field example](snippets/ReadonlyKeywordExamples.cs#InitReadonlyField)]

No exemplo anterior, se você usar uma instrução semelhante ao seguinte exemplo:

```csharp
p2.y = 66;        // Error
```

Você obterá a mensagem de erro do compilador:

**Um campo ReadOnly não pode ser atribuído (exceto em um construtor ou inicializador de variável)**

## <a name="ref-readonly-return-example"></a>Exemplo de retorno de readonly de ref

O `readonly` modificador em um `ref return` indica que a referência retornada não pode ser modificada. O exemplo a seguir retorna uma referência para a origem. Ele usa o `readonly` modificador para indicar que os chamadores não podem modificar a origem:

[!code-csharp[readonly return example](snippets/ReadonlyKeywordExamples.cs#ReadonlyReturn)]

O tipo retornado não precisa ser um `readonly struct`. Qualquer tipo que possa ser retornado por `ref` pode ser retornado por `ref readonly`.

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

Você também pode ver as propostas de especificação de idioma:

- [struct de ref e ReadOnly ReadOnly](~/_csharplang/proposals/csharp-7.2/readonly-ref.md)
- [Membros de struct ReadOnly](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)

## <a name="see-also"></a>Confira também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Modificadores](index.md)
- [const](const.md)
- [Fields](../../programming-guide/classes-and-structs/fields.md)
