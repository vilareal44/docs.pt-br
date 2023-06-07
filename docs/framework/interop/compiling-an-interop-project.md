---
title: Compilando um projeto de interoperabilidade
description: Examine como compilar um projeto de interoperabilidade COM, que será compilado como projetos gerenciados se eles fizerem referência a um ou mais assemblies que contêm tipos COM importados.
ms.date: 03/30/2017
helpviewer_keywords:
- interoperation with unmanaged code, compiling
- COM interop, compiling
- exposing COM components to .NET Framework
- compiling interop projects
- interoperation with unmanaged code, exposing COM components
- COM interop, exposing COM components
ms.assetid: 6fcf6588-5e25-41af-b4ae-780974f2c3df
ms.openlocfilehash: a8dfbeb88d0057eb3c9047b4435f021750ed86d2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620853"
---
# <a name="compiling-an-interop-project"></a>Compilando um projeto de interoperabilidade

Os projetos de interoperabilidade COM que referenciam um ou mais assemblies que contêm tipos COM importados são compilados como qualquer outro projeto gerenciado. É possível referenciar assemblies de interoperabilidade em um ambiente de desenvolvimento como o Visual Studio ou referenciá-los ao usar um compilador de linha de comando. Em ambos os casos, para ser compilado corretamente, o assembly de interoperabilidade deve estar no mesmo diretório dos outros arquivos de projeto.

 Há duas maneiras de referenciar assemblies de interoperabilidade:

- Tipos de interoperabilidade inseridos: a partir do .NET Framework 4 e do Visual Studio 2010, você pode instruir o compilador a inserir informações de tipo de um assembly de interoperabilidade em seu executável. Essa é a técnica recomendada.

- Implantando assemblies de interoperabilidade: é possível criar uma referência padrão a um assembly de interoperabilidade. Nesse caso, o assembly de interoperabilidade deve ser implantado com o aplicativo.

 As diferenças entre essas duas técnicas são abordadas mais detalhadamente em [Usando tipos COM em um código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100)).

 A incorporação de tipos de interoperabilidade com o Visual Studio é demonstrada em [instruções: incorporando tipos de assemblies gerenciados no Visual Studio](../../standard/assembly/embed-types-visual-studio.md).

 Para fazer referência a um assembly de interoperabilidade com um compilador de linha de comando e inserir informações de tipo em seus executáveis, use a opção [-link (opções do compilador C#)](../../csharp/language-reference/compiler-options/link-compiler-option.md) ou o comando [-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md) do compilador e especifique o nome do assembly de interoperabilidade.

> [!NOTE]
> Os aplicativos do Visual C++ não podem inserir informações de tipo, mas podem interoperar com aplicativos ou suplementos que têm essa capacidade.

 Para compilar um aplicativo que inclui um assembly de interoperabilidade primário quando ele é implantado, use a opção do compilador **/reference** e especifique o nome do assembly de interoperabilidade.

## <a name="see-also"></a>Consulte também

- [Expondo componentes do COM para o .NET Framework](exposing-com-components.md)
- [Componentes de independência de linguagem e componentes independentes da linguagem](../../standard/language-independence-and-language-independent-components.md)
- [Usando tipos COM no código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [Passo a passo: inserindo tipos de assemblies gerenciados no Visual Studio](../../standard/assembly/embed-types-visual-studio.md)
- [Importando uma biblioteca de tipos como um assembly](importing-a-type-library-as-an-assembly.md)
