---
title: O atributo 'Extension' pode ser aplicado apenas às declarações 'Module', 'Sub' ou 'Function'
ms.date: 07/20/2015
f1_keywords:
- bc36550
- vbc36550
helpviewer_keywords:
- BC36550
ms.assetid: 4387a51f-733c-45d7-abdb-eb64d4f51078
ms.openlocfilehash: 9b8f49c498699a8f7d1c4b329e82258501aa0c47
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363092"
---
# <a name="extension-attribute-can-be-applied-only-to-module-sub-or-function-declarations"></a>O atributo 'Extension' pode ser aplicado apenas às declarações 'Module', 'Sub' ou 'Function'

A única maneira de estender um tipo de dados no Visual Basic é definir um método de extensão dentro de um módulo padrão. O método de extensão pode ser um `Sub` procedimento ou um `Function` procedimento. Todos os métodos de extensão devem ser marcados com o atributo de extensão, `<Extension()>` , do <xref:System.Runtime.CompilerServices?displayProperty=nameWithType> namespace. Opcionalmente, um módulo que contém um método de extensão pode ser marcado da mesma maneira. Nenhum outro uso do atributo de extensão é válido.

**ID do erro:** BC36550

## <a name="to-correct-this-error"></a>Para corrigir este erro

- Remova o atributo de extensão.

- Reprojete sua extensão como um método, definido em um módulo delimitador.

## <a name="example"></a>Exemplo

O exemplo a seguir define um `Print` método para o `String` tipo de dados.

```vb
Imports StringUtility
Imports System.Runtime.CompilerServices
Namespace StringUtility
    <Extension()>
    Module StringExtensions
        <Extension()>
        Public Sub Print (ByVal str As String)
            Console.WriteLine(str)
        End Sub
    End Module
End Namespace
```

## <a name="see-also"></a>Confira também

- [Visão geral de atributos](../../programming-guide/concepts/attributes/index.md)
- [Métodos de Extensão](../../programming-guide/language-features/procedures/extension-methods.md)
- [Instrução Module](../statements/module-statement.md)
