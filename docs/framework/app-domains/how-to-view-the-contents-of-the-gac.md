---
title: 'Como: Exibir o conteúdo do cache de assembly global'
description: Saiba como exibir o conteúdo do cache de assembly global no .NET usando a ferramenta GAC (cache de assembly global) (gacutil.exe).
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- Gacutil.exe
- strong-named assemblies, global assembly cache
- GAC (global assembly cache), viewing contents
- list of assemblies in global assembly cache
- Global Assembly Cache tool
ms.assetid: c5f786a0-969b-4f14-9f02-e77c3384d9af
ms.openlocfilehash: 4d775efc073f3aad745eff7a7707efdec06172c2
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104686"
---
# <a name="how-to-view-the-contents-of-the-global-assembly-cache"></a>Como exibir o conteúdo do cache de assembly global

Use a [Ferramenta de Cache de Assembly Global (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) para exibir o conteúdo do GAC (cache de assembly global).

## <a name="view-the-assemblies-in-the-gac"></a>Exibir os assemblies no GAC

Para exibir uma lista de assemblies no cache de assembly global, abra o [Prompt de Comando do Desenvolvedor para Visual Studio](../tools/developer-command-prompt-for-vs.md) e, em seguida, digite o seguinte comando:

```shell
gacutil -l
```

-ou-

```shell
gacutil /l
```

> [!NOTE]
> Nas versões anteriores do .NET Framework, a extensão do shell do Windows [Shfusion.dll](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/34149zk3(v=vs.100)) permitia exibir o cache de assembly global no Explorador de Arquivos. Do .NET Framework 4 em diante, a Shfusion.dll tornou-se obsoleta.

## <a name="see-also"></a>Veja também

- [Trabalhando com assemblies e o cache de assemblies global](working-with-assemblies-and-the-gac.md)
- [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md)
