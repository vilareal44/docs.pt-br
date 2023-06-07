---
title: atributo cref-guia de programação C#
description: Saiba mais sobre o atributo cref. O atributo cref significa "referência de código" e especifica que o texto interno da marca é um elemento de código.
ms.date: 07/20/2015
helpviewer_keywords:
- cref [C#]
ms.assetid: 66a6b0e5-b961-4504-a461-3a4cf481fc8b
ms.openlocfilehash: 31fa1a3f182d7b72a1dfbe1ce47386f87fbbff75
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381990"
---
# <a name="cref-attribute-c-programming-guide"></a>atributo cref (guia de programação C#)

O atributo `cref` em uma marca de documentação XML significa “referência de código”. Ele especifica que o texto interno da marca é um elemento de código, como um tipo, método ou propriedade. Ferramentas de documentação, como o [DocFX](https://dotnet.github.io/docfx/) e o [Sandcastle](https://github.com/EWSoftware/SHFB), usam os atributos `cref` para gerar automaticamente os hiperlinks para a página em que o tipo ou o membro está documentado.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra `cref` atributos usados em [\<see>](./see.md) marcas.

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

Quando compilado, o programa produz o seguinte arquivo XML. Observe que o atributo `cref` do método `GetZero`, por exemplo, foi transformado em `"M:TestNamespace.TestClass.GetZero"` pelo compilador. O prefixo "M:" significa "método" e é uma convenção reconhecida por ferramentas de documentação como o DocFX e o Sandcastle. Para obter uma lista completa de prefixos, consulte [Processando o Arquivo XML](./processing-the-xml-file.md).

```xml  
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>CRefTest</name>
    </assembly>
    <members>
        <member name="T:TestNamespace.TestClass">
            <summary>
            TestClass contains several cref examples.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.#ctor">
            <summary>
            This sample shows how to specify the <see cref="T:TestNamespace.TestClass"/> constructor as a cref attribute.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.#ctor(System.Int32)">
            <summary>
            This sample shows how to specify the <see cref="M:TestNamespace.TestClass.#ctor(System.Int32)"/> constructor as a cref attribute.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.GetZero">
            <summary>
            The GetZero method.
            </summary>
            <example>
            This sample shows how to call the <see cref="M:TestNamespace.TestClass.GetZero"/> method.
            <code>
            class TestClass
            {
                static int Main()
                {
                    return GetZero();
                }
            }
            </code>
            </example>
        </member>
        <member name="M:TestNamespace.TestClass.GetGenericValue``1(``0)">
            <summary>
            The GetGenericValue method.
            </summary>
            <remarks>
            This sample shows how to specify the <see cref="M:TestNamespace.TestClass.GetGenericValue``1(``0)"/> method as a cref attribute.
            </remarks>
        </member>
        <member name="T:TestNamespace.GenericClass`1">
            <summary>
            GenericClass.
            </summary>
            <remarks>
            This example shows how to specify the <see cref="T:TestNamespace.GenericClass`1"/> type as a cref attribute.
            </remarks>
        </member>
    </members>
</doc>
```

## <a name="see-also"></a>Veja também

- [Comentários da documentação XML](./index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
