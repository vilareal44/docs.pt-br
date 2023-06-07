---
title: Source Link e bibliotecas .NET
description: Recomendações de melhores práticas de uso do Source Link para melhorar a depuração de bibliotecas .NET.
ms.date: 01/15/2019
ms.openlocfilehash: 5dee2a6b1f77daa641351e02c1dd3e0a38f66550
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84201977"
---
# <a name="source-link"></a>Source Link

O Source Link é uma tecnologia que permite a depuração de código-fonte dos assemblies .NET do NuGet por desenvolvedores. O Source Link é executado durante a criação do pacote NuGet e insere os metadados de controle do código-fonte nos assemblies e no pacote. Os desenvolvedores que baixam o pacote e têm o Source Link habilitado no Visual Studio podem intervir no código-fonte. O Source Link fornece metadados de controle do código-fonte para criar uma ótima experiência de depuração.

## <a name="source-link-demo"></a>Demonstração do Source Link

<!--markdownlint-disable MD034 -->
> [!VIDEO https://www.youtube.com/embed/gyRGhCQPkB4?start=61]

## <a name="using-source-link"></a>Usando o Source Link

Encontre instruções sobre como usar o Source Link no repositório GitHub [dotnet/sourcelink](https://github.com/dotnet/sourcelink/blob/master/README.md).

Use o [Explorador de Pacotes NuGet](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) para confirmar que os metadados do Source Link foram inseridos com êxito no pacote. Verifique se os `Repository` metadados estão presentes com um identificador de confirmação e se os arquivos. pdb estão localizados com o. dll de cada destino.

![Link de origem no Gerenciador de pacotes NuGet](./media/sourcelink/nuget-package-explorer-sourcelink.png "Link de origem no Gerenciador de pacotes NuGet")

✔️ CONSIDERE o uso do Source Link para adicionar metadados de controle do código-fonte aos assemblies e pacotes NuGet.

> [!TIP]
> Você ainda pode aprimorar a experiência de depuração do desenvolvedor com a adição de atributos do depurador aos seus tipos.
>
> * <xref:System.Diagnostics.DebuggerDisplayAttribute> pode personalizar como uma classe ou campo é exibido nas janelas de variáveis do depurador.
> * <xref:System.Diagnostics.DebuggerStepThroughAttribute> instrui o depurador a depurar o código em vez de intervir nele.
> * <xref:System.Diagnostics.DebuggerBrowsableAttribute> controla se um membro é exibido nas janelas de variáveis do depurador.

✔️ CONSIDERE publicar arquivos de símbolo (`*.pdb`).

> Para proporcionar a melhor experiência de depuração, a biblioteca deverá publicar arquivos de símbolo, além de usar o Source Link. Para obter mais informações sobre arquivos de símbolo e pacotes de símbolos, confira [Pacotes de símbolos](./nuget.md#symbol-packages).

✔️ Considere habilitar compilações determinísticas.

> As compilações determinísticas permitem a verificação de que o binário resultante foi criado a partir da origem especificada e fornece rastreamento. Para obter mais informações sobre compilações determinísticas e instruções para habilitá-las, consulte [compilações determinísticas](https://github.com/clairernovotny/DeterministicBuilds).

>[!div class="step-by-step"]
>[Anterior](dependencies.md) 
> [Avançar](publish-nuget-package.md)
