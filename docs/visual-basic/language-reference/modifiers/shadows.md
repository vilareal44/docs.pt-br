---
title: Sombras
ms.date: 07/20/2015
f1_keywords:
- vb.Shadows
- shadows
helpviewer_keywords:
- shadowing
- duplicate names [Visual Basic]
- scope [Visual Basic], shadowing
- Shadows keyword [Visual Basic]
- names [Visual Basic], shadowing
ms.assetid: 6bf687cd-0544-4797-b51b-911125ec57c6
ms.openlocfilehash: 7aed6bec21bd484cca019b061bd5915de13a9eb8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402701"
---
# <a name="shadows-visual-basic"></a>Sombras (Visual Basic)

Especifica que um elemento de programação declarado redeclara e oculta um elemento de nome idêntico ou conjunto de elementos sobrecarregados, em uma classe base.

## <a name="remarks"></a>Comentários

A principal finalidade do sombreamento (que também é conhecido como *ocultar por nome*) é preservar a definição de seus membros de classe. A classe base pode passar por uma alteração que cria um elemento com o mesmo nome de um que você já definiu. Se isso acontecer, o `Shadows` modificador força referências por meio de sua classe a ser resolvida para o membro que você definiu, em vez de para o novo elemento de classe base.

Sombreamento e substituição redefinem um elemento herdado, mas há diferenças significativas entre as duas abordagens. Para obter mais informações, consulte [sombreamento em Visual Basic](../../programming-guide/language-features/declared-elements/shadowing.md).

## <a name="rules"></a>Regras

- **Contexto de declaração.** Você pode usar `Shadows` somente no nível de classe. Isso significa que o contexto de declaração para um `Shadows` elemento deve ser uma classe e não pode ser um arquivo de origem, namespace, interface, módulo, estrutura ou procedimento.

  Você pode declarar apenas um elemento de sombreamento em uma única instrução de declaração.

- **Modificadores combinados.** Você não pode especificar `Shadows` juntos com `Overloads` , `Overrides` ou `Static` na mesma declaração.

- **Tipos de elemento.** Você pode sombrear qualquer tipo de elemento declarado com qualquer outro tipo. Se você sombrear uma propriedade ou um procedimento com outra propriedade ou procedimento, os parâmetros e o tipo de retorno não precisarão corresponder àqueles na propriedade ou procedimento da classe base.

- **Acess.** O elemento sombreado na classe base normalmente não está disponível de dentro da classe derivada que a sombreia. No entanto, as considerações a seguir se aplicam.

  - Se o elemento de sombreamento não estiver acessível a partir do código que se refere a ele, a referência será resolvida para o elemento sombreado. Por exemplo, se um `Private` elemento sombreia um elemento de classe base, o código que não tem permissão para acessar `Private` o elemento acessa o elemento de classe base em vez disso.

  - Se você sombrear um elemento, ainda poderá acessar o elemento sombreado por meio de um objeto declarado com o tipo da classe base. Você também pode acessá-lo por meio do `MyBase` .

O `Shadows` modificador pode ser usado nesses contextos:

- [Instrução Class](../statements/class-statement.md)

- [Instrução Const](../statements/const-statement.md)

- [Instrução Declare](../statements/declare-statement.md)

- [Instrução Delegate](../statements/delegate-statement.md)

- [Instrução Dim](../statements/dim-statement.md)

- [Instrução Enum](../statements/enum-statement.md)

- [Instrução Event](../statements/event-statement.md)

- [Instrução Function](../statements/function-statement.md)

- [Instrução Interface](../statements/interface-statement.md)

- [Instrução Property](../statements/property-statement.md)

- [Instrução Structure](../statements/structure-statement.md)

- [Instrução Sub](../statements/sub-statement.md)

## <a name="see-also"></a>Confira também

- [Compartilhado](shared.md)
- [Estático](static.md)
- [Privada](private.md)
- [Me, My, MyBase e MyClass](../../programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [MustOverride](mustoverride.md)
- [NotOverridable](notoverridable.md)
- [Sobrecargas](overloads.md)
- [Substituível](overridable.md)
- [Substituições](overrides.md)
- [Sombreamento no Visual Basic](../../programming-guide/language-features/declared-elements/shadowing.md)
