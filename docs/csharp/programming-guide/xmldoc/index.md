---
title: Comentários de documentação XML-guia de programação C#
description: Saiba mais sobre comentários de documentação XML. Você pode criar a documentação para seu código, incluindo elementos XML em campos de comentário especiais.
ms.date: 07/20/2015
f1_keywords:
- cs.xml
helpviewer_keywords:
- XML [C#], code comments
- comments [C#], XML
- documentation comments [C#]
- C# source code files
- C# language, XML code comments
- XML documentation comments [C#]
ms.assetid: 803b7f7b-7428-4725-b5db-9a6cff273199
ms.openlocfilehash: fbdeb53331d9fc63d24a3322ea13863d7c0a3630
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381873"
---
# <a name="xml-documentation-comments-c-programming-guide"></a>Comentários de documentação XML (guia de programação C#)

No C#, você pode criar documentação para seu código, incluindo elementos XML em campos de comentário especiais (indicados por barras triplas) no código-fonte diretamente antes do bloco de código ao qual os comentários se referem, por exemplo.

```csharp
/// <summary>
///  This class performs an important function.
/// </summary>
public class MyClass {}
```

Quando você compilar com a opção [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) , o compilador pesquisará todas as marcas XML no código-fonte e criará um arquivo de documentação XML. Para criar a documentação final com base no arquivo gerado pelo compilador, crie uma ferramenta personalizada ou use uma ferramenta como o [DocFX](https://dotnet.github.io/docfx/) ou o [Sandcastle](https://github.com/EWSoftware/SHFB).

Para consultar elementos XML (por exemplo, sua função processa elementos XML específicos que você deseja descrever em um comentário da documentação XML), você pode usar o mecanismo de citação padrão (`<` e `>`).  Para consultar identificadores genéricos em elementos de referência de código (`cref`), você pode usar os caracteres de escape (por exemplo, `cref="List&lt;T&gt;"`) ou chaves (`cref="List{T}"`).  Como um caso especial, o compilador analisa as chaves como colchetes angulares para tornar o comentário da documentação menos incômodo para o autor ao fazer referência a identificadores genéricos.

> [!NOTE]
> Os comentários da documentação XML não são metadados; eles não estão incluídos no assembly compilado e, portanto, não são acessíveis através de reflexão.

## <a name="in-this-section"></a>Nesta seção

- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)

- [Processando o arquivo XML](./processing-the-xml-file.md)

- [Delimitadores para marcações de documentação](./delimiters-for-documentation-tags.md)

- [Como usar as funcionalidades da documentação XML](./how-to-use-the-xml-documentation-features.md)

## <a name="related-sections"></a>Seções relacionadas

Para obter mais informações, veja:

- [-Doc (comentários de documentação do processo)](../../language-reference/compiler-options/doc-compiler-option.md)

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
