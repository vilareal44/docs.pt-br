---
title: <summary> -Guia de programação C#
description: Saiba mais sobre o XML <summary> marca que é usada para descrever um tipo ou um membro de tipo. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
f1_keywords:
- <summary>
- summary
helpviewer_keywords:
- <summary> C# XML tag
- summary C# XML tag
ms.assetid: b4c43d92-2067-4eac-a59a-d32f5248c08b
ms.openlocfilehash: f9243e598aaf0c12dd48b48045f461b4b307c18f
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380599"
---
# <a name="summary-c-programming-guide"></a>\<summary>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<summary>description</summary>
```

## <a name="parameters"></a>parâmetros

- `description`

  Um resumo do objeto.

## <a name="remarks"></a>Comentários

A `<summary>` marca deve ser usada para descrever um tipo ou um membro de tipo. Use [\<remarks>](./remarks.md) para adicionar informações complementares a uma descrição de tipo. Use o [atributo cref](./cref-attribute.md) para habilitar ferramentas de documentação como o [DocFX](https://dotnet.github.io/docfx/) e o [Sandcastle](https://github.com/EWSoftware/SHFB) para criar hiperlinks internos para páginas de documentação em elementos de código.

O texto da `<summary>` marca é a única fonte de informações sobre o tipo no IntelliSense e também é exibido na janela pesquisador de objetos.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo. Para criar a documentação final com base no arquivo gerado pelo compilador, você pode criar uma ferramenta personalizada ou usar uma ferramenta como [DocFX](https://dotnet.github.io/docfx/) ou [Sandcastle](https://github.com/EWSoftware/SHFB).

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#12)]

O exemplo anterior produz o seguinte arquivo XML.

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>YourNamespace</name>
    </assembly>
    <members>
        <member name="T:TestClass">
            text for class TestClass
        </member>
        <member name="M:TestClass.DoWork(System.Int32)">
            <summary>DoWork is a method in the TestClass class.
            <para>Here's how you could make a second paragraph in a description. <see cref="M:System.Console.WriteLine(System.String)"/> for information about output statements.</para>
            </summary>
            <seealso cref="M:TestClass.Main"/>
        </member>
        <member name="M:TestClass.Main">
            text for Main
        </member>
    </members>
</doc>
```

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como fazer uma referência `cref` para um tipo genérico.

[!code-csharp[csProgGuideDocComments#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#11)]

O exemplo anterior produz o seguinte arquivo XML.

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>CRefTest</name>
    </assembly>
    <members>
        <member name="T:A">
            <summary cref="T:C`1">
            </summary>
        </member>
        <member name="T:B">
            <summary cref="T:C`1">
            </summary>
        </member>
        <member name="T:C`1">
            <summary cref="T:A">
            </summary>
            <typeparam name="T"></typeparam>
        </member>
    </members>
</doc>
```

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
