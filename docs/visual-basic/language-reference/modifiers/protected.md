---
title: Protegido
ms.date: 07/20/2015
f1_keywords:
- vb.Protected
helpviewer_keywords:
- Protected Friend keyword combination
- Protected keyword [Visual Basic], and Friend
- Protected keyword [Visual Basic], syntax
- Protected access modifier
- Protected keyword [Visual Basic]
ms.assetid: 74ad3d56-309f-49d2-b60c-1d0157d010e8
ms.openlocfilehash: d66ed68004f8b6ef21ae703f02b317589814764b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398214"
---
# <a name="protected-visual-basic"></a>Protegido (Visual Basic)

Um modificador de acesso de membro que especifica que um ou mais elementos de programação declarados são acessíveis somente de dentro de sua própria classe ou de uma classe derivada.

## <a name="remarks"></a>Comentários

Às vezes, um elemento de programação declarado em uma classe contém dados confidenciais ou código restrito e você deseja limitar o acesso ao elemento. No entanto, se a classe for herdável e você esperar uma hierarquia de classes derivadas, talvez seja necessário que essas classes derivadas acessem os dados ou o código. Nesse caso, você deseja que o elemento seja acessível tanto da classe base quanto de todas as classes derivadas. Para limitar o acesso a um elemento dessa maneira, você pode declará-lo com `Protected` .

> [!NOTE]
> O `Protected` modificador de acesso pode ser combinado com dois outros modificadores:
>
> - O modificador [Friend protegido](protected-friend.md) torna um membro de classe acessível de dentro dessa classe, de classes derivadas e do mesmo assembly no qual a classe é definida.
> - O modificador [particular protegido](private-protected.md) torna um membro de classe acessível por tipos derivados, mas somente dentro de seu assembly de contenção.

## <a name="rules"></a>Regras

**Contexto de declaração.** Você pode usar `Protected` somente no nível de classe. Isso significa que o contexto de declaração para um `Protected` elemento deve ser uma classe e não pode ser um arquivo de origem, namespace, interface, módulo, estrutura ou procedimento.

## <a name="behavior"></a>Comportamento

- **Nível de acesso.** Todo o código em uma classe pode acessar seus elementos. O código em qualquer classe derivada de uma classe base pode acessar todos os `Protected` elementos da classe base. Isso é verdadeiro para todas as gerações de derivação. Isso significa que uma classe pode acessar `Protected` elementos da classe base da classe base e assim por diante.

     O acesso protegido não é um superconjunto ou subconjunto de acesso Friend.

- **Modificadores de acesso.** As palavras-chave que especificam o nível de acesso são chamadas de *modificadores de acesso*. Para obter uma comparação dos modificadores de acesso, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).

O `Protected` modificador pode ser usado nesses contextos:

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

- [Pública](public.md)
- [Público](friend.md)
- [Privada](private.md)
- [Particular protegido](private-protected.md)
- [Amigo Protegido](protected-friend.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
