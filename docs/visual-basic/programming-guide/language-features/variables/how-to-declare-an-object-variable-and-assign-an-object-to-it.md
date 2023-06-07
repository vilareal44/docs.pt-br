---
title: Como declarar uma variável de objeto e atribuir um objeto a ela
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], declaring
- declaring object variables [Visual Basic]
ms.assetid: 2fa77dde-1fb2-439a-80d4-3e9787649fad
ms.openlocfilehash: d9a8c1fb30bfa5988d48202e41202e7ede0f5f27
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410497"
---
# <a name="how-to-declare-an-object-variable-and-assign-an-object-to-it-in-visual-basic"></a>Como declarar uma variável de objeto e atribuir um objeto a ela no Visual Basic

Você declara uma variável do [tipo de dados Object](../../../language-reference/data-types/object-data-type.md) especificando `As Object` em uma [instrução Dim](../../../language-reference/statements/dim-statement.md). Você atribui um objeto a tal variável colocando o objeto após o sinal de igual ( `=` ) em uma instrução de atribuição ou uma cláusula de inicialização.

## <a name="example"></a>Exemplo

O exemplo a seguir declara uma `Object` variável e atribui a instância atual a ela.

```vb
Dim thisObject As Object
thisObject = "This is an Object"
```

Você pode combinar a declaração e a atribuição inicializando a variável como parte de sua declaração. O exemplo a seguir é equivalente ao exemplo anterior.

```vb
Dim thisObject As Object= "This is an Object"
```

## <a name="compile-the-code"></a>Compilar o código

Este exemplo requer:

- Uma referência ao <xref:System> namespace.

- Uma classe, estrutura ou módulo no qual colocar a `Dim` instrução.

- Um procedimento no qual colocar a instrução de atribuição.

## <a name="see-also"></a>Confira também

- [Declaração de Variável](variable-declaration.md)
- [Variáveis de Objeto](object-variables.md)
- [Declaração de Variável do Objeto](object-variable-declaration.md)
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
- [Instrução Dim](../../../language-reference/statements/dim-statement.md)
- [Inferência de Tipo de Variável Local](local-type-inference.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
