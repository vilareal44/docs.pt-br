---
title: Como usar os recursos de documentação XML-guia de programação C#
description: Saiba como usar os recursos de documentação XML. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 06/01/2018
helpviewer_keywords:
- XML documentation [C#]
- C# language, XML documentation features
ms.assetid: 8f33917b-9577-4c9a-818a-640dbbb0b399
ms.openlocfilehash: 9ad2cfe62c70174eec9020ad4c8ce11608aca36d
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380664"
---
# <a name="how-to-use-the-xml-documentation-features"></a>Como usar as funcionalidades da documentação XML

O exemplo a seguir fornece uma visão geral básica de um tipo que foi documentado.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#15)]

O exemplo gera um arquivo *. xml* com o conteúdo a seguir.

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>xmlsample</name>
    </assembly>
    <members>
        <member name="T:TestClass">
            <summary>
            Class level summary documentation goes here.
            </summary>
            <remarks>
            Longer comments can be associated with a type or member through
            the remarks tag.
            </remarks>
        </member>
        <member name="F:TestClass._name">
            <summary>
            Store for the Name property.
            </summary>
        </member>
        <member name="M:TestClass.#ctor">
            <summary>
            The class constructor.
            </summary>
        </member>
        <member name="P:TestClass.Name">
            <summary>
            Name property.
            </summary>
            <value>
            A value tag is used to describe the property value.
            </value>
        </member>
        <member name="M:TestClass.SomeMethod(System.String)">
            <summary>
            Description for SomeMethod.
            </summary>
            <param name="s"> Parameter description for s goes here.</param>
            <seealso cref="T:System.String">
            You can use the cref attribute on any tag to reference a type or member
            and the compiler will check that the reference exists.
            </seealso>
        </member>
        <member name="M:TestClass.SomeOtherMethod">
            <summary>
            Some other method.
            </summary>
            <returns>
            Return values are described through the returns tag.
            </returns>
            <seealso cref="M:TestClass.SomeMethod(System.String)">
            Notice the use of the cref attribute to reference a specific method.
            </seealso>
        </member>
        <member name="M:TestClass.Main(System.String[])">
            <summary>
            The entry point for the application.
            </summary>
            <param name="args"> A list of command line arguments.</param>
        </member>
        <member name="T:TestInterface">
            <summary>
            Documentation that describes the interface goes here.
            </summary>
            <remarks>
            Details about the interface go here.
            </remarks>
        </member>
        <member name="M:TestInterface.InterfaceMethod(System.Int32)">
            <summary>
            Documentation that describes the method goes here.
            </summary>
            <param name="n">
            Parameter n requires an integer argument.
            </param>
            <returns>
            The method returns an integer.
            </returns>
        </member>
    </members>
</doc>
```

## <a name="compiling-the-code"></a>Compilando o código

Para compilar o exemplo, digite o seguinte comando:

`csc XMLsample.cs /doc:XMLsample.xml`

Esse comando cria o arquivo XML *XMLsample.xml*, que pode ser exibido em seu navegador ou usando o `TYPE` comando.

## <a name="robust-programming"></a>Programação robusta

A documentação XML começa com `///` . Quando você cria um novo projeto, os assistentes colocam algumas linhas iniciais `///` no para você. O processamento desses comentários tem algumas restrições:

- A documentação deve ser em XML bem formado. Se o XML não estiver bem formado, um aviso será gerado e o arquivo de documentação conterá um comentário que diz que foi encontrado um erro.

- Os desenvolvedores são livres para criar seu próprio conjunto de marcas. Há um [conjunto recomendado de marcas](recommended-tags-for-documentation-comments.md). Algumas das marcas recomendadas têm significado especial:

  - A `<param>` marca é usada para descrever os parâmetros. Se ela é usada, o compilador verifica se o parâmetro existe e se todos os parâmetros são descritos na documentação. Se a verificação falhar, o compilador emitirá um aviso.

  - O `cref` atributo pode ser anexado a qualquer marca para fazer referência a um elemento de código. O compilador verifica se esse elemento de código existe. Se a verificação falhar, o compilador emitirá um aviso. O compilador respeita qualquer instrução `using` quando procura por um tipo descrito no atributo `cref`.

  - A `<summary>` marca é usada pelo IntelliSense dentro do Visual Studio para exibir informações adicionais sobre um tipo ou membro.

    > [!NOTE]
    > O arquivo XML não fornece informações completas sobre o tipo e os membros (por exemplo, ele não contém nenhuma informação de tipo). Para obter informações completas sobre um tipo ou membro, use o arquivo de documentação junto com a reflexão no tipo ou membro real.

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [-Doc (opções do compilador C#)](../../language-reference/compiler-options/doc-compiler-option.md)
- [Comentários da documentação XML](./index.md)
- [Processador de documentação do DocFX](https://dotnet.github.io/docfx/)
- [Processador de documentação do Sandcastle](https://github.com/EWSoftware/SHFB)
