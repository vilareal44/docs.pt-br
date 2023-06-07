---
title: Variáveis de estrutura
ms.date: 07/20/2015
helpviewer_keywords:
- structures [Visual Basic], variables
- structures [Visual Basic], structure variables
- variables [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: 156872f8-aabc-4454-8e2d-f2253c3c13c9
ms.openlocfilehash: 270e8ca26185e4a68def3b95f4ce6ab4c57a629c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393579"
---
# <a name="structure-variables-visual-basic"></a>Variáveis de estrutura (Visual Basic)

Depois de criar uma estrutura, você pode declarar variáveis em nível de procedimento e de módulo como esse tipo. Por exemplo, você pode criar uma estrutura que registra informações sobre um sistema de computador. O exemplo a seguir demonstra isso.

```vb
Public Structure systemInfo
    Public cPU As String
    Public memory As Long
    Public purchaseDate As Date
End Structure
```

Agora você pode declarar variáveis desse tipo. A declaração a seguir ilustra isso.

```vb
Dim mySystem, yourSystem As systemInfo
```

> [!NOTE]
> Em classes e módulos, as estruturas declaradas usando a [instrução Dim](../../../language-reference/statements/dim-statement.md) assumem o acesso público como padrão. Se você pretende que uma estrutura seja privada, certifique-se de declará-la usando a palavra-chave [Private](../../../language-reference/modifiers/private.md) .

## <a name="access-to-structure-values"></a>Acesso a valores de estrutura

Para atribuir e recuperar valores dos elementos de uma variável de estrutura, use a mesma sintaxe usada para definir e obter propriedades em um objeto. Você coloca o operador de acesso de membro ( `.` ) entre o nome da variável de estrutura e o nome do elemento. O exemplo a seguir acessa os elementos das variáveis declaradas anteriormente como tipo `systemInfo` .

```vb
mySystem.cPU = "486"
Dim tooOld As Boolean
If yourSystem.purchaseDate < #1/1/1992# Then tooOld = True
```

## <a name="assigning-structure-variables"></a>Atribuindo variáveis de estrutura

Você também pode atribuir uma variável a outra se ambas forem do mesmo tipo de estrutura. Isso copia todos os elementos de uma estrutura para os elementos correspondentes no outro. A declaração a seguir ilustra isso.

```vb
yourSystem = mySystem
```

Se um elemento de estrutura for um tipo de referência, como `String` uma `Object` matriz, ou, o ponteiro para os dados será copiado. No exemplo anterior, se `systemInfo` tiver incluído uma variável de objeto, o exemplo anterior teria copiado o ponteiro de `mySystem` para `yourSystem` , e uma alteração nos dados do objeto por meio de uma estrutura estaria em vigor quando acessada por meio da outra estrutura.

## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados compostos](composite-data-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Estruturas](structures.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Como: Declarar uma estrutura](how-to-declare-a-structure.md)
- [Estruturas e outros elementos de programação](structures-and-other-programming-elements.md)
- [Estruturas e classes](structures-and-classes.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
