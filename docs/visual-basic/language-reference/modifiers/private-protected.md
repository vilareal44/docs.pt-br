---
title: Protegido de forma particular
ms.date: 05/10/2018
f1_keywords:
- vb.PrivateProtected
helpviewer_keywords:
- Private Protected keyword [Visual Basic]
- Private Protected keyword [Visual Basic], syntax
ms.openlocfilehash: 8ad1509da71bc80b33700d363ddd4569a0965dff
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303459"
---
# <a name="private-protected-visual-basic"></a>Privado protegido (Visual Basic)

A combinação de palavras-chave `Private Protected` é um modificador de acesso de membro. Um `Private Protected` membro é acessível por todos os membros em sua classe que o contém, bem como por tipos derivados da classe que a contém, mas somente se eles forem encontrados em seu assembly de contenção.

Você pode especificar `Private Protected` somente membros de classes; não é possível aplicar `Private Protected` a membros de uma estrutura porque as estruturas não podem ser herdadas.

O `Private Protected` modificador de acesso é suportado pelo Visual Basic 15,5 e posterior. Para usá-lo, você pode adicionar o seguinte elemento ao seu arquivo de Visual Basic projeto ( \* . vbproj). Desde que Visual Basic 15,5 ou posterior esteja instalado no seu sistema, ele permite que você aproveite todos os recursos de linguagem suportados pela versão mais recente do compilador de Visual Basic:

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

Para obter mais informações, consulte [definindo a versão do idioma do Visual Basic](../configure-language-version.md).

> [!NOTE]
> No Visual Studio, a seleção de ajuda F1 no `private protected` fornece ajuda para [privado](private.md) ou [protegido](protected.md). O IDE escolhe o único token sob o cursor em vez da palavra composta.

## <a name="rules"></a>Regras

- **Contexto de declaração.** Você pode usar `Private Protected` somente no nível de classe. Isso significa que o contexto de declaração para um `Protected` elemento deve ser uma classe e não pode ser um arquivo de origem, namespace, interface, módulo, estrutura ou procedimento.

## <a name="behavior"></a>Comportamento

- **Nível de acesso.** Todo o código em uma classe pode acessar seus elementos. O código em qualquer classe que deriva de uma classe base e está contido no mesmo assembly pode acessar todos os `Private Protected` elementos da classe base. No entanto, o código em qualquer classe derivada de uma classe base e está contido em um assembly diferente não pode acessar os elementos da classe base `Private Protected` .

- **Modificadores de acesso.** As palavras-chave que especificam o nível de acesso são chamadas de *modificadores de acesso*. Para obter uma comparação dos modificadores de acesso, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).

O `Private Protected` modificador pode ser usado nesses contextos:

- [Declaração de classe](../statements/class-statement.md) de uma classe aninhada

- [Instrução Const](../statements/const-statement.md)

- [Instrução Declare](../statements/declare-statement.md)

- [Instrução delegate](../statements/delegate-statement.md) de um delegado aninhado em uma classe

- [Instrução Dim](../statements/dim-statement.md)

- [Instrução enum](../statements/enum-statement.md) de uma enumeração aninhada em uma classe

- [Instrução Event](../statements/event-statement.md)

- [Instrução Function](../statements/function-statement.md)

- [Declaração de interface](../statements/interface-statement.md) de uma interface aninhada em uma classe

- [Instrução Property](../statements/property-statement.md)

- [Instrução de estrutura](../statements/structure-statement.md) de uma estrutura aninhada em uma classe

- [Instrução Sub](../statements/sub-statement.md)

## <a name="see-also"></a>Veja também

- [Pública](public.md)
- [Protected](protected.md)
- [Público](friend.md)
- [Privada](private.md)
- [Amigo Protegido](./protected-friend.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
