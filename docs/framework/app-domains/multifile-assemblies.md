---
title: Assemblies de vários arquivos
description: Use assemblies de multiarquivos destinados ao .NET usando compiladores de linha de comando ou o Visual Studio com Visual C++. Um arquivo no assembly deve conter o manifesto do assembly.
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- entry point for assembly
- compiling assemblies
- command-line compilers
- assembly manifest, multifile assemblies
- code modules
- multifile assemblies
ms.assetid: 13509e73-db77-4645-8165-aad8dfaedff6
ms.openlocfilehash: a5fb41b3b136a325e6b8658da521cba3176e929f
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104639"
---
# <a name="multifile-assemblies"></a>Assemblies de vários arquivos

Você pode criar assemblies de multiarquivos direcionados ao .NET Framework usando compiladores de linha de comando ou o Visual Studio com Visual C++. Um arquivo no assembly deve conter o manifesto do assembly. Um assembly que inicia um aplicativo também deve conter um ponto de entrada, como um `Main` `WinMain` método ou.

Por exemplo, suponha que você tenha um aplicativo que contenha dois módulos de código, *Client.cs* e *Stringer.cs*. *Stringer.cs* cria o `myStringer` namespace que é referenciado pelo código em *Client.cs*. *Client.cs* contém o `Main` método, que é o ponto de entrada do aplicativo. Neste exemplo, você compila os dois módulos de código e cria um terceiro arquivo que contém o manifesto do assembly, que inicia o aplicativo. O manifesto do assembly faz referência aos módulos *cliente* e *Stringer* .

> [!NOTE]
> Os assemblies de vários arquivos podem ter apenas um ponto de entrada, mesmo que o assembly tenha vários módulos de código.

Existem vários motivos que levam você a querer criar um assembly de vários arquivos:

- Para combinar módulos escritos em linguagens diferentes. Essa é a razão mais comum para a criação de um assembly de vários arquivos.

- Para otimizar o download de um aplicativo colocando tipos raramente usados em um módulo que é baixado apenas quando necessário.

    > [!NOTE]
    > Se você estiver criando aplicativos que serão baixados usando a marca `<object>` com o Microsoft Internet Explorer, é importante criar assemblies de vários arquivos. Nesse cenário, você cria um arquivo separado do seus módulos de código que contenha somente o manifesto do assembly. O Internet Explorer baixa o manifesto do assembly primeiro e, em seguida, cria threads de trabalho para baixar quaisquer módulos adicionais ou assemblies necessários. Enquanto o arquivo que contém o manifesto do assembly estiver sendo baixado, o Internet Explorer não responderá à entrada do usuário. Quanto menor o arquivo que contém o manifesto do assembly, menos tempo o Internet Explorer ficará sem responder.

- Para combinar módulos de código escritos por vários desenvolvedores. Embora cada desenvolvedor possa compilar cada módulo de código em um assembly, isso pode forçar alguns tipos a serem expostos publicamente, que não o seriam se todos os módulos fossem colocados em um assembly de vários arquivos.

Depois de criar o assembly, você pode assinar o arquivo que contém o manifesto do assembly e, portanto, o assembly, ou pode dar ao arquivo e ao assembly um nome forte e colocá-lo no cache de assembly global.

## <a name="see-also"></a>Veja também

- [Como: criar um assembly de vários arquivos](build-multifile-assembly.md)
- [Programar com assemblies](../../standard/assembly/index.md)
