---
title: Sobrecargas
ms.date: 07/20/2015
f1_keywords:
- vb.Overloads
- Overloads
helpviewer_keywords:
- Overloads keyword [Visual Basic]
- hiding by signature
- Shadows keyword [Visual Basic]
- signature, hiding by
ms.assetid: 0c6820b8-25b2-4664-bc59-5ca93c99c042
ms.openlocfilehash: bd0931cab520f8580c0d7473a44e350752e287bb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392100"
---
# <a name="overloads-visual-basic"></a>Sobrecargas (Visual Basic)

Especifica que uma propriedade ou procedimento redeclara uma ou mais propriedades ou procedimentos existentes com o mesmo nome.

## <a name="remarks"></a>Comentários

O *sobrecarregamento* é a prática de fornecer mais de uma definição para uma determinada propriedade ou nome de procedimento no mesmo escopo. Redeclarar uma propriedade ou um procedimento com uma assinatura diferente às vezes é chamado de *ocultar por assinatura*.

## <a name="rules"></a>Regras

- **Contexto de declaração.** Você pode usar `Overloads` somente em uma instrução de declaração de propriedade ou de procedimento.

- **Modificadores combinados.** Você não pode especificar `Overloads` junto com [sombras](shadows.md) na mesma declaração de procedimento.

- **Diferenças necessárias.** A *assinatura* nessa declaração deve ser diferente da assinatura de cada propriedade ou procedimento que ela sobrecarrega. A assinatura inclui o nome da propriedade ou do procedimento junto com o seguinte:

  - o número de parâmetros

  - a ordem dos parâmetros

  - os tipos de dados dos parâmetros

  - o número de parâmetros de tipo (para um procedimento genérico)

  - o tipo de retorno (somente para um procedimento de operador de conversão)

  Todas as sobrecargas devem ter o mesmo nome, mas cada uma delas deve ser diferente de todos os outros em um ou mais dos aspectos anteriores. Isso permite que o compilador diferencie qual versão usar quando o código chama a propriedade ou o procedimento.

- **Diferenças não permitidas.** A alteração de um ou mais dos itens a seguir não é válida para sobrecarregar uma propriedade ou um procedimento, pois eles não fazem parte da assinatura:

  - Se ele retorna ou não um valor (para um procedimento)

  - o tipo de dados do valor de retorno (exceto para um operador de conversão)

  - os nomes dos parâmetros ou parâmetros de tipo

  - as restrições nos parâmetros de tipo (para um procedimento genérico)

  - Palavras-chave do modificador de parâmetro (como `ByRef` ou `Optional` )

  - Palavras-chave do modificador de propriedade ou procedimento (como `Public` ou `Shared` )

- **Modificador opcional.** Você não precisa usar o `Overloads` modificador ao definir várias propriedades ou procedimentos sobrecarregados na mesma classe. No entanto, se você usar `Overloads` o em uma das declarações, deverá usá-lo em todos eles.

- **Sombreamento e sobrecarga.** `Overloads`também pode ser usado para sombrear um membro existente ou um conjunto de Membros sobrecarregados, em uma classe base. Ao usar `Overloads` dessa forma, você declara a propriedade ou o método com o mesmo nome e a mesma lista de parâmetros que o membro da classe base e não fornece a `Shadows` palavra-chave.

Se você usar `Overrides` o, o compilador adicionará implicitamente `Overloads` para que suas APIs de biblioteca trabalhem com o C# mais facilmente.

O `Overloads` modificador pode ser usado nesses contextos:

- [Instrução Function](../statements/function-statement.md)

- [Instrução Operator](../statements/operator-statement.md)

- [Instrução Property](../statements/property-statement.md)

- [Instrução Sub](../statements/sub-statement.md)

## <a name="see-also"></a>Confira também

- [Sombras](shadows.md)
- [Sobrecarga de procedimento](../../programming-guide/language-features/procedures/procedure-overloading.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Procedimentos do operador](../../programming-guide/language-features/procedures/operator-procedures.md)
- [Como definir um operador de conversão](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
