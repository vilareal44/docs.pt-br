---
title: Marcas recomendadas para comentários de documentação – guia de programação C#
description: Saiba mais sobre as marcas recomendadas para comentários de documentação. Consulte uma lista de marcas recomendadas e exiba recursos adicionais disponíveis.
ms.date: 01/21/2020
helpviewer_keywords:
- XML [C#], tags
- XML documentation [C#], tags
ms.assetid: 6e98f7a9-38f4-4d74-b644-1ff1b23320fd
ms.openlocfilehash: 65bca6f979c5ffd91507b571a4f049377315192d
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381509"
---
# <a name="recommended-tags-for-documentation-comments-c-programming-guide"></a>Marcas recomendadas para comentários de documentação (guia de programação C#)

O compilador do C# processa comentários de documentação em seu código e os formata como XML em um arquivo, cujo nome você especifica na opção de linha de comando **/doc**. Para criar a documentação final com base no arquivo gerado pelo compilador, você pode criar uma ferramenta personalizada ou usar uma ferramenta como [DocFX](https://dotnet.github.io/docfx/) ou [Sandcastle](https://github.com/EWSoftware/SHFB).

As marcas são processadas em constructos de código, como tipos e membros de tipo.

> [!NOTE]
> Os comentários de documentação não podem ser aplicados a um namespace.  
  
 O compilador processará qualquer marca que seja um XML válido. As seguintes marcas fornecem as funcionalidades geralmente usadas na documentação do usuário.  
  
## <a name="tags"></a>Marcas  
  
|||||  
|---|---|---|---|
|[\<c>](./code-inline.md)|[\<para>](./para.md)|[\<see>](./see.md)*|[\<value>](./value.md)  
|[\<code>](./code.md)|[\<param>](./param.md)*|[\<seealso>](./seealso.md)*|  
|[\<example>](./example.md)|[\<paramref>](./paramref.md)|[\<summary>](./summary.md)|  
|[\<exception>](./exception.md)*|[\<permission>](./permission.md)*|[\<typeparam>](./typeparam.md)*|  
|[\<include>](./include.md)*|[\<remarks>](./remarks.md)|[\<typeparamref>](./typeparamref.md)|  
|[\<list>](./list.md)|[\<inheritdoc>](./inheritdoc.md)|[\<returns>](./returns.md)|
  
( \* indica que o compilador verifica a sintaxe.)

Se você quiser que os colchetes angulares sejam exibidos no texto de um comentário de documentação, use a codificação HTML de `<` e `>`, que são `&lt;` e `&gt;` respectivamente. Essa codificação é mostrada no exemplo a seguir.

```csharp
/// <summary>
/// This property always returns a value &lt; 1.
/// </summary>
```

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [-Doc (opções do compilador C#)](../../language-reference/compiler-options/doc-compiler-option.md)
- [Comentários da documentação XML](./index.md)
